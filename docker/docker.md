## docker

#### docker 安装

> 特殊符号 `

1. Uninstall old versions

   ```
   sudo yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

2. Set up the repository

   ```
   sudo yum install -y yum-utils
   ```

   ```
   #官方镜像
   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   
   #阿里云镜像
   sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo 
   ```

3. Install the *latest version* of Docker Engine and containerd

   ```
   sudo yum install docker-ce docker-ce-cli containerd.io
   ```

   > If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

4. Start Docker.

   ```
   #启动
   sudo systemctl start docker
   #停止
   sudo systemctl stop docker
   #重启		
   sudo service docker restart
   #开机启动
   sudo systemctl enable docker
   ```

5. 由于国内网络问题，需要配置加速器来加速。

   ```
   sudo vi /etc/docker/daemon.json
   ```

   在配置文件中加入

   ````
   {
     "registry-mirrors": ["http://hub-mirror.c.163.com"]
   }
   ````

   > 国内加速服务:
   > 网易：https://hub-mirror.c.163.com/
   > 阿里云：https://<你的ID>.mirror.aliyuncs.com  (需要自己注册)
   > 七牛云加速器：https://reg-mirror.qiniu.com
   > 阿里云镜像加速地址获取： https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

6. Uninstall docker

   ````
   # 卸载Docker Engine，CLI和Containerd软件包
   sudo yum remove docker-ce docker-ce-cli containerd.io
   # 主机上的映像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷
   sudo rm -rf /var/lib/docker
   sudo rm -rf /var/lib/containerd
   ````

#### docker-compose 安装

1. install

   ````
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    # 若是github访问太慢，可以用daocloud下载
    sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    
    ################## 其它安装方式 pip3
    #1 由于pip有版本限制，先进行一次升级
    python3 -m pip install -U pip 
    #2 直接通过pip3安装docker-compose
    sudo pip3 install -U docker-compose
    #3 下面的代码使compose可以在sudo下执行
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
   
   ````

2. Apply executable permissions to the binary:

   ````
   sudo chmod +x /usr/local/bin/docker-compose
   # 连接到/usr/bin下--这样就可以sudo执行了
   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
   ````

3. To uninstall Docker Compose if you installed using `curl`

   ````
   sudo rm /usr/local/bin/docker-compose
   ````

4. Common commands

   ````
   docker-compose up -d nginx           构建建启动nignx容器
   docker-compose exec nginx bash      登录到nginx容器中
   docker-compose down               删除所有nginx容器,镜像
   docker-compose ps                  显示所有容器
   docker-compose restart nginx          重新启动nginx容器
   docker-compose run --no-deps --rm php-fpm php -v 在php-fpm中不启动关联容器，并容器执行php -v 执行完成后删除容器
   docker-compose build nginx           构建镜像 。    
   docker-compose build --no-cache nginx  不带缓存的构建。
   docker-compose logs nginx           查看nginx的日志 
   docker-compose logs -f nginx          查看nginx的实时日志
   docker-compose config -q            验证（docker-compose.yml）文件配置，当配置正确时，不输出任何内容，当文件配置错误，输出错误信息。 
   docker-compose events --json nginx    以json的形式输出nginx的docker日志
   docker-compose pause nginx         暂停nignx容器
   docker-compose unpause nginx       恢复ningx容器
   docker-compose rm nginx            删除容器（删除前必须关闭容器）
   docker-compose stop nginx          停止nignx容器
   docker-compose start nginx          启动nignx容器
   ````

   

#### compose  nginx

1. docker 运行，用来获取我们需要的配置文件

   ````
   #1 运行
   sudo docker run -p 80:80 --name nginx -v /home/compose-workspace/nginx/html:/usr/share/nginx/html -v /home/compose-workspace/nginx/logs:/var/log/nginx -d nginx:latest
   #2 拷贝文件出来
   sudo docker container cp nginx:/etc/nginx /home/compose-workspace/nginx/
   cd /home/compose-workspace/nginx/
   mv nginx conf
   #3 关闭并移除容器
   sudo docker stop nginx
   sudo docker rm nginx
   ````

1. yml document：

   ````
   version: '3.1'
   services:
     nginx:
       image: nginx:latest
       container_name: nginx
       restart: always
       ports:
         - 80:80
         - 443:443
       volumes:
         - /home/compose-workspace/nginx/conf:/etc/nginx
         - /home/compose-workspace/nginx/logs:/var/log/nginx
         - /home/compose-workspace/nginx/html:/usr/share/nginx/html
   ````

2. 命令

   ````
   sudo docker-compose -f  /home/compose-workspace/nginx.yml up -d
   sudo docker-compose -f  /home/compose-workspace/nginx.yml down
   sudo docker-compose -f  /home/compose-workspace/nginx.yml ps
   sudo docker-compose -f  /home/compose-workspace/nginx.yml logs
   ````

3. nginx部署好后就可以直接使用了，下面是页面上传云服务器的演示代码(具体目录根据自己情况定)

   ````
   scp -r /c/Users/Jmake/H5Project/mrs-c-front/player/dist/* root@47.108.83.82: /home/compose-workspace/nginx/html/
   ````

####  compose  jenkins

1. 启动准备，创建目录，并给予权限

   ````
   # 创建目录（具体目录自己创建，下面代码只是演示）
   mkdir  /home/compose-workspace/jenkins/jenkins_home
   # 给予权限
   chown -R 1000:1000 /home/compose-workspace/jenkins/jenkins_home
   ````

2. yml document

   ````
   version: '3'
   services:
     jenkins:
       image: 'jenkins/jenkins:lts'
       container_name: jenkins
       restart: always
       ports:
         - '8080:8080'
         - '50000:50000'
       volumes:
         - '/home/compose-workspace/jenkins/jenkins_home:/var/jenkins_home'
         - '/home/compose-workspace/nginx/html:/var/www'
   ````

   > 注意：'/root/workspace/data/nginx/html:/var/www'该目录的挂载主要是跟nginx联动部署web应用的

3. 命令与nginx一样，不再叙述

4. 项目内bash命令

   ````
   ##################下面是项目内配置，用于代码构建及转移到部署目录  
   rm -rf node_modules
   npm config set registry https://registry.npm.taobao.org 
   npm install --max-old-space-size=4096
   npm run build:prod 
   
   tar -zcvf ./dist.tar ./dist
   rm -rf /var/www/*
   mv ./dist.tar /var/www/
   tar -zxvf /var/www/dist.tar -C /var/www/
   mv /var/www/dist/* /var/www/
   rm -rf /var/www/dist.tar /var/www/dist
   ````

   

####  compose  gitea（mysql）

1. ##### 安装前准备（ssh穿透）

   1.  mount `/home/git/.ssh` of the host into the container (yml文件已经配置，这一步跳过)

   2.  a SSH key pair needs to be created on the host（用户名自己定，我的是git）

      ```bash
      sudo -u git ssh-keygen -t rsa -b 4096 -C "Gitea Host Key"
      ```

   3. step a file named `/app/gitea/gitea` (with executable permissions) needs to be created on the host（注意：一定要给与执行权限chmod）

      ```bash
      ssh -p 2222 -o StrictHostKeyChecking=no git@127.0.0.1 "SSH_ORIGINAL_COMMAND=\"$SSH_ORIGINAL_COMMAND\" $0 $@"
      ```

   4. add the public key of the key you created above (“Gitea Host Key”) to `~/git/.ssh/authorized_keys`（.ssh目录的具体位置根据自己的情况设置）

      ```bash
      echo "$(cat /home/git/.ssh/id_rsa.pub)" >> /home/git/.ssh/authorized_keys
      ```

2. yml document 

   ````
   version: "3"
   
   networks:
     gitea:
       external: false
   services:
     server:
       image: gitea/gitea:1.14.2
       container_name: mygitea
       privileged: true 
       environment:
         - USER_UID=1000
         - USER_GID=1000
         - USER=git
         - DB_TYPE=mysql
         - DB_HOST=db:3306
         - DB_NAME=gitea
         - DB_USER=gitea
         - DB_PASSWD=gitea
         - TZ=Asia/Shanghai
         - DISABLE_GRAVATAR=true
       restart: always
       networks:
         - gitea
       volumes:
         - /root/workspace/data/gitea/data:/data
         - /home/git/.ssh/:/data/git/.ssh
         - /etc/timezone:/etc/timezone:ro
         - /etc/localtime:/etc/localtime:ro
       ports:
           - "10080:3000"
           - "10022:22"
           - "127.0.0.1:2222:22"
       depends_on:
         - db
   
     db:
       image: mysql:8.0.23
       container_name: mydb
       restart: always
       environment:
         - MYSQL_ROOT_PASSWORD=gitea
         - MYSQL_USER=gitea
         - MYSQL_PASSWORD=gitea
         - MYSQL_DATABASE=gitea
       networks:
         - gitea
       ports:
         - "3306:3306"
       volumes:
         - /root/workspace/data/mysql:/var/lib/mysql
   ````
   
3. 安装完成后页面配置展示

   ![image-20210603152544657](C:\Users\Jmake\AppData\Roaming\Typora\typora-user-images\image-20210603152544657.png)

4. 其它操作（自用）

   ````
   rm -rf /root/workspace/data/gitea/data/*
   rm -rf /root/workspace/data/mysql/*
   
   
   ````

> 特殊符号 ``



#### compose-all-merge：前面所有的yml合并后的一次性安装部署

在使用下面代码时，最好先创建好对应目录，并且创建好一个用户，并将对应的目录权限进行分配：

````
version: "3.5"

networks:
    gitea:
        name: gitea
        driver: bridge
services:
  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - 80:80
      - 443:443
    volumes:
      - /home/compose-workspace/nginx/conf:/etc/nginx
      - /home/compose-workspace/nginx/logs:/var/log/nginx
      - /home/compose-workspace/nginx/html:/usr/share/nginx/html
  jenkins:
    image: "jenkins/jenkins:lts"
    container_name: jenkins
    restart: always
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - "/home/compose-workspace/jenkins/jenkins_home:/var/jenkins_home"
      - "/home/compose-workspace/nginx/html:/var/www"
  gitea:
    container_name: gitea
    image: gitea/gitea:latest
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - SSH_PORT=10022
      - LFS_START_SERVER=false
      - DB_TYPE=sqlite3
    restart: always
    networks:
      - gitea
    volumes:
      - ./gitea:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - "3000:3000"
      - '10022:10022'
````



````
#######################################################################
###################1、Linux 创建用户角色并添加ssh登录权限###################
#######################################################################
# 1 root权限下
useradd /home/xiaoli xiaoli
# 2
passwd xiaoli
 
# 3
cd /home  && mkdir xiaoli
 
# 4 
chown xiaoli xiaoli/
 
# 5 切到xiaoli的角色
su xiaoli
 
# 6
cd /home/xiaoli
# then 
mkdir .ssh
# then 
cd .ssh
# then 
vi authorized_keys
# 写入ssh public key 后
chmod 600 authorized_keys
 
# 7
su root
#then 
 vi /etc/ssh/sshd_config
 
# 写入(daxiong是原有的用户）
 
AllowUsers daxiong xiaoli
 
# 保存
/usr/sbin/sshd -T
#没错后
service sshd restart

###################################################
###################2、赋予root权限###################
###################################################

方法一：修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉

## Allows people in group wheel to run all commands
%wheel    ALL=(ALL)    ALL

然后修改用户，使其属于root组（wheel），命令如下：

#usermod -g root tommy

修改完毕，现在可以用tommy帐号登录，然后用命令 su – ，即可获得root权限进行操作。

方法二：修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：

## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
tommy   ALL=(ALL)     ALL

修改完毕，现在可以用tommy帐号登录，然后用命令 sudo – ，即可获得root权限进行操作。

方法三：修改 /etc/passwd 文件，找到如下行，把用户ID修改为 0 ，如下所示：
tommy:x:0:33:tommy:/data/webroot:/bin/bash
````

