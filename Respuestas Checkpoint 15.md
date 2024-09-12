

                 **CHECKPOINT 15**

**¿Qué es Axios? ¿que beneficios tiene? ¿cúando lo usarías?**

Axios es una librería de Javascript que está enfocada en ayudarnos en las comunicaciones de envío / solicitud de información entre un programa que nosotros creemos y un servidor o una API que provee / recibe datos para su almacenamiento, proceso, etc

Utiliza las **solicitudes HTTP** para comunicarse con una API, lo cual nos permite interactuar con ella para obtener, crear, actualizar o eliminar datos (mediante solicitudes `GET`, `POST`, `PUT`, `DELETE`, etc.).

A modo de ejemplo se muestra como sería la obtención de datos a través de una solicitud HTTP empleando la librería **Axios**; más adelante se acompañará un ejemplo en detalle, siendo este más enfocado a mostrar la estructura que utiliza: 

```
axios.get('https://api.example.com/data')

.then(response => {

console.log('Datos recibidos:', response.data);

})

.catch(error => {

console.error('Error en la solicitud GET:', error);

});
```
A pesar de las pocas líneas que implementa el código, ya nos está dando una lectura a través de la consola, así como gestionando y mostrando mensajes de error en el caso de que la solicitud fuera fallida…

Una de las grandes ventajas que tiene **Axios**es que realiza las citadas solicitudes de forma asíncrona. Normalmente estas acciones no se ejecutarán instantáneamente, sino que demorarán algo de tiempo en llevarse a cabo. En lugar de esperar, la página seguirá corriendo mientras **Axios** se encarga de gestionar los estados de las Promesas, que es como se les llama a estas operaciones asíncronas.

Los estados de una Promesa son los siguientes:

**Pendiente (Pending)**: Cuando la operación asíncrona aún no ha terminado.

**Resuelta (Fulfilled)**: Cuando la operación asíncrona se completa exitosamente.

**Rechazada (Rejected)**: Cuando la operación asíncrona falla.

 

Antes de poder utilizar la librería, hemos de importarla como se hace habitualmente con cualquier librería externa:

```
import axios from 'axios';
```
Y ahora ya podríamos realizar una solicitud a la API, para obtener información por ejemplo del índice de desertificación de la península.

```
axios.get('https://opendata.aemet.es/opendata/api/valores/climatologicos/inventarioestaciones/todasestaciones/?api_key=jyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJqbW9udGVyb2dAYWVtZXQuZXMiLCJqdGkiOiI3NDRiYmVhMy02NDEyLTQxYWMtYmYzOC01MjhlZWJlM2FhMWEiLCJleHAiOjE0NzUwNTg3ODcsImlzcyI6IkFFTUVUIiwiaWF0IjoxNDc0NjI2Nzg3LCJ1c2VySWQiOiI3NDRiYmVhMy02NDEyLTQxYWMtYmYzOC01MjhlZWJlM2FhMWEiLCJyb2xlIjoiIn0.xh3LstTlsP9h5cxz3TLmYF4uJwhOKzA0B6-vH8lPGGw')


.then(response => { console.log('Datos recibidos:', response.data); })
```
La respuesta que nos daría la API de AEMET en este caso sería un conjunto de datos con el siguiente formato:
```
{

"descripcion" : "exito",

"estado" : 200,

"datos" : "https://opendata.aemet.es/opendata/sh/f24dba3a",

"metadatos" : "https://opendata.aemet.es/opendata/sh/142417df"

}
```
En los datos recibidos podemos observar que la solicitud se ejecutó satisfactoriamente por el valor de estado 200. 

Yendo a la página  **[https://opendata.aemet.es/opendata/sh/f24dba3a](https://opendata.aemet.es/opendata/sh/f24dba3a)**, obtenemos una imagen de la península ibérica con el mapa solicitado. (Se puede apreciar que toda la Cornisa Cantábrica, donde vivimos,  es un auténtico vergel vestidito de #008f39.



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


NOTA: Para poder extraer datos de las API de AEMET necesitamos registrarnos para recibir una API KEY, que no es sino una serie de caracteres que nos permiten validarnos/autenticarnos para realizar la petición.

El enlace adjunto sólo funcionará si se dispone del API KEY que se utilizó en la solicitud, dando un error de página al cargarse en cualquier otro escritorio.

En el siguiente ejemplo se muestra otro de los puntos fuertes de **Axios**, la gestión de errores. La propia librería se encargará del manejo de los errores que se nos puedan presentar en la comunicación con la API.

```
axios.get('https://api.example.com/data')

.then(response => {

console.log('Datos recibidos:', response.data);

// Maneja la respuesta exitosa

})

.catch(error => {

// Manejar errores

if (error.response) {

// La solicitud se completó y el servidor respondió con un código de estado fuera del rango 2xx

console.error('Error en la respuesta:', error.response.data);

console.error('Código de estado:', error.response.status);

console.error('Encabezados:', error.response.headers);

} else if (error.request) {

// La solicitud fue hecha pero no se recibió respuesta (problema de red, timeout, etc.)

console.error('No se recibió respuesta del servidor:', error.request);

} else {

// Ocurrió un error al configurar la solicitud

console.error('Error al configurar la solicitud:', error.message);

}

console.error('Información completa del error:', error.config);

// Muestra la configuración que se usó en la solicitud

**});
```

Según las respuestas que nos dé el servicio, podremos mostrar un error u otro para aislar y enfocar cuál ha sido el error que se ha producido al hacer la solicitud.


### El manejo de los errores sería el siguiente:

**error.response**: Aquí es donde encuentras la respuesta del servidor. Si la solicitud se envió correctamente pero el servidor devolvió un error (como un **404** o un **500**), puedes acceder a los datos de la respuesta (**error.response.data**), el código de estado (error.response.status), y los encabezados (**error.response.headers**).

**error.reques<code>t</code></strong>: Si la solicitud fue enviada pero <strong>no se recibió ninguna respuesta</strong>, encontrarás los detalles del error en <strong>error.request</strong>. Esto puede suceder en situaciones como problemas de red o cuando el servidor no está disponible.

**error.message**: Si ocurrió un error al configurar la solicitud (antes de que se enviara), error.message te dará detalles sobre ese error.

**error.config**: Muestra la configuración original de la solicitud, lo que puede ser útil para depurar problemas.

En resumen, se podría decir que **Axios **es como un cartero que nos permite hacer envíos de cartas/paquetes  a cualquier dirección existente, así como también el recibo de los mismos. En el caso de que hubiera algún problema con los paquetes enviados o recibidos, el mismo cartero se encargará de gestionar los errores y comunicárnoslo si así se lo hemos indicado previamente.

**¿Por qué es útil React Devtools?**

Básicamente porque te permite acceder en tiempo real a valores de estado (**state**) o propiedades (**props**) de forma directa a través de la búsqueda de los mismos vía la opción **componentes**, para hacer pruebas de funcionamiento, comprobar el estado de cualquier elemento u obtener información en un determinado momento sobre el valor que tenemos dentro de una propiedad en concreto.

Resumiendo, obtener información detallada sobre el estado y las propiedades de cada componente que conforma nuestra estructura del árbol de componentes en tiempo real.


### A pesar de que React DevTools tiene como principal objetivo el comentado al principio, las opciones que nos otorga de cara a optimizar el depurado del código son numerosas, y se listan a continuación las principales:


### **Inspeccionar la estructura de componentes**

React DevTools te permite ver la estructura del árbol de componentes de tu aplicación React. Esto es importante porque React maneja el DOM virtual y puede ser difícil ver cómo los componentes se relacionan entre sí solo desde el DOM real.



* Puedes expandir y colapsar los componentes para explorar su jerarquía.
* Ver qué componentes están montados en qué parte de la aplicación.


### **Ver y modificar el estado y las props**

Puedes ver y **manipular el estado (<code>state</code>)</strong> y las <strong>propiedades (<code>props</code>)</strong> de los componentes.



* Te permite saber cuál es el estado actual de un componente en cualquier momento.
* Puedes modificar el estado o las props directamente desde React DevTools para probar cómo se comporta tu aplicación sin necesidad de cambiar el código.

Esto es clave cuando estás depurando un comportamiento inesperado o verificando cómo se actualizan los estados.


### **Depurar componentes de clase y funcionales con hooks**

Si utilizamos **hooks**, React DevTools nos permite inspeccionar los **valores de los **mismos dentro de los componentes funcionales.



* Puedes ver el valor de cada hook como **useState, useEffect, useContext, **etc., lo que facilita mucho depurar problemas relacionados con éstos.


### **Simular ciclos de vida de los componentes**

En los componentes de clase, React DevTools también te permite ver el ciclo de vida del componente, como cuando se montan, actualizan o desmontan. Esto puede ser útil para ver en qué punto de su ciclo de vida un componente está causando problemas. Implementandolo con la opción debugger, podremos parar la aplicación allá donde sepamos que nos está dando problemas, y chequear ese punto como si congeláramos la reproducción en un video y pudiésemos analizar -y modificar- en ese momento cualquier detalle de la imagen -


### **Depuración en tiempo real**

Lo que se muestras en React DevTools se actualiza en tiempo real mientras interactúas con la aplicación. Esto quiere decir que se puede hacer clic en un botón, cambiar algo en tu aplicación y ver cómo eso afecta el estado, las props y el comportamiento del componente en ese mismo instante.


### **Uso polivalente tanto en desarrollo como en producción**

React DevTools no solo es útil en el entorno de desarrollo, sino que también se puede usar en producción, lo que te permite inspeccionar problemas directamente en una aplicación en vivo.

**¿Qué es un event listener? ¿que beneficios tiene? pon ejemplos**

Un event listener es una función de JavaScript que se utiliza dejando unos “testigos” como escucha en el código, a la espera que se produzcan determinadas acciones dentro de la página para que se ejecuten, respondiendo así a eventos específicos en la web, como clicks, teclas presionadas, desplazamientos, y demás.


### El principal beneficio que achacaría yo a los event listener es la automatización del proceso. De hecho, una vez que hayamos programado el mismo, éste esperará hasta que se cumpla la condición que desencadenará la ejecución de una acción determinada consecuencia del evento. Si no dispusiéramos de esta herramienta, simplemente tendríamos que tener a alguien pendiente de la página, e interactuar manualmente cuando se diera un evento determinado. Esto no es viable…

Seguidamente acompaño un listado ampliando las características principale de los event listener:



1. **Interactividad**: Permite que la aplicación responda de forma automática a las acciones del usuario que coincidan con las que nosotros hayamos programado previamente en los listeners, como hacer clic en un botón,  mover el ratón por un área del DOM determinada, llevar el puntero a un lugar, etc.
2. **Desacoplamiento**: Separan la lógica de manejo de eventos del resto del código, lo que hace que éste esté mejor organizado, lo que redundará en el futuro en que las labores para mantener el código sean más sencillas, al ser el conjunto más modular y organizado.
3. **Flexibilidad**: Según se requiera en la página, se pueden añadir, quitar o modificar event listeners de forma muy dinámica, adaptando el comportamiento de la página o los objetivos de los mismos, según sea necesario.


### A continuación se acompaña una lista de usos más habituales dentro del abanico de usos de Event Listeners:


#### 1. **Espera de un click en botón**

Este es uno de los usos más comunes. Queremos saber cuando el usuario va a pulsar un botón para realizar determinada acción en la página; podría tratarse de la casilla de un formulario, señalar una casilla de validación, o cualquier otra…

```
&lt;button id="miBoton">Haz clic en mí&lt;/button>

const boton = document.getElementById('miBoton');

boton.addEventListener('click', () => {

alert('¡Botón clickeado!');

});

```
Cuando el usuario hace click en el botón, se mostrará un mensaje de alerta.

La primera parte del código, la referida al botón, pertenece al HTML.


#### 2. **Espera de la pulsación de una tecla**

Event listeners esperando al presionar de una  tecla.

```
document.addEventListener('keydown', (event) => {

console.log(`Tecla presionada: ${event.key}`);

});
```
Cada vez que el usuario presiona una tecla, se imprime en la consola el nombre de la tecla presionada.


#### 3. **Escuchar el desplazamiento de la página**

Respuesta al desplazamiento del usuario en la página.

```
window.addEventListener('scroll', () => {

console.log('La página se está desplazando.');

});
```

Cada vez que el usuario se desplaza por la página, se imprime un mensaje en la consola.


#### 4. **Escuchar un cambio en un campo de texto**

Puedes reaccionar cuando el valor de un campo de texto cambia.


```
&lt;input type="text" id="miCampoTexto" placeholder="Escribe algo">

const campoTexto = document.getElementById('miCampoTexto');

campoTexto.addEventListener('input', () => {

console.log(`Valor actual: ${campoTexto.value}`);

});
```
Cuando el usuario escribe o cambia el valor del campo de texto, se muestra el nuevo valor en la consola.
