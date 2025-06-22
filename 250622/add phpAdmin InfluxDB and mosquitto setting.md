> 🗓️ 操作日期：2025-06-22
---

### 🎯 今日任務
- 0.Verify system configuration   
- 1.Use HeidiSQL create normal user account for node-red.  
- 2.Install phpMyAdmin in docker and usage with linked mysql sever by ubuntu.   
- 3.Install influxDB in docker and create new organization.  
- 4.Create images/eclipse-mosquitto > countinues , create new username / password authentication, and verify a private connection with MQTTBox.  
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
:white_check_mark: 重新建立DB(排序規則預設utf8mb4_0900_ai_ci)  
1. root帳號登入  
2. 圖形介面:刪除(右鍵刪除),重新建立(MySQL/右鍵/建立新的)  
3. 指令:CREATE DATABASE `sakila` /*!40100 COLLATE 'utf8mb4_0900_ai_ci' */  
4. 匯入範例資料:檔案/執行SQL檔案  
5. 關閉DB link,使用者改成一般帳號(anne/pw/3306/DBname) 此帳號為node-red連線用  
---
### 二、add images/phpMyAdmin and connet MySQL in Docker  
:white_check_mark: docker UI無設定/修改連線至MySQL的介面,使用linux指令完成修改   
:bulb: mysql_db_server:為docker desktop的containers/images=MySQL name  
1. in Docker : download images : phpmyadmin > containers  
2. 指令 : docker run --name phpmyadmin -d --link mysql_db_server:db -p 8080:80 phpmyadmin  
3. 執行環境 : ubuntu  
4. Test : http://localhost:8080/  
5. 參考 : https://hub.docker.com/_/phpmyadmin?uuid=DD91A413-C325-4758-B3AF-3D7FE2A72062  

---
### 三、add images/InfluxDB in Docker  
:white_check_mark: 建立InfluxDB :不用定義table,僅需將資料格式定義成.json  
1. in Docker : download images : influxdb > containers  
2. 建立influxDB : 建立帳號密碼  
3. :warning: 儲存TOKEN(僅出現一次)  
4. Test : http://localhost:8086/

---
### 四、Create images/eclipse-mosquitto > countinues , create new account, and verify a private connection with MQTTBox      
:white_check_mark: 建立私人的MQTTBox、建立新帳號與成功連線   
1. in Docker : download images/eclipse-mosquitto > containers   
    docker UI介面製作 Volumes :  
      host path:點選.conf檔案的路徑  
      container path:/mosquitto/config  
      參考 : https://hub.docker.com/_/eclipse-mosquitto?uuid=D61E1A08-72C7-47ED-914B-0A13A7417072  
    memo:指令建立 : cmd 執行,因為前面的路徑位置是windows的 C:\Users\user\Documents\Docker\Mosquitto\config , 1884原因:已先用docker環境建立1883:1883,用指令方式再練習建一個  
   ```
     docker run -it -p 1884:1883 -v "C:\Users\user\Documents\Docker\Mosquitto\config":/mosquitto/config eclipse-mosquitto
   ```
3. 打開MQTTBox.exe,create new MQTT CLIENTS  
  2.1 setting :⛔預設帳密為底線_,not -  

  |feild name  | input name|
  |------------|----------|
  |client name |Mosquitto|
  |protocol    |mqtt/tcp |
  |host        |本機ip   |
  |username    |mqtt_user|
  |password    |mqtt_password|

  
3. download mosquitto.conf  
  3.1 link: https://github.com/eclipse-mosquitto/mosquitto/blob/master/mosquitto.conf   
4. 檔案位置自行設定 --我的D:\202506Docker\Docker\Mosquitto\config\mosquitto.conf     
5. 修改mosquitto.conf   
   5.1 allow-anonymous false : 禁止匿名登入(Security/#allow_anonymous false)  
       操作 : 刪除#  
       memo : 可以先改成true,表示允許不用帳密皆可登入 >>allow-anonymous true  
    
   5.2 listener 可連線到本機:  
   5.2.1 開啟cmd,輸入 netstat -na,找到1883(狀態為listening)的本機位置  
   5.2.2 link ip & proxy : 至mosquitto.conf 搜尋 "listener" (Listeners/#listener port-number [ip address/host name/unix socket path] 的下一行)  
   操作 : 刪除# ,listener 1883 0.0.0.0  
   memo : 通常為指定ip,並不會使用0.0.0.0,此為個人環境操作,故以0.0.0.0設定  
 :bulb: 重啟containers/Mosquitto & check MQTTBox is connet:表示可連上本機ip:proxy  

   5.3 password_file 讀取含有登入帳密的檔案:   
   5.3.1 至mosquitto.conf 搜尋 "password_file"  
          (Default authentication and topic access control/# password_file, the plugin check will be made first.的下一行)   
   操作 : 刪除# ,password_file /mosquitto/config/pwfile   
   memo : 1. 路徑m小寫,原大寫同D:/設定會無法重新執行containers/Mosquitto  2. 改回allow-anonymous false   
 :bulb: 重新開啟containers/Mosquitto & check MQTTBox is connet:表示帳密正確  

   5.4 Creating a password file :     
   5.4.1 Creating a password file     
     step1.至Docker/Mosquitto/Exec執行    
     step2  
         2.1 開啟password file 的位置 : / # cd mosquitto > /mosquitto # cd config   
         2.2 查看資料夾內檔案 : /mosquitto/config #  
     ```
         ls -l
     ```
      result  
      -rwxrwxrwx    1 mosquitto mosquitto     40532 Jun 22 13:52 mosquitto.conf  
      -rwxrwxrwx    1 mosquitto mosquitto       119 Jun 22 13:45 passwd250622  
      -rwxrwxrwx    1 mosquitto mosquitto       602 Jun 16 11:28 pwfile  
   
     step3.建立新的password file : mosquitto_passwd -c <password file> <username>  -- <password file>: 路徑/檔名,<username>:MQTTBox登入帳號  
           e.g. /mosquitto/config/passwd250622test testaccount  
           參考 : https://mosquitto.org/documentation/authentication-methods/  
   
     step4.確認資料已產出 : /mosquitto/config #   
   ```
           ls -l
   ```
   result > -rw-------    1 root     root           125 Jun 22 14:02 passwd250622test  -- 僅root可讀取  

      step5.確認密碼為加密產出 : /mosquitto/config #   
   ```
        cat passwd250622test
   ```
    result > testaccount:$7$101$ubqe29PxLUhJtLAQ$y6WiW9lP4P7awTR9+Xvsx0D9w1ZM9L85B3rO7umDirY/  

      step6.Mosquitto 密碼檔權限處理紀錄 : 設定 password_file 時，Mosquitto 必須能讀取該檔案，否則 client 會出現 "not authorised"  
          6.1 設定檔案擁有者給 mosquitto 使用者 :  
       ```  
               chown mosquitto:mosquitto passwd250622test  
       ```  
          6.2 設定安全權限：所有人都可讀寫執行（課程先設定為777但較不安全,建議640） :  
       ```  
               chmod 777 passwd250622test  
       ```  
         result >  -rwxrwxrwx    1 mosquitto mosquitto       125 Jun 22 14:02 passwd250622test  
   
      step7.修改mosquitto.conf 讀取登入帳密的檔案 :  
       ```  
           password_file /mosquitto/config/passwd250622test  
       ```
   
      step8.重啟containers/Mosquitto & check MQTTBox is connet : 修改 MQTTBox username and password  
   
