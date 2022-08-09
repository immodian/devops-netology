5. 2 процессора, 1 ГБ оперативной памяти, 64 ГБ жёсткий диск, 4 МБ вдеопамять, 1 гигабитный сетевой адаптер.
6. Добавить в вагрант файл:
```
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end
```
8. 
- Переменная `HISTSIZE` задаёт длину журнала history. Описано на строке 666 man bash.
- В переменной `HISTCONTROL` директива `ignoreboth` объединяет `ignorespace` (не записывать историю команды начинающихся с пробела) и `ignoredups` (не записывать историю повтор команды)
9. 
10. Команда `touch {0..99999}` создаст 100000 файлов. Создание 300000 файлов одной строкой не удалось, упёрлось в лимит `ARG_MAX`.
11. a
12. 
```
vagrant@vagrant:~$ mkdir /tmp/new_path_directory
vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_directory/bash
vagrant@vagrant:~$ PATH=/tmp/new_path_directory:$PATH
vagrant@vagrant:~$ type -a bash
bash is /tmp/new_path_directory/bash
bash is /usr/bin/bash
bash is /bin/bash
```
13. Команда `at` используется для назначения одноразового задания на заданное время, а команда `batch` — для назначения одноразовых задач, которые должны выполняться, когда загрузка системы становится меньше 0,8.