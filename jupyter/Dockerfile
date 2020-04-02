# 指定基础image
FROM jupyter/datascience-notebook:03b897d05f16
MAINTAINER Xin Xiong <xiongxin20008@126.com>

# 替换CRAN镜像为国内的镜像，可以更快的安装R packages
ARG CRAN_MIRROR=https://mirrors.tuna.tsinghua.edu.cn/CRAN/

# 由于要安装程序，使用root身份
USER root

# 添加本地文件夹package到镜像中的/src
ADD ./package/ /src/

# link lib
RUN \
  ln -s /opt/conda/lib/libpcre.so /usr/lib/  && \
  ln -s /opt/conda/lib/liblzma.so /usr/lib/  && \
  ln -s /opt/conda/lib/libbz2.so /usr/lib/  && \
  ln -s /opt/conda/lib/libz.so /usr/lib/  && \
  ln -s /opt/conda/lib/libiconv.so /usr/lib/  && \
  ln -s /opt/conda/lib/libicuuc.so /usr/lib/  && \
  ln -s /opt/conda/lib/libicui18n.so /usr/lib/

# 安装前面添加到镜像/src文件夹中rpacks.txt文件中的R package
# 且使用国内的镜像地址CRAN_MIRROR
RUN \
  cd /src && \
  R -e 'install.packages(sub("(.+)\\\\n","\\1", scan("rpacks.txt", "character")), repos="'"${CRAN_MIRROR}"'")'

# 安装前面添加到镜像/src文件夹中requirements.txt文件中的Python package
RUN \
  cd /src && \
  pip --no-cache-dir install -r requirements.txt && \
  rm -rf /root/.cache

# 直接使用conda安装Python package
RUN conda install -c rdkit rdkit

# 切换到默认普通用户
USER jovyan
