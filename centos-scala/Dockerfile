FROM centos:latest
MAINTAINER douhongchen "marcodou@163.com"

### 更新源
RUN yum -y update

### 中文环境
ENV LANG zh_CN.UTF-8
RUN echo "LANG=\"zh_CN.UTF-8\"" > /etc/locale.conf
RUN echo "LC_ALL=\"zh_CN.UTF-8\"" >> /etc/locale.conf
RUN yum -y reinstall glibc-common

### 安装openssh-server和sudo软件包
RUN yum install -y openssh-server openssh-clients sudo git subversion which
RUN yum -y install python-setuptools
RUN easy_install supervisor
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisord.conf
COPY javaenv.sh /etc/profile.d/javaenv.sh

### 将sshd的UsePAM参数设置成no
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

### 添加测试用户work，密码work，并且将此用户添加到sudoers里
RUN useradd work
RUN echo "work:work" | chpasswd
RUN echo "work ALL=(ALL) ALL" >> /etc/sudoers

### 修改root用户密码
RUN echo "root:douhc" | chpasswd

### 创建mount node
RUN mkdir /home/work/.m2
RUN mkdir /home/work/.ivy2
RUN mkdir /home/work/.sbt
RUN mkdir /home/work/.coursier
RUN mkdir /home/work/Documents

#启动sshd服务并且暴露22端口
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir /var/run/sshd
EXPOSE 22
EXPOSE 80
EXPOSE 8080

### CMD ["/usr/sbin/sshd", "-D"]
CMD ["/usr/bin/supervisord"]
