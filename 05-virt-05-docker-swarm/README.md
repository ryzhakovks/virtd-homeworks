## Домашнее задание к занятию "5.5. Оркестрация кластером Docker контейнеров на примере Docker Swarm"
### Задача 1 
В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?
```
В режиме replicated сервис запускается в стольких экземплярах в скольких задано. В global на всех нодах(Пример exporter) 
```

Какой алгоритм выбора лидера используется в Docker Swarm кластере?
```
Лидер выбирается командой:
docker swarm init
Механизм выбора лидера основан на алгоритме raft, который включает в себя 3 принципа выбора лидера:
1)Кандидат получает большинство голосов (включая свой) и побеждает в выборах.
2)Кандидат получает сообщение от уже действующего лидера текущего срока или от любого сервера более старшего срока.
3)Кандидат не получает за некоторый таймаут большинство голосов
```

Что такое Overlay Network?
```
Сеть которая создается для связи с контейнерами запущенными на хостах.
```

### Задача 2 
```
root@node3:~# docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
sazy7ur1g5lfxl79tibl2h4ix     node1      Ready     Active                          20.10.11
r5jykvx50ojnx2n73q1bz944x     node2      Ready     Active                          20.10.11
qtqn9egqtoctc9oeni7jwjh6e *   node3      Ready     Active         Leader           20.10.11

root@node2:~# docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
m9tkm9rh5brhozz7iel9397ps     node1      Down      Active                          20.10.11
oq7x6mis99rdalbnmpt7m7ihc     node1      Ready     Active         Reachable        20.10.11
l8rfyrpl4nla8zrziphfzp4c8     node2      Down      Active                          20.10.11
xyeyfh5ol5sd5lyqiyk669g5b *   node2      Ready     Active         Reachable        20.10.11
2gsh75nrj5h2u0fd8chapfb5w     node3      Ready     Active         Leader           20.10.11

```
### Задача 3 
```
[centos@node01 ~]$ sudo docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
lwvlgem0bxyszr9drp4kvd22g *   node01.netology.yc   Ready     Active         Leader           20.10.11
1b0m3qloy8675xioyg3ma7qwm     node02.netology.yc   Ready     Active         Reachable        20.10.11
1oolu89qa8kblq64b380iuu3a     node03.netology.yc   Ready     Active         Reachable        20.10.11
is8fxbwb80dtsx0cbcsgmqi5o     node04.netology.yc   Ready     Active                          20.10.11
6unjfax1zg807mq8npqaul7v0     node05.netology.yc   Ready     Active                          20.10.11
r3sxrfs5nfmnk02fqabuwsvz8     node06.netology.yc   Ready     Active                          20.10.11

[centos@node01 ~]$ sudo docker service ls
ID             NAME                                MODE         REPLICAS   IMAGE                                          PORTS
t3z4pa0utu3f   swarm_monitoring_alertmanager       replicated   1/1        stefanprodan/swarmprom-alertmanager:v0.14.0    
0qdj7or18ad6   swarm_monitoring_caddy              replicated   1/1        stefanprodan/caddy:latest                      *:3000->3000/tcp, *:9090->9090/tcp, *:9093-9094->9093-9094/tcp
vemu68aju2ub   swarm_monitoring_cadvisor           global       6/6        google/cadvisor:latest                         
cbvx3wtxjzd8   swarm_monitoring_dockerd-exporter   global       6/6        stefanprodan/caddy:latest                      
st5xl7j59oip   swarm_monitoring_grafana            replicated   1/1        stefanprodan/swarmprom-grafana:5.3.4           
a8r2vgpuisbd   swarm_monitoring_node-exporter      global       6/6        stefanprodan/swarmprom-node-exporter:v0.16.0   
6r5iw49fovbu   swarm_monitoring_prometheus         replicated   1/1        stefanprodan/swarmprom-prometheus:v2.5.0       
jmqpbr86gl95   swarm_monitoring_unsee              replicated   1/1        cloudflare/unsee:v0.8.0  

```
![снимок](Снимок1111.JPG)



### Задача 4
Команда:
```
docker swarm update --autolock=true
```
Ограничивает доступ к конфигурации docker (read/write) требуя ввода ключа для рассшифровки журналов конфигруации RAFT.
Есть возможность ограничить доступ как одноразово (docker swarm init --autolock) так и постоянно (docker swarm init --autolock=true).
Для разблокировки требуется ввести docker swarm unlock и ключ расшифровки журналов RAFT
docker swarm unlock-key - показать ключ
 docker swarm unlock-key --rotate - обнолвление времени жизни ключа

