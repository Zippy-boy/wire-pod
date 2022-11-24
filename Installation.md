This is a guide for fully installing wire-pod. Please read every step fully before performing them!

# Prerequisites
* A Vector 1.0 running production software, set up with the mobile app. **Vector 2.0 is not supported at the moment**.
* A computer running Linux (distros with `pacman`, `dnf`, or `apt` are supported). Raspberry Pi OS (64-bit), Ubuntu, and Debian are good choices.
* Some command-line knowledge and experience.


# Preparing the bot

- ***NOTE: This section is for production bots only. If you have an OSKR/dev bot, skip this part.***

- The bot should start off on production software and set up with the mobile app. You should be able to see his eyes.

1. Put Vector into recovery mode. This can be done by setting him on the charger and holding his button for ~15 seconds. He will turn off. Keep holding until the lights come back on.

2. Once he has reached the anki.com/v or ddl.io/v screen, open Chrome (some other Chromium-based browsers work too) and go to [https://keriganc.com/vector-epod-setup](https://keriganc.com/vector-epod-setup).

3. Follow the directions. You may end up at a screen telling you to sign in to an account. Do **NOT** try signing in to an account. You may close the page at this point.

# Installing wire-pod

## Linux

1. Open a Terminal application

2. Install git via your package manager. On Debian/Ubuntu/Raspberry Pi OS, the command would be `sudo apt -y install git`.

3. Clone the directory with the following command.

```
cd ~
git clone https://github.com/kercre123/wire-pod
```

4. Run setup.sh and follow the directions. Default settings can be used by just hitting enter with no other input on many options.

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

```
sudo ./setup.sh scp vectorip /path/to/key
```

* If you are on my software (WireOS), you do not have to provide a key as the script will fetch it itself.

### Allow a production bot to communicate with your wire-pod server

* ***For production bots only***

* Open a Terminal and run the following commands (will change your computer's hostname to `escapepod`):

```
sudo hostnamectl set-hostname escapepod
sudo systemctl restart avahi-daemon
sudo systemctl enable avahi-daemon
```

## TODO: macOS and Windows