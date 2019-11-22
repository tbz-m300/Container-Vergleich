Vergleich von Container Runtimes
--------------------------------

# Container
## Was sind Container

Mit der Technologie Containervirtualisierung können mehrere Hostsysteme isoliert auf dem gleichen Virtsystem ausgeführt werden. 

Im Gegensatz zur Host Virtualisierung wie zum Beispiel mit VMware ESXi, KVM, Microsoft Hyper-V etc. wird kein komplettes Betriebssytem virtualisiert, sondern der Kernel wird vom Hostsystem genommen. Dies hat den Vorteil, dass viele Ressourcen gespart werden, da nur die benötigten Prozesse gestartet werden und nicht ein komplettes Betriebssystem gestartet werden muss. 

Je nach Art der Containervirtualisierung können nur gleiche Distributionen (zum Beispiel bei Jail funktioniert nur FreeBSD Container) auf einem Host ausgeführt werden. Andere Containervirtualisierung Systeme können zum Beispiel verschiedene Linux Distributioen auf dem selben Host betrieben werden. 

So ein Beispiel ist Docker. Wenn das Hostsystem zum Beispiel unter RedHat läuft, können auch Ubuntu, Debian, Alpine Linux etc ausgeführt werden. Linux Systeme sind jedoch nicht unter Windows lauffähig und umgekehrt gilt das selbe. 

Je nach Art der Containervirtualisierung sind verschiedene Verwendungszwecke vorgesehen. FreeBSD Jails sind zum Beispiel primär dazu gedacht, Dienste voneinander zu trennen und so mehr Sicherheit zu schaffen. Docker jedoch eignet sich sehr gut für DevOps um eine einfache Entwicklungsumgebung aufzubauen und diese immer wieder Reproduzierbar zu betreiben. 

Dank der Container Technologie ist es zudem möglich, sehr schnell mehr Ressourcen von zum Beispiel einem Webserver bereit zu stellen, sollten irgendwelche Leistungsspitzen erreicht werden.

## Vergleich Container Runtimes

|                    | jail                     | lxc                          | Docker                  | rkt                    | CRI-O                       | Containerd                        | Podman             |
|--------------------|--------------------------|------------------------------|-------------------------|------------------------|-----------------------------|-----------------------------------|--------------------|
| Entwickler         | FreeBSD                  | Virtuozzo, IBM, Google       | Docker Inc.             | CoreOS Inc.            | RedHat / Intel / SUSE / IBM | Cloud Native Computing Foundation | RedHat             |
| Erscheinungsjahr   | 1999                     | 2008                         | 2013                    | 2014                   | 2017                        | 2017                              | 2019               |
| Aktuelle Version   | -                        | 3.2.1                        | 19.03.4                 | End of Life            | 1.15.2                      | 1.3.0                             | 1.5.0              |
| Lizenz             | FreeBSD License          | GNU LGPL v2.1                | Apache 2.0              | Apache 2.0             | Apache 2.0                  | Apache 2.0                        | MIT                |
| Programmiersprache | C                        | C, Python, Shell, Lua        | Go                      | Go                     | Go                          | Go                                | Go                 |
| Betriebssystem     | FreeBSD                  | Linux                        | Linux / Windows / macOS | Linux / macOS          | Linux                       | Linux                             | Linux / macOS      |
| Project Homepage   | https://www.freebsd.org/ | https://linuxcontainers.org/ | https://www.docker.com/ | https://coreos.com/rkt | https://cri-o.io/           | https://containerd.io/            | https://podman.io/ |

## jail
FreeBSD jails ist eine der ersten Container Technologien. Sie wurde im Jahr 1999 mit FreeBSD 4.0 zum ersten Mal veröffentlicht. 

Jails wurde entwickelt, um das Betriebssystem in mehrere Bereiche aufzuteilen und somit die Security zu erhöhen. Es konnte so zum Beispiel in einer Jail ein Webserver betrieben werden und auf einer weiteren die dazugehörige Datenbank. Diese hatten nur über den Dienst Zugriff untereinander und es konnte nicht von einer Jail auf die andere Jail zugegriffen werden, um zum Beispiel Dateien auf dem Filesystem zu manipulieren. 

Zudem hatte ein Angreifer keinen Zugriff auf das Hostsystem. Somit wurde nur ein kleiner Teil des Betriebssytem kontaminiert bei einem Hacker Angriff. FreeBSD Jails werden hier nicht weiter behandelt. 

Dies ist in diesem Artikel nur Geschichtlich gesehen intressant und hat keine Berührungen mit Kubernetes.

## lxc (ohne Kubernetes)
LXC ist eine Container Technologie, welche für Linux entwickelt wurde. LXC hat die gleichen Ansätze wie FreeBSD Jails, das Betriebssystem zu partitionieren, damit Prozesse ihre eigene Laufzeitumgebung hat, jedoch den selben Kernel verwendet wird. 

Das erste Mal ist LXC im Jahr 2008 erschienen und seit dem von vielen Entwickler wie zum Beispiel Google, IBM, Kernel, Parallels weiterentwickelt worden.

### Installation auf CentOS 7.7.1908
[root@lxc-cos7 ~]# yum -y install epel-release\
[root@lxc-cos7 ~]# yum -y install lxc lxc-templates libcap-devel libcgroup busybox wget bridge-utils lxc-extra\
[root@lxc-cos7 ~]# systemctl start lxc\
[root@lxc-cos7 ~]# systemctl enable lxc\
[root@lxc-cos7 ~]# lxc-checkconfig

### Installation auf Ubuntu 18.04.3
[root@lxc-ubuntu1804 ~]# apt-get -y install lxd\
[root@lxc-ubuntu1804 ~]# lxd init

### Wichtige lxc Befehle
| Befehl      | Beschreibung                              |
|-------------|-------------------------------------------|
| lxc-ls      | Zeigt alle Containers (aktiv und inaktiv) |
| lxc-console | Verbindet mit der Container Console       |
| lxc-start   | Startet einen bestehenden Container       |
| lxc-stop    | Stoppt einen bestehenden Container        |
| lxc-create  | Erstellt einen neuen Container            |
| lxc-destroy | Löscht einen bestehenden Container        |

## Docker (ohne Kubernetes)
Docker wurde im Jahr 2013 von der Firma dotCloud entwickelt und veröffentlicht. dotCloud änderte im Jahr 2013 seinen Namen zu Docker Inc. 

Docker war sehr lange ein Synonym für Containervirtualisierung. Es wurde in viele grosse Linux Distrubutionen wie zum Beispiel RedHat Enterprise Linux 7 oder openSUSE integriert. 

Docker und viele andere Namhafte Hersteller schlossen sich im Jahr 2014 dem Kubernetes Projekt an, um dieses aktiv weiter zu entwickeln. 

Eine weitere bekannte und weit verbreitete Entwicklung von Docker Inc. ist DockerHub. DockerHub dient als Registry, in der die ganzen Container Images gespeichert werden und von jeder Person mit Docker verwendet werden können. 

Docker verwendet Microservices, dass heisst, dass pro Container nur ein Service angeboten wird. Um mehrere Container miteinander zu starten, welche zusammen gehören, kann das Tool docker-compose verwendet werden. 

Docker verwendet Dockerfile, welche in YAML geschrieben sind, um ein Image zu beschreiben. Aus einem Dockerfile wird jeweils ein Image gebildet. Dockerfile werden auch von anderen Container Runtimes unterstützt.

### Installation auf CentOS 7.7.1908
[root@docker-cos7 ~]# yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine\
[root@docker-cos7 ~]# yum -y update\
[root@docker-cos7 ~]# yum -y install yum-utils device-mapper-persistent-data lvm2\
[root@docker-cos7 ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo\
[root@docker-cos7 ~]# yum -y install docker-ce docker-ce-cli containerd.io\
[root@docker-cos7 ~]# systemctl start docker\
[root@docker-cos7 ~]# systemctl enable docker

### Installation auf Ubuntu 18.04.3
[root@docker-ubuntu1804 ~]# apt-get -y remove docker docker-engine docker.io containerd runc\
[root@docker-ubuntu1804 ~]# apt-get -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common\
[root@docker-ubuntu1804 ~]# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -\
[root@docker-ubuntu1804 ~]# apt-key fingerprint 0EBFCD88\
[root@docker-ubuntu1804 ~]# add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable"\
[root@docker-ubuntu1804 ~]# apt-get update\
[root@docker-ubuntu1804 ~]# apt-get -y install docker-ce docker-ce-cli containerd.io\
[root@docker-ubuntu1804 ~]# systemctl start docker\
[root@docker-ubuntu1804 ~]# systemctl enable docker

### Wichtige Docker Befehle
| Befehle                                     | Beschreibung                                   |
|---------------------------------------------|------------------------------------------------|
| docker ps                                   | Zeigt alle laufenden Containers                |
| docker ps -a                                | Zeigt alle Containers (aktiv und inaktiv)      |
| docker pull <imagename>                     | Läd Images von DockerHub herrunter             |
| docker images                               | Zeigt alle lokalen Images                      |
| docker search                               | Sucht Images in der Registry (DockerHub)       |
| docker run                                  | Startet einen neuen Container                  |
| docker volume ls                            | Zeigt alle Docker Volumes an                   |
| docker network ls                           | Zeigt alle Docker Netzwerke an                 |
| docker rm <containername / container id>    | Löscht einen Container                         |
| docker rmi <imagename / image id>           | Löscht ein lokales Image                       |
| docker start <containername / container id> | Startet einen vorhandenen Container            |
| docker stop <containername / container id>  | Stoppt ein laufenden Container                 |
| docker exec <containername / container id>  | Verbindet sich mit der Console des Colntainers |

## rkt
rkt (Ausgesprochen rocket) wurde ursprünglich von CoreOS Inc. entwickelt. 

Im Jahr 2018 wurde CoreOS von RedHat übernommen und in Ihr eigenes Produktportfolie integriert. Das Betriebssytem CoreOS wurde mit Fedora zusammen getan und heisst nun Fedora CoreOS. 

Durch diesen Zusammenschluss vorde die Entwicklung von rkt eingestellt. 

rkt hatte die Eigenschaften, dass keine Runtime installiert werden musste, um einen rkt Container zu betreiben sondern die Container in eine Binärdatei gepackt wurde und so ausführbar waren. rkt stand in Konkurenz zu Docker. 

Da rkt eingestellt wurde, werde ich in diesem Artikel nicht weiter darauf eingehen, da jedoch rkt oft gehört wurde, fand ich die Erwähung in diesem Artiel wichtig.

## Podman (ohne Kubernetes)
Podman ist noch ein sehr junges Projekt, welches von RedHat initiert wurde. Verstion 1.0 wurde im Januar 2019 veröffentlicht. 

Podman ist vom aufbau und der Syntaxe sehr ähnlich zu Docker aufgebaut. 

Der grösste Unterschied zwischen Docker und Podman besteht darin, dass Podman nicht mit root Rechten ausgeführt werden muss, was sehr viel zur Sicherheit beiträgt. 

Zudem gibt es keinen Daemon, welche die Container von Podman steuert. Das starten und stopen von Podman Containern erfolgt entweder über die Commandozeile oder via systemd. 

Mit RedHat Enterprise Linux 8.0 wurde Docker durch Podman ersetzt und Docker wird nicht mehr offiziell von RedHat Enterprise Linux supported.

### Installation auf CentOS 7.7.1908
[root@podman-cos7 ~]# yum -y install podman

### Installation auf Ubuntu 18.04.3
[root@podman-ubuntu1804 ~]# apt-get -y update\
[root@podman-ubuntu1804 ~]# apt-get -y install software-properties-common\
[root@podman-ubuntu1804 ~]# add-apt-repository -y ppa:projectatomic/ppa\
[root@podman-ubuntu1804 ~]# apt-get -y update\
[root@podman-ubuntu1804 ~]# apt-get -y install podman

### Wichtige Podman Befehle
| Befehle                                     | Beschreibung                                   |
|---------------------------------------------|------------------------------------------------|
| podman info                                 | Zeigt Infos zu Podman                          |
| podman ps                                   | Zeigt alle laufenden Containers                |
| podman ps -a                                | Zeigt alle Containers (aktiv und inaktiv)      |
| podman pull <imagename>                     | Läd Images von DockerHub herrunter             |
| podman images                               | Zeigt alle lokalen Images                      |
| podman search                               | Sucht Images in der Registry (DockerHub)       |
| podman run                                  | Startet einen neuen Container                  |
| podman volume ls                            | Zeigt alle Podman Volumes an                   |
| podman network ls                           | Zeigt alle Podman Netzwerke an                 |
| podman rm <containername / container id>    | Löscht einen Container                         |
| podman rmi <imagename / image id>           | Löscht ein lokales Image                       |
| podman start <containername / container id> | Startet einen vorhandenen Container            |
| podman stop <containername / container id>  | Stoppt ein laufenden Container                 |
| podman exec <containername / container id>  | Verbindet sich mit der Console des Colntainers |

# Kubernetes
## Was ist Kubernetes?
| Kubernetes         |                                   |
|--------------------|-----------------------------------|
| Entwickler         | Google                            |
| Maintainer         | Cloud Native Computing Foundation |
| Erscheinungsjahr   | 2014                              |
| Aktuelle Verstion  | 1.16.2 (15. Oktober 2019)         |
| Lizenz             | Apache 2.0                        |
| Programmiersprache | Go                                |
| Betriebssystem     | Plattformunabhängigkeit           |
| Website            | https://kubernetes.io/            |

Kubernetes wurde ursprünglich von Google für die Orchestrierung der Container entwickelt. Es dient zur automatischen Bereitstellung, Skalierung und verwalten von Containern auf diversen Runtimes wie zum Beispiel Docker, Podman, Contaierd, cri-o etc. 

Kubernetes wurde an einem späteren Zeitpunkt an die CNCF (Cloud Native Computing Foundation) gespendet, welche Kubernetes nun weiterentwickelt, zusammen mit anderen Partnern wie Microsoft, IBM, RedHat. 

Kubernetes läuft immer in einem Cluster Verbund, in dem es mindestens einen Master Server (Kubernetes-Master) und mindestens 1 Node gibt. Die Kubernetes Mastes können redundant im Loadbalancing ausgelegt werden. Node Server können unendlich viele hinzugefügt werden. Die Container werden gleichmässig vom Master auf die einzelnen Notes verteilt.

## Systemarchitektur
1x Kubernetes Master Server\
1x Kubernetes Nodes Server

### Master
* CPU: 2
* Memory: 8GB
* Disk sda: 20GB
  * sda1  /boot  1GB
  *  sda2  swap   4GB
  *  sda3  /     15GB

**Name:** kubemaster01.domain.local\
**IP:** 192.168.5.10/24\
**GW:** 192.168.5.1

### Node
* CPU: 2
* Memory: 4GB
* Disk sda: 20GB
  * sda1  /boot  1GB
  * sda2  swap   4GB
  * sda3  /     15GB
* Disk sdb: 40GB

**Name:** kubenode01.domain.local\
**IP:** 192.168.5.11/24\
**GW:** 192.168.5.1

## Kubernetes Cluster auf CentOS 7.7.1908
ISO URL: http://pkg.adfinis-sygroup.ch/centos/7.7.1908/isos/x86_64/CentOS-7-x86_64-Minimal-1908.iso

### Installation Kubernetes Cluster mit Docker
Referenz: https://kubernetes.io/docs/setup/production-environment/container-runtimes/ \
Referenz: https://www.howtoforge.com/tutorial/centos-kubernetes-docker-cluster/

#### Kubernetes Master Server
[root@kubemaster01 ~]# vi /etc/hosts\
\---\
192.168.5.10      kubemaster01 kubemaster01.domain.local\
192.168.5.11      kubenode01 kubenode01.domain.local\
\---

[root@kubemaster01 ~]# setenforce 0\
[root@kubemaster01 ~]# sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux\
[root@kubemaster01 ~]# systemctl stop firewalld\
[root@kubemaster01 ~]# systemctl disable firewalld\
[root@kubemaster01 ~]# swapoff -a\
[root@kubemaster01 ~]# vi /etc/fstab\
\---\
#UUID=1d31b270-6f96-4404-b0c4-3d99e4efff5d swap                    swap    defaults        0 0\
\---

[root@kubemaster01 ~]# yum -y install yum-utils device-mapper-persistent-data lvm2\
[root@kubemaster01 ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo \
[root@kubemaster01 ~]# yum update\
[root@kubemaster01 ~]# yum -y install docker-ce-18.09.9\
[root@kubemaster01 ~]# mkdir /etc/docker\
[root@kubemaster01 ~]# cat > /etc/docker/daemon.json <<EOF\
\> {\
\>   "exec-opts": ["native.cgroupdriver=systemd"],\
\>   "log-driver": "json-file",\
\>   "log-opts": {\
\>     "max-size": "100m"\
\>   },\
\>   "storage-driver": "overlay2",\
\>   "storage-opts": [\
\>     "overlay2.override_kernel_check=true"\
\>   ]\
\> }\
\> EOF

[root@kubemaster01 ~]# mkdir -p /etc/systemd/system/docker.service.d\
[root@kubemaster01 ~]# systemctl daemon-reload\
[root@kubemaster01 ~]# systemctl restart docker\
[root@kubemaster01 ~]# systemctl enable docker\
[root@kubemaster01 ~]# cat <<EOF > /etc/yum.repos.d/kubernetes.repo\
\> [kubernetes]\
\> name=Kubernetes\
\> baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64\
\> enabled=1\
\> gpgcheck=1\
\> repo_gpgcheck=1\
\> gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg\
\>         https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg\
\> EOF

[root@kubemaster01 ~]# yum -y install kubelet kubeadm kubectl\
[root@kubemaster01 ~]# systemctl start kubelet\
[root@kubemaster01 ~]# systemctl enable kubelet\
[root@kubemaster01 ~]# kubeadm init --apiserver-advertise-address=192.168.5.10 --pod-network-cidr=10.244.0.0/16\
[root@kubemaster01 ~]# mkdir -p $HOME/.kube\
[root@kubemaster01 ~]# cp -i /etc/kubernetes/admin.conf $HOME/.kube/config\
[root@kubemaster01 ~]# chown $(id -u):$(id -g) $HOME/.kube/config\
[root@kubemaster01 ~]# cat $HOME/.kube/config\
[root@kubemaster01 ~]# kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

[root@kubemaster01 ~]# kubectl get nodes\
NAME           STATUS   ROLES    AGE     VERSION\
kubemaster01   Ready    master   2m17s   v1.16.3

#### Kubernetes Node Server
[root@kubenode01 ~]# vi /etc/hosts\
\---\
192.168.5.10      kubemaster01 kubemaster01.domain.local\
192.168.5.11      kubenode01 kubenode01.domain.local\
\---

[root@kubenode01 ~]# setenforce 0\
[root@kubenode01 ~]# sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux\
[root@kubenode01 ~]# systemctl stop firewalld\
[root@kubenode01 ~]# systemctl disable firewalld\
[root@kubenode01 ~]# swapoff -a\
[root@kubenode01 ~]# vi /etc/fstab\
\---\
#UUID=68655183-2809-4451-b428-6d8096164cab swap                    swap    defaults        0 0\
\---

[root@kubenode01 ~]# yum -y install yum-utils device-mapper-persistent-data lvm2\
[root@kubenode01 ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo\
[root@kubenode01 ~]# yum update\
[root@kubenode01 ~]# yum -y install docker-ce-18.09.9\
[root@kubenode01 ~]# mkdir /etc/docker\
[root@kubenode01 ~]# cat > /etc/docker/daemon.json <<EOF\
\> {\
\>   "exec-opts": ["native.cgroupdriver=systemd"],\
\>   "log-driver": "json-file",\
\>   "log-opts": {\
\>     "max-size": "100m"\
\>   },\
\>   "storage-driver": "overlay2",\
\>   "storage-opts": [\
\>     "overlay2.override_kernel_check=true"\
\>   ]\
\> }\
\> EOF

[root@kubenode01 ~]# mkdir -p /etc/systemd/system/docker.service.d\
[root@kubenode01 ~]# systemctl daemon-reload\
[root@kubenode01 ~]# systemctl restart docker\
[root@kubenode01 ~]# systemctl enable docker\
[root@kubenode01 ~]# cat <<EOF > /etc/yum.repos.d/kubernetes.repo\
\> [kubernetes]\
\> name=Kubernetes\
\> baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64\
\> enabled=1\
\> gpgcheck=1\
\> repo_gpgcheck=1\
\> gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg\
\>         https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg\
\> EOF

[root@kubenode01 ~]# yum -y install kubelet kubeadm kubectl\
[root@kubenode01 ~]# systemctl start kubelet\
[root@kubenode01 ~]# systemctl enable kubelet\
[root@kubenode01 ~]# kubeadm join 192.168.5.10:6443 --token k1exfd.pqtcxyiidbv1t2e4 --discovery-token-ca-cert-hash sha256:31252c5deaec5c91f5d924b1d223db3afa9cbb379d93fe7bdb509ddfb0aa6e3d

#### Check installation
[root@kubemaster01 ~]# kubectl get nodes\
NAME             STATUS     ROLES    AGE   VERSION\
kubemaster01     Ready      master   29m   v1.16.3\
kubenode01       Ready      <none>   10s   v1.16.3

#### Nginx Instanz in Kubernetes Cluster (Docker) betreiben
[root@kubemaster01 ~]# kubectl create deployment nginx --image=nginx\
[root@kubemaster01 ~]# kubectl describe deployment nginx\
[root@kubemaster01 ~]# kubectl create service nodeport nginx --tcp=80:80\
[root@kubemaster01 ~]# kubectl get pods

[root@kubemaster01 ~]# kubectl get svc\
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE\
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        32m\
nginx        NodePort    10.108.101.154   <none>        80:30875/TCP   13s

In einem Webbrowser kann nun die URL http://192.168.5.11:30875 eingegeben werden, danach sollte die Standart Seite von Nginx kommen.

### Installation Kubernetes Cluster mit CRI-O
Referenz: https://kubevirt.io/2019/KubeVirt_k8s_crio_from_scratch.html

#### Kubernetes Master Server
[root@kubemaster01 ~]# vi /etc/hosts\
\---\
192.168.5.10      kubemaster01 kubemaster01.domain.local\
192.168.5.11      kubenode01 kubenode01.domain.local\
\---

[root@kubemaster01 ~]# yum -y install epel-release\
[root@kubemaster01 ~]# yum -y update\
[root@kubemaster01 ~]# yum -y install vim jq git\
[root@kubemaster01 ~]# cat > /etc/sysctl.d/99-kubernetes-cri.conf <<EOF\
\> net.bridge.bridge-nf-call-iptables  = 1\
\> net.ipv4.ip_forward                 = 1\
\> net.bridge.bridge-nf-call-ip6tables = 1\
\> EOF

[root@kubemaster01 ~]# modprobe br_netfilter\
[root@kubemaster01 ~]# echo br_netfilter > /etc/modules-load.d/br_netfilter.conf\
[root@kubemaster01 ~]# modprobe overlay\
[root@kubemaster01 ~]# echo overlay > /etc/modules-load.d/overlay.conf\
[root@kubemaster01 ~]# sysctl -p/etc/sysctl.d/99-kubernetes-cri.conf\
[root@kubemaster01 ~]# setenforce 0\
[root@kubemaster01 ~]# sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config\
[root@kubemaster01 ~]# yum -y install ansible\
[root@kubemaster01 ~]# git clone https://github.com/ptrnull/cri-o-ansible\
[root@kubemaster01 ~]# cd cri-o-ansible\
[root@kubemaster01 cri-o-ansible]# git checkout fixes_k8s_1_16\
[root@kubemaster01 cri-o-ansible]# echo 192.168.122.71 > hosts\
[root@kubemaster01 cri-o-ansible]# ansible-playbook cri-o.yml -i hosts\
[root@kubemaster01 ~]# vim /etc/crio/crio.conf\
\---\
#network_dir = "/etc/cni/net.d/"\
network_dir = "/etc/crio/net.d/"\
\---

[root@kubemaster01 ~]# mkdir /etc/crio/net.d\
[root@kubemaster01 ~]# systemctl daemon-reload\
[root@kubemaster01 ~]# cd /etc/crio/net.d/\
[root@kubemaster01 net.d]# wget https://raw.githubusercontent.com/cri-o/cri-o/master/contrib/cni/10-crio-bridge.conf\
[root@kubemaster01 net.d]# sed -i 's/10.88.0.0/10.244.0.0/g' 10-crio-bridge.conf\
[root@kubemaster01 ~]# systemctl enable crio\
[root@kubemaster01 ~]# systemctl restart crio\
[root@kubemaster01 ~]# cat <<EOF > /etc/yum.repos.d/kubernetes.repo\
\> [kubernetes]\
\> name=Kubernetes\
\> baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64\
\> enabled=1\
\> gpgcheck=1\
\> repo_gpgcheck=1\
\> gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg\
\>         https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg\
\> EOF

[root@kubemaster01 ~]# yum -y install kubelet kubeadm kubectl\
[root@kubemaster01 ~]# systemctl restart kubelet\
[root@kubemaster01 ~]# systemctl enable kubelet\
[root@kubemaster01 ~]# kubeadm init --pod-network-cidr=10.244.0.0/16\
[root@kubemaster01 ~]# wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml\
[root@kubemaster01 ~]# kubectl apply -f kube-flannel.yml\
[root@kubemaster01 ~]# git clone https://github.com/intel/multus-cni /root/src/github.com/multus-cni\
[root@kubemaster01 ~]# cd /root/src/github.com/multus-cni\
[root@kubemaster01 multus-cni]# git checkout origin/kube-1.16-change\
[root@kubemaster01 multus-cni]# cd images\
[root@kubemaster01 images]# kubectl create -f multus-daemonset-crio.yml\
[root@kubemaster01 ~]# kubectl taint nodes k8s-test node-role.kubernetes.io/master:NoSchedule-\
[root@kubemaster01 ~]# export KUBEVIRT_VERSION=\$(curl -s https://api.github.com/repos/kubevirt/kubevirt/releases/latest | jq -r .tag_name)\
[root@kubemaster01 ~]# kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/\${KUBEVIRT_VERSION}/kubevirt-operator.yaml \
[root@kubemaster01 ~]# kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/\${KUBEVIRT_VERSION}/kubevirt-cr.yaml \
[root@kubemaster01 ~]# kubectl create configmap kubevirt-config -n kubevirt --from-literal debug.useEmulation=true< \
wget -O virtctl https://github.com/kubevirt/kubevirt/releases/download/\$\{KUBEVIRT_VERSION\}/virtctl-\${KUBEVIRT_VERSION\}-linux-amd64 \
chmod +x virtctl\
[root@cos7 ~]# \(\
\>   set -x; cd "\$(mktemp -d)" \&\&\
\>   curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/download/v0.3.1/krew.{tar.gz,yaml}" \&\&\
\>   tar zxvf krew.tar.gz \&\&\
\>   ./krew-"\$(uname | tr '[:upper:]' '[:lower:]')_amd64" install \\\
\>     --manifest=krew.yaml --archive=krew.tar.gz\
\> )

[root@kubemaster01 ~]# vim .bashrc\
\---\
export PATH="\${KREW_ROOT:-\$HOME/.krew}/bin:\$PATH"\
\---

[root@kubemaster01 ~]# source .bashrc\
[root@kubemaster01 ~]# kubectl krew install virt\
[root@kubemaster01 ~]# kubectl apply -f https://raw.githubusercontent.com/kubevirt/kubevirt.github.io/master/labs/manifests/vm.yaml \
[root@kubemaster01 ~]# kubectl get vmis\
NAME     AGE   PHASE     IP            NODENAME\
testvm   23s   Running   10.244.0.18   kubemaster01
## Begriffserklärung

| Begriffserklärungen               |                                                                                                                                                        |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cloud Native Computing Foundation | Foundation, welche mit Kubernetes 1.0 gegründet wurde und viele grosse Hersteller wie z.B. RedHat, IBM, Intel, Google, VMware etc. Mitglieder sind.    |
| Cluster                           | Verbund von einem oder mehreren Kubernetes Master Server (Loadbalancing) und mehreren Kubernetes Node Servern                                          |
| Container                         | Lauffähige Instanz eines Container Images                                                                                                              |
| Go                                | Programmiersprache, welche von Google entwickelt wurde. Weit verbreitet bei skalierbare Netzwerkdienste, Cluster- und Cloud Computing                  |
| Images                            | Schreibgeschütztes Template zur Erstellung von Containern                                                                                              |
| Loadbalancing                     | Aufteilung des Loads (Netzwerktraffic, Systemressourcen,...) auf mehrere Server                                                                        |
| Master Server                     | Der Master Server koordiniert den Cluster                                                                                                              |
| Node Server                       | Die Node Server sind die Arbeiter, welche die Anwendungen / Pods ausführen.                                                                            |
| Pods                              | Die kleinste Einheit ist ein Pod, der eine Gruppe von Containern mit geteilten Ressourcen sein kann.                                                   |
| Registry                          | Zentraler Speicherort für Container Images. Meist in Verwendung mit Git. Bekannteste Registry ist aktuell DockerHub                                    |
| Stateless Application             | Container, welche keine Daten speichert (z.B. Webfrontend)                                                                                             |
| Statefull Application             | Container, welche veränderbare Daten beinhalten, welche gespeichert werden muss (z.B. Datebank)                                                        |
| Volume                            | In einem Volume werden persistente Daten gespeichert (z.B. Datenbank)                                                                                  |
| YAML / YML                        | YAML ist eine Auszeichnugnssprache. Sie wird in zusammenhang mit Container oft für Image Konfigurationen etc. oder Container Konfigurationen verwendet |
