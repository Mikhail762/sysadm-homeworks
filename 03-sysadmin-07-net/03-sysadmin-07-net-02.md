**Задание 2
Предлагаю выполнить задание 1 первый раз с параметрами по умолчанию и второй раз с 1% ошибок. После этого сравнить результаты.**  

Если я правильно понял, имеется в виду эти параметры из задания 1:  
`1 Гбит/с канал при 300 мс RTT`  
```buildoutcfg
vagrant@vagrant:~$ sudo tc qdisc add dev eth0 root netem delay 300ms
vagrant@vagrant:~$ iperf3 -c 192.168.56.1
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-10.00  sec  2.49 MBytes  2.09 Mbits/sec    0             sender
```  
теперь добавляю потери:  
```buildoutcfg
vagrant@vagrant:~$ sudo tc qdisc del dev eth0 root
vagrant@vagrant:~$ sudo tc qdisc add dev eth0 root netem delay 300ms loss 1%
vagrant@vagrant:~$ iperf3 -c 192.168.56.1 -t 60
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-60.00  sec  6.81 MBytes   952 Kbits/sec   41             sender
```  
Получается соотношение 2.09/0.952 = 2.195 раз.
Если убрать задержку в 300ms, получается:  502 Mbits/sec / 156 Mbits/sec =  3.2 раза.  

Если воспользоваться формулой Матисса rate < (MSS/RTT)*(C/sqrt(Loss)) [ C=1 ] по ссылке из предыдущего задания, то можно сравнить коэффициенты 1/sqrt(Loss).  
Для уровня потерь 10e-06(%) = 10^-8 (который считается не влияющим на качество передачи), коэффициент равен 10^4,  
а для уровня 1% - 10. Соотношение коэффициентов = 10000/10 = 1000 раз.
