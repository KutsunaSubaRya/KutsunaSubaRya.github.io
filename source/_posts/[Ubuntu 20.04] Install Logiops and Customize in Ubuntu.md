---
title: Install Logiops and Customize in Ubuntu 
date: 2022-02-26 04:02:40
updated: 2022-02-26 04:02:40
categories: 心得
tags: 
- Ubuntu
- 2022
---

## Link
Driver: [Logiops project address](https://github.com/PixlOne/logiops)
Compatible Devices: [Check here](https://github.com/PixlOne/logiops/blob/master/TESTED.md)
Logiops wiki: [Check Here](https://github.com/PixlOne/logiops/wiki)

## Dependencies

This project requires a C++14 compiler, `cmake`, `libevdev`, `libudev`, and `libconfig`.
```bash=
$ sudo apt install cmake libevdev-dev libudev-dev libconfig++-dev
```

## Install
Clone [this](https://github.com/PixlOne/logiops)
## Building
1. Enter directory
```bash=
$ cd logiops
```
2. Compile project
```bash=
$ mkdir build
$ cd build
$ cmake ..
$ make
```
3. Install
```bash=
$ sudo make install
```
4. Something you will like ...

    * Set the daemon to start at boot
    ```bash=
    $ sudo systemctl enable --now logid
    ```
    * Check the running status
    ```bash=
    $ sudo service logid status
    ```
    ![](https://i.imgur.com/mXcQTTT.png)
    * Restart
    ```bash=
    $ sudo service logid restart
    ```
    * Startup Script
    [reference here](https://linuxconfig.org/how-to-run-script-on-startup-on-ubuntu-20-04-focal-fossa-server-desktop)

## Check CID (Control IDs)

Check CID that your mouse support.
```bash=
$ sudo logid -v
```

will be like ... (All CIDs and it's corresponding function are in [CIDs List](https://github.com/PixlOne/logiops/wiki/CIDs))

![](https://i.imgur.com/WgE59mU.png)

## Configuration File

Your default configuration file is located in `/etc/logid.cfg`.
```bash=
# If you have logid.cfg
$ sudo vim /etc/logid.cfg
```

However, if you didn't find it, why not new one. (lol)
```bash=
# If you didn't find logid.cfg
$ sudo vim /etc/logid.cfg
```

You can customize it on your own !!! OwOb
This is official configuration file syntax detailed reference: [Check here](https://github.com/PixlOne/logiops/wiki/Configuration)
Or you can refer to mine.
* device name: 
    * Mine: Wireless Mobile Mouse MX Anywhere 2S
    * Check your [Compatible Devices](https://github.com/PixlOne/logiops/blob/master/TESTED.md)
* keys: 
    * Mine input event:
        * forward/back button: Switch desktop
        * left/right scroll: Switch page
        * etc ...
    * Check what input event you want: [Here](https://github.com/torvalds/linux/blob/master/include/uapi/linux/input-event-codes.h?fbclid=IwAR0oABkgq30BDnmV2LCanjIemtGdmGIVcCrwc4p0vzC5ftiJnJiqAHLgt7k)


```ini=
devices: (
{
    name: "Wireless Mobile Mouse MX Anywhere 2S";
    buttons: (
        {
            cid: 0x56;
            action =
            {
                type: "Keypress";
                keys: ["KEY_LEFTCTRL","KEY_LEFTALT","KEY_RIGHT"];
            };
        },
        {
            cid: 0x53;
            action =
            {
                type: "Keypress";
                keys: ["KEY_LEFTCTRL","KEY_LEFTALT","KEY_LEFT"];
            };
        },
        {
            cid: 0x5b;
            action =
            {
                type: "Keypress";
                keys:["KEY_LEFTCTRL","KEY_PAGEUP"];
            };
        },
        {
            cid: 0x5d;
            action =
            {
                type: "Keypress";
                keys:["KEY_LEFTCTRL","KEY_PAGEDOWN"];
            };
        },
        {
            cid: 0xd7;
            action =
            {
                type: "Gestures";
                gestures:(
                {
                    direction:"Up";
                    mode="OnInterval";
                    interval=75;
                    action=
                    {
                        type:"Keypress";
                        keys:["KEY_VOLUMEUP"];
                    }
                },
                {
                    direction:"Down";
                    mode="OnInterval";
                    interval=75;
                    action=
                    {
                        type:"Keypress";
                        keys:["KEY_VOLUMEDOWN"];
                    }
                },
                {
                    direction:"Left";
                    mode="OnRelease";
                    action=
                    {
                        type:"Keypress";
                        keys:["KEY_LEFTCTRL","KEY_C"];
                    }
                },
                {
                    direction:"Right";
                    mode="OnRelease";
                    action=
                    {
                        type:"Keypress";
                        keys:["KEY_LEFTCTRL","KEY_V"];
                    }
                },
                {
                    direction:"None";
                    mode="OnRelease";
                    action=
                    {
                        type:"Keypress";
                        keys:["KEY_ENTER"];
                    }
                }
                )
            };
        }

    );
    hiresscroll:
    {
        hires: true;
        invert: false;
        target: false;
    };
}
);

```
