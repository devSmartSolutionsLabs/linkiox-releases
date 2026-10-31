# RAIDI8 — firmware 1.0.0

- Modelo: **RAIDI8**
- SKU: `LXP-RD8`
- HW: `A`
- Gama: **Compact**
- Fecha: 2026-10-31
- Tamaño: `1221360` bytes
- SHA-256: `df49ce96bc031fc0a542467816807036045d151ccea04cd1638a483c8f7024cd`
- Firma: ECDSA P-256, key_id `c3f4154e093a2cb4`
- Tag: `raidi8-v1.0.0`

## Notas

RAIDI8 1.0.0 - gama Compact. Primera release oficial: LXP 1.0, OTA A/B firmada con anti-rollback, Modbus RS485, auditoria EEPROM/SD, HMI, programas VM y PWM por salida.

## Instalación

La app LINKIOX Ops descarga este binario desde `index.json` y lo instala por
OTA A/B. El equipo verifica SKU, HW, tamaño, SHA-256 y firma antes de activar
la partición nueva.
