# Formulario de contacto

Web estática hecha con Astro. El formulario envía nombre, email, teléfono y mensaje a la API de contacto del panel Laravel. El panel guarda el mensaje y manda al visitante el correo de confirmación.

## 1. Instalar y arrancar

Requisito: Node.js 22.12 o superior.

```bash
npm install
npm run dev
```

Abre http://localhost:4321. Sin la URL y el token, el envío muestra un error de configuración.

## 2. Configurar la API

En la raíz del repositorio, copia el ejemplo y rellena los dos valores:

```bash
cp .env.example .env
```

En Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Contenido de `.env`:

```
PUBLIC_CONTACT_API_URL=https://tu-panel.ejemplo/api/contact
PUBLIC_CONTACT_API_TOKEN=el_valor_de_CONTACT_API_TOKEN
```

`PUBLIC_CONTACT_API_URL` es la URL completa del `POST`. `PUBLIC_CONTACT_API_TOKEN` es el valor de `CONTACT_API_TOKEN` del panel.

El sitio es estático y la petición sale del navegador, así que Astro solo puede leer variables que empiezan por `PUBLIC_`. Esas dos quedan en el JavaScript publicado. La API acepta la llamada desde otro dominio.

`.env` no se sube a git. `.env.example` sí, y va vacío.

## 3. Reiniciar y probar

Astro lee esas variables al arrancar. Si `npm run dev` ya estaba en marcha, páralo y vuelve a lanzarlo:

```bash
npm run dev
```

1. Abre http://localhost:4321.
2. Rellena nombre, email, teléfono y mensaje.
3. Pulsa **Enviar**.

## 4. Publicar

Las variables `PUBLIC_` se incorporan al generar el sitio. Tienen que existir en el entorno de build antes de compilar.

```bash
npm run build
```

La carpeta `dist/` es la web estática. Súbela a cualquier hosting de archivos estáticos.

En el panel del hosting, define las mismas dos variables antes del build. No las pegues en el código.

## Qué hace el formulario

- Campos obligatorios: nombre, email, teléfono y mensaje.
- Comprueba que el email tenga un formato básico antes de llamar a la API.
- Si la API responde `422`, muestra los textos de cada campo tal como llegan.
- Si se supera el límite de 10 envíos por minuto, pide esperar un minuto.
- Muestra carga, éxito o error, y vacía los campos tras un envío correcto.
- Incluye un campo oculto anti-spam. Si un programa lo rellena, no llama a la API.
