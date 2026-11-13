# Plan y solicitud de créditos

[English](../en/roadmap-and-credits-ask.md) · [Volver al README](../../README.es.md)

SuperLeads atiende a cerca de 100 colegios y universidades y ha crecido 100% bootstrapped. Queremos que la siguiente
etapa de ese crecimiento corra en AWS: **Constanza** (nuestra IA) en Bedrock, **nuestro propio modelo de admisiones**
en SageMaker y las **llamadas** en Transcribe o en GPU de NVIDIA, todo anclado en **mx-central-1**.

La primera fase del laboratorio gastó su tope interno de GPU ($299.51 de $300 USD) y dejó un Nemotron entrenado que le
gana, en un examen sin conexión, a la IA de admisiones que usan hoy las escuelas. Falta probarlo con personas y en
producción, y abrir la línea de llamadas.

**Todos los costos de abajo son estimaciones** basadas en lo que costaron nuestros jobs y en precios públicos de lista
(ml.g6e.2xlarge ~$2.80 USD/h, ml.g6e.12xlarge ~$13.12 USD/h, ml.p5.4xlarge ~$7.91 USD/h). Spot y créditos cambian el
monto real.

## Cómo crecen nuestras cargas en AWS junto con nosotros

| Línea | Servicios de AWS | Hoy (medido) | Siguiente etapa (plan) | Qué la hace crecer |
|---|---|---|---|---|
| **Constanza, operación** | Bedrock (Claude), EC2 mx-central-1 | 54 rutinas activas, un equipo de "empleados" de IA especializados, ~15,600 llamadas al modelo desde oct-2026 | 6 meses más (~$9.3k USD, est.) | Más canales, rutinas y personas en el equipo |
| **Constanza, conversaciones de admisiones** | Bedrock (harness, turnos difíciles), endpoint de SageMaker (Nemotron) | Examen sin conexión: 8.2 (Claude Fable 5.2 + harness), 6.7 (nuestro Nemotron v5 + harness) | Piloto en una escuela (~$7k USD, est.) | El volumen de prospectos de cada institución |
| **Nuestro propio modelo** | GPU de SageMaker, S3, ECR | 29 jobs, 24.8 h, $299.51 USD; ~193 GB en S3 | Laboratorio (~$0.9k–2.1k USD, est.); Nemotron más grande en H100 | Calidad del modelo y costo de servir por conversación |
| **Llamadas** | Amazon Transcribe o ASR de NVIDIA en GPU; Bedrock para la analítica | 208,751 registros de llamadas, 621 h de más de 30 s, ninguna transcrita | Transcribir el acumulado, analítica de llamadas, piloto de agente de voz (~$0.1k–1.05k USD, est., más lo real del piloto) | Horas de llamadas por institución |

De ~100 instituciones a varias veces eso, estas cargas crecen con las conversaciones y las horas de llamadas; la meta
de diseño es que el equipo no tenga que crecer en la misma proporción. Reportaremos el costo real por institución
cuando corra el piloto, en lugar de proyectarlo hoy.

## El plan, en orden

```mermaid
flowchart LR
  A[1. Coincidencia humana<br/>con el juez] --> B[2. Prueba en vivo]
  B --> C[3. Que el modelo entrenado<br/>quepa en 1 GPU]
  C --> D[4. Datos oficiales<br/>de cada escuela]
  D --> E[5. Destilación v6]
  E --> F[9. Piloto en una escuela]
  D --> G[6. Maestro NVIDIA más grande]
  G --> E
  H[7. Panel de varios jueces] --> E
  I[8. Voz: transcribir llamadas] --> L[11. Analítica de llamadas<br/>para coaching]
  I --> J[12. Agente de voz con<br/>el harness de Constanza]
  F --> K[Reparto: Nemotron para lo simple,<br/>modelo grande para lo difícil]
```

## Experimentos y costo estimado

| # | Experimento | Qué pregunta contesta | Recursos | Costo est. (USD) |
|---|---|---|---|---|
| 1 | Prueba a ciegas con personas: 30 pares, 2 expertos en admisiones | ¿El juez de IA coincide con personas (meta ≥80%)? | Nuestra página | 0 |
| 2 | Prueba en vivo: 10 sesiones de una hora escribiendo como prospecto | ¿Cómo se porta fuera del examen (precios, groserías, preguntas raras)? | g6e.12xlarge, 10 h | ~130 |
| 3 | Cuantizar v5 a NVFP4 y repetir el examen en 1× L40S | ¿El modelo entrenado puede servirse a $2.80/h en lugar de $13.12/h? | g6e.12xlarge 1 h + g6e.2xlarge 1 h + 1 pasada del juez | ~30 |
| 4 | Fichas con datos oficiales; repetir el examen de 3 sistemas | ¿Cuánto del 5–10% que inventa es culpa de la ficha? | Bedrock + 1 examen en GPU | ~60 |
| 5 | Destilación v6: mejores fichas, más espacio para razonar, 3 variantes (rango, vueltas, mezcla de datos) | ¿Qué tan cerca de 8.0 puede llegar Nemotron? | 3 × (maestro + LoRA + unión + examen), v5 costó $70 cada uno | ~250 |
| 6 | Un Nemotron más grande como maestro (y alumno), varias H100 | ¿Un maestro NVIDIA más grande cierra la distancia con los modelos cerrados? | 8× H100 por ~6 h | ~300–600 |
| 7 | Panel de jueces que incluya modelos que no sean de Anthropic | Quitar el sesgo de familia del juez | Bedrock, 3 jueces × 3 sistemas | ~100 |
| 8 | Voz: transcribir 50 llamadas y luego las 621 h | ¿El reconocimiento de voz entiende el español de México por teléfono? | ASR de NVIDIA (Parakeet / Canary) en L40S (~20 h de GPU) o Amazon Transcribe (~$894 de lista por 621 h) | ~60 (GPU) a ~900 |
| 9 | Piloto en una escuela, 3 meses, prospectos reales | **¿Agenda más visitas que hoy?** | Endpoint siempre encendido en 1× L40S (2,190 h) + Bedrock para lo difícil | ~7,000 |
| 10 | Mantener a Constanza (operación) 6 meses | Contenido, SEO, señales comerciales, supervisión | Bedrock Opus (ritmo de nov ~$1,250/mes de lista) + EC2 (~$300/mes) | ~9,300 |
| 11 | Analítica de llamadas para coaching de admisiones sobre las 621 h transcritas | ¿Qué hacen por teléfono los asesores cuyas llamadas terminan en visita? | Bedrock sobre ~8M tokens de transcripción (supone ~150 palabras habladas por minuto) | ~50–150 |
| 12 | Piloto de agente de voz: el harness de Constanza detrás de nuestros agentes de voz en Dapta, una institución, 1 mes | ¿Llamar minutos después del registro agenda más visitas que hoy? | Bedrock para el harness; telefonía por Dapta (fuera de AWS) | Reportar lo real |

**Laboratorio (1–8): ~$0.9k–2.1k USD. Analítica de llamadas (11): ~$50–150 USD. Piloto (9): ~$7k USD. Operación (10):
~$9.3k USD por 6 meses. Piloto de voz (12): se mide, no se proyecta.**

Los créditos de AWS que tenemos hoy (cerca de $9k USD de los $10k USD de créditos AWS Activate Portfolio que obtuvimos
por NVIDIA Inception en oct 2026; saldo según el equipo, no se pudo verificar desde el servidor) cubren la operación de
Constanza y los primeros experimentos. **El piloto, tener el modelo siempre encendido y la línea de llamadas es lo que
destraban más créditos.**

## Qué permiten más créditos, en concreto

- **AWS (Activate / créditos):** el piloto de 3 meses en SageMaker (endpoint que se apaga fuera de horario), Bedrock
  para los turnos difíciles de Constanza y el panel de jueces, Transcribe o ASR en GPU para las llamadas, la analítica
  de llamadas y 6 meses más de la operación de Constanza. Si el piloto funciona, el mismo stack se lleva institución
  por institución en México y LATAM, desde mx-central-1.
- **NVIDIA (Inception):** capacidad de H100 o DGX Cloud para el experimento del maestro grande; NIM para servir; guía
  técnica sobre fine-tuning de Nemotron MoE, servir en NVFP4 y ASR telefónico en español; un caso de estudio conjunto
  cuando el piloto tenga resultados.

## El siguiente nivel de créditos que pedimos

AWS lista **AWS Credits for AI Startups** (US$200k+, solo por invitación, para startups listas para escalar después de
Activate Portfolio) como el nivel que se gestiona con el Account Manager de AWS de la startup. Esa es la conversación que
este repositorio respalda: el piloto (9), seis meses de operación (10) y la línea de llamadas (8, 11, 12) son los usos
concretos, y son las cargas que crecen con cada institución que sumamos.

## Decisiones y dependencias (que no son dinero)

- Revisión legal para entrenar con conversaciones anonimizadas de clientes; cláusula en los contratos con escuelas.
- Permiso por escrito de Anthropic antes de usar respuestas de Claude o Fable para entrenar (mientras tanto, solo NVIDIA).
- Permisos de escritura en el CRM de la escuela (mandar mensajes, crear citas) para operar en vivo.
- Una escuela piloto que ya registre sus citas en el CRM.
- Reglas de aviso y consentimiento para transcribir y analizar llamadas grabadas; las grabaciones se anonimizan igual
  que los chats.

## Criterios de éxito que nos comprometemos a reportar

| Etapa | Métrica | Meta |
|---|---|---|
| Prueba con personas | Coincidencia con el juez de IA | ≥ 80% de los pares |
| Examen | Invita / da precio / preferida frente a la IA de hoy | ≥ 90% / 0% / ≥ 80% |
| Piloto | Parte de los leads que agenda, contra el periodo anterior de la misma escuela y contra la IA de hoy | Reportar la mejora con intervalo de confianza, sea cual sea |
| Llamadas | Tasa de error de palabra del ASR en audio telefónico en español de México; visitas agendadas por el agente de voz | Reportar lo real |
| Costo | Costo de servir por conversación y por institución | Reportar lo real |
