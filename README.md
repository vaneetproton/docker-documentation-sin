# Introduction
Please follow this documentation for setting up the local development environment with Docker on your local machine.

#### GIT
Please make sure that you have `git clone` all the required repositories on to your machine before proceeding further. If you don't have **GIT** already installed on your machine, please install it using the following [steps](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

#### Docker
Also make sure that **docker** is installed on your machine. If not, please download and install it using below links.

[Linux](https://docs.docker.com/desktop/install/linux-install/)

[Windows](https://docs.docker.com/desktop/install/windows-install/)

[macOS](https://docs.docker.com/desktop/install/mac-install/)

## Project Folder Structure
```
Projects
|__magento_v243_website
|__flatdesigner_nodejs
|__docker
```
## Points to remember and double check
- Please make sure you have cloned the Magento website into the project folder.
- You can download **Flatdesigner Nodejs API** from [here](http://104.251.216.173/Downloads/flatdesigner_nodejs.tgz) and extract it into your Projects folder.
- Download the latest **Docker** folder from [here](http://104.251.216.173/Downloads/docker.zip) and extract it into your Projects folder.
- Download MySQL database for your Magento website, if you don't have it already on your machine. You can download it in ZIP format from [here](http://104.251.216.173/Downloads/db.zip) and extract the zip file into `Projects/docker/sql-db` directory.


## Install WSL Distro on Windows OS
WSL means Windows Subsystem for Linux from Microsoft which lets developers run a Linux environment -- including most command line tools, utilities, and applications -- directly on Windows.
**How to install WSL on Windows 10 & 11**
1. Open **Command Prompt** as an **Administrator** on your Windows machine.
2. Type the following to install the WSL on Windows 10 & 11 and press **Enter**.
   ```
   wsl --install -d Debian
   ```
   ![image](img/1.png)
3. After it is completed, it will open a `Debian Terminal` like shown the image below.
   ![image](img/2.png)
4. Enter **Username** and **Password** you want to set for your Debian App.
   ![image](img/3.png)
5. Now we will be running few commands on this terminal.
   ```
   $ sudo su -
   <Type Your Password>
   $ apt-get update
   $ apt install wget curl net-tools vim iputils-ping telnet make autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev git-core -y
   $ ln -s /mnt/c/Program\ Files/Docker/Docker/resources/bin/docker.exe /usr/local/bin/docker
   $ ln -s /mnt/c/Program\ Files/Docker/Docker/resources/bin/docker-compose.exe /usr/local/bin/docker-compose
   $ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
   $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
   $ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
   $ source ~/.bashrc
   $ git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
   $ rbenv install 2.7.6
   $ rbenv global 2.7.6
   $ gem install docker-sync
   $ echo "export DOCKER_HOST=tcp://127.0.0.1:2375" >> ~/.bashrc
   $ cd ~/
   $ wget --no-check-certificate https://caml.inria.fr/pub/distrib/ocaml-4.12/ocaml-4.12.0.tar.gz
   $ tar xvf ocaml-4.12.0.tar.gz
   $ cd ocaml-4.12.0
   $ ./configure
   $ make world
   $ make opt
   $ umask 022
   $ make install
   $ make clean
   $ cd ~/
   $ wget https://github.com/bcpierce00/unison/archive/refs/tags/v2.52.1.tar.gz
   $ tar xvf v2.52.1.tar.gz
   $ cd unison-2.52.1
   $ make UISTYLE=text
   $ cp src/unison /usr/local/bin/unison
   $ cp src/unison-fsmonitor /usr/local/bin/unison-fsmonitor
   ```
Now we will mount the Project or Github folder on Windows machine to this Docker terminal. Please make sure you change the `<path-to-project-folder-on-windows>` to the actual path on your Windows machine. If your project folder is in `C:\Github` drive, the path below would be `/mnt/c/Github`.
   ```
   $ mkdir /code
   $ mount --bind <path-to-project-folder-on-windows> /code
   $ echo "sudo mount --bind <path-to-project-folder-on-windows> /code" >> ~/.bashrc && source ~/.bashrc
   ```
Now 
Add the following at the bottom of the file, replacing "username" with your WSL username.
   ```
   username ALL=(root) NOPASSWD: /bin/mount
   ```

## HyperV and Docker setup
1. Open `Docker Desktop` application on your Windows machine and goto `Settings` and in `General` tab please enable the checkbox for `Expose daemon on   tcp://localhost:2375 without TLS` as shown in the image below.
   ![image](img/docker1.png)

2. Go to `WSL Integration` under `Resources` tab and enable the integration with the newly setup Linux Distro, in our case it is Debian and click on `Apply & restart` as shown in the image below.
   ![image](img/docker2.png)

3. Open `Control Panel` on your Windows machine and click on `Programs and Features`, after that click on `Turn Windows features on or off`, it will open a small window, please make sure that **Hyper-V** option is enabled in that small window.
   ![image](img/windows1.png)

4. Restart your computer.