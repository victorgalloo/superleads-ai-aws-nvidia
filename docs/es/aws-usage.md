# Uso de AWS

[English](../en/aws-usage.md) · [Volver al README](../../README.es.md)

## Cómo se obtuvieron estos números

| Fuente | Estado |
|---|---|
| Cost Explorer (`ce:GetCostAndUsage`) | **No se puede leer** desde el rol del servidor (AccessDenied). Por eso aquí no aparece el consumo de créditos por servicio; se puede agregar desde la consola de facturación. |
| `ec2:DescribeInstances`, `s3:ListAllMyBuckets`, `sagemaker:ListTrainingJobs`, métricas de CloudWatch | No permitidos para el rol del servidor (política de mínimo privilegio). |
| `sagemaker:DescribeTrainingJob` por job | Permitido. De ahí salen tipo de máquina, estado y **segundos facturables** de cada job. |
| Listado de objetos del bucket del proyecto | Permitido. De ahí salen el número de objetos y el tamaño. |
| Metadatos de la instancia (IMDS) | De ahí salen el tipo de máquina y la región del servidor. |
| Registros de tokens por llamada (nuestro código y el agente) | De ahí salen llamadas y tokens de Bedrock. |

Los montos en dólares son **estimaciones**: segundos facturables o tokens × precio público de lista. El cargo real (y
los créditos aplicados) los da la facturación de AWS.

## Servicios y cómo los usamos

| Servicio | Región | Cómo lo usamos | Uso verificado |
|---|---|---|---|
| **Amazon EC2** | mx-central-1 | Un m7i.2xlarge (8 vCPU, 30 GiB, disco de 193 GB) corre a Constanza, nuestro agente de operaciones (construido sobre Hermes Agent), 24/7: gateway de WhatsApp, agenda de rutinas, orquestadores de GPU, scripts de extracción y análisis | 1 instancia, siempre encendida. ~$294 USD al mes a precio de lista de us-east-1 (estimado; el precio en la región de México puede variar) |
| **Amazon Bedrock** | us-east-1 (perfiles entre regiones y globales) | (a) Modelo del agente de operaciones: Claude Opus 5.5. (b) Jueces y candidatos de referencia del examen: Opus 4.7, Opus 5.5, Sonnet 5.5, Fable 5.1, Fable 5.2. (c) Investigación y redacción en el producto de prospección | Ver tablas abajo |
| **Amazon SageMaker** (training jobs) | us-east-1 | Fine-tuning LoRA de Nemotron, unión de adaptadores e inferencia por lotes con vLLM para exámenes, maestro y fichas. Spot cuando se puede | 29 jobs, 24.8 h facturables, $299.51 |
| **Amazon S3** | us-east-1 | Corpus anonimizado (particionado por cuenta seudónima y resultado), fichas, sets de entrenamiento, checkpoints LoRA, modelos unidos, salidas de jobs | 1 bucket, 3,273 objetos, ~205 GB |
| **Amazon ECR** | us-east-1 | Dos imágenes para SageMaker: vLLM 0.27.1 y NVIDIA NeMo AutoModel 26.08 (de NGC) | 2 repositorios/etiquetas |
| **IAM** | global | Rol de mínimo privilegio para el servidor; rol aparte para los jobs de SageMaker | — |

## Detalle de SageMaker (todos los jobs, us-east-1)

| Para qué | Jobs | Horas facturables | Costo estimado |
|---|---|---|---|
| Entrenamiento LoRA (v3, v4, v5 y dos pruebas del pipeline) | 5 con cobro | 19.75 | $259.17 |
| Inferencia por lotes: exámenes | 11 con cobro | 3.79 | $31.22 |
| Inferencia por lotes: respuestas del maestro (v5) | 1 con cobro | 0.46 | $6.05 |
| Inferencia por lotes: fichas escritas por Nemotron | 1 con cobro | 0.11 | $1.44 |
| Unir LoRA con el modelo base (CPU) | 3 | 0.68 | $1.64 |
| Laboratorio de datos sintéticos, vuelta s1 (1× L40S spot) | 1 con cobro | 0.83 | $2.31 |
| Laboratorio de datos sintéticos, vuelta s2 (corriendo al publicar) | 1 | — | $0 por ahora |
| Gemelos de la carrera por capacidad y jobs detenidos antes de arrancar | 8 | 0 | $0 |
| **Total** | **31** | **25.6** | **$301.82** |

Por tipo de máquina: ml.g6e.12xlarge (4× NVIDIA L40S) 22.3 h / $292.85; ml.g6e.2xlarge (1× L40S) 2.6 h / $7.34;
ml.r5.8xlarge 0.68 h / $1.64; ml.p5.4xlarge (1× H100) se pidió 4 veces, nunca hubo capacidad, $0.

Los jobs grandes usaron spot. Cada orquestador revisa el gasto acumulado antes de lanzar y detiene cualquier job que
espere demasiado por capacidad, para que no cobre después.

## Detalle de Bedrock: laboratorio de IA de admisiones (11 a 13-nov-2026)

| Modelo | Llamadas | Tokens de entrada | Tokens de salida | Papel |
|---|---|---|---|---|
| Claude Opus 4.7 | 5,702 | 24.3M | 0.69M | Juez 1 |
| Claude Opus 5.5 | 3,955 | 16.3M | 1.84M | Juez 2 |
| Claude Sonnet 5.5 | 1,497 | 4.8M | 0.65M | Candidato de referencia; etiquetó 296 conversaciones |
| Claude Fable 5.1 | 449 | 5.3M | 0.34M | Fichas para análisis; candidato de referencia |
| Claude Fable 5.2 | 354 | 2.3M | 0.35M | El candidato de referencia más fuerte |
| **Total** | **11,957** | **53.1M** | **3.87M** | |

A precio de lista, la parte de Opus y Sonnet ronda $265 USD (sin Fable, porque no tenemos su precio confirmado); solo
la pasada del juez 2 ronda $100 USD.

## Detalle de Bedrock: el agente de operaciones

Sesiones registradas en la base del agente desde octubre de 2026 (el agente pasó de una laptop a EC2 el 3-nov-2026 y
su historia se unió).

| Mes | Sesiones cobradas por Bedrock | Llamadas al modelo | Costo estimado (cuenta del propio agente, precio de lista) |
|---|---|---|---|
| Oct 2026 | 37 | 4,303 | $482 |
| Nov 2026 (al día 13) | 533 | 11,273 | $596 |

El caché de prompts hace casi todo el trabajo: ~2,200 millones de tokens de entrada leídos de caché contra ~2.4
millones sin caché. En el mismo periodo la agenda registró ~7,300 corridas que no llamaron a ningún modelo (rutinas
de solo script).

## Lo que no pudimos verificar

- Créditos consumidos y restantes, por servicio (requiere Cost Explorer o la consola de facturación).
- Horas de EC2 y transferencia de datos desde el arranque (requiere `ec2:Describe*` o facturación).
- Uso del agente antes de octubre de 2026 (no está en la base del agente).
- Llamadas a Bedrock de los productos en el borde (usan sus propias llaves y registros).
