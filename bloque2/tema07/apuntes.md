# Tema 7 — Administración de sistemas operativos libres (Ubuntu Server)

---

## Índice

1. [Introducción](#1-introducción)
2. [Línea de comandos](#2-arquitectura-unix-y-línea-de-comandos)
3. [Usuarios y grupos locales](#3-usuarios-y-grupos-locales)
4. [Usuarios y grupos predeterminados](#4-usuarios-y-grupos-predeterminados)
5. [Permisos](#5-permisos)
6. [sudo y permisos elevados](#6-sudo-y-permisos-elevados)
7. [Servicios y tareas programadas](#7-servicios-y-tareas-programadas)


---

## 1. Introducción

En el Tema 6 instalaste Ubuntu Server. Ahora toca **administrarlo**: quién puede entrar, qué puede hacer cada uno, qué se ejecuta solo y cómo se protege la información.

| Tarea de un administrador | Dónde se ve en este tema |
|---|---|
| Crear y gestionar usuarios | Apartados 3 y 4 |
| Decidir quién accede a qué | Apartados 5 y 6 |
| Automatizar tareas y controlar servicios | Apartado 7 |
| Vigilar lo que pasa en el servidor | Apartado 8 |
| Proteger y copiar la información | Apartados 9 y 10 |

> Los bloques **"Pruébalo"** se hacen en tu Ubuntu Server del Tema 6.
>
> Los comandos básicos (`ls`, `cd`, `pwd`, `mkdir`...) ya los usaste en la práctica 6B.

---

## 2. Arquitectura UNIX y línea de comandos

Linux se basa en **UNIX**, un diseño de los años 70 que sigue vigente. Sus ideas principales:

| Característica | Qué significa |
|---|---|
| **Multiusuario y multitarea** | Varios usuarios y procesos a la vez |
| **Modular** | Muchas herramientas pequeñas que se combinan entre sí |
| **Sistema de archivos jerárquico** | Todo cuelga de una raíz, `/` |
| **Portabilidad** | Funciona en hardware muy distinto |

La **línea de comandos** es eficiente (sin menús), se puede **automatizar** con scripts, da **más control** y consume muy pocos recursos.

Como las herramientas son pequeñas, se **combinan**:

| Símbolo | Qué hace | Ejemplo |
|---|---|---|
| `\|` | Pasa la salida de un comando al siguiente | `ls /etc \| grep ssh` |
| `>` | Guarda la salida en un archivo (lo **sobrescribe**) | `date > fecha.txt` |
| `>>` | Añade la salida al final del archivo | `date >> fecha.txt` |
| `grep texto` | Filtra las líneas que contienen el texto | `cat /etc/passwd \| grep pcXXX` |
| `head` / `tail` | Muestra las primeras / últimas líneas | `tail -n 5 fecha.txt` |

**Pruébalo:**
1. `ls /etc | grep ssh`. ¿Qué sale?
2. `date >> fecha.txt` dos veces y `cat fecha.txt`. ¿Cuántas líneas hay?
3. `date > fecha.txt` y `cat fecha.txt`. ¿Qué ha cambiado y por qué?

---

## 3. Usuarios y grupos locales

Crear, modificar y borrar **usuarios** y **grupos** es una de las tareas básicas de un administrador. Linux guarda esa información en archivos de texto:

| Archivo | Qué contiene |
|---|---|
| `/etc/passwd` | La lista de usuarios (nombre, UID, carpeta personal, intérprete) |
| `/etc/group` | La lista de grupos y sus miembros |
| `/etc/shadow` | Las contraseñas **cifradas**. Solo lo lee `root` |

Cada usuario y cada grupo tiene un número: el **UID** y el **GID**. El superusuario `root` siempre tiene el UID **0**. Los usuarios normales empiezan en el 1000.

| Tarea | Comando |
|---|---|
| Crear un usuario | `sudo adduser pepito` |
| Cambiar su contraseña | `sudo passwd pepito` |
| Crear un grupo | `sudo groupadd amigos` |
| **Añadir** un usuario a un grupo | `sudo usermod -aG amigos pepito` |
| Cambiar el nombre de un grupo | `sudo groupmod -n vecinos amigos` |
| Ver los grupos de un usuario | `groups pepito` o `id pepito` |
| Borrar un usuario (y su carpeta) | `sudo userdel -r pepito` |
| Borrar un grupo | `sudo groupdel vecinos` |

> **`adduser` o `useradd`:** `adduser` es el cómodo de Debian y Ubuntu: pregunta la contraseña y crea la carpeta en `/home`. `useradd` es la herramienta básica de todas las distribuciones y **no** crea la carpeta si no se añade `-m`.
>
> **Cuidado con `-aG`:** `usermod -G` sin la `a` **reemplaza** todos los grupos del usuario. Con `-aG` los **añade** a los que ya tenía.

**Pruébalo:**
1. `sudo adduser pepito` (la contraseña no se ve; la información extra se puede dejar en blanco con Intro).
2. `id pepito` y `grep pepito /etc/passwd`. ¿Qué UID tiene? ¿Qué carpeta personal?
3. `ls /home`. ¿Aparece su carpeta?
4. `sudo userdel -r pepito` y vuelve a mirar `/home`.

---

## 4. Usuarios y grupos predeterminados

Además de los tuyos, el sistema trae usuarios y grupos que **ya existen** y que necesita para funcionar.

| Usuario | Para qué sirve |
|---|---|
| `root` | El **superusuario**: lo puede todo. Hay que usarlo con cuidado |
| `daemon` | Lo usan servicios que se ejecutan en segundo plano |
| `sys` | Tareas del sistema |
| `www-data` | El usuario con el que corre un servidor web |
| `nobody` | Un usuario sin permisos, para procesos que no necesitan ninguno |

La mayoría tienen como intérprete `/usr/sbin/nologin`: nadie puede **iniciar sesión** con ellos. Son **cuentas de servicio**.

| Grupo | Para qué sirve |
|---|---|
| `sudo` | Sus miembros pueden usar `sudo` (en Ubuntu) |
| `wheel` | Lo mismo en Red Hat y derivados: **no existe en Ubuntu** |
| `adm` | Pueden leer los registros del sistema |

> En Ubuntu, cada usuario tiene además **su propio grupo**, con su mismo nombre.

**Pruébalo:**
1. `grep -E "^(sudo|adm):" /etc/group`. ¿Está `pcXXX` en ellos?
2. `grep nologin /etc/passwd | head -5`. ¿Qué usuarios no pueden iniciar sesión?

---

## 5. Permisos

Cada archivo y cada carpeta tiene un **propietario**, un **grupo** y tres tipos de permisos para tres tipos de personas.

<img src="img/permisos.png" alt="Permisos -rwxr-xr--: tipo, propietario rwx, grupo r-x, otros r--, equivalente al número 754" width="600">

| Letra | Permiso | Valor | En una carpeta |
|---|---|---|---|
| `r` | Leer | 4 | Ver lo que contiene |
| `w` | Escribir | 2 | Crear y borrar archivos |
| `x` | Ejecutar | 1 | **Entrar** en ella |

Se ven con `ls -l`:

```
-rw-r--r-- 1 pcXXX pcXXX 120 oct 21 10:00 notas.txt
drwxrwx--- 2 root     equipo   4096 oct 21 10:05 compartida
```

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `chmod` | Cambia los **permisos** | `chmod 640 notas.txt` o `chmod g+w notas.txt` |
| `chown` | Cambia el **propietario** (y el grupo) | `sudo chown ana:equipo notas.txt` |
| `chgrp` | Cambia el **grupo** | `sudo chgrp equipo notas.txt` |

| Número | Quién puede hacer qué | Uso típico |
|---|---|---|
| `600` | Solo el propietario lee y escribe | Archivos privados |
| `644` | Propietario lee y escribe; el resto, solo lee | Archivos normales |
| `700` | Solo el propietario | Carpetas personales |
| `755` | Todos entran y leen; solo el propietario escribe | Carpetas y programas públicos |
| `770` | Propietario y grupo, todo; los demás, nada | Carpetas de un equipo |
| `2770` | Como 770, y lo que se cree dentro **hereda el grupo** de la carpeta | Carpetas **compartidas** de un grupo |

> El `2` inicial de `2770` activa el bit **setgid**: los archivos nuevos que se crean en esa carpeta pertenecen al grupo de la carpeta, y no al grupo personal de quien los crea. Es lo que hace falta para una carpeta compartida.

**Pruébalo:**
1. `touch prueba.txt` y `ls -l prueba.txt`. ¿Qué permisos tiene?
2. `chmod 600 prueba.txt` y `ls -l prueba.txt`. ¿Qué ha cambiado?
3. `chmod g+w,o+r prueba.txt` y mira otra vez. ¿Qué hace cada parte?

---

## 6. sudo y permisos elevados

Trabajar siempre como `root` es peligroso: un error lo borra todo. En Ubuntu el usuario `root` **no tiene contraseña** para entrar, y los administradores usan **`sudo`**.

| Cosa | Cómo funciona |
|---|---|
| `sudo comando` | Ejecuta **solo ese comando** como administrador, pidiendo **tu** contraseña |
| Grupo `sudo` | Quien está en él, puede usar `sudo` |
| `/etc/sudoers` | Define quién puede ejecutar qué con `sudo`. Se edita **solo** con `sudo visudo`, que comprueba la sintaxis antes de guardar |
| Registro | Cada uso de `sudo` queda **anotado**: se sabe quién ejecutó qué y cuándo |

Buenas prácticas de seguridad:

| Práctica | Por qué |
|---|---|
| **Mínimos privilegios** | Dar solo los permisos necesarios. Carpetas personales `700` y públicas `755` |
| Dar `sudo` solo a quien lo necesite | Menos riesgo de acceso no autorizado |
| **Usar grupos** para dar permisos | Es más fácil de revisar que dar permisos a cada persona |
| Revisar los permisos de vez en cuando | Se quedan obsoletos |
| Evitar `sudo` en scripts automáticos | Reduce riesgos |
| Mirar los registros de `sudo` | Para detectar usos extraños |

**Pruébalo:**
1. `sudo -l` muestra lo que puedes hacer con `sudo`.
2. `id`. ¿Está `sudo` entre tus grupos?
3. **No edites** `/etc/sudoers` a mano: si lo haces con un editor normal y te equivocas, pierdes `sudo`. Se usa `sudo visudo`.

---

## 7. Servicios y tareas programadas

Un **servicio** es un programa que se ejecuta **en segundo plano** (SSH, un servidor web...). En Ubuntu los gestiona **systemd**, con `systemctl`:

| Comando | Qué hace |
|---|---|
| `systemctl status nombre` | Ver si está en marcha |
| `sudo systemctl start nombre` | Arrancarlo |
| `sudo systemctl stop nombre` | Pararlo |
| `sudo systemctl restart nombre` | Reiniciarlo |
| `sudo systemctl enable nombre` | Que arranque **solo** con el sistema |
| `systemctl list-units --type=service` | Ver los servicios |

Cada servicio debería correr con su **propia cuenta de servicio**, con los permisos mínimos. Si lo atacan, el daño se queda en esa cuenta.

La **automatización** evita errores humanos y ahorra tiempo. Hay dos herramientas:

| Herramienta | Qué es |
|---|---|
| **cron** | Ejecuta tareas a horas fijas. Se configura con `crontab -e` |
| **Temporizadores de systemd** | La alternativa moderna: más flexible y con más control |

Cada línea de cron tiene **cinco campos de tiempo** y después el comando:

```
*  *  *  *  *   comando
│  │  │  │  └─ día de la semana (0-7, el 0 y el 7 son domingo)
│  │  │  └──── mes (1-12)
│  │  └─────── día del mes (1-31)
│  └────────── hora (0-23)
└───────────── minuto (0-59)
```

| Línea | Cuándo se ejecuta |
|---|---|
| `* * * * * comando` | Cada minuto |
| `30 2 * * * comando` | Todos los días a las 2:30 |
| `0 9 * * 1 comando` | Los lunes a las 9:00 |
| `*/15 * * * * comando` | Cada 15 minutos |

| Comando | Qué hace |
|---|---|
| `crontab -e` | Edita tus tareas (la primera vez pide elegir editor: `nano`) |
| `crontab -l` | Muestra tus tareas |
| `crontab -r` | **Borra todas** tus tareas |

**Pruébalo:**
1. `systemctl status cron`. ¿Está activo?
2. `systemctl list-units --type=service | head -15`.
3. `systemctl status ssh`. Si pone `inactive` y aun así te conectas bien por SSH, es normal en algunas versiones: Ubuntu arranca SSH solo cuando alguien se conecta (mira `ssh.socket`).

---

## Resumen del tema

| Tema | Lo que hay que saber hacer |
|---|---|
| Comandos | Combinar comandos con `\|`, `>` y `grep` |
| Usuarios y grupos | `adduser`, `passwd`, `groupadd`, `usermod -aG`, `userdel -r` y `id` |
| Predeterminados | Reconocer `root`, las cuentas de servicio y el grupo `sudo` |
| Permisos | Leer `ls -l` y usar `chmod`, `chown` y el `2770` de una carpeta compartida |
| sudo | Por qué se usa en vez de `root`, y que `sudoers` se edita con `visudo` |
| Servicios y cron | `systemctl status` y escribir una línea de `crontab` |


