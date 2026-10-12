# Tema 4 — Casos prácticos

[← Volver a los apuntes](apuntes.md)

> Lee el planteamiento, piensa tu respuesta, y solo después abre la resolución.

---

## Caso práctico 1 — "¿Qué herramienta de virtualización elijo?"

**Planteamiento:**
Una pequeña empresa ha comprado un equipo nuevo y quiere virtualizar en él un servidor antiguo de bases de datos. No quiere pagar licencias si no es necesario.

**Pregunta:** ¿Qué pasos seguirías y qué tipo de herramienta recomendarías?

<details>
<summary>Ver resolución</summary>

**Los tres pasos de siempre:**

| Paso | Aplicado al caso |
|---|---|
| 1. Objetivo | Virtualizar un **servidor** (no un escritorio) |
| 2. Productos | Hay opciones de pago y gratuitas |
| 3. Hardware | Comprobar que la CPU soporta virtualización y que hay RAM y disco suficientes |

**Opciones gratuitas para un servidor:**

| Opción | Cuándo encaja |
|---|---|
| Proxmox VE (KVM) | Servidor dedicado a máquinas virtuales, con interfaz web |
| Hyper-V | Si la empresa ya tiene Windows Server (viene incluido) |
| VMware | Revisar la licencia vigente, porque ha cambiado varias veces |

**Conclusión:** para un servidor en producción conviene un hipervisor de **tipo 1** gratuito, como Proxmox VE, o Hyper-V si ya hay Windows Server. VirtualBox (tipo 2) vale para hacer pruebas, pero no para un servidor de producción, porque depende de un sistema operativo debajo.

</details>

---

## Caso práctico 2 — "Un servidor de hace 20 años que nadie se atreve a tocar"

**Planteamiento:**
Una empresa tiene un servidor web Apache de 32 bits, con Perl y Debian, funcionando desde hace más de 20 años. Han intentado migrarlo varias veces sin éxito. El hardware ya no se fabrica y no es compatible con las copias de seguridad actuales.

**Pregunta:** ¿Se puede resolver con virtualización?

<details>
<summary>Ver resolución</summary>

**Sí.** Los servidores nuevos son de 64 bits, pero una herramienta de virtualización actual puede ejecutar el sistema antiguo de 32 bits dentro de una máquina virtual, sin tocar su sistema operativo ni sus librerías (por ejemplo, con Proxmox VE).

| Problema | Solución con virtualización |
|---|---|
| Hardware antiguo que ya no se fabrica | El sistema vive en una VM sobre hardware nuevo y con soporte |
| No hay copias de seguridad compatibles | Se hacen **instantáneas** y copias de la VM entera |
| Miedo a romper algo | Se prueba en un clon y se vuelve atrás si falla |

Con la VM funcionando se puede montar una infraestructura segura: un **proxy** en la entrada y dos servidores Apache detrás, conectados con *switches* virtuales (uno externo y otro interno). Así se reparte la carga y, si uno cae, el otro sigue funcionando.

</details>

---

## Caso práctico 3 — "He clonado la máquina y las dos se pelean"

**Planteamiento:**
Laura ha hecho un clon de su máquina virtual Windows Server para tener una copia. Al arrancar las dos a la vez con el adaptador puente, la red empieza a dar fallos: se cortan las conexiones y a veces una de las dos pierde la IP.

**Pregunta:** ¿Qué puede haber pasado y cómo lo comprobarías?

<details>
<summary>Ver resolución</summary>

**Causa más probable:** el clon se hizo **conservando la dirección MAC** de la original. Dos equipos con la misma MAC en la misma red provocan conflictos, porque el router no sabe a cuál entregar los datos.

| Comprobación | Cómo |
|---|---|
| ¿Tienen la misma MAC? | VirtualBox → *Configuración* → *Red* → *Avanzadas*, o `ipconfig /all` (dirección física) dentro de cada VM |
| ¿Tienen IP distinta? | `ipconfig` en cada una |
| ¿Se ven entre ellas? | `ping IP-de-la-otra` (si no responde, puede ser el cortafuegos) |

**Solución:** apagar las dos y volver a clonar eligiendo **"Generar nuevas direcciones MAC para todos los adaptadores de red"**, o cambiar la MAC del clon en la configuración de red.

> Además, el clon tiene el mismo nombre de equipo. En un entorno real hay que renombrarlo, porque dos equipos con el mismo nombre también dan problemas.

</details>

---

## Caso práctico 4 — "El servidor se cae y la web se queda sin servicio"

**Planteamiento:**
Una tienda online funciona con un único servidor físico. Cuando falla la fuente de alimentación, la web se queda caída varias horas y pierden ventas.

**Pregunta:** ¿Cómo ayudaría la virtualización a evitar que ocurra de nuevo?

<details>
<summary>Ver resolución</summary>

**Montar un entorno de alta disponibilidad:**

| Pieza | Para qué |
|---|---|
| Varios servidores físicos (nodos) | Si uno falla, hay otros que siguen funcionando |
| Almacenamiento compartido | Todos los nodos ven los mismos discos de las VM |
| Máquinas virtuales | Si cae un nodo, sus VM se **reinician automáticamente** en otro |
| Reparto de carga | Evita que un solo nodo se sature |
| Copias e instantáneas | Se puede recuperar la VM aunque haya un desastre |

El diseño se prueba antes en un entorno de **simulación** (virtual), forzando la caída de un nodo y comprobando que los servicios siguen activos.

</details>
