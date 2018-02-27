# FROM *** で使用するベースイメージを指定.
FROM centos:7

# OSのタイムゾーン設定
RUN \cp -p -f /usr/share/zoneinfo/Japan /etc/localtime

# コマンドを実行する場合にはRUNを用いる.
# RUN で実行するコマンドは && や \ を用いてなるべく連結させる.
# リポジトリ関連
RUN yum -y install epel-release && \
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

# 念のためglibc更新
RUN yum update -y glibc-common && yum clean all

# phpとapache関連
RUN yum install -y vim git && \
yum install -y --enablerepo=remi,remi-php72 php php-devel php-mbstring php-pdo php-gd php-mysql php-pear php-mcrypt php-tokenizer php-xdebug php-zip httpd mod_ssl iproute

RUN curl -sS https://getcomposer.org/installer | php && \
mv composer.phar /usr/local/bin/composer

# COPY は指定したホストのディレクトリをコピーする.
#COPY path/to/host /usr/share/nginx/html

# CMD はソフトウェアの実行に用いる.
# CMD ["command", "引数","引数"]
# のように記述する.
# ここでコマンドをforegroundで実行させる必要がある.
#CMD ["nginx", "-g", "daemon off;"]
# apacheの場合は以下.
# CMD ["apache2","-DFOREGROUND"]
# CMD命令は最後に記述されたものだけが有効となるので注意.
RUN systemctl enable httpd.service
ENTRYPOINT /sbin/init
