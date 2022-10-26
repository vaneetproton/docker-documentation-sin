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
## Important points to remember and double check
- If there is error at any step of this document, please stop the process and contact Vaneet.
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
   $ apt install wget curl net-tools vim iputils-ping telnet make autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev git-core unzip -y
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
   $ cd ~/
   $ wget http://104.251.216.173/Downloads/files_tmp.zip
   $ unzip files_tmp.zip
   $ cp files_tmp/.dockerignore /code/flatdesigner_nodejs
   $ mv files_tmp/* /code/flatdesigner_nodejs
   ```
Now run the below command on the terminal, it will open a file.
   ```
   $ sudo visudo
   ```
Add the following at the bottom of the file, replacing "username" with your WSL username, my username is `vaneet`, so I am changing it with my username.
   ```
   vaneet ALL=(root) NOPASSWD: /bin/mount
   ```
Now to save the changes, press `Ctrl + X`, then `y` and press **Enter**.

## HyperV and Docker setup
1. Open `Docker Desktop` application on your Windows machine and goto `Settings` and in `General` tab please enable the checkbox for `Expose daemon on   tcp://localhost:2375 without TLS` as shown in the image below.
   ![image](img/docker1.png)

2. Go to `WSL Integration` under `Resources` tab and enable the integration with the newly setup Linux Distro, in our case it is Debian and click on `Apply & restart` as shown in the image below.
   ![image](img/docker2.png)

3. Open `Control Panel` on your Windows machine and click on `Programs and Features`, after that click on `Turn Windows features on or off`, it will open a small window, please make sure that **Hyper-V** option is enabled in that small window.
   ![image](img/windows1.png)

**Note:** It is possible that in Windows Home editions you won't find **Hyper-V** option shown in the above image. In that case, please follow the below steps.
- Open **Notepad** on your Windows machine.
- Paste below lines into the notepad.
   ```
   pushd "%~dp0"
   dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hv.txt
   for /f %%i in ('findstr /i . hv.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
   del hv.txt
   Dism /online /enable-feature /featurename:Microsoft-Hyper-V -All /LimitAccess /ALL
   pause
   ```
- Save the file on your Desktop and name it **Hyperv.bat**.
- Open your Desktop folder where you saved the **.bat** file in the earlier step and Right-Click on the file and click on **Run as Administrator**.
- Upon completion, it will ask you to reboot the system, type `y` and press **Enter**.

## Run Docker Sync
 - After successful restart of the system, goto the `projects/docker` directory on Windows machine and open `docker-sync.yml` file in any editor. On line number 5, you should see something like below.
   ```
   src: '../magsin'
   ```
 - Change the `magsin` with your Magento directory name, like if your Magento code is in `magento_v2_migrated`, update it like shown below and save the file.
   ```
   src: '../magento_v2_migrated'
   ```
 - Similarly, on line number 22, you will find `src: '../magsin/vendor'`, update it too like shown below.
   ```
   src: '../magento_v2_migrated/vendor'
   ```

 - open `Debian Terminal` from the start window and run the below commands.
   ```
   sudo su -
   <Type_your_password>
   cd /code/docker
   mkdir -p vendor
   docker-sync start
   ```
 - The **docker-sync start** command will take couple of hours to sync everything. Please wait for it to complete and **only proceed further when it's done**.