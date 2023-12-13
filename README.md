# cloudflare-api-ddns 
  
说明：该脚本可实现自动更新cloudflare的dns记录，检查频率高达每分钟检查一次 
# 特点： 
     1、公网ip获取方式为从本机执行脚本获取，也就是执行命令的主机是自己拨号并有一个公网ip 
     2、需要用到ifconfig命令，需要安装net-tools这个软件包  
     3、因为用到了source命令，所以需要用bash执行环境，而不是sh  
     
备注： 关于第一点限制说明，执行脚本的主机必须有公网ip，这样就可以不受第三方接口的限制，比如接口频率限制，响应时间不稳定等，直接本机获取公网ip  
  
安装 net-tools  
  
```
apt-get install net-tools 
```

B站视频介绍 https://www.bilibili.com/video/BV1A5411A7DR/  
油管视频介绍 https://www.youtube.com/watch?v=3XmMTWJI8XE   


在linux中设置定时调度
```shell
crontab -e 

# 每1分钟执行一次
* * * * * /data/cf_ddns.sh -f true >> /tmp/log/ddns.log
# 每5分钟执行一次
*/5 * * * * /data/cf_ddns.sh -f true >> /tmp/log/ddns.log
```

在$HOME目录会记录对应的信息，模板如下：
下面对应的每个字段都要填写正确。
```
cfzone_id=
cfrecord_id=
cfzone_name=hoey.top # 主域名
cfrecord_name=dip.hoey.top # 二级域名
```
关于cf_zone_id的获取：
<img width="1402" alt="image" src="https://github.com/hoey94/cloudflare-api-ddns/assets/13510799/f2750bb0-a45e-492c-8177-7da2fa843085">


关于cf_record_id的获取：
需要提前创建好二级域名，然后用下面的命令获取id
55b531f053855eab73e2c665db8c4243是zone_id,KEY是你申请的KEY，可以在链接里面找https://dash.cloudflare.com/profile/api-tokens
```
curl -s -X GET "https://api.cloudflare.com/client/v4/zones/55b531f053855eab73e2c665db8c4243/dns_records?name=dip.hoey.top" \
 -H "X-Auth-Email: 646660803@qq.com"  \
 -H "X-Auth-Key: $KEY" \
 -H "Content-Type: application/json"   
```
请求成功返回的id字段就是record_id
