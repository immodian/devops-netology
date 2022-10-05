# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

1. Есть скрипт:
	```bash
	a=1
	b=2
	c=a+b
	d=$a+$b
	e=$(($a+$b))
	```
	* Какие значения переменным c,d,e будут присвоены?
	* Почему?
	
	```bash
	root@vagrant:/home/vagrant# a=1
	root@vagrant:/home/vagrant# b=2
	root@vagrant:/home/vagrant# c=a+b
	root@vagrant:/home/vagrant# d=$a+$b
	root@vagrant:/home/vagrant# e=$(($a+$b))
	root@vagrant:/home/vagrant# echo $c
	a+b
	root@vagrant:/home/vagrant# echo $d
	1+2
	root@vagrant:/home/vagrant# echo $e
	3
	```

c - присвоен вывод 3-х символов a, +, b
d - присвоено значение двух переменных и знака +
e - так как обёрнуто в функцию $(()), то выполнится арифметическое действие

2. На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным. В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
	```bash
	while ((1==1)
	 do
	 curl https://localhost:4757
	 if (($? != 0))
	  then
	  date >> curl.logs
	 fi
	done
	```
	
	Решение:
	
	```bash
	#!/usr/bin/env bash
	while ((1==1)) # ошибка со скобкой
	 do
	 curl https://localhost:4757
	 if (($? != 0))
	  then
	  date >> curl.log
	  else break # выход из цикла
	 fi
	sleep 1 # уменьшение частоты опроса
	done
	```


3. Необходимо написать скрипт, который проверяет доступность трёх IP: 192.168.0.1, 173.194.222.113, 87.250.250.242 по 80 порту и записывает результат в файл log. Проверять доступность необходимо пять раз для каждого узла.

	```bash
	root@vagrant:/home/vagrant# cat dz2.sh
	#!/usr/bin/env bash
	hosts=(192.168.0.1 173.194.222.113 87.250.250.242)
	for i in {1..5}
	do
	        echo $i >> hostreach.log
	        for host in ${hosts[@]}
	        do
	                curl ${host}:80 --connect-timeout 5
	                if (($? != 0))
	                then
	                        echo "${host}:80 недоступен" >> hostreach.log
	                else
	                        echo "${host}:80 доступен" >> hostreach.log
	                fi
	        done
	done
	root@vagrant:/home/vagrant# cat hostreach.log
	1
	192.168.0.1:80 недоступен
	173.194.222.113:80 доступен
	87.250.250.242:80 доступен
	2
	192.168.0.1:80 недоступен
	173.194.222.113:80 доступен
	87.250.250.242:80 доступен
	3
	192.168.0.1:80 недоступен
	173.194.222.113:80 доступен
	87.250.250.242:80 доступен
	4
	192.168.0.1:80 недоступен
	173.194.222.113:80 доступен
	87.250.250.242:80 доступен
	5
	192.168.0.1:80 недоступен
	173.194.222.113:80 доступен
	87.250.250.242:80 доступен
	```

4. Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается

	```bash
	root@vagrant:/home/vagrant# cat dz3.sh
	#!/usr/bin/env bash
	hosts=(192.168.0.1 173.194.222.113 87.250.250.242)
	while ((1==1))
	do
	 for host in ${hosts[@]}
	 do
	        curl ${host}:80 --connect-timeout 5
	        if (($? != 0))
	        then
	                echo "${host}:80 недоступен" >> error
	        else
	                exit
	        fi
	 done
	done
	root@vagrant:/home/vagrant# cat error
	192.168.0.1:80 недоступен
	```