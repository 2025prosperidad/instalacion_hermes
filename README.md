# Contador IA — Tu asistente contable en Telegram con Hermes Agent

Monta tu propio contador inteligente que vive en Telegram, lleva tu caja, clientes y facturas, y te manda reportes en PDF y Excel. Todo en tu propio servidor, hablándole como a una persona.

No necesitas saber programar. Solo copiar, pegar y hablarle por Telegram.

---

## Lo que vas a necesitar

- **VPS en Hostinger** plan KVM 2 o superior
- **Un dominio** apuntando al VPS (recomendado, para entrar con un nombre bonito)
- **Bot de Telegram** creado con [@BotFather](https://t.me/BotFather)
- **Tu Chat ID** de Telegram (lo sacas con [@userinfobot](https://t.me/userinfobot))
- **Una API key de OpenAI** con un poco de saldo (en [platform.openai.com](https://platform.openai.com)) — es el "cerebro" del agente

---

## Paso 1 — Instalar Hermes Agent en Hostinger

1. En el panel de Hostinger ve a **VPS** → **Docker Manager** → **Catalog**
2. Busca **"Hermes Agent"** y selecciónalo
3. Configura:
   - Usuario: `hermes`
   - Contraseña: una segura
   - Las casillas de API keys déjalas vacías
4. Click en **Deploy**

---

## Paso 2 — Conectar tu dominio (opcional)

Hostinger ya trae todo listo para ponerle un nombre bonito a tu agente con candado de seguridad. Asígnale tu dominio (ej: `hermes.tudominio.com`) para entrar con un nombre en vez de una IP.

---

## Paso 3 — Configurar con /setup

Entra al panel de Hermes y escribe:

```
/setup
```

Sigue el asistente:
- Activa el canal **Telegram**
- Pega tu **Bot Token** (de @BotFather)
- Pega tu **Chat ID** (de @userinfobot)

---

## Paso 4 — Conectar el cerebro (OpenAI)

1. Ve a la pestaña **PROVIDERS**
2. Pega tu **API key de OpenAI**
3. En **MODELS**, elige **gpt-4o**
4. Ve a **CHAT** y escríbele "hola" para confirmar que responde

> Un buen cerebro es clave. Con un modelo flojo el agente se equivoca; con este entiende todo a la primera.

---

## Paso 5 — Comprobar que te escribe por Telegram

En el chat de Hermes pega:

```
Mándame ahora un mensaje de prueba por Telegram que diga:
"✅ Hola, soy tu Contador IA y ya estamos conectados."
```

Si te llega a Telegram, vas perfecto.

---

## Paso 6 — Darle permiso para trabajar solo

Abre **Kodee** (la IA de Hostinger) y pega:

```
Para el contenedor de Hermes Agent recién instalado, déjalo completamente
autónomo: que el usuario hermes pueda usar sudo sin contraseña, que pueda
instalar librerías de Python y que pueda crear y modificar sus propios
archivos sin pedir permiso. Encuentra el contenedor con docker ps y aplica
los cambios necesarios.
```

---

## Paso 7 — Los 3 mensajes mágicos

Con solo 3 mensajes por Telegram, tu agente se convierte en tu contador.

### Mensaje 1 — Que te conozca

```
Quiero que recuerdes siempre estas cosas sobre mí y mi negocio:
- Soy [TU NOMBRE], el dueño.
- Manejo todo en pesos colombianos, por ejemplo $1.000.000.
- Cuando te pida un reporte, mándamelo siempre como archivo (PDF y Excel), nunca como un texto largo en el chat.
- Trabajo en horario de Colombia.

Guárdalo en tu memoria para que no se te olvide nunca y confírmame que lo guardaste.
```

### Mensaje 2 — Que sepa quién es

```
De ahora en adelante eres mi contador personal. Te llamas "Contador IA" y siempre me hablas en español. Guarda esto como tu personalidad permanente.

Estas son las órdenes que voy a usar contigo:
- "entrada [monto] [concepto]" → anota dinero que entró
- "salida [monto] [concepto]" → anota dinero que salió
- "cliente nuevo [nombre] [teléfono]" → registra un cliente
- "cargo [cliente] [monto] [concepto]" → anota lo que un cliente me queda debiendo
- "abono [cliente] [monto]" → anota un pago que me hizo un cliente
- "factura [proveedor] [monto] [fecha]" → anota una factura que tengo que pagar
- "resumen" → me mandas un reporte general en PDF y Excel
- "reporte clientes", "reporte caja", "reporte facturas" → reportes específicos en PDF y Excel

Reglas que nunca debes romper:
- Cada vez que anoto algo, confírmamelo y dime cómo va el saldo.
- Los reportes siempre como archivo bonito (PDF con buen diseño y Excel), nunca texto en el chat.
- Si te pido llevar algo nuevo (inventario, nómina, lo que sea), créalo tú solo y empieza a manejarlo.

Confírmame cuando hayas guardado esto como tu personalidad.
```

### Mensaje 3 — Que se prepare

```
Prepárate para llevar mi contabilidad. Necesito que guardes de forma ordenada y segura tres cosas: el dinero que entra y sale (mi caja), mis clientes y lo que me deben, y las facturas que tengo por pagar con sus fechas de vencimiento.

Asegúrate de poder generar reportes bonitos en PDF y Excel.

También quiero que todas las mañanas, a las 8 (hora de Colombia), revises si tengo facturas que se vencen pronto (en 3 días o menos) y me avises por Telegram automáticamente.

Cuando tengas todo listo, escríbeme:
"✅ Contador IA listo. Ya puedes empezar a usarme."
```

> Cambia `[TU NOMBRE]` por tu nombre. Si no estás en Colombia, cambia la moneda y el horario.

Cuando llegue el mensaje `✅ Contador IA listo`, ya puedes usarlo.

---

## Cómo usarlo en el día a día

Háblale por Telegram como a una persona.

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

### Reportes
```
resumen
reporte caja
reporte clientes
reporte facturas
```

### También entiende lenguaje normal
```
oye, hoy vendí 200 mil de contado, anótalo
¿cuánto me debe Juan Pérez?
¿cuánto tengo en caja?
```

### Y crece contigo
```
ahora llévame también el inventario de productos: nombre, cantidad y precio
agrega 50 camisetas a 30000 cada una
reporte inventario
```

El agente crea lo que necesite y empieza a manejarlo, sin que toques nada.

### Pídele un audio
```
mándame el resumen y además explícame en una nota de voz cómo va el negocio
```

---

## Si algo no funciona

| Problema | Solución |
|---|---|
| El agente se equivoca o se queda a medias | Asegúrate de tener **OpenAI (gpt-4o)** conectado. Con modelos gratis/flojos falla |
| No llega el mensaje a Telegram | Revisa que el Chat ID sea el correcto (con @userinfobot) |
| El agente dice que no tiene permisos | Vuelve a hacer el Paso 6 (permisos con Kodee) |
| Se saltó un paso de la preparación | Dile: "te faltó esto, complétalo" y explícale qué falta |
| Las alertas llegan a hora rara | Recuérdale que trabajas en horario de Colombia |

---

## Créditos

Guía creada por [Fredy Ortegón](https://github.com/2025prosperidad)

Hermes Agent es un proyecto de [Nous Research](https://nousresearch.com/)
