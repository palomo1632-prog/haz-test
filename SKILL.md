---
name: haz-test
description: Escribir o completar los tests de un proyecto con la filosofía de "expect tests" (assert sobre el resultado visible esperado, código real, dobles solo en los bordes externos). Primero detecta si el proyecto ya usa esta forma y la respeta; si no la usa, la implementa. Usar cuando el usuario diga "haz test", "hazme los tests", "escribí tests", "agregá tests", "tests para esto", "test de regresión de este bug" o invoque /haz-test.
---

# Haz test

Objetivo: tests que declaran el resultado visible esperado ("dado X, espero Y") y ejecutan el
código real. Un test tiene que romperse cuando se rompe una regla de negocio, no cuando alguien
refactoriza.

## Paso 0 — Reconocer el proyecto (siempre, antes de escribir nada)

Averigualo leyendo el repo; no preguntes lo que podés ver:

- ¿Hay runner y comando de test? (`package.json`, `pytest`, `go test`, `phpunit`, `cargo test`…)
- ¿Dónde viven los tests y cómo se llaman?
- ¿Los tests llaman al código real y solo simulan lo externo, o simulan todo?
- ¿Afirman sobre salidas visibles y contratos, o reimplementan la lógica?
- ¿Hay docs de convención de tests?

Decisión:

- **Ya usa esta forma** → seguí sus convenciones (runner, carpetas, nombres, helpers, idioma).
  No borres ni reescribas tests que pasan. Dejá la línea base (qué pasa y qué falla hoy) y sumá
  tests donde falta cobertura del camino crítico o en las zonas que toques.
- **No la usa o no hay tests** → implementala con el mínimo: agregá el runner estándar del stack
  sin cambiar comportamiento, creá la carpeta de tests, descubrí los archivos automáticamente y
  empezá por el camino crítico.

## Paso 1 — Invariantes

Antes de escribir tests, listá las invariantes: reglas que nunca se pueden romper (plata,
permisos, datos del usuario, orden de pasos, estados). Cada una lleva al menos un test.

## Reglas

1. "Dado X, espero Y" sobre lo que ve el usuario o el contrato público. Código real; nunca copies
   la lógica en el test. Si el test duplica un cálculo, cambialo por un assert sobre la salida.
2. Dobles solo en los bordes externos: red, APIs de terceros, mensajería, reloj, base de
   producción. Todo lo demás corre de verdad. Aislamiento: base o archivo temporal por test.
3. Un test = una regla, con nombre en una frase. Si nace de un bug, citá el caso o ticket.
4. Fixtures con valores distintos a los reales: un valor hardcodeado tiene que romper el test.
5. Todo bug arreglado trae un test de regresión que se ve ROJO sin el arreglo y VERDE con él.
   Mostralo antes de cerrar.
6. Si un bug se repite: 2ª vez, mismo test con otro estímulo; 3ª vez, matriz de variantes de la
   clase de casos en vez de otro parche.
7. Assert de texto exacto solo si ese texto lo recibe el usuario final y está aprobado. Nunca
   tests de tono o calidad de respuestas de IA: eso se valida con usuarios reales.
8. Suite sin red, determinista y rápida; descubrimiento automático de archivos para que ninguno
   quede afuera; un solo comando corre todo.
9. No testees lo trivial (getters, configuración, HTML estático) ni lo que no puede costar
   plata o confianza. Si el proyecto no tiene reglas de negocio (landing, marketing), decilo y
   no fuerces tests: con 0–2 alcanza.

## Proyecto nuevo

Trabajá en vertical: una invariante → test que falla (rojo) → implementación → verde → la
siguiente. No escribas todos los tests antes del código.

## Proyecto en marcha

No toques lo que pasa. Si no hay runner, agregalo sin cambiar comportamiento. Priorizá plata,
permisos y datos del usuario; el resto se agrega en las zonas que se toquen.

## Al cerrar, reportá

- Forma detectada (existente o implementada) y en qué te basaste.
- Invariantes cubiertas y cuáles quedaron sin test, con el motivo.
- Evidencia del rojo → verde de los tests de regresión.
