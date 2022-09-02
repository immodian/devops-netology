# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1. Какой системный вызов делает команда `cd`? В прошлом ДЗ мы выяснили, что `cd` не является самостоятельной  программой, это `shell builtin`, поэтому запустить `strace` непосредственно на `cd` не получится. Тем не менее, вы можете запустить `strace` на `/bin/bash -c 'cd /tmp'`. В этом случае вы увидите полный список системных вызовов, которые делает сам `bash` при старте. Вам нужно найти тот единственный, который относится именно к `cd`.

	`chdir("/tmp")`

1. Попробуйте использовать команду `file` на объекты разных типов на файловой системе. Например:
    ```bash
    vagrant@netology1:~$ file /dev/tty
    /dev/tty: character special (5/0)
    vagrant@netology1:~$ file /dev/sda
    /dev/sda: block special (8/0)
    vagrant@netology1:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB shared object, x86-64
    ```
    Используя `strace` выясните, где находится база данных `file` на основании которой она делает свои догадки.
		
	`/usr/share/misc/magic.mgc`
	
1. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

	```bash
	root@vagrant:~# vi vifile
	[1]+  Stopped                 vi vifile
	root@vagrant:~# bg
	[1]+ vi vifile &
	root@vagrant:~# rm .vifile.swp
	root@vagrant:~# lsof | grep vifile
	vi        5873                           root    3u      REG              253,0     4096   	 	1179657 /root/.vifile.swp (deleted)
	root@vagrant:~# echo "" > /proc/5873/fd/3
	root@vagrant:~# lsof | grep vifile
	vi        5873                           root    3u      REG              253,0        1    1179657 /root/.vifile.swp (deleted)
	```

1. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

	Зомби-процессы не занимают памяти (как процессы-сироты), но блокируют записи в таблице процессов, размер 		которой ограничен для каждого пользователя и системы в целом.

1. В iovisor BCC есть утилита `opensnoop`:
    ```bash
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    ```
    На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные [сведения по установке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).
	
	```
	vagrant@vagrant:~$ sudo opensnoop-bpfcc
	PID    COMM               FD ERR PATH
	649    irqbalance          6   0 /proc/interrupts
	649    irqbalance          6   0 /proc/stat
	649    irqbalance          6   0 /proc/irq/20/smp_affinity
	649    irqbalance          6   0 /proc/irq/0/smp_affinity
	649    irqbalance          6   0 /proc/irq/1/smp_affinity
	649    irqbalance          6   0 /proc/irq/8/smp_affinity
	649    irqbalance          6   0 /proc/irq/12/smp_affinity
	649    irqbalance          6   0 /proc/irq/14/smp_affinity
	649    irqbalance          6   0 /proc/irq/15/smp_affinity
	1      systemd            12   0 /proc/654/cgroup
	915    vminfo              5   0 /var/run/utmp
	641    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
	641    dbus-daemon        22   0 /usr/share/dbus-1/system-services
	641    dbus-daemon        -1   2 /lib/dbus-1/system-services
	641    dbus-daemon        22   0 /var/lib/snapd/dbus-1/system-services/
	915    vminfo              5   0 /var/run/utmp
	641    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
	641    dbus-daemon        22   0 /usr/share/dbus-1/system-services
	641    dbus-daemon        -1   2 /lib/dbus-1/system-services
	641    dbus-daemon        22   0 /var/lib/snapd/dbus-1/system-services/
	649    irqbalance          6   0 /proc/interrupts
	649    irqbalance          6   0 /proc/stat
	649    irqbalance          6   0 /proc/irq/20/smp_affinity
	649    irqbalance          6   0 /proc/irq/0/smp_affinity
	649    irqbalance          6   0 /proc/irq/1/smp_affinity
	649    irqbalance          6   0 /proc/irq/8/smp_affinity
	649    irqbalance          6   0 /proc/irq/12/smp_affinity
	649    irqbalance          6   0 /proc/irq/14/smp_affinity
	649    irqbalance          6   0 /proc/irq/15/smp_affinity
	915    vminfo              5   0 /var/run/utmp
	641    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
	641    dbus-daemon        22   0 /usr/share/dbus-1/system-services
	641    dbus-daemon        -1   2 /lib/dbus-1/system-services
	641    dbus-daemon        22   0 /var/lib/snapd/dbus-1/system-services/
	```
	
1. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.

	Системный вызов:
	
	```bash
	vagrant@vagrant:~$ strace uname -a
	...
	uname({sysname="Linux", nodename="vagrant", ...}) = 0
	```
	
	Цитата из man:
	
	```bash
  	Part of the utsname information is also accessible via
       /proc/sys/kernel/{ostype, hostname, osrelease, version,
       domainname}.
	```

1. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
    Есть ли смысл использовать в bash `&&`, если применить `set -e`?
	
	При использовании `&&` вместо `;`  следующая команда выполнится только после успешного выполнения предидущей. Смысла использовать `&&`, если применить `set -e` нет, так как `&&` не продолжит выполнение команд при ненулевом результате.
	
1. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?

	Опции:
	```bash
	-e  Exit immediately if a command exits with a non-zero status.
	-u  Treat unset variables as an error when substituting.
	-x  Print commands and their arguments as they are executed.
	-o option-name
	pipefail     the return value of a pipeline is the status of
                           the last command to exit with a non-zero status,
                           or zero if no command exited with a non-zero status
	```

	Конструкция `set -euxo pipefail` будучи добавленной в большой сценарий `bash` завершит работу, если часть скрипта не отработает, проверит инициализацию переменных в скрипте, напечатает все команды перед выполнением, укажет, все ли команды в пайпах завершились успешно - т.е хорошая команда для отладки.

1. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

	```bash
	vagrant@vagrant:~$ ps ax -o stat | sort | uniq -c
      1 D+
      8 I
     39 I<
      1 R+
     26 S
      1 S+
      6 S<
      1 Sl
      1 SLsl
      2 SN
      1 S<s
     15 Ss
      1 Ss+
      7 Ssl
      1 STAT
	```

	Наиболее часто встречается `I Idle kernel thread` и `S interruptible sleep`.
Так же пристутсвуют `R running or runnable`, `< high-priority`, `D uninterruptible sleep`, `+ is in the foreground process group`, `N low-priority`.

