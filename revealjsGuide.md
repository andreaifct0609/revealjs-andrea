# Guía para el uso de reveal.js

Reveal.js es un framework para la realización de presentaciones visualmente atractivas de manera sencilla, realizado mediante JavaScript. En esta guía se hablará de sus características más relevantes.

### Margen

Ejemplo básico de una presentación de revel.js completamente funcional:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="dist/reveal.css">
  <link rel="stylesheet" href="dist/theme/white.css">
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <section>Slide 1</section>
      <section>Slide 2</section>
    </div>
  </div>
  <script src="dist/reveal.js"></script>
  <script>
    Reveal.initialize();
  </script>
</body>
</html>
```

La jerarquía de marcado de presentación debe estar `.reveal > .slides > sectiondonde` el `section` elemento representa una diapositiva y se puede repetir indefinidamente.

Si coloca varios `section` elementos dentro de otro `section`, se mostrarán como diapositivas verticales . La primera de las diapositivas verticales es la "raíz" de las demás (en la parte superior) y se incluirá en la secuencia horizontal. Por ejemplo:

``` html
 <div class="reveal">
  <div class="slides">
    <section>Horizontal Slide</section>
    <section>
      <section>Vertical Slide 1</section>
      <section>Vertical Slide 2</section>
    </section>
  </div>
</div>
```

#### Viewport

La ventana gráfica de `revel.js` es el elemento DOM contenedor que determina el tamaño de su presentación en una página web. Por defecto, este será el `body` elemento. Si incluye varias presentaciones en la misma página, cada `.reveal` elemento de la presentación actuará como su ventana gráfica.

La ventana `reveal-viewportgráfica` siempre está decorada con una clase que se inicializa .js.

#### Estados de diapositiva

Si establece `data-state="make-it-pop"` en una diapositiva `<section>`, "make-it-pop" se aplicará como una clase en el elemento de la ventana gráfica cuando se abra esa diapositiva. Esto le permite aplicar cambios de estilo amplios a la página según la diapositiva activa.

``` html 
<section data-state="make-it-pop"></section>
```

``` css 
/* CSS */
.make-it-pop {
  filter: drop-shadow(0 0 10px purple);
}
```

También puede escuchar estos cambios de estado a través de JavaScript:

```js 
Reveal.on( 'make-it-pop', () => {
  console.log('✨');
} )
```

### Markdown

Es posible y muchas veces más conveniente escribir contenido de presentación usando Markdown. Para crear una diapositiva de Markdown, agregue el `data-markdown` atributo a su `<section>` elemento y envuelva el contenido `<textarea data-template>` como en el ejemplo siguiente:

``` html
<section data-markdown>
  <textarea data-template>
    ## Slide 1
    A paragraph with some text and a [link](http://hakim.se).
    ---
    ## Slide 2
    ---
    ## Slide 3
  </textarea>
</section>
```

Tenga en cuenta que esto es sensible a la sangría (evite mezclar tabulaciones y espacios) y saltos de línea (evite saltos consecutivos).

#### Complemento de Markdown

Esta funcionalidad está impulsada por el complemento Markdown incorporado que, a su vez, utiliza marcado para todo el análisis. El complemento Markdown se incluye en nuestros ejemplos de presentación predeterminados. Si desea agregarlo manualmente a una nueva presentación, siga estos pasos:

``` html
<script src="plugin/markdown/markdown.js"></script>
<script>
  Reveal.initialize({
    plugins: [ RevealMarkdown ]
  });
</script>
```

#### Markdown externo

Puede escribir su contenido como un archivo separado y hacer que revel.js lo cargue en tiempo de ejecución. Tenga en cuenta los argumentos del separador que determinan cómo se delimitan las diapositivas en el archivo externo: el `data-separator` atributo define una expresión regular para diapositivas horizontales (por defecto `^\r?\n---\r?\n$`, una regla horizontal delimitada por una nueva línea) y `data-separator-vertical` define diapositivas verticales (deshabilitado por defecto). El `data-separator-notes` atributo es una expresión regular para especificar el comienzo de las notas del orador de la diapositiva actual (el valor predeterminado es `notes?:`, por lo que coincidirá con "nota:" y "notas:"). El `data-charset` atributo es opcional y especifica qué juego de caracteres usar al cargar el archivo externo.

Cuando se usa localmente, esta función requiere que revel.js se ejecute desde un servidor web local . El siguiente ejemplo personaliza todas las opciones disponibles:

``` html
<section data-markdown="example.md"
         data-separator="^\n\n\n"
         data-separator-vertical="^\n\n"
         data-separator-notes="^Note:"
         data-charset="iso-8859-15">
    <!--
        Note that Windows uses `\r\n` instead of `\n` as its linefeed character.
        For a regex that supports all operating systems, use `\r?\n` instead of `\n`.
    -->
</section>
```

#### Atributos de elementos

Hay disponible una sintaxis especial (a través de comentarios HTML) para agregar atributos a los elementos de Markdown. Esto es útil para fragmentos, entre otras cosas.

``` html
<section data-markdown>
  <script type="text/template">
    - Item 1 <!-- .element: class="fragment" data-fragment-index="2" -->
    - Item 2 <!-- .element: class="fragment" data-fragment-index="1" -->
  </script>
</section>
```

#### Atributos de diapositivas

Hay disponible una sintaxis especial (a través de comentarios HTML) para agregar atributos a los `<section>` elementos de diapositiva generados por su Markdown.

``` html
<section data-markdown>
  <script type="text/template">
  <!-- .slide: data-background="#ff0000" -->
    Markdown content
  </script>
</section>
```

#### Resaltado de sintaxis

Revel.js incluye potentes funciones de resaltado de sintaxis. Con la sintaxis de corchetes que se muestra a continuación, puede resaltar líneas individuales e incluso recorrer varios resaltados separados paso a paso.

``` html
<section data-markdown>
  <textarea data-template>
    ```js [1-2|3|4]
    let a = 1;
    let b = 2;
    let c = x => 1 + 2 + x;
    c(3);
    ```
  </textarea>
</section>
```

#### Configurando marked

Usamos `marked` para analizar Markdown. Para personalizar la representación del marcado, puede pasar opciones al configurar Reveal :

``` js
Reveal.initialize({
  // Options which are passed into marked
  // See https://marked.js.org/#/USING`ADVANCED.md#options
  markdown: {
    smartypants: true
  }
});
```

### Fondos de diapositivas

Las diapositivas se encuentran dentro de una parte limitada de la pantalla de forma predeterminada para permitir que se ajusten a cualquier pantalla y escalen de manera uniforme. Puede aplicar fondos de página completa fuera del área de la diapositiva agregando un `data-background` atributo a sus `<section>` elementos. Se admiten cuatro tipos diferentes de fondos: color, imagen, video e iframe.

#### Fondos De Color

Se admiten todos los formatos de color CSS, incluidos valores hexadecimales, palabras clave `rgba()` o `hsl()`.

``` html
<section data-background-color="aquamarine">
  <h2>🐟</h2>
</section>
<section data-background-color="rgb(70, 70, 255)">
  <h2>🐳</h2>
</section>
```

#### Fondos De Pantalla

De forma predeterminada, las imágenes de fondo cambian de tamaño para cubrir la página completa. Opciones Disponibles:

| Atributo | Defecto | Descripción |
| ----------- | ----------- | ----------- |
| magen de fondo de datos |  | URL de la imagen a mostrar. Los GIF se reinician cuando se abre la diapositiva. |
| tamaño de fondo de datos | cubrir | Ver tamaño de fondo en MDN. |
| posición de fondo de datos | centrar | Consulte la posición de fondo en MDN. |
| repetición de fondo de datos | no repetir | Ver repetición de fondo en MDN. |
| opacidad de fondo de datos | 1 | Opacidad de la imagen de fondo en una escala de 0 a 1. 0 es transparente y 1 es completamente opaco. |

``` html 
<section data-background-image="http://example.com/image.png">
  <h2>Image</h2>
</section>
<section data-background-image="http://example.com/image.png" 
          data-background-size="100px" data-background-repeat="repeat">
  <h2>This background image will be sized to 100px and repeated</h2>
</section>
```

#### Fondos De Video

Reproduce automáticamente un video de tamaño completo detrás de la diapositiva.

| Atributo | Defecto | Descripción |
| ----------- | ----------- | ----------- |
| datos-fondo-video |  | Una sola fuente de video o una lista separada por comas de fuentes de video. |
| bucle de video de fondo de datos | falso | Indica si el video debe reproducirse repetidamente. |
| datos-fondo-video-silenciado | falso | Indica si el audio debe silenciarse. |
| tamaño de fondo de datos | cubrir | Úselo `cover` para pantalla completa y algunos recortes o `contain` para formato de pantalla ancha. |
| opacidad de fondo de datos | 1 | Opacidad del video de fondo en una escala de 0-1. 0 es transparente y 1 es completamente opaco.|

``` html
<section data-background-video="https://static.slid.es/site/homepage/v1/homepage-video-editor.mp4" 
          data-background-video-loop data-background-video-muted>
  <h2>Video</h2>
</section>
```

#### Fondos Iframe

Inserta una página web como fondo de diapositiva que cubre el 100% del ancho y alto de revel.js. El iframe está en la capa de fondo, detrás de las diapositivas y, como tal, no es posible interactuar con él de forma predeterminada. Para que su fondo sea interactivo, puede agregar el `data-background-interactive` atributo.

| Atributo | Defecto | Descripción |
| ----------- | ----------- | ----------- |
| iframe de fondo de datos |  | URL del iframe para cargar |
| datos-fondo-interactivo | falso | Incluya este atributo para que sea posible interactuar con el contenido del iframe. Habilitar esto evitará la interacción con el contenido de la diapositiva. |

``` html
<section data-background-iframe="https://slides.com"
          data-background-interactive>
  <h2>Iframe</h2>
</section>
```

Los iframes se cargan de forma diferida cuando se vuelven visibles. Si desea precargar iframes con anticipación, puede agregar un `data-preload` atributo a la diapositiva `<section>` . También puede habilitar la precarga global para todos los iframes usando la `preloadIframes` opción de configuración.

#### Transiciones de fondo

Usaremos un fundido cruzado para la transición entre fondos de diapositivas de forma predeterminada. Esto se puede cambiar usando la `backgroundTransition` opción de configuración

#### Fondo de parallax

Si desea utilizar un fondo de desplazamiento de paralaje, establezca las dos primeras propiedades a continuación al inicializar reveal.js (las otras dos son opcionales).

``` js
Reveal.initialize({
  // Parallax background image
  parallaxBackgroundImage: '', // e.g. "https://s3.amazonaws.com/hakim-static/reveal-js/reveal-parallax-1.jpg"

  // Parallax background size
  parallaxBackgroundSize: '', // CSS syntax, e.g. "2100px 900px" - currently only pixels are supported (don't use % or auto)

  // Number of pixels to move the parallax background per slide
  // - Calculated automatically unless specified
  // - Set to 0 to disable movement along an axis
  parallaxBackgroundHorizontal: 200,
  parallaxBackgroundVertical: 50
});
```

### Medios de comunicación

Ofrecemos mecanismos prácticos para la reproducción automática y la carga diferida de elementos multimedia HTML e iframes basados ​​en la visibilidad y la proximidad de las diapositivas. Esto funciona para los elementos `<video>` , `<audio>` y `<iframe>` .

#### Auto-reproducción

Agréguelo `data-autoplay` a su elemento multimedia si desea que comience a reproducirse automáticamente cuando se muestra la diapositiva:

``` html
<video data-autoplay src="http://clips.vorwaerts-gmbh.de/big`buck`bunny.mp4"></video>
```

Si desea habilitar o deshabilitar la reproducción automática globalmente, para todos los medios integrados, puede usar la `autoPlayMedia` opción de configuración. Si configura esta opción en `true` TODOS los medios se reproducirán automáticamente independientemente de los `data-autoplay` atributos individuales . Si lo configura en `autoPlayMedia: false` NO, los medios se reproducirán automáticamente.

``` js
Reveal.initialize({
autoPlayMedia: true
})
```

Tenga en cuenta que los iframes HTML `<video>/<audio>` y YouTube / Vimeo incrustados se pausan automáticamente cuando se aleja de una diapositiva. Esto se puede desactivar decorando su elemento con un `data-ignore` atributo.

#### Carga lenta

Cuando se trabaja en presentaciones con una gran cantidad de medios o contenido de iframe, es importante cargarlas de forma perezosa. La carga diferida significa que revel.js solo cargará contenido para las pocas diapositivas más cercanas a la diapositiva actual. El número de portaobjetos precargados está determinado por la `viewDistance` opción de configuración.

Para habilitar la carga diferida, todo lo que necesita hacer es cambiar sus `src` atributos a `data-src` como se muestra a continuación. Esto es compatible con elementos de imagen, video, audio e iframe.

``` html
<section>
  <img data-src="image.png">
  <iframe data-src="https://hakim.se"></iframe>
  <video>
    <source data-src="video.webm" type="video/webm" />
    <source data-src="video.mp4" type="video/mp4" />
  </video>
</section>
```

#### Iframes de carga diferida

Tenga en cuenta que los iframes cargados de forma diferida ignoran la `viewDistance` configuración y solo se cargarán cuando la diapositiva que los contiene se vuelva visible. Los iframes también se descargan tan pronto como se oculta la diapositiva.

Cuando cargamos un elemento de video o audio de forma diferida, revel.js no comenzará a reproducir ese contenido hasta que la diapositiva se vuelva visible. Sin embargo, no hay forma de controlar esto para un iframe, ya que podría contener cualquier tipo de contenido. Eso significa que si cargamos un iframe antes de que la diapositiva sea visible en la pantalla, podría comenzar a reproducir contenido multimedia y sonido en segundo plano.

Puede anular este comportamiento con el `data-preload` atributo. El iframe a continuación se cargará de acuerdo con `viewDistance`.

``` html
<section>
  <iframe data-src="https://hakim.se" data-preload></iframe>
</section
```

También puede cambiar el valor predeterminado de forma global con la `preloadIframes` opción de configuración. Si se establece en `true` TODOS los iframes con un `data-src` atributo se precargarán cuando se encuentren dentro `viewDistance` de los `data-preload` atributos individuales . Si se establece en `false`, todos los iframes solo se cargarán cuando sean visibles.

#### Iframes

Usar iframes es una forma conveniente de incluir contenido de fuentes externas, como un video de YouTube o una hoja de Google. Revela.js detecta automáticamente las URL incorporadas de YouTube y Vimeo y las reproduce automáticamente cuando la diapositiva se vuelve visible.

Si agrega un `<iframe>` interior a su diapositiva, estará limitado por el tamaño de la diapositiva. Para salir de esta restricción y agregar un iframe de página completa, puede usar un fondo de diapositiva de iframe.

#### Mensaje de publicación de iframe

revel.js envía automáticamente dos mensajes de publicación a iframes incrustados. `slide:start` cuando la diapositiva que contiene el iframe se hace visible y `slide:stop` cuando está oculta.

``` js
// JavaScript inside of an iframe embedded within reveal.js
window.addEventListener( 'message', event => {
	if( event.data === 'slide:start' ) {
		// The slide containing this iframe is visible
	}
	else if( event.data === 'slide:stop' ) {
		// The slide containing this iframe is not visible
	}
} );
```

### Presentación de código

revel.js incluye un poderoso conjunto de funciones destinadas a presentar código resaltado de sintaxis, impulsado por highlight.js . Esta funcionalidad vive en el complemento destacado y se incluye en nuestro modelo de presentación predeterminado.

A continuación se muestra un ejemplo con código clojure que se resaltará en la sintaxis. Cuando el `data-trim` atributo está presente, los espacios en blanco circundantes dentro del `<code>` se eliminan automáticamente.

HTML se escapará de forma predeterminada. Para evitar esto, agregue `data-noescape` al `<code>` elemento.

``` html
<section>
  <pre><code data-trim data-noescape>
(def lazy-fib
  (concat
   [0 1]
   ((fn rfib [a b]
        (lazy-cons (+ a b) (rfib b (+ a b)))) 0 1)))
  </code></pre>
</section>
```

#### Tematización

Asegúrese de que se incluya un tema de resaltado de sintaxis en su documento. Incluimos Monokai de forma predeterminada, que se distribuye con el repositorio revel.js en `lib / css / monokai.css` . Puede encontrar una lista completa de temas disponibles en [Highlightjs.org](https://highlightjs.org/static/demo/).

``` html
<link rel="stylesheet" href="lib/css/monokai.css">
<script src="plugin/highlight/highlight.js"></script>
<script>
  Reveal.initialize({
    plugins: [ RevealHighlight ]
  });
</script>
```

#### Números de línea y aspectos destacados

Puede habilitar números de línea agregando `data-line-numbers` a sus `<code>` etiquetas. Si desea resaltar líneas específicas, puede proporcionar una lista de números de línea separados por comas utilizando el mismo atributo. Por ejemplo, en el siguiente ejemplo se resaltan las líneas 3 y 8-10:

``` html
<pre><code data-line-numbers="3,8-10">
<table>
  <tr>
    <td>Apples</td>
    <td>$1</td>
    <td>7</td>
  </tr>
  <tr>
    <td>Oranges</td>
    <td>$2</td>
    <td>18</td>
  </tr>
</table>
</code></pre>
```

#### Aspectos destacados paso a paso

Puede recorrer varios aspectos destacados de código en el mismo bloque de código. Delimita cada uno de tus pasos destacados con el `|` personaje. Por ejemplo `data-line-numbers="1|2-3|4,6-10"`, producirá tres pasos. Comenzará resaltando la línea 1, el siguiente paso son las líneas 2-3 y finalmente las líneas 4 y 6 a 10.

``` html
<pre><code data-line-numbers="3-5|8-10|13-15">
<table>
  <tr>
    <td>Apples</td>
    <td>$1</td>
    <td>7</td>
  </tr>
  <tr>
    <td>Oranges</td>
    <td>$2</td>
    <td>18</td>
  </tr>
  <tr>
    <td>Kiwi</td>
    <td>$3</td>
    <td>1</td>
  </tr>
</table>
</code></pre>
```

#### Entidades HTML

El contenido agregado dentro de un `<code>` bloque es analizado como HTML por el navegador web. Si tiene caracteres HTML (<>) en su código, necesitará escapar de ellos ($ lt; $ gt;).

Para evitar tener que escapar estos caracteres manualmente, puede ajustar su código `<script type="text/template">` y lo manejaremos por usted.

``` html
<pre><code><script type="text/template">
sealed class Either<out A, out B> {
  data class Left<out A>(val a: A) : Either<A, Nothing>()
  data class Right<out B>(val b: B) : Either<Nothing, B>()
}
</script></code></pre>
```

### Matemáticas

Si desea mostrar ecuaciones matemáticas en su presentación, puede hacerlo fácilmente con el complemento MathJax. Este es un contenedor muy delgado alrededor de la biblioteca **MathJax**.

El complemento utiliza por defecto **LaTeX**, pero eso se puede ajustar a través del `math` objeto de configuración. Tenga en cuenta que MathJax se carga desde un servidor remoto. Si desea usarlo sin conexión, deberá descargar una copia de la biblioteca y ajustar el `mathjax` valor de configuración.

A continuación se muestra un ejemplo de cómo se puede configurar el complemento. Si no tiene la intención de cambiar estos valores, no necesita incluir la `math` opción de configuración en absoluto.

```html
<script src="plugin/math/math.js"></script>
<script>
  Reveal.initialize({
    math: {
      mathjax: 'https://cdn.jsdelivr.net/gh/mathjax/mathjax@2.7.8/MathJax.js',
      config: 'TeX-AMS_HTML-full',
      // pass other options into `MathJax.Hub.Config()`
      TeX: { Macros: { RR: "{\\bf R}" } }
    },
    plugins: [ RevealMath ]
  });
</script>
```

```html
<section>
  <h2>The Lorenz Equations</h2>
  \[\begin{aligned}
  \dot{x} &amp; = \sigma(y-x) \\
  \dot{y} &amp; = \rho x - y - xz \\
  \dot{z} &amp; = -\beta z + xy
  \end{aligned} \]
</section>
```



#### Reducción

Si desea incluir matemáticas dentro de una presentación escrita en Markdown, debe envolver la fórmula con comillas invertidas. Esto evita conflictos de sintaxis entre LaTeX y Markdown. Por ejemplo:

`$$ J(\theta_0,\theta_1) = \sum_{i=0} $$`

### Fragmentos 

Los fragmentos se utilizan para resaltar o revelar de forma incremental elementos individuales en una diapositiva. Cada elemento de la clase `fragment` será detallado antes de pasar a la siguiente diapositiva.

El estilo de fragmento predeterminado es comenzar invisible y desaparecer. Este estilo se puede cambiar agregando una clase diferente al fragmento.

```html
<p class="fragment">Fade in</p>
<p class="fragment fade-out">Fade out</p>
<p class="fragment highlight-red">Highlight red</p>
<p class="fragment fade-in-then-out">Fade in, then out</p>
<p class="fragment fade-up">Slide up while fading in</p>
```

|Nombre|Efecto|
|--------|--------|
|fade-out|Comienza visible, desaparece|
|fade-up|Se desliza hacia arriba mientras se desvanece|
|fade-down|Se desliza hacia abajo mientras se desvanece|
|fade-left|Se desliza hacia la izquierda mientras te desvaneces|
|fade-right|Se desliza hacia la derecha mientras se desvanece|
|fade-in-then-out|Se desvanece y luego desaparece en el siguiente paso|
|fade-in-then-semi-out|Se desvanece, luego al 50% en el siguiente paso|
|grow|Aumentar proporcionalmente|
|shrink|Reducir proporcionalmente|
|strike|Penetrar|
|highlight-red|Cambiar el texto a rojo|
|highlight-green|Cambiar el texto a verde|
|highlight-blue|Cambiar el texto en azul|
|highlight-current-red|Cambia el texto a rojo y vuelve al original en el siguiente paso|
|highlight-current-green|Cambia el texto a verde y vuelve al original en el siguiente paso|
|highlight-current-blue|Cambia el texto a azul y vuelve al original en el siguiente paso|

#### Fragmentos anidados

Se pueden aplicar múltiples fragmentos al mismo elemento secuencialmente envolviéndolo, esto se desvanecerá en el texto en el primer paso, se volverá rojo en el segundo y se desvanecerá en el tercero.

```html
<span class="fragment fade-in">
  <span class="fragment highlight-red">
    <span class="fragment fade-out">
      Fade in > Turn red > Fade out
    </span>
  </span>
</span>
```

#### Orden de fragmentos

De forma predeterminada, los fragmentos se recorrerán en el orden en que aparecen en el DOM. Este orden de visualización se puede cambiar mediante el `data-fragment-index` atributo. Tenga en cuenta que pueden aparecer varios elementos en el mismo índice.

```html
<p class="fragment" data-fragment-index="3">Appears last</p>
<p class="fragment" data-fragment-index="1">Appears first</p>
<p class="fragment" data-fragment-index="2">Appears second</p>
```

#### Eventos

Cuando se muestra u oculta un fragmento, revel.js enviará un evento.

```js
Reveal.on( 'fragmentshown', event => {
  // event.fragment = the fragment DOM element
} );
Reveal.on( 'fragmenthidden', event => {
  // event.fragment = the fragment DOM element
} );
```

### Vínculos internos

Puede crear enlaces de una diapositiva a otra. Empiece por darle a su diapositiva de destino un `id` atributo único . A continuación, puede crear un ancla con un href en el formato `#/<id>`. Aquí hay un ejemplo de trabajo completo:

```html
<section>
	<a href="#/grand-finale">Go to the last slide</a>
</section>
<section>
	<h2>Slide 2</h2>
</section>
<section id="grand-finale">
	<h2>The end</h2>
	<a href="#/0">Back to the first</a>
</section>
```

#### Enlaces numerados

También es posible vincular a las diapositivas según su índice de diapositivas. El formato href para un enlace numerado es `#/0` donde 0 es el número de diapositiva horizontal. Para vincular a una diapositiva vertical, use `#/0/0` donde el segundo número es el índice del objetivo de la diapositiva vertical.

```html
<a href="#/2">Go to 2nd slide</a>
<a href="#/3/2">Go to the 2nd vertical slide inside of the 3rd slide</a>
```

#### Enlaces de navegación

Puede agregar enlaces de navegación relativos que funcionan de manera similar a las flechas de control direccional integradas. Esto se hace agregando una de las siguientes clases a cualquier elemento HTML en el que se pueda hacer clic dentro del `.reveal` contenedor.

```html
<button class="navigate-left">Left</button>
<button class="navigate-right">Right</button>
<button class="navigate-up">Up</button>
<button class="navigate-down">Down</button>

<!-- Previous vertical OR horizontal slide -->
<button class="navigate-prev">Prev</button>

 <!-- Next vertical OR horizontal slide -->
<button class="navigate-next">Next</button>
```

Cada elemento de navegación recibe automáticamente una `enabled` clase cuando es una ruta de navegación válida basada en la diapositiva actual. Por ejemplo, si estás en la primera diapositiva solo `navigate-right` tendrás la `enabled` clase ya que no es posible navegar hacia la izquierda.

### Diseño

Proporcionamos algunas clases de ayuda diferentes para controlar el diseño y diseñar su contenido.

#### Apilar

El `r-stack` asistente de diseño le permite centrar y colocar varios elementos uno encima del otro. Está destinado a utilizarse junto con **fragmentos** para revelar elementos de forma incremental.

```html
<div class="r-stack">
  <img class="fragment" src="https://placekitten.com/450/300" width="450" height="300">
  <img class="fragment" src="https://placekitten.com/300/450" width="300" height="450">
  <img class="fragment" src="https://placekitten.com/400/400" width="400" height="400">
</div>[]()
```

Si desea mostrar cada uno de los elementos apilados individualmente, puede ajustar la configuración del fragmento:

```html
<div class="r-stack">
  <img class="fragment fade-out" data-fragment-index="0" src="https://placekitten.com/450/300" width="450" height="300">
  <img class="fragment current-visible" data-fragment-index="0" src="https://placekitten.com/300/450" width="300" height="450">
  <img class="fragment" src="https://placekitten.com/400/400" width="400" height="400">
</div>
```

#### Ajustar texto

La `r-fit-text` clase hace que el texto sea lo más grande posible sin desbordar la diapositiva. Esto es excelente cuando desea un texto GRANDE sin tener que encontrar manualmente el tamaño de fuente correcto.

```html
<h2 class="r-fit-text">BIG</h2>
```

```html
<h2 class="r-fit-text">FIT TEXT</h2>
<h2 class="r-fit-text">CAN BE USED FOR MULTIPLE HEADLINES</h2>
```

#### Tramo

El `r-stretch` asistente de diseño le permite cambiar el tamaño de un elemento, como una imagen o un video, para cubrir el espacio vertical restante en una diapositiva. Por ejemplo, en el siguiente ejemplo, nuestra diapositiva contiene un título , una imagen y una firma . Debido a que la imagen tiene la` .r-stretch` clase, su altura se establece en la altura de la diapositiva menos la altura combinada del título y la línea .

```html
<h2>Stretch Example</h2>
<img class="r-stretch" src="/images/slides-symbol-512x512.png">
<p>Image byline</p>
```

##### Limitaciones de estiramiento

- Solo se pueden estirar los descendientes directos de una sección de deslizamiento.
- Solo se puede estirar un descendiente por sección de diapositiva.

#### Cuadro

Decora cualquier elemento `r-frame` para que destaque sobre el fondo. Si el elemento enmarcado se coloca dentro de un ancla, aplicaremos un efecto de desplazamiento al borde.

```html
<img src="logo.svg" width="200">
<a href="#">
  <img class="r-frame" src="logo.svg" width="200">
</a>
```

### Visibilidad de diapositivas

El `data-visibility` atributo se puede utilizar para ocultar diapositivas. También se puede utilizar para marcar diapositivas como "no contadas" en el sistema de numeración interno de revel.js, lo que afecta el número de diapositiva visible y la barra de progreso.

#### Diapositivas ocultas

Para ocultar una diapositiva de la vista, agregue `data-visibility="hidden"`. Las diapositivas ocultas se eliminan del DOM tan pronto como se inicializa revel.js.

```html
<section>Slide 1</section>
<section data-visibility="hidden">Slide 2</section>
<section>Slide 3</section>
```

#### Diapositivas no contadas

Al preparar una presentación, a veces puede ser útil preparar diapositivas opcionales que puede que tenga o no tiempo para mostrar. Esto se puede hacer fácilmente agregando algunas diapositivas al final de la presentación, sin embargo, esto significa que la barra de progreso de revel.js y la numeración de las diapositivas indicarán que hay diapositivas adicionales.

Para "ocultar" esas diapositivas del sistema de numeración de revel.js, puede usar `data-visibility="uncounted"`.

```html
<section>Slide 1</section>
<section>Slide 2</section>
<section data-visibility="uncounted">Slide 3</section>
```

### Temas

Reveal.js tiene diversos temas incluidos, como:

* Negro: fondo negro, texto blanco, enlaces azules.
* Blanco: fondo blanco, texto negro, enlaces azules.
* Liga: fondo gris, texto blanco, enlaces azules.
* Beige: fondo beige, texto oscuro, enlaces marrones.
* Cielo: fondo azul, texto oscuro delgado, enlaces azules.
* Noche: fondo negro, texto blanco grueso, enlaces naranjas.
* Serif: fondo capuchino, texto gris, enlaces marrones.
* Sencillo: fondo blanco, texto negro, enlaces azules.
* Solarizado: fondo de color crea, texto verde oscuro, enlaces azules.
* Sangre: fondo oscuro, texto blanco grueso, enlaces rojos.
* Luna: fondo azul oscuro, texto gris grueso, enlaces azules.

Para cambiar el tema debe reemplazar la palabra **negro** con el nombre del tema escogido en index.html:

```css
<link rel="stylesheet" href="dist/theme/black.css">
```

#### Propiedades personalizadas

Todas las variables de tema se exponen como propiedades personalizadas de CSS en la pseudoclase `:root`. En el siguiente enlace puedes consultar [la lista de variables](https://github.com/hakimel/reveal.js/blob/master/css/theme/template/exposer.scss).

#### Crear un tema

Para crear su propio tema, comience por duplicar un .scssarchivo en [/ css / theme / source](https://github.com/hakimel/reveal.js/tree/master/css/theme/source). Se compilará automáticamente de Sass a CSS. Consulte el archivo [gulp](https://github.com/hakimel/reveal.js/blob/master/gulpfile.js) cuando ejecute:

```sh
npm run build -- css-themes
```

### Transiciones

Al navegar por una presentación, hacemos la transición entre diapositivas animándolas de derecha a izquierda de forma predeterminada. Esta transición se puede cambiar estableciendo la `transition` opción de configuración en un estilo de transición válido . Las transiciones también se pueden anular para una diapositiva específica usando el `data-transition` atributo.

```html
<section data-transition="zoom">
  <h2>This slide will override the presentation transition and zoom!</h2>
</section>

<section data-transition-speed="fast">
  <h2>Choose from three transition speeds: default, fast or slow!</h2>
</section>
```

#### Estilos

Esta es una lista completa de todos los estilos de transición disponibles. Funcionan tanto para diapositivas como para fondos de diapositivas:

* ninguno: cambiar fontos al instante.
* desteñir: fundido cruzado, predeterminado para transiciones de fondo.
* diapositiva: deslizar entre fondos, predeterminado para transiciones de diapositivas.
* convexo: deslizarse en un ángulo convexo.
* cóncavo: deslizar en un ángulo cóncavo.
* zoom: escale la diapositiva entrante hacia arriba para que crezca desde el centro de la pantalla.

#### Transiciones separadas de entrada a salida

También puede usar diferentes transiciones de entrada y salida para la misma diapositiva agregando `-in` o `-out` al nombre de la transición.

```html
<section data-transition="slide">
    The train goes on …
</section>
<section data-transition="slide">
    and on …
</section>
<section data-transition="slide-in fade-out">
    and stops.
</section>
<section data-transition="fade-in slide-out">
    (Passengers entering and leaving)
</section>
<section data-transition="slide">
    And it starts again.
</section>
```

#### Transiciones de fondo

Hacemos la transición entre fondos de diapositivas usando un fundido cruzado por defecto. Esto se puede cambiar a nivel global o anular para diapositivas específicas. Para cambiar las transiciones de fondo para todas las diapositivas, use la `backgroundTransition` opción de configuración.

```js
Reveal.initialize({
  backgroundTransition: 'slide'
});
```

Alternativamente, puede usar el `data-background-transition` atributo en cualquier `<section>` para anular esa transición específica.

### Opciones de configuración

El comportamiento de la presentación se puede ajustar con una amplia gama de opciones de configuración.

Estos objetos se pueden incluir en el lugar en el que inicialice revel.js. También es posible cambiar los valores de configuración en tiempo de ejecución.

Tenga en cuenta que **todos** los valores de configuración son **opcionales** y estarán predeterminados a los valores especificados a continuación.

```js
Reveal.initialize({

  // Display presentation control arrows
  controls: true,

  // Help the user learn the controls by providing hints, for example by
  // bouncing the down arrow when they first encounter a vertical slide
  controlsTutorial: true,

  // Determines where controls appear, "edges" or "bottom-right"
  controlsLayout: 'bottom-right',

  // Visibility rule for backwards navigation arrows; "faded", "hidden"
  // or "visible"
  controlsBackArrows: 'faded',

  // Display a presentation progress bar
  progress: true,

  // Display the page number of the current slide
  // - true:    Show slide number
  // - false:   Hide slide number
  //
  // Can optionally be set as a string that specifies the number formatting:
  // - "h.v":   Horizontal . vertical slide number (default)
  // - "h/v":   Horizontal / vertical slide number
  // - "c":   Flattened slide number
  // - "c/t":   Flattened slide number / total slides
  //
  // Alternatively, you can provide a function that returns the slide
  // number for the current slide. The function should take in a slide
  // object and return an array with one string [slideNumber] or
  // three strings [n1,delimiter,n2]. See #formatSlideNumber().
  slideNumber: false,

  // Can be used to limit the contexts in which the slide number appears
  // - "all":      Always show the slide number
  // - "print":    Only when printing to PDF
  // - "speaker":  Only in the speaker view
  showSlideNumber: 'all',

  // Use 1 based indexing for # links to match slide number (default is zero
  // based)
  hashOneBasedIndex: false,

  // Add the current slide number to the URL hash so that reloading the
  // page/copying the URL will return you to the same slide
  hash: false,

  // Flags if we should monitor the hash and change slides accordingly
  respondToHashChanges: true,

  // Push each slide change to the browser history.  Implies `hash: true`
  history: false,

  // Enable keyboard shortcuts for navigation
  keyboard: true,

  // Optional function that blocks keyboard events when retuning false
  //
  // If you set this to 'foucsed', we will only capture keyboard events
  // for embdedded decks when they are in focus
  keyboardCondition: null,

  // Disables the default reveal.js slide layout (scaling and centering)
  // so that you can use custom CSS layout
  disableLayout: false,

  // Enable the slide overview mode
  overview: true,

  // Vertical centering of slides
  center: true,

  // Enables touch navigation on devices with touch input
  touch: true,

  // Loop the presentation
  loop: false,

  // Change the presentation direction to be RTL
  rtl: false,

  // Changes the behavior of our navigation directions.
  //
  // "default"
  // Left/right arrow keys step between horizontal slides, up/down
  // arrow keys step between vertical slides. Space key steps through
  // all slides (both horizontal and vertical).
  //
  // "linear"
  // Removes the up/down arrows. Left/right arrows step through all
  // slides (both horizontal and vertical).
  //
  // "grid"
  // When this is enabled, stepping left/right from a vertical stack
  // to an adjacent vertical stack will land you at the same vertical
  // index.
  //
  // Consider a deck with six slides ordered in two vertical stacks:
  // 1.1    2.1
  // 1.2    2.2
  // 1.3    2.3
  //
  // If you're on slide 1.3 and navigate right, you will normally move
  // from 1.3 -> 2.1. If "grid" is used, the same navigation takes you
  // from 1.3 -> 2.3.
  navigationMode: 'default',

  // Randomizes the order of slides each time the presentation loads
  shuffle: false,

  // Turns fragments on and off globally
  fragments: true,

  // Flags whether to include the current fragment in the URL,
  // so that reloading brings you to the same fragment position
  fragmentInURL: true,

  // Flags if the presentation is running in an embedded mode,
  // i.e. contained within a limited portion of the screen
  embedded: false,

  // Flags if we should show a help overlay when the question-mark
  // key is pressed
  help: true,

  // Flags if it should be possible to pause the presentation (blackout)
  pause: true,

  // Flags if speaker notes should be visible to all viewers
  showNotes: false,

  // Global override for autolaying embedded media (video/audio/iframe)
  // - null:   Media will only autoplay if data-autoplay is present
  // - true:   All media will autoplay, regardless of individual setting
  // - false:  No media will autoplay, regardless of individual setting
  autoPlayMedia: null,

  // Global override for preloading lazy-loaded iframes
  // - null:   Iframes with data-src AND data-preload will be loaded when within
  //           the viewDistance, iframes with only data-src will be loaded when visible
  // - true:   All iframes with data-src will be loaded when within the viewDistance
  // - false:  All iframes with data-src will be loaded only when visible
  preloadIframes: null,

  // Can be used to globally disable auto-animation
  autoAnimate: true,

  // Optionally provide a custom element matcher that will be
  // used to dictate which elements we can animate between.
  autoAnimateMatcher: null,

  // Default settings for our auto-animate transitions, can be
  // overridden per-slide or per-element via data arguments
  autoAnimateEasing: 'ease',
  autoAnimateDuration: 1.0,
  autoAnimateUnmatched: true,

  // CSS properties that can be auto-animated. Position & scale
  // is matched separately so there's no need to include styles
  // like top/right/bottom/left, width/height or margin.
  autoAnimateStyles: [
    'opacity',
    'color',
    'background-color',
    'padding',
    'font-size',
    'line-height',
    'letter-spacing',
    'border-width',
    'border-color',
    'border-radius',
    'outline',
    'outline-offset'
  ],

  // Controls automatic progression to the next slide
  // - 0:      Auto-sliding only happens if the data-autoslide HTML attribute
  //           is present on the current slide or fragment
  // - 1+:     All slides will progress automatically at the given interval
  // - false:  No auto-sliding, even if data-autoslide is present
  autoSlide: 0,

  // Stop auto-sliding after user input
  autoSlideStoppable: true,

  // Use this method for navigation when auto-sliding (defaults to navigateNext)
  autoSlideMethod: null,

  // Specify the average time in seconds that you think you will spend
  // presenting each slide. This is used to show a pacing timer in the
  // speaker view
  defaultTiming: null,

  // Enable slide navigation via mouse wheel
  mouseWheel: false,

  // Opens links in an iframe preview overlay
  // Add `data-preview-link` and `data-preview-link="false"` to customise each link
  // individually
  previewLinks: false,

  // Exposes the reveal.js API through window.postMessage
  postMessage: true,

  // Dispatches all reveal.js events to the parent window through postMessage
  postMessageEvents: false,

  // Focuses body when page changes visibility to ensure keyboard shortcuts work
  focusBodyOnPageVisibilityChange: true,

  // Transition style
  transition: 'slide', // none/fade/slide/convex/concave/zoom

  // Transition speed
  transitionSpeed: 'default', // default/fast/slow

  // Transition style for full page slide backgrounds
  backgroundTransition: 'fade', // none/fade/slide/convex/concave/zoom

  // The maximum number of pages a single slide can expand onto when printing
  // to PDF, unlimited by default
  pdfMaxPagesPerSlide: Number.POSITIVE_INFINITY,

  // Prints each fragment on a separate slide
  pdfSeparateFragments: true,

  // Offset used to reduce the height of content within exported PDF pages.
  // This exists to account for environment differences based on how you
  // print to PDF. CLI printing options, like phantomjs and wkpdf, can end
  // on precisely the total height of the document whereas in-browser
  // printing has to end one pixel before.
  pdfPageHeightOffset: -1,

  // Number of slides away from the current that are visible
  viewDistance: 3,

  // Number of slides away from the current that are visible on mobile
  // devices. It is advisable to set this to a lower number than
  // viewDistance in order to save resources.
  mobileViewDistance: 2,

  // The display mode that will be used to show slides
  display: 'block',

  // Hide cursor if inactive
  hideInactiveCursor: true,

  // Time before the cursor is hidden (in ms)
  hideCursorTime: 5000

});
```

#### Reconfigurar

La configuración se puede actualizar después de la inicialización mediante el `configure` método.

```js
// Turn autoSlide off
Reveal.configure({ autoSlide: 0 });

// Start auto-sliding every 5s
Reveal.configure({ autoSlide: 5000 });
```

### Tamaño de presentación

Todas las presentaciones tienen un tamaño "normal", es decir, la resolución a la que se crean. revel.js escalará automáticamente las presentaciones de manera uniforme en función del tamaño normal para garantizar que todo encaje en cualquier pantalla o ventana gráfica determinada sin cambiar la relación de aspecto o el diseño de su contenido.

A continuación puede consultar una lista de **opciones de configuración** relacionadas con el tamaño, incluidos sus valores predeterminados:

```js
Reveal.initialize({
  // The "normal" size of the presentation, aspect ratio will
  // be preserved when the presentation is scaled to fit different
  // resolutions. Can be specified using percentage units.
  width: 960,
  height: 700,

  // Factor of the display size that should remain empty around
  // the content
  margin: 0.04,

  // Bounds for smallest/largest possible scale to apply to content
  minScale: 0.2,
  maxScale: 2.0
});
```

#### Centrar

Las diapositivas se centran verticalmente en la pantalla según la cantidad de contenido que contienen. Para desactivar esta y dejar portaobjetos fijados en su conjunto altura configurado `center` a `false`.

```js
Reveal.initialize({ center: false })
```

#### Incrustado

De forma predeterminada, revel.js asumirá que debería cubrir toda la ventana del navegador. Si desea incrustar su presentación en una porción más pequeña de una página web, o mostrar **varias presentaciones** en la misma página, puede usar la `embedded` como opción de configuración:

```js
Reveal.initialize({ embedded: false })
```

Una presentación incrustada basará su tamaño en las dimensiones de su `.reveal` raíz. Si el tamaño de ese elemento cambia de una fuente que no sea el `resize` evento de ventana , puede llamar `Reveal.layout()` para activar manualmente una actualización de diseño.

```js
// Change the size of our presentation
document.querySelector( '.reveal' ).style.width = '50vw';

// Make reveal.js aware of the size change
Reveal.layout();
```

#### BYOL

Si desea deshabilitar la escala y el centrado integrados y Traiga su propio diseño, configure `disableLayout: true`. Eso hará que sus diapositivas cubran el 100% del ancho y alto de la página disponible y deje el estilo receptivo a usted.

```js
Reveal.initialize({
  disableLayout: false
});
```

### Diapositivas verticales

Por defecto, el deslizamiento entre diapositivas en reveal.js se realiza en horizontal. Estas diapositivas se consideran las de mayor nivel o principales en tu proyecto.

También es posible agrupar un número de diapositivas verticales bajo una diapositiva horizontal, lo cual está pensado para agrupar contenido opcional que está conectado de manera lógica entre sí.

Se utilizan las flechas horizontales para desplazarse por el nivel superior de diapositivas. Al llegar a una agrupación vertical de diapositivas, es posible desplazarse entre ellas usando las flechas de desplazamiento vertical.

Un código de un ejemplo para programar esta estructura sería:

```html
<section>Horizontal Slide</section>
<section>
  <section>Vertical Slide 1</section>
  <section>Vertical Slide 2</section>
</section>
```

Es posible configurar más ampliamente la navegación entre diapositivas usando la opción de configuración navigationMode. La configuración por defecto (*default*) es la explicada anteriormente. El modo lineal (*linear*) hace que solo funcionen las flechas horizontales y permite navegar por todas las diapositivas en orden. Por último, el modo tabla (*grid*) hace que, al encontrarse en una diapositiva vertical, el moverse de forma horizontal permita llegar a una diapositiva de otra pila vertical que se encuentra al mismo nivel que aquella en la que nos encontrábamos, en lugar de ir a la siguente diapositiva principal.

### Auto-animar

reveal.js permite animar de modo automático elementos que se encuentran en diapositivas contiguas. Para ello, es necesario añadir "data-auto-animate" a dos láminas adyacentes, y los elementos comunes entre ellas obtendrán animación.

```html
<section data-auto-animate>
  <h1>Auto-Animate</h1>
</section>
<section data-auto-animate>
  <h1 style="margin-top: 100px; color: red;">Auto-Animate</h1>
</section>
```

Es posible de este modo animar transiciones en la mayoría de propiedades CSS animables, como *position*, *font-size*, *line-height*, *color*, *background-color*, *padding* y *margin*.

La auto-animación se puede usar no sólo para animar el mismo elemento con cambios estilísticos, sino también para animar elementos cuando se añaden otros elementos nuevos en la siguiente diapositiva:

```html
<section data-auto-animate>
  <h1>Implicit</h1>
</section>
<section data-auto-animate>
  <h1>Implicit</h1>
  <h1>Animation</h1>
</section>
```

reveal.js trata de encontrar elementos compatibles para animación de forma automática, pero también es posible forzar de manera manual una animación. Esto se realiza dándole a las diapositivas que quieres animar el mismo atributo *data-id*:

```html
<section data-auto-animate>
  <div data-id="box" style="height: 50px; background: salmon;"></div>
</section>
<section data-auto-animate>
  <div data-id="box" style="height: 200px; background: blue;"></div>
</section>
```

Es posible cambiar la configuración de animación para la presentación completa, para diapositivas específicas y para elementos individuales mediante atributos como *data-auto-animate-easing* o *data-auto-animate-duration*.

Es posible animar adiciones a listas:

```html
<section data-auto-animate>
  <ul>
    <li>Mercury</li>
    <li>Jupiter</li>
    <li>Mars</li>
  </ul>
</section>
<section data-auto-animate>
  <ul>
    <li>Mercury</li>
    <li>Earth</li>
    <li>Jupiter</li>
    <li>Saturn</li>
    <li>Mars</li>
  </ul>
</section>
```

### Auto-deslizar

Es posible configurar las diapositivas para que el desplazamiento entre las mismas se realice de modo automático. Para ello, es necesario especificar un intervalo de tiempo en milisegundos entre cada transición en la opción de configuración autoSlide:

```js
// Cambia la diapositiva cada cinco segundos
Reveal.initialize({
  autoSlide: 5000,
  loop: true
});
```

Un botón de pausa/reproducción aparecerá de modo automático en diapositivas que tengan el auto-deslizar activado. Las diapositivas también dejarán de desplazarse automáticamente si el usuario interactúa con la presentación de algún modo, aunque pueden volver a transicionar por sí solas si se pulsa el botón de reproducción posteriormente.

Se pueden desactivar los controles de reproducción mediante la opción *autoSlideStoppable*, estableciéndola como false.

Es posible sobreescribir la duración de los intervalos entre diapositivas con el atributo *data-autoslide*:

```html
<section data-autoslide="2000">
  <p>After 2 seconds the first fragment will be shown.</p>
  <p class="fragment" data-autoslide="10000">After 10 seconds the next fragment will be shown.</p>
  <p class="fragment">Now, the fragment is displayed for 2 seconds before the next slide is shown.</p>
</section>
```

También es posible configurar la presentación para que ignore todas las diapositivas verticales:

```js
Reveal.configure({
  autoSlideMethod: Reveal.navigateRight
});
```

### Notas para el presentador

reveal.js tiene incorporado un plugin que permite al orador incluir notas en su presentación que se abren en una ventana aparte del navegador. En esta ventana también se muestra un adelanto de la siguiente diapositiva de la presentación. Para abrir esta ventana se debe pulsar la tecla S en el teclado. Esta acción también inicia un temporizador en la ventana nueva.

Para incorporar notas a una diapositiva se emplea la etiqueta **aside**.

```html
<section>
  <h2>Some Slide</h2>

  <aside class="notes">
    Shhh, these are your private notes 📝
  </aside>
</section>
```

Al realizar esta presentación en modo local, esta característica requiere que se ejecute la presentación desde un servidor web local.

Las notas son, por defecto, sólo visibles para el presentador. Para compartir estas notas, inicializa reveal.js con la opción *showNotes* configurada como true.

### Número de diapositiva

Es posible mostrar el número de la diapositiva en la que nos encontramos cambiando la configuración de la opción *slideNumber* a true.

```js
Reveal.initialize({ slideNumber: true });
```

El formato puede ser personalizado mediante las siguientes opciones:

|  Valor | Descripción  |
|---|---|
|  h.v | Número Horizontal.vertical (por defecto)  |
|  h/v |  Número horizontal/vertical |
|  c | Indice único dentro del número total de diapositivas  |
|  c/t |  Posición dentro del número total/número total |

Es posible también configurar cuándo se quiere mostrar los números de diapositiva utilizando la opción showSlideNumber:

| Valor  |  Descripción |
|---|---|
| all  |  Mostrar números de diapositiva en todos los contextos (por defecto) |
|  print |  Mostrar números sólo al imprimir en PDF |
| speaker  | Mostrar números sólo en la vista del presentador |

### Modo panorámico

Pulsa las teclas ESC o O para activar o desactivar el modo panorámico. En este modo puedes ver todas las diapositivas de la presentación y navegar entre ellas.

### Modo de pantalla completa

Para ver la presentación a pantalla completa, pulsa la tecla F. Para salir del modo de pantalla completa, pulsa ESC
