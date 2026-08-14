# Requerimientos Funcionales: Sistema UCI EsSalud

---

## 1. Problema de Rotación: Diagnóstico Previo al Cambio de Turno

### RF-01: Bloqueo de Cierre de Turno sin Diagnóstico (Handover Obligatorio)
- **Pregunta que resuelve:** ¿Cómo garantizar que siempre exista un diagnóstico del médico saliente antes del cambio de turno?
- **Funcionamiento:** 
  - El sistema no permite completar la entrega de guardia ni cerrar la sesión del médico saliente si no ha registrado y firmado digitalmente la ficha de evolución/diagnóstico de sus pacientes asignados.
  - Si finaliza el horario sin registro, el sistema emite una alerta de bloqueo y notifica al supervisor del turno.

### RF-02: Presentación Automática al Turno Entrante
- **Pregunta que resuelve:** ¿Cómo asegurar que el médico entrante no pierda tiempo buscando la información crítica?
- **Funcionamiento:**
  - Al iniciar sesión el médico entrante, la pantalla principal del sistema despliega por defecto la ficha de diagnóstico previa firmada por el médico saliente para cada paciente de la unidad.

---

## 2. Problema de Medianoche: Emergencias Nocturnas e Indisponibilidad

### RF-03: Alerta Nocturna de Alta Prioridad
- **Pregunta que resuelve:** ¿Qué pasa si un paciente se pone grave en el turno noche y cómo se contacta rápidamente al médico encargado?
- **Funcionamiento:**
  - Si un paciente sufre una descompensación o la enfermera activa el botón de pánico nocturno, el sistema dispara una llamada push priority al smartphone del médico asignado.
  - La notificación invalida el modo "No Molestar" o "Silencio" del dispositivo y suena continuamente hasta ser confirmada.

### RF-04: Escalamiento Automático por Indisponibilidad
- **Pregunta que resuelve:** ¿Qué pasa si el médico encargado no está disponible o no responde?
- **Funcionamiento:**
  - El sistema inicia un temporizador de respuesta. Si el médico titular no confirma la lectura, la alerta escala automáticamente en cascada:
    1. **Nivel 1:** Médico de Retén / Respaldo de la UCI (seleccionado activamente según el estado en RF-10).
    2. **Nivel 2:** Jefe de Guardia de la Red Asistencial.
  - La reasignación queda registrada con fecha, hora y causa de falta de respuesta.

---

## 3. Problema de Actualizaciones en Tiempo Real: Notificaciones y Guardado

### RF-05: Difusión Multi-Rol (Push a Médicos y Enfermeras)
- **Pregunta que resuelve:** ¿Cómo maneja las notificaciones push a varios médicos/enfermeras simultáneamente?
- **Funcionamiento:**
  - El sistema utiliza un esquema de suscripción basado en canales por Cama/Caja UCI.
  - Al registrarse un cambio de diagnóstico o alerta, el servidor emite una notificación en paralelo a todos los dispositivos (móviles y tablets) del equipo médico y de enfermería asignado a esa cama en ese turno.

### RF-06: Sincronización Inmediata e Indicador de Certeza del Diagnóstico
- **Pregunta que resuelve:** ¿Qué estrategia se usará para guardar el diagnóstico y darle certeza visual al médico de que la información está a salvo en todo momento?
- **Funcionamiento:**
  - **Estrategia Local-First (Offline):** El diagnóstico se guarda primero en la memoria local del dispositivo en milisegundos para evitar bloqueos por fallas de red.
  - **Sincronización Cloud:** En segundo plano, el dato se replica hacia la base de datos centralizada y se distribuye a las pantallas del resto del personal.
  - **Indicador Visual de Certeza y Estado del Sistema:** La interfaz médica despliega un badge dinámico en tiempo real ("Guardado Local ✓" / "Sincronizado Cloud ✓✓") que brinda certeza absoluta al médico de que la información está persistida aun durante caídas de red o fallas de servidor.

---

## 4. Problema de Gestión de Horarios y Cobertura (Jefe de UCI / Coordinador)

### RF-07: Motor Anti-Cruces, Bloqueo Estricto de Turnos y Búsqueda Inmediata
- **Pregunta que resuelve:** ¿Cómo impedir técnicamente el solapamiento de horarios y qué acción inmediata realiza el sistema ante un cruce o conflicto?
- **Funcionamiento:**
  - **Bloqueo Absoluto de Solapamiento (Hard Lock):** El motor de validación bloquea en tiempo real e impide el guardado de cualquier intento de asignar a un profesional un turno que se cruce total o parcialmente con otro horario previamente registrado, o que incumpla el descanso reglamentario.
  - **Acción Inmediata de Reagendamiento (Búsqueda de Horario Próximo):** Al interceptar y bloquear el intento de cruce, el sistema ejecuta automáticamente una acción reactiva instantánea:
    1. **Búsqueda del Horario Próximo Disponible:** Calcula y sugiere de inmediato la ventana horaria libre más cercana dentro de la disponibilidad del profesional para reubicar la guardia.
    2. **Propuesta de Retén de Respaldo:** En caso de requerirse cobertura inmediata de ese slot, la regla sugiere al médico/enfermera de retén con horario 100% compatible y sin cruces.

### RF-08: Tablero Central de Disponibilidad y Cobertura
- **Pregunta que resuelve:** ¿Cómo brindarle al coordinador una vista clara y en tiempo real del personal disponible y sus reemplazos?
- **Funcionamiento:**
  - Ofrece un panel visual con la matriz completa de horarios de médicos y enfermeras, indicando ausencias, retardos, guardias activas y médicos de retén/respaldo libres.
  - Se alimenta en tiempo real con la autogestión de disponibilidad de los médicos (RF-10).
  - Permite la reasignación rápida de turnos ante imprevistos con notificación automática al personal afectado.

---

## 5. Problema de Registro Continuo de Enfermería y Autogestión de Personal

### RF-09: Registro y Sincronización de Hojas de Enfermería
- **Pregunta que resuelve:** ¿Cómo garantizar a la enfermera que sus notas y signos vitales quedan guardados sin riesgo de pérdida?
- **Funcionamiento:**
  - Las hojas de monitoreo de signos vitales, balance hídrico y observaciones de enfermería aplican la misma estrategia Local-First del diagnóstico médico.
  - Muestra un indicador visual explícito de confirmación (check de guardado local/nube) para dar certeza total a la enfermera durante su turno.

### RF-10: Autogestión de Disponibilidad y Estado de Retén (On-Call)
- **Pregunta que resuelve:** ¿Cómo marca un médico o enfermera su estado de disponibilidad para actuar como respaldo en emergencias nocturnas y alimentar el tablero de cobertura?
- **Funcionamiento:**
  - **Selector de Estado Activo:** Desde su perfil en la aplicación móvil, el médico puede cambiar su estado operativo en 1 toque: *"En Guardia Activa"*, *"Disponible para Retén/Respaldo"* o *"No Disponible / Descanso"*.
  - **Vinculación con la Cascadas de Alertas:** Si el profesional se marca como *"Disponible para Retén"*, entra automáticamente en la lista priorizada de auxilio de Nivel 1 del motor de escalamiento (RF-04).
  - **Flujo de Confirmación de Activación:** Ante una emergencia nocturna escalada, el médico de retén recibe un prompt de confirmación directa en pantalla para aceptar el turno de respaldo, actualizando su estado a *"Guardia Activa"* en el tablero central (RF-08) y registrando la trazabilidad de la llamada.