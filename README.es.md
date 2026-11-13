# SuperLeads: crecer junto con AWS y NVIDIA

**[Read in English](README.md)**

SuperLeads es el sistema de CRM y admisiones para colegios y universidades privadas en México y Latinoamérica.
Este repositorio muestra lo que construimos con **créditos de AWS** y como miembros de **NVIDIA Inception**, lo que
ya hace por nuestro negocio y hacia dónde queremos crecer con AWS.

Contiene **solo documentación**: nada de código, nada de datos de clientes, ninguna credencial.

## Por qué SuperLeads

- **Misión.** Ayudar a los colegios y universidades privadas a llenar sus lugares, asegurando que cada estudiante o
  familia interesada reciba una respuesta rápida y cálida y una invitación a conocer el plantel, a cualquier hora.
- **Cerca de 100 instituciones atendidas** (colegios y universidades) en México y LATAM.
- **Nuestro activo más importante es nuestra gente y nuestro know-how en admisiones educativas.** La IA se construye
  sobre ese conocimiento y se mide con las reglas que usan nuestros expertos en admisiones.
- **100% bootstrapped.** Hemos crecido sin financiamiento externo. Cada dólar de nube tiene que ganarse su lugar, y
  por eso medimos todo lo que aparece abajo.
- **Un mercado real y poco atendido.** Admisiones en español en LATAM, donde 48 de cada 100 prospectos escriben por
  primera vez fuera de horario y en 52 de cada 100 conversaciones nadie los invita a la visita.

## Crecer junto con AWS

**Lo que construimos con los créditos hasta hoy (oct–nov 2026):**

- **Constanza, nuestra IA, encendida 24/7** en Amazon EC2 en **mx-central-1 (México)** con Claude en **Amazon Bedrock**.
- **Nuestro propio modelo de admisiones**: NVIDIA Nemotron entrenado con GPU de **Amazon SageMaker** (29 jobs,
  $299.51 USD a precio de lista), a partir de un corpus anonimizado de 1.3M mensajes en **Amazon S3**.
- **Un examen a ciegas** de 300 mensajes reales de prospectos, calificado en Bedrock, que nos dice a qué sistema
  confiarle una escuela.

**Lo que crece con nosotros.** Cada institución nueva suma conversaciones, llamadas y rutinas. El stack está hecho para
que pasar de ~100 instituciones a varias veces eso sume carga en la nube, no personal en la misma proporción.

| Carga | Servicio de AWS | Hoy (medido) | Al crecer (plan; los costos son estimaciones) |
|---|---|---|---|
| Constanza, operación | Bedrock (Claude) + EC2 mx-central-1 | ~15,600 llamadas al modelo desde oct; 54 rutinas activas | Más rutinas y canales; ~$9.3k USD los próximos 6 meses |
| Constanza, conversaciones de admisiones | Bedrock (harness) + endpoint de SageMaker | Solo examen sin conexión: 8.2 / 10 contra 4.2 de la IA de hoy | Piloto en una escuela ~$7k USD por 3 meses, luego institución por institución |
| Nuestro propio modelo | GPU de SageMaker (L40S, H100) + S3 | 29 jobs, 24.8 h, ~193 GB en S3 | Siguiente fase del laboratorio ~$0.9k–2.1k USD; Nemotron más grande en H100 |
| Llamadas | Amazon Transcribe o ASR de NVIDIA en GPU | 621 h de llamadas en el CRM, ninguna transcrita | ~$60 USD (GPU) a ~$900 USD (Transcribe) por el acumulado; crece con cada institución |

La región importa: Constanza ya corre en **mx-central-1** y queremos ser un caso de referencia para ella.
Plan y solicitudes: [Plan y solicitud de créditos](docs/es/roadmap-and-credits-ask.md).

## Constanza, la IA de SuperLeads

Constanza es el nombre de nuestra IA. Hoy es nuestro **agente de operaciones**, construido sobre el framework abierto
Hermes Agent y encendido 24/7 en EC2 (mx-central-1) con Claude en Bedrock.

- **Coordina un equipo de "empleados" de IA especializados**: uno por red social, uno de SEO y sitios web, uno de
  clientes y alianzas. Cada uno se ve en nuestro tablero de supervisión, con sus rutinas, reportes y fallas.
- **Corre ~54 rutinas activas** (71 definidas): contenido diario, SEO técnico, visibilidad en respuestas de IA, señales
  comerciales públicas, investigación de alianzas, vigías del entrenamiento y autosupervisión.
- **Recibe instrucciones por notas de voz de WhatsApp** y contesta con texto o voz.

**Constanza será nuestro harness.** El mismo harness (fichas por escuela, reglas, ejemplos de estilo y una guardia en
código que bloquea precios y becas) sacó **8.2 / 10** con Claude Fable 5.2 en Bedrock en nuestro examen a ciegas,
contra 4.2 de la IA de admisiones que usan hoy las escuelas, con **0% de precios** e invitación a la visita en el 94%
de las respuestas. Ese harness es el que va a llevar las conversaciones de admisiones con los aspirantes: primero en
chat, luego por teléfono, con un solo nombre. Nuestro Nemotron entrenado saca 6.7 dentro del mismo harness con ~1/20
de los tokens de su maestro; el plan es un reparto (Nemotron para lo simple, Claude en Bedrock para lo difícil).
Todavía no ha hablado con prospectos reales.

## Llamadas: nuestra siguiente línea de crecimiento

Las admisiones se siguen ganando por teléfono. Nuestro CRM tiene **208,751 registros de llamadas; 621 horas son
llamadas de más de 30 segundos**, y ninguna está transcrita. Las llamadas abren tres líneas de trabajo (todo es
**plan**, con costos estimados):

1. **Transcripción** con Amazon Transcribe (~$894 USD de lista por las 621 h) o NVIDIA Parakeet / Canary en una L40S
   (~20 horas de GPU, ~$60 USD), después de una prueba de 50 llamadas con audio telefónico en español de México.
2. **Analítica de llamadas para coaching de admisiones**: qué dicen los mejores asesores y en qué momento se pierde la
   visita, medido con el mismo método de examen a ciegas que usamos en chat.
3. **Agentes de voz que agendan visitas al plantel.** Ya construimos agentes de voz de admisiones con Dapta (una sola
   meta por llamada: visita con día y hora). Lo siguiente: ponerles detrás el harness y las fichas de Constanza.

Detalle: [Impacto](docs/es/impact.md) y [Plan](docs/es/roadmap-and-credits-ask.md).

## La prueba: nuestra propia IA de admisiones sobre NVIDIA + AWS

Entrenamos nuestra propia IA de admisiones sobre **NVIDIA Nemotron 3.5 Lightning (30B-A3B)** con GPU de **Amazon
SageMaker** (L40S) y el contenedor **NVIDIA NeMo AutoModel**, a partir de un corpus anonimizado de **1.3 millones de
mensajes de chat de 86 mil prospectos en 80 escuelas**. El tercer entrenamiento (destilación maestro-alumno) llevó a
Nemotron de **6.5 a 7.2 / 10** contestando al instante (juez 1), con **0% de precios** y cerca de **1/20 de los tokens
del maestro**.

**Resultados principales** (examen a ciegas, 300 turnos reales, 6 escuelas que el modelo nunca vio)

| Sistema | Nota (juez: Opus 5.5) | Invita a la visita | Da un precio | Inventa un dato | Preferida frente a la IA de hoy |
|---|---|---|---|---|---|
| IA de admisiones que usan hoy las escuelas (integrada al CRM, GPT-4.1) | 4.2 | 32% | 8% | 34% | — |
| Nemotron sin entrenar + nuestro harness (sin razonar) | 5.8 | 70% | 0% | 22% | 69% |
| **Nemotron entrenado por SuperLeads (v5) + harness, razonando** | **6.7** | **92%** | **0%** | **5%** | **77%** |
| Claude Sonnet 5.5 + harness (Bedrock) | 7.8 | 94% | 0% | 2% | 93% |
| Claude Fable 5.2 + harness (Bedrock) | **8.2** | 94% | 0% | 2% | **96%** |

Los jueces son modelos de Anthropic y pueden favorecer a los de su familia; ninguna persona ha calificado todavía.
Metodología y advertencias completas: [Entrenamiento y evaluación](docs/es/training-and-evaluation.md).

## Contenido

| Documento | Qué cubre |
|---|---|
| [Impacto](docs/es/impact.md) | Quiénes somos, qué dicen los datos de admisiones en LATAM, Constanza, llamadas |
| [Plan y solicitud de créditos](docs/es/roadmap-and-credits-ask.md) | Cómo crecemos con AWS: experimentos, piloto, llamadas, costo estimado |
| [Arquitectura](docs/es/architecture.md) | Diagrama del sistema: Constanza, datos, entrenamiento, productos |
| [Entrenamiento y evaluación](docs/es/training-and-evaluation.md) | Datos, pipeline, cinco intentos, resultados, lecciones |
| [Uso de AWS](docs/es/aws-usage.md) | Cada servicio de AWS, cómo lo usamos y los números reales que pudimos verificar |
| [NVIDIA](docs/es/nvidia.md) | Nemotron, NeMo AutoModel, NVFP4, L40S/H100 y cómo usamos Inception |
| [Bitácora de cambios](docs/es/changelog.md) | Registro semanal del trabajo nuevo |

## Números de un vistazo (verificados con nuestros registros, nov-2026)

| Concepto | Valor |
|---|---|
| Jobs de GPU en SageMaker (us-east-1) | 29 jobs, 24.8 horas facturables, **$299.51 USD** (estimado a precio de lista; tope interno $300) |
| Llamadas a Bedrock del examen | 11,957 llamadas, 53.1M tokens de entrada / 3.9M de salida |
| Bedrock de Constanza (desde oct-2026) | ~15,600 llamadas al modelo, ~$1,080 USD a precio de lista (estimación del propio agente) |
| Corpus de entrenamiento en S3 | 1 bucket, 1,432 objetos, ~193 GB (datos anonimizados, checkpoints, salidas) |
| Servidor siempre encendido | 1× EC2 m7i.2xlarge, mx-central-1 (México) |
| Rutinas programadas | 71 definidas, 54 activas |

Cómo se obtuvo cada número y qué **no** pudimos verificar (Cost Explorer no se puede leer desde el rol del servidor)
está en [Uso de AWS](docs/es/aws-usage.md).

## Principios

- **Privacidad primero.** Todo registro que sale del CRM se anonimiza dentro de un Worker de Cloudflare antes de
  llegar a AWS. Nombres, teléfonos, correos, identificadores y direcciones se reemplazan; los IDs se vuelven
  seudónimos HMAC con sal.
- **Reglas en código, no en buena voluntad.** El harness bloquea en código cualquier precio o beca; ninguna versión de
  nuestro modelo ha mandado uno.
- **Números honestos.** Lo estimado se marca como estimado. Los intentos fallidos se reportan junto al que funcionó.
- **Entrenamiento con licencia en regla.** Ninguna salida de un modelo de Anthropic se usa para entrenar a Nemotron.

## Contacto

Equipo comercial de SuperLeads, [superleads.mx](https://superleads.mx).
