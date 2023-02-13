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

1. Put Vector into recovery mode. This can be done by setting him on the charger and holding his button for ~15 seconds. He will turn off. Keep holding until the lights come back on.
    - This is NOT the same as clearing user data. This step will not clear user data.

2. Once he has reached the anki.com/v or ddl.io/v screen, open Chrome (some other Chromium-based browsers work too) on a device with Bluetooth support and go to [https://keriganc.com/vector-wirepod-setup](https://keriganc.com/vector-wirepod-setup).
    - If you see an error about Chrome, even though you are running Chrome, enter `chrome://flags` in your URL bar, enable "Enable experimental web platform features", relaunch Chrome, then try again.
    - On many Linux distributions, you will need to open the Bluetooth menu and have it discovering in the background as you try pairing with vector-epod-setup.

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

8. If you have an **OSKR or dev bot**, you can set them up with wire-pod with the following command (replace vectorip with Vector's actual IP address, and /path/to/key with the actual path to the SSH key).
    -  If you are on my software (WireOS), you do not have to provide a key as the script will fetch it itself.
    -  Vector will end up a the blinking V on screen. This is normal. User data was NOT cleared, your bot was just returned to the onboarding status.

```
cd ~/wire-pod
sudo ./setup.sh scp vectorip /path/to/key
```

9. If you have a **production** bot you would like to set up with wire-pod, run the following commands in a Terminal application:

* Don't do this if you are here from the Windows guide

```
sudo hostnamectl set-hostname escapepod
sudo systemctl restart avahi-daemon
sudo systemctl enable avahi-daemon
```

10. At this point, voice commands should work if your bot had been set up previously.

## Guide 2: Windows 10 or higher (CURRENTLY BROKEN)

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

7. You should now be at an Ubuntu terminal. Leave that open in the background.

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

- It should now be setup!

***

# Authenticate the bot with wire-pod

1. ***PROD BOTS ONLY, skip if you have an OSKR/dev-unlocked bot as the setup.sh script will handle it:*** It is recommended to clear your bot's user data. This is not required, and you can still authenticate with wire-pod without it, but it may cause unexpected behavior.
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