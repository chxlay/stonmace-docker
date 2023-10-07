# 选择 Linux 版本 https://hub.docker.com
FROM centos:centos7

LABEL author=Alay<chxlay@aliyun.com>

# https://github.com/goharbor/harbor/releases
ARG HB_VERSION=2.6.0
ARG COMPOSE_V=2.10.0

# 创建临时工作目录,下载 tar包文件的
RUN set -x \
    && mkdir /opt/tmp -p \
    && cd /opt/tmp


# 二选一使用，下载地址 gitHub 太慢,选择这个 https://storage.googleapis.com/harbor-releases/release-2.6.0/harbor-offline-installer-v2.8.2.tgz
RUN curl -SLO https://storage.googleapis.com/harbor-releases/release-${HB_VERSION}/harbor-offline-installer-v${HB_VERSION}.tgz

# 二选一使用，或者采用本地上传方式（下载文件太大）
COPY ./harbor-offline-installer-v${HB_VERSION}.tgz ./

RUN set -x \
    && tar -zxvf harbor-offline-installer-v${HB_VERSION}.tgz \
    && mv harbor /home

# 删除安装
RUN set -x \
    && rm -rf /opt/tmp

# 挂在配置文件和 数据路径
VOLUME ["/data","/var/log/harbor"]

# 执行安装
WORKDIR /home/harbor

# 暴露端口号
EXPOSE 80 443

# 调试时用此命令
#ENTRYPOINT ["/usr/sbin/init"]

# 初始化程序
ENTRYPOINT ["./prepare"]
# 启动程序
CMD ["./install.sh"]