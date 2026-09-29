# Tema 2 — Casos prácticos

---

## Caso práctico 1 — Instalación desatendida en un aula de informática

**Planteamiento:**
El departamento de informática de un centro educativo tiene que instalar el mismo paquete de 5 aplicaciones (ofimática, navegador, antivirus, compresor de archivos y un editor de código) en los 25 equipos de un aula, todos con Windows.

**Pregunta:** ¿Instalarías cada aplicación a mano en cada equipo, o usarías una instalación desatendida? Justifica tu respuesta con lo visto en el punto 5 de los apuntes.

<details>
<summary>Ver una posible resolución</summary>

Con 25 equipos y 5 aplicaciones cada uno (125 instalaciones en total), lo lógico es usar una **instalación desatendida**:

- Se prepara un **fichero de respuestas** (`.xml`/`.ini` en Windows) con toda la configuración necesaria para cada aplicación.
- Se lanza en todos los equipos a la vez, sin tener que estar delante de cada uno siguiendo el asistente.
- Se garantiza que **todos los equipos quedan configurados igual** (misma versión, mismas opciones) — algo muy importante en un aula, donde todos los puestos deben ser idénticos.
- Se reduce el riesgo de errores humanos al repetir el mismo proceso manual 125 veces.

Si el centro tuviera muchas aulas y volviera a hacer esto a menudo, además se podría plantear usar una herramienta de gestión a gran escala como **SCCM** o **Altiris**, para centralizar y automatizar todavía más el despliegue.

</details>
