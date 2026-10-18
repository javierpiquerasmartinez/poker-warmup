# 05: Charts reales + tono frío

**What to build:** Cierre del MVP: sustituir los placeholders por los Charts reales (las cuadrículas 13×13 que el owner pega — contenido `RFI Btn` y `BB vs Btn`), regenerar la tabla de equities con el comando del ticket 04, y revisión fría end-to-end: validación ruidosa de datos reales, una ronda completa jugable de cada módulo con feedback correcto, y pulido general de estilo (thumb-first, minimalista, sin distracciones) según los criterios de éxito del FSD.

**Blocked by:** 02 y 04.

**Status:** ready-for-agent

- [ ] Los Charts reales (`RFI Btn`, `BB vs Btn`) están pegados y cargan sin errores; ningún placeholder TODO permanece en los datos en uso
- [ ] La tabla de equities está regenerada desde los Charts reales con el comando único
- [ ] Un barrido frío de preflop: cada Spot genera preguntas, valida y muestra el grid acorde al Chart real
- [ ] Un barrido frío de postflop: equities plausibles frente a las 3 clases con los rangos reales
- [ ] Los contadores y el resumen funcionan fin de ronda en ambos módulos
- [ ] Revisión de estilo thumb-first pasada: botones dimensionados para pulgar, tipografía clara, sin elementos distractores
- [ ] La carga falla ruidosamente si un dato real está mal formado (casos probados)
