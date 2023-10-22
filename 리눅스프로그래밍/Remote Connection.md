연결을 시도하는 Host OS(Client)는 Windows이며, 접속을 허가하는 Guest OS(Server)는 CentOS Fedora. 
### SSH Protocol
#### Linux 환경 구축
```shell
$ su root
$ vim /etc/selinux/config
	SELINUX=disabled
$ reboot
$ yum install -y openssh openssh-server openssh-client
$ systemctl enable sshd
$ systemctl start sshd
$ firewall-cmd --permanent --add-service=samba
$ firewall-cmd --reload
```
#### Xshell & Xftp
Host OS에서 <a href="https://www.netsarang.co.kr">www.netsarang.co.kr</a>에서 Xshell 및 Xftp 설치 후 접속
Xshell은 Linux shell을 이용할 수 있게 해주며 Xftp는 파일을 인터넷 통신으로 복사, 수정, 생성 등을 가능하게 해준다.
#### Putty
Host OS에서 <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/">www.chiark.greenend.org.uk/~sgtatham/putty/</a> 에서 putty 설치 후 접속. 
putty는 Xshell과 유사한 기능을 제공
#### EditPlus
Host OS에서 <a href="https://www.editplus.co.kr">www.editplus.co.kr</a>에서 EditPlus 설치 후 접속
text 편집기로 서버에 존재하는 파일에 접근해 파일을 쉽게 text를 통해 바로 수정할 수 있게 해줌
### SMB/CIFS Protocol
#### Samba Server Setup
```shell
$ su root
$ vim /etc/selinux/config
	SELINU=disabled
$ reboot
$ yum install -y samba samba-client
$ smbpasswd -a user1
$ systemctl enable smb
$ systemctl start smb
$ firewall-cmd --permanent --add-service=samba
$ firewall-cmd --reload
```

#### Samba Client Setup
Window 탐색기 - [도구] - [네트워크 드라이브 연결]을 통해 연결. 이때 설정한 user name, password를 입력해야 접속이 가능하다
### VNC Protocol

#### VNC Server Setup
```shell
$ su root
$ yum groupinstall "GNOME Desktop"
$ yum -y install tigervnc-server tigervnc
$ cd /etc/systemd/system/
$ cp /lib/systemd/system/vncserver@.service ./vncserver@:2.service
$ vim ./vncserver@:2.service
	ExecStart=/usr/bin/vncserver_wrapper user1 %i
$ su user1
$ vncserver          # set passwd
$ firewall-cmd --permanent --add-port=5902/tcp
$ firewall-cmd --reload
$ vncserver :2       # start and set passwd
$ vncserver -kill 2  # stop vncserver
```
#### VNC Clinet Setup
<a href="https://tigervnc.org">tigervnc.org</a>에 접속해 tigervnc 설치 후 IP 주소를 입력하여 접속