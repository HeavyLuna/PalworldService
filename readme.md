# 幻兽帕鲁优化部署

由于幻兽帕鲁内存在低于32G的时候很容易炸，写了个服务来自动重启服务。并写了个脚本检测内存重启服务。

此方案支持Linux部署环境

1. palserver.service 为封装的服务。用steam用户处理即可，部署步骤如下：

   ```
   sudo cp ./palserver.service /etc/systemd/system/palserver.service
   sudo touch /var/log/palserver.log
   sudo systemctl daemon-reload
   sudo systemctl enable palserver.service
   sudo systemctl start palserver.service
   ```

2. pal_auto_restart 为自动重启脚本，必须配合palserver.service使用，部署步骤如下：

   ``` 
   sudo cp ./pal_auto_restart /usr/local/bin/pal_auto_restart
   sudo chmod +x /usr/local/bin/pal_auto_restart 
   sudo touch /var/log/pal_auto_restart.log
   sudo chmod 666 /var/log/pal_auto_restart.log
   ```

3. crontab增加定时器，第一条为每天早上4点自动重启，第二条为每5分钟检查一下内存：

   ```
   0   4 * * * /usr/local/bin/pal_auto_restart 10
   */5 * * * * /usr/local/bin/pal_auto_restart 90
   ```

   