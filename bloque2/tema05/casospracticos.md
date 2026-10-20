# Tema 5 — Casos prácticos

[← Volver a los apuntes](apuntes.md)

> Lee el planteamiento, piensa tu respuesta, y solo después abre la resolución.

---

## Caso práctico 1 — "El ordenador se ralentiza cada mañana"

**Planteamiento:**
Marta trabaja en una oficina por las mañanas con un ordenador con Windows 10. Cada semana se instalan actualizaciones, y mientras se instalan el equipo va lento y la obliga a reiniciar.

**Pregunta:** ¿Cómo puede evitar que se actualice durante la mañana?

<details>
<summary>Ver resolución</summary>

Debe configurar las **horas activas**:

| Paso | Qué hacer |
|---|---|
| 1 | Configuración → *Actualización y seguridad* → *Windows Update* |
| 2 | Clic en **Cambiar horas activas** |
| 3 | Poner el horario de trabajo, por ejemplo de **09:00 a 14:00**, y guardar |

Windows no reiniciará solo para actualizar dentro de ese horario. Las actualizaciones **no se cancelan**: se instalan fuera de él.

> Las horas activas tienen un máximo de **18 horas** de duración.

</details>

---

## Caso práctico 2 — "Reinstalar y no encontrar los drivers"

**Planteamiento:**
Juan tiene un portátil HP y va a reinstalar el sistema operativo. Quiere descargar antes los controladores para instalarlos después y que todo funcione.

**Pregunta:** ¿Cómo obtiene los controladores de su equipo? ¿Cuáles debería bajar **primero**?

<details>
<summary>Ver resolución</summary>

| Paso | Qué hacer |
|---|---|
| 1 | Ir a la web de **soporte de HP** (el fabricante) |
| 2 | Introducir el **modelo** del portátil o su **número de serie** |
| 3 | Elegir el sistema operativo que va a instalar |
| 4 | Descargar los controladores a un **USB** |

**Los primeros: los de la red.** Si Windows no reconoce la tarjeta de red después de instalar, no habrá Internet para bajar el resto.

> El número de serie se ve en `cmd` con `wmic bios get serialnumber` o, en PowerShell, con `Get-CimInstance Win32_BIOS | Select-Object SerialNumber`.

</details>

---

## Caso práctico 3 — "De Windows 7 a Windows 10 sin perder nada"

**Planteamiento:**
Un usuario quiere pasar su portátil de Windows 7 a Windows 10. Para migrar hay que **reinstalar** el sistema por completo, y se borra todo lo que hay en `C:`.

**Pregunta:** ¿Qué debe hacer para no perder la información y cómo instala Windows 10?

<details>
<summary>Ver resolución</summary>

**Antes de instalar:**

| Qué | Cómo |
|---|---|
| Guardar los datos (escritorio, documentos, imágenes...) | En un **USB** o disco externo, o en la nube |
| Apuntar los programas instalados | Para volver a instalarlos después |

**Instalar:**

| Paso | Qué se hace |
|---|---|
| 1 | Descargar Windows 10 con la **Media Creation Tool** y crear un USB de 8 GB o más |
| 2 | Configurar la **BIOS** para que el equipo arranque desde el USB |
| 3 | Seguir el asistente de instalación |
| 4 | Instalar los programas y copiar de vuelta los datos |

</details>

---

## Caso práctico 4 — "Treinta ordenadores iguales para el aula"

**Planteamiento:**
Un centro tiene que preparar **30 ordenadores idénticos** con Windows y los mismos programas. Un técnico propone instalar uno a mano y **copiar su disco** a los otros 29 sin más.

**Preguntas:**
1. ¿Qué problema hay?
2. ¿Qué hay que hacer antes de copiar?

<details>
<summary>Ver resolución</summary>

**1. El problema:** los 30 equipos serían **copias exactas**, con el mismo **SID** (y el mismo nombre de equipo). Microsoft no da soporte a esto: exige pasar antes por Sysprep.

**2. El orden correcto:**

| Paso | Qué se hace |
|---|---|
| 1 | Instalar y configurar **un equipo modelo** |
| 2 | Ejecutar **Sysprep** con *OOBE* + *Generalizar* + *Apagar* |
| 3 | **Capturar la imagen** con DISM desde Windows PE |
| 4 | **Aplicar** la imagen en los otros 29 equipos |

Cada equipo arranca en OOBE como si fuera nuevo, con su propio SID.

> Sysprep va **antes** de capturar la imagen. Hacerlo después de copiarla no está soportado.

</details>

---

## Caso práctico 5 — "Sysprep no pudo validar la instalación"

**Planteamiento:**
Ana ejecuta Sysprep en su máquina virtual clonada y aparece el mensaje: *"Sysprep no pudo validar la instalación de Windows. Revise el archivo de registro en `%WINDIR%\System32\Sysprep\Panther\setupact.log`"*.

**Preguntas:**
1. ¿Tiene algo que ver con el SID?
2. ¿Qué debe hacer para averiguar y arreglar la causa?

<details>
<summary>Ver resolución</summary>

**1. No.** El error sale **antes** de cambiar nada: Sysprep revisa que el equipo está en condiciones de ser generalizado y, si no, se detiene. El SID aún no se ha tocado.

**2. Cómo averiguar la causa:** el propio mensaje dice dónde mirar.

| Paso | Qué hacer |
|---|---|
| 1 | En PowerShell como administrador: `Select-String -Path C:\Windows\System32\Sysprep\Panther\setupact.log,C:\Windows\System32\Sysprep\Panther\setuperr.log -Pattern "not provisioned"` |
| 2 | Buscar una línea como *"Package ... was installed for a user, but not provisioned for all users"*. Ahí sale el **nombre de la aplicación** culpable |
| 3 | Desinstalarla para todos los usuarios: `Get-AppxPackage -AllUsers *NombreApp* \| Remove-AppxPackage -AllUsers` |
| 4 | Volver a ejecutar Sysprep |

**La causa más habitual:** una aplicación de la **Microsoft Store** que se ha instalado o actualizado para un usuario, pero no para todos. Pasa si el equipo ha estado conectado a Internet y Windows ha actualizado sus aplicaciones solo.

**Cómo evitarlo:** no instalar ni actualizar aplicaciones de la Store antes de ejecutar Sysprep.

> Si Sysprep falla **durante** la generalización, Microsoft indica que esa imagen puede no volver a admitir Sysprep. Por eso, antes de ejecutarlo, haz siempre una **instantánea**.

</details>

---

## Caso práctico 6 — "El equipo va lento y no sé por qué"

**Planteamiento:**
Un compañero se queja de que su equipo con Windows va muy lento desde hace días. Tú eres el administrador.

**Pregunta:** ¿Dónde miras y qué herramientas usas?

<details>
<summary>Ver resolución</summary>

| Qué miras | Herramienta |
|---|---|
| ¿Qué proceso gasta CPU, memoria o disco? | Administrador de tareas, o `resmon` |
| ¿Cómo evoluciona con el tiempo? | Monitor de rendimiento (`perfmon`) |
| ¿Cuánto usa ahora mismo? | `Get-Counter` en PowerShell |
| ¿Hay programas que arrancan con Windows y sobran? | Administrador de tareas → pestaña *Inicio* |
| ¿Hay un programa nuevo que pueda ser el culpable? | `appwiz.cpl` y su fecha de instalación |
| ¿Qué dice el equipo? | Visor de eventos (Tema 3) |

Y por último, comprobar con **Windows Update** que no hay una actualización a medias consumiendo recursos.

</details>

---

**Siguiente paso:** [Práctica del Tema 5 →](practica-sysprep.md)
