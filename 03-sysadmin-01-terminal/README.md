# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"  

5. Какие ресурсы выделены по-умолчанию?  
  1024 Mb RAM  
  1 cpu  
  64 Gb HDD  
  
6. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?  
  Добавить в Vagrantfile:
     ``` 
     config.vm.provider "virtualbox" do |v|  
       v.memory = 2048  
       v.cpus = 2
	end	
  
8.1. какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?  
  (Номера строк для стандартной ширины 80 символов)  
  ```HISTSIZE``` (стр.709) - Количество команд, хранящихся для текущей сессии;  
  ```HISTFILESIZE``` (стр.1123) - Количество команд, хранящихся в .bash_history 
  
8.2. что делает директива ignoreboth в bash?  
  опция ```HISTCONTROL=ignoreboth``` в .bashrc - bash не будет сохранять в истории строки, начинающиеся с пробела, и повторяющиеся команды.  
9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?  
  С помощью {} можно создавать списки (стр.1464):  
   ```
   {0..10}  
   {a..z}{a..z}
   ```  
  Расширение параметров (стр.1551):  
    ```${parameter}``` - в строку подставляется значение параметра или выражения  
  Группировка команд (стр.340):  
    ```{command1; command2;} > file``` - позволяет направить в файл вывод обоих команд  
    
10. Как создать однократным вызовом touch 100000 файлов?  
  ```touch file{1..100000}```  
  А получилось ли создать 300000?  
  Нет, длина команды, заданной таким образом, превышает допустимую (ARG_MAX = 2097152)  
  
11. Что делает конструкция [[ -d /tmp ]]  
  Проверяет, существует ли директория /tmp  

12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

bash is /tmp/new_path_directory/bash  
bash is /usr/local/bin/bash  
bash is /bin/bash  

  ```
  mkdir /tmp/new_path_directory/bash  
  ln -s /bin/bash /usr/local/bin/bash  
  ln -s /bin/bash /tmp/new_path_directory/bash  
  PATH="/tmp/new_path_directory:$PATH"
  ```  
  
  13. Чем отличается планирование команд с помощью batch и at?  
    batch работает только в интерактивном режиме;   
    не позволяет задать время выполнения, а добавляет в очередь;  
    выполняет команду, когда средняя загрузка системы ниже 1.5 (или заданной в cron)
