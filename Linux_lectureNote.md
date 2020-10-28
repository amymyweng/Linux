##Module 1. Linux 介紹
##Module 2. 發行版本
##Module 3. 安裝規劃
##Module 4. 安裝系統
##Module 5. Post Installation 安裝後續
##Module 6. 初次使用系統
##Module 7. 查指令說明
##Module 8. 檔案系統
##Module 9. 基本檔案管理
##Module 10. 處理文字檔內容
##Module 11. I/O重導與管線
##Module 12. 資料串流指令
	uname -r 		# show linux kernal version
	file [fileName] 	# show file type
	
	ubuntu@ubuntu99:~$ file performance_small.txt 
	performance_small.txt: ASCII text, with very long lines, with CRLF line terminators

##Module 13. 檔案搜尋
##Module 14. Regular Expression 正規表達式
For Linux environment, ' ' 讓shell不去解釋任何特殊符號的意義 <br>
grep -n	#show result num line<br>

/etc/passwd  #also show system account + user account <br>


##Module 15. 延伸正規表達式
##Module 16. LVM系統
##Module 17. 檔案與目錄權限
##Module 18. 檔案與目錄授權
##Module 19. 檔案打包壓縮
##Module 20. 檔案解壓縮
	c,t,x (same type, can be choosed 1 type only)
         
	tar -c	#creat                                              
	tar -t	#list                                            
	tar -x	#extract                                           
	-v	#verbose mode(顯示詳細的處理過程)                     
	-f	#打包解壓縮的file name                                
	-C	#配合-x, 指定解開的路徑, 安裝hadoop時會用到            

##Module 21. 文書處理器vi/vim
##Module 22. 文書處理器nano
##Module 23. 帳號管理
##Module 24. 群組管理
##Module 25. 軟體管理YUM
##Module 26. 軟體管理APT
##Module 27. 行程管理
##Module 28. 背景程式
##Module 29. 系統日誌檢視
##Module 30. 系統開機
##Module 31. 系統服務
##Module 32. 網路設定
##Module 33. bash 變數
##Module 34. bash shell 指令
##Module 35. bash 環境設定檔
##Module 36. 簡單 shell script


###id 
現代Linux <br>
UID==0:root <br>
UID>1000:一般使用者<br>
UID<1000: system user <br>
27(sudo)is root group

	```bash
	ubuntu@ubuntu99:~$ id
	uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),131(lxd),132(sambashare)
	```
	```