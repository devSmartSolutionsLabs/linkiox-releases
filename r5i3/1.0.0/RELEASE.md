# R5I3 — firmware 1.0.0

- Modelo: **R5I3**
- SKU: `LXP-R5I3`
- HW: `A`
- Gama: **Compact**
- Fecha: 2026-10-31
- Tamaño: `1102768` bytes
- SHA-256: `26ccbeeac13f8ac44b5590bc926813e3e32823a646935043e3466b6747655cf9`
- Firma: ECDSA P-256, key_id `c3f4154e093a2cb4`
- Tag: `r5i3-v1.0.0`

## Notas

R5I3 1.0.0 - gama Compact. Primera release oficial: 5 reles, 3 DI opto, RTC DS3231, buzzer PWM, OTA A/B firmada con anti-rollback, LXP por Wi-Fi/UART y programas VM.

## Instalación

La app LINKIOX Ops descarga este binario desde `index.json` y lo instala por
OTA A/B. El equipo verifica SKU, HW, tamaño, SHA-256 y firma antes de activar
la partición nueva.
