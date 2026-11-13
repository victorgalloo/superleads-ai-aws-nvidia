# La IA de SuperLeads sobre AWS + NVIDIA

**[Read in English](README.md)**

SuperLeads construye sistemas de CRM y admisiones para colegios y universidades privadas en México y Latinoamérica.
Este repositorio es una vitrina pública de lo que construimos con **créditos de AWS** y como miembros de
**NVIDIA Inception**: arquitectura, flujos, experimentos, números reales de uso y lo que queremos hacer después.

Contiene **solo documentación**: nada de código, nada de datos de clientes, ninguna credencial.

## En un párrafo

Entrenamos y evaluamos nuestra propia IA de admisiones sobre **NVIDIA Nemotron 3.5 Lightning (30B-A3B)** con GPU de
**Amazon SageMaker** (L40S) y el contenedor **NVIDIA NeMo AutoModel**, a partir de un corpus anonimizado de
**1.3 millones de mensajes de chat de 86 mil prospectos en 80 escuelas**. Armamos un examen a ciegas con 300 mensajes
reales de escuelas apartadas, calificado por Claude Opus en **Amazon Bedrock**. El tercer entrenamiento (destilación
maestro-alumno) llevó a Nemotron de **6.5 a 7.2 / 10** contestando al instante (juez 1), con **0% de precios** y cerca
de **1/20 de los tokens del maestro**. La IA de admisiones que usan hoy las escuelas saca 4.2–4.8.
En paralelo, un agente de operaciones 24/7 (Hermes) en **Amazon EC2** corre 50 rutinas programadas con
**Claude Opus en Bedrock**.

## Resultados principales (examen a ciegas, 300 turnos reales, 6 escuelas que el modelo nunca vio)

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
| [Arquitectura](docs/es/architecture.md) | Diagrama del sistema: agente de operaciones, datos, entrenamiento, productos |
| [Entrenamiento y evaluación](docs/es/training-and-evaluation.md) | **El caso destacado**: datos, pipeline, cinco intentos, resultados, lecciones |
| [Uso de AWS](docs/es/aws-usage.md) | Cada servicio de AWS, cómo lo usamos y los números reales que pudimos verificar |
| [NVIDIA](docs/es/nvidia.md) | Nemotron, NeMo AutoModel, NVFP4, L40S/H100 y cómo usamos Inception |
| [Impacto](docs/es/impact.md) | Por qué importa para las admisiones educativas en LATAM, con hallazgos medidos |
| [Plan y solicitud de créditos](docs/es/roadmap-and-credits-ask.md) | Experimentos concretos, costo estimado, qué destraban más créditos |
| [Bitácora de cambios](docs/es/changelog.md) | Registro semanal del trabajo nuevo |

## Números de un vistazo (verificados con nuestros registros, nov-2026)

| Concepto | Valor |
|---|---|
| Jobs de GPU en SageMaker (us-east-1) | 29 jobs, 24.8 horas facturables, **$299.51 USD** (estimado a precio de lista; tope interno $300) |
| Llamadas a Bedrock del examen | 11,932 llamadas, 52.9M tokens de entrada / 3.8M de salida |
| Bedrock del agente de operaciones (desde oct-2026) | ~14,400 llamadas al modelo, ~$990 USD a precio de lista (estimación del propio agente) |
| Corpus de entrenamiento en S3 | 1 bucket, 1,432 objetos, ~193 GB (datos anonimizados, checkpoints, salidas) |
| Servidor siempre encendido | 1× EC2 m7i.2xlarge, mx-central-1 (México) |
| Rutinas programadas del agente | 67 definidas, 50 activas |

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

Victor Jiménez Gallo, Enrollment Growth Executive, SuperLeads, [superleads.mx](https://superleads.mx).
