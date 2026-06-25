# Contador IA — Tu asistente contable en Telegram con Hermes Agent

Monta tu propio contador inteligente que vive en Telegram, lleva tu caja, clientes, facturas y te manda reportes en PDF y Excel. Todo en tu propio servidor, sin depender de nadie.

## Requisitos

- **VPS en Hostinger** plan KVM 2 o superior
- **Dominio** apuntando al VPS (opcional pero recomendado para HTTPS)
- **Bot de Telegram** creado con [@BotFather](https://t.me/BotFather)
- **Tu Chat ID** de Telegram (lo sacas con [@userinfobot](https://t.me/userinfobot))
- **$10 de créditos en OpenRouter** (opcional, para modelos premium. Sin esto usas el modelo gratis de Nous Research)

---

## Paso 1 — Instalar Hermes Agent en Hostinger

1. En el panel de Hostinger ve a **VPS** → **Docker Manager** → **Catalog**
2. Busca **"Hermes Agent"** y selecciónalo
3. Configura:
   - `ADMIN_USERNAME`: `hermes`
   - `ADMIN_PASSWORD`: una contraseña segura
   - Nexos API Key: déjalo vacío
   - Oxylabs API Key: déjalo vacío
4. Click en **Deploy**

---

## Paso 2 — Configurar dominio con Traefik (opcional)

Hostinger trae Traefik preinstalado. Asigna un subdominio (ej: `hermes.tudominio.com`) al VPS para entrar con HTTPS en vez de una IP.

---

## Paso 3 — Ejecutar /setup

Entra al panel web de Hermes y ejecuta el comando:

```
/setup
```

Sigue el asistente paso a paso:
- **Provider**: Nous Research (gratis, viene por defecto)
- Activa el canal **Telegram**
- Pega tu **Bot Token** (de @BotFather)
- Pega tu **Chat ID** como Allowed Users y Home Channel

> **Importante:** pega los datos cuando el asistente te los pida. Nunca pegues textos largos directamente en la terminal.

---

## Paso 4 — Verificar conexión con Telegram

Envía este mensaje en el chat de Hermes:

```
Lee las variables TELEGRAM_BOT_TOKEN y TELEGRAM_CHAT_ID que ya están en tu
archivo .env. Con esos datos, envíame AHORA un mensaje de prueba a Telegram
que diga exactamente:

"✅ Canal Telegram verificado desde la terminal. Tu Contador IA está conectado."

No me pidas los datos: ya los tienes en el .env. Si el envío falla, muéstrame
el error exacto que devolvió Telegram para corregirlo.
```

Si te llega el mensaje a Telegram, el canal está funcionando.

---

## Paso 5 — Dar permisos autónomos con Kodee

Abre **Kodee** (la IA de Hostinger) y pega esto:

```
Para el contenedor de Hermes Agent recién instalado, ejecuta estos comandos
para dejarlo completamente autónomo:
1. Encuentra el nombre del contenedor con: docker ps --format "{{.Names}}"
2. Con ese nombre ejecuta:
   - Instala sudo dentro del contenedor
   - Agrega el usuario hermes a sudoers sin contraseña
   - Da permisos 777 al venv de Python
   - Da permisos 777 a /opt/data
   - Instala pip dentro del contenedor
```

---

## Paso 6 — Configurar zona horaria

Envía este mensaje por Telegram a tu agente:

```
Configura la zona horaria del sistema y de Python en horario de Colombia
(America/Bogota, UTC-5). Aplica estos cambios:

1. Deja el sistema operativo en zona horaria America/Bogota.
2. Asegúrate de que cualquier script o cron job que crees use America/Bogota
   para calcular fechas y horas.
3. Cuando termines, dime la fecha y hora actual del servidor para confirmar
   que ya está en hora de Colombia.
```

> Cambia `America/Bogota` por tu zona horaria si no estás en Colombia.

---

## Paso 7 — Configurar el SOUL (personalidad del agente)

Envía este mensaje por Telegram:

```
Escribe EXACTAMENTE el siguiente contenido en el archivo /opt/data/SOUL.md
(reemplaza lo que haya). Cuando termines, confírmame que quedó guardado:

Eres el asistente contable personal de [TU NOMBRE].
Tu nombre es "Contador IA". Siempre respondes en español.

Tu trabajo es administrar las finanzas del negocio. Usas una base de datos SQLite
dentro de /opt/data/ para guardar todo. Tú decides cómo organizar los archivos
y las tablas. Si algo no existe, lo creas.

COMANDOS BASE:
- "entrada [monto] [concepto]" → registra ingreso en caja
- "salida [monto] [concepto]" → registra egreso en caja
- "cliente nuevo [nombre] [teléfono]" → crea cliente
- "cargo [cliente] [monto] [concepto]" → agrega deuda a un cliente
- "abono [cliente] [monto]" → registra pago de un cliente
- "factura [proveedor] [monto] [fecha vencimiento]" → registra factura por pagar
- "resumen" → reporte general en PDF y Excel por Telegram
- "reporte clientes" → deudas en PDF y Excel por Telegram
- "reporte caja" → movimientos del mes en PDF y Excel
- "reporte facturas" → facturas pendientes en PDF y Excel

REPORTES:
- El PDF debe verse profesional: encabezado con el nombre del negocio y la fecha,
  tarjetas de resumen con totales, tablas con filas alternadas, colores para
  destacar (verde ingresos, rojo egresos), totales resaltados.
- El Excel acompaña al PDF como adjunto.

PUEDES CRECER Y MEJORAR:
- Estos comandos son el punto de partida, NO un límite.
- Si te pido "ahora llévame las cuentas de [algo nuevo]" (inventario, nómina,
  gastos por categoría, lo que sea), créalo tú: agrega tablas, columnas o
  archivos que necesites y empieza a administrarlo.
- Antes de crear algo nuevo, revisa lo que ya existe para no duplicar.

REGLAS PERMANENTES:
- Siempre confirma cada registro con el dato guardado
- Muestra saldo actualizado después de cada operación
- Los reportes siempre van como archivos adjuntos por Telegram (PDF y Excel)
- Nunca pongas datos contables como texto plano en el chat: siempre adjunta archivo
- Formato de moneda colombiano: $1.000.000
- Nunca inventes datos: solo reporta lo que está en la base de datos
```

> Cambia `[TU NOMBRE]` por tu nombre real. Cambia el formato de moneda si no usas pesos colombianos.

Espera la confirmación **"SOUL guardado"** antes de continuar.

---

## Paso 8 — Instalar el sistema contable

Envía este mensaje por Telegram:

```
Ya tienes permisos completos sobre /opt/data y la zona horaria configurada.
Configura mi sistema contable completo:

1. Instala las librerías que necesites para generar reportes en PDF (con diseño
   profesional) y Excel (openpyxl, reportlab o weasyprint/pdfkit, jinja2, pillow,
   python-telegram-bot y las que consideres).

2. Crea una base de datos SQLite en /opt/data/ con las tablas necesarias para
   manejar: caja (entradas y salidas), clientes con sus deudas, y facturas
   de proveedores con fechas de vencimiento.

3. Crea un sistema de alertas que me avise por Telegram cuando una factura
   esté a 3 días o menos de vencer. Que se ejecute automáticamente todos
   los días a las 8:00 am.

4. Guarda en tu memoria:
   - Prefiero reportes SIEMPRE como archivos adjuntos (PDF y Excel), nunca texto plano
   - Moneda: pesos colombianos, formato $1.000.000

5. Cuando todo esté listo envíame por Telegram:
   "✅ Contador IA listo. Comandos: entrada, salida, cliente nuevo, cargo,
   abono, factura, resumen, reporte clientes, reporte caja, reporte facturas"
```

Espera el mensaje de confirmación antes de usar el agente.

---

## Uso diario

Una vez configurado, habla con tu agente por Telegram como si fuera una persona:

### Registrar movimientos
```
entrada 500000 venta de producto
salida 120000 pago de arriendo
```

### Manejar clientes
```
cliente nuevo Juan Pérez 3001234567
cargo Juan Pérez 200000 mercancía fiada
abono Juan Pérez 50000
```

### Registrar facturas
```
factura Proveedor ABC 1500000 2026-07-01
```

### Pedir reportes
```
resumen
reporte caja
reporte clientes
reporte facturas
```

### Lenguaje natural (también funciona)
```
oye, hoy vendí 200 mil de contado, anótalo
¿cuánto me debe Juan Pérez?
¿cuánto tengo en caja?
¿cuál factura vence primero?
```

### Hacerlo crecer
```
ahora llévame también el inventario de productos: nombre, cantidad y precio
agrega 50 camisetas a 30000 cada una
reporte inventario
```

El agente crea las tablas y empieza a administrar lo que le pidas sin que toques código.

---

## Errores comunes

| Problema | Solución |
|---|---|
| Usé `/workspace` y no funciona | Usa `/opt/data/` — es la carpeta persistente |
| Los textos pegados en terminal salen con `[200~` | Pega en el formulario web o en Telegram, no en la terminal |
| El modelo se corta a mitad de respuesta | Carga créditos en OpenRouter. Los modelos `:free` tienen límite de 50 mensajes/día |
| Las alertas llegan a hora rara | Revisa que la zona horaria quedó en `America/Bogota` (Paso 6) |
| El agente se salta un paso del setup | Dile: "te faltó el paso X, complétalo" |
| No llega el mensaje a Telegram | Revisa que el Chat ID sea correcto (con @userinfobot) |

---

## Créditos

Guía creada por [Fredy Ortegón](https://github.com/2025prosperidad)

Hermes Agent es un proyecto open-source de [Nous Research](https://nousresearch.com/)
