Подключение принтера HP LJ 1010/1015/1018/1020 в Linux Debian(Ubuntu) c CUPS 1.4 и выше

----------------------- 
Вариант решения данной проблемы:
— Загрузить модуль сразу после включения принтера
— Выгрузить модуль сразу после заливки filmware

Для этого делаем следующее:

0) Отключаем принтер
1) Установим необходимые пакеты
 aptitude install cupsys gs-esp foomatic-bin foo2zjs cups-pdf   

2) Скачаем требуемое filmware для принтера сконвертируем и разместим в соответствующих папках:
 wget http://foo2zjs.rkkda.com/firmware/sihp1018.tar.gz
 tar xvzf sihp1018.tar.gz
 arm2hpdl sihp1018.img > sihp1018.dl
 cp sihp1018.dl /usr/share/foo2zjs/firmware
 cp sihp1018.img /usr/share/foo2zjs/firmware
 cp sihp1018.dl /lib/firmware/hp
 cp sihp1018.img /lib/firmware/hp

3) Создадим правило для диспетчера устройств udev для загрузки модуля при включении принтера.
 vi /etc/udev/rules.d/11-hplj10xx.rules

Указываем Vid и Pid своего принтера! Пример приведён для 1018.
#Own udev rule for HP Laserjet 1018
SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="03f0", ATTRS{idProduct}=="4117", RUN+="modprobe usblp"
