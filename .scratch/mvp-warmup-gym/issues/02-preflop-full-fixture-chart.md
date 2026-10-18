# 02: Preflop completo con Chart fixture

**What to build:** El módulo preflop jugable de verdad contra datos de Chart. Un archivo de Chart por Spot del pool (`RFI Btn`, `BB vs Btn`) con placeholder TODO para las cuadrículas reales; loader que falla ruidosamente ante Chart malformado o ausente; reparto de mano ponderado por Combos dibujando 2 cartas sin reemplazo de una baraja real; Spot aleatorio del pool; botones de Subaction solo los que el Spot permite (`FOLD | RAISE` / `FOLD | CALL | 3BET`); validación contra la Subaction canónica; feedback que revela la 13×13 del Spot con la Subaction correcta sombreada. Todo bajo el motor de sesión/seam, con tests end-to-end de stream sembrado usando Charts fixture.

**Blocked by:** 01.

**Status:** ready-for-agent

- [ ] Cada Spot del pool tiene su archivo de Chart con una entrada por Hand Class y exactamente una Subaction canónica, claramente marcado TODO
- [ ] Cargar Charts malformados o ausentes falla ruidosamente (sin defaults silenciosos)
- [ ] El módulo preflop genera: Spot aleatorio del pool + Combo ponderado por Combos (par 6 / suited 4 / offsuit 12, por construcción de baraja)
- [ ] Se muestran solo los botones de Subaction permitidos por el Spot actual
- [ ] Responder valida contra la 169 entradas del Chart del Spot actual
- [ ] El feedback muestra la 13×13 con la Subaction correcta sombreada
- [ ] Los contadores y resumen del bucle funcionan para este módulo
- [ ] Tests end-to-end del motor con RNG sembrado y Charts fixture cubren: generación, validación correcta/incorrecta y grid de feedback; barrido estadístico simple del reparto ponderado
- [ ] El pool de Spots y etiquetas permitidas viven como datos, no en branching
