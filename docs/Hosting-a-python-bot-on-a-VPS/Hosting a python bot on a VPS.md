# Hosting a python bot on a VPS
## Table of content
## Prerequisites
To be able to follow this tutorial, you will need:

- A hosting provider (VPS, dedicated server, raspi in your local network, etc.)
- Remote access to a host
- Root access to a host
- A script to run
- To be able to connect to the host, you will need a SSH client. For Linux and Mac, it's native. For Windows, there are several alternatives, the most popular being https://putty.org/. A good solution, for ease of use, is to use a terminal manager such as https://mremoteng.org/ (open source).
- A SFTP client, optional, if your code is not in a git repo (https://filezilla-project.org/download.php?type=clientis crossplatform for example)

In the following tutorial, the following variables will be used:

- [root_user] : the root user initially provided by your hosting provider
- [sudo_account] : the user you created following the tutorial
- [server_ip] : the IP of your server

## Notes
This tutorial will consider that the host is provided with a debian or debian based distribution.
## A few Linux commands, helps and references
A QWERTY layout:

![[Pasted image 20230510215028.png]]

| Reference | Description                                |
| --------- | ------------------------------------------ |
| .         | Current directory                          |
| ..        | Previous directory                         |
| ~         | Home directory (/home/[your_current_user]) |
| ^         | Ctrl key (^X is ctrl+x for example)        |
| M or Meta | Alt key (M-U is Alt+U for example)         |
| `$`       | The beginning of a command line            |

| Command     | Description                                                 | Example                    |
| ----------- | ----------------------------------------------------------- | -------------------------- |
| cd          | Change directory                                            | `cd ~/scripts`             |
| ls -l       | List content of directory as list                           | `ls -l .`                  |
| cat         | Display the whole content of a file                         | `cat ./main.py`            |
| tail -n 250 | Display the last 250 lines of a file (useful to check logs) | `tail -n 250 ./output.log` |
| nano        | Edit a file                                                 | `nano ./main.py`           |
| nohup       | Run a command without needing a shell                       | `nohup ./main.py`                           |

## Preparing the host
### Connect to the host with the provided account
#### Windows
TBD, but do the same thing as below using your client of choice pretty much
#### Linux & Mac
Open a terminal and type the following command:

```shell
$ ssh [root_user]@[server_ip]
```

This should ask for your password, that you will have to enter, or paste (be careful, ctrl+v is not often supported in terminals. You might have to use the middle click, or right click)

![[Pasted image 20230510195235.png]]

If everything goes right, you will be greeted by what is called a banner.
Below that, you'll have access to your server's shell (which might not look like the screenshot, but the idea is there)

![[Pasted image 20230510195152.png]]
### Update the host
Before anything, we'll ensure that the host is up to date.

```shell
$ apt update && apt upgrade
```

An example of an output:

```shell
$ apt update && apt upgrade
Hit:1 http://download.draios.com/stable/deb stable-amd64/ InRelease
Hit:2 http://mirrors.linode.com/debian bullseye InRelease
Get:3 http://mirrors.linode.com/debian-security bullseye-security/updates InRelease [48.4 kB]
Get:4 http://mirrors.linode.com/debian bullseye-updates InRelease [44.1 kB]
Get:5 http://mirrors.linode.com/debian-security bullseye-security/updates/main Sources [190 kB]
Get:6 http://mirrors.linode.com/debian-security bullseye-security/updates/main amd64 Packages [239 kB]
Fetched 521 kB in 4s (138 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages were automatically installed and are no longer required:
	apache2-bin apache2-data apache2-utils bind9utils bsdmainutils cpp-8 dh-python g++-6 gconf-service gconf2-common imagemagick-6-common libaom0 libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libasan5 libb64-0d libbabeltrace-ctf1 libbind9-140 libbind9-161
	libblas-common libboost-filesystem1.62.0 libboost-iostreams1.62.0 libboost-iostreams1.67.0 libboost-system1.62.0 libboost-system1.67.0 libcroco3 libcwidget3v5 libdav1d4 libdbus-glib-1-2 libde265-0 libdns-export1104 libdns1104 libdns1110 libdns162 libegl-mesa0 libegl1
	libegl1-mesa libevent-2.1-6 libfftw3-double3 libgbm1 libgconf-2-4 libgdk-pixbuf-xlib-2.0-0 libgdk-pixbuf2.0-0 libgfortran3 libgfortran5 libheif1 libicu57 libicu63 libirs141 libirs161 libisc-export1100 libisc1100 libisc1105 libisc160 libisccc140 libisccc161 libisccfg140
	libisccfg163 libisl19 libjemalloc1 libjq1 libjson-c3 libjsoncpp1 liblinear3 libllvm7 liblockfile-bin liblockfile1 liblqr-1-0 libltdl7 liblua5.2-0 libluajit-5.1-2 libluajit-5.1-common liblvm2app2.2 liblvm2cmd2.02 liblwres141 liblwres161 libmagickcore-6.q16-3
	libmagickcore-6.q16-6 libmagickwand-6.q16-3 libmagickwand-6.q16-6 libmpdec2 libonig4 libonig5 libopenjp2-7 libperl5.28 libprocps7 libpython2-dev libpython2-stdlib libpython2.7 libpython2.7-dev libpython2.7-minimal libpython2.7-stdlib libpython3.5 libpython3.5-dev
	libpython3.5-minimal libpython3.5-stdlib libpython3.7 libpython3.7-dev libpython3.7-minimal libpython3.7-stdlib libreadline5 libruby2.3 libruby2.5 libstdc++-6-dev libtinfo-dev libunbound2 libwayland-egl1-mesa libwayland-server0 libwebpdemux2 libwebpmux3 libx265-165 libx265-192
	linux-headers-4.19.0-16-common linux-headers-4.19.0-17-common linux-headers-4.19.0-21-common linux-headers-4.19.0-22-common linux-headers-4.19.0-6-common linux-headers-4.9.0-11-amd64 linux-headers-4.9.0-11-common linux-headers-4.9.0-8-amd64 linux-headers-4.9.0-8-common
	linux-headers-5.10.0-19-amd64 linux-headers-5.10.0-19-common linux-image-4.19.0-16-amd64 linux-image-4.19.0-17-amd64 linux-image-4.19.0-21-amd64 linux-image-4.19.0-22-amd64 linux-image-4.19.0-6-amd64 linux-image-4.9.0-11-amd64 linux-image-4.9.0-8-amd64
	linux-image-5.10.0-19-amd64 linux-kbuild-4.19 netcat-traditional perl-modules-5.28 php-common python-pkg-resources python2 python2-dev python2-minimal python2.7 python2.7-dev python2.7-minimal python3.5 python3.5-dev python3.5-minimal python3.7-minimal ruby-did-you-mean ruby2.3
	x11proto-input-dev x11proto-kb-dev
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```
#### Create a less privileged user
Since connecting using a root account is a *very bad practice*, we will be creating a user account that you will use for the rest of the tutorial.

Run the following command to create a new user:

```shell
$ adduser [sudo_user]
```

This should ask you for a password, be aware that this will most likely be using QWERTY layout. Set up a password with at least 12 characters, using a mix of uppercase, lowercase, special characters and number.
I would **highly** advise that you use a password manager both to save and to generate your passwords.

```shell
$ adduser [sudo_user]
Adding user '[sudo_user]' ...
Adding new group '[sudo_user]' (1001) ...
Adding new user '[sudo_user]' (1001) with group '[sudo_user]' ...
Creating home directory '/home/[sudo_user]' ...
Copying files from '/etc/skel' ...
Enter new UNIX password: ******
Retype new UNIX password: ******
```

If you want to change the password for the user afterwards, you can use the following command:

```shell
$ passwd [sudo_user]
Enter new UNIX password: ******
Retype new UNIX password: ******
```

While we want the user to be less privileged by default, we still want to be able to run privileged commands on demand, so we need to add this user to the group 'sudo'.

```shell
$ usermod -aG sudo [sudo_user]
```

This should be enough to allow ssh connections.
### Testing the new account
Repeat the chapter [[Hosting a python bot on a VPS#Connect to the host with the provided account]] using the new account information.

If everything is ok, disconnect from the previous ssh session that is using [root_user] by typing in the console:

```shell
$ exit
```
## Install the software
### Install the python environment manager
Because python is a clusterfuck and to avoid bricking the system, we will be using a virtual environment manager for python, that will make you able to have a dedicated version of python for each script that you use. 

That avoids conflicts, and makes everything smoother in the long run.

Start by installing the prerequisites:

```shell
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \
libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl
```

Then install pyenv itself:

```shell
$ curl https://pyenv.run | bash
```

At the end of the install, you should see something like:

```shell
WARNING: seems you still have not added 'pyenv' to the load path.

Load pyenv automatically by adding
the following to ~/.bashrc:

export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

Run the following commands to add pyenv to your path, as the warning above asks you to do:

```shell
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
$ echo 'eval "$(pyenv init -)"' >> ~/.profile
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ exec "$SHELL"
```

Update pyenv by running:

```shell
$ pyenv update
```

Create the working directory where you'll upload your scripts:

```shell
$ mkdir ~/scripts
```
### Install a script
#### Transfer your script to the host
If you're using a git repository, I will consider that you know how to clone a repo.
This part is for people that have the code on their computer and need to transfer it to the host.

For this, you will need to use your SFTP client, setup as follow:

* Host: sftp://[server_ip]
* Username: [sudo_user]
* Password: [sudo_user_password]
* Port: 22

If something asks you if you trust this host, say yes.

If you use Filezilla, it'll look something like:

![[Pasted image 20230510204111.png]]

On the left side, you'll see your computer's content, on the right side, you'll see the server.

Transfer your scripts into separate folders on the server, in the /home/[sudo_user]/scripts/MyBot directory that was created earlier (you can drag and drop).
#### Setting up a script
##### Setting up the virtual environment
For better comprehension, the following part will consider that your script is in the */home/user/scripts/MyBot* directory, and has a starting point in the file *main.py*

Enter the working directory:

```shell
$ cd /home/[sudo_user]/scripts/MyBot
```

Install a local python environment (you can change the version if you need a specific one, I'll use the latest stable at the time of writting this tutorial):

```shell
$ pyenv install 3.11.3
$ pyenv virtualenv 3.11.3 venv_MyBot # replace the MyBot name by the name of your script/bot
```

Then, activate the virtual env

```shell
$ pyenv activate venv_MyBot # replace the MyBot name by the name of your script/bot 
```

You should see something like *(venv_MyBot)* preceding your shell, which mean that you're using the newly created virtual environment.

For example, on my specific shell (which is customized), it looks like:

![[Pasted image 20230510210539.png]]
##### Setting up the actual script
Now, you should be able to run your script! (maybe)

Run the following command:

```shell
(venv_MyBot) $ python3 ./main.py
```

If there is no error, nice, it's almost done and you can skip the rest of this specific chapter.
If not, you'll most likely be missing some dependencies, with a message looking like:

```shell
ModuleNotFoundError: No module named 'schedule'
```

It means that you need to install the module named "schedule" in this case.

To install it, run the following command:

```shell
(venv_MyBot) $ pip3 install schedule
Collecting schedule  
	Using cached schedule-1.2.0-py2.py3-none-any.whl (11 kB)  
Installing collected packages: schedule  
Successfully installed schedule-1.2.0  
WARNING: You are using pip version 22.0.4; however, version 23.1.2 is available.  
You should consider upgrading via the '/home/user1/.pyenv/versions/3.10.4/envs/MyBot/bin/python3.10 -m pip install --upgrade pip' command.
```

Then, retry running the script, and rinse and repeat until it works.
Once it does, run the following command:

```shell
(venv_MyBot) $ pip3 freeze > requirements.txt
```

While not mandatory, this will make you able to reinstall the script with a single command the next time.

The command to install from the requirements.txt is, for information (don't need to run this now, since everything is already installed):

```shell
(venv_MyBot) $ pip3 install -r requirements.txt
```
### Running the script in background
> **Warning**
>
> Make sure that you have activated the virtual env before running the command below, especially after connecting in a new session.
> 

While the script is running good when you start it manually, it will stop when you close your terminal, which we do not want.

To be able to keep the script running when you leave the server, you will need to use the *nohup* command.

```shell
(venv_MyBot) $ nohup python3 ./main.py > output.log 2>&1 &
```

The command above uses nohup to tell the shell to run the command in a separate process, and redirects its output to a log file named output.log, that you can read to see if everything is working as intended, or if any error was generated.

After running the command, you can directly press "Enter" to check if it didn't instantly fail for some reason. Most of the time, it fails because you forgot to activate the python virtual environment.
