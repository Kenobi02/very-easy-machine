# primeros pasos

Maquina: **Inyection**

Dificultad: **muy facil**

Temas a tratar: _SQLMAP_ / _Pentesting web_ / _hacking web_ / _intrucion ssh_

Como primer paso realizaremos un escaneo de nmap de la siguiente forma:

```
 sudo nmap -p- -sCV --open -sS --min-rate 5000 -vvv -n -Pn 172.17.0.2
```

![Pingufoto1](https://github.com/user-attachments/assets/2ec2a564-2225-4a6d-a814-ad3e3e1d2bbb)

Gracias a este escaneo podemos descubrir que la maquina tiene abiertos los puertos **80** y **22**.
Asi que con eso ahora iremos a la web escribiremos en el buscador la ip de la maquina victima: ```172.17.0.2```
de esta forma veremos que hay en el puerto 80 de la maquina...

![pingu2](https://github.com/user-attachments/assets/305872f5-d1a1-4185-b3e4-c5eebf7448e3)
Es una web, ya he realizado el escaneo con **gobuster** y **wfuzz** y te adelanto que no hay nada ahi ni en el codigo fuente
entonces utilizaremos ```sqlmap``` para conseguir estas credenciales en su base de datos.

Aqui les dejo el comando que utilizaremos:

```
sqlmap --url http://172.17.0.2/ --dbs --batch --forms

```

con este escaneo de sqlmap estamos buscando bases de datos por eso el parametro ```--dbs``` y gracias a esto encontramos 5 bases de datos aqui debajo te las dejo:

![pingu3](https://github.com/user-attachments/assets/481f3219-9cd1-43b1-aeca-64b881325df8)

de las 5 en esta situacion solo nos interesa la que se llama **register** ya que por su nombre nos podemos imaginas que ahi dentro estan los usuarios registrados de la web.
entonces ahora usaremos el mismo comando pero con algunas diferencias ahora que ya tenemos identificada una base de datos.

```
sqlmap --url http://172.17.0.2/ 
