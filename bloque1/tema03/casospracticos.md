# Tema 3 — Casos prácticos


---

## Caso práctico 1 — "El ordenador va cada vez más lento"

**Planteamiento:**
Mario trabaja con Windows y tiene muchos archivos y programas abiertos a la vez. El sistema va cada vez más lento y a veces la pantalla se queda congelada unos segundos. No sabe qué programa o archivo le está causando el problema.

**Pregunta:** ¿Cómo ayudarías a Mario a saber qué software causa el problema de rendimiento? ¿Tiene solución?

<details>
<summary>Ver resolución</summary>

**1. Abrir el Administrador de tareas:** pulsando `Ctrl + Mayús + Esc`, o con `Ctrl + Alt + Supr` y eligiendo "Administrador de tareas", o con clic derecho en la barra de tareas.

**2. Localizar al culpable:** en la pestaña "Procesos" se ve cuánta CPU, memoria, disco y red consume cada programa. Basta con ordenar por cada columna y ver cuál se dispara.

**3. Actuar según lo que se encuentre:**

| Síntoma | Recurso saturado | Solución |
|---|---|---|
| Un programa consume casi toda la CPU | CPU | Cerrarlo (Finalizar tarea) si no se está usando |
| La memoria está al 90 % o más | RAM | Cerrar programas en segundo plano; si ocurre siempre, ampliar la RAM |
| El disco está al 100 % constantemente | Disco | Revisar qué lo usa; si es un HDD, valorar cambiarlo por un SSD |
| Tarda mucho en arrancar | Programas de inicio | Pestaña "Inicio" del Administrador de tareas: deshabilitar los que no hagan falta |

**4. Si aun así no se soluciona:** el equipo se ha quedado corto de hardware, y habrá que ampliar o sustituir el recurso que esté limitando (CPU o memoria).

> Cuidado: no hay que finalizar procesos del sistema que no se reconozcan, podría desestabilizar Windows.

</details>

---

## Caso práctico 2 — "Permisos en carpetas de Windows"

**Planteamiento:**
Carmen es propietaria de una academia con un ordenador común para los alumnos. Cada alumno deja sus trabajos en una carpeta con su nombre dentro de `C:\`. Carmen ha comprobado que algunos alumnos entran en las carpetas de sus compañeros y copian archivos.

**Pregunta:** ¿Cómo puede aplicar permisos para que a cada carpeta solo puedan acceder ella y el alumno correspondiente?

<details>
<summary>Ver resolución</summary>

**Pasos, para cada carpeta de alumno:**

1. Clic derecho en la carpeta → **Propiedades** → pestaña **Seguridad**.
2. Pulsar **Opciones avanzadas** y **deshabilitar la herencia** (convirtiendo los permisos heredados en explícitos). Si no, la carpeta sigue heredando los permisos generales de `C:\`, que permiten entrar a todos.
3. Eliminar de la lista al grupo **Usuarios** (y cualquier otro que no deba acceder).
4. Pulsar **Editar → Agregar**, añadir el usuario de Carmen con **Control total** y el del alumno con **Modificar**.
5. Aceptar y comprobar iniciando sesión con otro alumno: debe aparecer "Acceso denegado".

**Una forma alternativa:** añadir los dos usuarios con "Modificar" y **Denegar** permisos al resto. Funciona, pero hay que tener cuidado: **Denegar prevalece siempre sobre Permitir**. Si se deniega a un grupo al que también pertenece el propio alumno (por ejemplo, "Usuarios"), se le bloquearía a él también. Por eso es más limpio quitar la herencia y conceder solo a quien debe acceder.

</details>

---

## Caso práctico 3 — "Probar Ubuntu sin instalarlo"

**Planteamiento:**
María acaba de comprar un ordenador nuevo y quiere probar Ubuntu antes de decidir si lo instala. No quiere borrar nada del disco ni perder el Windows que ya trae.

**Pregunta:** ¿Puede probarlo sin ningún problema legal? ¿Cómo lo haría sin instalarlo?

<details>
<summary>Ver resolución</summary>

**Sí, sin problema:** Ubuntu es software libre y de código abierto, así que puede descargarlo, usarlo y probarlo libremente.

**Pasos:**

1. Descargar la imagen ISO desde la web oficial de Ubuntu.
2. Copiarla a un USB de arranque (con una herramienta como Rufus o balenaEtcher).
3. Reiniciar el ordenador y entrar en la BIOS o en el menú de arranque para arrancar desde el USB.
4. En la pantalla de Ubuntu, elegir **"Probar Ubuntu"** (en lugar de "Instalar Ubuntu").

Así Ubuntu funciona como **sistema operativo ejecutable (live)**: corre desde el USB y la memoria RAM, sin tocar el disco duro. Al apagar, no queda nada instalado y Windows sigue intacto. Si le convence, puede lanzar la instalación desde esa misma pantalla.

</details>

---

## Caso práctico 4 — "¿Qué licencia necesito?"

**Planteamiento:**
Una academia va a comprar licencias de Windows para tres situaciones distintas:

1. 20 ordenadores nuevos que llegan ya con el sistema operativo preinstalado.
2. 30 ordenadores de un aula que se reinstalan con frecuencia, todos con el mismo software.
3. El portátil de un profesor, que piensa cambiarlo por otro dentro de dos años y quiere llevarse su licencia.

**Pregunta:** ¿Qué tipo de licencia (OEM, retail o por volumen) elegirías para cada caso, y por qué?

<details>
<summary>Ver resolución</summary>

| Situación | Licencia | Por qué |
|---|---|---|
| 20 ordenadores nuevos con SO preinstalado | **OEM** | Va asociada al equipo en que viene instalada; es la más barata, y aquí no hace falta pasarla a otro equipo |
| 30 ordenadores del aula, reinstalaciones frecuentes | **Por volumen** | Un único acuerdo para toda la organización, con descuento por cantidad y activación más sencilla |
| Portátil del profesor, con cambio de equipo previsto | **Retail** | Es la única que se puede transferir de un equipo a otro (OEM está prohibida de ceder, y volumen es para organizaciones) |

</details>
