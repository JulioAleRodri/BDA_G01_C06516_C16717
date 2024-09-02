# Firebase Realtime Database

## Actividad para compartir conocimiento

### Autores

Geancarlo Rivera Hernandez C06516

Julio Alejandro Rodríguez Salguera C16717

## ¿Y qué es Firebase?

### Contexto

Firebase Realtime Database es una base de datos NoSQL basada en la nube que sincroniza datos en tiempo real, permitiendo que la información de los usuarios 
se mantenga actualizada incluso cuando la aplicación se desconecta. Esta base de datos está alojada en la nube y almacena la información como documentos JSON, 
además de permitir compartir información en multiplataforma y realizar actualizaciones de los datos más recientes a tiempo real [[D]](#RefD).

Esta plataforma ofrece las siguientes funcionalidades esenciales:
 
* En tiempo real: Cada vez que la información es cambiada, se actualiza automáticamente para cada uno de los dispositivos conectados en cuestión de milisegundos.
* Funciona sin conexión: No se pierde la información aunque el usuario no tenga conexión, pues los cambios se guardan en el almacenamiento del usuario, permitiendo
que al momento de volver a conectarse, se sincronice toda la información de manera automática.
* Accesible desde dispositivos clientes: Es accesible desde dispositivos móviles o navegadores, sin un servidor de aplicación.
* Escalable entre diferenes bases de datos: Brinda servicios para dividir los datos entre distintas instancias de la bases de datos [[D]](#RefD).

### Historia

Firebase Realtime Database, como bien su nombre indica, pertenece a la plataforma de desarrollo **Firebase** que es parte de Google. Pero ni siempre fue así, 
pues aunque ahora forma parte importante de su ecosistema, esta plataforma nació de la mano de James Tamplin y Andrew Lee, quienes comenzaron en 2008 creando 
una primera aplicación web gratuita llamada SendMeHome.com. El objetivo de esta web estaba alejado de lo que a hoy se dedica la empresa, pues buscaba convertir 
cada objeto en una historia colectiva, creada por todas las personas que interactuaban con ese objeto [[A]](#RefA).

Sin embargo, Tamplin y Lee se encontraron con obstáculos al tratar de implementar un chat en tiempo real que permitiera a los usuarios conversar mientras 
compartían historias sobre los objetos. Ante la falta de una solución adecuada en el mercado de ese momento, optaron por desarrollar su propio sistema de chat. 
Este reto técnico los llevó a un nuevo enfoque empresarial: crear widgets de chats en tiempo real que pudieran integrarse en otros sitios web. Así fue como en 2011 
nació Envolve, una plataforma que ofrecía chats como servicio [[A]](#RefA).

Posteriormente se dieron cuenta de que los usuarios y clientes de Envolve utilizaban Firebase para más que simples chats, pues, por ejemplo, lo usaban para intercambiar información
a tiempo real de videjuegos en linea, como herramienta de colaboración, análisis de productos, entre otros propósitos [[B]](#RefB). En ese momento decidieron crear
Firebase, para que brindara un servicio de base de datos en tiempo real y de esta manera solventar la necesidad que existía en el mercado [[A]](#RefA).

En el año 2014 Firebase fue adquirida por Google, lo que permitió al servicio crecer en su infraestructura y alcance, además de colaborar con **Google Cloud Plataform**, 
lo que les permitió a los usuarios utilizar los servicios en la nube de Google [[C]](#RefC). En retrospectiva, el éxito de Firebase y su servicio de base de datos en 
tiempo real se basó en comprender las necesidades y deseos de los usuarios, aprovechando la oportunidad para satisfacer las demandas del mercado. 
Por eso, si alguna vez observas a los usuarios utilizar un programa de una manera para la cual "no fue diseñado", recuerda esta historia y considera 
cómo podrías ofrecer una solución que responda a esa necesidad.

## ¿Y cómo se modela?

**Nota: Esta sección se centra en la utilización de Firebase Realtime Database en aplicaciones web**

A diferencia de SQL, las bases de datos noSQL no cuentan con una manera estandarizada de modelar, siendo que Firebase Realime Database se basa en un **arbol JSON** [[E]](#RefE).

Aún así, Firebase ofrece tres maneras distintas de estructurar la información en su árbol JSON, cada una con sus propias ventajas, desventajas y casos de uso 
recomendados. En general, el objetivo es crear colecciones grandes y documentos pequeños, ya que esto mejora el rendimiento y cumple con las limitaciones de la 
plataforma [[F]](#RefF). A continuación se presenta las maneras de estructurar los datos:

### Datos anidados en un documento

Los datos de los documentos se pueden guardar en objetos complejos dentro del mismo documento. Esto es muy útil, y hasta necesario, en el caso de tener información 
simple, limitada y que se sabe que no va a crecer en el futuro. Pero justamente por eso es que *no* es escalable, pues si se está guardando objetos complejos, 
muy grandes o que en el futuro pueden crecer sin límite, esto puede afectar la eficiencia y eficacia de las consultas, además de poder llegar a incumplir las 
limitaciones de Firebase. Como se puede ver en el siguiente ejemplo, los datos de los usuarios son fijos y simples, o en otras parabras, pueden cambiar pero no van 
a crecer, y no guardan información o estructuras complejas [[F]](#RefF).

~~~json
{
  "usuarios": {
    "usuario1": {
      "nombre": "Omar",
      "apellido": "Sanchez",
      "pais": "Costa Rica",
      "provincia": "Heredia",
      "canton": "San Rafael"
	}
  }
}
~~~

### Subcolecciones

En este tipo de estructura, se crean subcolecciones de los datos que se pueden expandir con el tiempo. Si se desea ver de otra manera, es como tener bolsas en las 
que se guardan los datos más complejos por cada documento, o en terminos de Programación Orientada a Objetos, como tener un objeto interno que guarda información 
del objeto que lo contiene. Esto tiene la clara ventaja de permitir almacenar información que puede crecer con el tiempo y vincularla con el "nodo padre" del arbol, 
además de que se existen consultas que se pueden utilizar sobre las subcolecciones directamente.

Sin embargo, esta estructura puede dificultar ciertos tipos de generalización y relaciones entre documentos. Por ejemplo, requiere realizar más consultas, 
lo que ralentiza el proceso, y requiere descargar todo el subconjunto de datos, aunque parte de esa información no se vaya a utilizar. Además, 
puede complicar la representación de relaciones de muchos a muchos. Es por estas razones que se recomienda utilizar subcolecciones en los casos en los que 
se guardará información que puede crecer, pero que está relacionada principalmente con el "nodo padre" y, cuando se lean los datos, tenga sentido leer 
todos los datos juntos [[F]](#RefF).

Un ejemplo de la utilización de una subcolección se muestra a continuación:

~~~json
{
  "usuarios": {
    "usuario1": {
      "nombre": "Omar",
      "apellido": "Sanchez",
      "pais": "Costa Rica",
      "provincia": "Heredia",
      "canton": "San Rafael",
      "contactos": {
        "contacto1": {
          "nombre": "Luis",
          "telefono": "+50612345678",
        },
        "contacto2": {
          "nombre": "Maria",
          "telefono": "+50687654321",
        }
      }
    }
  }
}

~~~

### Colecciones a nivel de raíz

Esta es la principal manera de estructurar la información, siendo que se crean colecciones al nivel de la raíz de la base de datos que contienen conjuntos 
de información. Esta es una manera fácil de crear relaciones muchos a muchos y brinda herramientas para hacer consultas más complejas. Pero vuelve más compleja 
la base de datos y puede complicar algunos tipos de consultas. Es la manera en la que se recomienda estructurar la información, pero se debe tener en consideración 
las necesidades y características del sistema, para no añadir complejidad innecesaria al mismo [[F]](#RefF). 
Un ejemplo de cómo usar esta estructuración se muestra a continuación:

~~~json
{
  "usuarios": {
    "usuario1": {
      "nombre": "Omar",
	  "apellido": "Sanchez",
	  "pais": "Costa Rica",
	  "provincia": "Heredia",
      "canton": "San Rafael"
    }
  }
  "chats": {
    "chat1": {
      "creador": "Omar",
      "nombre": "Brave"
      "participantes" {
        "participante1": "Omar"
      }
    }
  }
}

~~~

## ¿Y cómo hago consultas?

**Nota: Esta sección se centra en la utilización de Firebase Realtime Database en aplicaciones web**

De la misma manera en la que no existe un modelo específico para noSQL ni para Firebase Realtime Database, no existe un lenguaje específico u homogeneo 
para realizar consultas, no al menos de la manera en la que estamos acostumbrados con SQL. Además, la técnica o código utilizado para realizar consultas 
puede variar dependiendo de cuál herramienta, tecnología o lenguaje de programación se esté utilizando, siendo que en este caso se darán los ejemplo para 
aplicaciones web, utilizando `javascript`.

### Obtener la referencia a la base de datos

Como esencial primer paso, debemos obtener la referencia a la base de datos que nos permita manipular y realizar consultas en la misma [[G]](#RefG).
Esto se consigue fácilmente de la siguiente manera:

~~~js
import { getDatabase } from "firebase/database";

const MyBeautifulDatabase = getDatabase();
~~~

### Escribir datos

#### Escritura básica

Ahora a lo interesante, para escribir primeramente necesitaremos contar con la referencia a la base de datos y saber que la información se obtiene de manera 
asíncrona, sumado a que se obtiene la información al iniciar y cada vez que este cambia.

La manera más básica para escribir es utilizando la función `set()`, la cuál requiere saber cuál es la ruta dentro del árbol a la colección que deseamos modificar  [[G]](#RefG).
Por ejemplo, si deseamos añadir un nuevo usuario llamado "Omar" con su información a la base de datos, simplemente se puede utilizar el siguiente código:

~~~js
import { getDatabase } from "firebase/database";

const MyDatabase = getDatabase();

set(ref(MyDatabase, 'usuarios/1234567890'), {
  usuario: "Omar",
  apellido: "Sanchez",
  "pais": "Costa Rica"
});
~~~

Es importante destacar que el elemento `usuarios/1234567890` representa la ruta al ID del dato, donde "1234567890" es el identificador que se utiliza para 
interactuar con ese dato en el programa; Siendo que podría ser cualquier valor generado por el programa o definido según lo que se desee. 
Además, se recomienda extraer esta lógica a una función que pueda ser reutilizable.

### Leer datos

#### Escuchar eventos para obtener datos

En este caso se creará un escuchador o "*observer*" que va a obtener los datos de una dirección en 
específico; Siendo que una vez se empieza a escuchar, se realizar un lectura de los datos la primera 
vez y cada vez que la información cambie, por lo que se mantiene la información actualizada. 
Esto se puede realizar por medio de la operación `onValue()` y lo que se obtiene es una "*snapshot*" 
de los datos de la dirección y de todos sus hijos incluidos. En el caso de que la información no exista, 
al realizar la operación `val()`, esta devuelve `null` [[G]](#RefG).

~~~js
import { getDatabase } from "firebase/database";

const MyDatabase = getDatabase();

const lastMesagge = ref(MyDatabase, 'messages/' + someID + '/lastMesagge');

onValue(lastMesagge, (snapshot) => {
  const data = snapshot.val();
  updateLastMesagge(someUser, data);
});
~~~

Nótese que `someID`, `someUser` y `updateLastMessage()` son valores utilizados de ejemplo, el primero 
hace referencia al ID para identificar al usuario en la estructura del árbol, el segundo al propio usuario 
en la aplicación web y el tercero es una función para actualizar el último mensaje del usuario.

Además, se recomienda obtener el nivel más bajo para obtener la información necesaria, dado que se obtiene 
toda la información de los "nodos hijos", lo que podría implicar traer información que no se utilizará 
o descargar una gran cantidad de información innecesaria, por lo que siempre se debe tratar de obtener solo 
la información que se requiera.

#### Leer datos una sola vez

Se puede una única vez la información con la operación `get()`, pero esto no se recomienda, 
dado que aumenta la utilización de la red y disminuye la eficiencia (Además de que sale más caro para la billetera). 
Al utilizar `get()` no se aprovecha las cualidades de respuesta a tiempo real de Firebase 
y no está tan optimizada para funcionar sin no existe conexión [[G]](#RefG).

~~~js
import { getDatabase } from "firebase/database";

const MyDatabase = getDatabase();

get(child(MyDatabase, 'users/' + someID)).then((snapshot) => {
  if(snapshot.exist()) {
    doSomething(snapshot.val());
  } else {
    manageNoExist(snapshot.val());
  }
}).catch((error) => {
  manageError(error);
});
~~~

#### Leer datos una sola vez con un observador

Por otro lado, se puede utilizar la opciones `once()` o `onlyOnce: true` para obtener información que se encuentra 
en el caché, de manera auntomática. Eso implica que, en lugar de obtener la información más reciente 
de la base de datos, se utilizarán la información que se encuentre en la cahé del disco local.

Esto puede ser de utilidad en el caso de que no se requiera los dato más recientes, dado que estos no cambien 
tan frecuentemente o que no se necesiten [[G]](#RefG).

~~~js
import { getDatabase } from "firebase/database";

const MyDatabase = getDatabase();

const lastMesagge = ref(MyDatabase, 'messages/' + someID + '/lastMesagge');

onValue(lastMesagge, (snapshot) => {
  const data = snapshot.val();
  updateLastMesagge(someUser, data);
}, {
  onlyOnce:true
});
~~~

### Modificar o eliminar datos

#### Modificar campos específicos

Para esta sección es importante hacer una diferenciación, la operación `update()` 
modifica el contenido de datos ya existentes, mientras que `push()` agrega nuevos 
elementos a una lista ya existente y devuelve un ID único creado por la aplicación.

Específicamente, `update()` puede modificar los valores de los "nodos hijos" que pertenecen a un nivel 
menor, dada una llave específica que identifica al elemento. Este método puede editar varios elementos 
al mismo tiempo con una sola consulta [[G]](#RefG). A continuación se muestra el funcionamiento de estas dos operaciones:

~~~js
import { getDatabase } from "firebase/database";

function likePost(idUser, idPost, username) {
  const MyDatabase = getDatabase();

  const newPostLike = {
    idPost: idPost,
    idUser: idUser,
    userWhoLikes: username
  };

  const newLikePostKey = push(child(ref(MyDatabase), 'posts')).key;

  const updates = {};
  
  updates['/posts/' + idPost] = newPostLike;
  updates['/users-likes/' + idUser] = newPostLike;

  return update(ref(MyDatabase), updates);
}
~~~

#### Eliminar datos

Se puede eliminar algún dato de manera sencilla simplemente utilizando la operación `remove()`. 
O se puede utiliar el `set()` o `update()` con `null` para sobreescribir el valor, lo cuál puede 
ser útil si se desea eliminar varios "nodos hijos" con una sola llamada.

~~~js
import { getDatabase } from "firebase/database";

const MyDatabase = getDatabase();

const userRef = ref(MyDatabase, 'users/1234567890');

remove(userRef).then(() => {
  console.log("User remove");
}).catch((error) => {
  console.error("Can´t remove user:", error);
});
~~~

## Referencias

[A] S.Melendez, "*Sometimes You're Just One Hop From Something Huge*", Fast Company, May 27, 2014. [En línea]. Disponible en <span id="RefA"> https://www.fastcompany.com/3031109/sometimes-youre-just-one-hop-from-something-huge </span>

[B] M. Lehenbauer, "*Developers, meet Firebase!*", The Firebase Blog, Abr 12, 2014. [En línea]. Disponible en <span id="RefB"> https://firebase.blog/posts/2012/04/developers-meet-firebase </span>

[C] J. Tamplin, "*Firebase is Joining Google!*", The Firebase Blog, Oct 21, 2014. [En línea]. Disponible en <span id="RefC"> https://firebase.blog/posts/2014/10/firebase-is-joining-google </span>

[D] Firebase, "*Firebase Realtime Database*", Firebase.com. [En línea]. Disponible en <span id="RefD"> https://firebase.google.com/docs/database </span>. [Accedido: Sep. 1, 2024].

[E] Firebase, "*Structure Your Database*", Firebase.com. [En línea]. Disponible en <span id="RefE"> https://firebase.google.com/docs/database/web/structure-data </span>. [Accedido: Sep. 1, 2024].

[F] Firebase, "*Choose a data structure*", Firebase.com. [En línea]. Disponible en <span id="RefF"> https://firebase.google.com/docs/firestore/manage-data/structure-data </span>. [Accedido: Sep. 1, 2024].

[G] Firebase, "*Read and Write Data on the Web*", Firebase.com. [En línea]. Disponible en <span id="RefG"> https://firebase.google.com/docs/database/web/read-and-write#web </span>. [Accedido: Sep. 1, 2024].