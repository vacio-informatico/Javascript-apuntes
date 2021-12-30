v a c i o - i n f o r m a t i c o 
===
*Escrito por: Mirkin Ciro* <br>
*Pertenece a: vacio-informatico* | [Github](https://github.com/vacio-informatico/)<br>
[vacio-informatico web site](https://vacio-informatico.github.io/v-i/)

# Javascript

Apunte sobre el nivel superior de javascript. Que incluye principalmente el uso de APIs, como la geolocalizacion, drag and drop, etc.

## Prototipos (doc14)

Es un tipo de programacion donde los objetos de crean a travez de la heredacion de los prototipos.

* Prototype chain: 

    ```javascript 
        let object = {
            "propiedad": "dato" 
        }
        let string = "Jack"
        console.log(object.__proto__)
        console.log(string.__proto__)
    ```

    * __ proto __ : Dentro de __ proto __ esta el prototipo. (Accedemos al prototipo que heredamos).

    > Todos los objetos heredan minimo dos prototipos, el que contiene su tipo y el prototipo *Object*, el cual esta precente en todos los objetos.

    Nuestas variables heredan el prototipo de su tipo(Number, String, etc) y este a su vez hereda el prototipo Object.

    Al crear una funcion estamos creando un prototipo, y para acceder a este prototipo usamos la propiedad *prototype*.

    ```javascript 
        let variable = function(){}
        console.log(variable.prototype) //Prototipo que creamos
        
        console.log(variable.prototype.__proto__) //Prototipo Object
    ```

### Caracteristicas

* Un prototipo definido en su condigo fuente es mutable: Si creamos un portotipo es mutable(modificable).
* Son objetos, e incluso pueden contener funciones.
* Tienen existencia fisica en memoria.
* Puede ser modificado y llamado.
* Un objeto hereda las propiedades (valore y metodos) de su prototipo.

* Propiedad proto (dunder proto):
    Los metodos como *map()*, *reverse()* o *pop()* estan dentro de la propiedad que heredadan, que vendria a ser el prototipo Array(entre otros).

    ```javascript 
        class Objeto{
            constructor(){}

            say(){
                console.log("hello")
            }
        }

        const onjeto = new Objeto() //Creando prototipo
    ```

    Al crear una clase y un nuevo prototipo sus metodos se almacenan dentro del objeto dunder proto, ademas no se crea un nuevo prototipo sino hereda Object y lo redefine.

* Sobre sescribir proto y un metodo:
    ```javascript 
        class Objeto{
            constructor(){}

            say(){
                console.log("hello")
            }
        }

    ```
    Agrengando una funcion al objeto, no se esta modificando el metodo ya que este esta dentro de la propiedad.

    ```javascript 
        const objeto = new Objeto() //Creando prototipo
        Objeto.say = ()=>{
            console.log("afuera")
        }
    ```

    Modificando el metodo

    ```javascript 
        const objeto = new Objeto() //Creando prototipo
        objeto.__proto__.say = ()=>{
            console.log("afuera")
        }
    ```

    Heredando prototipo desde afuera hacia adentro

    ```javascript 
        const objeto = new Objeto() //Creando prototipo
        objeto.__proto__.say = ()=>{
            console.log("Adentro")
        }

        let arr = []
        arr.__proto__ = objeto //Hereda el prototipo
    ```
    ```javascript 
        const objeto = new Objeto() //Creando prototipo
        objeto.say = ()=>{
            console.log("afuera")
        }

        let arr = []
        arr.__proto__ = objeto //Hereda el prototipo
    ```

    Heredaria primero lo modificado afuera y luego lo declarado dentro.
    ```javascript  
        console.log(arr.__proto__.say)//Modificado afuera
        console.log(arr.__proto__.__proto__.say) //Declarado
    ```

    Esto se puede evitar asi:

    ```javascript  
        let arr = []
        arr.__proto__ = objeto.__proto__; //Hereda el prototipo
    ```
    O asi:
    ```javascript 
        const objeto = new Objeto() 
        objeto.__proto__.say = ()=>{ //Aqui
            console.log("Adentro")
        }

        let arr = []
        arr.__proto__ = objeto;
    ```

### Modo estricto ("use strict")

* Convierte errores en excepciones: <br>
    * Esto daria error si se usa el modo estricto.
    ```javascript  
        name = "Jack" 
    ```
* Mejora la optimizacion de los errores y mejores tiempo de ejecucion.
* No permite las malas sintaxis.

* Se puede usar en funciones y en el bloque global.
    ```javascript  
        "use strict"; 
    ```
    ```javascript  
        function something(){
            "use strict"; 
        }
    ```
* defineProperty() y Writeable
    ```javascript  
        const obj = {}
        Object.defineProperty(obj, 'name', {value: 'Jack', writeable: false}) //Evita la sobre escitura

        obj.nombre = 'Paul'
    ```
    Sin modo estricto no da error, con el modo activado si lo da.
* preventExtensions : Proive la Extencion de propiedades, (adicion)
    ```javascript  
         const obj = { name : 'jack'}
         Object.preventExtensions(obj)

         obj.lastName = 'torrance';
    ```
    Sin modo estricto solo da *undefined*, pero con el activado da error.
* Evita añadir propiedades a un String, Da error al intentarlo.
* Da errror
    ```javascript  
        function getName(name, name){}
    ```
    ```javascript  
        let name = 'Jack';
        delete name;
        //Se usa para eliminar propiedades no variables o funciones.
    ```
    ```javascript  
        let package = 'Wii'
        //Es una palabra reservada
    ```
    ```javascript  
        function say(){
            this.name = 'Jack'
            this.hello = function(){
                console.log('hello ', this.name)
            }
        }
        obj = new say()
        //Se elimina el this y por eso da error.
        //Esto porque this da undefined
    ```
    ```javascript  
        arguments = 'Hello';
        eval = 'hello';

        console.log(arguments, eval)
        //No pueden ser variables
    ```
    * Los numeros octales van con una o adelante del 0.
    ```javascript  
        console.log(0o21)
    ```
    * No existe *With*.

### Funciones flechas

* Son funciones conpactas

    ```javascript  
        const say = ()=>{}
    ```

* Si solo tiene una exprecion la retorna, no funciona con centecias.
* No son adecuadas para ser usadas como metodos y no pueden ser usadas como constructores.
    ```javascript  
        const objeto = {
            name: "Jack",
            say: () => {console.log(this.name) }
            //No se puede usar this porque no hace referencia al objeto que llama a la funcion.
        }
    ```

    Como constructor:
    ```javascript  
        const person = (name)=>{
            this.name = name
        }
        const person1 = new person('jack')

        //Todo los anterior no funciona
    ```

* *This*:
    * Afuera de una funcion *this* hace referencia a *window*, pero dentro de un objeto con un metodo(funcion normal no una func sflecha) hace referencia a una propiedad de ese objeto.
    ```javascript  
        function say(){
            console.log('Hi ', this.name)
        }
        const obj = {
            name: 'Jack',
            say: say
        }
        obj.say()
    ```
    Si una propiedad tiene el mismo nombre que su valor se puede resumir asi:
    ```javascript
        let name = 'jack'
        const obj = {
            name, say
        }
    ```    

    Es lo mismo que esto:
    ```javascript  
        const obj = {
            name: 'Jack',
            say: console.log('Hi ', this.name);
        }
        obj.say()
    ```
### Caracteristicas de las funciones

* Recursividad: Es del diablo...Generalmente...
* Clausuras o cierrres: Son funciones que retornan a otra funcion.
    ```javascript
        const say = (name) =>{
            return () =>{
                console.log(name)
            }
        }
        let sayName = say('Jack')
        sayName()
    ```
* Parametros por defecto:
    ```javascript
        const x = (a, b)=>{
            b = b || 0
        }    
    ```
    ```javascript
        const x =(a = 0, b = 0){}
    ```
* Parametro *rest*: Introduce todos los parametros decibidos en un array. Este parametro siempre tiene que ser el ultimo parametro.
    ```javascript
        const x = (...res)=>{
            console.info(res)
        }
    ```
* Destructuracion: 

### Mas operadores
* Operador ternario: 
    ```javascript
        num1 = (age >= num1) ? console.log("...") //If
            : console.log('...') //Else
    ```
    ```javascript
        num1 = (age >= num1) ? (
            console.log("..."), 
            console.info('other...')
            ) : console.log('...') //Else
    
* Operador Spread: Convierte un array en valores separados, Es usado como parametro *rest*, se puede usar para concatenar arrays.
    ```javascript 
        console.log(...tuArray)
        arr.push(...tuArray) //Concatenacion
        let res = [...arr2, ...array2] //Concatenar en otra variable
    ```
    ```javascript
        const sum =(...num){}
        sum([1, 4])

        //Al revez
        const sum =(n1, n2){}
        let arr = [1, 4]
        sum(...arr)
    ```
## APIs (doc15)

Interfaces = funcionalidades de los lenguajes. <br>
Procesa un parametro y nos retorna 'algo', pero no sabemos como procesa ese parametro.

### Objeto date

Date es un constructor.

* getDate() - Numero del dia
* getDay() - El numero del dia de la semana (Domingo = 0 - sabado = 6)
* getMonth() - El numero del mes (enero = 0)
* getYear() - El año actual menos 1900 (2020-1900 = 120)
<br>

* getHours() 
* getMinutes() 
* getSeconds() 

```javascript
    const fecha = new Date(); //Contiene metodos
    fecha.getDate()
```
```javascript
    const fecha = new Date(2020, 11, 15);
```

### LocalStorage y SeesonsStorage

* Diferencias: En una al cerrar la pagina se pierde la informacion(SessionStorage) y en la otra no(LocalStorage).

* setItem() - Ingresa un objeto (Los define)
* getItem() - Obtiene los valores
* removeItem() 
* clear() - Limpia todo (borra)

```javascript
    const name = localStorage.setItem('name': 'Jack')
    seesionStorage.setItem('name': 'Jack')
```
### Drag & Drops

* Eventos del objeto:
    * dradstart: Agarrar
    * drag: Mover
    * dragend: Soltar
    ```javascript
        addEventListenet('dragstart', ()=>{})
    ```
* Eventos de la zona:
    * dragenter: Entra en la zona.
    * dragover: Se mueve dentro de la zona, y por defecto no nos permite soltarlo en ningun lado.
    * drop: Cuando se suelta.
    * dragleave: Cuando sale de la zona.
     ```javascript
        addEventListenet('dragover', (e) =>{
            e.preventDefault(); //Nos permite soltar
        })
        addEventListenet('drop', ()=>{}) //Suelta
    ```
* Propiedads dataTransfer: Este es el objeto transmite informacion entre las dos partes, el objeto y la zona.
    * setData() - Crea
    * getData() - 
    ```javascript
        addEventListenet('dragstart', (e) =>{
            e.dataTransfer.setData('text-plain', e.target.className); 
        })
        addEventListenet('drop', (e) =>{
            e.dataTransfer.getData('text-plain')
        })
    ```
### Geolocalization

Se usa accediendo al objeto *navigator*

* getCurrentPosition() - Obtiene los datos de la posision actual
    * parametros (position, error, options)
    * Propiedades de option:
        * maximumAge - Como la informacion se almacena en cache, Este parametro establece el tiempo de refresco, 0 = la actual(no se guarda).
        * timeout - El tiempo de retrazo en la peticion.
        * enableHightAccuracy - Establece la alta presicion, (si es un booleano).
* watchPosition() - Ante cambios de la localizacion, vigila la posicion ante cambio.
* clearWatch - Elimina al *watchPosition*.

Posicion:
```javascript
    const position = (pos) =>{
        console.log(pos)
        console.log(pos.coords.latitude, pos.coords.longitude)
    }

        const geo = navigator.geolocation
        geo.getCurrentPosition(position)
```

Errores:
```javascript
    const position = (pos) =>{}

    const err = (e) => console.log(e)
    
    const geo = navigator.geolocation
    geo.getCurrentPosition(position, err)
```

Opciones:
```javascript
    const position = (pos) =>{}
    const err = (e) => {}

    const options = {
        maxiumAge: 0,
        timeout: 2000,
        enableHightAccuracy: true
    }
    
    const geo = navigator.geolocation
    geo.getCurrentPosition(position, err)
```

### History 

Este objeto tambien deciende de window.

Metodos:

* back() - Lo mismo que el boton para volver hacia atras.
* forward() - Lo contrario al anterior.
* go() - Recarga la pagina.
    * go(1) - va a la primera pagina.
    * go(-1) - va a la segunda, anterior.
* pushState({data}, 'title', '?url') - Crea una nueva entrada en el historia.
* replaceState() - Recive los mismo parametros que *pushState()*, pero este no deja entradas en el historial.

* Propiedades state y eventos popstate:
    * state: Retorna el objeto que que pasamos a *pushState()*, cuando estamos en la entrada de historial que se creo.
    * popstate: El evento *popstate* percibe lo mismo que *history.state*.

```javascript
    history.go()
    addEventListener('popstate', (e) => console.log(e.state))
```

### FileReader

Este objeto esta creado para trabajar con los eventos que contiene(se aborta lectura, error en  lectura, etc), no cuando se lo llama.

Pero en la basa, esto trabaja con archivos. La diferencia que tiene con *fetch* es que este ultimo trabaja con archivos es un servidor ya definidos, y *FileReader()* trabaja datos ingresador por el usuario.

`ojavascript
    const reader = new FileReader()
```
* ReadAsText() - Retorna el archivo.
* ReadAsDataURL() - Retorna una URL valida.

```html
    <input type="file" id="file" />
```

```javascript
    const file = document.getElementById('file')
    file.addEventListener('change', (e)=> {
        readFile(file.files[0])
    })
    
    const readFile = (f) =>{
        const reader = new FileReader()
        reader.ReadAsText(f)
        reader.addEventListener('load', (e)=> {
            console.log(e.currentTarget.result) 
        })
    }
```

> El evento *change* funciona cunado un formulario cambia de valor.

Multiples archivos:
```html
    <input type="file" id="file" multiple="" />
```
```javascript
    const file = document.getElementById('file')
    file.addEventListener('change', (e)=> {
        readFile(file.files)
    })
    
    const readFile = (file) =>{
        const reader = new FileReader()
        for(file of f){
            reader.ReadAsURL(f)
            reader.addEventListener('load', (e)=> {
                console.log(e.currentTarget.result) 
            })
        }
    }
```

Imagenes:
```javascript
    const file = document.getElementById('file')
    file.addEventListener('change', (e)=> {
        readFile(file.files)
    })
    
    const readFile = (file) =>{
        const reader = new FileReader()
        for(file of f){
            reader.ReadAsText(f)
            reader.addEventListener('load', (e)=> {
                img.innerHTML = `<img src="${e.currentTarget.result}`
            })
        }
    }
```

### IndexedDB

Es una base de datos indexada, mongoDB trabaja con esto, es NoSQL.

Caracteristicas

* Amacen ainformacion en el navegador de forma similar a LocalStorage.
* Es orientada a objetos.
* Es asincrono
* Trabaja con eventos del DOM 

Procedimientos de creacion.

* Objeto IndexedDB: Ejecuta la solicitud de .
    ```javascript
        const IDBRequest = indexedDB.open('user', 1)
    ```
* metodo open() - Crea o abre una base de datos, tambien recive la vercion.

Eventos:

* upgradeneeded - Verifica si la DB hay que crearla.
    ```javascript
        IDBRequest.addEventListener('upgradeneeded', () =>{
            const db = IDBRequest.result //Resultado
        })
    ```
* sucess - Todo salio correctamente.
* error - Erro al abrir la DB 

Almacen de objetos:

* Metodo createObjectStore() - 
    ```javascript
        IDBRequest.addEventListener('upgradeneeded', () =>{
            const db = IDBRequest.result
            db.createObjectStore('names', {
                autoIncrement: true
            })
        })
    ```
* autoIncrement y KeyPath :
    * autoIncrement: Es como un ID, que en cada registro se incrementa.
    * KeyPath: Asocia los nombres, y recibe el ID, creo.

Almacenar objetos:

```javascript
        const addObject = (obj) =>{
            const db = IDBRequest.result
            //Define una accion
            const IDBTransaction = db.transaction('names', 'readwrite') //readwrite - readonly
            //Inicia la accion
            const objectStore = IDBTransaction.objectStore('names')
            //Accion
            objectStore.add(obj)

            IDBTransaction.addEventListener('complete', () =>{
                console.log('Correctamente')
            })
        }
```

Estas sentencias se utilizan en todas las acciones, como añadir o eliminar.
```javascript
    const IDBTransaction = db.transaction('names', 'readwrite') //readwrite - readonly
    //Inicia la accion
    const objectStore = IDBTransaction.objectStore('names')
```

Obtener datos:
```javascript
        const readObject = (obj) =>{
            const db = IDBRequest.result
            //Define una accion
            const IDBTransaction = db.transaction('names', 'readonly') //readwrite - readonly
            //Inicia la accion
            const objectStore = IDBTransaction.objectStore('names')
            
            const cursor = objectStore.openCursor()
            cursor.addEventListener('successs', () =>{
                if(cursor.result){
                    console.log(cursor.result.value)
                    cursor.result.continue() 
                } else{
                    console.log('Fin')
                }
            })
        }
```

Editar datos: La funcion recive el dato a modificar.
```javascript
        const editObject = (key, obj) =>{
            const db = IDBRequest.result
            //Define una accion
            const IDBTransaction = db.transaction('names', 'readwrite') //readwrite - readonly
            //Inicia la accion
            const objectStore = IDBTransaction.objectStore('names')
            
            objectStore.put(obj, key)

            IDBTransaction.addEventListener('complete', () => console.log("Modificado")
        }
    editObject(2, {})
```
> Si el objeto existe lo agrega sino lo crea

Eliminar datos: 
```javascript
        const deleteObject = (key) =>{
            const db = IDBRequest.result
            //Define una accion
            const IDBTransaction = db.transaction('names', 'readwrite') //readwrite - readonly
            //Inicia la accion
            const objectStore = IDBTransaction.objectStore('names')
            
            objectStore.delete(key)

            IDBTransaction.addEventListener('complete', () => console.log("Eliminado"))
        }
    editObject(2, {})
```

Solucionar repeticiones:

```javascript
    /**
     * TransactionOperation
     * @param {String} name 
     * @param {String} mode Modo
     * @param {String} msg Mensaje a mostrar luego de realizar la opcion.
     * 
    */
    const getIDBData = (name, mode, msg) =>{
        const db = IDBRequest.result
        //Define una accion
        const IDBTransaction = db.transaction(name, mode)
        //Inicia la accion
        const objectStore = IDBTransaction.objectStore(name)
        
        IDBTransaction.addEventListener('complete', () => console.log(msg))

        return objectStore
    }
```

```javascript
    const editObject = (key, obj) =>{
        const IDBData = getIDBData('names', 'readwrite', "Modificado correctamente")

        IDBData.put(obj, key)
    }
```
## (doc16)
### Match Media

Nos permite trabajar con responsive desing, es recomendable usarla para cosas que no se puedan hacer con estilos CSS.

* matchMedia()
* Propiedades match(matches) - Retorna un *True* si se esta dentro del rango.
* Evento onchange: Se ejecuta cada vez que cambia la resolucion.
* Se puede usar en el remplazo de clases y logica vinculada a ese cambio de clases.

```javascript
    const mq = matchMedia('(max-width: 500px)') 
    if(mq.matches){
        //Esta dentro del rango
    }
```
```javascript
    mq.addEventListener('change', () =>{
        if(mq.matches){
        
        }
    })
```

### Intersection Observer

Se usa para verificar si *algo* esta en el viewport del navegador.

* IntersectionObserver()
* callbacks y options:
    * callback: La funcion que se ejecuta al ejecutarse el evento de interseccion.
    * options: Es un objeto con configuraciones extras. como margenes o alturas, etc.
* isIntersercting - Retorna *true* si el elemento esta dentro del viewport 
* Configuracion

```javascript
    const boxes = ducument.querySelectorAll('box')
    const verifuVisibility = (entries) =>{
        const entry = entries[0]
        if(entry.isIntersecting){

        }
    }
    const observer = new IntersectionObserver(verifuVisibility, options)
    observer.observe(box)
``` 

Observar a multiples elementos:   
```javascript
    const box = ducument.getElementById('box')
    const verifuVisibility = (entries) =>{
        for(const entry of entries){
            if(entry.isIntersecting) console.log('box: ', entry.target.textContent)
        }
    }
    const observer = new IntersectionObserver(verifuVisibility, options)
    for(const box of boxes){
        observer.observe(box)
    }
```

### Visibility Change

Este evento nos permite saber cuando se cambia de pestaña

* Evento visibilitychange
    ```javascript
        addEventListener('visibilitychange', () =>{
            console.log(e.target.vidibilityState)
        })
    ```
    ```javascript
        addEventListener('visibilitychange', () =>{
            if(e.target.vidibilityState == "visible"){
                console.log('Che poke te fuiste?')
            } else {
                 alert('Ha, volviste')
            }
        })
    ```
* Usos: 
    * Pausar cosas, entre otras.

### Notifications

Nos permite enviar notificaciones.

```javascript
    if(!('Notification' in window)){
        console.log('No esta disponible')
    }
```

* Notification.requestPermission() - Notifica o pregunta a usuario si quiere recibir notificaciones.
* Notification.permission - Nos permite saber si se aceptaron las notificaciones.
    ```javascript
        if(!('Notification' in window)){
        }
        Notification.requestPermission(() =>{
            if(Notification.permission == "granted"){
                console.log('Se permitieron')
                new Notifications()
            }
        }) 
    ```
* Notification(smg, options) - Al enviar una notificacion solo hay que declarar la instancia no hay que asignarla.
    ```javascript
        Notification.requestPermission(() =>{
            if(Notification.permission == "granted"){
                new Notification("@vacio_informatico en Instagram...")
            }
        }) 
    ```
### Web Workers

Nos permite paralelizar eventos o procesos, osea llamar a archivos para que esten en segundo plano.

> Esto obiamente tiene sus limitaciones.

* Tipos
    * Dediated worker - Worker() -> es un constructor. 
        ```javascript
            const worker = new Worker('worker.js')
        ```
    * Shared worker. (Estan mas adelante. o eso pense, pero en realidad no)
    * Service worker. (este si)
    * Abstract worker.
* parametros
* metodo postMessage() - Envia mensajes del *worker* al *Script*. <br>
    Script Principal
    ```javascript
        const worker = new Worker('worker.js')
        const bucle = () =>{
            worker.postMessage('Este es el mensaje que se envia')   
        }
        addEventListener('message', bucle)
    ```
    Script palalelos
    ```javascript
            const bucle = () =>{
                while(true) console.log('Wiiiii')
            }
            addEventListener('message', bucle)
    ```
* Evento onmessage
    ```javascript
        addEventListener('message', e =>{
            console.log(e.data) //Este es el mensaje que se envio desde el principal
            postMessage('Esta es la respuesta del secundario')
        })
    ```
    Principal:
    ```javascript
        worker.addEventListener('message', e =>{
            console.log(e.data)
            worker.terminate() //Finaliza los procesos(workers) 
        })
    ```

* Politica de origen cruzado (Same-Origin): No se pueden cargar *workers* de otros puertos.

### Navigator

Este objeto contiene toda las especificaciones del navegador. Busca esto en DuckDuckGo.

Interfaces:

* Navigator ID
* NavigatorLenguage
* NavigatorOnLine
* NavigatorContentUtils
* NavigatorStorageUtilis
* NavigatorCookies - Retorna si estan las *cookies* activadas.
* NavigatorConcurrentHardware
* NavigatorPlugins
* navigator.serviceWorker - Este tipo de *worker* comparten informacion.

Metodos:
* NavigatorUseMedia() - No recomendado.
* navigator.vibrate() - Para emitor vibraciones.
* Hay mas...Busca en DuckDuckGo o algun otro buscador.

### Memorization

Permite acortar tiempos de ejecucion. Casi nadie lo usa.

    Se usa cuando se sabe que ciertos datos pueden llegar a repetirce, se usa en esos datos para ahorrar tiempo.

```javascript
    let cache = [] //En realidad no es cache cache pero podria ser

    const memoizer = func =>{
        return e =>{
            const index = e.toString()
            if(cache[index] == undefined){
                cache[index] = func(e)
            }
            return cache[index]
        }
    }
```
```javascript
    const something = () =>{
        const a =12
        const b =20
        let x =0
        for(;;){
            for(;;) x += a*b
        }
        return x
    }

    const memo = memoizer(something)
``` 

### Cache

*Trabaja con promesas*

Inicializacion:
```javascript
    //Crea un nuevo objeto cache
    caches.open("Static-files").then(cache =>{

    })
```  

* Cache.add(*request*) - Toma una URL y la agrega al objeto cache. (Añade)
    ```javascript
        caches.open("Static-files").then(cache =>{
            cache.add("index.html") //Almacena si existe, el archivo.
        })
    ```  
* Cache.addAll(*request*) - Recive un array con los archivos. (Añade)
    ```javascript
        caches.open("Static-files").then(cache =>{
            cache.addAll(["", "", ""]) //Almacena si existe, el archivo.
        })
    ```  
* Cache.match(*request*, *options*) - Retorna un recurso (Optiene)
    ```javascript
        caches.open("Static-files").then(cache =>{
            cache.match("index.html").then((res) =>{
                console.log(res)
            })
        })
    ```  
* Cache.matchAll(*request*, *options*) - Lo mismo, pero retorna un arreglo con las respuestas.
* Cache.put(*request*, *response*) - (Edita)
    ```javascript
        caches.open("Static-files").then(cache =>{
            fetch("index.html").then( res =>{
                cache.put('index.html', res)
            })  
        })
    ``` 
    Equivale a esto(esto es mas corto :) 
    ```javascript
        caches.open("Static-files").then(cache =>{
            cache.add('index.html') 
        })
    ``` 
* Cache.delete(*request*, *options*) - (Elimina)
    ```javascript
        caches.open("Static-files").then(cache =>{
            cache.delete('index.html') 
        })
    ``` 
    ```javascript
        caches.open("Static-files").then(cache =>{
            cache.delete('index.html').then(()=> console.log("se elimino")) 
        })
    ``` 
    
* Cache.keys(*request*, *options*) - Retorna toda la data almacenada en cache, y toda su informacion (Optiene)
    ```javascript
        caches.open("Static-files").then(cache =>{
            //...
            cache.keys().then(res =>{
                console.log(res)
            }) 
        })
    ``` 

### Service Workers

Hace de intermediario entre un sitio web y el servidor, este es un archivo Javascript que intercepta las peticiones del sitio al servidor y hce de intermediario entre ambos.

Sirve para estar a la escucha de eventos.

    A nivel nativo no tiene mucha utilidad, al menos que se use con nodejs

* Intefaz:
    ```javascript
        if(navigator.serviceWorker){
            navigator.serviceWorker.register("sw.js")
        }
    ``` 
    **sw.js**
    ```javascript
        if(navigator.serviceWorker){
            navigator.serviceWorker.register("sw.js")
        }
    ``` 
* *self* hace referencia a el service worker. Es lo mismo que *this*. <br>
    **sw.js** <br>
    Este evento se ejecuta una sola vez.
    ```javascript
        self.addEventListener('install', (e) =>{
            console.log('Instalado')
        })
    ``` 
    ```javascript
        self.addEventListener('activate', () =>{
            console.log('activado')
        })
    ``` 
    ```javascript
        self.addEventListener('error', (e) =>{
            console.log('ocurrio un error ', e)
        })
    ``` 
    ```javascript
        self.addEventListener('fetch', () =>{
            console.log('Interceptando peticion')
        })
    ``` 
* postMessage() - Funciona igual que en un serviceWorker normal <br>
    *index*
    ```javascript
        navigator.serviceWorker.ready.then(res => res.active.postMessge('Hola'))

        navigator.serviceWorker.addEventLIstener('message' (e) =>{
            console.log('Recivi un SMS: ', e.data)
        })
    ``` 
    *serviceWorker*
    ```javascript
        self.addEventListener('message', (e) =>{
            console.log(e.data))
            e.source.postMessage('Hola, todo bien?')
        })
    ``` 

* Guardar archivos en cache para poder mostrarlos al estar *offline*
    ```javascript
        let pageState = 'offline'

        self.addEventListener('install', (e) =>{
            console.log('Instalado')
            cache.open(pageState).then(cache =>{
                cache.add('index.html').then(res =>{
                    console.log("Todo piola")
                }).catch( e => console.log(e))
            })
        })
    
        self.addEventListener('activate', () =>{
            cache.keys().then(key =>{
                return promise.all(
                    key.map(cache => if(cache !== version) {
                        return cache.delete(cache)
                    }))
                )
            })
        })
        self.addEventListener('fetch', (e) =>{
            e.respondWith(
                async function () =>{
                    const answerCache = await caches.match(e.request)
                    answerCache ? return answerCache : return e.request
                }
            )
        })
    ``` 
### Cookies

    Clave = valor ; atr ; atr ;atr

* Crear:
    ```javascript
        document.cookie = 'name = Jack'
    ```
* Obtenerlas:
    ```javascript
        document.cookie
    ```
* Atributos:
    * expires: Cuando va a expirar.
    * path: Donde se va a almacenar.
    * max-age: Timepo que dura en segundos.
    ```javascript
        document.cookie = "name=Jack; 
            expires = Mon 26 apr 2022 12:00:00 UTC; 
            path= /; age=6000 "
    ```

    Por defecto una Cookie expira en la sesion
    Tiene un limite de 4kilobyte

* Funcion de creacion:
    ```javascript
        const setCookie = (name, day) =>{
            let expires = new Date()
            expires.setTime(date.getTime() + day*1000*60*60*24)
            expires.toUTCString()

            document.cookie = `${name}; expires=${expires}`
        }
        setCookie("user=Jack", 3)
    ```
* Funcion de creacion:
    ```javascript
        const getCookie = (name) =>{
            let cookies = document.cookie
            cookies = cookies.split(";")

            for(let i=0; cookies.length > i; i++){
                cookie = cookies[i].trim()
                if(cookie.strartsWith(name)) return cookie.split('=')[1]
            }
            return false 
    ```

* Para editar una *cookie* solo hay que sobre escribirla.
* Eliminarla:
    ```javascript
        document.cookie = "name=0; max-age=0"
    ```

* Verificar el complimiento del RGPD y el ePrivacy antes de usar cookies.

* Modal para cookies:
    ```javascript
        if(geCoikie('acceptedCookies') !== "si"){
            document.getElementById('accept').addEventListener('click', ()=>{
                setCookie("acceptedCookies=si", 7)
            })
            document.getElementById('deny').addEventListener('click', ()=>{
                setCookie("acceptedCookies=nou", 7)
            })
        }
    ```
### Screen

* screen.width - Ancho total de la pantalla 
* screen.height

* screen.availWidth - Ancho disponible de la pantalla 
* screen.availHigth

* screen.pixelDepth - Resolucion de color de la pantall
* screen.colorDepth - Profundid de bits de la paleta de colores

### Canvas

Crear graficos, no son vectoriales. sirve para graficas, aunque depende.

```html
    <canvas id='canvas'></canvas>
```
```javascript
    const canvas = document.getElementById('canvas')
    const ctx = canvas.getContext('2d')
```

* strokeRect(left, top, width, heigth) - Un cuadrado
    ```javascript
        ctx.strokeRect(50,0, 400, 200)
    ```
* arc(x, y, radio, ) - Circulo
    ```javascript
        ctx.arc(20, 50, 100, 10, 40)
        ctx.stroke()
    ``
* strokeStyle - Color del borde.
    ```javascript
        ctx.strokeStyle = '#ccc'
    ```
* fillRect - Relleno, o capa.
    ```javascript
        ctx.fillRect(10, 20 400, 200)
    ```
* fillStyle - Color del relleno.
    ```javascript
        ctx.fillStyle = ''
    ```
* lineWidth - Grodor del borde, (grosor de la linea)
    ```javascript
        ctx.lineWidth = '4'
    ```
* moveTo
* lineTo(x, y) - Trazar una linea en conrdenadas. <br>
    Ambos puntos de unen.
    ```javascript
        ctx.beginPath() //Inicia el recorrido
        ctx.lineTo(100, 300) //Punto uno
        ctx.lineTo(100, 350) //Punto dos
        ctx.stroke() //Crea la linea
        ctx.closePath() //Termina el recorrido
    ```
* closePath() - Cierra una linea.
* beginPath() - Inicia un nuevo recorrido, no es necesario la primera vez.