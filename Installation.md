This is a guide for fully installing wire-pod. Please read every step fully before performing them!

# Prerequisites
* An Anki or Digital Dream Labs Vector robot
    - Can be a regular production Vector 1.0 or 2.0, can also be OSKR/dev-unlocked
* A computer running Linux or Windows 10/11
    - Distros with `pacman`, `dnf`, or `apt` are supported. Raspberry Pi OS (64-bit), Ubuntu, and Debian are good choices
* A device with Bluetooth support
    - Can be the same machine as above, doesn't have to be a seperate one
    - A computer running Windows or macOS is preferred
    - An Android phone works too
* Some command-line knowledge and experience

***

# Preparing the bot (production bots only)

- ***NOTE: This section is for production bots only. If you have an OSKR/dev-unlocked bot, skip this part.***

- What this section does:
    -   Puts the bot in recovery mode, where you can put on any (production-signed) firmware version
    -   It does NOT matter what firmware version you start on. v1.8, v1.6, v2.0, etc. This is able to put any bot on the software version it needs to be on.
    -   Applies a special firmware compiled by DDL which allows functionality with escape pod
        -   The use of this OTA does not require paying for escape pod
        -   You can tell if this software is applied by not by going to the CCIS page and looking at the firmware string. It should have `ep` at the end of it.
        -   Just running v2.0.* on your bot is NOT enough. You need this special ep firmware.

1. Put Vector into recovery mode. This can be done by setting him on the charger and holding his button for ~15 seconds. He will turn off. Keep holding until the lights come back on.
    - This is NOT the same as clearing user data. This step will not clear user data.

2. Once he has reached the anki.com/v or ddl.io/v screen, open Chrome (some other Chromium-based browsers work too) on a device with Bluetooth support and go to [https://keriganc.com/vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup).
    - If you see an error about Chrome, even though you are running Chrome, enter `chrome://flags` in your URL bar, enable "Enable experimental web platform features", relaunch Chrome, then try again.
    - On many Linux distributions, you may need to open the system's Bluetooth settings menu and have it discovering in the background as you try pairing with vector-wirepod-setup.
        -   BLE support is much more stable in the very modern distros, like Debian bookworm, so you don't need to do this on some

3. Follow the directions. The bot should start downloading an update. Once it is done, keep the page open but don't do anything else on the page. Follow the steps below to install wire-pod.

***

# Installing wire-pod

- wire-pod supports most Linux distributions and Windows 10/11 (with WSL). Follow one of the following guides, then continue on to "Authenticate the bot with wire-pod".

## Guide 1: Linux

1. On the device you would like to install wire-pod on, open a Terminal application

2. Install git via your package manager. On Debian/Ubuntu/Raspberry Pi OS, the command would be `sudo apt install -y git`.

3. Clone the directory with the following commands.

```
cd ~
git clone https://github.com/kercre123/wire-pod
```

4. Run setup.sh.

```
cd ~/wire-pod
sudo STT=vosk ./setup.sh
```

(If you want to choose a different STT service, use just `sudo ./setup.sh`)

5. Start wire-pod with the following command:

```
sudo ./chipper/start.sh
```

It should show a log similar to the following. 

```
Initializing variables
SDK info path: /home/kerigan/.anki_vector/
API config JSON created
Initiating vosk voice processor with language 
Loading plugins
Wire-pod is not setup. Use the webserver at port 8080 to set up wire-pod.
Starting webserver at port 8080 (http://localhost:8080)
Starting SDK app
Starting server at port 80 for connCheck
Configuration page: http://192.168.1.221:8080
```

6. With a device on the same network as wire-pod, open a browser and head to the configuration page. In the case of the above log, http://192.168.1.221:8080. In that page, follow the instructions. Wire-pod should then be set up!

7. If you want to make wire-pod run in the background (as a daemon), run the following commands (press CTRL+C to stop wire-pod if you are running it):

```
sudo ./setup.sh daemon-enable
sudo systemctl start wire-pod
```

8. If you have an **OSKR or dev bot**, you can set them up with wire-pod via the web configuration interface. In the "Bot Setup" section, look for "Set up OSKR/dev bot", enter the bot's IP address, upload the key, then press set up.
    -  Vector will end up a the blinking V on screen. This is normal. User data was NOT cleared, your bot was just returned to the onboarding status.
    -  AFTER YOU HAVE DONE THIS, use [vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup) to finish setting up the bot.

9. If you are going the production-bot route and you would like to set up with wire-pod, run the following commands in a Terminal application:

* Don't do this if you are here from the Windows guide

```
sudo hostnamectl set-hostname escapepod
sudo systemctl restart avahi-daemon
sudo systemctl enable avahi-daemon
```

10. Continue on to "Authenticate the bot with wire-pod", near the bottom of this page.

## Guide 2: Windows 10/11

- NOTE: Make sure your installation is fully up to date.

1. Open up Powershell as administrator
	-	Open the start menu, type in Powershell, right click, click "Run as administrator"

2. Enter the following commands (this will change your computer's name to `escapepod`):

```
Rename-Computer -NewName "escapepod"
wsl --install
```

3. Reboot your computer.

4. When you log in after the reboot, Ubuntu should start install. Wait for it to finish.

5. The Ubuntu installer should ask for a UNIX username. Enter one. example: `wire`

6. It should then ask for a UNIX password. Make sure you remember this! It will not show any indication that you are typing anything, that is normal.

7. You should now be at an Ubuntu terminal. In this terminal, run the following command:

```
sudo apt install -y net-tools
```

- Note: This will ask for a password. Enter it like normal. It won't any indication that you are typing it in, this is normal.

- Keep this Ubuntu terminal running in the background

8. Open up Powershell as administrator
	-	Open the start menu, type in Powershell, right click the first result, click "Run as administrator"

9. In Powershell, run the following command. When it asks for a confirmation, enter `Y`.

```
Set-ExecutionPolicy Bypass
```

10. In Powershell, run the following commands:

```
cd ~
curl -o wsl-firewall.ps1 https://keriganc.com/wsl-firewall.ps1
.\wsl-firewall.ps1
```

11. Return to the Ubuntu terminal window and follow the Linux instructions for installation.

(if it asks for a password, enter what you entered for the UNIX password earlier)

12. Once that is finished, run the following commands to start the server:

```
cd ~/wire-pod
sudo ./chipper/start.sh
```

* NOTE: WSL does not have functional systemd, so you will need to run the above two commands every time you want to start the server. It will not start automatically.

- It should now be setup! Now you can continue

***

# Authenticate the bot with wire-pod

1. ***PRODUCTION BOTS ONLY, skip if you have an OSKR/dev-unlocked bot as the setup.sh script will handle it:*** It is recommended to clear your bot's user data. This is not required, and you can still authenticate with wire-pod without it (as long as the last server you have authenticated the bot with was the DDL/Anki production stack), but it may cause unexpected behavior.
    1.  Place Vector on the charger
    2.  Double press his button
    3.  Lift his lift up then down
    4.  Take Vector off of the charger and twist one of the wheels until the cursor is on "RESET" (or "CLEAR USER DATA")
    5.  Lift the lift up then down again
    6.  Move the wheel until the cursor is on "CONFIRM"
    7.  Lift the lift up then down again

2. Refresh the [vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup) page and follow the instructions

3. You should end up at a screen with an "ACTIVATE" button. Click on it.
    -   If it loads for a little bit then shows back up again, click on it again

4. Enter the desired settings (can be changed later) then click "SAVE SETTINGS".

5. Once setup shows "Vector setup is complete!", you are done! Your bot should now be fully authenticated and set up!