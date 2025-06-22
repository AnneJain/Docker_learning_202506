https://www.empoweredautomation.com/difference-between-hmi-and-scada#:~:text=The%20main%20difference%20between%20SCADA%20(Supervisory%20Control%20and,system,%20visualize%20data,%20and%20manage%20the%20control%20processes.  
https://medium.com/@turkey039/%E8%87%AA%E5%8B%95%E5%8C%96-ai-%E7%9C%9F%E7%9A%84%E9%82%A3%E9%BA%BC%E7%A5%9E%E5%A5%87-%E5%BE%9E-0-%E9%96%8B%E5%A7%8B%E7%9A%84-n8n-openai-%E5%AF%A6%E6%88%B0%E7%AC%AC%E4%B8%80%E7%AF%87-8a43480369db  
https://ithelp.ithome.com.tw/articles/10365452  

influxDB:課程電腦  
usr name anne  
pw Docker202506  
token -wwPz7xguOBGC-KJzIQPgQ17dRedMWXzi6fRhCCjgc9Tg4gowOb-MOUHxoPnLxiUsbv2djSx10swBTk7UtbFxQ==  

mosquitto.conf檔案下載,放到C:\Users\user\Documents\Docker\Mosquitto\config
https://github.com/eclipse-mosquitto/mosquitto/blob/master/mosquitto.conf

1.cmd 執行建立,因為前面的路徑位置是windows的 C:\Users\user\Documents\Docker\Mosquitto\config
$ docker run -it -p 1884:1883 -v "C:\Users\user\Documents\Docker\Mosquitto\config":/mosquitto/config eclipse-mosquitto
--1884原因:已先用docker環境建立1883:1883,用指令方式再練習建一個
2.docker UI介面製作
Volumes :host path:點選.conf檔案的路徑 /container path:參考下列網址文件/Configuration>複製此路徑 /mosquitto/config
https://hub.docker.com/_/eclipse-mosquitto?uuid=D61E1A08-72C7-47ED-914B-0A13A7417072

Creating a password file
https://mosquitto.org/documentation/authentication-methods/

mosquitto_passwd -c <password file> <username>
mosquitto_passwd -c 路徑/filename admin

到docker exec下指令
