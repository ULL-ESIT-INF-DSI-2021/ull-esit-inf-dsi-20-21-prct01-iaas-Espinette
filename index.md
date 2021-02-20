# INFORME PRACTICA 1
## Configuración de máquina virtual en el IaaS
### ALBERTO RIOS DE LA ROSA
### alu0101235929@ull.edu.es

### INTRODUCCIÓN

En esta práctiva se lleva a cabo la **configuración de la máquina virtual y la instalación de git y Node.js en la misma** que hay disponible a través del servicio IaaS de la ULL, así como la instalación y configuración de las herramientas necesarias para trabjar con la asignatura. En este informe se indicaran los pasos realizados en esta práctica en el que aprenderemos a iniciar una máquina virtual por `ssh` sin introcucir contraseña y sin poner el nombre de usuario o la direccion ip de la máquina, tambien aprenderemos a configurar el git en la máquina de la IaaS para enlazarlo con el Github personal nuestro y más cosas que veremos según desarrollemos la configuración.

### ELABORACIÓN POR PASOS 

### CONFIGURACION MAQUINA VIRTUAL

#### 1. Configurar el servicio VPN de la ULL

Para poder acceder al servicio IaaS desde fuera de la universidad, será necesario conectarse a la VPN de la ULL. Para poder conectarse a ella lo haremos a través de la [documentacion de configruación de la VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/) y descargaremos las instrucciones correspondientes al sistema opertivo que usaremos, las seguimos paso a paso y nos conectamos a la VPN de la ULL.

#### 2. Obtener la dirección IP a través del Servicio IaaS de la ULL

Ya conectados a la VPN de la ULL, deberemos acceder al [Servicio IaaS de la ULL](https://iaas.ull.es/ovirt-engine/sso/login.html) e introducir nuestras credenciales de la ULL. Ya iniciada la sesión se nos mostrarán todas las máquinas virtuales que tenemos asociadas a nuestra cuenta e iniciaremos la maquina virtual que se llama *DSI*. Al arrancar la máquina se asignará una de entre todas las maquinas disponibles de la asignatura, se dará cuenta de lo sucedido ya que al nombre de la máquina se le añadira un sufijo del estilo *DSI-55*. Clicamos en la maquina y en la parte derecha donde dice *Interfaces de red* vemos la la dirección IP que tiene asociada la máquina.

#### 3. Acceder a la máquina virtual por ssh y establecer contraseña

Al igual que para acceder al Servicio IaaS si queremos acceder por `ssh` debemos estar conectado a la VPN de la ULL en todo momento. Para conectarnos por `ssh` debemos abrir una terminal y teclear lo siguiente, siendo la X los distintos numeros de la IP obtenida con anterirorida:

```bash
  ssh usuario@10.6.XXX.XXX
```
Introducimos *yes* y pulse intro ante la siguiente pregunta:

```bash
The authenticity of host '10.6.XXX.XXX (10.6.XXX.XXX)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Ya realizado se nos pedirá una contraseña debiendo introducir *usuario* pues es la credencial por defecto tanto para el nombre de usuario como la contraseña. Una vez introducida el sistema nos pedirá automaticamente que la modifiquemos, para ello debemos introducir nuevamente la contraseña actual (*usuario*) y luego nos pedirá introducir dos veces la nueva contraseña, es importante que no nos olvidemos de la contraseña siendo recomendable apuntarla por algún lado. Ya realizado esto volveremos a iniciar la máquina por `ssh` pero esta vez nos pedirá la nueva contraseña para acceder.


#### 4. Modificar nombre de host de la máquina virtual

Para cambiar el nombre host de nuestra máquina debemos cambiar el contenido del fichero `etc/hostname` que contendrá el nombre de la máquina, en nuestro caso *ubuntu* y lo modificaremos por el nuevo nombre que queremos poner, en este caso se nos pide poner *iaas-dsi2* como nombre para la máquina. Vemos los comandos necesarios para ello:

```bash
usuario@ubuntu:~$ cat /etc/hostname
ubuntu
usuario@ubuntu:~$ sudo vi /etc/hostname
usuario@ubuntu:~$ cat /etc/hostname
iaas-dsi2
```
También debemos modificar el fichero `etc/hosts`, lo vemos en los siguientes comandos:

```bash
usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...
usuario@ubuntu:~$ sudo vi /etc/hosts
usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	iaas-dsi2
...
```
Aqui ya hemos cambiado el nombre de host *ubuntu* por el nuevo nombre que le hemos asignado *iaas-dsi2*. Antes de reiniciar la maquina virtual para hacer los cambios efectivos debemos actualizar el software de la misma, usando:

```bash
usuario@ubuntu:~$ sudo apt update
...
usuario@ubuntu:~$ sudo apt upgrade
...
```
Ahora ya podemos reiniciar la máquina:

```bash
usuario@ubuntu:~$ sudo reboot
Connection to 10.6.XXX.XXX closed by remote host.
Connection to 10.6.XXX.XXX closed.
```

#### 5. Editar fichero host de la máquina local para incluir información de la máquina virtual

Antes de abrir la máquina virtual de nuevo, modificaremos el fichero `etc/hosts` de nuestra máquina local para incluir la IP de la máquina virtual y así no tendremos que conectarnos por ssh introdcuiendo la IP, si nlo por el nombre que le pusimos anterirormente, los pasos a seguir son los siguientes:

```bash
#Mostramos el fichero que contiene las ip seguido del host al que pertenece dicha ip
alberto@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...

#Modificamos ese fichero añadiendo una linea con la ip de nuestra máquina virtual y el nombre que le asiganmos a ella
alberto@ubuntu:~$ sudo vi /etc/hosts

#Mostramos el fichero nuevamente para confirmar que los cambios han sido correctos
alberto@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubutnu

10.6.XXX.XXX	iaas-dsi2
...
```

#### 6. Configurar la clave pública-privada en la máquina local

Para comprobar si ya tenemos dicha clave observaremos si tenemos el fichero `etc/hosts`en nuestra máquina en caso de tenerla podemos saltarnos este paso, en caso de no disponer de dicha clave ejecutaos el siguiente comando:

```bash
alberto@ubuntu:~$ ssh-keygen
```

Por lo general tomaremos los valores por defecto por lo que no escribiremos nada y pulsaremos intro, ya generadas las claves ejecutamos el comando que nos permite copiar la clave pública desde la máquina local a la virtual y responderemos a la pregunta responderemos con un *yes*.

```bash
alberto@ubuntu:~$ ssh-copy-id usuario@iaas-dsi2
The authenticity of host 'iaas-dsi2 (10.6.XXX.XXX)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
usuario@iaas-dsi2's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'usuario@iaas-dsi2'"
and check to make sure that only the key(s) you wanted were added.
```
Ya hecho esto, si iniciamos la maquina virtual por `ssh` ejcutando el siguiente comando nos debería dejar entrar sin introducir ninguna contraseña.

```bash
alberto@ubuntu:~$ ssh usuario@iaas-dsi2
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Feb  5 10:16:46 WET 2021

...

Last login: Fri Feb  5 10:14:25 2021 from 10.20.53.243
usuario@iaas-dsi2:~$
```

#### 7. Acceder a la máquina virtual sin necesidad de introcuir el nombre de usuario

Para poder hacer esto debemos configurar el fichero `~/.ssh/config`de la máquina local, haciendo lo siguiente:

```bash
alberto@ubuntu:~$ touch ~/.ssh/config 
alberto@ubuntu:~$ vi ~/.ssh/config 
alberto@ubuntu:~$ cat ~/.ssh/config 
Host iaas-dsi2
  HostName iaas-dsi2
  User usuario
```

Ya una vez hecho esto podremos hacer la conexión por `ssh` solamente indicando el nombre de la máquina virtual.

```bash
alberto@ubuntu:~$ ssh iaas-dsi2
```

#### 8. Generar la clave pública-privada de la máquina virtual

Es seguir los pasos que hicimos en el paso 6, lo que esta vez todos tendremos que crear la clave privada

```bash
usuario@iaas-dsi2:~$ ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/usuario/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/usuario/.ssh/id_rsa.
Your public key has been saved in /home/usuario/.ssh/id_rsa.pub.
The key fingerprint is:
...
```

Para comprobar si las claves se han optenido correctamente miraremos si el siguiente fichero se ha creado correctamente.

```bash
usuario@iaas-dsi2:~$ cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDfCKDPGn7qLhwmjKCYaCBeOZVObmdHQ/GFOYALTU1Lmnjb108HGSr7aDaFZunI1TtwAKh1qGdo6LYCPECY+y90LA+vtRTdQnoPqGzsTctillZkRJoMv7beLhpHVKFadXNc/DKlwCU83uHwTRGmqb3OY5246rSIA+/blFpDEBpK088oXvTailphaCeZRHV+Qg12LJu5Q2uKBTjckU0yebz4Xx2wXjZQkpohX8PSOJpKy6QlNmG8j3DDY+qrRmy+LScRGyWHlqQIVR2YrejuHqs2mG1b8FSGNwUUCp20rc0TWV22ggjQxEmjCRAHIofRsZ7zN752aChLqWGXcDJLTI8d 
usuario@iaas-dsi2
```

Ya una vez hecho esto, hemos terminado la configuración inicial de la maquina virtual

### INSTALACIÓN DE GIT Y NODE.JS EN LA MAQUINA VIRTUAL DEL IAAS

#### 1. Instalar git en la máquina virtual

Normalmente suele venir preinstalado en el sistema operativo, pero en caso de que no esté ejecutaremos:

```bash
usuario@iaas-dsi2:~$ sudo apt install git
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho
git ya está en su versión más reciente (1:2.25.1-1ubuntu3).
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```

#### 2. Configuración git máquina virtual

Para seguir la configuración tenemos el siguiente [libro](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez) aunque la configuración se resume en ejecutar los siguientes comandos:

´´´bash
usuario@iaas-dsi2:~$ git config --global user.name "Alberto Rios de la Rosa"
usuario@iaas-dsi2:~$ git config --global user.email alu0101235929@ull.edu.es
usuario@iaas-dsi2:~$ git config --list
user.name=Alberto Rios de la Rosa
user.email=alu0101235929@ull.edu.es
´´´

#### 3. Configurar prompt de la terminal en la máquina virtual

Está configuración se hace para que aparezca la rama actual en la que nos encontramos cuando accedemos a algún directorio que resulta estar asociado a un repositorio git. Para esto, descargaremos el script [git prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) y le echamos un vistazo al contenido para aprender a configurarlo en nuestra máquina virtual. Los pasos que doy a continuación serían para una `bash`:

```bash
usuario@iaas-dsi2:~$ mv git-prompt.sh .git-prompt.sh
usuario@iaas-dsi2:~$ vi .bashrc
usuario@iaas-dsi2:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi2:~$ exec bash -l
[~()]$
```

Podemos ver que el prompt ha cambiado de `usuario@iaas-dsi2:~$` a `[~()]$`.
No obstante, para comprobar si realmente el prompt muestra lo que deseamos, que no es otra cosa que la rama actual de trabajo cuando accedemos a un directorio asociado a un repositorio, tendremos que añadir la clave pública de la máquina virtual en la configuración de las claves de nuestra cuenta de GitHub, de modo que nos sea mucho más fácil trabajar con repositorios remotos, y así poder también clonar alguno de los repositorios para hacer la prueba. En primer lugar, copie la clave pública de su máquina virtual:

```bash
[~()]$cat ~/.ssh/id_rsa.pub
```

Una vez copiada, acceda a la configuración de su cuenta de GitHub (account settings), y en la sección SSH and GPG keys, pulse sobre el botón New SSH key. En el formulario añada un título para la clave (yo tengo usuario@iaas-dsi2) y pegue la clave pública de su máquina virtual en el campo de texto key. Por último, pulse sobre el botón Add SSH key. Si todo ha ido bien, ahora podría ejecutar el siguiente comando desde la máquina virtual para clonar un repositorio:

```bash
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2021/prct01-iaas-vscode.git
Clonando en 'prct01-iaas-vscode'...
remote: Enumerating objects: 100, done.
remote: Counting objects: 100% (100/100), done.
remote: Compressing objects: 100% (79/79), done.
remote: Total 100 (delta 32), reused 50 (delta 16), pack-reused 0
Recibiendo objetos: 100% (100/100), 341.75 KiB | 1.63 MiB/s, listo.
Resolviendo deltas: 100% (32/32), listo.
[~()]$ls

#Vemos que se ha creado correctamente
prct01-iaas-vscode

#Accedemos al directorio
[~()]$cd prct01-iaas-vscode/
[~/prct01-iaas-vscode(main)]$
```

#### 4. Instalar Node Version Manager (nvm), el gestor de versiones de Node.js

[Node.js](https://nodejs.org/en/) es un entorno que permite la ejecución de código desarrollado en JavaScript y variantes, como por ejemplo, TypeScript.

```bash
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
[~()]$nvm --version
0.37.2
````

Ya instalado nvm vamos a proceder a instalar la versión más reciente de Node.js:

```bash
[~()]$nvm install node
Downloading and installing node v15.8.0...
Downloading https://nodejs.org/dist/v15.8.0/node-v15.8.0-linux-x64.tar.xz...
####################################################################################################################################################################################### 100,0%
Computing checksum with sha256sum
Checksums matched!
Now using node v15.8.0 (npm v7.5.1)
Creating default alias: default -> node (-> v15.8.0)
[~()]$node --version
v15.8.0
[~()]$npm --version
7.5.1
````
Vemos que se ha instalado la versión más reciente de Node, pero para isntalar una versión más concreta hacemos lo siguiente:

```bash
[~()]$nvm install 12.0.0
Downloading and installing node v12.0.0...
Downloading https://nodejs.org/dist/v12.0.0/node-v12.0.0-linux-x64.tar.xz...
####################################################################################################################################################################################### 100,0%
Computing checksum with sha256sum
Checksums matched!
Now using node v12.0.0 (npm v6.9.0)
[~()]$node --version
v12.0.0
[~()]$npm --version
6.9.0
````

Por último, para cambiar entre versiones, podemos ejecutar los siguientes comandos:

```bash
[~()]$nvm list
->      v12.0.0
        v15.8.0
default -> node (-> v15.8.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v15.8.0) (default)
stable -> 15.8 (-> v15.8.0) (default)
lts/* -> lts/fermium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.23.2 (-> N/A)
lts/erbium -> v12.20.1 (-> N/A)
lts/fermium -> v14.15.4 (-> N/A)
[~()]$nvm use v15.8.0 
Now using node v15.8.0 (npm v7.5.1)
[~()]$node --version
v15.8.0
[~()]$npm --version
7.5.1
````

Una vez hecho esto ya habremos terminado la configuración de nuestra máquina virtual para el correcto desarrollo de la asignatura, y podremos proceder a la realización de futuras prácticas facilitando el trabajo en estás.







