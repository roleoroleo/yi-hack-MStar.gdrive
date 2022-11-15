# Google drive loader for yi smart ip camera
This tool was originally created by Oleksandr Porunov:
https://github.com/porunov/xiaomi_gdrive

I ported it on yi cam based on MStar platform.

Google drive loader for yi smart ip camera (MStar based). yi_gdrive let you automatically upload your videos from yi smart ip camera to your Google Drive account. Also it can automatically remove old files from your Google Drive account to prevent space exhaustion.
If you want to run this tool, you need to hack the cam:
https://github.com/roleoroleo/yi-hack-MStar

![ants_smart_webcam_ xiaomi](https://cloud.githubusercontent.com/assets/17673243/17768152/76d2a56a-653b-11e6-81db-522a29f9f1f2.png)

### Step-by-step instruction for installing yi_gdrive

1. Turn off your camera and get microSD
2. Download yi_gdrive
3. Create "yi-hack" folder into your microSD
4. Copy gdrive folders inside "yi-hack" folder into your microSD:
5. If you want to set the time when your GDrive can interact with the Internet (i.e. send or remove files to Google Drive) you can change change the time in GDriveAutoremover and GDriveUploader files. If you want to let the camera interact with the Internet 24 hour/day (immediately send a video after it is recorded) then skip this step. If you still want to change the time then open your GDriveUploader script and change start_time and finish_time variables to whatever you want in the next format: HH:MM:SS. To do it find the next line:

  ```
  start_time="00:00:00"
  ```
  
  and change the time in this line to whatever you want. Example 7:35:00 pm will be:    
  
  ```
  start_time="19:35:00"
  ```
  
  Then find the next line:   
  
  ```
  finish_time="23:59:59"
  ```
  
  and change it to whatever you want. Example 01:00:05 am will be:   
  
  ```
  finish_time="01:00:05"
  ```
  
  Your camera will be able to interact with the Internet from start_time to finish_time.
  
6. Put microSD into your camera
7. Turn on camera
8. After turning on your camera, use telnet or SSH (the default user is `root` without passowrd) to connect to your camera:
9. Go to the browser, create your Google Drive application and OAuth keys for Google Drive API (example tutorial: http://www.iperiusbackup.net/en/how-to-enable-google-drive-api-and-get-client-credentials/). Essential steps' summary:
    1.  Create a project by click **NEW PROJECT** button or select your existing project to work on;
    3.  Enter [https://console.cloud.google.com/apis/](https://console.cloud.google.com/apis/) page;
    4.  Click the **+ ENABLE APIS AND SERVICES** button on top or the **Library** tab in the left side bar. Then search `Google Drive API`, select and enable it;
    5.  Configure **OAuth consent screen**: Choose `External` User Type, fill App name, user support email, Developer contact email, and then:
        1.  SAVE AND CONTINUE
        2.  BACK TO DASHBOARD
        3.  PUBLISH APP
    6.  Click **Credentials** tab in left side bar:
        1.  Click **+ CREATE CREDENTIALS** button, choose the second item **OAuth client ID**;
        2.  Choose `Web application` as the Application type, input the app name;
        3.  Add `http://127.0.0.1` as one of your Authorized redirect URIs when you create the credentials.
        4.  Fetch ans save your client ID and client secret (or download the JSON file).

10. Go back to your SSH console
11. Run GDriveConf from /tmp/sd/yi-hack/gdrive folder to configure your Google Drive access:

  ```
  cd /tmp/sd/
  sh yi-hack/gdrive/GDriveConf
  ```

13. Paste your client id and press enter
14. Paste your client secret and press enter
15. Copy link which you see and paste into your browser
16. Click "Accept"
17. Select and copy the code **from the browser address bar**. :warning: Even when you see a page shows "This site can't be reached", you've already got the code in your address bar. e.g.:
    1.  `127.0.1/?state=state&code=4%F0AfgeXvsbZbJJklkweJKJklsfl899DcstdjfkIfdMQxffdf&scope=https:%3A%2F%2Fwww.googleapis.com%2Fauth%2fdrive` 
    2.  copy the string between `&code=` and `&scope=` which is your authorization code
18. Go to your console back
19. Paste your code and press enter
20. You will be suggested to see the folders. Press Enter if you want to see all folders. If you want to see only root folders type `root` and press Enter.
//Folders showing isn't fast. Wait for 5-10 seconds to see your directories.
21. You will see your folders (number of folder is on the left side)
22. Type the number of a folder and press Enter. (If you want to save videos in the root dir then just press Enter)
23. You will be asked if you want to turn on automatic remove. Press `1` and type Enter if you want. Press `0` and type Enter if you do not want. GDriveAutoremover itself will delete old files in case if your disk space overflows.
24. Reboot your camera:

  ```
  reboot  # do it manually if this command doesn't work
  ```
25. Done

You can check some [screenshots](https://github.com/roleoroleo/yi-hack-MStar.gdrive/tree/master/screenshots) for reference guide.

How it works:
The script in the loop will create the same folders as in the record folder and upload videos into Google Drive. After the reboot, or failure of the internet script continues normally send files. If you have enabled automatic remove, GDriveAutoremover will check your free space every 45 minutes. In case when disk space is not enough, the script will erase old videos (IMPORTANT: do not put anything extra in the folder which is designed for video because GDriveAutoremover can remove it if it considers that the disk space is not enough).

This scripts were tested under:
- model y203c with firmware 4.5.0 and yi-hack-MStar 0.3.5
- model y201c(Yi 1080p Dome BFUS) with firmware 4.6.0.0A_201908271549 and yi-hack-MStar 0.4.7 by @denven