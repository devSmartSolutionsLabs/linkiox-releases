# LINKIOX Releases — canal OTA

Canal público de firmware firmado para los equipos LINKIOX.

- **Repositorio:** `devSmartSolutionsLabs/linkiox-releases`
- **Índice:** [`index.json`](index.json)
- **Firma:** ECDSA P-256 (`key_id` `c3f4154e093a2cb4`)
- **Esquema del índice:** 2
- **Última actualización:** 2026-11-10

> Este repositorio contiene **solo publicaciones**. El código fuente vive en
> `linkiox-firmware`, `linkiox-app-ops` y `linkiox-app-blocks`.

## Modelos LINKIOX

| Característica | **RAIDI8** | **R5I3** |
|---|---|---|
| SKU | `LXP-RD8` | `LXP-R5I3` |
| Gama | Compact | Compact |
| MCU | ESP32‑S3 | ESP32 clásico |
| Flash / PSRAM | 8 MB / 8 MB | 4 MB / sin PSRAM |
| Entradas digitales | 8 optoacopladas, 2.5 kVrms | 3 optoacopladas, 2.5 kVrms |
| Salidas de relé | 8, 250 VAC 10 A | 5, 250 VAC 10 A |
| Salida especial | — | Buzzer por MOSFET |
| Entradas analógicas | 8, 0–5 V, 16 bits | — |
| GPIO directos | 5 (`GP1…GP5`) | — |
| RTC | DS3231 | DS3231 |
| EEPROM | 24C512 | — |
| microSD | Sí, SPI | — |
| RS485 / Modbus RTU | Sí | — |
| USB | USB Serial/JTAG nativo | CP210x UART0 |
| Wi‑Fi | 2.4 GHz STA/AP | 2.4 GHz STA/AP |
| MCPWM | 6 canales + capture | — |
| PWM LEDC | — | Buzzer `do.5` |
| OTA A/B firmada | Sí | Sí |
| Secure Boot | Sí | Sí |
| Programas firmados | Sí | Sí |
| Log crítico persistente | EEPROM | No |
| Datalogger diario | microSD | No |

### RAIDI8

Placa de control y adquisición para procesos con muchas E/S.

**Hardware**

- ESP32‑S3, 8 MB flash y 8 MB PSRAM.
- 8 relés `OUT0…OUT7`, 250 VAC 10 A.
- 8 entradas digitales `DI0…DI7` optoacopladas, activas en bajo, pull‑up,
  debounce 20 ms, aislamiento 2.5 kVrms.
- 8 entradas analógicas `AI0…AI7`, 0–5 V, 16 bits, mediante 2× ADS1115.
- 5 GPIO directos:
  - `GP1 = GPIO38`
  - `GP2 = GPIO47`
  - `GP3 = GPIO48` (comparte card‑detect de microSD)
  - `GP4 = GPIO21`
  - `GP5 = GPIO14` (CS alternativo de microSD)
- DS3231 por I²C (`SDA=5`, `SCL=4`).
- EEPROM 24C512 (`0x50`) para log crítico.
- microSD por SPI (`SCK=12`, `MOSI=11`, `MISO=13`, `CS=10`, CD=48).
- 2× MCP23017 (`0x20`, `0x21`) para relés, DI y señales internas.
- 2× ADS1115 (`0x48`, `0x49`).
- RS485 Modbus RTU esclavo:
  - UART1, `TX=17`, `RX=18`.
  - DE/RE en MCP#2 GPB0.
  - Mapa: coils = DO, discrete = DI, input regs = AI, holding = setpoints.
- USB Serial/JTAG nativo.
- Wi‑Fi 2.4 GHz STA/APSTA.
- MCPWM del ESP32‑S3:
  - 2 grupos, 6 canales lógicos.
  - 2 generadores por canal (simple o complementario A/B).
  - Hasta 12 salidas PWM a nivel de periférico; RAIDI8 expone 5 GPIO.
  - 3 entradas de fault y 3 de sync por grupo.
  - Dead time, polaridad, carrier, fault/brake, sync, 2 comparadores.
  - Modos de conteo up/down/up‑down.
  - Reloj base de 160 MHz y resolución de hasta 6.25 ns.
  - Timer de 16 bits con resolución automática para ajustar el rango.
  - 6 canales de capture con timer de 32 bits a 80 MHz (12.5 ns).
- Alimentación y protecciones según placa; los relés requieren carga y
  aislamiento adecuados.

**Uso típico**

Automatización de máquinas, tableros con muchos relés, lectura de sensores
analógicos, datalogger, Modbus RTU, control de válvulas, variadores y
generación de pulsos.

### R5I3

Placa compacta para máquinas pequeñas y control de campo.

**Hardware**

- ESP32 clásico, 4 MB flash, sin PSRAM.
- 5 relés:
  - `R1 = GPIO13` → `do.0`
  - `R2 = GPIO12` → `do.1`
  - `R3 = GPIO14` → `do.2`
  - `R4 = GPIO27` → `do.3`
  - `R5 = GPIO26` → `do.4`
- Buzzer por MOSFET:
  - `GPIO25` → `do.5`
  - PWM por LEDC, 100–20 000 Hz, por defecto 2 700 Hz / 50 %.
- 3 entradas optoacopladas:
  - `DI1 = GPIO15` → `di.0`
  - `DI2 = GPIO2` → `di.1`
  - `DI3 = GPIO4` → `di.2`
  - Activas en bajo, pull‑up, debounce 20 ms, aislamiento 2.5 kVrms.
- DS3231 por I²C (`SDA=21`, `SCL=22`).
- Sin MCP23017, ADS1115, EEPROM ni microSD.
- Sin entradas analógicas.
- Sin RS485/Modbus.
- USB mediante CP210x sobre UART0 (`115200 8N1`).
- Wi‑Fi 2.4 GHz STA/APSTA.
- SoftAP de fábrica: `LINKIOX-R5I3-SETUP` / `linkiox123` (192.168.4.1).
- OTA A/B firmada.
- Sin MCPWM; el único PWM es el buzzer por LEDC.

**Avisos de hardware**

- `GPIO12` (R2) es strapping de flash: añadir pull‑down externo.
- `GPIO2` (DI2) y `GPIO15` (DI1) son strapping: las entradas opto deben quedar
  inactivas al arrancar.
- `GPIO25` (buzzer): el MOSFET debe tener pull‑down en la compuerta.
- Los relés necesitan driver/aislamiento; el GPIO no alimenta la bobina.
- En ESP32 el bootloader va en `0x1000`, no en `0x0`.

**Uso típico**

Control de bombas, ventiladores, compuertas, alarmas locales, máquinas
compactas y tableros de pocas E/S.

## Versiones publicadas

| SKU | Modelo | Versión | Fecha | Estado | Notas |
|---|---|---|---|---|---|
| `LXP-RD8` | RAIDI8 | **1.4.3** | 2026-11-11 | Estable | Reanuda programa tras OTA + rollback |
| `LXP-R5I3` | R5I3 | **1.4.3** | 2026-11-11 | Estable | Reanuda programa tras OTA; sin MCPWM |

### Novedades de 1.4.3

- Tras una OTA o un rollback, si el equipo estaba en RUN, vuelve a cargar el
  programa activo y entra en RUN automáticamente.
- Si el blob NVS del programa falta o está ilegible, se recupera desde el
  repositorio de programas.
- Un arranque normal por corte de luz sigue entrando en STOP por seguridad.

### Novedades de 1.4.2

- **Rollback OTA manual**: acción `ota.rollback` con PIN de ingeniería.
- Valida la imagen de la otra partición y la marca como arranque.
- `/info` informa `running_partition` (`ota_0` u `ota_1`).
- Ops 0.11.25+2070 añade el botón **Hacer rollback**.

### Novedades de 1.4.1

- **WSS con verificación TLS**: el cliente WebSocket valida el certificado del
  servidor contra el bundle de CAs del firmware.
- Un certificado autofirmado o no confiable se rechaza.
- Sin cambios de formato: los programas de 1.4.0 siguen siendo válidos.
- Verificado en RAIDI8 con un WSS público válido y con un WSS autofirmado
  rechazado.

### Novedades de 1.4.0

Comunes a ambos modelos:

- **Telemetría WebSocket**: conexiones `ws://` y `wss://` además de
  HTTP/HTTPS.
- Conexión persistente con reconexión en la siguiente publicación.
- Cabeceras personalizadas en el handshake.
- Estado en `GET /lxp/v1/comm` con `transport` y `ws_connected`.
- Se mantiene el envío por intervalo y/o cambio, y la tarea sigue fuera del
  scan esperando Wi-Fi.

Exclusivo de **RAIDI8**:

- MCPWM completo y capture de 1.3.0.

### Novedades de 1.3.1

Comunes a ambos modelos:

- **Telemetría HTTP/HTTPS**:
  - conexiones configurables,
  - publicadores por intervalo y/o cambio,
  - variables DI/DO/AI/tags,
  - JSON con `device`, `ts` y `vars`,
  - cabeceras HTTP,
  - reintentos y estado en `GET /lxp/v1/comm`.
- El envío corre fuera del scan: un servidor lento no bloquea la VM.
- Si el equipo no tiene Wi-Fi, la tarea espera y no envía.

Exclusivo de **RAIDI8**:

- MCPWM completo y capture de 1.3.0.

### Novedades de 1.3.0

Comunes a ambos modelos:

- VM v2, Ladder/FBD/ST y bloques actualizados.
- Seguridad de usuarios, roles, TOTP y auditoría.
- OTA A/B firmada.
- Diagnóstico y eventos.

Exclusivas de **RAIDI8**:

- MCPWM completo:
  - 2 grupos, 6 canales.
  - Salida simple o complementaria A/B.
  - Dead time de subida/bajada.
  - Polaridad e inversión.
  - Conteo up/down/up‑down.
  - 2 comparadores por operador.
  - Carrier, fault/brake y sync.
  - Actualización en TEZ/TEP/sync.
  - Matriz de eventos personalizada por generador (timer/cmp/fault).
  - Capture de 6 canales.
  - Diagnóstico `GET /lxp/v1/hw`.
  - Acción `hw.mcpwm.recover`.
- Bloques **Generador MCPWM** y **Captura MCPWM** en App Blocks.

**R5I3 1.3.0 no incluye MCPWM** porque su hardware no expone GPIOs MCPWM; su PWM
es el buzzer por LEDC. El resto de la base de firmware es común.

**Restricciones MCPWM en RAIDI8**

- GPIO seguros: `GP1=38`, `GP2=47`, `GP4=21`.
- `GP3=48` y `GP5=14` comparten microSD; requieren `allow_sd`.
- Un par complementario consume 2 GPIO.
- Timer de 16 bits. Con `1 Hz…50 kHz`, la resolución automática es `100 kHz` y
  la frecuencia mínima efectiva queda en **2 Hz**. Para `1 Hz`, baja
  `freq_max_hz` o fija una `resolution_hz` menor.
- Carrier: comparte reloj con el grupo. Con `resolution_hz=1 MHz`, debe estar
  aproximadamente entre **1.25 MHz y 20 MHz**; por defecto 2 MHz.
- El **ESP32‑S3 no tiene matriz ETM** ni trigger ETM de ADC. Esas funciones
  existen en otros chips (C5/C6/P4), no en RAIDI8. Por eso no se exponen.
- El generador MCPWM solo puede forzar HIGH, LOW o TOGGLE; no hay estado
  high‑Z por hardware en la salida PWM.

Requiere App Blocks **0.32.0+48** para el transporte WebSocket y
**0.31.0+47** para el bloque de telemetría HTTP. Para `wss://` seguro se
recomienda firmware **1.4.1+**.

## Capacidades de firmware

### Programación

- VM v2 determinista.
- Ladder, FBD y Structured Text.
- 64 variables, 256 instrucciones, programa de hasta 4 KB.
- Scan configurable de 20 a 5 000 ms.
- 20 000 instrucciones por scan.
- Variables `BOOL`, `INT`, `DINT`, `TIME`, `REAL`.
- Variables `RETAIN` persistentes.
- Temporizadores, contadores, flancos, biestables, PID, filtros, escalado,
  alarmas, horarios y comparadores.
- Tabla de tags y monitor en vivo.
- Programas firmados con ECDSA P-256 y anti‑replay.
- Bytecode binario LXBC firmado.

### Seguridad

- Usuarios y roles reales del firmware:
  `operator`, `technician`, `supervisor`, `engineer`, `admin`, `auditor`,
  `service`, `owner`.
- TOTP para admin/auditor/service.
- Bloqueo por intentos fallidos.
- Auditoría de acciones.
- Clave del dueño y grants temporales por QR.
- Secure Boot.
- OTA firmada.
- Watchdog y estado seguro de salidas.
- Sesión de administrador con caducidad.

### Conectividad

- HTTP y WebSocket.
- mDNS.
- SoftAP de recuperación.
- USB nativo en RAIDI8.
- UART0/CP210x en R5I3.
- Modbus RTU esclavo solo en RAIDI8.
- Descubrimiento UDP.

### Telemetría HTTP / WebSocket

- Conexiones HTTP/HTTPS y `ws://`/`wss://`.
- `wss://` valida el certificado del servidor con el bundle de CAs.
- Publicadores con intervalo y/o envío por cambio.
- Variables: DI, DO, AI, tags y variables internas.
- JSON:

```json
{"device":"LXP-RD8-26A-00001","ts":1794354113000,
 "vars":{"D1":false,"AI1":1,"OUT1":false}}
```

- Cabeceras personalizadas.
- Reintento y reconexión automáticos.
- Estado en `GET /lxp/v1/comm` con `transport` y `ws_connected`.
- Configuración firmada dentro del programa (`comm`).
- La tarea de red espera a que haya Wi-Fi y nunca bloquea el scan.

### Registro y diagnóstico

- Eventos de equipo interpretados en Ops.
- Log crítico persistente en RAIDI8 (EEPROM).
- Datalogger CSV diario en RAIDI8 (microSD).
- Métricas de scan, overruns, RAM y diagnóstico.
- Estado de E/S, RTC, Wi‑Fi y hardware.

### Apps

- **Ops**: operación, usuarios, dueño, OTA, bitácora, herramientas de campo.
- **Blocks**: programación FBD/Ladder/ST, tags, firma, USB/Wi‑Fi.
- **Factory**: aprovisionamiento, calibración y línea de producción.
- **Web/HMI**: dashboard local y HMI.

## Instalación

### OTA desde Ops

1. Abre **Equipo → Actualización de firmware**.
2. Pulsa **Buscar actualizaciones**.
3. Selecciona la versión del modelo.
4. Ops verifica `sha256` y firma del índice.
5. Instala y deja reiniciar.

### Desde Factory

Cada release incluye:

- `<modelo>-<versión>.bin` — firmware OTA.
- `bootloader.bin`.
- `partition-table.bin`.
- `ota_data_initial.bin`.
- `secure_boot_pubkey.pem` — clave **pública**.

La clave privada de Secure Boot no se publica.

## Integridad y seguridad

Cada entrada de `index.json` incluye:

- `sku`, `model`, `hw_rev`, `version`.
- `size`, `sha256`.
- `url` del asset.
- `sig` y `key_id` de la entrada.
- `signature` y `key_id` del índice completo.

El equipo rechaza firmware con SHA‑256 incorrecto, tamaño distinto o firma
inválida. El índice también va firmado para impedir alteraciones de versión,
URL o hash.

## Historial del canal

Este canal se reinició el **2026-11-10** con una publicación limpia.

| Fecha | SKU | Versión | Cambios |
|---|---|---|---|
| 2026-11-11 | `LXP-RD8` | 1.4.3 | Reanuda programa en RUN tras OTA/rollback |
| 2026-11-11 | `LXP-R5I3` | 1.4.3 | Reanuda programa tras OTA; sin MCPWM |
| 2026-11-11 | `LXP-RD8` | 1.4.2 | Rollback OTA manual; WSS/TLS |
| 2026-11-11 | `LXP-R5I3` | 1.4.2 | Rollback OTA manual; sin MCPWM |
| 2026-11-10 | `LXP-RD8` | 1.4.1 | WSS con verificación TLS; telemetría HTTP/WS |
| 2026-11-10 | `LXP-R5I3` | 1.4.1 | WSS con verificación TLS; sin MCPWM |
| 2026-11-10 | `LXP-RD8` | 1.4.0 | Telemetría WebSocket, MCPWM completo y capture |
| 2026-11-10 | `LXP-R5I3` | 1.4.0 | Telemetría WebSocket; sin MCPWM |
| 2026-11-10 | `LXP-RD8` | 1.3.1 | Telemetría HTTP, MCPWM completo y capture |
| 2026-11-10 | `LXP-R5I3` | 1.3.1 | Telemetría HTTP; sin MCPWM |
| 2026-11-10 | `LXP-RD8` | 1.3.0 | MCPWM completo, capture, diagnóstico `/hw` |
| 2026-11-10 | `LXP-R5I3` | 1.3.0 | Base común de firmware; sin MCPWM |

Las versiones anteriores (`1.2.3` a `1.2.7`) fueron retiradas al reiniciar el
canal.

## Cómo publicar una nueva versión

1. Compilar el perfil:
   ```bash
   LINKIOX_PROFILE=raidi8 ./tools/build.sh
   LINKIOX_PROFILE=r5i3   ./tools/build.sh
   ```
2. Generar el índice y el binario:
   ```bash
   python tools/make_release.py --profile raidi8 --version X.Y.Z \
     --bin build_raidi8/linkiox-fw.bin \
     --notes "Cambios de la versión" \
     --out-repo ../linkiox-releases --url-style release
   ```
3. Crear el release y subir assets:
   ```bash
   python tools/make_github_release.py --profile raidi8 --version X.Y.Z
   ```
4. Actualizar en este README:
   - tabla **Versiones publicadas**,
   - **Novedades**,
   - **Historial del canal**,
   - capacidades del modelo si cambiaron.
5. Commit y push de `index.json` y `README.md`.

## Contacto

Soporte: LINKIOX / Smart Solutions Labs.
