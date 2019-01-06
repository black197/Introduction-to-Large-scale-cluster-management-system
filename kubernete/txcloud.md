# 腾讯云服务器ssh配置说明

## 服务器信息
**resourseid      实例名                      公网IP           内网IP**  
ins-4xx9tdj7    APP-ubuntu-1GB-sh-54603     212.64.60.120	172.17.0.8  
ins-1g5my2ed	NAME-ubuntu-1GB-sh-54602	212.64.52.235	172.17.0.12  
ins-govysa91	DATA2-ubuntu-1GB-sh-54601	212.64.88.194	172.17.0.17  
ins-7xyt3lep	DATA1-ubuntu-1GB-sh-54604	212.64.66.254	172.17.0.15  
ins-3jmsbn6p	MASTER-ubuntu-8GB-sh-6601	111.231.137.229	172.17.32.10  

**说明：**
从本地ssh云服务器应使用公网ip（每个公网ip限速1Mbps），在云服务器之间互联应使用内网ip（无限速）。

## 账号信息
用户名：ubuntu  
密码：Passw0rd  
sudo 无密码  
su (root) 密码：000  
su (root) 密码修改命令：sudo passwd  
data1:mysql 数据库 用户名 root 密码 123456  

## 配置命令
rm -rf ~/.ssh  
ssh 212.64.60.120  
yes  
sudo vi ~/.ssh/id_rsa  
sudo vi ~/.ssh/config  
sudo vi /etc/hosts  
ssh app  

**~/.ssh/id_rsa:**  
-----BEGIN RSA PRIVATE KEY-----  
MIICXAIBAAKBgQC034DXHtQBAEyejBG7DHqfqH8GV/DSD2oGZT+xCwDv1TBwlwMH  
jueiSYGL1sxaHlIBQ9A6wDdJMKET9ya+S4iLwwlB3PCh+5/UIaFEnat0Ry/DCHrd  
P+HLsGr5VgGSdZ6JKcxADOHIo5kYyUyGTufwoiCF5X0utci8/DDExNFKLwIDAQAB  
AoGAbpn5RBJS22XedFj8gp+n4Dd9rVhbJ2hLkiuZnd43rXB01XRSYu3M0N0X/XXU  
sgq2ZJWeID7nz7aP2RCZvWWc0arAOe9qwfvYqvdFLz0s9YJRyfp13tttB3bRMr9y  
sttmXMtkuCFQLoxs6/rT+L08wlmJSHlyJo++7hyzu/Z3GzECQQDXZyK2cc19GPgn  
6weRsEz3QTWctvW+SAzeazDuT0F8sJSA6PM8lPgNqPTfJh0S77aefehj0e3IQCqw  
QRgnS/wHAkEA1vZbLKyzK6NWma4FLMNc0qrOOC7U1qurvyTUarPUDC5+HmYPPUPT  
5a7grqvl4EKPbtXAVfOkhI5T9vNyAqSGmQJAYglq3ybEo98tct2hwElBfneLcxxC  
lKwuTzzyNESWRa4IqPNdYYFbtLvlV3r9WJUJxPEBSA1P8AhkZXv7BkerGQJAAzAs  
MgFtttv5UNYv5XYQTl+SJ2sqZPSu22rka6C3KGcYH8NLvpDe960cT/rkserKzc4F  
yECQ1BZ4UFVT/44JIQJBAKSpRW0NqGTbscKckvA70wYLvA1dyfo/8dOpOsrVpK8H  
qVHzp3YI7IQ2KRMyTkO2u/b8F8ouqwtZJwbOAt95wq8=  
-----END RSA PRIVATE KEY-----  


**~/.ssh/config:**  
Host app  
Port 22  
User ubuntu  

Host name  
Port 22  
User ubuntu  

Host data1  
Port 22  
User ubuntu  

Host data2  
Port 22  
User ubuntu  

Host master  
Port 22  
User ubuntu  

**/etc/hosts:**  
212.64.60.120   app  
212.64.52.235   name  
212.64.66.254   data1  
212.64.88.194   data2  
111.231.137.229 master  

上方为公网ip域名，从本地ssh云服务器时使用（每个公网ip限速1Mbps）  
——————————————————————  
上方为内网ip域名，云服务器之间相互连接时使用（无限速）  

172.17.0.8 app  
172.17.0.12 name  
172.17.0.15 data1  
172.17.0.17 data2  
172.17.32.10 master  
