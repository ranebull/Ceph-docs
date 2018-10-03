## Мониторинг и построение графиков при ребалансе данных c использовании кластерной и публичной сетей##

### Цель: анализ трафика, проходящего через публичную и кластерную сеть при ребалансе данных###  

### Исходные данные ###

**Для проведения данного теста в кластер буден введен еще один хост h204 с OSD объемом 100Gb, после чего начнется ребаланс данных, процессы которого будут анализироваться.**

**Виртуальные машины:**

	h201: vda(50Gb)-система vdb(100Gb)-OSD
		  eth0:10.0.2.201/24(публичная) eth1:10.0.3.201/24(кластерная)
	h202: vda(50Gb)-система vdb(100Gb)-OSD
		  eth0:10.0.2.202/24(публичная) eth1:10.0.3.202/24(кластерная)
	h203: vda(50Gb)-система vdb(100Gb)-OSD
		  eth0:10.0.2.203/24(публичная) eth1:10.0.3.203/24(кластерная)
	h204: vda(50Gb)-система vdb(100Gb)-OSD
		  eth0:10.0.2.204/24(публичная) eth1:10.0.3.204/24(кластерная)

**Конфигурационный файл сeph.conf**
	
	[global]
	fsid = 5231a9c6-2a1f-4c49-bb1f-4946f35fd408
	public_network = 10.0.2.0/24
	cluster_network = 10.0.3.0/24
	mon_initial_members = h201, h202, h203
	mon_host = 10.0.2.201,10.0.2.202,10.0.2.203
	auth_cluster_required = cephx
	auth_service_required = cephx
	auth_client_required = cephx

Начало процесса :   18:00
Окончание процесса: 19:35

**Графики теста:**

	test-rebalance-clu-graph.pdf

### Итог: ###

На графиках мы наглядно видим, что процесс ребаланса происходит полностью по кластерной сети eth1:10.0.3.0/24. По поведению трафика в кластерной сети мы видим, как данные с разных OSD частями реплицируются на хост h204. В публичной сети eth0:10.0.2.0/24 трафик практически отсутствует.   