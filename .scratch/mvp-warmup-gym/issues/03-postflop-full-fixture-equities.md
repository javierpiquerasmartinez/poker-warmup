# 03: Postflop completo con equities fixture

**What to build:** El módulo postflop jugable con flujo real y datos de equity falsos: flops repartidos con baraja real (uniformes entre los 19,600 posibles), clasificación Hero/Neutral/Villain validada contra una tabla de equities fixture con umbrales BTN ≥ 52% / ≤ 48% / Neutral como constantes nombradas, y feedback mostrando ambas equities en pantalla. Consumo de la tabla de equities tras una interfaz simple de lookup por flop, de modo que cambiar la fuente de datos luego toque un solo punto.

**Blocked by:** 01.

**Status:** ready-for-agent

- [ ] El módulo postflop reparte flops válidos sin cartas repetidas, uniformes en el espacio de flops
- [ ] La clasificación usa constantes nombradas para los umbrales
- [ ] El veredicto correcto con la tabla fixture coincide con Hero/Neutral/Villain esperado
- [ ] El feedback muestra las equities de ambos rangos (BTN % / BB %) del flop
- [ ] Los contadores y resumen funcionan en este módulo
- [ ] La tabla de equities se consume por una interfaz de lookup (fuente intercambiable para el ticket 04)
- [ ] Tests end-to-end del motor con RNG sembrado y equities fixture cubren: reparto válido, tres clases de veredicto en fronteras de umbral y feedback
