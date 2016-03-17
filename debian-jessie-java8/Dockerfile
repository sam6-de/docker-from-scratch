FROM bundeskanzler4711/debian-jessie-base

RUN apt-get -y update 
RUN apt-get -y --force-yes dist-upgrade
RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list
RUN echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install -y --force-yes oracle-java8-installer oracle-java8-set-default oracle-java8-unlimited-jce-policy
RUN apt-get -y --force-yes autoremove
RUN apt-get clean

CMD java -version
