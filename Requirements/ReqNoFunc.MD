# Requerimientos No Funcionales: Sistema UCI EsSalud

---

## 1. Arquitectura de Notificaciones Push y Comunicación en Tiempo Real

### RNF-01: Entrega Multicast de Baja Latencia
- **Pregunta Técnica:** ¿Cómo enviar notificaciones push a múltiples dispositivos simultáneamente sin saturación ni retardos?
- **Estrategia:**
  - **Arquitectura Event-Driven:** Uso de un Broker de Mensajería distribuido combinado con protocolos WebSockets y HTTP/2 para mantener canales bidireccionales activos por cama/caja UCI.
  - **Latencia Máxima:** Entrega de alertas push priority y actualizaciones de estado en $< 1$ segundo a todos los terminales (móviles y tablets) suscritos al canal.

---

## 2. Estrategia de Persistencia, Disponibilidad y Resiliencia "En Todo Momento"

### RNF-02: Base de Datos Distribuida y Arquitectura Local-First
- **Pregunta Técnica:** ¿Qué estrategia de guardado garantiza que diagnósticos, hojas de enfermería y registros jamás se pierdan y estén disponibles en todo momento?
- **Estrategia:**
  - **Estrategia Local-First (Offline Resilience):** Uso de bases de datos embebidas (SQLite/IndexedDB) en los dispositivos móviles/tablets para permitir el registro inmediato de evoluciones y signos vitales en milisegundos, manteniendo operatividad total aun en zonas sin cobertura de red.
  - **Base de Datos Cloud Distribuida:** Motor NoSQL distribuido configurado con replicación multirregión y consistencia fuerte para datos clínicos.
  - **Caché de Ultra Baja Latencia:** Clúster Redis distribuido para servir fichas activas de pacientes, estados de retén y matrices de horarios con tiempos de respuesta $< 100$ ms.

---

## 3. SLA, Rendimiento Exigido y Escalabilidad

### RNF-03: Métricas Críticas del Sistema
| Indicador | Métrica Requerida | Justificación Técnica y Operativa |
| :--- | :--- | :--- |
| **Tiempo de Inicio de Aplicación** | $< 1$ segundo | Acceso e intervención inmediata ante emergencias críticas de medianoche. |
| **Tiempo de Carga de Configuración** | $< 5$ segundos | Inicialización y despliegue rápido de pantallas al cambiar de turno. |
| **Disponibilidad del Sistema (SLA)** | $99.9\%$ | Garantiza que el sistema esté operativo en todo momento (máximo 8.76h de parada anual planificada). |
| **Tiempo de Recuperación (RTO)** | $< 5$ minutos | Minimiza el impacto clínico ante caídas de red o fallas de servidor. |
| **Escalabilidad Gradual** | 1K $\rightarrow$ 100K $\rightarrow$ 10M | Infraestructura elástica autosostenible para expansión masiva a nivel de red nacional asistencial. |

---

## 4. Usabilidad, Experiencia de Usuario (UX) y Onboarding en Personal Rotatorio

### RNF-04: Curva de Aprendizaje Mínima e Interfaz Clínicamente Eficiente
- **Pregunta Técnica:** ¿Cómo asegurar que el personal médico que rota semanalmente de hospital (Pablo) adopte la app de inmediato sin errores ni capacitación previa?
- **Estrategia:**
  - **Navegación Eficiente ($\le 3$ Clics):** Flujos de trabajo optimizados para ejecutar tareas prioritarias (firma de handover, consulta de ficha previa, registro de signos vitales, cambio de estado de disponibilidad) en máximo 3 toques en pantalla.
  - **Tiempo de Onboarding Inmediato:** Curva de aprendizaje intuitiva $\le 5$ minutos para personal de alta rotación, reduciendo la carga cognitiva sin necesidad de cursos ni manuales extensos.
  - **Feedback Visual Claro de Guardado:** Confirmadores visuales unificados (checks de sincronización local vs. nube) para brindar total certeza de registro a médicos (Pablo) y enfermeras (Claudia).

---

## 5. Seguridad, Validación de Reglas y Firma Digital

### RNF-05: Integridad Clínica, Validación Estricta de Horarios e Inmutabilidad
- **Pregunta Técnica:** ¿Cómo garantizar la validez legal del handover y la protección anti-cruces de horarios en el backend?
- **Estrategia:**
  - **Firma Digital Legally Binding:** Integración de PKI/certificados digitales para la firma inmutable de las fichas de entrega de turno (RF-01).
  - **Validación Atómica de Horarios:** El motor de reglas (RF-07 y RF-10) ejecuta transacciones atómicas con locks distribuidos en Redis/DB para evitar condiciones de carrera (*race conditions*) durante la asignación simultánea de turnos y activación de retén.