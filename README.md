#Sistema de Soporte Automatizado con IA — Entre Ágiles

Sistema de centralización de comunicación con inteligencia artificial para Entre Ágiles, empresa colombiana de capacitación y certificaciones en Tecnologías de la Información. Automatiza la atención al cliente por Telegram con IA, enrutamiento a asesores según horario y bandeja unificada.

Costo operativo mensual: $0

📋 Descripción

Antes del proyecto, los asesores atendían clientes por WhatsApp personal de forma desorganizada, sin historial compartido ni cobertura garantizada. Este sistema reemplaza ese proceso con una solución completamente automatizada y self-hosted.

Flujo principal:

Cliente escribe al bot de Telegram
Chatwoot recibe el mensaje y notifica a n8n
n8n envía el mensaje a Groq (IA) para clasificación
Si la IA puede resolver → responde automáticamente
Si necesita asesor → verifica horarios y asigna al disponible
El asesor responde desde Chatwoot → cliente recibe en Telegram
🛠️ Stack tecnológico
Tecnología	Rol	Costo
Telegram Bot API	Canal de comunicación con clientes	$0
Chatwoot	Bandeja unificada para asesores	$0
n8n Community	Motor de automatización y lógica	$0
Groq + GPT-OSS 20B	Clasificación y respuesta con IA	$0
PostgreSQL	Base de datos de casos y conversaciones	$0
Redis	Cola de trabajos para Chatwoot	$0
Docker + Docker Compose	Containerización de servicios	$0
Ubuntu Server 22.04	Sistema operativo del servidor	$0
Nginx + Let's Encrypt	Proxy reverso con SSL	$0
DuckDNS	DNS dinámico gratuito	$0
MikroTik	Router con reglas NAT	Hardware propio
------------------------------------------------------
🏗️ Arquitectura
-Cliente (Telegram)
       │
       ▼
  Chatwoot ──── webhook ───► n8n
  (bandeja)                   │
       ▲                      ├──► Groq IA
       │                      │    (clasifica)
       └──── API ─────────────┤
             (respuesta)      ├──► Verifica horarios
                              │
                              └──► Asigna asesor
                                   en Chatwoot
-------------------------------------------------------                                   
Infraestructura de red
Internet
   │
   ▼
Movistar (IP pública)
   │ NAT
   ▼
MikroTik (192.168.1.57)
   │ dstnat puertos 80/443
   ▼
Servidor local (192.168.88.45)
   │ Ubuntu Server + Docker
   ├── Chatwoot :3000
   ├── n8n :5678
   └── PostgreSQL :5432
⚙️ Flujo n8n — 15 nodos
N1 Webhook ──► N2 Solo mensajes ──► Code JS ──► N4 Construir prompt
  Chatwoot        entrantes
                                                       │
                                                       ▼
                                                N5 Groq API
                                                       │
                                                N6 Parsear respuesta
                                                       │
                                               N7 ¿Resuelve la IA?
                                              ┌────────┴────────┐
                                           SÍ │                 │ NO
                                              ▼                 ▼
                                    N8 IA responde       N9 Hora Colombia
                                    vía Chatwoot                │
                                                        N10 Verificar
                                                        disponibilidad
                                                                │
                                                       N11 ¿Hay disponibles?
                                                      ┌─────────┴─────────┐
                                                   SÍ │                   │ NO
                                                      ▼                   ▼
                                             N13 Seleccionar       N12 Fuera de
                                                 asesor                horario
                                                      │
                                             N14 Asignar en Chatwoot
                                                      │
                                             N15 Confirmar al cliente
🤖 Asistente IA — Sara

El bot responde como Sara, asistente virtual de Entre Ágiles. Características:

Tono cálido y cercano, lenguaje colombiano
Recuerda el historial de la conversación (últimos 6 mensajes)
Detecta automáticamente si el cliente necesita un asesor humano
Categoriza casos: exámenes, plataforma, técnico, general
Maneja preguntas de conversación natural
Pide datos del cliente cuando hay un problema específico
📅 Enrutamiento por horarios

El sistema verifica en tiempo real qué asesor está disponible:

Asesor	Horario
Angel David Garzón	Lun–Vie 8am–12pm y 1pm–4:30pm, Sáb 8am–12pm
Edwar Ortiz Melo	Lun–Jue 1pm–9pm, Vie 9am–5:30pm, Sáb 8am–12pm
Stephania Ortiz	Lun–Mar 10am–8pm, Mié 3:30pm–8pm, Jue–Vie horario variable, Sáb 8am–12pm

Si ningún asesor está disponible, el cliente recibe un mensaje con el horario de atención.

🚀 Instalación
Requisitos previos
Ubuntu Server 22.04 LTS
Docker y Docker Compose
Node.js v20+ (para n8n)
Cuenta en Groq (gratuita): https://console.groq.com
Bot de Telegram creado con @BotFather
1. Clonar el repositorio
bash
git clone https://github.com/tu-usuario/entreagiles-soporte-ia
cd entreagiles-soporte-ia
2. Levantar Chatwoot
bash
cd chatwoot
docker compose up -d
3. Instalar y configurar n8n
bash
npm install -g n8n
export WEBHOOK_URL=https://tu-dominio.duckdns.org/
n8n start
4. Importar el flujo en n8n
Abrir n8n en http://localhost:5678
Menú → Import from file → seleccionar n8n/flujo_entreagiles.json
Reemplazar las credenciales marcadas como REEMPLAZAR_CON_*
5. Registrar webhook de Telegram
https://api.telegram.org/botTOKEN/setWebhook?url=https://tu-dominio/webhooks/telegram/TOKEN
🔑 Variables de configuración
Variable	Descripción
GROQ_API_KEY	API key de console.groq.com
CHATWOOT_TOKEN	Access token de Settings → Profile en Chatwoot
TELEGRAM_BOT_TOKEN	Token de @BotFather
WEBHOOK_URL	URL pública del servidor con n8n
📊 Resultados
Costo mensual: $0 (vs $116+ USD con soluciones SaaS equivalentes)
Ahorro anual estimado: $1,400 USD
Tiempo de respuesta IA: < 2 segundos
Cobertura: 24/7 con respuestas automáticas, horario laboral con asesores humanos
📁 Estructura del repositorio
entreagiles-soporte-ia/
├── README.md
├── n8n/
│   └── flujo_entreagiles.json
├── chatwoot/
│   └── docker-compose.yml
└── docs/
    └── arquitectura.md
👤 Autor

Angel David Garzon Prieto
Gestor de operaciones — Entre Ágiles
Proyecto de automatización e inteligencia artificial aplicada a operaciones de soporte.

📄 Licencia

MIT License — libre para usar, modificar y distribuir.
