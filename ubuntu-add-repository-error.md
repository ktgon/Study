#### 별도의 repository를 등록하여 package를 설치하는 경우 오류 발생시 처리 방안  

1. /etc/apt/sources.list 파일에 llvm-19 repository 추가 후 sudo apt update 수행  
\# llvm-19  
deb http://apt.llvm.org/noble/ llvm-toolchain-noble-19 main  
deb-src http://apt.llvm.org/noble/ llvm-toolchain-noble-19 main
```
=> 아래 오류 발생  
W: GPG error: https://apt.llvm.org/noble llvm-toolchain-noble-19 InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 15CF4D18AF4F7421  
E: The repository 'http://apt.llvm.org/noble llvm-toolchain-noble-19 InRelease' is not signed.  
N: Updating from such a repository can't be done securely, and is therefore disabled by default.  
N: See apt-secure(8) manpage for repository creation and user configuration details.
```
2. /etc/apt/apt.conf.d/99verify-peer.conf 생성 후 아래 내용 추가  
Acquire { https::Verify-Peer false }

3. 다시 sudo apt update 수행
동일 오류 발생함.

4. 아래 웹페이지를 참조하여 GPG 키를 추가함.  
https://blog.logger.one/entry/Ubuntu-%EC%97%90%EC%84%9C-GPG-ERROR-NOPUBKEY-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95

5. 이 후 아래 명령 처리 성공함.
```zsh
❯ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv AF4F7421
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
Executing: /tmp/apt-key-gpghome.c2k9h5dehj/gpg.1.sh --keyserver keyserver.ubuntu.com --recv AF4F7421
gpg: key 15CF4D18AF4F7421: public key "Sylvestre Ledru - Debian LLVM packages <sylvestre@debian.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1

> sudo apt update
> sudo apt install llvm-19 clang-19 lldb-19
```
