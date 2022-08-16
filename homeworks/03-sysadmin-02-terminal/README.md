## 1. Какого типа команда `cd`?
Команда `cd` - встроенная в bash команда. 
## 2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l?
Альтернатива данной конструкции - использовать ключ `-с`:
`grep <some_string> <some_file> -c`
## 3. Какой процесс с `PID 1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
Процесс systemd.
```
vagrant@vagrant:~# pstree -p
systemd(1)─┬─ModemManager(706)─┬─{ModemManager}(714)
...
```
## 4. Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?
```
ls > /dev/pts/1
```
## 5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл?
```
vagrant@vagrant:~# echo "hello" > file
vagrant@vagrant:~# cat < file > outfile
vagrant@vagrant:~# cat outfile
hello
```
## 6. 

## 11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью `/proc/cpuinfo`.
Версия sse4_2
```
root@vagrant:~# cat /proc/cpuinfo | grep sse
flags: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single fsgsbase avx2 invpcid rdseed clflushopt md_clear flush_l1d arch_capabilities
```
## 13. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.
Перенесу процесс `top` в screen:
```
vagrant@vagrant:~$ top
ctrl + z
vagrant@vagrant:~$ jobs -l
[1]+  2485 Stopped                 top
vagrant@vagrant:~$ bg
[1]+ top &
vagrant@vagrant:~$ ps -a
    PID TTY          TIME CMD
   1795 pts/3    00:00:00 sudo
   1797 pts/3    00:00:00 su
   1798 pts/3    00:00:00 bash
   2485 pts/0    00:00:00 top
   2496 pts/0    00:00:00 ps
vagrant@vagrant:~$ disown top
-bash: warning: deleting stopped job 1 with process group 2485
vagrant@vagrant:~$ screen
vagrant@vagrant:~$ reptyr 2485
```
## 14. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.
Команда tee записывает вывод другой команды в один или несколько файлов.  
Конструкция `echo string | sudo tee /root/new_file` работает потому что перенаправление данных идёт под обычным пользователем, а запись в новый файл каталога рута с повышением прав.
