# Definición del Problema: Sistema de Gestión de Red UCI - EsSalud 

## 1. Contexto y Alcance Arquitectural

El Seguro Social de Salud del Perú (**EsSalud**) administra una red nacional de centros asistenciales divididos en Instituciones Prestadoras de Servicios de Salud (IPRESS) de distintos niveles de complejidad (como los Hospitales Nacionales Edgardo Rebagliati, Guillermo Almenara y Alberto Sabogal). Operativamente, estas IPRESS emplean la plataforma **ESSI** (*EsSalud Servicios de Salud Inteligente*) y se rigen por normativas específicas para la atención en Cuidados Intensivos e Intermedios (UCI/UCIN) emanadas por la Gerencia Central de Prestaciones de Salud (GCPS).

A pesar de contar con módulos de Historia Clínica Electrónica (HCE) en ESSI, la gestión operacional en las UCI presenta vacíos en la automatización en tiempo real, el control automatizado de guardias y la respuesta a contingencias nocturnas.

### Requerimientos de Escalamiento y Métricas SLAs

El piloto se lanzará en la región Lima y sus distritos, debiendo soportar la siguiente proyección de crecimiento y rendimiento:

* **Escalamiento Progresivo de Nodos:**
* **Lanzamiento (Piloto Red Lima):** 1,000 hospitales.
* **6 Meses (Red Nacional):** 100,000 hospitales.
* **2 Años (Proyección Multirred Global):** 10,000,000 de hospitales.

* **Métricas y SLAs:**
* **Tiempo de Inicio de la Aplicación:** $< 1$ segundo.
* **Carga de Configuración del Sistema:** $< 5$ segundos.
* **Disponibilidad del Sistema (Uptime):** $99.9\%$.
* **Tiempo de Recuperación (RTO):** $< 5$ minutos ante caídas del sistema.

## 2. Desafíos Críticos de Operación y Sustento Institucional

### A. Gestión de Emergencias Nocturnas ("El Problema de Medianoche")

* **Diagnóstico Operativo:** Durante el turno noche en UCI, el deterioro acelerado de la condición hemodinámica de un paciente crítico exige la intervención inmediata del médico responsable. Actualmente, el flujo de alerta depende de llamadas telefónicas manuales o avisos presenciales del personal de enfermería, lo que genera demoras en la respuesta.


* **Sustento Normativo y de Proceso:** La normativa de cuidados críticos de EsSalud exige atención continua e inmediata. La falta de un mecanismo de contacto activo y automatizado en la HCE expone la atención del paciente crítico a fallas de localización.


* **Requerimientos Específicos:**
1. **Protocolo de Contacto Inmediato:** Emisión de alertas push multiterminal de baja latencia dirigidas al médico internista/intensivista responsable.


2. **Plan de Escalado por Ausencia:** Mecanismo automático que, tras un tiempo sin respuesta (p. ej. 3 minutos), escale y reasigne la alerta al médico de retén o jefe de turno de la red.


3. **Persistencia en Crisis:** Guardado inmutable e instantáneo del diagnóstico y evolución del paciente "en todo momento", evitando pérdidas de datos durante emergencias.

### B. Sincronización de Turnos sin Cruces y Rotación Médica

* **Diagnóstico Operativo y Evidencia de Auditoría:** Informes de control de la Contraloría General de la República en hospitales clave de EsSalud (Almenara, Rebagliati, Sabogal, entre otros) revelaron que el **75% de nosocomios supervisados no ejerce un control efectivo de asistencia y permanencia en la UCI**, y el **50% presenta fallas en la publicación e integración de la programación de turnos médicos**.
* **Impacto del Problema de Rotación:** La alta rotación de médicos internistas provoca discontinuidad informativa, donde el profesional entrante no dispone inmediatamente de la valoración médica realizada en el turno previo.


* **Requerimientos Específicos:**
1. **Motor Anti-Cruces de Horarios:** Validación automatizada de reglas que impida técnicamente el registro de turnos duplicados o solapados.


2. **Handover Obligatorio:** Cierre condicionado de guardia para el médico saliente, exigiendo el registro de la nota de entrega de turno y diagnóstico antes del cambio de personal.

### C. Comunicación y Actualización en Tiempo Real

* **Diagnóstico Operativo:** En una UCI, los parámetros fisiológicos y los diagnósticos médicos sufren variaciones constantes. Los sistemas tradicionales de HCE que funcionan por consulta/refresh manual generan desfases de información entre médicos y enfermeras.


* **Requerimientos Específicos:**
1. **Notificaciones Push Multicanal:** Arquitectura orientada a eventos para notificar instantáneamente al personal de turno sobre cambios en las indicaciones médicas o estados del paciente.


2. **Estrategia de Persistencia Distribuida:** Replicación de baja latencia que garantice la consulta sincrónica del diagnóstico actualizado en todas las pantallas de la unidad de forma ininterrumpida.

## 3. Matriz Comparativa: Situación Actual EsSalud vs. Meta del Nuevo Sistema

| Dimensión | Estado Actual en Red EsSalud (ESSI / Práctica) | Meta de la Arquitectura UCI Propuesta |
| --- | --- | --- |
| **Programación de Guardias** | Desorden en programación y deficiencia en el control de permanencia en UCI (Contraloría). | Motor de reglas de programación que garantiza cero cruces de horario y manejo óptimo de guardias.
| **Continuidad Diagnóstica** | Transferencia de información susceptible a pérdidas por alta rotación de internistas. | Transferencia obligatoria de diagnóstico previa al cambio de turno, accesible para el siguiente médico.
| **Alerta Nocturna ("Medianoche")** | Llamadas telefónicas manuales sin garantía de respuesta o escalamiento automático. | Alerta push multicanal automatizada con escalamiento a médico de retén si el responsable no responde.
| **Sincronización de Datos** | Carga y consulta de historias clínicas mediante refresco manual en la plataforma. | Persistencia en tiempo real distribuida, con notificaciones push instantáneas a enfermeras y médicos.
| **Escala de Operación** | Infraestructura orientada a redes asistenciales locales/regionales. | Arquitectura elástica proyectada de 1K a 100K (6 meses) y 10M de hospitales (2 años).
| **Rendimiento y SLAs** | Tiempos de inicio variables dependientes de red y servidor asistencial. | Inicio $< 1$s, Carga Config. $< 5$s, Disponibilidad $99.9\%$, Recuperación (RTO) $< 5$ min.
