This tutorial is taken from the detailed instruction from [dnovischi](https://github.com/dnovischi/jetson-nano-pythorch/issues/2#issuecomment-1101727407) on how to upgrade your Jetson Nano form 18.04 to 20.04 with Cuda libraries that are compatible with Pytorch.

#### 1. Install the official ubuntu 18.04 image:
- [Jetson Nano 2GB](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit)
- [Jetson Nano 4GB](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)

#### 2. Insert the sd card into the nano and boot

#### 3. Update and upgrade:

    sudo apt update -y
    sudo apt upgrade -y

#### 4. Install latest jetpack:

    sudo apt install nvidia-jetpack

#### 5. Remove Chromium browser, upgrade and get rid of unused packages:

    sudo apt remove --purge chromium-browser chromium-browser-l10n
    sudo apt update
    sudo apt upgrade
    sudo apt autoremove

#### 6. Install nano editor:

    sudo apt install nano

#### 7. Next, you need to enable distribution upgrades in the update manager by setting prompt=normal in the `/etc/update-manager/release-upgrades` file. As usual, close with `<Ctrl> + <X>, <Y>` and `<Enter>`.

    sudo nano /etc/update-manager/release-upgrades

- Change `Prompt=never` to `Prompt=normal`, save and exit.

#### 8. With the update manager set, we need to refresh the software database again. Once done, you can reboot.

    sudo apt update
    sudo apt dist-upgrade
    sudo reboot

#### 9. With all preparations made, it's time for the upgrade to Ubuntu 20.04.
It will take several hours. Unfortunately, some input is required throughout the procedure as there are questions to be answered.
Check your screen now and then. Answer all questions with the suggested default value.

    sudo do-release-upgrade

- When the process finishes, **do not reboot!**

#### 10. First, check that `WaylandEnable=false` is uncommented in the `/etc/gdm3/custom.conf` file.

#### 11. Uncomment `Driver "nividia"` in the `/etc/X11/xorg.conf` file.

#### 12. Finally, reset the upgrade manager to never (`Prompt=never`) and now reboot.

    sudo nano /etc/gdm3/custom.conf
    sudo nano /etc/X11/xorg.conf
    sudo nano /etc/update-manager/release-upgrades
    sudo reboot

#### 13. Update, upgrade and auto-remove:

    sudo apt update
    sudo apt upgrade
    sudo apt autoremove

#### 14. Next, remove an annoying circular symbolic link in `/usr/share/applications` that makes the same app appear 86 times in your software overview.

#### 15. Re-enable the original NVIDIA repositories, which were disabled during the upgrade. In the folder `/etc/apt/sources.list.d/` you will find the five files that needed to be changed. Open each file with `sudo nano` and **remove the hash** in front of the line to activate the repository.

#### 16. Again, Update, upgrade and auto-remove:

sudo apt update
sudo apt upgrade
sudo apt autoremove

#### 17. Some software packages, especially the CUDA software, requires a gcc version 8. We shall install this version besides the already available version 9. With a simple command, you can now switch between the two versions. The gcc compiler is always accompanied by the corresponding g++ compiler. The latter will also be installed.

    # install gcc and g++ version 8
    sudo apt install gcc-8 g++-8
    # setup the gcc selector
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 9
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
    # setup the g++ selector
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 9
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
    # if you want to make a selection use these commands
    sudo update-alternatives --config gcc
    sudo update-alternatives --config g++

#### 18. You may run into problems upgrading Ubuntu 20.04 on your Jetson Nano after a while. The Software Updater cannot install all the packages listed.

    sudo apt --fix-broken
    sudo apt intall -f