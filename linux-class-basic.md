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

### grep(Global Regular Expression Print)  
\-i : case-insensitive  
\-v : inverts the match, showing lines that do not contain the pattern.  
\-r or \-R : Recursively searches directories.  
\-l : Lists file names only if the file contains the pattern.  
\-n : Displays line numbers alongside each matching line.  
\-c : Counts the number of lines that match the pattern.  

```
grep -i "apple" fruits.txt
grep -v "apple" fruits.txt
grep -r "apple" /path/to/directory
grep -l "apple" /path/to/directory/*
grep -n "apple" fruits.txt
grep -c "apple" fruits.txt
cat fruits.txt | grep "apple"
grep -E "appl(e|es)" fruits.txt
```

### wc(word count)  
reads one or more input files and provides statistical information (such as the number of lines, words, characters)  
also take input from stdin  

\-l : counts the number of lines  
\-w : counts the number of words  
\-c : counts the number of bytes  
\-m : counts the number of characters  
\-L : gives the length of the longest line  

```
wc file.txt
wc file.txt file1.txt
echo "hello world" | wc
```

### head/tail  
- head  
outputs the beginning sections (default is the first 10 lines)  
\-n \[number\] Outputs the specified number of lines from the start.   
\-c \[number\] Outputs the specified number of bytes from the start.   
\-q Suppresses printing of file headers if multiple files are specified.  => 파일별 구분을 없앰.   

- tail
outputs the end section (default is the last 10 lines) of a text file or stdin  
\-n \[number\] Outputs the specified number of lines from the end.  
\-c \[number\] Outputs the specified number of bytes from the end.  
\-f follow mode.  
\-\-pid=\[PID\] stop following the file when the processes with PID exits.   

```
head -n 2 f1.txt
head f1.txt f2.txt
head -q f1.txt f2.txt

tail -n 2 f1.txt
tail -f --pid=1234 logfile.log
```

### diff/cmp  
- diff  
compare files line by line and report the differences between them.  
It shows the changes required to make one file match the other.  
\-c context mode, provices some context lines around the differences.  
\-u unified mode, shows differences together in a unified format.  
\-i ignore case differences.
\-r recursively compare directories.  
\-w ignore differences in white spaces.

- cmp  
compare two files byte by byte  
useful for binary files but works with text files as well  
exit status (0: files are identical, 1: files differ, 2: an error occurred)  
\-b prints the differing bytes  
\-i SKIP skips the first SKIP bytes of both files  
\-i SKIP1:SKIP2  skips different numbers of bytes for each file  
-l Prints the byte numbers(decimal) and the differing byte values in octal.  
-n LIMIT compares only up to LIMIT bytes.  
-s Silent mode, does not output differences, only returns the exit status to indicate if files differ.  

```
# f1.txt
hello world
hello world1
hello world1

# f2.txt
hello world
hello world2
hello world1

diff f1.txt f2.txt
# 2c2
# < hello world1
# ---
# > hello world2
# 참고) a - add, c - change, d - delete

diff -u f1.txt f2.txt
# --- f1.txt      2025-02-18 17:05:54.676849435 +0900
# +++ f2.txt      2025-02-18 17:06:17.049154561 +0900
# @@ -1,3 +1,3 @@
#  hello world
# -hello world1
# +hello world2
#  hello world1

cmp f1.txt f2.txt
# f1.txt f2.txt differ: char 24, line 2  -> 2번째 라인의 24 바이트부터 다르다. 

cmp -l f1.txt f2.txt
# 24 61 62  -> 24바이트 위치에서 ascii 61 -> 62로 바꿔야한다. 

cmp -b f1.txt f2.txt
# f1.txt f2.txt differ: byte 24, line 2 is  61 1  62 2

```
