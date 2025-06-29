#🐳 Docker 安裝步驟
 
> 🗓️ 操作日期：2025-06-15  
> 💻 平台環境：WSL2 + Ubuntu + Docker Desktop + MQTTBox

---

### 🎯 今日任務
- 認識 Docker 與本機環境建置
- 修改 WSL RAM 限制
- 使用 Docker 安裝 Node-RED、MySQL、MQTT（Mosquitto）
- 配合MySql資料庫的軟體-HeidiSQL
- 安裝MQTTBox與建立Topic

---
### 一、安裝Docker Desktop  
1.至官網選擇windows x86_64 https://docs.docker.com/desktop/setup/install/windows-install/  
2.打開本機cmd以系統管理員身分執行,指令 wsl --install,完成後重開機  
3.建立user account(同步顯示正在下載ubuntu...等字眼)  
4.修改記憶體大小:  
 4.1 至cmd查看本機memory size,指令 free  
 4.2 WSL Setting/記憶體大小/預設4G左右,改成12288(12*1024) --依自身電腦設定,我的只有8G,max只能設定8192  
 4.3 重開機  
 4.4 至ubuntu確認memory size是否修改成功,指令 free;double check 本機cmd memory size,指令 wsl  free --非必要,個人習慣  
5.安裝Docker:docker desktop install,以系統管理員安裝,完成後重開機  
6.開啟ubuntu check sever,指令 free --非必要,個人習慣  
7.點選桌面docker程式開啟軟體  

### 二、在Docker中安裝 Node-RED、MySQL、MQTT（Mosquitto)  
1.在images搜尋輸入關鍵字下載Node-RED、MySQL、MQTT（Mosquitto）  
2.下載完成後勾選images中物件,點選action建立成container   

 2.1 Node-RED  
 2.1.1 設定 host port：  

| 本機 port | Docker port     |
|-----------|-----------------|
| 1880      | 1880/tcp (預設) |

 2.2 MySQL
須注意本機是否已有安裝 MySQL，若有需先卸載，Docker 設定會衝突到本機的 port。  
 2.2.1 設定 host port（外部:內部顯示為 3306:3306）

| 本機 port（外部） | Docker port（內部） |
|------------------|----------------------|
| 3306             | 3306/tcp (預設)      |
| 33060            | 33060/tcp (預設)     |

 2.2.2 設定環境變數（Environment variables）

| 變數名稱            | 說明     |
|---------------------|----------|
| MYSQL_ROOT_PASSWORD | 自定義   |
| MYSQL_DATABASE      | 自定義   |
| MYSQL_USR           | 自定義   |
| MYSQL_PASSWORD      | 自定義   |

 2.2.3 再新增一個 MySQL 作為 UT 環境
修改 host port：

| 本機 port（外部） | Docker port（內部） |
|------------------|----------------------|
| 3307             | 3306/tcp (預設)      |
| 33070            | 33060/tcp (預設)     |

 2.3 MQTT（Mosquitto）

| 本機 port（外部） | Docker port（內部） |
|------------------|----------------------|
| 1883             | 1883/tcp (預設)      |

3.至container確認是否有啟動:綠燈  
4.chrome輸入本機ip:1880確認建立成功  

### 三、本機安裝配合MySql資料庫的軟體-HeidiSQL  
 3.1 下載安裝後修改並測試是否能連到MYSQL(記得修改port & user password)  
 3.2 匯入DB資料(測試資料等)  
 3.3 下載網址 https://www.heidisql.com/download.php  

### 四、 安裝MQTTBox與建立Topic
 4.1 至微軟商城下載MQTTBox https://apps.microsoft.com/detail/9nblggh55jzg?hl=zh-TW&gl=TW  
 4.2 設定公共連線 mqttgo.io:1883  
 4.3 建立topic,並可同步subscribe進行資料接收  

---

### 💬 其他課程筆記
1. docker desktop會自動建立proxy,linux需手動建立
2. HW:公共 IP 和 私有 IP 的概念與差別  
🌐 公共 IP（Public IP）  
 ✅ 定義：在網際網路上唯一的 IP 位址。由 ISP（網路服務提供商） 指派，用來讓你連到全球網路。  
 ✅ 使用場景：給路由器或伺服器對外連線用。  
🏠 私有 IP（Private IP）  
 ✅ 定義：區域網路（LAN）內部使用的 IP 位址，不能直接對外使用。由 Wi-Fi 路由器或 DHCP server 指定，通常給手機、電腦、印表機等設備用。  
 ✅ 常見範圍：  
 類型	IP 範圍  
 A類	10.0.0.0 ~ 10.255.255.255  
 B類	172.16.0.0 ~ 172.31.255.255  
 C類	192.168.0.0 ~ 192.168.255.255  
📌 NAT（網路位址轉換）:路由器把私有 IP 轉換成公共 IP再對外連線。  

📚 IP 類別簡介（以 IPv4 為主）
IPv4 是由 **32 位元（bit）**組成的 IP 地址（例如：192.168.1.1），而這 32 位會根據開頭的位元來區分屬於哪一類。
| 類別	| 開頭位元	| IP 範圍（第一段）	| 主機數量	| 用途|
| --- | ------ | --------------- | -------| ---|
| A 類	| 0xxxxxxx	| 1.0.0.0 ~ 126.255.255.255	| 約 1.6 億	| 適用超大型網路，例如 ISP |
| B 類	| 10xxxxxx	| 128.0.0.0 ~ 191.255.255.255	| 約 6.5 萬	| 中型網路，如學校、企業 |
| C 類	| 110xxxxx	| 192.0.0.0 ~ 223.255.255.255	| 約 254	| 小型網路，如家庭、小公司 |
| D 類	| 1110xxxx	| 224.0.0.0 ~ 239.255.255.255	| -	| 用於群播（Multicast) |
| E 類	| 1111xxxx	| 240.0.0.0 ~ 255.255.255.255	| -	| 保留用，實驗性用途 |

📦 常見的私有 IP 區段
這三個私有 IP 區段，也剛好來自 A、B、C 類裡面的一小段：

| 類別	| 私有 IP 區段|
| --- | --------- |
| A 類	| 10.0.0.0 ~ 10.255.255.255 |
| B 類	| 172.16.0.0 ~ 172.31.255.255 |
| C 類	| 192.168.0.0 ~ 192.168.255.255 |



