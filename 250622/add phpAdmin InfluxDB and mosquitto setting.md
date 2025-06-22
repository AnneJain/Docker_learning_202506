> 🗓️ 操作日期：2025-06-22
---

### 🎯 今日任務
- 0.use ubuntu to work linux/docker,then open docker desktop.exe by ubuntu cmd  
- 1.使用HeidiSQL建立一般user帳號權限,後續提供給node-red使用  
- 2.install phpmyadmin in docker and usage with linked mysql sever by ubuntu cmd  
- 3.install influxDB in docker and create new organization  
- 4.setting MTQQBox by private and create countinues/mosquitto:create admin account and normal user account  
---

### 零、啟動環境:使用ubuntu or WSL開啟Linux環境
1. free :check memory
2. df -k :check SSD/HDD memory
3. ps -ef :check proxy(before open docker desktop.ext):此時尚未有docker的啟用紀錄  
  open docker desktop > ps -ef會出現類似下方內容  
  root       553   552  3 09:52 pts/2    00:00:00 /mnt/wsl/docker-desktop/docker-desktop-user-distro proxy --distro-name U

#### user@LAPTOP-AQVLNOHO:~$ free

|  | total     | used     | free     | shared  | buff/cache   | available   |
|--|----------|----------|----------|----------|--------------|--------------|  
| Mem | 8011992 |468376| 7303744 |  3520  |  393260  | 7543616|
| Swap :warning:|2097152|  0|    2097152| 通常swap used為0|若有使用量表示已使用到硬碟資源|基本上不使用|                                 
---
### 一、使用HeidiSQL建立一般user帳號權限,後續提供給node-red使用
1.重新建立DB(排序規則預設utf8mb4_0900_ai_ci):  
1.0root帳號登入  
1.1圖形介面:刪除(右鍵刪除),重新建立(MySQL/右鍵/建立新的)  
1.2指令:CREATE DATABASE `sakila` /*!40100 COLLATE 'utf8mb4_0900_ai_ci' */  
1.3匯入範例資料:檔案/執行SQL檔案  
1.4關閉DB link,使用者改成一般帳號(anne/pw/3306/DBname) 此帳號為node-red連線用  
---
### 二、add images/phpMyAdmin and connet MySQL in Docker  
:warning: docker UI無設定/修改連線至MySQL的介面,故使用linux指令完成修改  
2.1 MySQL_Prod:為docker desktop的containers/images=MySQL name  
指令 : docker run --name phpmyadmin -d --link mysql_db_server:db -p 8080:80 phpmyadmin 
執行環境 : ubuntu  
Test : http://localhost:8080/  
參考 : https://hub.docker.com/_/phpmyadmin?uuid=DD91A413-C325-4758-B3AF-3D7FE2A72062  
--個人設定 docker run --name phpmyadmin -d --link MySQL_Prod:db -p 8080:80 -v /some/local/directory/conf.d:/etc/phpmyadmin/conf.d:ro phpmyadmin  
---
### 三、add images/InfluxDB in Docker   
3.1 in Docker : 一般下載images > containers  
3.2 建立influxDB : 建立帳號密碼
