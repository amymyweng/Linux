#  #
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
- 確定系統ssh server有啟動:	<br>
`lsof -nPi` 22 listen
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

### <
輸入導向<br>
cat<<end, 遇到結束字元"end"cat停止

	buntu@ubuntu99:~$ cat <<end
	> 123
	> 234
	> end
	123
	234

- Python 第一提示字元`>>>`
- PYthon 第二提示字元`...`
- Linux 第一提示字元`$`
- Linux 第二提示字元`>`

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

- [Alt]+[N]: show line number
- Ctrl + K : cut line
- Alt + U: paste line

## Module 23. 帳號管理
## Module 24. 群組管理
## Module 25. 軟體管理YUM (CentOS)

- rpm: 單一套件安裝 (`rpm -qa`: show所有已安裝套件)
- yum : 倉庫式的套件安裝，會自動處理套件相依性的問題 

yum安裝容易,rpm成查詢用途  <br>

yum.conf; *.repo        <br>

- yum update:升級所有套件(Be careful!!)

## Module 26. 軟體管理APT (Debian)

- `/etc/apt/sources.list` : 更新repository 倉庫 (主設定檔)
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
## Module 31. 系統服務
## Module 32. 網路設定
## Module 33. bash 變數
## Module 34. bash shell 指令
## Module 35. bash 環境設定檔
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


==============================================================
## Module b. Linux cheat sheet
	uname -r 			# show linux kernal version

	file [fileName] 	# show file type

	ubuntu@ubuntu99:~$ file performance_small.txt 
	performance_small.txt: ASCII text, with very long lines, with CRLF line terminators

	df -h				# check file system size
	du -h				# Summarize disk usage

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


