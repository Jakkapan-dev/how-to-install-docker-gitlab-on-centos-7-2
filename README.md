# how-to-install-docker-gitlab-on-centos-7-2

---------------setting centos-------------
sed -i 's|^mirrorlist=|g' /etc/yum.repos.d/CentosOS-Base.repo

sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Base.repo

yum update 
yum install httpd
systmectl enable httpd
systemctl start httpd.service
-----------------------------------------------

--------------install firewall ------------------
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewal-cmd --reload
---------------------------------------------------

-------------install ftpd--------------------------
yum install vsftpd
systemctl enable vsftpd
systemctl start vsftpd
firewall-cmd --permanent --zone=public --add-service=ftp
firewall-cmd --reload
-----------------------------------------------------

-----------install docker---------------------------
sudo yum remove -y docker docker-common docker-selinux docker-engine (ลบ docker เก่าออก)
sudo yum install -y yum-utils

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo (ติดตั้ง repo ของ docker)

sudo yum install -y docker-ce docker-ce-cli containerd.io ติดตั้ง docker enagine

sudo systemctl enable docker บูต docker
sudo systemctl start docker

sudo docker run hello-world ทดสอบ docker

---------------- install gitlap-----------------------------------
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version ติดตั้ง Docker Compose

sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --permanent --add-port=22/tcp
sudo firewall-cmd --reload เปิดพอร์ตให้ GitLab ใช้งาน

sudo mkdir -p /srv/gitlab/config /srv/gitlab/logs /srv/gitlab/data สร้างโฟลเดอร์เก็บข้อมูล GitLab (persistent data)

ติดตั้ง GitLab CE ผ่าน Docker
sudo docker run --detach \
  --hostname gitlab.local \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume /srv/gitlab/config:/etc/gitlab \
  --volume /srv/gitlab/logs:/var/log/gitlab \
  --volume /srv/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest

  sudo docker logs -f gitlab ดูสถานะ

  
  เข้าหน้าเว็บ GitLab
  เปิดเว็บเบราว์เซอร์ แล้วเข้าไปที่:
👉 http://gitlab.local
หรือ
👉 http://192.168.212.1

sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password ดูรหัสผ่านเริ่มต้นของผู้ใช้ root

sudo docker ps ตรวจสอบ Container
-----------------------------------------------------------------------------------

 






