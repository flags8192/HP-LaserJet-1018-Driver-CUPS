Подключение принтера HP LJ 1010/1015/1018/1020 в Linux Debian(Ubuntu) c CUPS 1.4 и выше
4 мин
62K
Настройка Linux
*
При обновлении версии CUPS возникла проблемма его несовместимости с загруженным модулем usbpl, необходимый для загрузки firmware в принтер. При их одновременной работе возникает конфликт на шине usb(одновременное обращение), отражающееся в логах системы /var/log/syslog следующим образом:
-----------------------

Jul 1 02:18:57 kernel: [ 3115.009361] usb 1-2.5: usbfs: interface 0 claimed by usblp while 'usb' sets config #1

----------------------- 
Вариант решения данной проблемы:
— Загрузить модуль сразу после включения принтера
— Выгрузить модуль сразу после заливки filmware

Для этого делаем следующее:


0) Отключаем принтер
1) Установим необходимые пакеты
 aptitude install cupsys gs-esp foomatic-bin foo2zjs cups-pdf   
Объяснить код с

2) Скачаем требуемое filmware для принтера сконвертируем и разместим в соответствующих папках:
 wget http://foo2zjs.rkkda.com/firmware/sihp1018.tar.gz
 tar xvzf sihp1018.tar.gz
 arm2hpdl sihp1018.img > sihp1018.dl
 cp sihp1018.dl /usr/share/foo2zjs/firmware
 cp sihp1018.img /usr/share/foo2zjs/firmware
 cp sihp1018.dl /lib/firmware/hp
 cp sihp1018.img /lib/firmware/hp
Объяснить код с

3) Создадим правило для диспетчера устройств udev для загрузки модуля при включении принтера.
 vi /etc/udev/rules.d/11-hplj10xx.rules
