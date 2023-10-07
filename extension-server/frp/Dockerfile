# 以build 镜像的时候下载frp程序包的方式的（下载的工具比较多，问题也比较多）
FROM centos:centos7

LABEL author=Alay<chxlay@aliyun.com>
ENV FRP_DIR=/var/frp

ARG VERSION

# 创建临时工作目录,下载 frp tar包文件的
WORKDIR /tmp

# 本地拷贝 Frp 程序压缩包到 容器内(上传和下载 二选一)
COPY frp_${VERSION}_linux_amd64.tar.gz ./
# 下载 frp的发行版 tag(上传和下载 二选一)
RUN curl -SlO https://github.com/fatedier/frp/releases/download/v${VERSION}/frp_${VERSION}_linux_amd64.tar.gz

RUN set -x \
    # 解压 frp
    && tar -xvf frp_${VERSION}_linux_amd64.tar.gz \
    # 移动到目录
    && mv frp_${VERSION}_linux_amd64 ${FRP_DIR} \
    # 删除临时路径目录
    && rm -rf /tmp

WORKDIR ${FRP_DIR}
# 创建配置文件
RUN mkdir -p ${FRP_DIR}/conf

# 服务端配置文件由本地拷贝到容器（如果使用配置文件挂载到宿主机的话，此处无效）
COPY ./*.ini ${FRP_DIR}/conf/

# 挂载配置文件路径到宿主机
VOLUME ${FRP_DIR}/conf

# 执行运行,指定配置文件方式进行启动(客户端：frpc,服务端 frps )
CMD ./frps -c ./conf/frps.ini