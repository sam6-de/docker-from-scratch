FROM bundeskanzler4711/jessie-minbase

RUN apt-get -y update 
RUN apt-get -y --force-yes dist-upgrade
RUN echo "deb http://http.debian.net/debian jessie-backports main" | tee /etc/apt/sources.list.d/jessie-backports.list
RUN apt-get update
RUN apt-get install -y --force-yes openjdk-8-jre-headless openjdk-8-jdk
RUN apt-get -y --force-yes autoremove
RUN apt-get clean
RUN /usr/sbin/update-java-alternatives -s java-1.8.0-openjdk-amd64

CMD java -version
