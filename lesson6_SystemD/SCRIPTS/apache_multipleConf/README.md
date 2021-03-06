3. Дополнить юнит-файл apache httpd возможностьб запустить несколько инстансов сервера с разными конфигами
---------------------------------------
#####
Создаем unit для запуска по шаблону используя подстановку %i и %I [Specifiers](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Specifiers) /etc/systemd/system/[httpd@.service](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/configs/httpd%40.service)

Создаем конфиг на каждый экземпляр(instance)
/etc/httpd/conf/[httpd_site1.conf](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/configs/httpd_site1.conf)
/etc/httpd/conf/[httpd_site2.conf](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/configs/httpd_site2.conf)

Добавим в конфиг [httpd_site1.conf](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/configs/httpd_site1.conf) и [httpd_site2.conf](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/configs/httpd_site2.conf) путь к PidFile по каждому экземпляру для одновременной работы экземпляров
`PidFile /var/run/httpd/httpd_siteX.pid`

Также созданим цель(Target) для юнитов экземпляров [httpd.target](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/configs/httpd.target) для запуска инстансов по одному target.
`Wants=httpd@site1.service httpd@site2.service`
Указываем в Wants стартовать когда основной юнит стартует. Если сервис упал или не может стартовать, это не влияет на основной сервис. Если основной сервис выключается/перезапускается, то зависимые сервисы остаются незатронутыми.
`PartOf=httpd.target`
PartOf - Если httpd.target выключается/перезапускается, то основной сервис так же выключется/перезапускается.

![target](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/img/target.gif)

![pids](https://github.com/kyourselfer/OTUS_LinuxAdmin201804/blob/master/lesson6_SystemD/SCRIPTS/apache_multipleConf/img/pids.gif)
