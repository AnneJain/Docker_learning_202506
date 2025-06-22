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
### 四、setting MTQQBox by private and create countinues/mosquitto:create admin account and normal user account  
:white_check_mark: 建立私人的MQTTBox並建立admin與一般user帳號  
1. in Docker : download images : eclipse-mosquitto > containers  
2. 打開MQTTBox.exe,create new MQTT CLIENTS  
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
   memo : 通常為指定ip,並不會使用0.0.0.0,此為各人環境操作,故以0.0.0.0設定  
 :bulb: 重新開啟containers/Mosquitto & check MQTTBox is connet:表示可連上本機ip:proxy  

   5.3 password_file 讀取含有登入帳密的檔案:  
   5.3.1 至mosquitto.conf 搜尋 "password_file" 
          (Default authentication and topic access control/# password_file, the plugin check will be made first.的下一行)  
   操作 : 刪除# ,password_file /mosquitto/config/pwfile  
   memo : 1. 路徑m小寫,原大寫同D:/設定會無法重新執行containers/Mosquitto  2. 改回allow-anonymous false  
 :bulb: 重新開啟containers/Mosquitto & check MQTTBox is connet:表示帳密正確

   5.4 產生password_feild :  
   5.4.1 
   
7. 打開MQTTBox.exe
  

8. test
9. 
