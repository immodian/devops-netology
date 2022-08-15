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
