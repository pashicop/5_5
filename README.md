# 5_5
## Задача 1
Дайте письменые ответы на следующие вопросы:

В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?  
global - запускает сервис в единственном экземпляре на всех нодах
replication - запускает указанное количество реплик сервиса на нодах

Какой алгоритм выбора лидера используется в Docker Swarm кластере?   
Используется raft consensus алгоритм, при котором требуется (n/2 + 1) голосов для выбора лидера 


Что такое Overlay Network?  
Сеть поверх используемой докер хостом сети. Используется для взаимодействия между несколькими докер хостами. 


## Задача 2
Создать ваш первый Docker Swarm кластер в Яндекс.Облаке

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker node ls

```
pashi@pashi-docker:~/5_5/src/terraform$ ssh centos@51.250.93.246
[centos@node01 ~]$ sudo -i
[root@node01 ~]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
udu2ucrvcgapq50snd6lezewl *   node01.netology.yc   Ready     Active         Reachable        20.10.17
fx8tfqdv8m3s699wwqil7zkmo     node02.netology.yc   Ready     Active         Reachable        20.10.17
zp3lmvi75wbxkfawta0qsuobe     node03.netology.yc   Ready     Active         Leader           20.10.17
x2pm0dilzwp11tdq4fi16k3mc     node04.netology.yc   Ready     Active                          20.10.17
e0wp7dd1dcivkohc8awyljd3r     node05.netology.yc   Ready     Active                          20.10.17
od5lzoey752ojjztfshmd8qx3     node06.netology.yc   Ready     Active                          20.10.17
[root@node01 ~]# 

```
![Screenshot from 2022-07-18 16-20-10](https://user-images.githubusercontent.com/97126500/179524662-ead20fb2-daad-4cd7-a4d8-cdf7deead736.png)


## Задача 3
Создать ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker service ls
```
pashi@pashi-docker:~/5_5/src/terraform$ ssh centos@51.250.88.52
[centos@node03 ~]$ sudo -i
[root@node03 ~]# docker service ls
ID             NAME                                MODE         REPLICAS   IMAGE                                          PORTS
8mobbnq2pbzq   swarm_monitoring_alertmanager       replicated   1/1        stefanprodan/swarmprom-alertmanager:v0.14.0    
9kg7gqwfyr0k   swarm_monitoring_caddy              replicated   1/1        stefanprodan/caddy:latest                      *:3000->3000/tcp, *:9090->9090/tcp, *:9093-9094->9093-9094/tcp
temynm03zs7u   swarm_monitoring_cadvisor           global       6/6        google/cadvisor:latest                         
tkwsv6ugemkf   swarm_monitoring_dockerd-exporter   global       6/6        stefanprodan/caddy:latest                      
7tw5zpd4rg8a   swarm_monitoring_grafana            replicated   1/1        stefanprodan/swarmprom-grafana:5.3.4           
vxrxis2f7v2i   swarm_monitoring_node-exporter      global       6/6        stefanprodan/swarmprom-node-exporter:v0.16.0   
n6gwrgyimngu   swarm_monitoring_prometheus         replicated   1/1        stefanprodan/swarmprom-prometheus:v2.5.0       
llckrstwvgsx   swarm_monitoring_unsee              replicated   1/1        cloudflare/unsee:v0.8.0                        
[root@node03 ~]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
udu2ucrvcgapq50snd6lezewl     node01.netology.yc   Ready     Active         Reachable        20.10.17
fx8tfqdv8m3s699wwqil7zkmo     node02.netology.yc   Ready     Active         Reachable        20.10.17
zp3lmvi75wbxkfawta0qsuobe *   node03.netology.yc   Ready     Active         Leader           20.10.17
x2pm0dilzwp11tdq4fi16k3mc     node04.netology.yc   Ready     Active                          20.10.17
e0wp7dd1dcivkohc8awyljd3r     node05.netology.yc   Ready     Active                          20.10.17
od5lzoey752ojjztfshmd8qx3     node06.netology.yc   Ready     Active                          20.10.17
[root@node03 ~]# reboot
Connection to 51.250.88.52 closed by remote host.
Connection to 51.250.88.52 closed.
pashi@pashi-docker:~/5_5/src/terraform$ 
```

![Screenshot from 2022-07-18 16-31-08](https://user-images.githubusercontent.com/97126500/179524828-b08006ac-af8a-4be4-8d2b-fe10f87fcc63.png)

## Задача 4 (*)
Выполнить на лидере Docker Swarm кластера команду (указанную ниже) и дать письменное описание её функционала, что она делает и зачем она нужна:

см.документацию: https://docs.docker.com/engine/swarm/swarm_manager_locking/
docker swarm update --autolock=true

Применил на лидере autolock, сохранил ключ, перезагрузил одну из Нод, и она автоматически не подключилась к докер сворму. В 'docker node ls' осталась  Unreachable. Пришлось заходить на неё по ssh и запускать 'docker swarm unlock'  и вводить ключ. После этого она стала работать и видна в докер сворме.
```
pashi@pashi-docker:~/5_5/src/terraform$ ssh centos@51.250.93.246
[centos@node01 ~]$ sudo -i
[root@node01 ~]# docker node ls
Error response from daemon: Swarm is encrypted and needs to be unlocked before it can be used. Please use "docker swarm unlock" to unlock it.
[root@node01 ~]# docker swarm unlock 
Please enter unlock key: 
[root@node01 ~]# docker swarm unlock 
Error: swarm is not locked
[root@node01 ~]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
udu2ucrvcgapq50snd6lezewl *   node01.netology.yc   Ready     Active         Reachable        20.10.17
fx8tfqdv8m3s699wwqil7zkmo     node02.netology.yc   Ready     Active         Leader           20.10.17
zp3lmvi75wbxkfawta0qsuobe     node03.netology.yc   Ready     Active         Reachable        20.10.17
x2pm0dilzwp11tdq4fi16k3mc     node04.netology.yc   Ready     Active                          20.10.17
e0wp7dd1dcivkohc8awyljd3r     node05.netology.yc   Ready     Active                          20.10.17
od5lzoey752ojjztfshmd8qx3     node06.netology.yc   Ready     Active                          20.10.17
[root@node01 ~]# service docker restart
Redirecting to /bin/systemctl restart docker.service
[root@node01 ~]# docker node ls
Error response from daemon: Swarm is encrypted and needs to be unlocked before it can be used. Please use "docker swarm unlock" to unlock it.
[root@node01 ~]# docker swarm unlock 
Please enter unlock key: 
[root@node01 ~]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
udu2ucrvcgapq50snd6lezewl *   node01.netology.yc   Ready     Active         Reachable        20.10.17
fx8tfqdv8m3s699wwqil7zkmo     node02.netology.yc   Ready     Active         Leader           20.10.17
zp3lmvi75wbxkfawta0qsuobe     node03.netology.yc   Ready     Active         Reachable        20.10.17
x2pm0dilzwp11tdq4fi16k3mc     node04.netology.yc   Ready     Active                          20.10.17
e0wp7dd1dcivkohc8awyljd3r     node05.netology.yc   Ready     Active                          20.10.17
od5lzoey752ojjztfshmd8qx3     node06.netology.yc   Ready     Active                          20.10.17
[root@node01 ~]# 
```
Нужно для защиты конфиденциальных данных Docker secrets, в которых можно хранить SSH-ключи, имя БД и логины/пароли к ней и тд.
