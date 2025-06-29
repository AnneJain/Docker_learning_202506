> 🗓️ 操作日期：2025-06-29
---

### 🎯 今日任務
- 0.check env setting.    
- 1.Create new username / password in file, and  setting user access limit with MQTTBox.   
- 2.Add FUXA(dashboard).  
- 3.Resetting Node-Red、influxDB.    
- 4.Setting ModBus.
- 5.connection on FUXA
---
### 零、確認環境變數(memory/SSD/HDD/proxy):開啟ubuntu/WSL
1. free :check memory
2. df -k :check SSD/HDD memory
3. ps -ef :check proxy(before open docker desktop.ext):此時尚未有docker的啟用紀錄  
4. sudo su - :進入系統管理員權限、輸入登入密碼  
5. ifconfig : 查看網路卡資訊(會顯示 not found,接續執行 sudo apt install net-tools 以系統管理員身分進行安裝) ps.系統管理員與一般user身分安裝的路徑會有差異  
6. netstat -na more : 檢查mosquitto有成功開啟  

|Proto|Recv-Q |Send-Q|Local Address | Foreign Address |  State |
|-----|-------|------|--------------|-----------------|--------|    
|tcp6 |    0  |  0   | :::1883      |  :::*           |  LISTEN|  


---

### 一、刪除新增moquitto登入的帳號密碼與設定各帳號讀寫權限    
:white_check_mark: 刪除原本密碼檔中的帳號並重新建立(與上週不同,上周為產生新的password檔案(txt),本周為修改檔案中資料)     
🛠️ File Setting : pwfile.txt (container/Mosquitto/Exec執行)      
1. cd mosquitto > cd config : 開啟檔案存放的路徑位置。    
2. cat pwfile : 檢視檔案中的現有帳號密碼。    
3. mosquitto_passwd : mosquitto_passwd is a tool for managing password files for mosquitto 
4. mosquitto_passwd -D pwfile accountA : 刪除pwfile檔案中accountA的帳密。  
5. cat pwfile : 檢視檔案中的現有帳號密碼。  
6. mosquitto_passwd -b pwfile accountB passwordB : 新增accountB帳密於pwfile。
7. cat pwfile : 檢視檔案中的現有帳號密碼。

🛠️ File Setting : aclfile.txt (container/Mosquitto/File/mosquitto/config/aclfile 右鍵 edit)   
1. 1.1#User 'admin' has full access to everything : #使用者 'admin' 擁有所有主題的完整存取權限   
    user admin
    topic readwrite # : 授予 'admin' 對所有主題（# 萬用字元）的讀取 (read) 和寫入 (write) 權限。  
    
   1.2#User 'anne' can publish to its own sensor topic and read commands : #使用者 'anne' 可以發布到自己的感測器主題並讀取指令     
    user anne
    topic write anne/feeds/# : 允許 'anne' 在 'anne/feeds/' 命名空間下的所有主題上發布訊息。  
    topic read anne/feeds/# :允許 'anne' 訂閱 'anne/feeds/' 命名空間下讀取所有主題內容。
 
   1.3#User 'gee' can only read data from all devices :#使用者 'gee' 只能讀取所有裝置的資料     
    user gee  
    topic read anne/feeds/+/# : 允許 'gee' 讀取 'anne' 命名空間下兩層及以上的主題   
   
   1.4#User 'guest' has very limited access : #使用者 'guest' 只有非常有限的存取權限  
    user guest  
    topic write public/feeds/# : #允許 'guest' 在 'public/' 命名空間下的所有主題上發布訊息。❌無法讀取anne底下的資料。  
    topic read public/feeds/# : #允許 'guest' 讀取 'public/' 命名空間下的所有主題。❌無法讀取anne底下的資料。  

---

### 二、add images/FUXA in Docker : 作為dashboard
:white_check_mark: 下載FUXA from images/frangoteam/fuxa   
1. in Docker : download images : frangoteam/fuxa > containers  

---

### 三、Resetting Node-Red、influxDB.  
:white_check_mark: 重新建立Node-Red、influxDB :閱讀文件設定volumes等變數。    
1. Node-Red
  1.1 官方文件  https://hub.docker.com/r/nodered/node-red?uuid=DD91A413-C325-4758-B3AF-3D7FE2A72062  
  1.2 need to use the specific tag name to install due to an upstream docker bug in the architecture detection  
   in docker desktop  
   1.2.1 /volumes/create/new volume/ name : NodeRED_data
   1.2.2 /images/"nodered/node-red"/run : | Node-Red | 1880:1880 | host path : NodeRED_data | container path : /data |  
   in terminal
   ```  
   docker run -it -p 1880:1880 -v NodeRED_data:/data --name mynodered nodered/node-red:1.0.1-10-minimal-arm32v6  
   ```  
  1.3 run in containers  and check file settings.js in volumes/NodeRED_data  
  
2. influxDB
   2.1 官方文件  https://hub.docker.com/_/influxdb?uuid=DD91A413-C325-4758-B3AF-3D7FE2A72062  
   2.2 To start an InfluxDB v2 container, enter the following command  
    in docker desktop  
    2.2.1 /volumes/create/new volume/ name1 : influxdb_data ; name2 :influxdb_config  
    2.2.2 /images/"nodered/node-red"/run : | influxDB | 8086:8086|    
        | host path 1 : influxdb_data   | container path 1 : /var/lib/influxdb2 |    
        | host path 2 : influxdb_config | container path 2 : /etc/influxdb2     |    
    in terminal
     ```  
       docker run \
       -p 8086:8086 \
       -v "$PWD/data:/var/lib/influxdb2" \
       -v "$PWD/config:/etc/influxdb2" \
       influxdb:2
   ```  
   2.3 run in containers (volumes尚未有資料)
   


9. test
  open docker desktop > ps -ef會出現類似下方內容  
  root       553   552  3 09:52 pts/2    00:00:00 /mnt/wsl/docker-desktop/docker-desktop-user-distro proxy --distro-name U  
