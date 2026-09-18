# artifizia-demo

Deck comercial de Artifizia servido por GitHub Pages para enviar por link a prospectos.

**URL:** https://martindomingomkt.github.io/artifizia-demo/

`index.html` es un artifact autocontenido (fuentes woff2, SVG de marca y runtime JS
embebidos en base64 dentro de etiquetas `<script type="__bundler/*">`). El cuerpo de
la página vive como string JSON dentro de la etiqueta `template`.

## Regla crítica

**El archivo se copia byte a byte. No formatear, no reindentar, no pasar prettier ni
ninguna herramienta que reescriba HTML.** Alterar el escapado de esos strings JSON
rompe el render de la página.

## Publicar una versión nueva

Desde la raíz de este repo, con la ruta del HTML nuevo:

```bash
cp "$HOME/Documents/Claude/Projects/artifizia - estrategia/artifizia-movil-vN.html" index.html
shasum -a 256 "$HOME/Documents/Claude/Projects/artifizia - estrategia/artifizia-movil-vN.html" index.html   # deben coincidir
git commit -am "Deck vN" && git push
```

Pages republica en ~1 minuto. Antes de publicar, revisar que el deck no contenga
precios, nombres de prospectos ni datos de cliente: **este repo es público.**

## Notas

- `.nojekyll` evita que Jekyll procese el contenido.
- `robots.txt` bloquea la indexación en buscadores. La URL sigue siendo accesible
  para cualquiera que la tenga; solo evita que se encuentre por Google.

## Si más adelante quiero dominio propio (p. ej. `deck.artifizia.com`)

No configurado ahora. Haría falta:

1. **En este repo:** crear un archivo `CNAME` en la raíz con el hostname, una sola
   línea sin protocolo (`deck.artifizia.com`). Equivale a rellenar el campo
   "Custom domain" en Settings → Pages.
2. **En el DNS de Cloudflare** (artifizia.com está en Cloudflare Registrar): añadir
   un registro `CNAME` con nombre `deck` apuntando a `martindomingomkt.github.io`.
   Importante: poner el proxy de Cloudflare en **DNS only** (nube gris), no
   proxied — el proxy naranja delante de Pages rompe la emisión del certificado.
3. Esperar la validación de GitHub y activar **Enforce HTTPS** en Settings → Pages
   (el certificado Let's Encrypt tarda unos minutos en emitirse).
