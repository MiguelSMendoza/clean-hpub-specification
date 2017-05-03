# Clean HPub Specification 1.0

## Abstract

En este documento se detallan las características que debe poseer un paquete CHPub para ser transformado en cualquier sistema de publicación. Basado en la [especificación de HPub](https://github.com/bakerframework/baker/wiki/hpub-specification) de Alessandro Morandi para Baker Framework pretende simplificar el mismo y delegar la tarea del diseño y la forma de presentación en el medio de transformación.

# Especificación

1. Una publicación CHPub es un conjunto de ficheros empaquetados en un fichero **ZIP** con el formato: [NOMBRE].zip
2. Una publicación está formada por diferentes apartados. Cada apartado constará de un fichero HTML nombrado con el formato [NOMBRE]_A[XX].html siendo NOMBRE el nombre de la publicación, y XX un número entre 00 y 99. 
3. Los recursos adicionales de cada apartado como imágenes, vídeos, etc. serán localizados en un directorio de nombre [NOMBRE]_A[XX], y referenciados en su correspondiente fichero de apartado de forma relativa. 
  * Ej.: ```<img src="CHPub_A02/img/img01.jpg" alt="Lorem"/>```
4. Incluye un fichero denominado "book.json" con metadatos sobre la publicación como se describe en el apartado *book.json*.
5. El código HTML que se utilizará en los ficheros deberá ser lo más sencillo posible, evitando utilizar CSS y limitando el uso de etiquetas más allá de las que se especifican en el correspondiente apartado *etiquetas*.
6. Se permite el uso de etiquetas personalizadas relacionadas con extensiones definidas en el apartado *extensiones*.
7. Incluyendo una imagen en la raíz de la publicación con el nombre cover.jpg se podrá utilizar en las portadas de las publicaciones que se generarán. Se recomienda una resolución de 1920x1080.

# Estructura

```
├── CHPub
│   ├── cover.png
│   ├── book.json
│   ├── CHPub_A1.html
│   ├── CHPub_A1
│   │   ├── image
│   │   │   ├── cover.png
│   │   │   ├── image01.png
│   │   ├── video
│   │   │   ├── video1.mp4
│   │   │   ├── video2.mp4
│   │   ├── audio
│   │   │   ├── audio1.mp3
│   ├── CHPub_A2.html
│   ├── CHPub_A2
│   │   ├── image
│   │   │   ├── cover.png
│   │   │   ├── image01.png
│   │   │   ├── image02.png
```

# book.json

El fichero *book.json* se situará en la raíz de la publicación y deberá contener un objeto JSON bien formado. 
Ejemplo de fichero book.json:
```
{    
	"title": "Lorem Ipsum",
	"author": ["Miguel S. Mendoza", "Marta S. Román"],
	"creator": ["Gabriel S. Román"],
	"editor":"NetRunners",
	"date": "2016-04-29",
	"url": "http://www.netrunners.es/",
	"description":"Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
	"keywords":"specification, hpub, publication",
	"contents": [
		"CHPub_A01",
		"CHPub_A02",
		"CHPub_A03",
		"CHPub_A04"
	]
}
```

## Parámetros

* **title** (*string*): Título de la publicación.
* **author** (*array*): Autor/es de la publicación. Persona/s generadoras de los contenidos independientemente del formato. Un string con el nombre de cada autor diferente.
* **creator** (*array*): Creador/es de la publicación. Persona/s creadoras de la publicación en CHPub. Un string con el nombre de cada creador diferente.
* **editor** (*string*): Nombre del editor de la publicación.
* **date** (*string*): Fecha de la publicación en formato YYYY-MM-DD.
* **url** (*string*): URL de la publicación o de la editorial.
* **description** (*string*): Descripción de la publicación.
* **keywords** (*string*): Palabras clave relacionadas con la publicación.
* **contents** (*array*): Lista de todos los ficheros de apartados de la publicación en el orden de visualización. 

# Etiquetas

A diferencia de la especificación original de HPub, en Clean HPub se pretenden crear publicaciones con el menor diseño posible, centradas en el contenido. De esta forma el diseño dependerá exclusivamente del sistema de transformación que reciba la publicación como entrada y genere una publicación en otro formato, ya sea PDF, EPub, etc. 
La regla es simple: Se permite cualquier etiqueta del estándar HTML4 siempre y cuando no se utilicen el atributo *style*.
Más adelante, en el apartado *extensiones*, se detallarán etiquetas adicionales relacionadas con los componentes extra que se podrán utilizar. 

## Relación de etiquetas HTML válidas


Para limitar el diseño se especifican a continuación las etiquetas HTML que se permitirán por defecto en lo ficheros HTML de cada apartado para una publicación de libro normal.
* **html**: Etiqueta contenedora de las etiquetas head y body.
* **head**: En ella se incluirán algunos metadatos permitidos para cada apartado. Por el momento las etiquetas permitidas son:
  * **title**: Título del apartado.
* **body**: Etiqueta contenedora del resto de etiquetas del apartado.
* **h1**: Título del apartado. Solo debe existir uno.
* **h*X***: Títulos de subapartados siendo X el nivel de ordenación. Se debe respetar la estructura de niveles de subarpartados:
  * No se debe introducir un h3 si antes no se ha introducido un h2. 
  * Todo h3 se considera subapartado del h2 inmediatamente anterior, del mismo modo que un h4 lo hará para un h3, y así sucesivamente.
* **p**: Párrafos de contenido.
* **blockquote**: Se utiliza para hacer referencia a una cita externa.
* **strong**: Texto en negrita.
* **u**: Texto subrayado.
* **i**: Texto en cursiva.
* **a**: Hiperenlace. Los siguientes atributos son obligatorios:
  * ***href***: Ruta del enlace.
  * ***title***: Descripción del enlace.
* **ul**: Lista no numerada. Para cada elemento de la lista se utilizará la etiqueta li.
* **ol**: Lista numerada. Para cada elemento de la lista se utilizará la etiqueta li.
* **img**: Imágenes. Los siguientes atributos son obligatorios:
  * ***src***: Ruta relativa a la imagen. Las imágenes deberán encontrarse en la carpeta del apartado.
  * ***alt***: Descripción de la imagen.
  
# Extensiones

Además de las etiquetas HTML básicas descritas anteriormente, y para aumentar la versatilidad de las publicaciones, se han creado etiquetas específicas para diferentes tipos de material. Algunas de ellas están relacionadas con extensiones externas HTML5 que cambiarán dependiendo de la transformación a realizar.



## Etiquetas

* **gallery**: Esta etiqueta se utilizará para englobar a un conjunto de etiquetas **img** y mostrarlas como una galería de imágenes.
```
<gallery>
    <img src="CHPub_A1/images/imagen01.jpg" alt="Caption 01" />
    <img src="CHPub_A1/images/imagen02.jpg" alt="Caption 02" />
    <img src="CHPub_A1/images/imagen03.jpg" alt="Caption 03" />
</gallery>
```

* **tooltip**: Permite mostrar un pequeño *tooltip* de ayuda sobre un texto concreto. Útil para notas de autor o pequeñas aclaraciones.
```
<p>Proin <tooltip title="Tooltip Text.">convallis cursus</tooltip> massa.</p>
```

* **video**: Incluye un vídeo en la publicación. El vídeo podrá ser un fichero en la carpeta del apartado o un identificador de vídeo externo como Vimeo o Youtube. En los siguientes ejemplos los comentarios no son necesarios, solo se añaden para que se identifique el identificador de cada vídeo externo:
```
<video src="CHPub_A01/video/vide01.mp4">Video description.</video>
<-- https://vimeo.com/119787432 -->
<video type="vimeo" src="119787432">Video description.</video>
<-- https://www.youtube.com/watch?v=bYE67_Wfizw -->
<video type="youtube" src="bYE67_Wfizw">Video description.</video>
```

* **videogallery**: Genera una galería con los vídeos que se introduzcan entre la etiqueta de apertura y cierre:
```
<videogallery>
    <source title="Video Title" src="CHPub_A01/video/vide01.mp4">Video description.</source>
    <source title="Video Title" type="vimeo" src="119787432">Video description.</source>
    <source title="Video Title" type="youtube" src="bYE67_Wfizw">Video description.</source>
<videogallery>
```

* **audio**: Etiqueta de audio que hará referencia a un fichero en la carpeta del apartado.
```
<audio src="CHPub_A1/audio/audio01.mp3">Audio Description 01.</audio>
```

* **columns**: Estructura el contenido en dos o más columnas. Cada columna se representa con la etiqueta **column**.
```
<columns>
    <column>
      <p>Ut dictum lectus in turpis lacinia, et blandit metus ornare.</p>
    </column>
    <column>
      <p>Nullam at tortor nec nibh interdum laoreet.</p>
    </column>
</columns>
```

# Ejemplo de Apartado

```
<html>
	<head>
		<title>Lorem Ipsum</title>
	</head>
	<body>
		<h1>Neque porro quisquam est qui dolorem</h1>
		<p>Lorem ipsum dolor sit <strong>amet</strong>, consectetur adipiscing elit.</p>
		<ul>
			<li>Proin</li>
			<li>luctus</li>
			<li>libero</li>
		</ul>
		<img src="CHPub_A1/images/imagen01.jpg" alt="Image Title">
		<p>In hac habitasse platea <u>dictumst</u>. Interdum et malesuada fames ac ante ipsum primis in <i>faucibus</i>.</p>
		<video type="vimeo" src="119787432">Video Decription 01.</video>
		<h2>Ipsum quia dolor sit amet</h2>
		<p>Pellentesque hendrerit leo quis mi <a href="http://www.netrunners.es" title="Link Description">lobortis tincidunt</a>. Proin quis felis eget orci <tooltip title="Tooltip Text.">convallis cursus</tooltip> ac eget massa.</p>
		<ol>
			<li>odio</li>
			<li>lectus</li>
		</ol>
		<video type="youtube" src="bYE67_Wfizw">Video Decription 02.</video>
		<h3>Integer orci leo</h3>
		<blockquote>Donec congue fermentum viverra.</blockquote>
		<columns>
			<column>
			<p>Ut dictum lectus in turpis lacinia, et blandit metus ornare.</p>
			</column>
			<column>
			<p>Nullam at tortor nec nibh interdum laoreet.</p>
			</column>
		</columns>
		<video src="CHPub_A1/video/video01.mp4">Video Decription 03.</video>
		<h2>Dolor sit amet</h2>
		<p>Donec sollicitudin diam neque, non lobortis dolor blandit in.</p>
		<h3>In hac habitasse platea dictumst</h3>
		<p>Morbi nunc tortor, pellentesque quis augue vel, bibendum venenatis tellus.</p>
		<audio src="CHPub_A1/audio/audio01.mp3">Audio Description 01.</audio>
		<p>Mauris eget augue vitae ipsum hendrerit finibus convallis et urna.</p>
		<gallery>
			<img src="CHPub_A1/images/imagen02.jpg" alt="Caption 01">
			<img src="CHPub_A1/images/imagen03.jpg" alt="Caption 02">
			<img src="CHPub_A1/images/imagen04.jpg" alt="Caption 03">
		</gallery>
	</body>
</html>
```
# Más Información

[SMendoza](https://www.smendoza.net/clean-hpub-specification/)