cat /etc/hostname
Filesystem Hierarchy Standard (FHS)

/proc 
A virtual filesystem that provides a mechanism for kernel-space to send information to user-space.
It contains runtime system information 
커널이 시스템과 프로세스 정보를 보여줌(읽기 전용 데이터)

/var
Stands for "variable," this directory contains files that are expected to grow over time, 
such as logs, spool files, and cached data

/run
A temporary filesystem that transient state file since an early boot stage. 
It is used for non-persistent application state or runtime data, such as process IDs or lock files.
부팅 후 서비스들이 동작하면서 필요한 임시 데이터 저장(쓰기 기능, PID파일, 소켓 등) 

/mnt
A common mount point for temporarily mounting file system, like external storage devices

/media
Used for mounting removable media like USB devices, CD-ROMEs, etc.

/opt
Often used for the installation of third-party software.

touch 
create a new empty file 
lock file로 많이 사용한다. 

find [path] [expression]

locate [options] pattern 
searches for files by name, using a database 
because locate uses a pre-built database, its searches are faster than find for simple locating files by name.
locate "*.png"
locate -i "project" -> ignore case

whatis - display a one-line description of a command 

crontab 
-e: Edit the current user's crontab file.
-l: List the current user's cron jobs.
-r: Remove the current user's crontab.
-u <user>: Specify which user's crontab to manage 

m h dom mon dow    command
*/10 - every 10 minutes 

chown
-R: Recursively change ownership for all files and directories within a specified directory
chown john:developers file.txt
chown -R john /home/john

chmod
-R: recursively 
chmod 755 file.txt
chmod g+w file.txt
chmod -R a+rx /project

chgrp 
-R: recursively  
=>보통은 chown을 많이 쓴다. 

### Environment Variable  
- PATH, HOME, USER, SHELL, TERM, PWD, LD_LIBRARY_PATH, JAVA_HOME
- View Environment Variables   
env, printenv, set(shell var + env var)
- export <VAR>=<VALUE> => 다른 세션에서도 사용 가능하게 됨
- echo 또는 쉘 스크립트에서 $HOME와 같은 형식으로 사용이 가능함.
- /etc/environment or /etc/profile => for all users  
~/.bashrc, ~/.bash_profile, ~/.profile => for a specific user
.profile은 bash가 아니어도 호출된다. => 좀 더 generic 하다.    
.bash_profile이 없는 경우 .profile이 호출된다.  
.bashrc - runs every time a new interactive, non-login shell is started
.bash_profile - runs once at login for login shells.
로그인을 하는 경우 => .bash_profile 이후 .bashrc 호출됨.(일반적으로 호출하도록 설정을 해준다. . ~/.bashrc) 
새로운 쉘을 여는 경우 => .bashrc 호출됨.
```
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

### SSH  
```
ssh username@hostname
ssh -p 2222 ubuntu@localhost

ssh-keygen -t rsa -b 4096 -f ~/.ssh/custom_rsa_key

ssh-copy-id -p 2222 -i ~/.ssh/custom_rsa_key.pub ubuntu@localhost
cat ~/.ssh/custom_rsa_key.pub | ssh -p 2222 ubuntu@localhost "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys
ssh -i ~/.ssh/custom_rsa_key -p 2222 ubuntu@localhost

scp localfile.txt username@hostname:/remote/directory/
scp -p 2222 localfile.txt username@hostname:/remote/directory/

sftp username@hostname
sftp -P 2222 username@hostname

exit
```


