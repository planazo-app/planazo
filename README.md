# La página de invitación

Lo que ve quien recibe un enlace y todavía no tiene la app.

## Por qué existe

Antes, quien recibía un código tenía que instalar la app y registrarse **antes**
de poder ver a qué le invitaban. Eso es pedir un acto de fe, y el documento de
producto llama al enlace «la pieza más importante de todo el producto desde el
punto de vista de crecimiento».

Ahora el enlace enseña el plan —qué, cuándo, la zona y cuánta gente va— y solo
entonces ofrece instalar.

## Lo que NO enseña, a propósito

- **El punto de encuentro exacto.** Una página pública con la hora y el sitio
  donde va a estar un grupo de gente es justo lo que el apartado 3.5 quiere
  evitar. Solo sale la zona.
- **Quién va.** Ni nombres ni fotos: solo cuántos son.

No depende de que esta página se acuerde de ocultarlo: la función de base de
datos `vista_previa_invitacion` **no devuelve** esos datos. Ver
`db/21_vista_invitacion.sql`.

Mirar el enlace tampoco lo gasta. Solo `usar_invitacion` consume un uso.

## Qué hay aquí

```
web/i/index.html   La página
web/404.html       La misma, para alojamientos que no sirven rutas inventadas
web/icono.png      El muñeco del logo, con transparencia
web/favicon.png
```

## Las tres formas del enlace

La página acepta las tres, y la app también sabe leerlas al pegar (ver
`app/pruebas/enlaces.mjs`):

| Forma | Cuándo hace falta |
|---|---|
| `planazo.lol/i/CODIGO` | la bonita; necesita que el alojamiento sirva rutas inventadas |
| `planazo.lol/i/#CODIGO` | funciona en cualquier sitio |
| `planazo.lol/i/?c=CODIGO` | igual, si la almohadilla molesta |

En GitHub Pages la primera funciona gracias a `404.html`, aunque la respuesta
lleve código 404. Si eso importa —por ejemplo para que WhatsApp enseñe vista
previa—, usa la segunda forma o un alojamiento con reglas de reescritura.

## Cómo publicarla

**No está publicada todavía.** Hasta que lo esté, no se debe poner
`EXPO_PUBLIC_DOMINIO` en `app/.env`: la app empezaría a repartir enlaces que
llevan a una página que no existe, y eso es peor que un código suelto.

En GitHub Pages, que es lo que ya usas para tus otras apps:

1. Crea un repositorio `planazo` y sube el contenido de esta carpeta.
2. Settings → Pages → Deploy from a branch → `main` / `root`.
3. Custom domain: `planazo.lol`.
4. En Porkbun, apunta el dominio a GitHub Pages: cuatro registros `A` a
   `185.199.108.153`, `.109.153`, `.110.153` y `.111.153`, y un `CNAME` de
   `www` a `albadiazm.github.io`.
5. Cuando responda, en `app/.env`: `EXPO_PUBLIC_DOMINIO=planazo.lol`.

## Lo que falta

- **La vista previa de WhatsApp** es genérica: sin servidor no se pueden poner
  etiquetas distintas por plan. Para eso haría falta alojamiento con código.
- **El botón lleva al APK**, no a Google Play. Cuando esté publicada, cambia
  `APK` en el `<script>`.
- **En iPhone no hay nada que instalar** todavía. El botón debería detectarlo y
  decirlo, en vez de ofrecer un APK que no le sirve.
