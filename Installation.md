This is a guide for fully installing wire-pod. Please read every step fully before performing them!

# Prerequisites
* A Vector 1.0
    - Vector 2.0 is not supported at the moment
* A computer running Linux (distros with `pacman`, `dnf`, or `apt` are supported). Raspberry Pi OS (64-bit), Ubuntu, and Debian are good choices
    - Windows may also be used, a guide will exist below soon
* A device with Bluetooth support
    - Can be the same machine as above
    - A computer running Windows or macOS is preferred
    - An Android phone works too
* Some command-line knowledge and experience


# Preparing the bot (production bots only)

- ***NOTE: This section is for production bots only. If you have an OSKR/dev bot, skip this part.***

1. Put Vector into recovery mode. This can be done by setting him on the charger and holding his button for ~15 seconds. He will turn off. Keep holding until the lights come back on.

2. Once he has reached the anki.com/v or ddl.io/v screen, open Chrome (some other Chromium-based browsers work too) on a device with Bluetooth support and go to [https://keriganc.com/vector-epod-setup](https://keriganc.com/vector-epod-setup).
    - If you see an error about Chrome, even though you are running Chrome, enter `chrome://flags` in your URL bar, enable "Enable experimental web platform features", relaunch Chrome, then try again.

3. Follow the directions. The bot should start downloading an update. Once it is done, keep the page open but don't do anything else on the page. Follow the steps below to install wire-pod.

# Installing wire-pod

## Linux

1. On the device you would like to install wire-pod on, open a Terminal application

2. Install git via your package manager. On Debian/Ubuntu/Raspberry Pi OS, the command would be `sudo apt -y install git`.

3. Clone the directory with the following commands.

```
cd ~
git clone https://github.com/kercre123/wire-pod
```

4. Run setup.sh and follow the directions. Default settings can be used by just hitting enter with no other input on many of the options.

```
cd ~/wire-pod
sudo ./setup.sh
```

5. Start wire-pod with the following command:

```
sudo ./chipper/start.sh
```

6. It should show `Server started successfully!` after a while. If it gives an error along the lines of `nil pointer dereference`, run setup.sh and give a different port for chipper.

7. If you want to make wire-pod run in the background (as a daemon), run the following commands (press CTRL+C to stop wire-pod if you are running it):

```
sudo ./setup.sh daemon-enable
sudo systemctl start wire-pod
```

8. If you have an **OSKR or dev bot**, you can set them up with wire-pod with the following command (replace vectorip with Vector's actual IP address, and /path/to/key with the actual path to the SSH key).
    -  If you are on my software (WireOS), you do not have to provide a key as the script will fetch it itself.

```
sudo ./setup.sh scp vectorip /path/to/key
```

9. If you have a **production** bot you would like to set up with wire-pod, run the following commands in a Terminal application:

```
sudo hostnamectl set-hostname escapepod
sudo systemctl restart avahi-daemon
sudo systemctl enable avahi-daemon
```

10. At this point, voice commands should work.

# Authenticate the bot with wire-pod

-   This isn't totally required, but this will allow you to customize your bot's settings and allow the SDK interface to work.
-   If you have cleared Vector's user data, this step is required.

1. Go back to the [vector-epod-setup](https://keriganc.com/vector-epod-setup) page

2. Make sure Vector is on the charger, then double press the back button

3. Press "PAIR WITH VECTOR" and follow the instructions

4. You should end up at a screen with an "ACTIVATE" button. Click on it
    -   If it gets stuck for more than ~15 seconds, refresh the page and try again

5. Your bot should now be fully authenticated!