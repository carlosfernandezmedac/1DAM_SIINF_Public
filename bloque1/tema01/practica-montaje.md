# Práctica — Identificación, documentación y verificación de componentes de hardware

**Modalidad:** grupos de 2-3 personas

---

## Objetivo

Identificar los componentes físicos del equipo de clase, verificar su información básica en la BIOS, documentar los resultados y diseñar un equipo equivalente con hardware actual.

## Antes de empezar — seguridad

Antes de abrir la caja, recuerda lo visto en el punto 4 de los apuntes (Seguridad y prevención de riesgos):

- Apaga el equipo y **desconecta el cable de corriente** antes de manipular el interior.
- La caja de este modelo es *toolless* (se abre sin herramientas, con una pestaña o palanca lateral) — no fuerces nada si no cede.
- Si detectas cualquier riesgo (cable pelado, olor a quemado, componente suelto), avisa al profesor antes de seguir.

---

## Actividad 1 — Identificación de componentes físicos

Abre el equipo e identifica los principales componentes de hardware. Anota los datos que se puedan leer directamente de las etiquetas o de las piezas, y **fotografía cada componente** que identifiques (la pieza en sí y, si tiene, su etiqueta con el modelo).

Debes identificar, registrar y fotografiar:

- **Procesador (CPU):** modelo, velocidad y número de núcleos.
- **Placa base:** fabricante y modelo.
- **Memoria RAM:** cantidad total, tipo y velocidad. *(No des por supuesto el tipo — compruébalo en la propia memoria o en la BIOS.)*
- **Disco duro o SSD:** tipo (HDD o SSD), capacidad y velocidad (RPM o MB/s).
- **Unidad de CD/DVD:** indicar si existe, modelo y si funciona correctamente.
- **Tarjeta gráfica:** integrada o dedicada, modelo y memoria.
- **Fuente de alimentación:** potencia (W) y tipo de conector.
- **Puertos disponibles:** cantidad y tipo (USB 2.0, USB 3.0, HDMI, VGA, Ethernet, etc.).
- **Conexiones cableadas actuales:** identifica qué cables tiene el equipo tanto en la parte trasera como en la parte interna. Por ejemplo, monitor por VGA, teclado y ratón por USB, red por Ethernet, corriente, cable de datos SATA (disco/SSD → placa base), cable de alimentación SATA (fuente → disco/SSD), conector de alimentación de la placa base (24 pines). Anota el tipo de conector de cada cable.
- **Caja/torre del equipo:** identifica el factor de forma exacto (no des por hecho que es ATX — comprueba qué formato es realmente este modelo), el material y características como ventilación o filtros.

## Actividad 2 — Acceso a la BIOS

1. Enciende el equipo y presiona la tecla correspondiente para entrar en la BIOS.

2. Toma capturas de pantalla o fotografías donde se observe:
   - Modelo del procesador.
   - Cantidad de memoria RAM detectada.
   - Información del disco duro.
   - Temperaturas o estado general del sistema (si aparece).

## Actividad 3 — Identificación de la placa base en internet

- Busca el modelo exacto de la placa base.
- En el documento, incluye:
  - Nombre completo y fabricante de la placa base.
  - Principales características técnicas:
    - Tipo de socket del procesador.
    - Tipo y número de ranuras para memoria RAM.
    - Puertos de expansión (PCI, PCIe).
    - Tipo de conectores SATA o IDE.
    - Tamaño o formato de la placa.
  - Enlace a la página oficial del fabricante (HP) u otra información referente.

## Actividad 4 — Diseño de un equipo equivalente actual

Con un **presupuesto máximo de 1.000 €**, busca en tiendas online (PcComponentes, Amazon, etc) los componentes de un **PC de sobremesa de generación actual** que pueda sustituir a este equipo, mejorando sus prestaciones.

Entrega una tabla con estos componentes, su modelo, precio y enlace a la tienda:

| Componente | Modelo elegido | Precio | Enlace |
|---|---|---|---|
| Procesador (CPU) | | | |
| Placa base | | | |
| Memoria RAM | | | |
| Almacenamiento (SSD) | | | |
| Fuente de alimentación | | | |
| Caja/torre | | | |
| Tarjeta gráfica *(si aplica)* | | | |
| **Total** | | | |

**Comprobación de compatibilidad.** Antes de dar por buena tu selección, verifica y justifica cada punto:

- [ ] El **socket** de la CPU coincide con el que admite la placa base.
- [ ] El **tipo de RAM** (DDR4, DDR5...) es compatible con la placa base, y no superas el número de ranuras disponibles.
- [ ] La **potencia de la fuente de alimentación** es suficiente para todos los componentes (suma el consumo de cada pieza + un margen).
- [ ] El **formato de la placa base** (ATX, MicroATX...) cabe en la caja elegida.
- [ ] El **precio total no supera los 1.000 €**.

## Actividad 5 — Entrega

El documento de entrega debe incluir:

- Nombres y apellidos de los integrantes del grupo.
- Tabla completa con los datos de los componentes, con sus fotografías (Actividad 1).
- Capturas o fotos de la BIOS (Actividad 2).
- Enlaces de referencia de la placa base (Actividad 3).
- Tabla del equipo equivalente actual, con su comprobación de compatibilidad (Actividad 4).
- Breve comentario sobre el estado general del equipo (si funciona todo correctamente o si hay fallos detectados).

## Actividad 6 — Verificación en clase

Cada grupo debe mostrar al profesor que el equipo arranca correctamente y que pueden acceder a la BIOS, comprobando así el funcionamiento básico del sistema.
