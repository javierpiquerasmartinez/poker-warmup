# 04: Script de equities + tabla precalculada (ADR-0001)

**What to build:** El generador offline de la tabla de equities: un script Node (Monte Carlo sobre runouts aleatorios, muestreo de ambos Ranges ponderado por Combos, Ranges derivados de los Charts — entradas no `FOLD`) que produce un activo JSON estático con las equities de los 19,600 flops, regenerable con un solo comando npm. El postflop pasa a consumir el activo generado en lugar del fixture, manteniendo la interfaz de lookup existente. Anclas matemáticas fijas validan el cálculo.

**Blocked by:** 02 (necesita los archivos de Chart, fuente de los Ranges).

**Status:** ready-for-agent

- [ ] Existe un comando npm único que regenera la tabla completa de 19,600 flops desde los Charts actuales
- [ ] Los Ranges del script derivan de los Charts (no-`FOLD` para `RFI Btn` y `BB vs Btn`), ponderados por Combos
- [ ] Anclas conocidas pasan: AA vs KK ≈ 81/19, y casos de rango dominante (100% vs 0% de fuerza) con resultados conocidos
- [ ] El activo generado contiene la equity de ambos rangos por flop y se consume en el postflop vía la interfaz de lookup existente (fixture retirado)
- [ ] El script y sus anclas corren en Vitest sin navegador
- [ ] ADR-0001 queda reflejado fielmente (precalculo offline, regeneración por comando, sin cálculo runtime)
