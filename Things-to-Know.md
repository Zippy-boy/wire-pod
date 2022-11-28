Things that are good to know.

# Running wire-pod in the background

- NOTE: Does not work in Windows under WSL

You can setup a systemd daemon for wire-pod. This allows it to run in the background and it to run automatically at startup.
```
cd ~/wire-pod
sudo ./setup.sh daemon-enable
```
To start the service, either restart your computer or run:

`sudo systemctl start wire-pod`

To see logs, run:

`journalctl -fe | grep start.sh`

If you would like to disable the daemon, run:
```
cd ~/wire-pod
sudo ./setup.sh daemon-disable
```


***


# Updating wire-pod

To update to a newer version of wire-pod, make sure chipper is not running then run the following commands:

```
cd ~/wire-pod
sudo git pull
cd chipper
sudo ./start.sh
```

If you have set wire-pod up as a daemon, run `sudo systemctl start wire-pod` rather than `sudo ./start.sh`.
