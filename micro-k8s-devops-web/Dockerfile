# Version 1.0.0
FROM 192.168.3.101:8001/library/oracle-jdk:1.8

MAINTAINER mubai "zhangzehao37@163.com"

# 定义时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
&& echo 'Asia/Shanghai' >/etc/timezone

EXPOSE 8088

# 环境变量
# JAR运行目录
ENV WORK_PATH /home/app
# 日志数据目录
ENV WORK_DATA /home/data
ENV JAVA_OPTS='-XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Xloggc:$WORK_DATA/logs/gc_%p.log -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=30m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=$WORK_DATA/logs/java_heapdump.hprof'
#由jenkins执行docker build注入参数 MEM_ARG, JAR_FILE
ARG MEM_ARG
ENV MEM_OPTS ${MEM_ARG}

ARG JAR_FILE
ADD target/${JAR_FILE} $WORK_PATH/app.jar

#VOLUME
VOLUME $WORK_DATA $WORK_PATH

# 输出JVM参数
RUN echo $MEM_OPTS $JAVA_OPTS

# WORKDIR
WORKDIR $WORK_PATH

ENTRYPOINT java $MEM_OPTS $JAVA_OPTS -jar -Djava.security.egd=file:/dev/./urandom app.jar
