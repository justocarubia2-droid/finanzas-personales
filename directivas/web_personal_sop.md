# Directiva: Web Personal de Gastos e Inversiones

## Objetivo
Crear un dashboard financiero personal moderno y profesional inspirado en aplicaciones SaaS de alta gama (como Stripe o Linear). El diseño debe utilizar un tema oscuro ("Dark Mode") limpio y elegante, dominado por tonos azules profundos y opacos (navy/dark blue). La interfaz debe transmitir calma y legibilidad, con sombras sutiles, bordes suaves, y tipografía moderna (Inter). Se deben evitar por completo los estilos sci-fi, efectos neon o temática "hacker".

## Entradas
- Webhook Payload (GET a `https://n8n.tecleti.com/webhook/d437bbf9-059a-4763-8f87-2d02c8da4a33`).
- Los datos vienen en un array o varios arrays, diferenciados por la clave `Categoria`:
  - `"Categoria": "Gastos"`
  - `"Categoria": "Resumen"`
  - `"Categoria": "Portafolio Abierto"`
  - `"Categoria": "Cerradas"`
  - `"Categoria": "Operaciones"`
- Los mapeos de columnas utilizan nombres variables como `col_2`, `col_3`, y claves dinámicas dependiendo del tipo.

## Salidas
- Una aplicación web local interactiva en `index.html` (Standalone React + Tailwind).
- Estilos aplicados estrictamente con TailwindCSS mediante CDN. Elementos de diseño minimalista, profesional, de alta legibilidad, sin glows ni logotipos recargados ("Conectado" badge oculto y logo del header removido por orden del usuario).
- Recharts (via CDN) implementado para proveer análisis de Gastos por Categoría, evitando la simple visualización de tabla que ya da Spreadsheets.

## Lógica y Pasos
1. Estructurar la aplicación en un archivo `index.html` importando React, Babel y TailwindCSS por CDN (para evitar conflictos de `npm install` con OneDrive).
2. Crear un Layout con navegación en estilo "píldora flotante" (pill navigation) para cambiar entre dos vistas: "Gastos" e "Inversiones".
3. Implementar un componente de Carga (Loading) con animación de vidrio pulsante y opacidad.
4. Implementar la llamada asíncrona (fetch) al webhook de n8n al iniciar la app.
5. Parsear y separar la respuesta global de n8n según la clave `Categoria`. Filtrando filas falsas de encabezado.
6. Construir componentes flotantes (Glass Cards) para mostrar los datos de Resumen, Portafolio y Gastos.
7. Aplicar colores semánticos tenues (verde cálido para ganancias, rojo apagado para pérdidas).

## Restricciones y Casos Borde
- Nomenclatura Financiera: No utilizar el término "Resultado Latente", el usuario prefiere "PyL vendiendo hoy" o "Ganancia si vendieras hoy".
- El JSON viene con nombres de columnas estáticos mezclados con dinámicos (ej: `"col_2"`, `"col_3"`). Es crucial mapear correctamente estas claves a valores legibles en la UI para cada categoría.
- Asegurarse de manejar valores nulos o cadenas vacías.
- Si el fetch falla o es lento, la pantalla de "loading" espacial debe mantenerse activa. La animación no debe pelearse con React, el ícono Lucide fue aislado en un wrapper seguro `<span/>` protegido con referencias useRef().
- **Node_modules en OneDrive**: Instalar dependencias con `npm install` falla si la carpeta está sincronizada con OneDrive (Lanza `ECONNRESET` o `ENOENT` porque OneDrive bloquea archivos). *Solución tomada*: Usar un `index.html` standalone apoyado en importaciones por CDN.
