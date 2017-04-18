FROM 	sinenomine/clefos-base-s390x:latest

ENV 	hadoop_ver 2.8.0
ENV 	spark_ver 1.5.2

COPY 	IBM_Spark_DK_1.5.2.0_Linux_s390x.bin spark.rsp /

RUN 	yum update --setopt=tsflags=nodocs -y && \
        yum install -y --setopt=tsflags=nodocs net-tools curl tar zip telnet hostname procps-ng && \
        echo "Installing Apache Spark" && \
        /IBM_Spark_DK_1.5.2.0_Linux_s390x.bin -f /spark.rsp -i silent && \
        rm -f /IBM_Spark_DK_1.5.2.0_Linux_s390x.bin /spark.rsp cbe* && \
	yum clean all && \
	rm -rf /var/log/yum.log /tmp/* /var/cache/yum/*

#RUN 	mkdir -p /opt && \
#	cd /opt && \
#	curl http://www.us.apache.org/dist/hadoop/common/hadoop-${hadoop_ver}/hadoop-${hadoop_ver}.tar.gz | \
#		tar -zx hadoop-${hadoop_ver}/lib/native && \
#	ln -s hadoop-${hadoop_ver} hadoop && \
#	echo Hadoop ${hadoop_ver} native libraries installed in /opt/hadoop/lib/native

# Add the GCS connector.
RUN 	cd /opt/ibm/spark/lib && \
    	curl -O https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-latest-hadoop2.jar

RUN	chown 0:0 /etc/hosts && chmod +w /etc/hosts

ADD 	log4j.properties /opt/ibm/spark/conf/log4j.properties
ADD 	start-common.sh start-worker start-master /
ADD 	core-site.xml /opt/ibm/spark/conf/core-site.xml
ADD 	spark-defaults.conf /opt/ibm/spark/conf/spark-defaults.conf

ENV 	SPARK_HOME=/opt/ibm/spark JAVA_HOME=/opt/ibm/java PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin:$JAVA_HOME/bin
