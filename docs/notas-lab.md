# Notas de laboratorio (resumen mantenido por Claude)

Resumen del estado del montaje y de los resultados obtenidos. Las notas propias del
estudiante están en `bitacora.md`.

## Montaje (estado a 17/11/26)
- Figura: `figuras/montaje-lab.{svg,pdf,png}`.
- Red de instrumentación: PC dell-i5 — Zyxel — switches 90/91 (10.0.0.90/91, web) y nodo16p.
- Red TSN: 90↔91, 90(PORT_4)↔`enp4s0f3`, 91(PORT_4)↔`enp3s0f3`. En el 90, PORT_2 va al 91.
- nodo16p: dos ptp4l independientes.
  - `gPTP3.cfg` + `enp3s0f3` → `/dev/ptp11`, tag `eth3`, cuelga del 91 (2 saltos al GM).
  - `gPTP4.cfg` + `enp4s0f3` → `/dev/ptp15`, tag `eth4`, cuelga del 90 (1 salto al GM).
- GM fijado por BMCA (priority1): switch 90 (`70f8e7.d04f.d90000`). Switch 91 = `…d60000`.
  Nodo con `priority1 255`. Pendiente: anotar valores exactos de priority1 de 90 y 91.

## Primeras observaciones de ptp4l (≈30 s, régimen estable)
| | eth3 (vía 91) | eth4 (directo 90) |
|---|---|---|
| rms offset | ~5–12 ns | ~3–6 ns |
| max | ~9–23 ns | ~4–11 ns |
| freq | ≈ +73 870 ppb | ≈ +69 400 ppb |
| path delay | ~202 ns | ~212 ns |

Convergencia tras arranque: 4–6 s. Offsets = estimación interna del esclavo, no error real.

## Path delay = 0 ns en el enlace 90–91
- Se mantiene a 0 con cable de 30 cm y de 10 m → no es un retardo pequeño real.
- Barrido de `Rx latency 1Gbps` en PORT_2 del switch 90 (Tx = 323 sin tocar):

| Rx (ns) | delay 10 m | delay 30 cm |
|---|---|---|
| 1690 | 0 | 0 |
| 1490 | 42–50 | 0–2 |
| 1290 | 142–150 | 94–102 |
| 1090 | 242–250 | 194–202 |
| 890 | 342–350 | 294–302 |
| 690 | 442–450 | 394–402 |

- Pendiente 0,5 en ambos casos (como predice la fórmula de Pdelay).
- Valor real extrapolado con 1690: ≈ −54 ns (10 m) y ≈ −102 ns (30 cm) → el switch recorta negativos a 0.
- Separación entre rectas ≈ 48 ns / 9,7 m ≈ 5 ns/m (velocidad típica del cobre).
- Sobrecompensación de las latencias de fábrica ≈ 104 ns sobre el delay (suma de las 4 latencias ≈ 208 ns).
  No se puede repartir entre Tx/Rx ni entre switches con este método (asimetría indetectable por PTP).
- Lecturas oscilan en pasos de 8 ns → posible reloj de timestamping a 125 MHz (no verificado).
- Rx restaurada a 1690 al terminar (confirmar).

## Pendientes
- Saber si el switch usa internamente el valor negativo o el 0 recortado (comparar PHC del nodo / preguntar a SOC-E).
- Preguntar a SOC-E el origen de las latencias 323/1690 ns, one-step, versión 802.1AS, persistencia de la config.
- Gráfica de los barridos y tabla para la memoria (estudiante).

## Captura de 15 min, una muestra por Sync (19/11/26)
- `summary_interval -3` en `[global]` de gPTP3/4.cfg → una línea por Sync (8/s). Se comentó `gmCapable` (opción no reconocida por la versión de ptp4l usada).
- CSV generados con awk (`tiempo;offset;freq;delay`) y analizados en LibreOffice Calc.
- Observación cualitativa: el offset de eth4 (directo al GM) tiene bastante menos ruido que el de eth3 (vía switch 91).
  Pendiente: cuantificar (desviación típica, máx. absoluto) y comprobar el alineamiento temporal de las dos series.
