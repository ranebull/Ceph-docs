
#  Создание/изменение  структуры Ceph кластера.

#### ( Данный способ является более безопасным, чем выгрузка CRUSHMAP прямое его редактирование с последующей загрузкой.)


## Команды  управления Бакетами(bucket)/узлами структуры кластера Ceph


**Вывод структуры кластера Ceph**

	# Вывод структуры в виде XML
	ceph osd crush dump

	# Выводс труктуры в виде дерева 
	osd crush tree	[--show-shadow]												

	# Выгрузка и декомпиляция CURSHMAP (для более детального анализа)
	ceph osd getcrushmap -o crush.bin && crushtool -d crush.bin -o crush.src		

**Создание Бакета/узла**

	# Допустимые типы узлов по порядку их иерархии:
	#
	#  root 	- Коренной уровень (самый верхний узел структуры)
	#  region	- Регион
	#  datacenter	- Датацентр
	#  room		- Помещение
	#  pod		- ПОД
	#  pdu		- ПДУ
	#  row		- Ряд
	#  rack		- Стелаж
	#  chassis	- Шасси
	#  host		- Хост
	#  osd		- OSD
	#
	# <Name> - Наименование узла
	# <Type> - Тип узла
	# 
	ceph osd crush add-bucket <Name> <Type>

**Удаление Бакета/узла**

	# <Name> - Имя удаляемого узла
	# <Backet> - Наименование вышестоящего узла (не обязятелен при полном удалении узла)
	#
	ceph osd crush remove <Name> [<Backet>]

**Присоединение  одного узла структуры к другому**
 
	# <Src Name> - Имя присоединяемого узла
	# <Dst Type> - Тип узла к которому присоединяется исходный узел
	# <Dst Name> - Наименование узла к которому присоединяется исходный узел
	#
	ceph osd crush link <Src Name> <Dst Type>=<Dst Name>

**Отсоединение  одного узла структуры от другого**

	# <Name> - Имя отсоединяемого узла
	# <Backet> - Наименование узла от которого отсоединяется узел
	#
	ceph osd crush unlink <Name> <Backet>

**Перемещение узла по структуре**

	# <Src Name> - Имя перемещаемого узла
	# <Dst Type> - Тип узла куда идет перемещение
	# <Dst Name> - Наименование узла к которому перемещается исходный узел
	#
	ceph osd crush move <Src Name> <Dst Type>=<Dst Name>


####  Изменение уровня точек отказа делается с помощью Rules. Методы оперирования с Rules описано в rules.md
