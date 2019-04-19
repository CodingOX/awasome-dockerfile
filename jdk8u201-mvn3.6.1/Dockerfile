FROM jeanblanchard/alpine-glibc
MAINTAINER "liuchunfu" <liuchunfu@outlook.com>

# Java Version
ENV JAVA_VERSION_MAJOR 8
ENV JAVA_VERSION_MINOR 201
ENV MAVEN_VERSION="3.6.1" 
ENV M2_HOME=/usr/lib/mvn

# 因为Oracle的JDK不能下载，所以先下载在本地,什么都不带，默认应该是放在temp目录下下的
COPY jdk-8u201-linux-x64.tar.gz /temp/java.tar.gz
RUN  mkdir -p /opt/oracle-jdk && tar -zxf /temp/java.tar.gz -C /opt/oracle-jdk && \
  # 建立软链接
  ln -s /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR} /opt/jdk && \
  #删除jdk源文件
  rm -fr /temp/java.tar.gz

# 处理jdk对于AES256不能加密的问题
COPY jce_policy-8.zip /temp/jce_policy-8.zip
RUN  unzip /temp/jce_policy-8.zip -d /temp && \
   mv /temp/UnlimitedJCEPolicyJDK8/*.jar /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/security/ && \
   rm -fr  /temp/UnlimitedJCEPolicyJDK8/ && \
   rm -fr /temp/jce_policy-8.zip

# 删除jdk目录下一些可能用不到的资源
RUN rm -rf /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/*src.zip \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/lib/missioncontrol \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/lib/visualvm \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/lib/*javafx* \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/plugin.jar \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/ext/jfxrt.jar \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/bin/javaws \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/javaws.jar \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/desktop \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/plugin \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/deploy* \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/*javafx* \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/*jfx* \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libdecora_sse.so \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libprism_*.so \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libfxplugins.so \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libglass.so \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libgstreamer-lite.so \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libjavafx*.so \
         /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre/lib/amd64/libjfx*.so
# 设置java的环境变量
ENV JAVA_HOME /opt/oracle-jdk/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}
ENV PATH ${PATH}:${JAVA_HOME}/bin

#处理Maven
COPY apache-maven-3.6.1-bin.tar.gz  /temp/maven.tar.gz 
RUN mkdir $M2_HOME && tar -zxf /temp/maven.tar.gz -C /temp && \
  mv /temp/apache-maven-$MAVEN_VERSION/*  $M2_HOME && \
  ln -s $M2_HOME/bin/mvn /usr/bin/mvn && \
  rm -fr /temp/maven.tar.gz && rm -fr /temp/apache-maven-$MAVEN_VERSION