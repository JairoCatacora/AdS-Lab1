# Arquitectura: Sistema de Gestión de Red UCI de EsSalud

## Idea General

La arquitectura combina dos formas de trabajar, según lo que necesita cada acción:

- Cuando algo debe impedirse o garantizarse antes de continuar, la acción se resuelve al momento, de forma directa y con validación inmediata (síncrona).
- Cuando algo debe avisarse o propagarse a otros, el sistema emite un **evento** que viaja de forma asíncrona a quienes deban enterarse (médicos, enfermeras, tablero de guardia), sin que el que originó el cambio tenga que esperar a que todos lo reciban.

## Cómo funcionaría

1. Cada dispositivo (celular/tablet) guarda primero localmente lo que el médico o enfermera registra, para que nunca se sienta lento ni dependa de la red.
2. Ese registro luego se sincroniza hacia el servidor en segundo plano, y al llegar dispara un evento.
3. Ese evento es lo que hace que, en paralelo, se actualicen las pantallas de otros médicos/enfermeras, se manden notificaciones push, y se refresque el tablero de disponibilidad, todo esto sin bloquear a nadie.
4. Casos como una alerta nocturna funcionan igual: se dispara un evento apenas ocurre, y si nadie responde en cierto tiempo, el mismo mecanismo de eventos escala automáticamente al siguiente médico disponible.
5. Casos como asignar un turno sí se validan al momento (no por evento), porque ahí es necesario decir "sí" o "no" de inmediato para evitar cruces de horario.

## Por qué usar EDA

- Encaja con los requerimientos de notificaciones push en tiempo real y bajo.
- Permite que el sistema siga funcionando, de forma local, aunque haya caídas de red o de servidor.
- Facilita escalar de 1,000 a 10 millones de hospitales, porque cada notificación/evento se puede manejar de forma independiente por cama o por hospital, sin saturar un solo punto central.