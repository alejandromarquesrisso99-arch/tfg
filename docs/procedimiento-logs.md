# Procedimiento: captura de logs de ptp4l (eth3 y eth4)

Requisitos previos (una sola vez):
- En `[global]` de `gPTP3.cfg` y `gPTP4.cfg`: `summary_interval -3` (una línea por Sync).
- Rx latency de los switches en su valor de referencia (1690) y GM = switch 90.

Cambiar `NOMBRE` por un identificador de la prueba (ej. `p1_30cm`). Usar el mismo en todos los pasos.

## 1. Arrancar (PC de instrumentación, dos terminales)

Terminal 1:
```bash
ssh nodo16p
```
```bash
cd ~/tfg_alejandro
```
```bash
sudo ptp4l -f gPTP3.cfg -i enp3s0f3 -m 2>&1 | tee NOMBRE_eth3.log
```

Terminal 2:
```bash
ssh nodo16p
```
```bash
cd ~/tfg_alejandro
```
```bash
sudo ptp4l -f gPTP4.cfg -i enp4s0f3 -m 2>&1 | tee NOMBRE_eth4.log
```

Comprobar que salen ~8 líneas/s con `master offset ... s2`. Anotar hora de inicio.
No tocar switches, cables ni configuración durante la captura.

## 2. Parar (tras la duración fijada, p. ej. 15 min)

`Ctrl+C` en las dos terminales. Anotar hora de fin.

## 3. Generar los CSV (en el nodo, una de las terminales)

```bash
(echo "tiempo;offset;freq;delay"; awk '/master offset/ && / s2 / {t=$1; gsub(/ptp4l\[|\]:/,"",t); sub(/\./,",",t); for(i=1;i<=NF;i++){if($i=="offset")o=$(i+1); if($i=="freq")f=$(i+1); if($i=="delay")d=$(i+1)}; print t";"o";"f";"d}' NOMBRE_eth3.log) > NOMBRE_eth3.csv
```
```bash
(echo "tiempo;offset;freq;delay"; awk '/master offset/ && / s2 / {t=$1; gsub(/ptp4l\[|\]:/,"",t); sub(/\./,",",t); for(i=1;i<=NF;i++){if($i=="offset")o=$(i+1); if($i=="freq")f=$(i+1); if($i=="delay")d=$(i+1)}; print t";"o";"f";"d}' NOMBRE_eth4.log) > NOMBRE_eth4.csv
```

## 4. Comprobar

```bash
head -3 NOMBRE_eth3.csv NOMBRE_eth4.csv
```
```bash
wc -l NOMBRE_eth3.csv NOMBRE_eth4.csv
```
Esperado: cabecera + líneas `1210,774;-1;+73757;206`; ~7 200 líneas por fichero en 15 min.

## 5. Copiar al PC (terminal nueva en el PC, sin ssh)

```bash
mkdir -p ~/datos_tfg
```
```bash
scp 'nodo16p:~/tfg_alejandro/NOMBRE_*' ~/datos_tfg/
```
(Se copian los .log crudos y los .csv.)

## 6. Abrir en LibreOffice Calc

```bash
libreoffice --calc ~/datos_tfg/NOMBRE_eth3.csv ~/datos_tfg/NOMBRE_eth4.csv
```
Importación: separador solo `;`, idioma Español. Juntar hojas (Move or Copy Sheet) y guardar como `.ods`.

## 7. Anotar en la bitácora
Nombre de la prueba, fecha, horas de inicio y fin, cable 90–91, valores de latencias, GM y cualquier incidencia.
