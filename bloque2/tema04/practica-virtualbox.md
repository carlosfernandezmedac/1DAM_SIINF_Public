# Tema 4 — Práctica: VirtualBox a fondo con Windows Server

[← Volver a los apuntes](apuntes.md)

---

## Antes de empezar

Necesitas:

- VirtualBox.
- Tu máquina virtual de **Windows Server 2019** de las prácticas anteriores (disco de 50 GB, 2 GB de RAM y adaptador puente). **No hay que crear ninguna máquina nueva.**
- Espacio libre en tu equipo para un clon: tanto como el **tamaño real** de tu disco, que medirás en la Parte 1.

**Objetivo:** manejar las funciones de VirtualBox que usa un administrador: Guest Additions, carpetas compartidas, clones, instantáneas, discos adicionales y red entre máquinas.

> **Orden de las partes.** Hazlas en este orden. En especial, el disco nuevo (Parte 6) va **después** de la instantánea (Parte 5): al restaurar una instantánea, la configuración de la VM vuelve a como estaba entonces, y el disco nuevo desaparecería.

---

## Parte 1 — Conoce tu máquina virtual

Con la VM **apagada**:

1. Selecciona la VM en VirtualBox y haz **una captura** del panel de la derecha, donde se ve la RAM, los procesadores, el disco y la red.
2. Abre *Archivo* → *Herramientas* → *Administrador de medios virtuales*, selecciona el disco de tu VM y anota:
   - **Tamaño virtual** y **tamaño real**.
   - La **ruta** del archivo `.vdi`.
3. **Responde en el documento:** ¿por qué el tamaño real es menor que el virtual?

---

## Parte 2 — Guest Additions, portapapeles y arrastrar y soltar

1. Enciende la VM e inicia sesión como Administrador.
2. Comprueba si ya están instaladas: *Panel de control* → *Programas y características* → busca **Oracle VirtualBox Guest Additions**. Si aparece, salta al punto 4.
3. Instálalas: menú *Dispositivos* → *Insertar imagen de CD de las Guest Additions*. En la VM abre el Explorador → unidad **VirtualBox Guest Additions** → ejecuta `VBoxWindowsAdditions.exe` → *Siguiente* hasta el final → **Reiniciar ahora**.
4. Activa el intercambio: menú *Dispositivos* → *Portapapeles compartido* → **Bidireccional**, y *Dispositivos* → *Arrastrar y soltar* → **Bidireccional**.
5. Pruébalo:
   - Copia un texto del Bloc de notas de **tu PC** y pégalo en el Bloc de notas de **la VM** (y al revés).
   - Arrastra un archivo del escritorio de tu PC al escritorio de la VM.

**Capturas:** *Programas y características* con las Guest Additions, y la VM con el archivo arrastrado y el texto pegado.

---

## Parte 3 — Carpeta compartida entre tu PC y la VM

1. En tu **PC real**, crea en el escritorio una carpeta llamada `compartida_<tu número de equipo>` (por ejemplo, `compartida_107`).
2. En VirtualBox: *Configuración* de la VM → *Carpetas compartidas* → botón ➕:
   - **Ruta:** la carpeta que acabas de crear.
   - Marca **Automontar** y **Hacer permanente**. Acepta.
3. En la VM, abre el Explorador → *Este equipo*: debe aparecer la carpeta como una unidad de red. Si no aparece, escribe `\\VBOXSVR\compartida_<tu número>` en la barra de direcciones, o reinicia la VM.
4. Comprueba que funciona en los dos sentidos:
   - Crea `desde-la-vm.txt` dentro de la carpeta **en la VM** y comprueba que aparece **en tu PC**.
   - Crea `desde-el-host.txt` **en tu PC** y comprueba que aparece **en la VM**.

**Captura:** el Explorador de la VM con la carpeta compartida y los dos archivos.

---

## Parte 4 — Clon completo y red entre máquinas

1. **Antes de clonar**, permite el ping en la VM original (así también lo tendrá el clon). En `cmd` como administrador:

```
netsh advfirewall firewall add rule name="Ping" protocol=icmpv4:8,any dir=in action=allow
```

   Si en el Tema 3 ya lo permitiste, no hace falta. Después, **apaga la VM**.

2. Comprueba que tu equipo tiene espacio libre suficiente (el tamaño real que anotaste en la Parte 1, más unos GB de margen).
3. En VirtualBox: clic derecho en la VM → **Clonar** y configura:

| Opción | Valor |
|---|---|
| Nombre | `Windows Server Reserva` |
| Política de direcciones MAC | **Generar nuevas direcciones MAC para todos los adaptadores de red** |
| Tipo de clon | **Clon completo** |
| Alcance (si lo pregunta) | Solo el estado actual |

   **Captura:** la lista de VirtualBox con las dos máquinas.

4. **Responde en el documento:** ¿qué diferencia hay entre un clon y una instantánea? (Ayuda: tabla del apartado 5.2 de los apuntes.)
5. Comprueba que las dos VM usan el **Adaptador puente** (*Configuración* → *Red*) y arranca las dos. Necesitas RAM para dos máquinas a la vez (2 GB cada una): si tu equipo va justo, hazlo con un compañero, cada uno con su VM.
6. En cada VM ejecuta `ipconfig` y anota la IP: **deben ser distintas**. Después, desde cada una, `ping IP-de-la-otra`.

**Capturas:** `ipconfig` y el ping en las dos máquinas, con respuesta.

> Las dos máquinas tienen el mismo nombre de equipo. Para este ejercicio no importa, pero en un entorno real habría que renombrar el clon.

Al terminar, apaga el clon. Si vas justo de espacio, puedes eliminarlo cuando hagamos la práctica del tema 5 (*Eliminar* → *Eliminar todos los archivos*).

---

## Parte 5 — Instantánea: volver atrás en el tiempo

En la VM **original**:

1. Enciéndela y crea en el escritorio un archivo `prueba-snapshot_PC<tu número de equipo>.txt` con el Bloc de notas. **Captura 1:** el escritorio con el archivo.
2. Con la VM encendida: *Herramientas* → *Instantáneas* → **Tomar**:
   - **Nombre:** `Antes de borrar el archivo`
   - **Descripción:** `Estado con prueba-snapshot.txt en el escritorio`

   **Captura 2:** la lista de instantáneas.
3. **Borra** el archivo del escritorio y **vacía la papelera**. **Captura 3:** el escritorio sin el archivo.
4. **Apaga la VM** (desde el menú Inicio de Windows). El botón *Restaurar* solo funciona con la máquina apagada.
5. En *Instantáneas*, selecciona `Antes de borrar el archivo` → **Restaurar**. Cuando pregunte si quieres crear una instantánea del estado actual, **desmarca la casilla** y confirma.
6. Arranca la VM. El archivo ha vuelto. **Captura 4:** el escritorio con el archivo de nuevo.
7. En la carpeta de la VM, abre la subcarpeta `Snapshots` y anota qué archivos hay y cuánto ocupan.

**Responde en el documento:** ¿qué ocurre con todo lo que hayas hecho en la VM después de tomar la instantánea cuando la restauras?

---

## Parte 6 — Segundo disco duro

Con la VM **apagada**:

1. *Configuración* → *Almacenamiento* → en el controlador **SATA**, pulsa el icono de **Añadir disco duro** → *Crear*:
   - Tipo **VDI**, **Reservado dinámicamente**.
   - Nombre `Datos2`, tamaño **1 GB**.

   Selecciónalo y pulsa *Elegir*. **Captura:** *Almacenamiento* con los dos discos.
2. Arranca la VM y ejecuta `diskmgmt.msc` (Administración de discos):
   - Si pide **inicializar el disco**, acepta (GPT).
   - Clic derecho en el espacio **sin asignar** → *Nuevo volumen simple* → *Siguiente* hasta el final, con una **letra libre** (E: o F: por ejemplo), sistema **NTFS** y etiqueta `Datos2`.

   **Captura:** Administración de discos con el disco ya formateado.
3. Abre la unidad nueva en el Explorador y crea un archivo en ella. **Captura.**

**Responde en el documento:** ¿para qué sirve tener varios discos en un entorno virtual? Cita al menos dos usos.

---

## Problemas frecuentes

| Problema | Causa probable y solución |
|---|---|
| No aparece la carpeta compartida en la VM | Faltan las Guest Additions o hay que reiniciar la VM. Prueba con `\\VBOXSVR\nombre-de-la-carpeta` |
| Arrastrar y soltar no funciona | Comprueba que están las Guest Additions y que está en *Bidireccional*. A veces falla en Windows: usa la carpeta compartida |
| El botón *Restaurar* está desactivado | La VM está encendida: apágala primero |
| El clon y la original no se ven | Revisa que las dos usan *Adaptador puente*, que tienen MAC distinta y que el ping está permitido en el cortafuegos |
| No hay espacio para clonar | El clon completo ocupa tanto como el tamaño real de tu disco: libera espacio o consulta al profesor |
| El disco nuevo no aparece en el Explorador | Falta inicializarlo y crear el volumen en Administración de discos |
| La VM va muy lenta (aparece la tortuga) | Mira el Tema 3: la virtualización por hardware no está disponible o está desactivada |

Si te topas con alguno de estos problemas, anótalo en el documento como **incidencia**, con el mismo formato del Tema 2.

---

## Parte opcional — clon enlazado

Haz otro clon de la VM, esta vez de tipo **Clon enlazado**, y compara cuánto ocupa con el clon completo. ¿Qué pasaría con el clon enlazado si borraras la VM original?

---

## El informe

Un documento en **PDF**, entregado en el Aula Virtual, con:

- Las **capturas** de cada parte, cada una con un **pie** que diga qué se ve.
- Las **respuestas** a las cuatro preguntas (tamaño real y virtual, clon e instantánea, restaurar una instantánea y varios discos).
- Las incidencias, si las hubo.

No hace falta vídeo: la instantánea se demuestra con sus **cuatro capturas** (archivo, instantánea creada, archivo borrado y archivo recuperado).

---

## Rúbrica de evaluación

Cada ítem vale **1,25 puntos** (8 ítems = 10 puntos).

| Ítem | ¿Cumple? |
|---|---|
| Parte 1: captura de la configuración de la VM, tamaño virtual y real del `.vdi`, y explica la diferencia | ☐ |
| Parte 2: Guest Additions instaladas y portapapeles y arrastrar y soltar bidireccionales, con captura de la prueba | ☐ |
| Parte 3: carpeta `compartida_<nº de equipo>` accesible desde la VM, con archivos creados en los dos sentidos | ☐ |
| Parte 4: clon completo `Windows Server Reserva` creado con nueva MAC, y explica la diferencia entre clon e instantánea | ☐ |
| Parte 4: las dos VM en adaptador puente con IP distinta y ping entre ellas, con capturas | ☐ |
| Parte 5: las cuatro capturas de la instantánea (archivo, instantánea con descripción, archivo borrado, archivo recuperado) | ☐ |
| Parte 6: segundo disco de al menos 1 GB reconocido y formateado en Windows Server, con capturas, y explica su utilidad | ☐ |
| Documento ordenado, con pies en las capturas, y entregado en PDF en el Aula Virtual | ☐ |

**Nota:** ______ / 10
