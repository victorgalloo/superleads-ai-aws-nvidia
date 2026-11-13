# Impacto: por qué importa

[English](../en/impact.md) · [Volver al README](../../README.es.md)

## Quiénes somos

SuperLeads es el sistema de CRM y admisiones para colegios y universidades privadas en México y Latinoamérica.

- **Cerca de 100 instituciones atendidas** (colegios y universidades). Según la prensa, pasamos de 5 escuelas
  (nov-2024) a 54 (nov-2025) y a 100 (sep-2026).
- **Nuestro activo más importante es nuestra gente y nuestro know-how en admisiones educativas.** Las reglas que sigue
  nuestra IA (invitar a la visita, nunca dar precios por mensaje, nunca inventar datos) salen de ese conocimiento, y
  nuestros datos las confirman (ver abajo).
- **100% bootstrapped**, sin financiamiento externo. El gasto en la nube se mide job por job.
- **Misión:** que cada estudiante o familia interesada reciba una respuesta rápida y cálida y una invitación a la
  visita, a cualquier hora.

## El problema

Los colegios y universidades privadas llenan sus lugares con equipos de admisiones que contestan WhatsApp, chat web,
llamadas y correo. La mayoría de los prospectos se pierden no porque la escuela no les convenga, sino porque nadie
contestó a tiempo, nadie los invitó a la visita o nadie les dio seguimiento.

## Lo que dicen los datos (1.3M mensajes anonimizados, 86 mil prospectos, 80 escuelas)

Todas las comparaciones se hacen **dentro de cada escuela** y luego se promedian; mezclar escuelas invirtió un
resultado. Son asociaciones, no causas; para probar causa hace falta un piloto.

| Hallazgo | Número | Qué hace nuestra IA al respecto |
|---|---|---|
| Prospectos que escriben por primera vez fuera de horario (lun–vie 9 a 18, hora del centro) | **48 de 100** | Contesta a cualquier hora |
| Conversaciones en las que nadie invitó a la visita | **52 de 100** | Invita en ~90% de las respuestas (la IA de hoy: ~30%) |
| Agendan cuando alguien invitó vs cuando nadie invitó | **17 vs 7 de cada 100** (30 de 35 escuelas) | Cada respuesta acerca a la visita |
| Prospectos que preguntan el precio | 26 de 100 | Nunca lo da; invita a verlo en la visita |
| Precio preguntado al inicio: invitar sin precio vs dar el precio por mensaje | Agendan 15 vs 9 de cada 100 (7 de 9 escuelas; 1,999 casos) | Guardia en código: 0% de precios |
| Respuesta el mismo día vs después de un día | Agendan 10 vs 5 de cada 100 | Contesta al instante |
| De los que agendan: en 24 h / en la primera semana | 54 / 73 de cada 100 | La invitación tiene que llegar en la primera plática |
| El prospecto deja de contestar y nadie le vuelve a escribir | 43 de 100 | El seguimiento de varios días está en el plan |
| Leads que agendan una visita (101 escuelas, 354,602 leads, 12 meses) | **2.8%** | La métrica que queremos mover |
| Escuelas donde la IA de hoy agenda en el chat vs solo el personal | Agenda el 8.5% vs 2.8% de los leads | Escuelas distintas; es pista, no prueba |

## Constanza: una sola IA, de la operación a las admisiones

**Constanza** es la IA de SuperLeads. Hoy opera nuestro día a día 24/7 en Amazon EC2 (mx-central-1) con Claude en
Amazon Bedrock, construida sobre el framework abierto Hermes Agent.

- **Un equipo de "empleados" de IA especializados** que ella coordina: uno por red social, uno de SEO y sitios web,
  uno de clientes y alianzas. Todos se ven en nuestro tablero de supervisión.
- **50 rutinas programadas activas** (67 definidas) en un solo servidor EC2: contenido diario por red social,
  auditorías técnicas de SEO, piloto automático de SEO para dos sitios, revisión semanal de cómo responden los
  asistentes de IA sobre nuestro mercado, señales comerciales de datos públicos (directores de admisiones nuevos,
  programas nuevos, vacantes), investigación de alianzas y supervisión.
- **Se supervisa sola**: tablero en vivo, reportes de rutinas cada 30 minutos, revisión diaria de las rutinas que
  fallan y bitácora diaria, para que un equipo chico opere lo que de otro modo requeriría varias personas.
- **Recibe instrucciones por notas de voz de WhatsApp**; contesta con texto o voz.

**Constanza será nuestro harness.** El harness que se mide abajo (fichas por escuela, reglas, ejemplos de estilo,
guardia en código para precios y becas) es el que va a llevar las conversaciones de admisiones con los aspirantes. Con
Claude Fable 5.2 en Bedrock sacó 8.2 / 10; con nuestro Nemotron entrenado, 6.7. Es examen sin conexión, todavía no
producción.

## Mejoras medidas de nuestra IA de admisiones (examen a ciegas, sin conexión)

| Métrica | IA de hoy | Nuestro Nemotron entrenado (v5) | Mejor referencia (Fable 5.2) |
|---|---|---|---|
| Calidad global (juez 2) | 4.2 | 6.7 | 8.2 |
| Invita a la visita | 32% | 92% | 94% |
| Da un precio | 8% | **0%** | 0% |
| Inventa un dato | 34% | 5% | 2% |
| Preferida frente a la IA de hoy | — | 77% | 96% |

Es un examen sin conexión, no producción. Lo siguiente es comprobar que el juez coincide con personas y un piloto en
una escuela.

## Llamadas: la siguiente línea de crecimiento

Muchas admisiones se deciden por teléfono, y hoy las llamadas son un punto ciego.

| Concepto | Estado |
|---|---|
| Registros de llamadas en el CRM (solo metadatos) | **208,751 llamadas; 621 horas de más de 30 s** (medido) |
| Transcritas hasta hoy | Ninguna (medido) |
| Costo de transcribir las 621 h | ~$894 USD con Amazon Transcribe a precio de lista, o ~$60 USD con NVIDIA Parakeet / Canary en una L40S (~20 h de GPU) (**estimaciones**) |
| Analítica de llamadas para coaching de admisiones | **Plan**: qué dicen los mejores asesores y dónde se pierde la visita, calificado con nuestro método de examen a ciegas |
| Agentes de voz que agendan visitas al plantel | Hoy los construimos con Dapta (una sola meta por llamada: visita con día y hora). **Plan**: ponerles detrás el harness y las fichas de Constanza, con la misma guardia de precios y becas |

## Resultados medibles en nuestro propio proceso comercial

- **95% de asistencia en un día completo de juntas.** En un día de juntas seguidas con directores de colegios y
  universidades (de 10:00 a 16:00), se llevó a cabo el 95% de las citas agendadas. Es un hito de un día, publicado por
  el líder comercial, no un promedio sostenido. Él lo atribuyó a IA, automatizaciones y al equipo; entre las herramientas
  están el brief automático antes de cada junta, un motor de seguimiento que propone horarios reales y Constanza.
- **Cada junta de diagnóstico agendada recibe un brief de investigación** esa misma mañana, generado solo.
- **Motor de seguimiento**: cada comercial abre una tarjeta con a quién escribir, por qué ahora y el mensaje ya escrito;
  el CRM se actualiza solo.
- **Video casi sin costo**: videos de marca en 2 a 3 minutos, cerca de $1 peso por video promocional.

## Por qué debería importarles a AWS y NVIDIA

1. **Un cliente que crece, no un experimento.** ~100 instituciones, bootstrapped, con su IA corriendo ya en AWS en
   mx-central-1. Cada institución nueva suma carga de Bedrock, SageMaker y (pronto) voz.
2. **Un mercado real y poco atendido.** Admisiones en español en LATAM; pocos proveedores construyen modelos para eso.
3. **Un método que se repite.** Anonimizar → fichas → harness con guardia en código → examen a ciegas → destilar. Ya dio
   una mejora medible con un costo total de GPU de $299.51 USD.
4. **Eficiente por diseño.** El Nemotron destilado contesta con ~1/20 de los tokens del maestro, lo que hace costeable
   un endpoint siempre encendido para las escuelas.
5. **Privacidad y políticas desde la construcción.** Los datos personales nunca salen del anonimizador; las políticas
   de precios y becas se aplican en código; los datos se quedan en AWS.
6. **Medición honesta.** Reportamos los fracasos (dos entrenamientos que empeoraron el modelo) y el sesgo del juez junto
   a los logros.
