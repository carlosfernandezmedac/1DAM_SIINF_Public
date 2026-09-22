# Tema 1 — Casos prácticos

---

## Caso práctico 1 — "El ordenador emite una serie de sonidos agudos y no arranca"

**Planteamiento:**
Álvaro ha comprado varios componentes para mejorar su ordenador (placa base Dell): un SSD, memoria RAM y una tarjeta gráfica. Los ha instalado todos. Al encenderlo, escucha **dos tonos agudos** y no aparece nada en pantalla.

**❓ Pregunta:** ¿Qué harías como técnico informático para diagnosticar y solucionar el problema?

<details>
<summary>✅ Ver resolución</summary>

**Paso 1.** Cada fabricante de placa base tiene su propio código de pitidos. Lo primero es consultar la web del fabricante o el manual.

**Paso 2.** En las placas Dell, según la tabla de códigos:

| Nº de tonos | Componente afectado | Qué significa |
|---|---|---|
| 1 | Placa base | Fallo de ROM/BIOS |
| **2** | **Memoria** | **No se detecta ninguna RAM** |
| 3 | Placa base (chipset) | Fallo del reloj, teclado o E/S |
| 4 | Memoria | Fallo de memoria (RAM) |
| 5 | Reloj en tiempo real | Fallo de la batería CMOS |
| 6 | Vídeo | Fallo de tarjeta o chip de vídeo |
| 7 | CPU | Fallo de la unidad de procesamiento |

**Conclusión:** 2 tonos en Dell = **no se detecta la memoria RAM**. Álvaro debe revisar que el módulo de RAM esté bien encajado en su ranura, o probar con otro módulo compatible.

</details>

---

## Caso práctico 2 — "Míriam no puede imprimir con su nueva impresora"

**Planteamiento:**
Míriam ha conectado una impresora nueva y más moderna a su ordenador. Está bien conectada y encendida, pero no consigue imprimir nada.

**❓ Pregunta:** ¿Cómo identificarías el problema como técnico informático?

<details>
<summary>✅ Ver resolución</summary>

**Paso 1.** Al ser un dispositivo nuevo, es muy probable que el ordenador **no tenga instalado el controlador (driver)** necesario para comunicarse con él.

**Paso 2.** Se comprueba en el **Administrador de dispositivos** (Windows): si el dispositivo aparece con un icono de aviso ⚠️, indica que es un "dispositivo desconocido" — falta el controlador.

**Paso 3.** Solución: instalar el controlador correcto, normalmente descargándolo de la web del fabricante de la impresora o dejando que Windows lo busque automáticamente.

</details>

---

## Caso práctico 3 — Diagnóstico de componentes (repaso)

**Planteamiento:**
Un compañero te trae un ordenador que no enciende en absoluto: no hay pitidos, no hay ventiladores, no hay luces. Repasa mentalmente los 4 componentes imprescindibles del Tema 1.

**❓ Pregunta:** ¿Por dónde empezarías a comprobar, y en qué orden?

<details>
<summary>✅ Ver una posible resolución</summary>

Si no hay **ningún** signo de vida (ni luces ni pitidos), el problema suele estar **antes** de que la placa base pueda hacer su autodiagnóstico:

1. **Fuente de alimentación** — ¿está encendida, bien conectada, el cable no está dañado? Es lo primero a comprobar, porque sin corriente nada más puede funcionar.
2. **Conexión de la placa base** — ¿el conector de 24 pines y el de la CPU están bien encajados?
3. Si la fuente funciona y la placa recibe corriente pero sigue sin dar señales, entonces sí tocaría revisar **RAM** y **almacenamiento**, como en el Caso práctico 1.

La diferencia clave con el Caso 1: allí *sí* había pitidos (la placa "hablaba"); aquí no hay ninguno, así que hay que ir un paso **más atrás**, al suministro eléctrico.

</details>

---

## Caso práctico 4 — Clasificar

**5.** Clasifica estos periféricos según su tipo (entrada / salida / E-S / almacenamiento / comunicación):

`ratón` · `impresora` · `pantalla táctil` · `disco duro externo` · `tarjeta de red` · `micrófono` · `auriculares` · `USB`

<details><summary>Solución</summary>

| Periférico | Tipo |
|---|---|
| Ratón | Entrada |
| Impresora | Salida |
| Pantalla táctil | Entrada/Salida |
| Disco duro externo | Almacenamiento |
| Tarjeta de red | Comunicación |
| Micrófono | Entrada |
| Auriculares | Salida |
| USB (pendrive) | Almacenamiento |

</details>

---


## Caso práctico 5 — PAS

**6** Explica con tus palabras qué significa el protocolo **PAS** y pon un ejemplo de cuándo se aplicaría en un entorno de oficina informático.

<details><summary>Solución</summary>

**PAS = Proteger, Avisar, Socorrer.** Es el protocolo de actuación ante un accidente:
- **Proteger**: evitar que el accidente vaya a más (por ejemplo, cortar la corriente si hay riesgo eléctrico).
- **Avisar**: llamar a los servicios de emergencia.
- **Socorrer**: ayudar a la persona accidentada según tu formación.

Ejemplo: si un compañero recibe una descarga eléctrica al manipular una fuente de alimentación, primero se corta la corriente (proteger), se llama al 112 (avisar) y se atiende a la persona sin moverla si no es necesario (socorrer).

</details>

---

## ## Caso práctico 6 — Clasificar


**10.** Busca en tu propio ordenador (o en uno del aula) el **Administrador de dispositivos** de Windows:

- Un dispositivo de entrada
- Un dispositivo de salida
- Un dispositivo de almacenamiento
- Si aparece algún dispositivo con el icono de advertencia ⚠️ (controlador no instalado)

<details><summary>Pista</summary>En Windows: clic derecho en el menú Inicio → Administrador de dispositivos. En Linux, abre una terminal y ejecuta <code>lsusb</code> para dispositivos USB o <code>lspci</code> para dispositivos internos.</details>
