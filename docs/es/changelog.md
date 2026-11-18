# Bitácora de cambios

[English](../en/changelog.md) · [Volver al README](../../README.es.md)

Registro semanal del trabajo nuevo en AWS y NVIDIA. Lo más nuevo arriba. Los números salen de nuestros registros; lo
estimado se marca. Lo actualiza una rutina semanal del agente de operaciones (ver `ROUTINE.md`).

## Semana del 16 al 20 de noviembre de 2026

**Visibilidad**
- **18-nov:** Publicado el caso en LinkedIn (en X desde el 17-nov).

## Semana del 9 al 13 de noviembre de 2026

**Créditos y programas**
- Documentamos los $10k USD en créditos AWS Activate Portfolio obtenidos por NVIDIA Inception (oct 2026) y el siguiente nivel que pedimos (AWS Credits for AI Startups, con nuestro Account Manager de AWS).
- Solicitamos el piloto de créditos de NVIDIA Brev para miembros de Inception.
- Reunión con AWS Startups agendada para el 20 nov 2026.

**IA de admisiones sobre NVIDIA Nemotron (SageMaker + Bedrock)**
- Se armó el corpus anonimizado en S3: 1.3M mensajes de chat, 86 mil prospectos, 80 escuelas (~193 GB con checkpoints).
- Fichas por escuela (precios y becas fuera, en código); las de entrenamiento las escribe Nemotron en GPU.
- Harness (reglas + ficha + guardia en código + un reintento) y examen a ciegas de 300 turnos en 6 escuelas apartadas.
- Cinco iteraciones: LoRA v3 y v4 (peor que sin entrenar, quedan como lección), harness v0 y v1, y **destilación LoRA
  v5: 7.2 contestando al instante contra 6.5 sin entrenar (juez 1), 7.4 razonando, 0% de precios**.
- Segundo juez, más estricto (Opus 5.5), y Fable 5.2 como referencia: Fable 8.2, Sonnet 7.8, Nemotron v5 6.7, IA actual
  del CRM 4.2.
- Análisis de todas las conversaciones cerradas (solo conteos): 48% de los prospectos escriben fuera de horario;
  invitar a la visita va con 2.4 veces más citas dentro de la misma escuela.
- Citas reales: 354,602 leads en 101 escuelas; agenda el 2.8%.
- Página en vivo del avance para el equipo, con buzón de ideas que contesta el agente.
- SageMaker: 29 jobs, 24.8 horas facturables, $299.51 USD (estimado a precio de lista). Bedrock del laboratorio:
  11,957 llamadas.

**Narrativa (13-nov)**
- **13-nov:** la vitrina ahora abre con la misión y la historia de crecimiento de SuperLeads (~100 instituciones, 100%
  bootstrapped, gente y know-how en admisiones), una sección «Crecer junto con AWS», Constanza (nuestra IA y futuro
  harness de admisiones) y las llamadas como siguiente línea de crecimiento (transcripción, analítica, agentes de voz;
  plan con estimaciones).

**Agente de operaciones**
- Vigía del entrenamiento (cada 15 min) con avisos por WhatsApp; sonda de disponibilidad de modelos en Bedrock (cada 3 h).
- El tablero del agente cambió de nombre y quedó detrás del inicio de sesión único; instrucciones por texto o nota de voz.
- Se creó la rutina semanal de esta vitrina (este repositorio).
- Cuatro rutinas nuevas (ya son 71 definidas, 54 activas): respuestas a comentarios en nuestros perfiles sociales tres
  veces al día, seguimiento con nuestro equipo de cuenta de AWS antes de la reunión del 20-nov, una publicación
  programada que anuncia esta vitrina (17-nov) y esta actualización semanal.
- Semana en Bedrock: 444 sesiones, 8,941 llamadas al modelo, ~$468 USD a precio de lista (estimación del propio agente).

**Productos**
- El inicio de sesión único ya cubre cinco apps internas: tablero del agente, página de avance del entrenamiento, el
  diagnóstico Rayos X de inscripciones, una bitácora y una app de contexto compartido.
- Sitio web: el caché en el borde quitó esperas de hasta 3.8 s en la primera carga de las páginas alojadas en el CRM;
  16 páginas débiles se unieron a su página principal; guías nuevas de admisiones y una sección de 20 preguntas
  escrita para respuestas de IA.
- Propuestas: revisión de salud del dominio remitente y reintentos ante el límite de envío de correo; cada propuesta
  tiene un asesor asignado que se entera del envío, la apertura y la firma.
- El producto de investigación de prospectos, el anonimizador del censo del CRM y el arnés de entrenamiento entraron a
  control de versiones (solo código; los datos quedan fuera).

## Semana del 2 al 6 de noviembre de 2026

- **3-nov:** el agente de operaciones pasó de una laptop a un EC2 m7i.2xlarge en mx-central-1, encendido 24/7 con
  Claude Opus 5.5 en Bedrock.
- Las rutinas de contenido, SEO, señales comerciales y supervisión se mudaron al servidor.

## Octubre de 2026

- **1-oct:** solicitud a NVIDIA Inception. **8-oct:** aceptados.
- Bedrock quedó como proveedor de modelos del agente de operaciones.
