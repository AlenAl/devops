# devops
小型自建GO开发测试环境-持续集成持续发布

## 一、安装docker
  yum install -y yum-utils device-mapper-persistent-data lvm2    
  yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo  
  yum makecache fast  
  yum -y install docker-ce  
  systemctl start docker`

## 二、安装gitlab
下载镜像：docker pull gitlab/gitlab-ce:13.1.4-ce.0  

启动：  
docker run -d  -p 8443:443 -p 8181:80 -p 222:22 --name gitlab --restart always -v /home/gitlab/config:/etc/gitlab -v /home/gitlab/logs:/var/log/gitlab -v /home/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:13.1.4-ce.0  

vim /home/gitlab/config/gitlab.rb  

//配置http协议所使用的访问地址,不加端口号默认为80  
external_url 'http://192.168.199.231'  
 
配置ssh协议所使用的访问地址和端口  
gitlab_rails['gitlab_ssh_host'] = '192.168.199.231'  
gitlab_rails['gitlab_shell_ssh_port'] = 222 # 此端口是run时22端口映射的222端口  

注：占用内存高，停掉进程  
gitlab-ctl stop prometheus  

## 三、安装jenkins  
下载镜像：docker pull jenkins/jenkins:2.235.2-lts-centos7  

启动：   
docker run -d -p 8080:8080 -p 50000:50000 -v /home/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v /etc/docker:/etc/docker --name jenkins --restart always --privileged=true  -u root jenkins/jenkins:2.235.2-lts-centos7  

容器内操作  
wget https://golang.google.cn/dl/go1.14.4.linux-amd64.tar.gz  
export GOROOT=/var/jenkins_home/go  
export GOPATH=/var/jenkins_home/gowork  
export GOBIN="/var/jenkins_home/gowork/bin"  
export PATH="$PATH:$GOROOT/bin:$GOBIN"  
export GO111MODULE=on  
export GOPROXY=https://goproxy.cn  

 插件：  
Go-Plugin-Jenkins  
GitLab Plugin  
Docker  
Go  
SSH  
docker build step  
Blue Ocean  

Blue Ocean 流水线配置 （单元测试）  
代码检查、代码编译、单元测试  
构建镜像、推送镜像、远程发布、消息通知  

## 四、启动Mysql镜像  
docker pull mysql:5.7.31  

启动：   
docker run -d --name mysql --restart always -p 3306:3306 -v /home/mysql/mysql:/var/lib/mysql -v /home/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7.31  

## 五、启动mile-api镜像  
jenkins自动构建-开发版本  

启动：  
docker run -d -p 80:9001 --name mile-api --restart always --privileged=true mile-api:dev  

## 六、启动Redis镜像
启动：docker run --name redis-api --restart always -p 6380:6379 -d redis:6.0.6 --requirepass Alen123456

## 七、安装Docker私有仓库 http://192.168.6.211:8282/
harbor.v1.10.1.tar.gz  
./install.sh  

登陆私有仓库：  
docker login 192.168.6.211:8282  

推送私有仓库：  
docker tag mile-api:1.0 192.168.6.211:8282/oa/mile-api:1.0  
docker push 192.168.6.211:8282/oa/mile-api:1.0  

拉取私有仓库：  
docker pull 192.168.6.211:8282/oa/mile-api:1.0  


## 八、安装Typecho导航  
docker run -d --name=typecho -p 86:80 -v /home/typecho/app:/app -v /home/typecho/data:/data --restart always  -u root 80x86/typecho:latest
