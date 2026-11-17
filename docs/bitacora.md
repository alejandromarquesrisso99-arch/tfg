# Bitácora del TFG (notas del estudiante)

## 10/11/26
- Investigación de cómo funciona el protocolo gPTP en redes TSN
  - En Pdelay se asume que ida y vuelta tardan lo mismo, ¿esto es siempre cierto? Podría llegar a aumentar el desfase por un pequeño error.
  - ¿Cómo puede contener el Follow_Up info de la salida de él mismo?
  - ¿Para qué sirve el path delay si en el Follow_Up ya se tiene en cuenta el tiempo de viaje?

## 13/11/26
- La investigación refleja que el modo one-step reduce a la mitad la cantidad de tramas de sincronización que circulan por la red, disminuyendo la carga de los procesadores y mitigando posibles cuellos de botella. Nuestro modelo RELY-TSN12 (https://soc-e.com/products/rely-tsn12/) puede operar en este modo. Cómo funciona: inyecta la marca de tiempo y tiempos de retención dentro del paquete Sync y calcula el CRC de Ethernet antes de que el último bit salga por el cable hacia el siguiente salto.
- El diseño de la arquitectura de nodos debe seguir n = 3f + 1 para proteger al sistema de f fallos.
- En el lab, al lanzar ptp4l en dos interfaces, falló uno de los bridges: ambos crean un fichero temporal con el mismo nombre, el segundo sobreescribe y a veces hay conflicto. Se puede especificar que pidan recursos diferentes.
- En el lab no hemos podido ver cómo configurar nuestros switches para modo one-step; hay que investigarlo.
