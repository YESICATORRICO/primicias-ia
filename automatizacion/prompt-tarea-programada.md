# Instrucciones de la tarea programada

Este es el texto completo que recibe Claude en cada actualización automática de Primicias IA. Es el corazón del proyecto: define qué buscar, qué fuentes aceptar, cómo verificar, cómo escribir para lectores no técnicos y cómo publicar.

- **Herramienta:** tarea programada de Claude (Cowork), que corre en la nube.
- **Frecuencia:** 3 veces por día, hora de Argentina (mañana, tarde y noche).
- **Qué modifica:** solo el bloque de datos de las ediciones. En este repositorio ese bloque está en [`data/ediciones.js`](../data/ediciones.js).

> Nota: en la versión de producción la página vive como un Artifact de Claude, por eso las instrucciones mencionan esa herramienta y el archivo `project/Main.dc.html`. La versión de este repositorio es una exportación independiente con el mismo diseño y los mismos datos.

---

```text
Actualizá la landing "Primicias" con las noticias más relevantes de inteligencia artificial publicadas desde la última actualización. Respondé y escribí todo en español.

QUÉ ES: una landing (Artifact de tipo Design) para lectores NO técnicos. Muestra ediciones por fecha, un historial, un filtro desplegable por categoría y, en cada noticia, una marca de verificación (✓✓ "Confirmada por dos medios" o ✓ "Un solo medio"). Todo el contenido vive en el archivo `project/Main.dc.html`, dentro del bloque JavaScript delimitado por los comentarios `// ===== DATOS: EDICIONES (la más reciente primero) =====` y `// ===== FIN DATOS =====`. Ahí están las constantes `UPDATED_AT` y `EDITIONS` (un array de ediciones, la más reciente primero). NO cambies el diseño, los colores, el markup, la lista `CATEGORIES` ni el resto del código: solo editás esos datos. La marca se dibuja sola a partir del campo `verified` de cada noticia.

REGLA DE TEXTO VISIBLE: nunca menciones en el contenido de la página los horarios de actualización (8, 16, 22 h ni similares) ni que hay corridas programadas.

CATEGORÍAS (lista fija; cada noticia lleva exactamente una, escrita idéntica): "Política y regulación", "Economía y finanzas", "Empresas y productos", "Seguridad y privacidad", "Educación", "Ciencia y salud", "Infraestructura y energía", "Trabajo y sociedad", "Entretenimiento y cultura". Elegí la que mejor describa el tema principal. No inventes categorías nuevas.

PASOS
1. Averiguá la fecha y hora actual en hora argentina (America/Argentina/Buenos_Aires).
2. Leé el archivo actual con la herramienta Artifact (action "read", path "project/Main.dc.html") y abrí el archivo guardado para ver qué noticias ya están publicadas.
3. Buscá en la web noticias de IA publicadas HOY (o desde la última actualización si es la primera corrida del día, incluyendo la noche anterior que no esté cubierta). Hacé varias búsquedas distintas (empresas de IA, regulación, Latinoamérica/Argentina, ciencia y salud, seguridad, mercados, trabajo, educación, productos de consumo). Revisá también la última edición de The Batch (deeplearning.ai/the-batch).
4. FUENTES AUTORIZADAS: solo estas. Cualquier noticia que no provenga de esta lista o de una fuente oficial no se publica.
   - Agencias: Reuters, AP, AFP, EFE, Europa Press.
   - Medios internacionales: Bloomberg, CNBC, CNN, BBC, The Guardian, NYT, WSJ, FT.
   - Tecnología: The Verge, TechCrunch, Ars Technica, Wired, MIT Technology Review.
   - Especializados en IA: The Batch (DeepLearning.AI). Es la única newsletter autorizada; usá su nota publicada en deeplearning.ai/the-batch y poné como fuente "The Batch (DeepLearning.AI)". Como sale una vez por semana, usala solo para noticias de la ventana de tiempo de esta corrida y no la cuentes como la segunda fuente de una noticia que ella misma toma de otro medio.
   - Medios en español: El País, La Nación, Clarín, Infobae.
   - Fuentes oficiales: gobiernos, organismos, comunicados de las empresas.
   Enlazá siempre la nota original del medio autorizado, nunca un agregador ni otra newsletter. Si una cifra la da la propia empresa, aclaralo en el resumen.
5. NIVEL DE VERIFICACIÓN (campo `verified` de cada noticia):
   - `verified: true` (✓✓ Confirmada): además de la nota enlazada, hay al menos otra cobertura que cumple las tres condiciones: (a) es de un medio de la lista o de una fuente oficial directamente involucrada (el gobierno, organismo o empresa de la que habla la noticia); (b) es de otra empresa periodística; (c) tiene redacción propia: NO cuentan las copias del mismo cable de agencia (ej. dos medios que publican el mismo texto de Europa Press o AFP) ni las notas que solo citan a la primera ("según informó Bloomberg…"). Si la noticia es un anuncio de una empresa, dos coberturas confirman que el anuncio existió y las cifras de la empresa se aclaran en el resumen.
   - `verified: false` (✓ Un solo medio): no apareció esa segunda cobertura, pero la nota es de un medio de la lista.
   Reglas: la noticia destacada (`featured: true`) debe ser SIEMPRE `verified: true`. Como máximo 2 noticias con `verified: false` por edición (por día, sumando todas las corridas). Si una noticia con `verified: false` de la edición de HOY consigue en esta corrida una segunda cobertura que cumpla las condiciones, cambiala a `verified: true` (podés mejorar el resumen con la nueva información); eso cuenta como novedad. No toques las de días anteriores.
6. Elegí entre 2 y 6 noticias NUEVAS y relevantes (no repitas temas ya publicados en ninguna edición, salvo que haya un desarrollo nuevo importante). Priorizá las confirmadas. Si no hay nada nuevo que valga la pena, solo actualizá `UPDATED_AT` (y los cambios de `verified` del paso 5, si los hay).
7. Formato de cada noticia (objeto en `items`): { featured: bool, verified: bool, category: "una de las categorías de la lista", title: "titular propio, claro", summary: "2-4 oraciones en lenguaje simple, explicando cualquier término técnico", why: "una frase que empieza en minúscula y explica por qué le importa a un lector común", source: "Nombre del medio", url: "https://..." }. Escribí con palabras propias: nunca copies párrafos; como máximo una cita textual de menos de 15 palabras por fuente. Usá comillas dobles en los strings JS y escapá las comillas internas.
8. Edición del día: si ya existe una edición con `date` igual a hoy (formato AAAA-MM-DD), agregá las noticias nuevas a su `items` (ordenadas por relevancia; exactamente una con `featured: true`, la más importante del día y confirmada). Si no existe, creá una nueva al PRINCIPIO del array con: date, shortLabel (ej. "6 nov"), longDate (ej. "Viernes 6 de noviembre de 2026"), note: "Edición en curso: se suman noticias a lo largo del día.", facts: [], items: [...]. NUNCA borres ni modifiques ediciones de días anteriores: son el historial.
9. En la última corrida del día (la de la noche, después de las 21 h): poné `note: ""` en la edición del día (queda cerrada) y completá `facts` con 2 o 3 datos clave del día ({ big: "cifra corta", text: "explicación breve" }), solo con cifras de noticias con `verified: true`.
10. Actualizá `UPDATED_AT` a la fecha y hora actual, formato "6 nov 2026, 08:00 h" (la página muestra solo la fecha).
11. Guardá el archivo editado, revisá que el JavaScript siga siendo válido (llaves, comas, comillas), que cada `category` esté en la lista y que cada noticia nueva tenga `verified`, y publicalo con la herramienta Artifact. Si la publicación es rechazada por una versión más nueva, volvé a leer el archivo, reaplicá tus cambios sobre esa versión y publicá de nuevo.
12. Noticias que quedaron afuera: si alguna noticia relevante de un medio autorizado no entró (por el tope de 2 de un solo medio u otro motivo), registrala con fecha, hora, medio, titular, resumen de una línea, motivo y link en el documento de descartes del proyecto.
13. Al terminar, dejá un resumen breve con los titulares agregados, su categoría, su marca (✓✓ o ✓) y sus fuentes, las que cambiaron de ✓ a ✓✓, y las que quedaron afuera (o indicá que no hubo novedades).
```
