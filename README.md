# WebMCP
## canary chrome + ollama


WebMCP (Web Model Context Protocol) es una propuesta de estándar W3C que permite a las páginas web exponer herramientas estructuradas a agentes de IA integrados en el navegador, mediante la API navigator.modelContext 
webmcp.link
.
En lugar de depender de scraping del DOM o reconocimiento visual (frágil y propenso a errores), WebMCP permite declarar capacidades como funciones con esquemas JSON bien definidos, que los modelos de lenguaje ya entienden nativamente 
DEV社区
.
¿Por qué importa?

¿Por qué importa?
Enfoque tradicional
Con WebMCP
El agente "adivina" qué hace un botón
El sitio declara explícitamente calcular_descuento({precio, porcentaje})
Rompe si cambia el HTML/CSS
Rompe solo si cambia el contrato (schema)
Requiere prompts complejos
Tool-calling nativo del modelo
Sin validación de entrada
JSON Schema + validación en el handler
🚀 Requisitos previos
Chrome Canary 146 o superior 
venturebeat.com
Habilitar el flag experimental:
Navegar a chrome://flags
Buscar: WebMCP for testing o Experimental Web Platform Features 
bug0.com
Activar y reiniciar
Servir el archivo desde http://localhost o https:// (los contextos seguros son obligatorios para APIs experimentales)

# Opción rápida con Python
python -m http.server 8000

# O con Node.js
npx serve .


📁 webmcp-native-demo/
├── index.html         

├── README.md          

└── docs/
    └── API_REFERENCE.md #
    


```js
navigator.modelContext.registerTool({
  name: "obtener_precio_con_descuento",
  description: "Calcula el precio final aplicando un porcentaje de descuento",
  inputSchema: {
    type: "object",
    properties: {
      precio: { type: "number", description: "Precio original en euros", minimum: 0 },
      porcentaje: { type: "number", description: "Descuento (0-100)", minimum: 0, maximum: 100 }
    },
    required: ["precio", "porcentaje"]
  },
  execute: async ({ precio, porcentaje }) => {
    const descuento = precio * (porcentaje / 100);
    const final = precio - descuento;
    return {
      content: [{
        type: "text",
        text: `💰 Final: ${final.toFixed(2)} €`
      }]
    };
  }
});
```

🎯 Casos de uso prácticos
Este patrón es útil cuando:


✅ Tu aplicación ya tiene lógica de negocio en JavaScript que quieres reutilizar

✅ Necesitas que el agente opere con los mismos permisos/autenticación del usuario

✅ Quieres evitar mantener un servidor MCP separado para acciones simples

✅ Estás prototipando flujos agénticos antes de escalar a backend




