Plan: Sitio de Política de Privacidad — GitHub Pages

Contexto

Apple y Google requieren una URL pública de política de privacidad para apps con IAP. Se crea un repositorio independiente bamboo-prompts-privacy con un único index.html que:

- Replica la estética de la app (mismos colores, tipografía, paleta light/dark)
- Detecta el idioma del navegador → muestra ES o EN (misma lógica que la app)
- Se publica gratis via GitHub Pages → URL: nagalanodev.github.io/bamboo-prompts-privacy

---

Paleta y tipografía a replicar

Del archivo src/constants/theme.ts:

┌───────────────┬─────────┬─────────┐
│ Token │ Light │ Dark │
├───────────────┼─────────┼─────────┤
│ background │ #E6D3A3 │ #162B1C │
├───────────────┼─────────┼─────────┤
│ surface │ #FBF6EC │ #1E3826 │
├───────────────┼─────────┼─────────┤
│ text │ #2F5D3A │ #E6D3A3 │
├───────────────┼─────────┼─────────┤
│ textSecondary │ #5C7D62 │ #A8C9A0 │
├───────────────┼─────────┼─────────┤
│ accent │ #6FAF5E │ #8FCA80 │
├───────────────┼─────────┼─────────┤
│ border │ #C5B07A │ #3D6645 │
└───────────────┴─────────┴─────────┘

Fuentes: Georgia, serif para headings — system-ui, sans-serif para cuerpo.

---

Estructura del repositorio

bamboo-prompts-privacy/
└── index.html ← todo en un solo archivo

Sin frameworks, sin build steps, sin dependencias. HTML + CSS + JS vanilla.

---

Lógica de idioma

Misma regla que la app: si navigator.language empieza con 'es' → español, caso contrario → inglés.

const lang = navigator.language?.startsWith('es') ? 'es' : 'en';

---

Secciones del contenido (ES / EN)

1.  Título — Política de Privacidad / Privacy Policy
2.  Última actualización — fecha fija en el HTML
3.  Datos que recopilamos — ninguno directamente desde la app
4.  Almacenamiento local — preferencias e historial se guardan solo en el dispositivo
5.  Compras in-app — procesadas por Apple/Google, no accedemos a datos de pago
6.  Contacto — email del developer

---

Estructura del index.html

 <head>
   - meta charset, viewport
   - CSS variables con light/dark según prefers-color-scheme
   - Estilos con los tokens de la app
 </head>
 <body>
   - Header con nombre de app + ícono 🎋
   - Contenido de política (div oculto por idioma)
   - <script> para detectar lang y mostrar la sección correcta
 </body>

El <script> al final:

1.  Lee navigator.language
2.  Muestra el div #es o #en
3.  Actualiza document.documentElement.lang

---

Habilitación de GitHub Pages

Tras crear el repo y hacer push:

1.  Settings → Pages → Source: main branch, / (root)
2.  URL resultante: https://nagalanodev.github.io/bamboo-prompts-privacy
3.  Disponible en ~1 min

---

Verificación

1.  Abrir la URL en browser con idioma ES → ver contenido en español
2.  Abrir con browser en idioma EN → ver contenido en inglés
3.  Con prefers-color-scheme: dark → ver paleta dark mode
4.  Pegar la URL en App Store Connect (campo Privacy Policy URL) → no da error de URL inválida
5.  Pegar la URL en Google Play Console → idem
