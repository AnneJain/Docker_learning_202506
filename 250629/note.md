1.自建門禁  
https://makerpro.cc/2021/10/use-m5stack-to-built-smart-access-control-systems/https://makerpro.cc/2021/10/use-m5stack-to-built-smart-access-control-systems/  
2.node red 要先建volumes(第三項指令)
https://hub.docker.com/r/nodered/node-red?uuid=DD91A413-C325-4758-B3AF-3D7FE2A72062  
3.influxDB建立  
https://hub.docker.com/_/influxdb?uuid=DD91A413-C325-4758-B3AF-3D7FE2A72062  
volumes:  
docker run \
    -p 8086:8086 \
    -v "$PWD/data:/var/lib/influxdb2" \
    -v "$PWD/config:/etc/influxdb2" \
    influxdb:2
