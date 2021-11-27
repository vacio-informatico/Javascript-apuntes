# Javascript

Apunte sobre el nivel medio de javascript.

## Window 

Window es el objeto con mas alta jerarquia en js, incluso el DOM depende de window.

* open() - carga un recurso en el contexto (crea una ventana emergente)
* close() - cierra la ventana emergente
* closed - verifica si la ventana referente esta cerrada o no (verifica el estado de una ventana)
* stop() - detiene la carga de una ventana (detiene todo)
* print() - abre un cuadro de dialogo para imprimir el documento (imprime ventana)
* prompt() - solicita un dato al usuario
* confirm() - solicita una confirmacion (retorna un bool)

```javascript
    let win = open('vacio-informatico.github.io/v-i')
    win.close()
    win.closed
    
    window.stop()

    alert()
    print()

    prompt('Ingrese un dato')
    let answ = confirm('Seguro quieres salir?') //true or false
```

* screen - retorna una referencia al objeto de pantalla asociado con la ventana (retorna las propiedades de la ventana)-[objeto]
* screeenLeft - retorna la distancia horizontal entre el borde izquierdo del navegador y el borde de la pantalla (distancia entre bordes: pantalla - ventana)
* screeeTop - lo mismo 
* srollX - retorna el numero de px que el doc se desplaxa en X (px de sroll - cuanto te desplazaste en px)
* scrollY - retorna el numero de px que el doc se desplaxa en Y (pixeles de scroll) 
* scroll() - desplaza la ventana a un lugar particular del doc(con options y posiciones) - ()
* scrollTo() - es casi lo mismo que scroll()

```javascript
    const screen = window.screen
    screen.width //alcho de la pantalla (total)

    window.screeenLeft // es de lectura
    window.screeeTop
```
```javascript
    const x = window.srollX
    const y = window.scrollY
    alert(x, y)

    window.scroll(0,100) //obsoluto
    window.scrollTo()
    /* relativos*/
    window.scrollBy
    window.scrollByLines
    window.scrollByPages
```

* resizeBy() . cambia el temaño de la ventana a una cantidad especifica
* resizeBy() - redimenciona dinamicamente la ventana
* moveBy() - mueve la ventana obsolutamente
* moveTo - mueve la ventana relativamente

```javascript
    /* necesita servidor*/
    window.minimize() //no tiene soporte
    win = window.resizeBy(100, 200)
    resizeTo()
    win.moveBy(100, 200)
    moveTo()
```

### location

* location.href - retorna URL del sitio
* location.hostname - retorna dominio y sub dominio
* location.pathname - retorna rota y nombre del archivo actual (ruta del doc)
* location.protocol - retorna protocolo usado (http || https)
* location.assign() - carga un nuevo documento

```javascript
    const href = window.location.href
    document.write(href)

    window.location.hostname
    window.location.pathname 
    window.location.protocol

    window.location.assign('vacio-informatico.github.io/v-i')
```

## Eventos (doc_3)

Todo es un evento UwU

1. Un evento o "event handlers" - cada cosa que sucede.
2. Escuchar eventos o "event listeners" - se asigna codigo a un evento.

```javascript 
    btn.onclick = ()=>{} //No recomendable

    /*Se tiene que usar la vieja sintaxis y no puede recibe parametros*/
    btn.addEventListener('click', func)
    function func(){}
    btn.removeEventListener('click', func) //Remueve evento xd
```

3. objeto event - Almacena el tipo de evento, este contine la propiedad *target* que almacena el elemento desencadenante del evento, y ademas contiene todas las caracteristicas de un evento


```javascript 
    //objeto event
    btn.addEventListener('click', (e)=>{
        console.log(e, e.target)
    })
```

4. Flujo de eventos o "event flow" -  el orden de ejecucion de los eventos, que son dos: Event Bubbling y Event Capturing.
5. Event Bubbling vs Event Capturing - el primero es el por defecto(cascada - del mas especifico al menos(FIFO o algo asi)) y el segundo es lo contrario.

```javascript 
    //Event Capturing
    btn.addEventListener('click', (e)=>{
        console.log(e, e.target)
    }, true) 
```

    Al usar true en un elemnto, se ejecuta primero en el mismo y luego los demas elementos anidados, esto con un efecto en cascada.

6. event.stopPropagation() -  Esto detiene el efecto cascada(punto anterior:4:5) de eventos asociados.

```javascript 
    container.addEventListener('click', (e)=>{
        alert("container")
    })
    btn.addEventListener('click', (e)=>{
        alert('btn')
        e.stopPropagation()
    }) 
```

    'on' se usa para referisre a un evento, pero no el evento en si. onclick = click

### Mouse

* click - ocurre con un click (apretar y soltar)
* dblclick - ocurre con un doble click
* mouseover - el puntero se mueve sobre un elemento o sus hijos (cuando entra de un elemento)
* mouseout - el puntero esta fuera de un elemento (cuando sale de un elemento)
<br>


* contextmenu - al abrir el menu contextual
* mouseleave - fuera de un elemento (cuando sale no para de ejecutase)
* mousedown - click sobre un elemento (solo apretar)
* mouseup - se suelta el puntero del mouse sobre un elemento
* mousermove - mientras el puntero se mueve sobre un elemento
* mouseenter - mientras el puntero se mueve sobre un elemento(especial para internerExplorer)

```javascript 
    container.addEventListener('mousedown', (e)=>{
        console.log('avento del mouse')
    })
```

### Teclado

Estos eventos se ejecutan con el puntero dentro de un elemento, ademas ocurren en el siguente orden:

* keydown - cuando una tecla se deja de precionar (precionar)
* keypress - cuando usa tecla se preciona (precionar y soltar)
* keyup - despues de de que los dos eventos anteriores de finalizen consecutivamente (soltar) {e.key}

### Interfaz

* error -  cuando ocucrrre un error al cargar un archivo multimedia (eje: una imagen no se carga)
* load - cuando un objeto se a cargado ()
* beforeunload - antes de que el documento esta a punto de descargarse (antes de irse a otro sitio)
* unload - cuando se descargo una pagina (al irte del sitio)
* rerize - cuando se cambia el tamaño de la vista del documento (al cambiar la resolucion)
* sroll - cuando de hace scrolll
* select - despues de que el usuario selecciona algun texto de *input* o *textarea* - al seleccionar texto 

```javascript 
    addEventListener('load', (e)=>{
        console.log('se cargo el sitio')
    })
    addEventListener('load', (e)=>{
        console.log('se cargo el sitio')
    })
```
```javascript
    const input = document.querySelector(".input")
    input.addEventlistener('select', (e)=>{
        let start = e.target.selectionStart
        let end = e.target.selectionEnd
        let allText = input.value

        container.innerHTML = allText.substring(start, end)
    }

```

### Temporizadores

Trabajos con tiempos y consumen muchos recursos.

* setTimeout() - recive una funcion y su retraso de ejecucion
* cleartimeout() - recive el nombre de la funcion a finaliza (elimina un temporizador)
* setInterval() - recibe una funcion y su intervalo de ejecucion
* claerInterval() - elimina el setInterval() (funciona igual que el anterior)

```javascript
    setTimeout(func, 2000) //func es una funcion
    const time = setInterval(()=>{
        document.write('hello')
    }, 2000)
    setTimeout(()=>{
        console.log('end interval')
        clearInterval(time)
    }, 5000)
```

## Control de flujo y manejo de errores (doc10)

1. Sentencias bloque : {}
2. Sentencias de control del flujo: condicionales y otros
3. Sentencias de manejo de expciones: Controlan los erroes e inprevistos
<br>

1. expciones ECMASript - relacionadas con javascript como tegnologia
2. DOMExption DOMError 

### try... catch

Los errores manejados por *catch* son objetos, ademas no se manejan errores de sintaxis sino errores logicos o relacionados con ECMAScript. **Estas sentecias se usan en casos especificos no son para depurar el codigo**, se deberia usar en aplicaciones grandes donde la probabilidad de un error es pequeña pero si sucediera cagaria **todo**.

    No abusar en casos vulgares, si usar an async...await.

```javascript
    try{

    }catch(err){
        console.log(typeof err) //Muestra la definicion de un dato
        console.log()
    }
```
```javascript
    try{}catch(err){ 
        //catch incondicional
       console.log()
    }
```
```javascript
    try{}catch(err){ 
        //catch condicional - contiene una condicion
       if(...){
           console.log()
       }
    }
```

Las sentencias dentro de *finally* se ejecutas a toda costa, aya o no errores,  se retornen o no valores. ademas *thonw* ejecuta o simula un error intencional, esto funciona sin o con el *try{...}catch{...}*.

```javascript
    try{
        throw "error uwu"
        throw {
            error: "uwu",
            info: "info"
        }
        throw ["uwu"]
    }
    catch(e){
        console.log(e.info)
    }
    finally{
        console.log("wiii")
    }
```

## Buenas practicas (doc11)

### identificar

* Deprecated - Obsoleto 
* contiene Bugs y fallos: en algunos casos, tambien puede consumir demaciados recursos o tener augeros de seguridad.
* Hay mejores formas de hacerlo

### Efectos negativos

* innecesariamente largo
* Afecta al SEO - mal rendimiento y conportamiento

### Verificacion

* verificar librerias
    * verificar si tienen o usan funcines, metodologias obsoletas
* Verificar em sitios basados en estandares oficiales
    * Verificar los estandares si hay dudas
<br>

* Detectar navegadores obsoletos

## Callbacks (doc12)

Es una funcion que ejecuta a una funcion.

```javascript
    function functionCallback(callback){
        callback('jack')
    }
    printName(name => console.log(name))
    // Es sin parentesis porque pasamos la funcion y no su implementacion o retorno
    functionCallback(printName) 
```

    Las promesas solucionan las desventajas de los callbacks.

### Promesas

Las promesas son un objetos, que contiene dos callbacks, estos a su vez reprecentas dos cosas distintas.

* resolve: La terminacion de una reprecentacion asincrona
* reject: Fracaso de una operacion asincrona
* then: nos permite acceder al valor retornado por resolve, ademas el mismo solo es util cuado resolve se ejecuta (es verdadero) 

    Estas luego fueron remplazadas por *async...await*. <br>
    Estas son asincronas.

```javascript
    let name = 'jack' 
    const promise = new Promise((resolve, reject) => {
        if(name !== "jack") reject('no es jack')
        else resolve(name)
    })

    console.log(promise)
```
```javascript
    //Nos deja acceder al valor de resolve
    promise.then(result => console.log(result)).catch(e) {
        console.log(e)
    }
```

### Async Await

En el siguiente ejemplo usar *async..await* es inecesario, y es mejor usar una promesa, pero esta bien para ver como se implementa.

```javascript
    const ob = {
        name1: 'jack',
        name1: 'jack torrance'
    }
```

Promesa
```javascript
    const getInfo = ()=>{
        return new Promise((resolve, reject) =>{
            setTimeout(()=> {resolve(ob)}, 3000)
        })
    }

    const viewResult = async () =>{
        let result = await getInfo()
        console.log(result)
    }
    viewResult()
```

En este ejemplo, al hacer muchas implementaciones de la funcion *getInfo* y querer obtener todos los datos de manera ordenada si hay que usar *async...await*.
<br>

    Esto porque *await* retiene el flujo y espera asta obtener el o los datos.

```javascript
    const getName = (text) =>{
        return new Promise((resolve, reject) =>{
            setTimeout(()=> {resolve(text)}, Math.random()*200)
        })
    }

    const viewResult = async () =>{
        let name1 = await getName("1: jack")
        let name2 = await getName("2: torrance")
        let name3 = await getName("3: tomy")

        console.log(name1, nama2, name2)
    }
    viewResult()
```

## Peticiones HTTP (doc13)

* Definicion: Es pedir *algo*(informacion, datos, fotos, etc) a un servidor, esto mediante el protocolo *http*. Ahora lo que quedaria es usar la informacion que nor retorna el servidor, osae ese *algo*.
* Cliente & Servidor: Luego del punto anterior nos quedan dos partes, el Cliente(nuestra pagina o aplicacion) y El servidor(los datos).
* No se guarda informacion: las peticiones HTTP npo guardan informacion, solo son el medio(protocolo) para obtener la informacion.

### Datos estructurados JSON

* Definion: La diferencia entre JSON y los arrays asociativos(objetos), esque JSON utiliza comillas en sus claves. Pero en esencia es los mismo, *clave* y *valor* dentro de un conjunto.

    JSON utiliza comillas porque es usado en peticiones a un servidor, y el no ser un String podria traer muchos problemas.

```javascript
    let object = {
        "name": 'jack',
        "age": 31
    }
```

* Serializacion y Deserializacion: 
    * **Aclaracion**: El formateo del codigo no influye en el resultado.
    * JSON serializado: Es una cadena de texto con formato JSON.
    ```javascript
        let object = '{"name": "jack", "age": 31}'
    ```
    Serializar un *JSON normal* - Pasarlo a String
    ```javascript
        const object = {"name": 'jack', "age": 31} 
        const newObject = JSON.stringify(object)  //Serializado
    ```
    
    * JSON deserializado: es un JSON normal, no es us String.
    ```javascript
        let object = {"name": 'jack', "age": 31}
    ```
    Deserializar un *String en formato JSON* - Pasarlo a JSON normal
    ```javascript
        const object = '{"name": "jack", "age": 31}'
        const newObject = JSON.parse(object) //Deserializado
    ```
<br>

    Basicamente **nosotros** trabajamos el JSON y **el servidor** trabaja el String

* JSON polyfill: Son funciones que simulan JSON, esto porque las versiones antiguas de interner explorer no soportan JSON. Esto casi nunca se usa.

### AJAX

Se necesita tener un servidor por cuestiones de seguridad(puden usar XAMPP). Ahora lo que nos permite AJAX es que nuestra aplicacion no tenga que actualisarse(refrescarse) y con esto nos olvidamos de lo *sincrono* y pasamos a lo *asincrono*

* Objeto XMLHttpRequest : Es un objeto para enviar peticiones(get, post, etc).

```javascript
        const request = new XMLHttpRequest()
```

* Peticiones GET: Pedir *algo*.

```javascript
        const request = new XMLHttpRequest()
        request.open('GET', 'info.txt') //Tipo de peticion y Direccion(URL)
        request.send() //Carga la URL
```

Este codigo nos retorna algo cuando el codigo de respuesta es **3** o **4** y el status es **200**

Forma usada por los dinosaurios

Estados(readystate)

1. La solicitud se creo correctamente.
2. La solicitud se envio correctamente.
3. La peticion se esta procesando. 
4. Se nos dio una respuesta y el resultado ya se puede usar.


```javascript
        const request = new XMLHttpRequest()

        request.addEventListener('readystatechange', ()=>{
            console.log(request.readysate)
            console.log(request.response)
        })

        request.open('GET', 'info.txt') 
        request.send()
```

Forma usada por los humanos (en realidad no ya que se suele usar *fetch*)

```javascript
        request.addEventListener('load', ()=>{
            let response = ''
            if(request.status == 200){
                console.log(request.response)
                response = JSON.parse(request.response)
            } else response = 'El recurso no fue encontrado con exito'
            console.log(response)
        })
```

Estados(status): <br>
Existen varios tipos de estado y es por eso que el codigo de arriba no es valido en aplicacion profecional, pero para un ejemplo esta bien :)

200. La direccion es valida. 
201. Creacion de un recurso con exito.
404. La direccion o archivo no fue encontrada o es invalida.

* Objeto ActiveXObject : es la anternativa a AJAX para los navegadores que no lo soportan.

```javascript
    let request
    if(window.XMLHttpRequest()) request = new XMLHttpRequest()
    else request = new ActiveXObject("Microsoft.XMLHTTP")
```

### peticiones POST

Este tipo de peticiones no son visibles en la URL, ya son enviadas a travez del metodo POST, ques preferible para en envio de datos sencibles(pero como siempre no hay 100% seguridad) y disparar de acciones.

```javascript
        const request = new XMLHttpRequest()

        request.addEventListener('load', ()=>{
            let response = ''
            if(request.status == 200 || request.status == 201){
                console.log(request.response)
            } else response = 'El recurso no fue encontrado con exito'
            console.log(JSON.parse(response))
        })

        request.open('POST', 'https://reqres.in/api/users')
        
        /* "Encabezado", "valor" */
        request.sendRequestHeadet('Content-type', ' application/json;charset=UTF8')

        request.send(JSON.stringify({
            "nombre": "morfeo",
            "trabajo": "lider"
        }))
```

Consultar la pagina usada en el ejemplo: [reqres](https://reqres.in/)

### Fetch

Esto es una forma de trabar con el concepto de AJAX, y puede ser usado al tener pocas peticiones y respuestas un tanto especificas.

* Este esta basado en promesas, por ende siempre retorna una encapsulada. para poder trabajar con el contenido de esta promesa se usan los distintos metodos de *fetch*

    Usa GET por defecto.

```javascript 
    const peticion = fetch('https://reqres.in/api/unknown/2') //Retorna la promesa
    peticion.then(res => console.log(res))
```

Tipos de retorno:

```javascript 
    peticion
        .then(res => res.text())
        .then(res => console.log(res))
```
```javascript 
    peticion
        .then(res => res.json())
        .then(res => console.log(res))
```

blob crea una URL temporal para almacenar la imagen(o algun otro archivo)

```html 
    <img class="img">
```
```javascript 
    const img = document.querySelector('.img')

    fetch('vacio-informatico.png')
        .then(res => res.blob())
        .then(imgRes => {
            img.src = URL.createObjectURL(imgRes)
        })
```
#### Peticion POST

Las cabeceras(headers) y sus valores dependen del tipo de informacion que se envie.

```javascript 
    const peticion = fetch('https://reqres.in/api/users', {
        method: "POST",
        body: JSON.stringify({
            "nombre": "jack",
            "appellido": "torrance"
        }),
        headers: {
            "Content.type" : "application/json"
        }
    })
    
    peticion
        .then(res => res.json())
        .then(res => console.log(res))
```

Otra forma de hacer la peticion es esta:

```javascript 
    const headers  = {
        method: "POST",
        body: JSON.stringify({
            "nombre": "jack",
            "appellido": "torrance"
        }),
        headers: {
            "Content.type" : "application/json"
        }
    }

    const peticion = fetch('https://reqres.in/api/users', headers)
```

### Libreria Axios

Este es el remplazo de fetch, pero no es nativo. Ademas cuando queremos trabjar en un sitio web especial para enviar, recibir peticiones axios es la mejor opcion. 

* Instalacion: [axion github](https://github.com/axios/axios) - Using jsDeliver CDN, y sino lo puden instalar usando NPM y requerirlo con nodeJs. Pero sino peguen la CDN **arriba del script donde trabajen**.

* Casi al igual que *fetch*, axios esta basado en promesas.

* Implementacion:

    * Implentacion con *fetch*.
    ```javascript
        fetch('https://reqres.in/api/unknown/2')
            .then(res => res.json())
            .then(res => console.log(res))
    ```

    * Implentacion con *axios*.
    ```javascript
        axios('https://reqres.in/api/unknown/2')
            .then(res => console.log(res.data))
    ```

    Como pueden apreciar, con axios no es necesario converir la respuesta a JSON.

* Trabaja con GET por defecto, ademas automaticamente define los *headers*.

* Definir un tipo de metodo:
```javascript
        axios.get('https://reqres.in/api/unknown/2')
            .then(res => console.log(res.data))
```
```javascript
        const data = {
            "name": "jack",
            "lastName": "torrance"
        }

        axios.post('https://reqres.in/api/users', data)
            .then(res => console.log(res.data))
```

Otra forma es esta: 
```javascript
        const data = {
            method: 'POST',
            data: {"name": "jack"}
        }
        
        axios('https://reqres.in/api/users', data)
            .then(res => console.log(res.data))
```

### Fetch y Axios con async..await

Con *fetch*:
```javascript
    const getName = async () =>{
        const request = await fetch('https://reqres.in/api/unknown/2') 
        const response = await request.json()
        return response
    }
    getName() //Esto puede tener varios usos
```
Con *axios*:
```javascript
    const getName = async () =>{
        const request = await axios('https://reqres.in/api/unknown/2')
        return request.data
    }
    getName()
```