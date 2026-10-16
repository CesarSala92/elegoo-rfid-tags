# Generador de tags RFID Elegoo

Herramienta web gratuita para crear etiquetas NFC (NTAG213/215/216) que el **Canvas de Elegoo** (Centauri Carbon) reconoce como bobinas de filamento. Así puedes usar bobinas de cualquier marca con el sistema multimaterial.

Todo funciona en tu navegador: no se envía ningún dato a ningún servidor.

## Qué hace

Eliges material, subtipo, color, acabado, temperaturas, diámetro, peso y fecha, y la herramienta genera:

- **Comandos para NFC Tools** en formato `A2:página:datos`, listos para copiar y pegar.
- **Comandos para Proxmark3**.
- **El mapa de páginas** del tag, con lo que va en cada byte.

## Cómo grabar un tag

1. Instala **NFC Tools** (gratis, iOS o Android).
2. En la herramienta, configura la bobina y pulsa **Copiar**.
3. En NFC Tools ve a **Otros → Comandos NFC avanzados**. No uses «Escribir».
4. Pega los comandos en el campo de datos y pulsa enviar.
5. Acerca una pegatina **NTAG213** virgen al móvil y mantenla quieta hasta ver el check.

## Formato del tag

Los datos que lee la impresora están en las páginas 16 a 24. Todos los valores van en big-endian.

| Página | Contenido |
|---|---|
| 4–9 | Enlace NDEF a elegoo.com (opcional, solo para el móvil) |
| 16 | Cabecera `0x36` + inicio del código de fabricante `EEEEEEEE` |
| 17 | Fin del código de fabricante + código de filamento |
| 18 | Material (PLA, PETG, ABS…) |
| 19 | Subtipo: familia + variante |
| 20 | Color RGB + acabado (mate, seda, translúcido, galaxy) |
| 21 | Temperatura mínima / máxima del extrusor (°C) |
| 22 | Uso desconocido (a cero) |
| 23 | Diámetro (centésimas de mm) / peso (g) |
| 24 | Fecha de fabricación en BCD (AA MM) |

## Limitaciones conocidas

- **Solo reconoce la marca Elegoo.** El firmware solo acepta el código de fabricante de Elegoo, así que cualquier bobina aparecerá como «ELEGOO».
- **Solo materiales del catálogo.** No se pueden crear materiales nuevos; hay que elegir el más parecido.
- **Centauri Carbon 1:** en las pruebas, el Canvas de la CC1 muestra el rango de temperatura por defecto del material (por ejemplo, 190–230 °C en PLA) en lugar del que lleva el tag. La temperatura con la que se imprime la decide el perfil de filamento del slicer.
- **Subtipos ocultos:** los marcados con `*` existen en el firmware pero no aparecen en la pantalla de la CC2.

La guía oficial de Elegoo no coincide con lo que leen las impresoras. El formato usado aquí es el que la comunidad ha obtenido de tags originales.

## Créditos

Basado en el trabajo de la comunidad:

- [DnG-Crafts/ELG-RFID](https://github.com/DnG-Crafts/ELG-RFID): formato real y volcado de un tag original.
- [Savion/elegoo-rfid-editor](https://github.com/Savion/elegoo-rfid-editor): tablas de materiales y subtipos del firmware.
- [cjtallman/canvas-rfid-tool](https://github.com/cjtallman/canvas-rfid-tool): acabados, fecha en BCD y formato de comandos para NFC Tools.
- [AnySpool](https://www.anyspool.de/): inspiración del flujo con NFC Tools.

Proyecto no oficial, sin relación con Elegoo. Úsalo bajo tu responsabilidad.

---

Preparado por **[César Sala](https://www.linkedin.com/in/cesarsala)** · Hecho con ayuda de Claude.

---

## English

Free web tool that generates NFC tag data (NTAG213/215/216) so the **Elegoo Canvas** (Centauri Carbon) recognizes any filament spool. It runs entirely in your browser.

**To write a tag:** set up the spool, copy the commands, then in **NFC Tools** go to **Other → Advanced NFC commands**, paste them into the data field, send, and hold a blank NTAG213 sticker to your phone until the checkmark appears.

The printer only recognizes Elegoo's manufacturer code and its built-in material list. On the Centauri Carbon 1, the Canvas shows the material's default temperature range rather than the one stored on the tag. See the credits above for the community projects this is based on.
