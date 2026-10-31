# LINKIOX — canal de firmware

Repositorio **público** con el índice de versiones y los binarios de firmware
que consume **LINKIOX Ops** para actualizar los equipos por OTA.

El código fuente y el pipeline de compilación viven en el repositorio privado
`devSmartSolutionsLabs/linkiox-firmware`.

## Gama Compact

Solo se publican los dos modelos de la **gama Compact**:

| Modelo | SKU | Nombre comercial | MCU | Flash | HW | Última |
|---|---|---|---|---|---|---|
| RAIDI8 | `LXP-RD8` | SmartController RAIDI8 | ESP32-S3 | 8 MB | A | **1.0.0** |
| R5I3 | `LXP-R5I3` | R5I3 | ESP32 | 4 MB | A | **1.0.0** |

> La gama **Modular** (PLC + módulos IO) es un desarrollo posterior. No hay
> modelos ni binarios publicados de esa gama todavía.

Cada placa tiene **su propio SKU**. La app solo ofrece releases cuyo `sku`
coincide exactamente con el del equipo; además el firmware verifica la firma
ECDSA contra su propio `sku` y `hw_rev`, así que un binario de otro modelo no
puede instalarse aunque se fuerce desde fuera.

## Estructura

```text
index.json                              índice de releases (lo consulta la app)
raidi8/1.0.0/raidi8-1.0.0.bin           binario RAIDI8
raidi8/1.0.0/RELEASE.md                 notas de la versión
r5i3/1.0.0/r5i3-1.0.0.bin               binario R5I3
r5i3/1.0.0/RELEASE.md                   notas de la versión
```

La URL por defecto en la app es:

```text
https://raw.githubusercontent.com/devSmartSolutionsLabs/linkiox-releases/main/index.json
```

## Versión 1.0.0 — primera release oficial

Esta es la **primera release de la gama Compact**. Se reinició el canal para
eliminar versiones de desarrollo y la placa de banco S3-DEV, que no es un
producto.

### RAIDI8

- SKU `LXP-RD8`, HW A, firmware `1.0.0`.
- 8 DI + 8 DO + 8 AI, RTC DS3231, EEPROM 24C512, microSD, RS485/Modbus.
- LXP 1.0: HTTP, WebSocket, descubrimiento, HMI, programas VM, auditoría.
- OTA A/B firmada.
- PWM por salida declarada en manifiesto.

### R5I3

- SKU `LXP-R5I3`, HW A, firmware `1.0.0`.
- 5 relés, 3 entradas opto, RTC DS3231, buzzer PWM.
- Sin EEPROM ni SD: el registro crítico persistente no está disponible.
- LXP 1.0 por Wi-Fi y UART (CP210x).
- OTA A/B firmada.

## Tags

```text
raidi8-v1.0.0
r5i3-v1.0.0
```

## Formato de `index.json` (schema 2)

```json
{
  "schema": 2,
  "updated": "2026-10-31",
  "releases": [
    {
      "sku": "LXP-RD8",
      "model": "RAIDI8",
      "name": "SmartController RAIDI8",
      "hw_rev": "A",
      "version": "1.0.0",
      "released": "2026-10-31",
      "size": 1221152,
      "sha256": "...",
      "url": "https://raw.githubusercontent.com/.../raidi8/1.0.0/raidi8-1.0.0.bin",
      "notes": "...",
      "mandatory": false,
      "sig": "...",
      "key_id": "c3f4154e093a2cb4",
      "signed": true
    }
  ],
  "models": [
    { "sku": "LXP-RD8", "model": "RAIDI8", "latest": "1.0.0", "releases": 1 }
  ]
}
```

## Firma

Los binarios se firman con **ECDSA P-256** sobre:

```text
LXP1|<sku>|hw_rev|version|size|sha256
```

La app instala solo releases firmadas con la clave `c3f4154e093a2cb4`.
