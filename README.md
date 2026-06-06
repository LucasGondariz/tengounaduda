# Tengo una duda

Una web simple donde escribís una consulta, se envía **en paralelo a 4 modelos de IA distintos** (vía [OpenRouter](https://openrouter.ai)) y compará las respuestas lado a lado. Cada tarjeta se completa apenas su modelo responde, y podés elegir con cuál seguir charlando.

Todo corre sobre **n8n** autohospedado en Docker: n8n sirve la web *y* orquesta las llamadas a las IAs. No hace falta un backend aparte.

![demo](docs/demo.png)

---

## Cómo funciona

- Un **webhook GET** (`/webhook/web`) sirve la página HTML.
- Un **webhook POST** (`/webhook/consulta`) recibe `{ pregunta, slug, nombre }`, llama a **un** modelo y devuelve su respuesta.
- La web dispara **4 llamadas en paralelo** (una por modelo)

```
Navegador  --GET /webhook/web-->     n8n  (devuelve el HTML)
Navegador  --POST /webhook/consulta--> n8n --> OpenRouter --> IA   (x4 en paralelo)
```

---

## Requisitos

- [Docker](https://docs.docker.com/get-docker/)
- Una API key gratuita de OpenRouter: la podes conseguir en https://openrouter.ai desde **Settings → Keys**.

---

## Puesta en marcha

**1. Cloná el repo**
```bash
git clone https://github.com/LucasGondariz/tengounaduda.git
cd tengounaduda
```

**2. Configurá tu API key**

Copiá la plantilla de variables de entorno y editá el archivo `.env` con tu key:
```bash
cp .env.example .env
```
Abrí `.env` y reemplazá el valor:
```
OPENROUTER_API_KEY=api_token_real
```
> El archivo `.env` está en `.gitignore`, así que la key nunca se sube a GitHub.

**3. Levantá n8n**
```bash
docker compose up -d
```
Esperá unos segundos y abrí http://localhost:8645. La primera vez n8n te pide crear un usuario local (es solo para tu instancia).

**4. Importá el workflow**

En n8n: menú **⋯ (arriba a la derecha) → Import from File** → elegí `workflows/workflow n8n.json`.

**5. Activá el workflow**

Con el workflow abierto, prender el toggle **Active** (arriba a la derecha).

**6. Usá la web**

Abrí 👉 **http://localhost:5678/webhook/web**

Escribí una consulta! ✨
