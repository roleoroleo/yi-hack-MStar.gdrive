# Google drive loader for yi smart ip camera
This tool was originally created by Oleksandr Porunov:
https://github.com/porunov/xiaomi_gdrive

I ported it on yi cam based on MStar platform.

Google drive loader for yi smart ip camera (MStar based). yi_gdrive let you automatically upload your videos from xiaomi smart ip camera to your google drive account. Also it can automatically remove old files from your google drive account to prevent space exhaustion.
If you want to run this tool, you need to hack the cam:
https://github.com/roleoroleo/yi-hack-MStar

![ants_smart_webcam_ xiaomi](https://cloud.githubusercontent.com/assets/17673243/17768152/76d2a56a-653b-11e6-81db-522a29f9f1f2.png)

### Step-by-step instruction for installing xiaomi_gdrive

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
8. After turnung on a camera use telnet or ssh to connect to your camera:
9. Go to the browser
10. Create your Google Drive application and OAuth keys for Google Drive API (example tutorial: http://www.iperiusbackup.net/en/how-to-enable-google-drive-api-and-get-client-credentials/)
   
   Example:
   - Go to Google Api Console (https://console.developers.google.com/?hl=RU)
   - Click "Drive API"
   - Clieck "Create project" and create it (if don't have one)
   - Click "Enable"
   - Go to Credentials and add credentials to your project
      1. Where will you be calling the API from? : Other UI (e.g. Windows, CLI tool)
      2. What data will you be accessing? : User data
      3. Click "What credentials do I need?"
      4. Name your credentials as you want
      5. Product name shown to users - use any name
      6. Click "Done"
   - Click on your credentials
   - Save your client id and client secret

11. Go to your console back
12. Run GDriveConf from /tmp/sd/yi-hack/gdrive folder to configure your Google Drive access:

  ```
  sh /tmp/sd/yi-hack/gdrive/GDriveConf
  ```

13. Paste your client id and press enter
14. Paste your client secret and press enter
15. Copy link which you see and paste into your browser
16. Click "Accept"
17. Copy code which you see
18. Go to your console back
19. Paste your code and press enter
20. You will be suggested to see the folders. Press Enter if you want to see all folders. If you want to see only root folders type `root` and press Enter.
//Folders showing isn't fast. Wait for 5-10 seconds to see your directories.
21. You will see your folders (number of folder is on the left side)
22. Type the number of a folder and press Enter. (If you want to save videos in the root dir then just press Enter)
23. You will be asked if you want to turn on automatic remove. Press `1` and type Enter if you want. Press `0` and type Enter if you do not want. GDriveAutoremover itself will delete old files in case if your disk space overflows.
24. Reboot your camera:

  ```
  reboot
  ```

25. Done

How it works:
The script in the loop will create the same folders as in the record folder and upload videos into Google Drive. After the reboot, or failure of the internet script continues normally send files. If you have enabled automatic remove, GDriveAutoremover will check your free space every 45 minutes. In case when disk space is not enough, the script will erase old videos (IMPORTANT: do not put anything extra in the folder which is designed for video because GDriveAutoremover can remove it if it considers that the disk space is not enough).

This scripts were tested under model y203c with firmware 4.5.0 and yi-hack-MStar 0.3.5
