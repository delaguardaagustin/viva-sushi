# Viva Sushi — Página de pedidos

Página web de pedidos para **Viva Sushi** (San Ramón, Santiago de Chile).
El cliente arma su pedido y lo envía por WhatsApp con el detalle ya escrito.

> **No hay delivery.** El local atiende solo para servir en el local o para llevar.

🔗 **WhatsApp:** [+56 9 9788 7871](https://wa.me/56997887871)
📍 **Dirección:** Santa Rosa #8689, San Ramón
🕐 **Horario:** Martes a Sábado, 13:00 a 22:00

---

## Qué hace

- Carta completa navegable por categorías (153 productos en 15 categorías) y **buscador** (sin tildes ni mayúsculas).
- Carrito con cantidades y total en vivo.
- Elección de modalidad: **Para llevar** o **Para servir**.
- Al enviar, abre WhatsApp con el pedido completo redactado: productos, cantidades, total, nombre, teléfono, salsas, palitos, medio de pago y comentarios.
- Muestra **Abierto/Cerrado** según la hora de Santiago. Con el local cerrado deja pedir igual, avisando cuándo abren.
- Recuerda (sin obligar) elegir las salsas que incluyen las promos y los palitos.
- El carrito se guarda en el navegador por 24 horas (si se recarga, no se pierde).
- Enlaces a Instagram, Facebook y Google Maps.

## Cómo funciona

`index.html` sin dependencias, sin build y sin backend, más la carpeta `assets/`
con el logo y las fotos de los rolls (recortadas de la carta oficial del local).
Se sube tal cual a cualquier hosting estático.

## Cómo editar la carta

Toda la carta vive en un solo bloque de JavaScript dentro de `index.html`.
Busca `var CARTA = [` y edita ahí.

```js
{ cat:"Nombre de la categoría", nota:"Texto opcional bajo el título", items:[
  {n:"Nombre del producto", d:"Descripción", p:4500, tag:"Etiqueta opcional"}
]},
```

| Campo | Qué es |
|-------|--------|
| `cat` | Nombre de la categoría (crea también su pestaña) |
| `nota` | Texto aclaratorio bajo el título de la categoría (opcional) |
| `n`   | Nombre del producto |
| `d`   | Descripción / ingredientes |
| `p`   | Precio en pesos, número entero sin puntos (`4500`) |
| `tag` | Etiqueta roja destacada, ej. `"Popular"` (opcional) |

Para cambiar el número de WhatsApp, edita la constante `WHATSAPP` (formato internacional, sin `+` ni espacios).

## Opciones rápidas (arriba del script en `index.html`)

| Constante | Para qué sirve |
|-----------|----------------|
| `WHATSAPP` | Número que recibe los pedidos |
| `PEDIR_DIRECCION` | `true` muestra el campo "Dirección"; `false` lo quita del formulario y del mensaje (el local no tiene delivery) |
| `HORARIO` | Días (0 = domingo … 6 = sábado) y horas de apertura/cierre. Cambiarlo también en la ficha y el pie de la página y en el bloque JSON-LD del `<head>` |

Las promos que digan `N salsas a elección` en su descripción activan el recordatorio de salsas. Mantén esa frase al editarlas.

## Qué se agregó en la revisión de septiembre 2026

Aviso al pedir con el local cerrado · recordatorio de salsas y palitos · buscador · carrito guardado (`localStorage`) · diálogo del pedido accesible (foco atrapado, Escape, roles ARIA) · SEO (título, descripción, Open Graph, Twitter, favicon, JSON-LD de Restaurante con los datos reales de la página). Corrige: teléfono pegado con +56 9, categorías tapadas por la barra fija al tocar su pestaña, controles ocultos alcanzables con el teclado. Detalle en `INFORME-QA.md`.

Si la página se publica en otro dominio, cambiar las URL absolutas `https://delaguardaagustin.github.io/viva-sushi/` (canonical, Open Graph, JSON-LD) en el `<head>`.

## Asistente de ayuda (chat)

Botón flotante **Ayuda** (abajo a la derecha, sube sobre la barra «Pedir» cuando hay pedido y se oculta con el modal del pedido abierto). Es un asistente **por reglas + búsqueda sobre `CARTA`**, no una IA: funciona 100% en el navegador, sin servidor, sin claves, sin costo, no envía datos a terceros, no guarda ni registra la conversación (vive solo en la memoria de la página; se pierde al recargar). El panel se construye al abrirlo por primera vez.

Qué responde: horario y si está abierto ahora (reusa `estadoLocal()`), dirección y «Cómo llegar», medios de pago (solo efectivo y débito/crédito), modalidades/delivery (cita el texto real de la ficha de la página), «qué lleva / cuánto cuesta / tienen X» (búsqueda aproximada con sinónimos, hasta 3 resultados con botón «Agregar al pedido» que usa `addItem`), recomendaciones por preferencia (vegetariano, sin pescado, camarón, apanado/frito, palta, queso crema, económico, popular…), promos VIVA para N personas (guía de ~10 piezas por persona, `AYUDA_PIEZAS_PERSONA`), «ver mi pedido», «cuánto llevo» y «hablar con el local» (abre `wa.me`). Alergias, salud, reclamos, tiempos y pedidos especiales se derivan al WhatsApp sin responder por cuenta propia.

**Límites:** no entiende frases complejas ni faltas de ortografía grandes; la carta no dice qué va crudo o cocido, ni alérgenos, así que «sin pescado» y «vegetariano» son heurísticas sobre las descripciones (el propio chat lo avisa y remite al local).

**Cómo agregar o cambiar respuestas** (bloque de script al final de `index.html`, comentado «ASISTENTE DE AYUDA»):
- Nueva intención: escribe una función `ayAlgo()` que use `ayResponde(texto, [nodos])` y agrega una línea a `AYUDA_INTENTS` con un patrón (`re`, sobre texto sin tildes y en minúsculas) y la función. El orden es la prioridad.
- Chips rápidos: `AYUDA_CHIPS`. Saludo (primera línea fija): `AYUDA_SALUDO`.
- Sinónimos de búsqueda: `AYUDA_SIN`. Palabras ignoradas: `AYUDA_STOP`. Preferencias de recomendación: `AYUDA_PREFS`.
- Si cambian el horario, dirección o medios de pago, actualiza también `ayUbicacion()` y `ayPago()` (el horario sale de `HORARIO`).

## Categorías actuales

Promociones VIVA · Rolls especiales · California Rolls · Hosomaki Rolls · Cheese Rolls ·
Avocado Rolls · Tempura Rolls · Sake Rolls · Futomaki · Rolls veganos · Temaki · Handroll ·
Nigiri y Sashimi · Entradas y salsas · Arma tu roll

## Pendientes

- [ ] Confirmar precios de **California Sake** (#5) y **California Tako** (#8) — la foto de la carta no permitía distinguir entre `$3.600` y `$3.800`.
- [ ] Definir si existen los productos **E12** y **E17** (la carta salta de E11 a E13 y de E16 a E18).
- [ ] Agregar sección de bebidas si corresponde.

## Salsas incluidas y salsas extra

El pedido separa dos bloques: **Salsas incluidas** (aparece solo si hay una promo VIVA; el máximo es lo que suman las promos del pedido, tomado de la frase `N salsas a elección` de su descripción, y soya + teriyaki nunca pasan de ese número) y **Salsas extra** (`SALSA_EXTRA` = $500 cada una, se suman al total y al mensaje de WhatsApp). Sin ninguna promo VIVA no se cobran salsas mientras `COBRAR_SALSAS_SIN_PROMO = false`. `MAX_SALSAS` es el tope de extras por tipo. Las constantes están arriba en `index.html`.

### Qué trae salsa incluida

- Promos VIVA: las que diga su descripción (`N salsas a elección`).
- Rolls (especiales, California, Hosomaki, Cheese, Avocado, Tempura, Sake, Futomaki y veganos): 1 salsa por unidad, **salvo E2 y E4** (ya vienen con su salsa). Se controla con `CATS_CON_SALSA` y `SIN_SALSA` en `index.html`.
- Temaki, handroll, nigiri, entradas y "Arma tu roll": sin salsa incluida (por confirmar con el local).
