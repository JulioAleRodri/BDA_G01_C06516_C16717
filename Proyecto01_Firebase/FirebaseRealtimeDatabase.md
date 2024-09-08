<!-- markdownlint-disable MD033 -->

# Firebase Realtime Database

## Actividad para compartir conocimiento

### Autores

- Geancarlo Rivera Hernandez C06516
- Julio Alejandro Rodríguez Salguera C16717

#### Bitácora

[Ir a la bitácora](./Bitacora/README.md)
***

### Tabla de Contenidos

- [Firebase Realtime Database](#firebase-realtime-database)
  - [Actividad para compartir conocimiento](#actividad-para-compartir-conocimiento)
    - [Autores](#autores)
      - [Bitácora](#bitácora)
    - [Tabla de Contenidos](#tabla-de-contenidos)
  - [¿Y qué es Firebase?](#y-qué-es-firebase)
    - [Contexto](#contexto)
    - [Historia](#historia)
  - [¿Y cómo se modela?](#y-cómo-se-modela)
    - [Datos anidados en un documento](#datos-anidados-en-un-documento)
    - [Sub-colecciones](#sub-colecciones)
    - [Colecciones a nivel de raíz](#colecciones-a-nivel-de-raíz)
  - [¿Y cómo hago consultas?](#y-cómo-hago-consultas)
    - [Obtener la referencia a la base de datos](#obtener-la-referencia-a-la-base-de-datos)
    - [Escribir datos](#escribir-datos)
      - [Escritura básica](#escritura-básica)
    - [Leer datos](#leer-datos)
      - [Escuchar eventos para obtener datos](#escuchar-eventos-para-obtener-datos)
      - [Leer datos una sola vez](#leer-datos-una-sola-vez)
      - [Leer datos una sola vez con un observador](#leer-datos-una-sola-vez-con-un-observador)
    - [Modificar o eliminar datos](#modificar-o-eliminar-datos)
      - [Modificar campos específicos](#modificar-campos-específicos)
      - [Eliminar datos](#eliminar-datos)
  - [¿Existen estructuras o algoritmos?](#existen-estructuras-o-algoritmos)
    - [Listas](#listas)
      - [Escuchar cambios de los nodos hijos](#escuchar-cambios-de-los-nodos-hijos)
    - [Ordenamiento y filtrado](#ordenamiento-y-filtrado)
      - [Ordenamiento](#ordenamiento)
      - [Filtrado](#filtrado)
  - [¿Cuáles mecanismos de optimización ofrece?](#cuáles-mecanismos-de-optimización-ofrece)
    - [Indexación de datos](#indexación-de-datos)
      - [Indexación para optimizar `OrderByChild()`](#indexación-para-optimizar-orderbychild)
    - [Optimización de consultas](#optimización-de-consultas)
    - [Optimización de conexiones](#optimización-de-conexiones)
    - [División de datos en múltiples instancias (*Sharding*)](#división-de-datos-en-múltiples-instancias-sharding)
    - [Recomendaciones adicionales de optimización](#recomendaciones-adicionales-de-optimización)
  - [¿Qué mecanismos de recuperación y continuidad de servicio ofrece?](#qué-mecanismos-de-recuperación-y-continuidad-de-servicio-ofrece)
    - [Gestión de presencia](#gestión-de-presencia)
      - [Operación `onDisconnect()`](#operación-ondisconnect)
      - [Monitorización de la conexión](#monitorización-de-la-conexión)
      - [Timestamps](#timestamps)
      - [Sincronización estado local y de servidor](#sincronización-estado-local-y-de-servidor)
    - [Recuperación de datos](#recuperación-de-datos)
  - [Conclusiones](#conclusiones)
  - [Referencias](#referencias)

***

## ¿Y qué es Firebase?

### Contexto

Firebase Realtime Database es una base de datos NoSQL basada en la nube que sincroniza datos en tiempo real, permitiendo que la información de los usuarios se mantenga actualizada incluso cuando la aplicación se desconecta. Esta base de datos está alojada en la nube y almacena la información como documentos [JSON](https://www.json.org/json-en.html), además de permitir compartir información en multiplataforma y realizar actualizaciones de los datos más recientes a tiempo real [[1]](#Ref1).

Esta plataforma ofrece las siguientes funcionalidades esenciales:

- En tiempo real: Cada vez que la información es cambiada, se actualiza automáticamente para cada uno de los dispositivos conectados en cuestión de milisegundos.
- Funciona sin conexión: No se pierde la información aunque el usuario no tenga conexión, pues los cambios se guardan en el almacenamiento del usuario, permitiendo que al momento de volver a conectarse, se sincronice toda la información de manera automática.
- Accesible desde dispositivos clientes: Es accesible desde dispositivos móviles o navegadores, sin un servidor de aplicación.
- Escalable entre diferentes bases de datos: Brinda servicios para dividir los datos entre distintas instancias de la base de datos [[1]](#Ref1).

### Historia

Firebase Realtime Database, como bien su nombre indica, pertenece a la plataforma de desarrollo **Firebase** que es parte de Google. Pero no siempre fue así, pues aunque ahora forma parte importante de su ecosistema, esta plataforma nació de la mano de James Tamplin y Andrew Lee, quienes comenzaron en 2008 creando una primera aplicación web gratuita llamada SendMeHome.com. El objetivo de esta web estaba alejado de lo que a hoy se dedica la empresa, pues buscaba convertir cada objeto en una historia colectiva, creada por todas las personas que interactuaban con ese objeto [[2]](#Ref2).

Sin embargo, Tamplin y Lee se encontraron con obstáculos al tratar de implementar un chat en tiempo real que permitiera a los usuarios conversar mientras compartían historias sobre los objetos. Ante la falta de una solución adecuada en el mercado de ese momento, optaron por desarrollar su propio sistema de chat. Este reto técnico los llevó a un nuevo enfoque empresarial: crear widgets de chats en tiempo real que pudieran integrarse en otros sitios web. Así fue como en 2011 nació Envolve, una plataforma que ofrecía chats como servicio [[2]](#Ref2).

Posteriormente se dieron cuenta de que los usuarios y clientes de Envolve utilizaban Firebase para más que simples chats, pues, por ejemplo, lo usaban para intercambiar información a tiempo real de videojuegos en línea, como herramienta de colaboración, análisis de productos, entre otros propósitos [[3]](#Ref3). En ese momento decidieron crear Firebase, para que brindara un servicio de base de datos en tiempo real y de esta manera solventar la necesidad que existía en el mercado [[2]](#Ref2).

En el año 2014 Firebase fue adquirida por Google, lo que permitió al servicio crecer en su infraestructura y alcance, además de colaborar con **Google Cloud Platform**, lo que les permitió a los usuarios utilizar los servicios en la nube de Google [[4]](#Ref4). En retrospectiva, el éxito de Firebase y su servicio de base de datos en tiempo real se basó en comprender las necesidades y deseos de los usuarios, aprovechando la oportunidad para satisfacer las demandas del mercado. Por esto, si alguna vez observas a los usuarios utilizar un programa de una manera para la cual "no fue diseñado", recuerda esta historia y considera cómo podrías ofrecer una solución que responda a esa necesidad.

## ¿Y cómo se modela?

**Nota:** Esta sección se centra en la utilización de Firebase Realtime Database en aplicaciones web.

A diferencia de SQL, las bases de datos NoSQL no cuentan con una manera estandarizada de modelar, siendo que Firebase Realtime Database se basa en un **árbol JSON** [[5]](#Ref5).

Aún así, Firebase ofrece tres maneras distintas de estructurar la información en su árbol JSON, cada una con sus propias ventajas, desventajas y casos de uso recomendados. En general, el objetivo es crear colecciones grandes y documentos pequeños, ya que esto mejora el rendimiento y cumple con las limitaciones de la plataforma [[6]](#Ref6). A continuación se presentan las maneras de estructurar los datos:

### Datos anidados en un documento

Los datos de los documentos se pueden guardar en objetos complejos dentro del mismo documento. Esto es muy útil, y hasta necesario, en el caso de tener información simple, limitada y que se sabe que no va a crecer en el futuro. Pero justamente por eso es que *no* es escalable, pues si se está guardando objetos complejos, muy grandes o que en el futuro pueden crecer sin límite, esto puede afectar la eficiencia y eficacia de las consultas, además de poder llegar a incumplir las limitaciones de Firebase. Como se puede ver en el siguiente ejemplo, los datos de los usuarios son fijos y simples, o en otras palabras, pueden cambiar pero no van a crecer, y no guardan información o estructuras complejas [[6]](#Ref6).

~~~json
{
  "users": {
    "user1": {
      "name": "Omar",
      "lastName": "Camacho",
      "country": "Costa Rica",
      "province": "Heredia",
      "canton": "San Rafael"
    }
  }
}
~~~

### Sub-colecciones

En este tipo de estructura, se crean sub-colecciones de los datos que se pueden expandir con el tiempo. Si se desea ver de otra manera, es como tener bolsas en las que se guardan los datos más complejos por cada documento, o en términos de Programación Orientada a Objetos, como tener un objeto interno que guarda información del objeto que lo contiene. Esto tiene la clara ventaja de permitir almacenar información que puede crecer con el tiempo y vincularla con el "nodo padre" del árbol, además de que existen consultas que se pueden utilizar sobre las sub-colecciones directamente.

Sin embargo, esta estructura puede dificultar ciertos tipos de generalización y relaciones entre documentos. Por ejemplo, requiere realizar más consultas, lo que ralentiza el proceso, y requiere descargar todo el subconjunto de datos, aunque parte de esa información no se vaya a utilizar. Además, puede complicar la representación de relaciones de muchos a muchos. Es por estas razones que se recomienda utilizar sub-colecciones en los casos en los que se guardará información que puede crecer, pero que está relacionada principalmente con el "nodo padre" y, cuando se lean los datos, tenga sentido leer todos los datos juntos [[6]](#Ref6).

Un ejemplo de la utilización de una sub-colección se muestra a continuación:

~~~json
{
  "users": {
    "user1": {
      "name": "Omar",
      "lastName": "Camacho",
      "country": "Costa Rica",
      "province": "Heredia",
      "canton": "San Rafael",
      "contacts": {
        "contact1": {
          "name": "Luis",
          "phoneNumber": "+50612345678",
        },
        "contact2": {
          "name": "Maria",
          "phoneNumber": "+50687654321",
        }
      }
    }
  }
}

~~~

### Colecciones a nivel de raíz

Esta es la principal manera de estructurar la información, siendo que se crean colecciones al nivel de la raíz de la base de datos que contiene conjuntos de información. Esta es una manera fácil de crear relaciones muchos a muchos y brinda herramientas para hacer consultas más complejas. Sin embargo, esto vuelve más compleja la base de datos y puede complicar algunos tipos de consultas. De igual forma, esta es la manera en la que se recomienda estructurar la información, pero se debe tener en consideración las necesidades y características del sistema para evitar añadir complejidad innecesaria [[6]](#Ref6).

Un ejemplo de cómo usar esta estructuración se muestra a continuación:

~~~json
{
  "users": {
    "users1": {
      "name": "Omar",
      "lastName": "Camacho",
      "country": "Costa Rica",
      "province": "Heredia",
      "canton": "San Rafael"
    }
  },
  "chats": {
    "chat1": {
      "autor": "Omar",
      "name": "Brave"
      "participants" {
        "participants1": "Omar"
      }
    }
  }
}

~~~

## ¿Y cómo hago consultas?

**Nota:** Esta sección se centra en la utilización de Firebase Realtime Database en aplicaciones web.

De la misma manera en la que no existe un modelo específico para NoSQL ni para Firebase Realtime Database, no existe un lenguaje específico u homogéneo para realizar consultas, al menos no de la manera en la que estamos acostumbrados con SQL. Además, la técnica o código utilizado para realizar consultas puede variar dependiendo de cuál herramienta, tecnología o lenguaje de programación se esté utilizando, para nuestro caso, se darán ejemplos para aplicaciones web utilizando `javascript`.

### Obtener la referencia a la base de datos

Como esencial primer paso, debemos obtener la referencia a la base de datos que nos permita manipular y realizar consultas en la misma [[7]](#Ref7).
Esto se consigue fácilmente de la siguiente manera:

~~~js
import { getDatabase } from "firebase/database";

const MyBeautifulDatabase = getDatabase();
~~~

### Escribir datos

#### Escritura básica

Ahora a lo interesante, para escribir primeramente necesitaremos contar con la referencia a la base de datos y saber que la información se obtiene de manera asíncrona, sumado a que se obtiene la información al iniciar y cada vez que esta cambia.

La manera más básica para escribir es utilizando la función `set()`, la cual requiere saber cuál es la ruta dentro del árbol a la colección que deseamos modificar [[7]](#Ref7). Por ejemplo, si deseamos añadir un nuevo usuario llamado "Omar" con su información a la base de datos, simplemente se puede utilizar el siguiente código:

~~~js
import { getDatabase, ref, set } from "firebase/database";

const MyDatabase = getDatabase();

userID = '1234567890'

set(ref(MyDatabase, 'usuarios/' + userID), {
  name: "Omar",
  lastName: "Camacho",
  country: "Costa Rica"
});
~~~

Es importante destacar que el elemento `usuarios/1234567890` representa la ruta al ID del dato, donde "1234567890" es el identificador que se utiliza para
interactuar con ese dato en el programa; Siendo que podría ser cualquier valor generado por el programa o definido según lo que se desee. Además, se recomienda extraer esta lógica a una función que pueda ser reutilizable.

### Leer datos

#### Escuchar eventos para obtener datos

En este caso se creará un escuchador u "*observer*" que va a obtener los datos de una dirección en específico; siendo que una vez se empieza a escuchar, se realiza una lectura de los datos una primera vez y cada vez que la información cambia, de esta forma se mantiene la información actualizada. Esto se puede realizar por medio de la operación `onValue()` y lo que se obtiene es una "*snapshot*" de los datos de la dirección y de todos sus hijos incluidos. En el caso de que la información no exista, al realizar la operación `val()`, esta devuelve `null` [[7]](#Ref7).

~~~js
import { getDatabase, ref, onValue } from "firebase/database";

function changeLastMessage(somebodyID, somebodyUser, lastMessage) {
  const MyDatabase = getDatabase();
  
  const lastMessage = ref(MyDatabase, 'messages/' + somebodyID + '/lastMessage');
  
  onValue(lastMessage, (snapshot) => {
    const data = snapshot.val();
    updateLastMessage(somebodyUser, data);
  });
} 
~~~

Nótese que `someID`, `someUser` y `updateLastMessage()` son valores utilizados de ejemplo. El primero hace referencia al ID para identificar al usuario en la estructura del árbol, el segundo al propio usuario en la aplicación web y el tercero es una función para actualizar el último mensaje del usuario.

Además, se recomienda obtener el nivel más bajo para obtener la información necesaria, dado que se obtiene toda la información de los "nodos hijos", lo que podría implicar traer información que no se utilizará o descargar una gran cantidad de información innecesaria. Por lo tanto, siempre se debe tratar de obtener solo la información que se requiera.

#### Leer datos una sola vez

Se puede obtener una única vez la información con la operación `get()`, pero esto no se recomienda, dado que aumenta la utilización de la red y disminuye la eficiencia (además de que sale más caro para la billetera). Al utilizar `get()` no se aprovechan las cualidades de respuesta a tiempo real de Firebase y no está tan optimizada para funcionar si no existe conexión [[7]](#Ref7).

~~~js
import { getDatabase, ref, child, get } from "firebase/database";

const MyDatabase = getDatabase();

somebodyID = '1234567890';

get(child(MyDatabase, 'users/' + somebodyID))
    .then((snapshot) => {
  if(snapshot.exist()) {
    doSomethingWithData(snapshot.val());
  } else {
    doSomethingWithoutData(snapshot.val());
  }
}).catch((error) => {
  manageError(error);
});
~~~

#### Leer datos una sola vez con un observador

Por otro lado, se puede utilizar la opción `once()` o `onlyOnce: true` para obtener información que se encuentra en el caché, de manera automática. Eso implica que, en lugar de obtener la información más reciente de la base de datos, se utilizará la información que se encuentre en el caché del disco local.

Esto puede ser de utilidad en el caso de que no se requieran los dato más recientes, dado que estos no cambien
con tanta frecuencia o que no se necesiten [[7]](#Ref7).

~~~js
import { getDatabase, ref, onValue } from "firebase/database";

const MyDatabase = getDatabase();

somebodyID = '1234567890';

const lastMessage = ref(MyDatabase, 'messages/' + somebodyID + '/lastMessage');

onValue(lastMessage, (snapshot) => {
  const data = snapshot.val();
  updateLastMessage(somebodyUser, data);
}, { onlyOnce: true });
~~~

### Modificar o eliminar datos

#### Modificar campos específicos

Para esta sección es importante hacer una diferenciación. La operación `update()` modifica el contenido de datos ya existentes, mientras que `push()` agrega nuevos elementos a una lista ya existente y devuelve un ID único creado por la aplicación.

Específicamente, `update()` puede modificar los valores de los "nodos hijos" que pertenecen a un nivel menor, dada una llave específica que identifica al elemento. Este método puede editar varios elementos al mismo tiempo con una sola consulta [[7]](#Ref7). A continuación se muestra el funcionamiento de estas dos operaciones:

~~~js
import { getDatabase, ref, child, push, update } from "firebase/database";

function likePost(userID, postID, username) {
  const MyDatabase = getDatabase();

  const newPostLike = {
    userID: userID,
    postID: postID,
    userWhoLiked: username
  };

  const newLikePostKey = push(child(ref(MyDatabase), 'posts')).key;

  const updates = {};
  
  updates['/users-likes/' + userID] = newPostLike;
  updates['/posts/' + postID] = newPostLike;

  return update(ref(MyDatabase), updates);
}
~~~

#### Eliminar datos

Se puede eliminar algún dato de manera sencilla simplemente utilizando la operación `remove()`.
O se puede utilizar el `set()` o `update()` con `null` para sobreescribir el valor, lo cuál puede
ser útil si se desea eliminar varios "nodos hijos" con una sola llamada  [[7]](#Ref7).

~~~js
import { getDatabase, ref, remove } from "firebase/database";

const MyDatabase = getDatabase();

const userRef = ref(MyDatabase, 'users/1234567890');

remove(userRef).then(() => {
  console.log("User remove");
}).catch((error) => {
  console.error("Can't remove user:", error);
});
~~~

## ¿Existen estructuras o algoritmos?

Como se mencionó anteriormente, la estructura utilizada en Firebase Realtime Database es un *árbol JSON*, por lo que las relaciones que se crean son de "padre-hijo". De manera general, Firebase ofrece menos opciones de estructuras y algoritmos en comparación con una base de datos relacional, pero sí existen opciones interesantes.

### Listas

Como su nombre indica, son una colección de datos relacionados a una referencia "padre". Lo interesante de esta estructura es que permite agregar nuevos datos utilizando la operación `push()`, que genera una llave única para cada uno de los "hijos" agregados a la referencia. La operación `push()` permite a varios usuarios agregar datos de forma **paralela**, por lo que estas listas se pueden utilizar para datos que son modificados constantemente por los usuarios [[8]](#Ref8).

~~~js
import { getDatabase, ref, push, set } from "firebase/database";

function sentMessage(chatId, userId, newMessage) {
  const MyDatabase = getDatabase();
  
  const newChatMessage = push(ref(MyDatabase, 'chat-messages'))
  
  set(ref(newChatMessage, {
    chat: chatId,
    user: userId,
    message: newMessage
  })
}
~~~

#### Escuchar cambios de los nodos hijos

De la misma manera que existen "*triggers*" en las bases de datos relacionales, Firebase ofrece la opción
de escuchar eventos de los nodos hijos y reaccionar ante estos. Por ejemplo, si se agrega, elimina o modifica
un nodo hijo, se puede reaccionar cambiando la interfaz gráfica para que este cambio se refleje para el usuario [[8]](#Ref8).

~~~js
import { getDatabase, ref, onChildAdded, onChildChanged, onChildRemoved } from "firebase/database";

const MyDatabase = getDatabase();
  
const messagesRef = ref(MyDatabase, 'chat-messages/1234567890');

onChildAdded(messagesRef, (data) => {
  addMessageToChat(data.key, data.val().chatId, data.val().userId, data.val().message);
});

onChildUpdate(messagesRef, (data) => {
  changeMessageFromChat(data.key, data.val().chatId, data.val().userId, data.val().message);
});

onChildDelete(messagesRef, (data) => {
  deleteMessageToChat(data.key);
});
~~~

### Ordenamiento y filtrado

#### Ordenamiento

Se pueden realizar consultas ordenando los resultados dado una llave, un valor o hasta el valor de un nodo hijo,
utilizando las opciones de `orderByKey()`, `orderByValue()` y `orderByChild()` [[8]](#Ref8).

~~~js
import { getDatabase, ref, query, orderByValue } from "firebase/database";

const MyDatabase = getDatabase();
  
const userMessages = query(ref(MyDatabase, 'user-messages/1234567890'), orderByValue('message-date'));
~~~

#### Filtrado

En este caso se puede realizar un filtrado para solamente tomar los datos que cumplan con ciertas características, además de que se puede combinar con el ordenamiento [[8]](#Ref8). A continuación se presentan algunas de las opciones para realizar el filtrado de datos:

- `limitToFirst()`: Limita la cantidad de objetos obtenidos desde el inicio de la lista de resultados.
- `limitToLast()`: Limita la cantidad de objetos obtenidos desde el final de la lista de resultados.
- `startAt()` y `startAfter()`: El primero devuelve elementos mayores o iguales al valor especificado, y el segundo devuelve elementos mayores, ambos según el método de ordenación elegido.
- `endAt()` y `endBefore()`: El primero devuelve elementos menores o iguales al valor especificado, y el segundo devuelve elementos menores, ambos según el método de ordenación elegido.
- `equalTo()`: Devuelve una lista de elementos cuyo valor sea igual al valor específico dado.

~~~js
import { getDatabase, ref, query, orderByValue } from "firebase/database";

const MyDatabase = getDatabase();
  
const userMessages = query(ref(MyDatabase, 'user-messages/1234567890'), orderByValue('message-date'), limitToLast(25));
~~~

## ¿Cuáles mecanismos de optimización ofrece?

Firebase Realtime Database ofrece diferentes mecanismos y herramientas de optimización para mejorar el rendimiento de las consultas y reducir los costos asociados.

### Indexación de datos

Firebase Realtime Database permite indexar datos para optimizar el rendimiento de las consultas  mediante la regla `.indexOn`.
 Los índices pueden crearse en cualquier nivel del árbol de la base de datos, aunque se recomienda hacerlo en los niveles más bajos posibles para mejorar la eficiencia, dado que se obtiene la información de los "nodos hijos" de la referencia. Además, Firebase soporta índices compuestos, lo que permite realizar consultas más complejas y eficientes. Al establecer las claves que se utilizarán en las consultas, la base de datos indexará esas claves en los servidores, lo que mejora significativamente el rendimiento de las consultas a medida que la aplicación crece [[9]](#Ref9).

#### Indexación para optimizar `OrderByChild()`

Firebase es muy flexible en cuanto a la indexación de datos, para ilustrar un ejemplo, vamos a establecer un índice para optimizar las consultas ordenadas con la operación `orderByChild()` presentada anteriormente.

Considere la siguiente estructura de datos:

~~~json
{
  "users": {
    "user1": {
      "name": "Omar",
      "lastName": "Camacho",
      "country": "Costa Rica",
      "province": "Heredia",
      "canton": "San Rafael"
    },
    "user2": {
      "name": "Kenneth",
      "lastName": "Villalobos",
      "country": "Costa Rica",
      "province": "San José",
      "canton": "San José"
    }
  }
}
~~~

Por defecto, Firebase indexa los datos por la clave de los usuarios (supongamos en este caso que la clave es el nombre) y optimiza las consultas para esta clave. Sin embargo, suponiendo que en nuestro contexto de aplicación, necesitamos ordenar a los usuarios por provincia o cantón, pero no por la clave de usuario, podemos establecer un índice para estos atributos. Para ello, utilizamos la regla `.indexOn` en el *Archivo de Reglas de Seguridad de la Base de Datos de Firebase*, como se muestra a continuación:

~~~json
{
  "rules": {
    "users": {
      ".indexOn": ["province", "canton"]
    }
  }
}
~~~

Con esta regla, Firebase indexará los datos de los usuarios por provincia y cantón. Esto permitirá realizar consultas más eficientes y rápidas, especialmente cuando se utilice la operación `orderByChild()` para ordenar los resultados basados en estos atributos. Al crear estos índices, Firebase optimiza el acceso y la recuperación de datos, reduciendo el tiempo de consulta y mejorando el rendimiento general de la base de datos [[9]](#Ref9).

Note además, que se está creando un *índice compuesto*, ya que se están indexando múltiples atributos en una sola regla. Esto permite realizar consultas más complejas y eficientes. Al establecer índices compuestos, se mejora significativamente el rendimiento de las consultas a medida que la aplicación crece y se manejan grandes volúmenes de datos. De igual forma se pueden establecer índices de una sola clave para optimizar consultas específicas [[9]](#Ref9).

### Optimización de consultas

Firebase Realtime Database ofrece varias optimizaciones para mejorar el rendimiento de las consultas. Por ejemplo, la operación `orderByChild()` como ya se vió anteriormente, permite utilizar índices, lo que acelera y hace más eficiente la búsqueda de datos. Además, el uso de reglas basadas en consultas (*Query-based Rules*) permiten filtrar los datos antes de que sean descargados, reduciendo la cantidad de información que se transfiere y procesa en el cliente, esto a su vez optimiza la eficiencia y disminuye los costos. Un ejemplo es la restricción `query.limitToFirst`, que limita la cantidad de datos descargados, mejorando el rendimiento y reduciendo costos [[10]](#Ref10).

### Optimización de conexiones

Firebase optimiza las conexiones permitiendo el uso de [SDKs](https://aws.amazon.com/what-is/sdk/?nc1=h_ls) (Software Development Kits) nativos en lugar de utilizar [APIs REST](https://aws.amazon.com/what-is/restful-api/?nc1=h_ls). Los SDKs nativos permiten mantener una conexión persistente con la base de datos, gracias a esto se reducen los costos de estar realizando conexiones y desconexiones constantes, ya que implican costos de cifrado SSL y carga en la base de datos debido a las solicitudes REST, como `Get` y `Put`, que son de corta duración. Además, Firebase recomienda que si es necesario usar la API REST que ofrece, es mejor utilizarla con *HTTP Keep-Alive*, que permite mantener una conexión abierta o eventos enviados por el servidor, lo que reduce los costos asociados con los intercambios de SSL. De igual forma, la recomendación principal es utilizar los SDKs nativos para aprovechar al máximo las ventajas de Firebase [[10]](#Ref10).

### División de datos en múltiples instancias (*Sharding*)

Firebase Realtime Database permite dividir los datos en múltiples instancias, lo que se conoce como *sharding*. Esta técnica permite escalar horizontalmente la base de datos, distribuyendo los datos en múltiples instancias para mejorar el rendimiento y la escalabilidad. Aplicar  esta estrategia permite obtener tres beneficios clave  [[10]](#Ref10):

1. Aumenta el número total de conexiones activas simultáneas permitidas en la aplicación al distribuirlas entre diferentes instancias de bases de datos.
2. Equilibra la carga de trabajo entre las instancias mejorando el rendimiento y la capacidad de respuesta de la base de datos.
3. Facilita la restricción de acceso, permitiendo que solo los usuarios autorizados accedan a instancias específicas de la base de datos, lo que mejora la seguridad, reduce la latencia y protege la privacidad de los datos.

### Recomendaciones adicionales de optimización

Además de las optimizaciones mencionadas, Firebase Realtime Database ofrece una serie de recomendaciones adicionales para mejorar el rendimiento y la eficiencia de las consultas de sus bases de datos. Algunas de estas recomendaciones incluyen [[10]](#Ref10):

1. **Estructuración eficiente de los datos:** Es recomendable mantener la estructura lo más plana posible para minimizar la cantidad de datos innecesarios que se descargan. Además, agrupar múltiples actualizaciones en una sola operación ayuda a optimizar el uso de la base de datos.
2. **Control de acceso y seguridad:** Es fundamental implementar reglas de seguridad que protejan el acceso a los datos y eviten operaciones no autorizadas. Esto no solo reduce la exposición de la base de datos, sino también la carga que esta debe manejar.
3. **Optimización de rendimiento:** Limitar las descargas de datos utilizando reglas basadas en consultas y filtrado tales como `limitToFirst()` o `limitToLast()` vistas previamente, restringe la cantidad de información leída y descargada en el cliente, lo que mejora el rendimiento y reduce los costos. Adicionalmente, es útil reutilizar sesiones SSL para disminuir el overhead de cifrado.
4. **Limpieza de datos:** Eliminar datos obsoletos, duplicados o innecesarios de la base de datos ayuda a mantener un rendimiento óptimo y a reducir la carga. Es recomendable programar tareas de limpieza periódicas para mantener la base de datos ordenada y eficiente.

## ¿Qué mecanismos de recuperación y continuidad de servicio ofrece?

**Nota:** Esta sección se centra en la utilización de Firebase Realtime Database en aplicaciones web con JavaScript.

Firebase Realtime Database ofrece mecanismos de recuperación y continuidad de servicio para garantizar la disponibilidad y confiabilidad de los datos en caso de fallos o interrupciones. Principalmente directivas para trabajar aún cuando se pierda la conexión a internet. A continuación se presentan algunos de los mecanismos más importantes utilizando `JavaScript`.

### Gestión de presencia

#### Operación `onDisconnect()`

Firebase ofrece el método `onDisconnect()` para gestionar la presencia de los usuarios, permitiendo programar acciones automáticas, como eliminar o actualizar datos, cuando un usuario se desconecta, ya sea de forma intencional o por fallos inesperados. Esto garantiza que operaciones de limpieza, actualización, o cualquier acción definida se ejecute incluso en casos de fallas de red o cierres inesperados del cliente [[11]](#Ref11).

A continuación se muestra un ejemplo simple de cómo utilizar `onDisconnect()` para emitir un mensaje de salida cuando un usuario se desconecta de la aplicación:

~~~js
import { getDatabase, ref, onDisconnect } from "firebase/database";

const db = getDatabase();
const presenceRef = ref(db, "disconnectMessage");

onDisconnect(presenceRef).set("User has disconnected");

~~~

En este ejemplo, cuando un usuario se desconecta de la aplicación, Firebase emitirá un mensaje de salida que se almacenará en la base de datos en la referencia `disconnectMessage`. De esta forma se tiene un log de los usuarios que se desconectan y se podría realizar un seguimiento de la presencia de los usuarios en la aplicación y actuar en consecuencia.

#### Monitorización de la conexión

Firebase Realtime Database proporciona un nodo especial en la base de datos (`/.info/connected`) para monitorear si un cliente está conectado o desconectado del servidor. Al utilizar este nodo, la aplicación puede detectar cambios en el estado de conexión y reaccionar de manera adecuada. Es útil para implementar funcionalidades que dependen del estado en línea o fuera de línea del cliente [[11]](#Ref11).

A continuación se muestra un ejemplo de cómo utilizar el nodo `/.info/connected` para monitorear el estado de conexión de un cliente:

~~~js
import { getDatabase, ref, onValue } from "firebase/database";

const db = getDatabase();
const connectedRef = ref(db, ".info/connected");

onValue(connectedRef, (snap) => {
  if (snap.val() === true) {
    console.log("Connected");
  } else {
    console.log("Disconnected");
  }
});
~~~

En este ejemplo, la aplicación detecta cuando el cliente se conecta o desconecta y registra el estado en la consola. Esto permite a la aplicación ajustar su comportamiento en función del estado de conexión del cliente. Esto funciona gracias a que `/.info/connected` retorna una referencia con un valor booleano correspondiente al estado de conexión del cliente.

#### Timestamps

Firebase también ofrece la capacidad de insertar marcas de tiempo generadas en el servidor como parte de los datos almacenados. Este mecanismo permite registrar la hora exacta en que un cliente se desconecta o realiza una acción, usando `serverTimestamp()`. Esto es especialmente útil para manejar eventos como la última vez que un usuario estuvo en línea [[11]](#Ref11).

A continuación se muestra un ejemplo de cómo utilizar `serverTimestamp()` para registrar la hora exacta en que un usuario se desconecta:

~~~js
import { getDatabase, ref, onDisconnect, serverTimestamp } from "firebase/database";

const db = getDatabase();
const lastOnlineRef = ref(db, "users/omar/lastOnline");

onDisconnect(lastOnlineRef).set(serverTimestamp());
~~~

En este ejemplo, la base de datos registra la última vez que el usuario estuvo en línea cuando el cliente se desconecta. La marca de tiempo generada por el servidor asegura que el valor es preciso, independientemente del tiempo local del cliente. Note además que `serverTimestamp()` se está utilizando la operación `onDisconnect()` para activar la escritura de la marca de tiempo en la base de datos cuando el cliente se desconecta.

#### Sincronización estado local y de servidor

Gracias a los mecanismos previamente vistos, Firebase Realtime Database permite mantener la sincronización entre el estado local y el estado del servidor, incluso en situaciones de desconexión. Esto garantiza que los datos se actualicen y sincronicen correctamente, incluso si el cliente se desconecta temporalmente. Al utilizar operaciones en conjunto como `onDisconnect()`, `serverTimestamp()` `.info/connected`, la aplicación puede mantener la integridad de los datos y garantizar que los cambios se reflejen correctamente [[11]](#Ref11).

A continuación se muestra un ejemplo de cómo gestionar la sincronización entre el estado local y el estado del servidor:

~~~js
import { getDatabase, ref, onValue, set, serverTimestamp } from "firebase/database";

const db = getDatabase();
const connectedRef = ref(db, '.info/connected');
const myConnectionsRef = ref(db, 'users/omar/connections');
const lastOnlineRef = ref(db, 'users/omar/lastOnline');

onValue(connectedRef, (snap) => {
  if (snap.val() === true) {
    const con = push(myConnectionsRef);
    set(con, true);
    onDisconnect(con).remove();
    onDisconnect(lastOnlineRef).set(serverTimestamp());
  }
});
~~~

Este ejemplo demuestra un sistema de presencia donde se registra el estado de conexión del usuario y se eliminan sus conexiones cuando se desconecta. Además, se actualiza la hora de la última conexión. Este mecanismo asegura que el estado entre el cliente y el servidor esté siempre sincronizado.

### Recuperación de datos

En cuando al respaldo y recuperación de datos, Firebase Realtime Database proporciona replicación automática de datos en múltiples ubicaciones para asegurar alta disponibilidad y durabilidad, incluso durante fallos en la infraestructura. Aunque no ofrece un esquema de replicación tan detallado como Cloud Firestore, que permite personalizar, programar copias de seguridad y otras características [[12]](#Ref12), la replicación automática en Realtime Database asegura que los datos sean accesibles y protegidos. Para realizar copias de seguridad en Firebase Realtime Database, se pueden ejecutar manualmente, utilizando herramientas o scripts personalizados para exportar y restaurar datos. Además se pueden programar respaldos por periodos de tiempo según se requiera [[13]](#Ref13). También es posible integrar Realtime Database con Cloud Firestore u otros servicios en la nube para aprovechar funciones avanzadas de copias de seguridad y recuperación [[14]](#Ref14).

## Conclusiones

Firebase Realtime Database es una base de datos en tiempo real que ofrece una estructura de datos flexible y escalable, ideal para aplicaciones web y móviles que requieren sincronización en tiempo real y alta disponibilidad. La estructura de árbol JSON permite organizar los datos de manera jerárquica y eficiente, facilitando la gestión y consulta de información. La base de datos ofrece mecanismos de optimización, recuperación y continuidad de servicio para garantizar el rendimiento, la integridad y la disponibilidad de los datos. Al utilizar las herramientas y recomendaciones proporcionadas por Firebase, los desarrolladores pueden crear aplicaciones robustas y eficientes que respondan a las necesidades de los usuarios y se adapten a los cambios en el entorno de desarrollo.

***

## Referencias

[1] Firebase, "*Firebase Realtime Database*", Firebase.com. [En línea]. Disponible en <span id="Ref1">https://firebase.google.com/docs/database</span>. [Accedido: Sep. 1, 2024].

[2] S. Melendez, "*Sometimes You're Just One Hop From Something Huge*", Fast Company, May 27, 2014. [En línea]. Disponible en <span id="Ref2">https://www.fastcompany.com/3031109/sometimes-youre-just-one-hop-from-something-huge</span>

[3] M. Lehenbauer, "*Developers, meet Firebase!*", The Firebase Blog, Abr 12, 2014. [En línea]. Disponible en <span id="Ref3">https://firebase.blog/posts/2012/04/developers-meet-firebase</span>

[4] J. Tamplin, "*Firebase is Joining Google!*", The Firebase Blog, Oct 21, 2014. [En línea]. Disponible en <span id="Ref4">https://firebase.blog/posts/2014/10/firebase-is-joining-google</span>

[5] Firebase, "*Structure Your Database*", Firebase.com. [En línea]. Disponible en <span id="Ref5">https://firebase.google.com/docs/database/web/structure-data</span>. [Accedido: Sep. 1, 2024].

[6] Firebase, "*Choose a data structure*", Firebase.com. [En línea]. Disponible en <span id="Ref6">https://firebase.google.com/docs/firestore/manage-data/structure-data</span>. [Accedido: Sep. 1, 2024].

[7] Firebase, "*Read and Write Data on the Web*", Firebase.com. [En línea]. Disponible en <span id="Ref7">https://firebase.google.com/docs/database/web/read-and-write#web</span>. [Accedido: Sep. 1, 2024].

[8] Firebase, "*Work with Lists of Data on the Web*", Firebase.com. [En línea]. Disponible en <span id="Ref8">https://firebase.google.com/docs/database/web/lists-of-data#web_4</span>. [Accedido: Sep. 4, 2024].

[9] Firebase, "*Index Your Data*", Firebase.com. [En línea]. Disponible en <span id="Ref9">https://firebase.google.com/docs/database/security/indexing-data</span>. [Accedido: Sep. 5, 2024].

[10] Firebase, "*Optimize Database Performance*", Firebase.com. [En línea]. Disponible en <span id="Ref10">https://firebase.google.com/docs/database/usage/optimize</span>. [Accedido: Sep. 6, 2024].

[11] Firebase, "*Enabling Offline Capabilities*", Firebase.com. [En línea]. Disponible en <span id="Ref11">https://firebase.google.com/docs/database/web/offline-capabilities</span>. [Accedido: Sep. 6, 2024].

[12] Firebase, "*Disaster recovery planning*", Firebase.com. [En línea]. Disponible en <span id="Ref12">https://firebase.google.com/docs/firestore/disaster-recovery</span>. [Accedido: Sep. 6, 2024].

[13] Firebase, "*Automated Backups*", Firebase.com. [En línea]. Disponible en <span id="Ref13">https://firebase.google.com/docs/database/backups</span>. [Accedido: Sep. 6, 2024].

[14] Firebase, "*Extend Realtime Database with Cloud Functions*", Firebase.com. [En línea]. Disponible en <span id="Ref14">https://firebase.google.com/docs/database/extend-with-functions?gen=2nd</span>. [Accedido: Sep. 6, 2024].

***

[Regresar a la página principal](../README.md)
