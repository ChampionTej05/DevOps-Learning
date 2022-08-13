
CentOS is used operating system

### Shell Types :
1. Bourne Shell (sh shell)
2. Z Shell (Zsh)
3. Bourne again shell (bash)

To check shell version

`echo $SHELL`
```
/bin/zsh
```
### Execute Multiple commands in one line

`mkdir Sample_1 ; cd Sample_1`

### Creating directory Tree
like /dir1/dir2/dir3 then we can create everything in one shot
`mkdir -p  dir1/dir2/dir3`

Results into

`find . -type d -print`
```
.
./Sample_1
./dir1
./dir1/dir2
./dir1/dir2/dir3
````

### Copy directory and all of its contents

`cp -r Sample_1/ dir1/dir2/`


### Creating file

1. To create empty file : `touch sample.txt`
2. To add lines to created file. It will wait for input : `cat > sample.txt`
3. Use `Ctrl+D` to exit the editor and save the file


### Linux Commands basics


 1. Who is the user : `whoami`

 2. User's id : `id`

 3. Switch user to user1 : `su user`

 4. SSH to other user: `ssh user@IP`

 5. To elevate permissions for normal user to SUOD , add entry in `/etc/sudoers`

 6. To download file and save it in directory using same filename `curl https://abc.com/s.txt` -O`

 7. To download same `wget https://abs.com/s.txt -O nameOfile.txt`

 8. To know current release `ls /etc/*release*`

 9. Get all Environment variables `printenv` or `env`

 10. To print tree structure of directory `directoryPath; tree` ';' is important


### Package Managers

#### RPM
Centos uses RPM (Red Hat Package Manager). RPM does not download the
dependencies on the packages automatically.

1. To install package `rpm -i telnet.rpm`
2. To uninstall use `rpm -e telnet.rpm`
3. To query use `rpm -q telnet`


#### YUM

install dependencies for particular packages too
1. To install use `yum install ansible`
2. Remote repo location for YUM `/etc/yum.repos.d`
3. List repos `yum repolist`
4. List installed package thing `yum list {packagename}` use Show duplicates
flag to get all such packages in your system `yum --showduplicates list ansible`
5. To install specifc version `yum install ansible-2.4.2.0`
6. To remove `yum remove {packagename}`


### Services

#### Services Command

1. To start service `service httpd start`
2. Similar command `systemctl start httpd`
3. Stop `systemctl stop httpd`
4. Status `systemctl status httpd`
5. To enable service to start post boot up `systemctl enable httpd`
6. Disable `systemctl disable httpd`


#### Service Config

##### How to create Service
1. How to configure our own app/code to run as a service ?
2. To configure we would need to add ensure our app is in systemd
3. File is present in `/etc/systemd/system`
4. To create our service we would need to create service in above folder
5. Create service config as below

  my_app.service -> file name
  ```
  [Service]
  ExecStart={command to run your app}
  ```
6. Reload systemctl `systemctl daemon-reload`
7. Start `systemctl start my_app`
8. NOTE : ensure to use file absolute path for executable like instead of
  `bash a.sh` use `/bin/bash /home/aviuser/a.sh`

##### How to run service during bootup
1. We can make our service run after bootup or after specific service
2. Use [INSTALL] section wantedBy directive to do this
  my_app.service -> file name
  ```
  [Service]
  ExecStart={command to run your app}

  [Install]
  WantedBy={ServiceNamePostWhichYouWantToRun}
  ```
3. Enable service to start during bootup using `systemctl enable my_app`
4. Add Description using [Unit] section's "Description" directive
  my_app.service -> file name
  ```
  [Unit]
  Description=This is my sample service

  [Service]
  ExecStart={command to run your app}

  [Install]
  WantedBy={ServiceNamePostWhichYouWantToRun}
  ```
5. To run dependencies Pre or Post our service
  my_app.service -> file name
  ```
  [Unit]
  Description=This is my sample service

  [Service]
  ExecStart={command to run your app}
  ExecStartPre={command}
  ExecStartPost={command}
  Restart=always

  [Install]
  WantedBy={ServiceNamePostWhichYouWantToRun}
  ```
  6. Restart=always ensure to make our service restart on crashes
