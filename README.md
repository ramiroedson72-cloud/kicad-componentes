# Librería de componentes para KiCad 10

Librería personal de símbolos, footprints y modelos 3D para KiCad 10.
Nickname de la librería: **SnapEDA** (por su origen; hoy también incluye partes hechas a mano).

## Contenido

| Símbolo | Footprint | Encapsulado | 3D |
|---|---|---|---|
| `ESP32-C3_SUPERMINI_TH` | `MODULE_ESP32-C3_SUPERMINI_TH` | módulo THT | ✔ |
| `ESP32-DEVKITC-32` | `MODULE_ESP32-DEVKITC-32` | módulo THT 2×19, paso 2.54 | ✔ |
| `TPM408-2.8_ILI9341` | `TPM408-2.8_ILI9341` | TFT 2.8" 240×320, 14+4 pines | — |
| `MC00100` | `CONV_MC00100` | buck LM2596, 43×21 mm | ✔ |
| `SY8205FCC` | `SOIC-8_L5.0-W4.0-P1.27-LS6.0-BL-EP2.0` | SOIC-8 con pad térmico | ✔ |
| `JW5068A` | `QFN-20_L3.0-W3.0-P0.45-TL-EP` | VQFN-20 3×3 | ✔ |
| `WAGO_236-402` | `TerminalBlock_WAGO_236-402_1x02_P5.00mm_45Degree` | clema 2 polos | ✔ |
| `WH148-1A-2-18T-L20` | `TRIM_WH148-1A-2-18T-L20` | potenciómetro rotativo 16 mm | ✔ |
| `PPTC041LFBN-RC` | `SULLINS_PPTC041LFBN-RC` | header hembra 1×4, 2.54 mm | ✔ |
| `FGH60N60SFD` | `TO-247-3_Vertical` | IGBT TO-247 | ✔ |
| `IRF3205SPBF` | `TRANS_IRF3205SPBF` | MOSFET N 55 V 110 A, D2PAK | ✔ |
| `MMBT3904_215` | `TRANS_MMBT3904_215` | NPN SOT-23 | ✔ (de KiCad) |
| `VOLTMETER_0V28` | `VOLTMETER_0V28_2381AS` | voltímetro LED 0.28" 0–100 V, 3 hilos | ✔ |

Hay dos footprints sin símbolo propio en esta librería:

- `NHD-0420H1Z` — LCD 4×20. Su símbolo está en la librería de fábrica de KiCad.
- `Mini560_Pads_2x14.8mm` — solo el patrón de pads del módulo buck Mini560.

## Cómo usarla

Clona el repo y registra la librería en KiCad:

**Preferences → Manage Symbol Libraries → Global**

| Nickname | Library Path |
|---|---|
| `SnapEDA` | `<ruta-del-repo>/SnapEDA.kicad_sym` |

**Preferences → Manage Footprint Libraries → Global**

| Nickname | Library Path |
|---|---|
| `SnapEDA` | `<ruta-del-repo>/SnapEDA.pretty` |

El nickname tiene que ser exactamente `SnapEDA`: los símbolos apuntan a su footprint
como `SnapEDA:<nombre>`.

## ⚠️ Rutas de los modelos 3D

Los footprints referencian los modelos 3D con **ruta absoluta** de la máquina donde se
armó la librería:

```
C:/Users/Ramiro/Documents/KiCad/10.0/SnapEDA/3dmodels/<archivo>.step
```

Si clonas el repo en otro lado, los 3D no van a cargar hasta que ajustes esas rutas.
La forma limpia de arreglarlo es definir una variable de entorno en KiCad
(**Preferences → Configure Paths**), por ejemplo `KICAD_SNAPEDA_DIR` apuntando a la
carpeta del repo, y reemplazar el prefijo absoluto por `${KICAD_SNAPEDA_DIR}` en los
`.kicad_mod`.

El footprint `TRANS_MMBT3904_215` es la excepción: usa el `SOT-23.step` que viene con
KiCad, así que su ruta apunta a la instalación de KiCad, no a este repo.

## Origen de los archivos y términos de uso

Esto es una librería personal. Los archivos no son todos míos:

- La mayoría de los símbolos y footprints se descargaron de **[SnapEDA / SnapMagic](https://www.snapeda.com)**
  y están sujetos a sus términos de uso.
- Varios modelos 3D son de los **fabricantes** (Infineon, Nexperia, Espressif, WAGO,
  Sullins, Newhaven) o del repositorio oficial
  [kicad-packages3D](https://gitlab.com/kicad/libraries/kicad-packages3D).
- El footprint `TO-247-3_Vertical` y el modelo `SOT-23.step` vienen de las librerías de
  fábrica de KiCad (CC-BY-SA 4.0 con excepción de librerías).
- El símbolo, footprint y las cotas del **voltímetro `VOLTMETER_0V28`** los hice yo,
  midiendo la geometría de un modelo STEP publicado en
  [GrabCAD](https://grabcad.com/library/voltmeter-2-8-inch-display-dc-0v-100v-3-wire-1)
  por el usuario "J S". El módulo físico es genérico (marcado `2381AS-1`).

Si eres el titular de alguno de estos archivos y no quieres que estén aquí, abre un
issue y los quito.

## Nota sobre el voltímetro 0.28"

Las fichas de los vendedores de este módulo se contradicen entre sí. Las cotas de este
footprint salieron de medir la geometría del STEP, no de copiar una ficha:

- **31.64 × 10.50 × 8.50 mm** (display 5.5 + PCB 1.5 + componentes atrás 1.5)
- Barrenos de montaje **Ø2.10 mm** (solo pasa M2) a **26.50 mm** entre centros
- 3 cables Ø1.28 mm a paso 2.00 mm, salen por la cara trasera
- Orden de los cables: **amarillo (VIN) → rojo (VCC+) → negro (GND)**
- Ventana del display 22.60 × 10.00 mm

La ficha de UNIT Electronics dice 25 × 15 × 10 mm, y está mal.
El footprint asume montaje con **separadores M2 de 6 mm** (offset Z de 14.5 mm en el 3D).
