# A file for installing mono
FROM centos:centos7
MAINTAINER Henrik Feldt

RUN curl -f https://packagecloud.io/install/repositories/haf/oss/config_file.repo\?os\=centos\&dist\=7\&name\=docker >/etc/yum.repos.d/haf_oss.repo
RUN sed -i'' 's/repo_gpgcheck=1/repo_gpgcheck=0/' /etc/yum.repos.d/haf_oss.repo
RUN yum -q makecache -y --disablerepo='*' --enablerepo='haf_oss'

RUN yum install mono fsharp -y

