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

- ***NOTE: This section is for production bots only. If you have an OSKR/dev-unlocked bot, skip this section.***

- What this section does:
    -   Puts the bot in recovery mode, where you can put on any (production-signed) firmware version
    -   It does NOT matter what firmware version you start on. v1.8, v1.6, v2.0, etc. This is able to put any bot on the software version it needs to be on.
    -   Applies a special firmware compiled by DDL which allows functionality with escape pod
        -   The use of this OTA does not require paying for escape pod
        -   You can tell if this software is applied by not by going to the CCIS page and looking at the firmware string. It should have `ep` at the end of it.
        -   Just running v2.0.* on your bot is NOT enough. ***You need this special ep firmware***.

1. Put Vector into recovery mode. This can be done by setting him on the charger and holding his button for ~15 seconds. He will turn off. Keep holding until the lights come back on.
    - This is NOT the same as clearing user data. This step will not clear user data.

2. Once he has reached the ***anki.com/v or ddl.io/v screen***, open Chrome (some other Chromium-based browsers work too) on a device with Bluetooth support and go to [https://keriganc.com/vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup).
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
git clone https://github.com/kercre123/wire-pod --depth=1
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

- I have created an installer executable for wire-pod.

- To set up wire-pod on your Windows machine:

1. Make sure no other wire-pod instances are running on the network.
2. Make sure no other devices on the network are called `escapepod`.
3. If you want to use a regular production robot, you must [set your computer's name](https://www.pcmag.com/how-to/how-to-change-computer-name-windows) to `escapepod` then reboot your computer.
4. Head to [the latest releases page](https://github.com/kercre123/wire-pod/releases/latest).
5. Download "wire-pod-installer.exe" from that releases page and run it. It may take a little while at first launch. Do not download the .zip file - that will be gotten by the installer.
6. Windows SmartScreen may come up. Click `More Info` then select `Run Anyway`.
7. Click "Next", wait for it to finish installing, then click "Exit". Wire-pod will eventually open.
8. On the message pop-up, click "Open browser" and finish setting wire-pod up.
9. Continue on to the next section to authenticate your robot with wire-pod.

# Authenticate the bot with wire-pod

This is a required step which allows the "Bot Settings" portion in the web app to work.

***The steps are different depending on if your bot is unlocked or not. Double-check that you are completing the correct section.***

## Authenticate a **production** bot

1. It is recommended to clear your bot's user data. This is not required, and you can still authenticate with wire-pod without it (as long as the last server you have authenticated the bot with was the DDL/Anki production stack), but it may cause unexpected behavior.
    1.  Place Vector on the charger
    2.  Double press his button
    3.  Lift his lift up then down
    4.  Take Vector off of the charger and twist one of the wheels until the cursor is on "RESET" (or "CLEAR USER DATA")
    5.  Lift the lift up then down again
    6.  Move the wheel until the cursor is on "CONFIRM"
    7.  Lift the lift up then down again

2. Refresh the [vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup) page and follow the instructions
    -   Note that the vector-wirepod-setup page is a page which does not just serve one purpose. If the bot is in recovery mode, it puts firmware on the bot. If not, it will attempt to authenticate the bot.

3. You should end up at a screen with an "ACTIVATE" button. Click on it.
    -   If it loads for a little bit then shows back up again, click on it again

4. Enter the desired settings (can be changed later) then click "SAVE SETTINGS".

5. Once setup shows "Vector setup is complete!", you are done! Your bot should now be fully authenticated and set up!

## Authenticate an **unlocked** bot

1. Head to the wire-pod web interface. This is the same as the configuration page mentioned in the previous section.

2. Click on "Bot Setup"

3. In the second section, "Set up OSKR/dev bot", follow the instructions.
    -   You may need to run this twice if it ever shows "not running (error: <error>)". It's success will be made clear.

4. Refresh the [vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup) page and follow the instructions.
    -   Note that the vector-wirepod-setup page is a page which does not just serve one purpose. If the bot is in recovery mode, it puts firmware on the bot. If not, it will attempt to authenticate the bot.

5. Enter the desired settings (can be changed later) then click "SAVE SETTINGS".

6. Once setup shows "Vector setup is complete!", you are done! Your bot should now be fully authenticated and set up!

# Modifying bot settings

1. Head to the web app/configuration page.

2. Click on "Bot Settings" and select your bot.

3. Here, you can modify bot settings like location and weather units.