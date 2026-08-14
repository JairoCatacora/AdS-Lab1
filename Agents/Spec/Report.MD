# Reporte de Evaluación — Agente Eval-Spec

Resultado de ejecutar el [prompt de evaluación](../../README.MD#prompt-usado-para-evaluar-requerimientos-vs-personas-usuarios-modelo) definido en el README, aplicando la rúbrica de [Eval-Spec.MD](Eval-Spec.MD) sobre [ReqFunc.MD](../../Requirements/ReqFunc.MD) y [ReqNoFunc.MD](../../Requirements/ReqNoFunc.MD) contra las personas [Pablo](../../Personas/UM-Pablo.MD), [Claudia](../../Personas/UM-Claudia.MD) y [Roberto](../../Personas/UM-Roberto.MD).

## Rúbrica Individual por Persona

### Rúbrica: Pablo (Médico internista)

| # | Necesidad | Tipo |
|---|---|---|
| 1 | Recibir y registrar el diagnóstico y evolución de cada paciente de forma rápida apenas empieza su turno | Objetivo |
| 2 | Confiar que el diagnóstico del médico anterior está completo, actualizado y disponible antes de tomar decisiones | Objetivo |
| 3 | Minimizar el tiempo dedicado a la aplicación para poder concentrarse en el paciente | Objetivo |
| 4 | Cambios de turno donde la información del paciente se pierde, se entrega verbalmente o queda incompleta | Frustración |
| 5 | Aplicaciones lentas o complejas de configurar, poco intuitivas para alguien que rota de hospital cada pocas semanas | Frustración |
| 6 | Falta de certeza sobre si el sistema estará disponible en el momento crítico (caídas del sistema) | Frustración |

**Score Pablo = promedio simple de las 6 necesidades.**

### Rúbrica: Claudia (Enfermera UCI)

| # | Necesidad | Tipo |
|---|---|---|
| 1 | Ser notificada cuando cambie el estado de un paciente o cuando el médico registre una nueva indicación | Objetivo |
| 2 | Poder escalar rápidamente una emergencia al médico de guardia si un paciente se agrava en la madrugada | Objetivo |
| 3 | Tener certeza de que la información que registra (signos vitales, observaciones, etc) queda guardada y disponible para el equipo médico | Objetivo |
| 4 | No saber a quién contactar cuando el médico encargado no responde en el turno noche | Frustración |
| 5 | Notificaciones que llegan tarde o se pierden entre varios médicos/enfermeras | Frustración |

**Score Claudia = promedio simple de las 5 necesidades.**

### Rúbrica: Roberto (Médico de guardia / Coordinador)

| # | Necesidad | Tipo |
|---|---|---|
| 1 | Tener una vista clara de los horarios de todo el personal médico y de enfermería, sin cruces de turnos ni huecos de cobertura | Objetivo |
| 2 | Ser contactado de forma efectiva si sucede una emergencia y ser el médico de respaldo disponible | Objetivo |
| 3 | Escalar el piloto sin que la gestión de horarios se vuelva caótica | Objetivo |
| 4 | Cruces de horarios que dejan turnos sin cobertura médica | Frustración |
| 5 | No tener visibilidad de quién está realmente disponible en un momento dado (turnos, ausencias, respaldos) | Frustración |
| 6 | Herramientas que no escalan: lo que funciona para un hospital no funciona para cientos o miles | Frustración |

**Score Roberto = promedio simple de las 6 necesidades.**

**Score Global = promedio simple de Score Pablo, Score Claudia y Score Roberto. Veredicto PASSED si Score Global ≥ 7/10 y ninguna persona < 5/10.**

---

## ITERACIÓN #1

### Persona: Pablo (Médico internista)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Recibir y registrar el diagnóstico rápido al iniciar turno | Objetivo | BI-3 (guardado, pero sin flujo de presentación al turno entrante) | 1 | 0 | 1.1 |
| Confiar en que el diagnóstico previo está completo y disponible | Objetivo | BI-5 (exige el registro, no garantiza que se muestre al entrante) | 1 | 0 | 1.1 |
| Minimizar el tiempo dedicado a la aplicación | Objetivo | Gap — no hay ningún RNF de usabilidad/onboarding | 0 | 0 | 0.0 |
| Pérdida/entrega verbal de información en cambio de turno | Frustración | BI-5 (Handover Obligatorio) | 5 | 4 | 10.0 |
| Apps lentas/complejas de configurar para personal rotativo | Frustración | Gap — no existe requerimiento de UX/onboarding | 0 | 0 | 0.0 |
| Falta de certeza de disponibilidad del sistema en momento crítico | Frustración | BI-3 (persistencia), sin SLA numérico ni indicador visual | 1 | 0 | 1.1 |

**Score Pablo: 2.22/10**

### Persona: Claudia (Enfermera UCI)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Ser notificada de cambios de estado o nuevas indicaciones | Objetivo | BI-6 (genérico, sin canal por cama/rol) | 1 | 0 | 1.1 |
| Escalar rápido una emergencia al médico de guardia | Objetivo | BI-1, BI-2 (no describen cómo la enfermera dispara la alerta) | 1 | 0 | 1.1 |
| Certeza de que lo que registra queda guardado y disponible | Objetivo | BI-3 habla de "diagnóstico", no de hojas de enfermería/signos vitales | 1 | 0 | 1.1 |
| No saber a quién contactar si el médico no responde de noche | Frustración | BI-2 (Plan de Escalado por Ausencia) | 5 | 4 | 10.0 |
| Notificaciones que llegan tarde o se pierden | Frustración | BI-6, BI-7, sin latencia máxima comprometida | 1 | 0 | 1.1 |

**Score Claudia: 2.88/10**

### Persona: Roberto (Médico de guardia / Coordinador)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Vista clara de horarios de todo el personal, sin cruces ni huecos | Objetivo | BI-4 solo valida cruces, no ofrece ninguna vista/tablero | 1 | 0 | 1.1 |
| Ser contactado efectivamente y ser respaldo disponible | Objetivo | BI-1, BI-2 (no hay forma de que Roberto marque su propia disponibilidad) | 1 | 0 | 1.1 |
| Escalar el piloto sin que la gestión de horarios sea caótica | Objetivo | Gap — no existe ningún RNF de escalabilidad (1K→100K→10M) | 0 | 0 | 0.0 |
| Cruces de horarios que dejan turnos sin cobertura | Frustración | BI-4 bloquea el cruce, pero no reagenda ni sugiere retén | 1 | 0 | 1.1 |
| No tener visibilidad de quién está realmente disponible | Frustración | Gap — no existe tablero central de cobertura | 0 | 0 | 0.0 |
| Herramientas que no escalan de 1 hospital a miles | Frustración | Gap — no existe ningún RNF de escalabilidad ni arquitectura distribuida | 0 | 0 | 0.0 |

**Score Roberto: 0.55/10**

### Resumen

| Persona | Score /10 |
|---|---|
| Pablo | 2.22 |
| Claudia | 2.88 |
| Roberto | 0.55 |
| **PROMEDIO** | **1.88/10 — 18.8% — FAILED** |

Veredicto: **FAILED** (promedio muy por debajo de 7/10, y las tres personas caen por debajo de 5/10 — Roberto en particular queda casi completamente desatendido).

### Gaps detectados y cómo se cerraron en la Iteración #1

| Gap en el Borrador Inicial | Persona afectada | Requerimiento agregado en la Iteración #1 |
|---|---|---|
| Ningún flujo de presentación automática del diagnóstico previo al médico entrante | Pablo | **RF-02** — Presentación Automática al Turno Entrante |
| Ningún RNF de usabilidad/onboarding para personal de alta rotación | Pablo | **RNF-04** — Curva de Aprendizaje Mínima e Interfaz Clínicamente Eficiente |
| Sin SLA numérico ni indicador de disponibilidad del sistema | Pablo | **RNF-03** — Métricas Críticas del Sistema (SLA 99.9%, RTO < 5 min) |
| Notificación genérica sin mecanismo de suscripción por cama/rol | Claudia | **RF-05** — Difusión Multi-Rol (canales por Cama/Caja UCI) |
| Sin mecanismo explícito para que la enfermera dispare una alerta nocturna | Claudia | **RF-03** — Alerta Nocturna de Alta Prioridad (botón de pánico) |
| Persistencia solo mencionada para "diagnóstico", no para hojas de enfermería | Claudia | **RF-09** — Registro y Sincronización de Hojas de Enfermería |
| Sin latencia máxima comprometida para notificaciones | Claudia | **RNF-01** — Entrega Multicast de Baja Latencia (< 1 s) |
| Anti-cruces solo bloquea, no reagenda ni ofrece vista de cobertura | Roberto | **RF-07** — Motor Anti-Cruces con reagendamiento automático + **RF-08** — Tablero Central de Disponibilidad |
| Sin ningún RNF de escalabilidad de la red de hospitales | Roberto | **RNF-03** — Escalabilidad Gradual (1K → 100K → 10M) + **RNF-02** — Base de Datos Distribuida |
| Sin forma de que un médico marque su propia disponibilidad como retén | Roberto | *(gap que persiste incluso en la Iteración #1 — no se cierra hasta **RF-10** en la Iteración #2)* |

---

## ITERACIÓN #2

### Persona: Pablo (Médico internista)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Recibir y registrar el diagnóstico rápido al iniciar turno | Objetivo | RF-02, RF-06 | 5 | 4 | 10.0 |
| Confiar en que el diagnóstico previo está completo y disponible | Objetivo | RF-01, RF-02 | 5 | 4 | 10.0 |
| Minimizar el tiempo dedicado a la aplicación | Objetivo | RNF-04 | 5 | 4 | 10.0 |
| Pérdida/entrega verbal de información en cambio de turno | Frustración | RF-01 | 5 | 4 | 10.0 |
| Apps lentas/complejas de configurar para personal rotativo | Frustración | RNF-04, RNF-03 | 5 | 4 | 10.0 |
| Falta de certeza de disponibilidad del sistema en momento crítico | Frustración | RNF-03 (SLA) | 5 | 0 | 5.6 |

**Score Pablo: 9.27/10**
*Gap detectado: RNF-03 fija las métricas de disponibilidad (99.9%, RTO < 5 min), pero ningún requerimiento describe un indicador visual que le muestre a Pablo, en el momento, que el sistema sigue operando/guardando (RF-09 sí define ese indicador, pero solo para las hojas de enfermería).*

### Persona: Claudia (Enfermera UCI)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Ser notificada de cambios de estado o nuevas indicaciones | Objetivo | RF-05 | 5 | 4 | 10.0 |
| Escalar rápido una emergencia al médico de guardia | Objetivo | RF-03, RF-04 | 5 | 4 | 10.0 |
| Certeza de que lo que registra queda guardado y disponible | Objetivo | RF-09 | 5 | 4 | 10.0 |
| No saber a quién contactar si el médico no responde de noche | Frustración | RF-04 (escalamiento Nivel 1/Nivel 2) | 5 | 4 | 10.0 |
| Notificaciones que llegan tarde o se pierden | Frustración | RNF-01, RF-05 | 5 | 4 | 10.0 |

**Score Claudia: 10.0/10**
*Sin gaps: las 5 necesidades quedan cubiertas de forma explícita y con flujo accionable.*

### Persona: Roberto (Médico de guardia / Coordinador)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Vista clara de horarios de todo el personal, sin cruces ni huecos | Objetivo | RF-07, RF-08 | 5 | 4 | 10.0 |
| Ser contactado efectivamente y ser respaldo disponible | Objetivo | RF-03, RF-04 | 1 | 0 | 1.1 |
| Escalar el piloto sin que la gestión de horarios sea caótica | Objetivo | RNF-03, RF-07, RF-08 | 5 | 4 | 10.0 |
| Cruces de horarios que dejan turnos sin cobertura | Frustración | RF-07 (Hard Lock + reagendamiento) | 5 | 4 | 10.0 |
| No tener visibilidad de quién está realmente disponible | Frustración | RF-08 | 5 | 4 | 10.0 |
| Herramientas que no escalan de 1 hospital a miles | Frustración | RNF-03, RNF-02 | 5 | 4 | 10.0 |

**Score Roberto: 8.52/10**
*Gap detectado: ningún RF define cómo un médico (p. ej. Roberto como retén) marca su propio estado de disponibilidad/indisponibilidad para ser llamado como respaldo. RF-08 asume que el sistema "conoce" quién está de retén, pero no existe el flujo de origen de ese dato.*

### Resumen

| Persona | Score /10 |
|---|---|
| Pablo | 9.27 |
| Claudia | 10.00 |
| Roberto | 8.52 |
| **PROMEDIO** | **9.26/10 — 92.6% — PASSED** |

Veredicto: **PASSED** (promedio ≥ 7/10 y ninguna persona por debajo de 5/10).

### Gaps y Recomendaciones para una eventual Iteración #2

1. **Indicador de estado del sistema para médicos (Pablo).** Extender el indicador visual de guardado local/nube que ya existe en RF-09 (hoy exclusivo de enfermería) para que también aparezca en el flujo de registro de diagnóstico del médico (RF-06), dando certeza inmediata de que su diagnóstico quedó persistido pese a caídas de red.
2. **Auto-gestión de disponibilidad como retén (Roberto).** Agregar un requerimiento funcional (p. ej. RF-10) que permita a un médico marcar su propio estado como "disponible / no disponible para retén", alimentando directamente el motor de escalamiento (RF-04) y el tablero de cobertura (RF-08), en vez de asumir que ese dato ya existe en el sistema.

Ambos gaps son menores y no bloquean el piloto: el promedio global (92.6%) supera ampliamente el umbral de aprobación (70%) y ninguna persona quedó desatendida en sus necesidades críticas (diagnóstico disponible, alerta nocturna, anti-cruces de horario). Se recomienda incorporar RF-10 y la extensión de RF-09 antes del lanzamiento a 100K hospitales (hito de 6 meses), no como bloqueo del piloto inicial de 1K hospitales.

---

## ITERACIÓN #3

Cambios aplicados a la especificación desde la Iteración #1:
- **RF-06** se amplió a "Sincronización Inmediata e Indicador de Certeza del Diagnóstico": ahora incluye un badge dinámico ("Guardado Local ✓" / "Sincronizado Cloud ✓✓") visible para el médico, cerrando el gap de Pablo.
- **RF-10 (nuevo):** "Autogestión de Disponibilidad y Estado de Retén (On-Call)" — selector de estado en 1 toque ("En Guardia Activa" / "Disponible para Retén" / "No Disponible"), vinculado a la cascada de RF-04 y con flujo de confirmación de activación, cerrando el gap de Roberto.
- Referencias cruzadas actualizadas en RF-04, RF-08, RNF-02, RNF-04 y RNF-05 para reflejar RF-10.

### Persona: Pablo (Médico internista)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Recibir y registrar el diagnóstico rápido al iniciar turno | Objetivo | RF-02, RF-06 | 5 | 4 | 10.0 |
| Confiar en que el diagnóstico previo está completo y disponible | Objetivo | RF-01, RF-02 | 5 | 4 | 10.0 |
| Minimizar el tiempo dedicado a la aplicación | Objetivo | RNF-04 | 5 | 4 | 10.0 |
| Pérdida/entrega verbal de información en cambio de turno | Frustración | RF-01 | 5 | 4 | 10.0 |
| Apps lentas/complejas de configurar para personal rotativo | Frustración | RNF-04, RNF-03 | 5 | 4 | 10.0 |
| Falta de certeza de disponibilidad del sistema en momento crítico | Frustración | RF-06 (Indicador Visual de Certeza y Estado del Sistema) | 5 | 4 | 10.0 |

**Score Pablo: 10.0/10** — gap de la Iteración #1 cerrado por RF-06.

### Persona: Claudia (Enfermera UCI)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Ser notificada de cambios de estado o nuevas indicaciones | Objetivo | RF-05 | 5 | 4 | 10.0 |
| Escalar rápido una emergencia al médico de guardia | Objetivo | RF-03, RF-04 | 5 | 4 | 10.0 |
| Certeza de que lo que registra queda guardado y disponible | Objetivo | RF-09 | 5 | 4 | 10.0 |
| No saber a quién contactar si el médico no responde de noche | Frustración | RF-04 (escalamiento Nivel 1/Nivel 2) | 5 | 4 | 10.0 |
| Notificaciones que llegan tarde o se pierden | Frustración | RNF-01, RF-05 | 5 | 4 | 10.0 |

**Score Claudia: 10.0/10** — sin cambios respecto a la Iteración #1.

### Persona: Roberto (Médico de guardia / Coordinador)

| Necesidad | Tipo | Evidencia | Cumplimiento | Flujo claro | Score /10 |
|---|---|---|---|---|---|
| Vista clara de horarios de todo el personal, sin cruces ni huecos | Objetivo | RF-07, RF-08 | 5 | 4 | 10.0 |
| Ser contactado efectivamente y ser respaldo disponible | Objetivo | RF-04, RF-10 (selector de estado + confirmación de activación) | 5 | 4 | 10.0 |
| Escalar el piloto sin que la gestión de horarios sea caótica | Objetivo | RNF-03, RF-07, RF-08 | 5 | 4 | 10.0 |
| Cruces de horarios que dejan turnos sin cobertura | Frustración | RF-07 (Hard Lock + reagendamiento) | 5 | 4 | 10.0 |
| No tener visibilidad de quién está realmente disponible | Frustración | RF-08 (alimentado en tiempo real por RF-10) | 5 | 4 | 10.0 |
| Herramientas que no escalan de 1 hospital a miles | Frustración | RNF-03, RNF-02 | 5 | 4 | 10.0 |

**Score Roberto: 10.0/10** — gap de la Iteración #1 cerrado por RF-10.

### Resumen

| Persona | Score /10 |
|---|---|
| Pablo | 10.00 |
| Claudia | 10.00 |
| Roberto | 10.00 |
| **PROMEDIO** | **10.00/10 — 100% — PASSED** |

Veredicto: **PASSED**.

### Gaps y Recomendaciones

Ninguno. Las 17 necesidades evaluadas (6 de Pablo, 5 de Claudia, 6 de Roberto) quedan cubiertas con cumplimiento total y flujo claro, con evidencia trazable a un RF/RNF específico.

### Evolución entre iteraciones

| Persona | Iteración #1 (Borrador) | Iteración #2 | Iteración #3 |
|---|---|---|---|
| Pablo | 2.22/10 | 9.27/10 | 10.00/10 |
| Claudia | 2.88/10 | 10.00/10 | 10.00/10 |
| Roberto | 0.55/10 | 8.52/10 | 10.00/10 |
| **PROMEDIO** | **1.88/10 (18.8%) — FAILED** | **9.26/10 (92.6%) — PASSED** | **10.00/10 (100%) — PASSED** |

La Iteración #1 falla porque describe capacidades a alto nivel sin flujo accionable ni cobertura de UX/escalabilidad — Roberto, el coordinador, es quien más sufre por la ausencia total de tablero de cobertura y de RNF de escalabilidad. La Iteración #2 elabora esos enunciados en RF-01...RF-09 y agrega RNF-01 a RNF-05, cerrando casi todos los gaps salvo dos menores. La Iteración #3 los cierra por completo mediante la extensión de RF-06 y la incorporación de RF-10.
