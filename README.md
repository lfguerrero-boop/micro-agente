# Mini-agente de dados

Un agente de IA básico hecho con Node.js y el SDK de Google Gemini (`@google/genai`). Tiene una sola herramienta: **tirar un dado de N caras**. El modelo decide cuándo la necesita, tu código la ejecuta y el modelo responde con el resultado en lenguaje natural.

## Cómo funciona

1. El usuario hace una pregunta, por ejemplo "tira un dado de 20 caras".
2. Gemini responde que quiere usar la herramienta `tirar_dado` con `caras: 20`.
3. Tu código ejecuta la función real y obtiene un número.
4. Ese número se devuelve al modelo, que redacta la respuesta final.

Si la pregunta no tiene que ver con dados (por ejemplo, "cuéntame un chiste"), el modelo responde directamente sin usar la herramienta.

## Requisitos

- [Node.js](https://nodejs.org/) instalado.
- Una clave de API de Gemini, que puedes crear en [Google AI Studio](https://aistudio.google.com/apikey).

## Instalación

```bash
npm install
```

Crea un archivo `.env` en la raíz del proyecto con tu clave:

```
GEMINI_API_KEY=tu_clave_aqui
```

El archivo `.env` está en `.gitignore`, así que no se sube al repositorio.

## Uso

**Interfaz web:**

```bash
node server.js
```

Abre http://localhost:3000. Puedes cambiar el puerto con la variable `PORT`.

**Terminal:**

```bash
node index.js "tira un dado de 20 caras"
```

Sin argumentos usa una pregunta de ejemplo.

## Estructura

| Archivo | Para qué sirve |
| --- | --- |
| `index.js` | Versión de terminal, con el ciclo del agente comentado paso a paso. |
| `agente.js` | El mismo ciclo, pero devuelve `{ texto, pasos }`. Lo usa el servidor. Aquí están las instrucciones de estilo del modelo. |
| `server.js` | Servidor web con módulos de Node, sin dependencias extra. Expone `POST /api/chat` y sirve `public/`. |
| `public/` | Interfaz: `index.html`, `styles.css` y `main.js`. |
| `.claude/` | Configuración de Claude Code: permisos y subagentes (`revisor-frontend`, `probador-agente`). |

## Agregar una herramienta

1. Escribe la función y regístrala en el objeto `herramientas`.
2. Agrega su descripción en `functionDeclarations`.
3. Hazlo en `index.js` y en `agente.js` si quieres que funcione en ambas versiones.

## Notas

- Cada pregunta es independiente: no se guarda la conversación entre mensajes.
- El modelo se define en la constante `MODELO` (`gemini-3.6-flash`).
- Si la clave está vacía o es inválida, la web muestra un error genérico y el detalle aparece en la consola del servidor.
