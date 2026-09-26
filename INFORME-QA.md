# Informe de revisión — Viva Sushi (26-09-2026)

Rama de trabajo: `mejoras-2026-09-26` (desde `main`, sin push, nada publicado).
Pruebas hechas con Playwright (Chrome) en 360x740, 390x844, 768x1024 y 1366x768, más axe-core. Capturas antes/después en `qa/` (carpeta ignorada por git, no se publica).

## Bugs encontrados y corregidos

| # | Severidad | Qué pasaba | Cómo se reprodujo | Arreglo |
|---|-----------|------------|-------------------|---------|
| 1 | Alta | Teléfono pegado con +56 9 quedaba mal escrito. Ej: pegar `+56 9 1234 5678` daba `56912` | Pegar en el campo Contacto (el `maxlength=8` cortaba el texto antes de limpiarlo) | Se quitó el `maxlength`; ahora acepta `+56 9 1234 5678`, `56912345678`, `912345678` y `1234 5678` |
| 2 | Media | Al tocar una categoría (ej. Temaki) el título quedaba escondido bajo el header y la barra de categorías | Tocar la pestaña y medir: título a 52 px, barra termina en 135 px | Margen de scroll para cada categoría |
| 3 | Media | Con el carrito vacío, la barra "Pedir" seguía accesible con Tab aunque estaba fuera de pantalla | Navegar con teclado | La barra queda `visibility:hidden` cuando no se muestra |
| 4 | Media | Modal sin semántica: 3 selects sin nombre (axe "critical"), sin `role=dialog`, foco se iba detrás del modal | axe + Tab | `role=dialog`, `aria-modal`, etiquetas asociadas, foco atrapado, fondo inerte, Escape, devuelve el foco |
| 5 | Baja | Al usar +/− con teclado el foco se perdía (el botón se reconstruía) | Enter repetido en un producto | El foco se mantiene en el mismo botón |
| 6 | Baja | Sin `<main>`, botones +/− sin nombre para lector de pantalla, 326 avisos de "región" en axe | axe | Corregido. axe queda en 0 problemas en los 4 tamaños |
| 7 | Baja | README decía 145 productos; hay 153 (y faltaba la categoría Handroll) | Contar `{n:` | README actualizado |
| 8 | Baja | Cantidad sin tope (se podía pasar de 99 y armar mensajes enormes) | Código | Tope de 99 por producto |
| 9 | Baja | El texto de "Copiar pedido" terminaba con "Enviar a: 56997887871" (sin formato) | Copiar | Ahora dice "Enviar al WhatsApp +56 9 9788 7871" |

Sin problemas: cero desbordes horizontales, el modal siempre queda dentro de la pantalla, cero errores de consola, ninguna imagen rota, imágenes livianas (las fotos pesan 15 KB c/u; el logo 65 KB). Las fotos de rolls son de 420 px, se ven algo blandas en pantallas grandes (limitación del material original).
Mensaje de WhatsApp verificado: tildes y ñ (José Núñez), &, %, #, comillas, total correcto ($17.500 en la prueba), salsas, palitos, pago, modalidad. Botón copiar verificado.
Estado abierto/cerrado verificado: 12:59 cerrado, 13:00 abierto, 21:59 abierto, 22:00 cerrado, lunes, domingo, medianoche y viernes 22:00 ("mañana"), también con horario de invierno.

## Mejoras hechas

- Aviso al pedir con el local cerrado: "El local está cerrado ahora y abre [hoy/mañana/el martes] a las 13:00. Puedes enviar tu pedido igual: te responderán cuando abra." (arriba del pedido y en la confirmación). No bloquea el envío. El estado de la portada se actualiza solo cada minuto.
- Recordatorio suave de salsas (según lo que dicen las promos: "3 salsas a elección" → "te faltan 3") y de palitos. No obliga a nada.
- Buscador de la carta (ignora tildes y mayúsculas, varias palabras, mensaje si no hay resultados).
- Carrito guardado en el navegador 24 h (`localStorage`, tolera datos dañados y navegador bloqueado); botones "Vaciar pedido" (pide confirmar tocando dos veces) y "Empezar un pedido nuevo".
- Accesibilidad del modal, enlace "Ir a la carta" para teclado, tamaños de imagen para evitar saltos al cargar.
- SEO: título y descripción, Open Graph/Twitter con imagen (`assets/og-image.jpg`), favicon, `apple-touch-icon`, canonical y JSON-LD Restaurante con dirección, teléfono, horario, medios de pago, Instagram y Facebook, todo tomado de la propia página.
- Campo Dirección: una sola línea lo controla. En `index.html`, `var PEDIR_DIRECCION = true;` → `false` lo quita del formulario y del mensaje.

## Preguntas para el cliente

1. **Delivery:** la página dice "No contamos con delivery" pero el formulario pedía "Dirección (solo si es despacho)". ¿Se quita el campo (`PEDIR_DIRECCION=false`) o hacen despacho a veces?
2. **Horario:** ¿es siempre martes a sábado 13:00–22:00? ¿Feriados? ¿Toman pedidos hasta las 22:00 o hasta antes?
3. **Con el local cerrado** ¿prefieren que se pueda pedir (como quedó) o que el botón se bloquee?
4. **Medios de pago:** ¿los seis son válidos para todo (Efectivo, Débito, Crédito, Transferencia, Pluxee, Edenred) y también para llevar? Si es transferencia, ¿mandan datos por WhatsApp?
5. **Descuento por retiro** o promo para "para llevar"; ¿cobran salsas o palitos extra? (hoy el recordatorio no los cobra).
6. **Precios pendientes:** California Sake (#5) y California Tako (#8) ($3.600 o $3.800). Productos E12 y E17: ¿existen? Bebidas: ¿venden?
7. **Numeración de "Arma tu roll":** Rellenos B15/B16 se repiten con Envolturas C15/C16 en el número; "Massago" (Envoltura C19) vs "Masago" (Nigiri). ¿Se unifica?
8. **Dominio:** hoy vive en `delaguardaagustin.github.io/viva-sushi`; ¿quieren dominio propio? Si cambia, hay que actualizar las URL absolutas del `<head>`.
9. Tiempo estimado de preparación para mostrarlo en el mensaje de confirmación.

## Ideas para ofrecerles como servicio (sin precios)

- Mantención mensual de la carta (cambios de precios, productos, promos del día) en menos de 24 h.
- Respuestas rápidas y plantillas de WhatsApp Business (confirmación, "estamos cerrados", "listo para retiro").
- Ficha de Google Business optimizada (horario, fotos, botón de pedir) y enlace desde Instagram/Facebook.
- Fotos propias de los productos (las actuales son recortes de la carta).
- Reporte mensual simple: visitas, pedidos enviados, productos más pedidos.
- Aviso automático por WhatsApp a quien pide fuera de horario y recordatorio a clientes frecuentes.
- Promo por retiro o código de descuento para primer pedido.

## Pendiente de tu revisión

- El pedido llega por WhatsApp sin cambios de formato; agregué solo texto en pantalla (avisos y pistas). No se cambió ninguna regla, precio, producto ni horario.
- Para publicar: revisar la rama `mejoras-2026-09-26` y hacer merge/push cuando lo decidas. Yo no publiqué nada.
