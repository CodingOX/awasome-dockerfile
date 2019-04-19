FROM jeanblanchard/alpine-glibc
MAINTAINER "liuchunfu" <liuchunfu@outlook.com>

# Java Version

ENV M2_HOME=/usr/lib/mvn
ENV JAVA_HOME=/usr/java
ENV PATH=${PATH}:${JAVA_HOME}/bin

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