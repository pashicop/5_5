# 5_5
Задача 1
Дайте письменые ответы на следующие вопросы:

В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?
global - на все 
replication - на некоторые

Какой алгоритм выбора лидера используется в Docker Swarm кластере?


Что такое Overlay Network?


Задача 2
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
Задача 3
Создать ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker service ls
Задача 4 (*)
Выполнить на лидере Docker Swarm кластера команду (указанную ниже) и дать письменное описание её функционала, что она делает и зачем она нужна:

# см.документацию: https://docs.docker.com/engine/swarm/swarm_manager_locking/
docker swarm update --autolock=true
