### ProFTPD.

Comenzamos por instalar el paquete necesario para tener el servidor ftp funcional con

[dnf](https://www.server-world.info/en/command/html/dnf.html) install proftpd

Ahora modificamos las reglas de firewall de la misma máquina para que permita el servicio

```
firewall-cmd --add-service=ftp --permanent
firewall-cmd --reload
```


![](https://lh6.googleusercontent.com/-v8Qgl3RGIwlDjUlCKVsZshOPnt43QigUMip5-MLtiFR8SZPYm08Hc6GI5O5N8J34l2MQloY0sbpyPAkHnsuyFMBUANHSv_m9haHVPwMDjgMV-8NJecQO7NO3ZayRzy79zv4FVMs)

Notemos que el servicio está listo


