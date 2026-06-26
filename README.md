# Contador IA — Tu asistente contable en Telegram con Hermes Agent

Monta tu propio contador inteligente que vive en Telegram: lleva tu caja, clientes y facturas, te manda reportes en PDF o Excel, y hasta te los explica con una nota de voz. Todo en tu propio servidor, hablándole como a una persona.

No necesitas saber programar. Solo copiar, pegar y hablarle por Telegram.

---

## Lo que vas a necesitar

- **VPS en Hostinger** plan KVM 2 o superior
- **Bot de Telegram** creado con [@BotFather](https://t.me/BotFather)
- **Tu Chat ID** de Telegram (con [@userinfobot](https://t.me/userinfobot))
- **Una API key de OpenAI** con un poco de saldo (en [platform.openai.com](https://platform.openai.com)) — es el "cerebro" del agente

> 💡 **Lo más importante de toda la guía:** usa **OpenAI** como cerebro, no el
> modelo gratis. Con el cerebro bueno todo funciona a la primera; con uno flojo,
> el agente se equivoca y se rinde.

---

## Paso 1 — Instalar Hermes Agent en Hostinger

1. En Hostinger ve a **VPS** → **Docker Manager** → **Catalog**
2. Busca **"Hermes Agent"** y selecciónalo
3. Usuario `hermes`, una contraseña segura, las casillas de API keys vacías
4. Click en **Deploy**

---

> 💡 **Opcional — HTTPS con Traefik:** Hostinger ofrece desplegar **Traefik**
> (botón **"Deploy Traefik"**) para darle un certificado SSL automático a tus
> contenedores, con HTTPS sin configurar nada. Es opcional; tu Hermes funciona
> igual sin esto.

---

## Paso 2 — Conectar el cerebro (OpenAI)

1. En el panel ve a **KEYS → PROVIDERS**
2. Busca **OpenAI** y pega tu **API key** (`sk-...`) en el campo
3. Confirma que abajo en la pantalla aparezca el modelo de OpenAI y **NO** un modelo `:free`

> ⚠️ Pega la API key en el formulario de PROVIDERS, **nunca en la terminal**
> (mete basura tipo `[200~` y da error). Y no uses el botón "OAuth/Login con
> ChatGPT" — tú tienes una API key, va en PROVIDERS.

---

## Paso 3 — Conectar Telegram (con QR)

1. Ve a **CHANNELS → Telegram**
2. Usa el botón **"SET UP WITH QR"** y escanea el código con tu celular
   (más fácil que copiar el token y el chat id a mano)
3. Dale a **Restart Gateway** hasta que el estado diga **Running**

**Prueba:** desde tu celular, escríbele `hola` a tu bot. Si te responde, ya está conectado.

---

## Paso 4 — Darle su personalidad (el SOUL, desde el panel)

El SOUL es la personalidad permanente del agente. Se edita directo en el panel:

1. Ve a **PROFILES** → menú de los 3 puntos → **EDIT SOUL.MD**
2. Pega esto y guarda:

```
Eres mi contador personal. Te llamas "Contador IA" y siempre hablas en español.

Órdenes que entiendes:
- "entrada [monto] [concepto]" → anota dinero que entró
- "salida [monto] [concepto]" → anota dinero que salió
- "cliente nuevo [nombre] [teléfono]" → registra un cliente
- "cargo [cliente] [monto] [concepto]" → anota lo que un cliente queda debiendo
- "abono [cliente] [monto]" → anota un pago de un cliente
- "factura [proveedor] [monto] [fecha]" → anota una factura por pagar
- "resumen" → reporte general (en PDF o Excel, según lo que pida)
- "reporte clientes", "reporte caja", "reporte facturas" → reportes (en PDF o Excel, según lo que pida)

Reglas:
- Cada vez que anoto algo, confírmalo y muestra cómo va el saldo.
- Los reportes siempre como archivo (nunca texto largo). Puedo pedirlos en PDF o en Excel: usa el formato que indique. Si no especifico, manda solo el PDF (con buen diseño).
- Si te pido llevar algo nuevo (inventario, nómina, etc.), créalo tú solo y manéjalo.
- Cuando aprendas algo nuevo permanente, AGRÉGALO a este SOUL. Solo agrega, nunca borres lo anterior, y avísame.
```

---

## Paso 5 — Memoria, voz y zona horaria (desde el chat)

Háblale por Telegram como a una persona.

**Mensaje 1 — que te conozca:**
```
Recuerda siempre: soy [TU NOMBRE], el dueño. Manejo todo en pesos colombianos ($1.000.000). Los reportes los quiero como archivo (nunca texto largo en el chat), y puedo pedírtelos en PDF o en Excel: dame el formato que te indique. Si no te digo cuál, mándame solo el PDF. Guárdalo en tu memoria.
```

**Mensaje 2 — voz y zona horaria:**
```
Dos ajustes: 1) Configura tu voz para que cuando me mandes audios me hables con voz de español de Colombia (voz es-CO-SalomeNeural); deja eso fijo en tu configuración. 2) Configura la zona horaria en America/Bogota. Confírmame los dos cuando los apliques.
```

> Cambia `[TU NOMBRE]` por tu nombre. Si no estás en Colombia, ajusta la moneda, la voz y la zona horaria.

---

## Paso 6 — Que se prepare (desde el chat)

```
Prepárate para llevar mi contabilidad. Guarda de forma ordenada y segura: el dinero que entra y sale (caja), mis clientes y lo que me deben, y las facturas por pagar con sus fechas. Asegúrate de poder hacer reportes bonitos en PDF y Excel.

Todas las mañanas a las 8 revisa si tengo facturas por vencer pronto (3 días o menos) y avísame por Telegram.

Cuando esté listo, escríbeme: "✅ Contador IA listo. Ya puedes usarme."
```

Cuando llegue el `✅ Contador IA listo`, ya puedes usarlo.

---

## Cómo usarlo en el día a día

### Registrar plata
```
entrada 500000 venta de producto
salida 120000 pago de arriendo
```

### Clientes
```
cliente nuevo Juan Pérez 3001234567
cargo Juan Pérez 200000 mercancía fiada
abono Juan Pérez 50000
```

### Facturas
```
factura Proveedor ABC 1500000 2026-07-01
```

### Reportes (elige el formato)
```
resumen
resumen en excel
reporte caja
reporte clientes
reporte facturas
```

### Pídele un audio
```
mándame el resumen y además explícame en una nota de voz cómo va el negocio
```

### También entiende lenguaje normal
```
oye, hoy vendí 200 mil de contado, anótalo
¿cuánto me debe Juan Pérez?
¿cuánto tengo en caja?
```

### Y crece contigo
```
ahora llévame también el inventario: nombre, cantidad y precio
agrega 50 camisetas a 30000 cada una
reporte inventario
```

El agente crea lo que necesite, lo maneja solo, y agrega las órdenes nuevas a su propia personalidad (SOUL) para no olvidarlas.

---

## Si algo no funciona

| Problema | Solución |
|---|---|
| **El agente se equivoca o se queda a medias** | El #1 de todos: asegúrate de tener **OpenAI** como cerebro. Mira abajo que NO diga `:free` |
| La API key da error 401 con basura `[200~` | La pegaste en la terminal. Pégala en el formulario de PROVIDERS |
| "Session token not available" al conectar OpenAI | Usaste el botón OAuth/Login. Ponla en PROVIDERS, no en OAuth |
| No llega el mensaje a Telegram | Revisa que el **Gateway** esté en **Running** |
| El audio no sale en voz colombiana | Revisa CONFIG → Text-to-Speech: la voz de `edge` debe ser `es-CO-SalomeNeural` |
| Las alertas llegan a hora rara | Confirma que la zona horaria quedó en `America/Bogota` |
| Se saltó un paso de la preparación | Dile: "te faltó esto, complétalo" |

---

## Créditos

Guía creada por [Fredy Ortegón](https://github.com/2025prosperidad)

Hermes Agent es un proyecto de [Nous Research](https://nousresearch.com/)
