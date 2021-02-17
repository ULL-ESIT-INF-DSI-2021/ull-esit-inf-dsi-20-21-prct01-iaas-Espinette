# INFORME PRACTICA 1
## Configuración de máquina virtual en el IaaS
### ALBERTO RIOS DE LA ROSA
### alu0101235929@ull.edu.es

#### INTRODUCCIÓN

En esta práctiva se lleva a cabo la **configuración de la máquina virtual y la instalación de git y Node.js en la misma** que hay disponible a través del servicio IaaS de la ULL, así como la instalación y configuración de las herramientas necesarias para trabjar con la asignatura. En este informe se indicaran los pasos realizados en esta práctica en el que aprenderemos a iniciar una máquina virtual por `ssh` sin introcucir contraseña y sin poner el nombre de usuario o la direccion ip de la máquina, tambien aprenderemos a configurar el git en la máquina de la IaaS para enlazarlo con el Github personal nuestro y más cosas que veremos según desarrollemos la configuración.

#### ELABORACIÓN POR PASOS 

#### CONFIGURACION MAQUINA VIRTUAL

##### 1. Configurar el servicio VPN de la ULL

Para poder acceder al servicio IaaS desde fuera de la universidad, será necesario conectarse a la VPN de la ULL. Para poder conectarse a ella lo haremos a través de la [documentacion de configruación de la VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/) y descargaremos las instrucciones correspondientes al sistema opertivo que usaremos, las seguimos paso a paso y nos conectamos a la VPN de la ULL.

##### 2. Obtener la dirección IP a través del Servicio IaaS de la ULL

Ya conectados a la VPN de la ULL, deberemos acceder al [Servicio IaaS de la ULL](https://iaas.ull.es/ovirt-engine/sso/login.html) e introducir nuestras credenciales de la ULL. Ya iniciada la sesión se nos mostrarán todas las máquinas virtuales que tenemos asociadas a nuestra cuenta e iniciaremos la maquina virtual que se llama *DSI*. Al arrancar la máquina se asignará una de entre todas las maquinas disponibles de la asignatura, se dará cuenta de lo sucedido ya que al nombre de la máquina se le añadira un sufijo del estilo *DSI-55*. Clicamos en la maquina y en la parte derecha donde dice *Interfaces de red* vemos la la dirección IP que tiene asociada la máquina.

##### 3. Acceder a la máquina virtual por ssh y establecer contraseña

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





You can use the [editor on GitHub](https://github.com/ULL-ESIT-INF-DSI-2021/ull-esit-inf-dsi-20-21-prct01-iaas-Espinette/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ULL-ESIT-INF-DSI-2021/ull-esit-inf-dsi-20-21-prct01-iaas-Espinette/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
