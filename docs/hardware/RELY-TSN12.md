# SOC-E RELY-TSN12 — datos relevantes para el TFG

Fuente: `RELY-TSN12_datasheet.pdf` (hoja comercial de SOC-E, ref. TSN32.05).

- Puertos: 8x 10/100/1000BASE-T TSN, 4x 100/1000BASE-SFP TSN, 1x puerto de servicio (gestión fuera de banda).
- **1x salida PPS (conector SMA)** → útil para medir el error de sincronización del switch con osciloscopio/contador.
- Plataforma: Xilinx Zynq UltraScale+ MPSoC (4x A53, 2x R5F, FPGA 16 nm); funciones TSN implementadas en FPGA.
- Estándares TSN: 802.1AS, 802.1Qav (CBS), 802.1Qbv (TAS), 802.1Qci (PSFP), 802.1CB (FRER), 802.1Qcc.
- Gestión: web HTTPS, CLI por SSH, NETCONF/YANG, Syslog, estadísticas por puerto, port mirroring de entrada/salida.

## Lo que la hoja de datos NO dice (hay que buscarlo en el manual de usuario o preguntar a SOC-E)
- Si soporta **one-step** (Sync y/o Pdelay) y cómo se configura.
- Versión de 802.1AS (2011 o 2020), soporte multidominio / Hot Standby (802.1ASdm).
- Parámetros configurables de gPTP (intervalos, prioridades BMCA, timeouts) y su servo.
- Si el PPS sale del reloj sincronizado con el GM y con qué precisión/retardo fijo.
