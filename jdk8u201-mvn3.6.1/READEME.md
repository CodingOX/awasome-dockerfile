# 中文

## 下载文件

这个一个Dockerfile脚本，集成了`JDK8 201`和`Maven 3.6.1`

需要注意的是，因为`Orale JDK`的相关政策，直接通过`wget`或者`curl`下载`JDK8 201`已经变的不那么容器了，所以在使用前，还是请你自行下载3个文件：

- `jdk-8u201-linux-x64.tar.gz` ，下载地址：<https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html>
- `jce_policy-8.zip`，下载地址：http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip
- `apache-maven-3.6.1-bin.tar.gz`，下载地址：http://archive.apache.org/dist/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz

下载完成后放在`Dockerfile`的同级目录下。

## 内容

```dockerfile
FROM jeanblanchard/alpine-glibc
MAINTAINER "liuchunfu" <liuchunfu@outlook.com>

# Java Version

ENV M2_HOME=/usr/mvn
ENV JAVA_HOME=/usr/java
ENV PATH=${PATH}:${JAVA_HOME}/bin:${M2_HOME}/bin

# 因为Oracle的JDK不能下载，所以先下载在本地,什么都不带，默认应该是放在temp目录下下的
COPY jdk-8u201-linux-x64.tar.gz /temp/java.tar.gz
RUN  mkdir -p ${JAVA_HOME} && \ 
  tar -zxf /temp/java.tar.gz -C  /temp && \
  mv /temp/jdk1.8.0_201/*  ${JAVA_HOME}

# 处理jdk对于AES256不能加密的问题
COPY jce_policy-8.zip /temp/jce_policy-8.zip
RUN  unzip /temp/jce_policy-8.zip -d /temp && \
   mv /temp/UnlimitedJCEPolicyJDK8/*.jar ${JAVA_HOME}/jre/lib/security/

# 删除jdk目录下一些可能用不到的资源
RUN rm -rf ${JAVA_HOME}/*src.zip \
         ${JAVA_HOME}/lib/missioncontrol \
         ${JAVA_HOME}/lib/visualvm \
         ${JAVA_HOME}/lib/*javafx* \
         ${JAVA_HOME}/jre/lib/plugin.jar \
         ${JAVA_HOME}/jre/lib/ext/jfxrt.jar \
         ${JAVA_HOME}/jre/bin/javaws \
         ${JAVA_HOME}/jre/lib/javaws.jar \
         ${JAVA_HOME}/jre/lib/desktop \
         ${JAVA_HOME}/jre/plugin \
         ${JAVA_HOME}/jre/lib/deploy* \
         ${JAVA_HOME}/jre/lib/*javafx* \
         ${JAVA_HOME}/jre/lib/*jfx* \
         ${JAVA_HOME}/jre/lib/amd64/libdecora_sse.so \
         ${JAVA_HOME}/jre/lib/amd64/libprism_*.so \
         ${JAVA_HOME}/jre/lib/amd64/libfxplugins.so \
         ${JAVA_HOME}/jre/lib/amd64/libglass.so \
         ${JAVA_HOME}/jre/lib/amd64/libgstreamer-lite.so \
         ${JAVA_HOME}/jre/lib/amd64/libjavafx*.so \
         ${JAVA_HOME}/jre/lib/amd64/libjfx*.so

#处理Maven
COPY apache-maven-3.6.1-bin.tar.gz  /temp/maven.tar.gz 
RUN mkdir $M2_HOME && \
  tar -zxf /temp/maven.tar.gz -C /temp && \
  mv /temp/apache-maven-3.6.1/*  $M2_HOME

#直接把temp这个临时目录删除
RUN rm -fr /temp
```

## 构建和运行

构建指令

```shell 
docker build -t alistart/jdk8-mvn3:1.1 .
```

运行的时候创建一个容器即可

```shell
docker run -it --name jdk-mvn alistart/jdk8-mvn3:1.1
```

## 参考

参考地址：<https://hub.docker.com/r/99taxis/mini-java8/dockerfile>



# English

## Download file

This a Dockerfile script, integrates ` JDK8 201 ` and ` Maven 3.6.1 `

It is important to note that because ` Orale JDK ` relevant policy, direct by ` wget ` or ` curl ` download ` JDK8 201 ` have become less container, so before use, or would you please download three files:

- ` JDK-8u201-Linux-x64.tar.gz `, download address: < https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html >
- ` jce_policy-8.zip `, download address: http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip
- ` apache maven- 3.6.1 track - bin. Tar. Gz `, download address: http://archive.apache.org/dist/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz

After the download，put 3 files and ` Dockerfile ` directory at the same level.

## Content

```dockerfile
FROM jeanblanchard/alpine-glibc
MAINTAINER "liuchunfu" <liuchunfu@outlook.com>

# Java Version

ENV M2_HOME=/usr/mvn
ENV JAVA_HOME=/usr/java
ENV PATH=${PATH}:${JAVA_HOME}/bin:${M2_HOME}/bin

# 因为Oracle的JDK不能下载，所以先下载在本地,什么都不带，默认应该是放在temp目录下下的
COPY jdk-8u201-linux-x64.tar.gz /temp/java.tar.gz
RUN  mkdir -p ${JAVA_HOME} && \ 
  tar -zxf /temp/java.tar.gz -C  /temp && \
  mv /temp/jdk1.8.0_201/*  ${JAVA_HOME}

# 处理jdk对于AES256不能加密的问题
COPY jce_policy-8.zip /temp/jce_policy-8.zip
RUN  unzip /temp/jce_policy-8.zip -d /temp && \
   mv /temp/UnlimitedJCEPolicyJDK8/*.jar ${JAVA_HOME}/jre/lib/security/

# 删除jdk目录下一些可能用不到的资源
RUN rm -rf ${JAVA_HOME}/*src.zip \
         ${JAVA_HOME}/lib/missioncontrol \
         ${JAVA_HOME}/lib/visualvm \
         ${JAVA_HOME}/lib/*javafx* \
         ${JAVA_HOME}/jre/lib/plugin.jar \
         ${JAVA_HOME}/jre/lib/ext/jfxrt.jar \
         ${JAVA_HOME}/jre/bin/javaws \
         ${JAVA_HOME}/jre/lib/javaws.jar \
         ${JAVA_HOME}/jre/lib/desktop \
         ${JAVA_HOME}/jre/plugin \
         ${JAVA_HOME}/jre/lib/deploy* \
         ${JAVA_HOME}/jre/lib/*javafx* \
         ${JAVA_HOME}/jre/lib/*jfx* \
         ${JAVA_HOME}/jre/lib/amd64/libdecora_sse.so \
         ${JAVA_HOME}/jre/lib/amd64/libprism_*.so \
         ${JAVA_HOME}/jre/lib/amd64/libfxplugins.so \
         ${JAVA_HOME}/jre/lib/amd64/libglass.so \
         ${JAVA_HOME}/jre/lib/amd64/libgstreamer-lite.so \
         ${JAVA_HOME}/jre/lib/amd64/libjavafx*.so \
         ${JAVA_HOME}/jre/lib/amd64/libjfx*.so

#处理Maven
COPY apache-maven-3.6.1-bin.tar.gz  /temp/maven.tar.gz 
RUN mkdir $M2_HOME && \
  tar -zxf /temp/maven.tar.gz -C /temp && \
  mv /temp/apache-maven-3.6.1/*  $M2_HOME

#直接把temp这个临时目录删除
RUN rm -fr /temp
```

