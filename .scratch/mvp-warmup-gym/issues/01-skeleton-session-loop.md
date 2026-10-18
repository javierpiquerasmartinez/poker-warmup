# 01: Esqueleto + bucle de sesión con datos falsos

**What to build:** La forma completa del bucle de calentamiento end-to-end con datos maniquí: SPA con dos pestañas (Postflop/Preflop) en español, y en el módulo postflop un bucle jugable (pregunta → tres opciones de clasificación → veredicto instantáneo → siguiente → contadores en vivo → resumen al cerrar). Las preguntas/fakes viven en datos de fixture, no en la lógica del motor. Establece el scaffold (React + Vite + TypeScript + Vitest) y la primera versión del **seam acordado**: el motor de sesión puro guiado por RNG sembrado.

**Blocked by:** None (can start immediately).

**Status:** ready-for-agent

- [ ] La app abre en una vista con dos pestañas navegables en español
- [ ] El tab Postflop muestra una pregunta de fixture (flop estático) con 3 botones (Hero / Neutral / Villain)
- [ ] Al responder: veredicto inmediato (acierto/error) y feedback visible
- [ ] El siguiente flop aparece automáticamente; el bucle es infinito hasta que el usuario cierra
- [ ] Contadores en vivo visibles (spots completados, aciertos, racha)
- [ ] Al cerrar el bucle aparece un resumen (accuracy, spots, mejor racha)
- [ ] El estado del bucle es manejado por un motor de sesión puro testable con RNG sembrado; tests de Vitest cubren un stream sembrado end-to-end
- [ ] El tab Preflop existe como placeholder (pestaña navegable, contenido stub)
- [ ] Vocabulario del dominio (Spot, Chart, Subaction, Hand Class, Combo, Range, Hero, Villain) usado en tipos y módulos
- [ ] UI thumb-first básica, sin router
