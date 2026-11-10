# LINKIOX Releases — canal OTA

Canal público de firmware firmado para los equipos LINKIOX.

- **Repositorio:** `devSmartSolutionsLabs/linkiox-releases`
- **Índice:** [`index.json`](index.json)
- **Firma:** ECDSA P-256 (`key_id` `c3f4154e093a2cb4`)
- **Esquema del índice:** 2
- **Última actualización:** 2026-11-10

> Este repositorio contiene **solo publicaciones**. El código fuente vive en
> `linkiox-firmware`, `linkiox-app-ops` y `linkiox-app-blocks`.

## Versión actual

| SKU | Modelo | Versión | Fecha | Estado | Notas |
|---|---|---|---|---|---|
| `LXP-RD8` | RAIDI8 | **1.3.0** | 2026-11-10 | Estable | MCPWM completo + capture |

En esta publicación **solo se incluye RAIDI8**. La versión 1.3.0 incorpora
MCPWM avanzado, una capacidad específica del ESP32‑S3 que RAIDI8 usa. R5I3 no
se publica en esta versión.

## Novedades de 1.3.0

### MCPWM completo

- 2 grupos, 6 canales lógicos (3 timers/operadores por grupo).
- 2 generadores por operador: salida simple o complementaria A/B.
- Dead time de subida y bajada configurable en nanosegundos.
- Polaridad e inversión por salida.
- Modos de conteo ascendente, descendente y ascendente/descendente.
- 2 comparadores por operador, con acción `low`, `high` o `toggle`.
- Carrier: frecuencia, duty, primer pulso e inversión antes/después.
- Fault/brake: GPIO, activo bajo, CBC/OST y recuperación en TEZ/TEP.
- Sync por GPIO: flanco, count ticks y dirección.
- Actualización de periodo, comparador, generador y dead time en TEZ/TEP/sync.
- Capture de 6 canales con flanco de subida/bajada, inversión y prescale.
- Diagnóstico en `GET /lxp/v1/hw`.
- Acción `hw.mcpwm.recover` para recuperar faults OST/latched.
- Al salir de RUN o cambiar de programa, los canales se apagan y se liberan.

### Bloques de App Blocks

- **Generador MCPWM** con panel avanzado completo.
- **Captura MCPWM**.
- Requiere App Blocks **0.30.0+46** o superior.

### Restricciones de hardware

- GPIO seguros en RAIDI8: `GP1=38`, `GP2=47`, `GP4=21`.
- `GP3=48` y `GP5=14` comparten microSD; solo se aceptan con `allow_sd`.
- Un par complementario consume 2 GPIO.
- El carrier comparte reloj con el grupo. Con `resolution_hz=1 MHz`, el carrier
  debe estar aproximadamente entre 1.25 MHz y 20 MHz. El valor por defecto es
  2 MHz.

## Instalación

### Desde Ops (recomendado)

1. Abre **Equipo → Actualización de firmware**.
2. Pulsa **Buscar actualizaciones**.
3. Selecciona **1.3.0** para RAIDI8.
4. Ops verifica `sha256` y la firma del índice.
5. Instala y deja que el equipo reinicie.

### Desde Factory

El release incluye los artefactos de línea:

- `raidi8-1.3.0.bin` — firmware para OTA.
- `bootloader.bin` — bootloader.
- `partition-table.bin` — tabla de particiones.
- `ota_data_initial.bin` — estado inicial de OTA.
- `secure_boot_pubkey.pem` — clave **pública** de Secure Boot.

La clave privada de Secure Boot **no** se publica.

## Integridad y seguridad

Cada entrada de `index.json` incluye:

- `sku`, `model`, `hw_rev`, `version`
- `size`, `sha256`
- `url` del asset
- `sig` y `key_id` de la entrada
- `signature` y `key_id` del índice completo

El equipo rechaza firmware con SHA‑256 incorrecto, tamaño distinto o firma
inválida. El índice también va firmado para impedir que se alteren versiones,
URLs o hashes.

## Historial del canal

Este canal se reinició el **2026-11-10** con una publicación limpia.

| Fecha | SKU | Versión | Cambios |
|---|---|---|---|
| 2026-11-10 | `LXP-RD8` | 1.3.0 | MCPWM completo, capture, diagnóstico `/hw` |

Las versiones anteriores del canal (`1.2.3` a `1.2.7`) fueron retiradas para
empezar una línea de publicación limpia.

## Cómo publicar una nueva versión

1. Compilar el perfil:
   ```bash
   LINKIOX_PROFILE=raidi8 ./tools/build.sh
   ```
2. Generar el índice y el binario:
   ```bash
   python tools/make_release.py --profile raidi8 --version X.Y.Z \
     --bin build_raidi8/linkiox-fw.bin \
     --notes "Cambios de la versión" \
     --out-repo ../linkiox-releases --url-style release
   ```
3. Crear el release y subir los assets:
   ```bash
   python tools/make_github_release.py --profile raidi8 --version X.Y.Z
   ```
4. Actualizar la tabla **Versión actual**, **Novedades** e **Historial** de este
   README.
5. Hacer commit y push del `index.json` y del `README.md`.

## Contacto

Soporte: LINKIOX / Smart Solutions Labs.
