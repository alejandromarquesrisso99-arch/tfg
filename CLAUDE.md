# TFG — Sincronización de reloj tolerante a fallos en redes TSN (gPTP)

Grado en Ingeniería Electrónica Industrial y Automática.

## Objetivo
Caracterizar gPTP (IEEE 802.1AS) sobre una red de switches TSN reales, evaluar su
comportamiento ante fallos y proponer mecanismos que mejoren su tolerancia a fallos
y su nivel de sincronismo con el reloj maestro.

- **A.** Estudiar gPTP y montar un prototipo (switches TSN + nodos Linux con linuxptp)
  para caracterizar el efecto de sus parámetros sobre la precisión.
- **B.** Campaña de inyección de fallos de parada, omisión y temporización
  (de más benignos a más malignos).
- **C.** Evaluar la tolerancia a fallos de gPTP, diseñar un mecanismo que la extienda
  y validarlo (tolerancia y prestaciones).

## Referencias base
1. IEEE Std 802.1AS-2020.
2. IEEE Std 802.1ASdm-2024 (Hot Standby, Clock Drift Error Reduction).
3. Gutiérrez et al., "Synchronization Quality of IEEE 802.1AS in Large-Scale Industrial Automation Networks", RTAS 2017.
4. Kyriakakis et al., "Fault-tolerant Clock Synchronization using PTP Multi-Domain Aggregation", ISORC 2021.
5. linuxptp (ptp4l, phc2sys): https://linuxptp.sourceforge.net

## Normas de uso de la IA (obligatorias, fijadas por la dirección del TFG)
La IA puede usarse para: complementar la búsqueda bibliográfica, asistir en la
programación, corregir textos y generar materiales complementarios (figuras).

- **El estudiante aporta siempre la primera versión** de textos e implementaciones.
  Claude revisa, corrige, sugiere y explica; no redacta capítulos ni escribe código
  de cero en su lugar.
- El estudiante supervisa todo resultado y debe poder explicar y justificar la ayuda recibida.
- Citar solo fuentes verificables; señalar explícitamente cualquier dato no verificado.
  No inventar referencias, DOIs ni resultados.
- Mantener registro de la ayuda recibida en `docs/registro-ia.md`.
