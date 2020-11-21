# Linux#  #
## Module 1. Linux 介紹

- 1977 BSD

## Module 2. 發行版本

- **RedHat/CentOS**		<br>
佔有率50%, 防火系統好, 企業/公有雲用
- **Debian/Ubuntu**		<br>
佔有率50%, 開源, 私有雲用
- **FreeBSD**		<br>
最早發表, 學術網路用


## Module 3. 安裝規劃

- VMware (enterprise standard)
- VM player (free)
- VirtulBox (Sun developed)
- Hyper-V (Microsoft)

## Module 4. 安裝系統
## Module 5. Post Installation 安裝後續

## Module 6. 初次使用系統

- ubuntu server: 遠端文字模式登入

- 確定系統ssh server有啟動: `lsof -nPi` 22 listen (Alpine not default install)
- `netstat -tulpn` :在Alpine(Hadoop授課VM) 可用來查看listen port


- client 端使用終端機模擬器Putty(free tool)
- putty 無法接desktop terminal
- console: 主控台
- console login: 本機桌面登入
- poweroff 是個有特別開放給"非"root的指令，所以主機通常會放在有門禁的機房
- `alias list=ls` 用list替代ls

## Module 7. 查指令說明

ls/cp/cat/rm

	buntu@ubuntu99:~$ man man

       1   Executable programs or shell commands							#for user
       2   System calls (functions provided by the kernel)
       3   Library calls (functions within program libraries)
       4   Special files (usually found in /dev)
       5   File formats and conventions, e.g. /etc/passwd
       6   Games
       7   Miscellaneous (including macro packages and conventions),  e.g.  man(7), groff(7)
       8   System administration commands (usually only for root)			#for root
       9   Kernel routines [Non standard]



## Module 8. 檔案系統

FHS: Filesystem Hierarchy Standard

- /etc    ：設定檔
- /sbin   ：
- /usr   ：系統軟體資源 Unic software resource
- /usr/sbin    ：
- /var: Variable 日常郵件，印表機後台，部分軟體log

- redHat and debian 預設路徑/顯示方式不同

`ls -h` :human readable


## Module 9. 基本檔案管理

`mkdir rmdir cp mv rm`	<br>
`mv 123.txt 123.txt` : redhat會問Y or N 要不要覆蓋，debian不會問 

## Module 10. 處理文字檔內容

`wc cat head tail less`

## Module 11. I/O重導與管線
### > >> 
輸出導向, `>`create new file ,`>>` append to exist file<br>

標準錯誤輸出, file 2>&1

	ubuntu@ubuntu99:~$ ls -l dir > lsError.log 2>&1
	ubuntu@ubuntu99:~$ cat lsError.log 
	ls: cannot access 'dir': No such file or directory
	ubuntu@ubuntu99:~$ 

- 什麼是檔案描述符？

檔案描述符是一個簡單的正整數，用以標明每一個被程式所開啟的檔案和socket。
最前面的三個檔案描述符（0，1，2）分別與標準輸入（stdin），標準輸出（stdout）和標準錯誤（stderr）對應。

- Unix/Linux/BSD 都有三個特別檔案，分別:

	1. 標準輸入 即 STDIN , 在 /dev/stdin , 一般指鍵盤輸入, shell裡代號是 0
	2. 標準輸出 STDOUT, 在 /dev/stdout, 一般指終端(terminal), 就是顯示器, shell裡代號是 1
	3. 標準錯誤 STDERR, 在 /dev/stderr也是指終端(terminal), 不同的是, 錯誤資訊送到這裡shell裡代號是2  

- error `2` example:

		bigred@cvn68:~/0_test$ ls 123
		ls: cannot access '123': No such file or directory
		bigred@cvn68:~/0_test$ echo $?
		2

- `$?` show last command file description

		bigred@cvn68:~/0_test$ ls -ltr
		total 12
		drwxr-sr-x 5 bigred bigred 4096 Nov 17 10:09 bigboss
		-rwxr-xr-x 1 bigred bigred   21 Nov 17 10:22 rabbit.txt
		-rwxr-xr-x 1 bigred bigred  178 Nov 17 12:43 myboss.exe

		bigred@cvn68:~/0_test$ more myboss.exe
		#!/bin/bash
		
		if [ ! -d bigboss ]; then
		        mkdir -p bigboss/{bin,home/{ds101,ds102},tmp,mytest1}
		        [ "$?" == "0" ] && echo "bigboss created"
		else
		        echo "Directory bigboss exists"
		fi

		bigred@cvn68:~/0_test$ ./myboss.exe
		Directory bigboss exists



### <
輸入導向<br>
cat<<end, 遇到結束字元"end"cat停止

	buntu@ubuntu99:~$ cat <<end
	> 123
	> 234
	> end
	123
	234


- Linux bash shell 第一提示字元(first prompt): `$` (一般使用者) or `#` (系統管理員)
- Linux bash shell 第二提示字元(second prompt): `>`


- second prompt 表指令不完整 
- ctrl + C : 放棄錯誤(KeyboradInterrupt in Python3) 

		hadoop@ubuntu99:~$ eixt'
		> 12
		> '
		eixt
		12
		: command not found

- Python shell 第一提示字元 `>>>`
- PYthon shell 第二提示字元 `...`

		hadoop@ubuntu99:~$ python3
		Python 3.8.5 (default, Jul 28 2020, 12:59:40) 
		[GCC 9.3.0] on linux
		Type "help", "copyright", "credits" or "license" for more information.
		>>> ( 2+
		... 
		... 1
		... 
		... )
		3



### |
管線


## Module 12. 資料串流指令
tr, cut, sort/uniq (少用到)

## Module 13. 檔案搜尋
### 萬用字元(Wildcard, Pattern Matching, File globbing): 用於搜尋檔案名稱

- `'` 單引號:不解解釋和特殊符號意義
- `"` 雙引號:
- ```` 反引號(倒單引號)，取得指令執行結果，相當於**bash shell script**，相當於$()

腳本表no need compile script(ex. python, bash)		<br>

- 找/etc/下含有數字的檔名

- 找非英文字開頭檔名 `^`Represents any character not in the brackets

		ubuntu@ubuntu99:~$ ls -d /etc/[^a-z]*
		/etc/NetworkManager  /etc/PackageKit  /etc/UPower  /etc/X11
- `?` Represents a single character

		buntu@ubuntu99:~/myweng/wildcard$ ls
		apple  bbplus  bl  black  blue  hat.txt
		ubuntu@ubuntu99:~/myweng/wildcard$ ls h?t.txt
		hat.txt

- LInux無附檔名, 123.txt 可能不是真的txt

### which
which後面接完整檔名，不可使用萬用字元<br>

	ubuntu@ubuntu99:~$ which python3
	/usr/bin/python3

`echo $PATH` PATH:bash的環境變數，like Python的global var

- root/user 看到的PATH不同
		
		ubuntu@ubuntu99:~$ echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

		root@ubuntu99:~# echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

- history <br>
--Ubuntu `history`是bash shell內建的指令 (比較正規)<br> 
--red 是外部指令<br>
--`man history` 在第三章，非第一章(使用者可用)及第八章(系統管理員可用)，所以非ubunto內建指令<br>
-- `help` 查的到的是shell內建指令

###whereis
可以搜尋執行檔，原始檔，說明檔，系統設定檔

	ubuntu@ubuntu99:~$ whereis passwd
	passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man5/passwd.5.gz /usr/share/man/man1/passwd.1.gz /usr/share/man/man1/passwd.1ssl.gz

###find
`find` 當作救援用。可依照檔名/結尾檔名/Size/天數<br>

	ubuntu@ubuntu99:~$ find myweng/ -name hat*
	myweng/wildcard/hat.txt

	find foldername/. -name fileNmae -size fileSize - type f(file)/d(dir) -atime -7(past 7 days)

	



## Module 14. Regular Expression (RE)
#### RE: 用於搜尋文字檔內容

- `^word`: word當字首
- `word$`: word當結尾
- `W\{n,m\}`: n~m個W字符


For Linux environment, ' ' 讓shell不去解釋任何特殊符號的意義 <br>

	grep -n		#show result num line
	grep -v		#non-matching
	grep -i 	#ignore case distinctions
	grep -r 	#recursive

/etc/passwd  #also show system account + user account <br>



## Module 15. 延伸Regular Expression (RE)

- `egrep` deprecated 即將廢棄, 用 `grep -E`。 (obsolete表已經廢棄)
- 特殊符號: `+`,`?`,`|`,`()`
- `sed -r` shell script使用，稱為stream editor,指令式的文書處理器 (ubunto using nana, like vi and gvim)
- `awk` shell command, like python3. AWK, K is C language author.


	```bash
	NAME
	       grep, egrep, fgrep, rgrep - print lines that match patterns
	SYNOPSIS
	       grep [OPTION...] PATTERNS [FILE...]
	       grep [OPTION...] -e PATTERNS ... [FILE...]
	       grep [OPTION...] -f PATTERN_FILE ... [FILE...]
	```

## Module 16. LVM系統

 可online extend檔案系統，先格式化兩顆硬碟，加入同一VG，from UNIX		<br>

- VG: Volume Group
- LV: Logical Volume
- PV: Physical Volume
- PE: Physical Extent
- LE: Logical Extend

- /dev/sda5: 從SATA切出第一顆硬碟當PE
	1. /dev 存放device file的目錄，每一個device file對應到某個硬碟
	2. s: SCI or SATA
	3. d: disk
	4. a: first disk
	5. logical partition	

### pvs/ vgs/ lvs

	root@ubuntu99:~# pvs
	  PV         VG       Fmt  Attr PSize    PFree 
	  /dev/sda5  vgubuntu lvm2 a--  <499.50g 32.00m

	root@ubuntu99:~# vgs
	  VG       #PV #LV #SN Attr   VSize    VFree 
	  vgubuntu   1   2   0 wz--n- <499.50g 32.00m		#500G幾乎切光
		
安裝時有選用LVM此指令lvs才有info			<br>
若沒有用LMS，硬碟爆掉就無法擴充(need enter console to solve)		<br>
		
	root@ubuntu99:~# lvs	
	  LV     VG       Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
	  root   vgubuntu -wi-ao---- 498.51g                                                    
	  swap_1 vgubuntu -wi-ao---- 976.00m
    


## Module 17. 檔案與目錄權限

`umask` Display or set file mode mask.		<br>

- `r` Read: file(car,less,head,vi,nano....), dir(ls,find)
- `w` Write: file(vi,vi,nano,>,>>...), dir(cp,rm,mv,>,>>,...)
- `x` Execute: file, dir(cd)

`ls -l`: show permission info

#### -/d/l rwx rwx rwx

- fileType: -(一般檔案) d(目錄) l(soft link)
- Owner permission: 檔案擁有者(owner)的權限
- Group permission: 檔案所屬群組(group)的權限
- Other permission: 其他人(other)的權限

`$` :一般user		<br>
`~`: home directory			<br>

`chmod` : change mod		<br>
組合7, 6 , 5, 4 :正常權限, 3,2,1 for management	<br>
`777` :權限全開

#### Modern Style

- Permission: e, w, x, a(all)
- action: +(授權), -(移除), =(置換)
- 對象: u(owner), g(group), o(other)

- `chmod 777` == `chmod a=rwx`
- `chmod 644` == `chmod u=rw,go=r`
- `chmod 600` == `chmod u=rw,go=`

#### RedHat and Unutu having big difference

Redhadt: default /home/user permission `drwx------` <-- this why Redhat with higer security	<br>
Ubuntu: default /home/user permission `drwxr-xr-x`			<br>    

Ubuntu /root  drwx------	<br>










## Module 18. 檔案與目錄授權

`chown`		<br> 
user: 同時修改user/group (此group同user login group);使用id查login group		<br>
只有root可執行chown


## Module 19. 檔案打包壓縮
## Module 20. 檔案解壓縮

	tar -tvf		#check tar content 
	tar -xvf		#extract
	tar -cvf		#creat

- `tar` 打包
- c,t,x (same type, can be choosed 1 type only)
         
		tar -c	#creat                                              
		tar -t	#list                                            
		tar -x	#extract 
		-v	#verbose mode(顯示詳細的處理過程)                     
		-f	#打包解壓縮的file name                                
		-C	#配合-x, 指定解開的路徑, 安裝hadoop時會用到            

#### 壓縮

- .tar.gz == .tgz (tar+gzip) 	**general, Linux/Windows/Mac OK
- .tar.bz2 == .tbz (tar+bz2)	**trend to phase out, after 2013, Linux kernel 官網不再提供bz2的壓縮檔 
- .tar.xz == .txz (tar+xz)		**技術新,壓縮比高, 7-zip usable

#### application path

- tar -xvf XXX.tar -C /usr/local	#第三方已編譯完成的應用程式 (open source)
- tar -xvf XXX.tar -C /opt			#廠商的應用程式 (vendor)



## Module 21. 文書處理器vi/vim

- vim: RedHat distribution (CentOS, RHEL, SuSE, Fedora...)  <br>
- nano: Debian distributions (Ubuntu...)
- vi: Optional (RedHat/Debian 都有提供)

	root@ubuntu99:~# apt show vim 
	Package: vim
	Version: 2:8.1.2269-1ubuntu5
	Priority: optional
	Section: editors
	Origin: Ubuntu



## Module 22. 文書處理器nano

### File Management

	Ctrl+G	# Display help text
	Ctrl+X # Close the current file buffer / Exit from nano
	Ctrl+O # save

### Edit 

	Alt+U # Undo the last operation
	Alt+N # show line number
	Ctrl+K # cut line
	Alt+U # paste line
	
	
Write the current file to disk
## Module 23. 帳號管理

UID: User ID		<br>
GIP: Group ID		<br>

- Root account: UID=0
- System account: UID <1000
- User account: UID>=1000

- Application account (for certain application managment) (don't use the account to login)

- `$6$`: SHA-512 加密方式, 不可逆
- `$6$............$` : SALT

		root@ubuntu99:/etc# tail -5  /etc/shadow 
		pulse:*:18474:0:99999:7:::
		gnome-initial-setup:*:18474:0:99999:7:::
		gdm:*:18474:0:99999:7:::
		ubuntu:$6$1Wee872Jp6MfrkuC$OaSeRaUf/PmM84Ol2EPtdCwf7Qy/8sfZy5gvjpED4wAVV1QX9gB22ruBY20EBhgqPBYWd6pMmTN.56jUgvIck0:18520:0:99999:7:::
		systemd-coredump:!!:18520::::::

- Debian distribution: root is disable.

- sudo -i
- grep '^root' /etc/shaodw'
- server Ubuntu: * 表停用
- Desktop Ubuntu: ! 表停用

		root@bdse8:~# grep '^root' /etc/shadow
		root:*:18474:0:99999:7:::

		root@ubuntu99:/etc# grep '^root' /etc/shadow
		root:!:18520:0:99999:7:::

#### 建置帳號

- `adduser hadoop`
 
		root@ubuntu99:~# adduser hadoop
		Adding user `hadoop' ...
		Adding new group `hadoop' (1001) ...
		Adding new user `hadoop' (1001) with group `hadoop' ...
		Creating home directory `/home/hadoop' ...
		Copying files from `/etc/skel' ...
		新 密碼： 
		再次輸入新的 密碼： 
		passwd: password updated successfully
		Changing the user information for hadoop
		Enter the new value, or press ENTER for the default
			Full Name []: 
			Room Number []: 
			Work Phone []: 
			Home Phone []: 
			Other []: 
		Is the information correct? [Y/n] 

- check new account:
- 3 files be upddated: group, passwd, shadow

		root@ubuntu99:/etc# ls -ltr | tail -10
		-rw-r--r--  1 root root      12 Oct 30 09:53 timezone
		lrwxrwxrwx  1 root root      31 Oct 30 09:53 localtime -> /usr/share/zoneinfo/Asia/Taipei
		drwxr-xr-x  5 root lp      4096 Oct 30 09:58 cups
		-rw-r--r--  1 root root    2784 Oct 30 10:00 passwd-
		-rw-r--r--  1 root root    1066 Oct 30 10:00 group
		-rw-r-----  1 root shadow   888 Oct 30 10:00 gshadow
		-rw-r--r--  1 root root      40 Oct 30 10:00 subuid
		-rw-r--r--  1 root root      40 Oct 30 10:00 subgid
		-rw-r-----  1 root shadow  1569 Oct 30 10:00 shadow
		-rw-r--r--  1 root root    2787 Oct 30 10:01 passwd

		root@ubuntu99:/etc# tail -3 passwd
		ubuntu:x:1000:1000:ubuntu,,,:/home/ubuntu:/bin/bash
		systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
		hadoop:x:1001:1001:,,,:/home/hadoop:/bin/bash

		root@ubuntu99:/etc# tail -3 shadow
		ubuntu:$6$1Wee872Jp6MfrkuC$OaSeRaUf/PmM84Ol2EPtdCwf7Qy/8sfZy5gvjpED4wAVV1QX9gB22ruBY20EBhgqPBYWd6pMmTN.56jUgvIck0:18520:0:99999:7:::
		systemd-coredump:!!:18520::::::
		hadoop:$6$YXP/FKV/UCzK.SGS$SE/f/Oxg5GM/VWq/Xown4HKncieywZ5DuCSXV8oztJ8M6JUSjbvmSRl8IA9ulXBldOcQdL5geGt/jnIHjKRhF1:18565:0:99999:7:::

#### Using new user "hadoop" to install hadoop

1. Using login account to login system
2. Using `sudo -i` to root, 安裝軟體<包含系統軟體(apt)and Python套件(pip3)>, exit to login account
3. Using `sudo - hadoop` switch to Hadoop ecosystem 管理員, then 架設/管理/使用 Hadoop ecosystem 應用程式, exit to login account

##### !!!!!注意!!!!! su 沒下 `-` 會帶不到hadoop的環境設定 (雖然身分有切換):

		ubuntu@ubuntu99:~$ su hadoop
		密碼： 
		hadoop@ubuntu99:/home/ubuntu$ id
		uid=1001(hadoop) gid=1001(hadoop) groups=1001(hadoop)

#### 刪除使用者

- Ubuntu: deluser --remove-home hadoop 

## Module 24. 群組管理

- 現代的Linux，採用 Private Group Scheme (PGS)的風格
- 每一個帳號都有一個專屬群組

		root@ubuntu99:~# grep '^hadoop' /etc/passwd /etc/group /etc/shadow
		/etc/passwd:hadoop:x:1001:1001:,,,:/home/hadoop:/bin/bash
		/etc/group:hadoop:x:1001:
		/etc/shadow:hadoop:$6$YXP/FKV/UCzK.SGS$SE/f/Oxg5GM/VWq/Xown4HKncieywZ5DuCSXV8oztJ8M6JUSjbvmSRl8IA9ulXBldOcQdL5geGt/jnIHjKRhF1:18565:0:99999:7:::

## Module 25. 軟體管理YUM (CentOS)

- rpm: 單一套件安裝 (`rpm -qa`: show所有已安裝套件)
- yum : 倉庫式的套件安裝，會自動處理套件相依性的問題 

yum安裝容易,rpm成查詢用途  <br>

yum.conf; *.repo        <br>

- yum update:升級所有套件(Be careful!!)

## Module 26. 軟體管理APT (Debian)

- `/etc/sources.list` : 更新repository 倉庫 (主設定檔)
- dpkg : 單一套件安裝 (已經不用來安裝及移除, replaced by apt)

		dpkg -l 			#show 已安裝套件
		dpkg -S keyword 	#反查

- **apt : 倉庫式的套件安裝，會自動處理套件相依性的問題**
- `/etc/apt/sources.list.d` :  (目錄，內含其他的子設定檔) (ex.mariaDB)

- apt -get update: 要自行更新，更新倉庫清單
- apt 是 apt-get 的簡單版

- apt-get upgrade : 更新系統升級 (be careful!!) (ex. OpenCV要確認升級後可否用)

- apt show package-name : 查詢
- apt install package-name : 安裝

- apt remove package : 移除套件(會保留設定檔)
- apt-get purge package : 移除套件(同時移除設定檔)
- apt autoremove : 移除不用的套件
- apt clean : 清除之前下載的安裝檔 (*.deb)

		apt show openjdk-11-jdk   
		...
		Depends: openjdk-11-jre (= 11.0.9+11-0ubuntu1~20.04), openjdk-11-jdk-headless (= 11.0.9+11-0ubuntu1~20.04), libc6 (>= 2.2.5)

		apt install openjdk-11-jdk		#start installing 
	
		root@bdse8:~# javac -version
		javac 11.0.9

		root@bdse8:~# java -version
		openjdk version "11.0.9" 2020-10-20
		OpenJDK Runtime Environment (build 11.0.9+11-Ubuntu-0ubuntu1.20.04)
		OpenJDK 64-Bit Server VM (build 11.0.9+11-Ubuntu-0ubuntu1.20.04, mixed mode, sharing)
	
ps. headless: without GUI


## Module 27. 行程管理
## Module 28. 背景程式
## Module 29. 系統日誌檢視

## Module 30. 系統開機

- GRUB: GRand Unified Bootloader

- GRUB Lagacy
- GRUB2 (after Ubuntu 12.04)

		ubuntu@ubuntu99:~$ dpkg -l | grep -i 'grub'
		ii  grub-common                                2.04-1ubuntu26.3                    amd64        GRand Unified Bootloader (common files)
		ii  grub-gfxpayload-lists                      0.7                                 amd64        GRUB gfxpayload blacklist
		ii  grub-pc                                    2.04-1ubuntu26.3                    amd64        GRand Unified Bootloader, version 2 (PC/BIOS version)
		ii  grub-pc-bin                                2.04-1ubuntu26.3                    amd64        GRand Unified Bootloader, version 2 (PC/BIOS modules)
		ii  grub2-common                               2.04-1ubuntu26.3                    amd64        GRand Unified Bootloader (common files for version 2)


## Module 31. 系統服務

- systemd系統，第一支啟動的程式稱為"systemd"

		ubuntu@ubuntu99:~$ pstree -hp 
		systemd(1)─┬─ModemManager(893)─┬─{ModemManager}(919)
		           │                   └─{ModemManager}(922)
		           ├─NetworkManager(787)─┬─{NetworkManager}(903)
		           │                     └─{NetworkManager}(904)
		           ├─VGAuthService(761)
		           ├─accounts-daemon(779)─┬─{accounts-daemon}(813)
		           │                      └─{accounts-daemon}(876)

		ubuntu@ubuntu99:~$ ls -l /usr/sbin/init 
		lrwxrwxrwx 1 root root 20 Sep 15 11:58 /usr/sbin/init -> /lib/systemd/systemd

- systemctl 常見指令

		list-unit-files
		status
		restart

## Module 32. 網路設定

#### Ubuntu(20.04):

- Server: 使用netplan管網路設定
- Server: 更改網路設定 /etc/netplan/00-installer-config.yaml

		root@bdse8:~# dpkg -l | grep -i 'netplan'
		ii  libnetplan0:amd64                    0.100-0ubuntu4~20.04.2            amd64        YAML network configuration abstraction runtime library
		ii  netplan.io                           0.100-0ubuntu4~20.04.2            amd64        YAML network configuration abstraction for various backends

- Desktop: 使用NetworkManager
- desktop 更改網路設定 `root@ubuntu99:~# nmtui`

		root@ubuntu99:~# dpkg -l | grep 'network-mana' -i
		ii  network-manager                            1.22.10-1ubuntu2.1                  amd64        network management framework (daemon and userspace tools)
		...




## Module 33. bash 變數

### 環境變數

- 又稱全域變數
- /home/user/.bash_profile
- 如果需永久儲存環境變數，可設定在shell的環境設定檔 (shell startup script)
- `set` : 檢視區域+環境
- `env` : 檢視區域

- PATH變數 屬於env

		ubuntu@ubuntu99:~$ env | grep 'PATH'
		WINDOWPATH=2
		PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

- SHELL變數 屬於env
- 第一個shell is Bourne Shell

		ubuntu@ubuntu99:~$ env | grep '^SHELL'
		SHELL=/bin/bash

- /bin/sh : Bourne Shell 
- /bin/bash : Bourne-Again Shell
 
### 區域變數







## Module 34. bash shell 指令


## Module 35. bash 環境設定檔

- Shell startup scripts: shell 設定檔

#### 取得bash方式

- login shell (ex. by using tty3 login)
- non-login shell (ex. by using GUI to open trminal)

### 所有人的Shell startup scripts
#### 1. /etc/profile
- 所有使用者在登入期間，首先會被bash所讀取的檔案
- 裡面又會去執行下面兩段: (see below code block)
- . /etc/bash.bashrc  
- /etc/profile.d/*.sh

		root@ubuntu99:~# cat /etc/profile
		# /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
		# and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).
		
		if [ "${PS1-}" ]; then
		  if [ "${BASH-}" ] && [ "$BASH" != "/bin/sh" ]; then
		    # The file bash.bashrc already sets the default PS1.
		    # PS1='\h:\w\$ '
		    if [ -f /etc/bash.bashrc ]; then
		      . /etc/bash.bashrc
		    fi
		  else
		    if [ "`id -u`" -eq 0 ]; then
		      PS1='# '
		    else
		      PS1='$ '
		    fi
		  fi
		fi
		
		if [ -d /etc/profile.d ]; then
		  for i in /etc/profile.d/*.sh; do
		    if [ -r $i ]; then
		      . $i
		    fi
		  done
		  unset i
		fi

#### 2. ~/.bash_profile
- /etc/profile 執行完後，接著執行此檔案內容

### 個人設定檔
#### 1. /etc/profile

- 載入 /etc/bash.bashrc  
- 載入 /etc/profile.d/*.sh

#### 2. ~/.bash_profile
#### 3. ~/.profile
#### 4. ~/.bashrc 

### Note => hadoop 環境設定會寫在 ~/.bashrc

#### source

- 在目前shell來讀取並執行fileNmae中的命令。該filename檔案可以無"執行許可權" 
- `source filename` : 設置環境變量
- `. filename`
- 當shell 有x權時，可用sh filename and ./filename 執行腳本是相同 (redhat)

		root@bdse8:/etc/profile.d# ./openjdk.sh
		-bash: ./openjdk.sh: Permission denied
		root@bdse8:/etc/profile.d# bash openjdk.sh			#bash是另開一個子shell執行, 執行完就回來
		root@bdse8:/etc/profile.d# echo $JAVA_HOME  		#所以仍吃不到變數
		
		root@bdse8:/etc/profile.d# . openjdk.sh				#source 才能在當前shell裡執行(not creat 子shell)
		root@bdse8:/etc/profile.d# echo $JAVA_HOME
		/usr/lib/jvm/java-11-openjdk-amd64
		root@bdse8:/etc/profile.d# $JAVA_HOME/bin/javac -version
		javac 11.0.9

- tldp.org (bash formal document)

- LSB : Linux Standard Base

		root@bdse8:/etc/profile.d# file $JAVA_HOME/bin/javac
		/usr/lib/jvm/java-11-openjdk-amd64/bin/javac: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=b47dbae98c0efe565684371ab80b55e0d1d838bb, for GNU/Linux 3.2.0, stripped


## Module 36. 簡單 shell script


==============================================================
## Module a. Linux basic concepts

- DNS:名稱解析中心
- gateway: 預設閘道
- 測試ping <br> 
FQDN (Fully Qualified Domain Name)		<br>
ex. dns.hinet.net: dns(hostname), hinet.net(domain name)	<br><br>



###id 
現代Linux <br>
UID==0:root <br>
UID>1000:一般使用者<br>
UID<1000: system user <br>
<br>
27(sudo)is root group

	ubuntu@ubuntu99:~$ id
	uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),
	24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),131(lxd),132(sambashare)

### File in memory, not in disk
- /proc
- /run
- /sys

### New line format
用notepad++, "顯示所有字元"可看到換行符號，不同OS不同，如下:

\r: carrage return	<br>
\n: new line (line feed)	<br>

- Windows: `\r\n`
- Linux: `\n`
- Max: `\r`

### 127.0.0.1 

稱為loopback, 本機localhost			<br>

### 發展...

1. cloud (SESE) {Internet}
2. IoT (internet of things) ex. temparature/moisture sensor {Collecting Data, embedded skills}  
3. Big Data: {Management}(Apache Hadoop), {Analysis}(Apache Spark) 
4. AI: deep learning {Traing data}

https://databricks.com/sparkaisummit/north-america-2020

### tty1~tty7

- Ctrl + Alt + F1/F2/F3~F6/F7

- tty7: Graphic Mode (RedHat/Debian)
- tty1: Ubuntu代表登入畫面
- tty2: Ubuntu代表回到原畫面
- tty3~tty6: 一般的console(主控台)



==============================================================
## Module b. Linux cheat sheet
	uname -r 			# show linux kernal version

	file [fileName] 	# show file type

	ubuntu@ubuntu99:~$ file performance_small.txt 
	performance_small.txt: ASCII text, with very long lines, with CRLF line terminators

	df -h				# check file system size
	du -h				# Summarize disk usage

	jps 				# 列出本機所有Java程序的pid 


### ubuntu 檔案系統	

	root@ubuntu99:~# df -h
	檔案系統                   容量  已用  可用 已用% 掛載點
	udev                       7.8G     0  7.8G    0% /dev
	tmpfs                      1.6G  1.9M  1.6G    1% /run
	/dev/mapper/vgubuntu-root  490G   24G  442G    5% /								#主要check root是否爆掉
	tmpfs                      7.9G     0  7.9G    0% /dev/shm
	tmpfs                      5.0M  4.0K  5.0M    1% /run/lock
	tmpfs                      7.9G     0  7.9G    0% /sys/fs/cgroup
	/dev/loop5                  50M   50M     0  100% /snap/snap-store/467
	/dev/loop2                 256M  256M     0  100% /snap/gnome-3-34-1804/36
	/dev/loop3                  63M   63M     0  100% /snap/gtk-common-themes/1506
	/dev/loop7                  31M   31M     0  100% /snap/snapd/9721
	/dev/loop1                  56M   56M     0  100% /snap/core18/1932
	/dev/loop4                 218M  218M     0  100% /snap/gnome-3-34-1804/60
	/dev/loop8                  51M   51M     0  100% /snap/snap-store/481
	/dev/loop6                  31M   31M     0  100% /snap/snapd/9279
	/dev/loop0                  56M   56M     0  100% /snap/core18/1885
	/dev/sda1                  511M  4.0K  511M    1% /boot/efi						# Linux kernal
	tmpfs                      1.6G   24K  1.6G    1% /run/user/1000

### 



==============================================================
## Module c. Basic...

- 動態主機設定協定（Dynamic Host Configuration Protocol, DHCP）

- `nc` 在一般的 Linux 系統中，從 0 到 1023 這個範圍的連接埠（port）是需要有 root 權限才能使用的，而 1024 以上的連接埠則是可以讓一般的使用者使用

		-w 超時時間 
		-z 指定nc只掃描正在監聽的守護程序，不傳送任何資料。

## Bash script

	bigred@cvn68:~/0_test$ cat /proc/cpuinfo | grep CPU
	model name      : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
	model name      : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
	model name      : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
	model name      : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
	model name      : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
	model name      : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz

- take import info

- `-`標準輸入，用curl 下載後導給bash

		rbean@ds101:~$ curl http://192.168.23.89:8080/mysys | bash -
		  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
		                                 Dload  Upload   Total   Spent    Left  Speed
		100   307  100   307    0     0   149k      0 --:--:-- --:--:-- --:--:--  149k
		Name : ds101
		User : rbean
		Memory : 11Gi
		CPU : Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz
		Disk : 491G

go run httpd.go


ExFAT , formated

hadoop ,NAS

sort






## Hadoop

### hdfs: 管理者指令 

(提供管理者對HDFS進行一些運行與維護功能，包括對NameNode或DataNode的啟動與操作、執行NameNode的更新、回復NameNode遺失的Metadata、執行DataNode的負載平衡、資料轉移…等)

	bigred@adm100:~$ hdfs
	Usage: hdfs [--config confdir] [--loglevel loglevel] COMMAND
	       where COMMAND is one of:

常用:
                                
	  namenode -format     format the DFS filesystem

	  namenode             run the DFS namenode
	  secondarynamenode    run the DFS secondary namenode
	  datanode             run a DFS datanode

	  haadmin              run a DFS HA admin client
	  version              print the version
	  dfs                  run a filesystem command on the file systems supported in Hadoo     

### hdfs dfs: 使用者指令

對檔案文件或目錄的操作，例如：建立目錄/檔案、拷貝目錄/檔案、刪除目錄/檔案、權限設定、查看檔案內容…等

	bigred@adm100:~$ hdfs dfs
	Usage: hadoop fs [generic options]

常用:

	        [-cat [-ignoreCrc] <src> ...]
	        [-checksum <src> ...]
	        [-chgrp [-R] GROUP PATH...]
	        [-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
	        [-cp [-f] [-p | -p[topax]] [-d] <src> ... <dst>]

	        [-df [-h] [<path> ...]]
	        [-du [-s] [-h] [-x] <path> ...]

	        [-find <path> ... <expression> ...]
	        [-get [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]

	        [-help [cmd ...]]
	        [-ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]]
	        [-mkdir [-p] <path> ...]

	        [-mv <src> ... <dst>]
	        [-put [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	        [-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
	        [-rmdir [--ignore-fail-on-non-empty] <dir> ...]
	        [-tail [-f] <file>]

### goland --> Docker, Kubernets (可在外面ex.wins 用chorom連線http://192.168.23.195:8080/)(192.168.23.195 is cvn68 ip)

	bigred@adm100:~$ cat httpd.go
	package main
	import (
	  "flag"
	  "log"
	  "net/http"
	)
	
	func main() {
	port := flag.String("p", "8080", "port to serve on")
	directory := flag.String("d", "/opt/www/", "the directory of static file to host")
	flag.Parse()
	
	http.Handle("/", http.FileServer(http.Dir("/opt/www/")))
	
	log.Printf("Serving %s on HTTP port: %s\n", *directory, *port)
	log.Fatal(http.ListenAndServe(":"+*port, nil))
	}

	bigred@adm100:~$ go run httpd.go
	2020/11/17 19:30:13 Serving /opt/www/ on HTTP port: 8080




### hadoop hdfs 操作命令 (Example)

- hdfs的root

		bigred@adm100:~$ hdfs dfs -ls /
		Found 1 items
		drwxr-xr-x   - bigred bigboss          0 2020-11-17 07:58 /dataset

- `hdfs dfs -put` 傳檔案進hdfs

		bigred@adm100:~$ ls -al /opt/dataset/customer.csv
		-rwxrwxrwx 1 bigred bigboss 695  2月 11  2019 /opt/dataset/customer.csv
		bigred@adm100:~$ hdfs dfs -put /opt/dataset/customer.csv /dataset/
		bigred@adm100:~$ hdfs dfs -ls /dataset
		Found 1 items
		-rw-r--r--   2 bigred bigboss        695 2020-11-17 10:56 /dataset/customer.csv

- 建folder(hadoop user家目錄要自己建...)

		bigred@adm100:~$ hdfs dfs -mkdir -p /user/{bigred,rbean,gbean}
		bigred@adm100:~$ hdfs dfs -ls /user
		Found 3 items
		drwxr-xr-x   - bigred bigboss          0 2020-11-17 11:00 /user/bigred
		drwxr-xr-x   - bigred bigboss          0 2020-11-17 11:00 /user/gbean
		drwxr-xr-x   - bigred bigboss          0 2020-11-17 11:00 /user/rbean

- 改folder owner (依序將user家目錄改成對應user)

		bigred@adm100:~$ hdfs dfs -chown rbean:rbean /user/rbean
		bigred@adm100:~$ hdfs dfs -chown gbean:gbean /user/gbean
		bigred@adm100:~$ hdfs dfs -ls /user
		Found 3 items
		drwxr-xr-x   - bigred bigboss          0 2020-11-17 11:00 /user/bigred
		drwxr-xr-x   - gbean  gbean            0 2020-11-17 11:00 /user/gbean
		drwxr-xr-x   - rbean  rbean            0 2020-11-17 11:00 /user/rbean

- cat內文，pipeline至bash執行

		rbean@ds101:~$ hdfs dfs -cat /apps/hi.sh
		echo 'hi'
		rbean@ds101:~$ hdfs dfs -cat /apps/hi.sh | bash -
		hi

- put file into HDFS

		bigred@adm100:~$ hdfs dfs -put /opt/dataset/customer.csv /dataset/
		put: `/dataset/customer.csv': File exists
		bigred@adm100:~$ hdfs dfs -ls /dataset
		Found 3 items
		drwxr-xr-x   - bigred bigboss          0 2020-11-18 01:32 /dataset/TW
		drwxr-xr-x   - bigred bigboss          0 2020-11-18 01:32 /dataset/US
		-rw-r--r--   2 bigred bigboss        695 2020-11-17 10:56 /dataset/customer.csv
		bigred@adm100:~$ hdfs dfs -put -f /opt/dataset/customer.csv /dataset/
		bigred@adm100:~$ hdfs dfs -ls /dataset
		Found 3 items
		drwxr-xr-x   - bigred bigboss          0 2020-11-18 01:32 /dataset/TW
		drwxr-xr-x   - bigred bigboss          0 2020-11-18 01:32 /dataset/US
		-rw-r--r--   2 bigred bigboss        695 2020-11-18 01:36 /dataset/customer.csv

- hdfs not support file edit function


### AAA (computer security) in Hadoop

- **Authentication**: 帳號審核,身分驗證(client login server)
- **Authorization**: 授權，(user進入hadoop後，家目錄，hadoop use account name 'rbean,gean...'to do mapping, hadoop 有自己的group mapping設定，也可by user做檔案授權) 
- **Accounting**: record accounting operation log

### Hadoop block concept 

- snappy (server compress tech)server檔案壓縮格式
- discommison (退役機制，當hadoop datanode要退役硬體升級loading輕鬆，下指令即可輕鬆備份至新硬體)(HDFS空間不夠-->擴充是同時擴充CPU power + storage)
- Low balancing (一個block有多人同時讀取，會按block儲存順位分散給不同人，分散loading)
- 容錯

		chmod 1777 sticky (雖然權限全開，但'1'表sitcky寫入，非owner無法刪除)

-  Example.txt(612MB)--> 每128MB切一個block，由hdfs 
 
- java決定要放入哪幾個datanode
- **Pipelined** Write: if blockA要傳到dataNode1, 每收到16M就會傳給second node(ex.dataNode5)(server to server傳送)
- **replication** factor: hdfs block複製份數

### 檢視HDFS系統資訊

	hdfs dfsadmin -report

- Name node 目錄內容(進入Name node `nna`看metadata內容, fsimage_XXXXXXX)

		$ ssh nna  tree  nn
		nn
		├── current
		│   ├── edits_0000000000000000001-0000000000000000066
		│   ├── edits_0000000000000000067-0000000000000000068
		│   ├── edits_inprogress_0000000000000000069
		│   ├── fsimage_0000000000000000000
		│   ├── fsimage_0000000000000000000.md5
		│   ├── fsimage_0000000000000000068
		│   ├── fsimage_0000000000000000068.md5
		│   ├── seen_txid
		│   └── VERSION
		└── in_use.lock

- Data node目錄內容(blk_XXXXXX)

		$ ssh wka01 tree dn
		dn
		├── current
		│   ├── BP-564818905-172.30.0.10-1541644288235
		│   │   ├── current
		│   │   │   ├── dfsUsed
		│   │   │   ├── finalized
		│   │   │   │   └── subdir0
		│   │   │   │       └── subdir0
		│   │   │   │           ├── blk_1073741829
		│   │   │   │           ├── blk_1073741829_1009.meta
		│   │   │   │           ├── blk_1073741830
		│   │   │   │           ├── blk_1073741830_1010.meta
		│   │   │   │           ├── blk_1073741831
		│   │   │   │           └── blk_1073741831_1011.meta
		│   │   │   ├── rbw
		│   │   │   └── VERSION
		│   │   ├── scanner.cursor
		│   │   └── tmp
		│   └── VERSION
		└── in_use.lock

- Show file/block information `hdfs fsck`

		bigred@adm100:~$ hdfs fsck  /dataset/twmpp/month=05  -files  -blocks  -locations
		Connecting to namenode via http://nna:50070/fsck?ugi=bigred&files=1&blocks=1&locations=1&path=%2Fdataset%2Ftwmpp%2Fmonth%3D05
		FSCK started by bigred (auth:SIMPLE) from /172.30.0.6 for path /dataset/twmpp/month=05 at Fri Nov 20 09:19:00 UTC 2020
		/dataset/twmpp/month=05 <dir>
		/dataset/twmpp/month=05/000000_0 154321493 bytes, 2 block(s):  OK
		0. BP-2100678568-172.30.0.10-1605599343740:blk_1073748055_7231 len=134217728 Live_repl=2 [DatanodeInfoWithStorage[172.30.0.16:50010,DS-ecb35884-2a87-4330-bd64-7e4f5afa348d,DISK], DatanodeInfoWithStorage[172.30.0.17:50010,DS-d458e70b-c89e-4d84-b193-1cfc9b42758a,DISK]]
		1. BP-2100678568-172.30.0.10-1605599343740:blk_1073748056_7232 len=20103765 Live_repl=2 [DatanodeInfoWithStorage[172.30.0.16:50010,DS-ecb35884-2a87-4330-bd64-7e4f5afa348d,DISK], DatanodeInfoWithStorage[172.30.0.17:50010,DS-d458e70b-c89e-4d84-b193-1cfc9b42758a,DISK]]
		
		Status: HEALTHY
		 Total size:    154321493 B
		 Total dirs:    1
		 Total files:   1
		 Total symlinks:                0
		 Total blocks (validated):      2 (avg. block size 77160746 B)
		 Minimally replicated blocks:   2 (100.0 %)
		 Over-replicated blocks:        0 (0.0 %)
		 Under-replicated blocks:       0 (0.0 %)
		 Mis-replicated blocks:         0 (0.0 %)
		 Default replication factor:    2
		 Average block replication:     2.0
		 Corrupt blocks:                0
		 Missing replicas:              0 (0.0 %)
		 Number of data-nodes:          3
		 Number of racks:               1
		FSCK ended at Fri Nov 20 09:19:00 UTC 2020 in 0 milliseconds
		
		
		The filesystem under path '/dataset/twmpp/month=05' is HEALTHY



### HDFS 容錯處理 (遺失修復)

[重要] 上述命令, 執行時會拋出 java.io.FileNotFoundException 例外物件, 同時也會觸動 HDFS 修復遺失的 Block

### HDFS CORRUPT (Block 損毀無法修復)

- block 損毀，無法cat

		bigred@adm100:~$ hdfs dfs -cat /tmp/passwd
		cat: Could not obtain block: BP-2100678568-172.30.0.10-1605599343740:blk_1073741850_1026 file=/tmp/passwd


- File corrupt (real block損毀... -ls meta 仍會在但實際block已經遺失)

		bigred@adm100:~$ hdfs fsck /tmp/passwd -files -blocks -locations
		Connecting to namenode via http://nna:50070/fsck?ugi=bigred&files=1&blocks=1&locations=1&path=%2Ftmp%2Fpasswd
		FSCK started by bigred (auth:SIMPLE) from /172.30.0.6 for path /tmp/passwd at Wed Nov 18 08:10:21 UTC 2020
		/tmp/passwd 1421 bytes, 1 block(s):
		/tmp/passwd: CORRUPT blockpool BP-2100678568-172.30.0.10-1605599343740 block blk_1073741850
		 MISSING 1 blocks of total size 1421 B
		0. BP-2100678568-172.30.0.10-1605599343740:blk_1073741850_1026 len=1421 MISSING!
		
		Status: CORRUPT
		 Total size:    1421 B
		 Total dirs:    0
		 Total files:   1
		 Total symlinks:                0
		 Total blocks (validated):      1 (avg. block size 1421 B)
		  ********************************
		  UNDER MIN REPL'D BLOCKS:      1 (100.0 %)
		  dfs.namenode.replication.min: 1
		  CORRUPT FILES:        1
		  MISSING BLOCKS:       1
		  MISSING SIZE:         1421 B
		  CORRUPT BLOCKS:       1
		  ********************************
		 Minimally replicated blocks:   0 (0.0 %)
		 Over-replicated blocks:        0 (0.0 %)
		 Under-replicated blocks:       0 (0.0 %)
		 Mis-replicated blocks:         0 (0.0 %)
		 Default replication factor:    2
		 Average block replication:     0.0
		 Corrupt blocks:                1
		 Missing replicas:              0
		 Number of data-nodes:          3
		 Number of racks:               1
		FSCK ended at Wed Nov 18 08:10:21 UTC 2020 in 0 milliseconds
		
		
		The filesystem under path '/tmp/passwd' is CORRUPT


### 20201119 note

- HDFS Syncronization: like Microsoft AD (active Directory), single sign on


## YARN (Yet Another Resource Negotiator)(資源配置)

## MapReduce (YARN-based的平行運算機制)

#### Shuffle 

- shuffle: 從Map 產生輸出開始，包括系統執行排序以及傳送Map 輸出到Reducer 作為輸入的過程
- shaffle is a java script run a each datanode +N(不只在各datanode各一, which can be modified(ex, IBM  改成傳給reduce前先壓縮)

		bigred@adm100:~$ more /opt/hadoop-2.10.1/etc/hadoop/yarn-site.xml
		<?xml version="1.0"?>
		<configuration>
		
		  <property>
		    <name>yarn.nodemanager.aux-services</name>
		    <value>mapreduce_shuffle</value>
		  </property>
		  <property>
		    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
		    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
		  </property>

#### Resource Manager/Job History Server/ Node Manager

- **NameNode**(nna)/**DataNode**(wka01,wka02,wka03) **jps** (列出本機所有Java程序的pid)

		bigred@nna:~$ jps
		144 SecondaryNameNode
		1411 Jps
		47 NameNode

		bigred@rma:~$ jps
		1764 ResourceManager
		2132 Jps
		1835 JobHistoryServer

		bigred@wka01:~$ jps
		48 DataNode
		377 Jps
		171 NodeManager

#### HDFS 壓縮(map可直接對bzip2)

- zip 可放進HDFS, but not supprot hadoop-streaming input .zip

		bigred@adm100:~$ hdfs dfs -rm -r wc;hadoop jar /opt/hadoop-2.10.1/share/hadoop/tools/lib/hadoop-streaming-2.10.1.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input word.zip -output wc/
		rm: `wc': No such file or directory
		packageJobJar: [/tmp/hadoop-unjar6941214294439210634/] [] /tmp/streamjob8478517033579304640.jar tmpDir=null
		Error: java.lang.RuntimeException: PipeMapRed.waitOutputThreads(): subprocess failed with code 1
		        at org.apache.hadoop.streaming.PipeMapRed.waitOutputThreads(PipeMapRed.java:325)
		        at org.apache.hadoop.streaming.PipeMapRed.mapRedFinished(PipeMapRed.java:538)
		        at org.apache.hadoop.streaming.PipeMapper.close(PipeMapper.java:130)
		        at org.apache.hadoop.mapred.MapRunner.run(MapRunner.java:61)
		        at org.apache.hadoop.streaming.PipeMapRunner.run(PipeMapRunner.java:34)

- bzip2 is a free and open-source file, compression program that uses the Burrows–Wheeler algorithm.
- bxip2: a block-sorting file compressor (以file size 為區塊定義的壓縮格式)

		bzip2  -1 -kz  '1000000 Sales Records.csv' -c  > sales.bz2

### Hadoop Streaming

- Hadoop Streaming a utility that comes with the Hadoop distribution.  
- Hadoop streaming can be performed using languages like **Python**, Java, PHP, Scala, Perl, UNIX, and many more. 
- The utility allows us to **create** and **run** Map/Reduce jobs with any executable or script as the mapper and/or the reducer.
- It can be used to execute programs for big data analysis.
- 像是java class的cmd

### Python MapReduce Job

- Example: 算word.txt文字出現次數 by mapper.py/reducer.py

		bigred@adm100:~$ hadoop jar /opt/hadoop-2.10.1/share/hadoop/tools/lib/hadoop-streaming-2.10.1.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input word.txt -output wc/
		packageJobJar: [/tmp/hadoop-unjar1570291024092227269/] [] /tmp/streamjob7128216635605627450.jar tmpDir=null

- 註: `- files <file1,...>` specify a comma-separated list of files to be copied to the map reduce cluster
		
		bigred@adm100:~$ hdfs dfs -ls wc
		Found 2 items
		-rw-r--r--   2 bigred bigboss          0 2020-11-19 10:12 wc/_SUCCESS
		-rw-r--r--   2 bigred bigboss        119 2020-11-19 10:12 wc/part-00000
		
		bigred@adm100:~$ hdfs dfs -cat wc/part-00000
		Bird   1
		Cat   1
		Elephant   1
		Fish   1
		Tiger   1
		deer   2
		dolphin   2
		horse   1
		lion   3
		mouse   1
		turtle 1
			
### Pig  

- Apache Pig is a platform for analyzing large data sets that consists of a **high-level language** for **expressing** data analysis programs, coupled with infrastructure for evaluating these programs.

		bigred@adm100:~$ pig -e 'cat wc'  2>/dev/null   ###不用再下hdfs dfs -ls XXX
		Bird   1
		Cat   1
		Elephant   1
		Fish   1
		Tiger   1
		deer   2
		dolphin   2
		horse   1
		lion   3
		mouse   1
		turtle 1

## 研究 MapReduce 運算資源

- 認識 **teragen** 測試資料檔

		bigred@adm100:~$ yarn  jar /opt/hadoop-2.10.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.10.1.jar teragen -Dmapred.map.tasks=1 12  /tmp/12row                                                                                               
		
		bigred@adm100:~$ hdfs dfs -get /tmp/12row/part-m-00000  teragen-12row

- Memory **Jim Gray** 

		bigred@adm100:~$ od -Ax -a -v -w10 teragen-12row | head -3
		000000   J   i   m   G   r   a   y   R   I   P
		00000a nul dc1   0   0   0   0   0   0   0   0
		000014   0   0   0   0   0   0   0   0   0   0

- Teragen 檔案格式說明
	* (10 bytes key) (constant 2 bytes) (32 bytes rowid) 
	* (constant 4 bytes) (48 bytes filler) (constant 4 bytes)
	* The rowid is the right justified row id as a hex number.

- 建立16M teragen檔案

		hdfs dfs -rm -r /tmp/16m &>/dev/null; yarn  jar /opt/hadoop-2.10.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.10.1.jar teragen -Dmapreduce.job.maps=1 167770  /tmp/16m
		.......
		2020-07-06 04:59:02,263 INFO terasort.TeraGen: Generating 167770 using 1
		2020-07-06 04:59:02,331 INFO mapreduce.JobSubmitter: number of splits:1
		........
		2020-07-11 08:57:58,478 INFO mapreduce.Job:  map 0% reduce 0%
		2020-07-11 08:58:03,559 INFO mapreduce.Job:  map 100% reduce 0%

[註] 由以上資訊可知, 只有執行  Map, 並沒執行 Reduce    <br>
[註] 167770, 單位是 100 byte 的一筆資料

- 執行 terasort **排序**功能

		bigred@adm100:~$ hdfs dfs -rm -r /tmp/sortest/ &>/dev/null; yarn  jar  /opt/hadoop-2.10.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.10.1.jar terasort -Dmapred.reduce.tasks=1  /tmp/320m  /tmp/sortest/
		2020-11-19 10:44:14,142 INFO terasort.TeraSort: starting
		.......
		2020-11-19 10:44:15,967 INFO mapreduce.JobSubmitter: number of splits:3  		#由此可知"應該"有3個map(但可能更多)
		........
		2020-11-19 10:44:22,588 INFO mapreduce.Job:  map 0% reduce 0%
		2020-11-19 10:44:31,745 INFO mapreduce.Job:  map 33% reduce 0%
		2020-11-19 10:44:34,780 INFO mapreduce.Job:  map 67% reduce 0%
		2020-11-19 10:44:35,786 INFO mapreduce.Job:  map 100% reduce 0%
		2020-11-19 10:44:40,821 INFO mapreduce.Job:  map 100% reduce 100%				#有map,也有reduce(沒reduce job 會0%)
		.......
        Job Counters
                Killed map tasks=1				#丟棄1個map
                Launched map tasks=4			#執行4個map
                Launched reduce tasks=1			#執行1個reduce 
                Data-local map tasks=1
                Rack-local map tasks=3
		......

- 檢視 MapReduce 運算資源 (結過竟然丟出4個map,1個reduce)

		bigred@adm100:~$ stopjps
		[wka01]
		18:44:17
		44229 MRAppMaster 384m		
		18:44:23
		45108 YarnChild 384m    	# 一個map job = 一個YarnChild jps (mapred-site.xml設定map的Memory=384M)
		44229 MRAppMaster 384m		
		18:44:34
		44229 MRAppMaster 384m
		---------------------------------		
		[wka02]
		18:44:23
		45710 YarnChild 384m		# 一個map job
		18:44:33
		46539 YarnChild 384m		# 一個map job
		45710 YarnChild 384m		
		18:44:34
		45710 YarnChild 384m		
		---------------------------------
		[wka03]
		18:44:24
		45542 YarnChild 384m		# 一個map job
		18:44:32
		46291 YarnChild 512m		# 一個reduce job = 一個YarnChild jps (mapred-site.xml設定map的Memory=512M)

- MapReduce 啟動 **Speculative Execution** 功能 (不信任機制, MapReduce有自己的演算法會依hardware condition決定要不要多灑map/reduce job)

		bigred@adm100:~$ more /opt/hadoop-2.10.1/etc/hadoop/mapred-site.xml
			......
		   <!-- Disable Speculative Execution -->
		   <!--
		   <property>
		      <name>mapred.map.tasks.speculative.execution</name>
		      <value>false</value>
		   </property>
		   <property>
		      <name>mapred.reduce.tasks.speculative.execution</name>
		      <value>false</value>
		   </property>
		   -->
			......

## hadoop [COMMAND]

- **Accessing Cloud Data.** not onlyt hdfs. s3a:amazon

		# Create a directory
		hadoop fs -mkdir s3a://bucket1/datasets
		
		# Upload a file from the cluster filesystem
		hadoop fs -put /datasets/example.orc s3a://bucket1/datasets/

- hdfs dfs XXX 

		hadoop fs -mkdir -p /dataset/sales  #等效hdfs dfs

- hadoop 指令

		bigred@adm100:~$ hadoop
		Usage: hadoop [--config confdir] [COMMAND | CLASSNAME]
		  CLASSNAME            run the class named CLASSNAME
		 or
		  where COMMAND is one of:
		  fs                   run a generic filesystem user client
		  version              print the version
		  jar <jar>            run a jar file
		                       note: please use "yarn jar" to launch
		                             YARN applications, not this command.
		  checknative [-a|-h]  check native hadoop and compression libraries availability
		  distcp <srcurl> <desturl> copy file or directories recursively
		  archive -archiveName NAME -p <parent path> <src>* <dest> create a hadoop archive
		  classpath            prints the class path needed to get the
		                       Hadoop jar and the required libraries
		  credential           interact with credential providers
		  daemonlog            get/set the log level for each daemon
		  trace                view and modify Hadoop tracing settings
		
		Most commands print help when invoked w/o parameters.





## Hive

- A data warehouse infrastructure that provides data summarization and ad hoc querying.
- The Apache Hive ™ data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage using SQL. 
- Structure can be projected onto data already in storage. 
- A command line tool and JDBC driver are provided to connect users to Hive.

### Apache Derby

- an Apache DB subproject, is an open source relational database implemented entirely in Java
 
### 執行Hive

- 寫hsql執行檔 (for creat DB,table) (which is metadata, like schema)

		bigred@adm100:~$ cat daily_mask.hsql
		CREATE DATABASE IF NOT EXISTS twmask;
		USE twmask;
		DROP TABLE IF EXISTS daily_mask;
		
		CREATE EXTERNAL TABLE daily_mask (
		  code string,
		  name string,
		  address string,
		  tel string,
		  adult INT,
		  child INT,
		  dt string
		)
		ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
		STORED AS TEXTFILE LOCATION '/raw/twmask';

- 執行 (-S --silent mode)

		bigred@adm100:~$ hive -S -f daily_mask.hsql

- show table

		bigred@adm100:~$ hive -S -e 'show databases' 2>/dev/null
		default
		twmask

### metastore_db

- table schema by user.(先有data,才有table,even欄位名稱都是自定義)

		bigred@adm100:~$ ls -al metastore_db/
		total 48
		drwxrwxr-x 5 bigred bigred  4096 11月 19 19:40 .
		drwxr-xr-x 1 bigred bigred  4096 11月 19 18:28 ..
		-rw-rw-r-- 1 bigred bigred     4 11月 19 19:40 dbex.lck
		-rw-rw-r-- 1 bigred bigred    38 11月 19 19:40 db.lck
		drwxrwxr-x 2 bigred bigred  4096 11月 16 15:31 log
		-rw-rw-r-- 1 bigred bigred   608 11月 16 15:31 README_DO_NOT_TOUCH_FILES.txt
		drwxrwxr-x 2 bigred bigred 12288 11月 16 15:31 seg0
		-rw-rw-r-- 1 bigred bigred   904 11月 16 15:31 service.properties
		drwxrwxr-x 2 bigred bigred  4096 11月 19 19:40 tmp



### hive based on yarn (簡單SQL command可能不用觸發yarn/Mapreduce) 

- fail example: `where code='2090011' and adult>0` added, 沒啟動yarn

		bigred@adm100:~$ hive -S -e "use twmask;select code, name, adult, replace(dt,'/', '-') from daily_mask where code='2090011' and adult>0"
		 ............
		Caused by: java.net.ConnectException: Connection refused
		        at sun.nio.ch.SocketChannelImpl.checkConnect(Native Method)
		        at sun.nio.ch.SocketChannelImpl.finishConnect(SocketChannelImpl.java:714)
		        at org.apache.hadoop.net.SocketIOWithTimeout.connect(SocketIOWithTimeout.java:206)
		        at org.apache.hadoop.net.NetUtils.connect(NetUtils.java:532)
		        at org.apache.hadoop.ipc.Client$Connection.setupConnection(Client.java:701)
		        at org.apache.hadoop.ipc.Client$Connection.setupIOstreams(Client.java:814)
		        at org.apache.hadoop.ipc.Client$Connection.access$3700(Client.java:423)
		        at org.apache.hadoop.ipc.Client.getConnection(Client.java:1610)
		        at org.apache.hadoop.ipc.Client.call(Client.java:1441)
		        ... 56 more
		Job Submission failed with exception 'java.net.ConnectException(Call From adm100/172.30.0.6 to rma:8032 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused)'
		FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.mr.MapRedTask. Call From adm100/172.30.0.6 to rma:8032 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused

- OK while start yarn

		bigred@adm100:~$ hive -S -e "use twmask;select code, name, adult, replace(dt,'/', '-') from daily_mask where code='2101090011' and adult>0"
		.......


## YARN be replaced by MESOS? K8S?

### Mesos

- A distributed systems kernel
- Mesos is built using the same principles as the Linux kernel, only at a different level of abstraction. 
- The Mesos kernel runs on every machine and provides applications (e.g., Hadoop, Spark, Kafka, Elasticsearch) with API’s for resource management and scheduling across entire datacenter and cloud environments.

### K8S

- Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
- It groups containers that make up an application into logical units for easy management and discovery. 
- Kubernetes builds upon 15 years of experience of running production workloads at Google, combined with best-of-breed ideas and practices from the community.



