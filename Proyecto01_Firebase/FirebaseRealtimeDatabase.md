# Firebase Realtime Database

## Actividad para compartir conocimiento

### Autores

Geancarlo Rivera Hernandez C06516

Julio Alejandro Rodríguez Salguera C16717

## �Y qu� es Firebase?

### Contexto

Firebase Realtime Database es una base de datos NoSQL basada en la nube que sincroniza datos en tiempo real, permitiendo que la informaci�n de los usuarios se mantenga actualizada incluso cuando la aplicación se desconecta. Esta base de datos est� alojada en la nube y almacena la informaci�n como documentos JSON, adem�s de permitir compartir informaci�n en multiplataforma y realizar actualizaciones de los datos m�s recientes a tiempo real [[D]](#RefD).

Esta plataforma ofrece las siguientes funcionalidades esenciales:

* En tiempo real: Cada vez que la informaci�n es cambiada, se actualiza autom�ticamente para cada uno de los dispositivos conectados en cuesti�n de milisegundos.
* Funciona sin conexi�n: No se pierde la informaci�n aunque el usuario no tenga conexi�n, pues los cambios se guardan en el almacenamiento del usuario, permitiendo
que al momento de volver a conectarse, se sincronice toda la informaci�n de manera autom�tica.
* Accesible desde dispositivos clientes: Es accesible desde dispositivos m�viles o navegadores, sin un servidor de aplicaci�n.
* Escalable entre diferenes bases de datos: Brinda servicios para dividir los datos entre distintas instancias de la bases de datos [[D]](#RefD).

### Historia

Firebase Realtime Database, como bien su nombre indica, pertenece a la plataforma de desarrollo **Firebase** que es parte de Google. Pero ni siempre fue as�,
pues aunque ahora forma parte importante de su ecosistema, esta plataforma naci� de la mano de James Tamplin y Andrew Lee, quienes comenzaron en 2008 creando
una primera aplicaci�n web gratuita llamada SendMeHome.com. El objetivo de esta web estaba alejado de lo que a hoy se dedica la empresa, pues buscaba convertir
cada objeto en una historia colectiva, creada por todas las personas que interactuaban con ese objeto [[A]](#RefA).

Sin embargo, Tamplin y Lee se encontraron con obst�culos al tratar de implementar un chat en tiempo real que permitiera a los usuarios conversar mientras
compart�an historias sobre los objetos. Ante la falta de una soluci�n adecuada en el mercado de ese momento, optaron por desarrollar su propio sistema de chat.
Este reto t�cnico los llev� a un nuevo enfoque empresarial: crear widgets de chats en tiempo real que pudieran integrarse en otros sitios web. As� fue como en 2011
naci� Envolve, una plataforma que ofrec�a chats como servicio [[A]](#RefA).

Posteriormente se dieron cuenta de que los usuarios y clientes de Envolve utilizaban Firebase para m�s que simples chats, pues, por ejemplo, lo usaban para intercambiar informaci�n
a tiempo real de videjuegos en linea, como herramienta de colaboraci�n, an�lisis de productos, entre otros prop�sitos [[B]](#RefB). En ese momento decidieron crear
Firebase, para que brindara un servicio de base de datos en tiempo real y de esta manera solventar la necesidad que exist�a en el mercado [[A]](#RefA).

En el a�o 2014 Firebase fue adquirida por Google, lo que permiti� al servicio crecer en su infraestructura y alcance, adem�s de colaborar con **Google Cloud Plataform**,
lo que les permiti� a los usuarios utilizar los servicios en la nube de Google [[C]](#RefC). En retrospectiva, el �xito de Firebase y su servicio de base de datos en
tiempo real se bas� en comprender las necesidades y deseos de los usuarios, aprovechando la oportunidad para satisfacer las demandas del mercado.
Por eso, si alguna vez observas a los usuarios utilizar un programa de una manera para la cual "no fue dise�ado", recuerda esta historia y considera
c�mo podr�as ofrecer una soluci�n que responda a esa necesidad.

## �Y c�mo se modela?

**Nota:** Esta secci�n se centra en la utilizaci�n de Firebase Realtime Database en aplicaciones web.

A diferencia de SQL, las bases de datos noSQL no cuentan con una manera estandarizada de modelar, siendo que Firebase Realime Database se basa en un **árbol JSON** [[E]](#RefE).

A�n as�, Firebase ofrece tres maneras distintas de estructurar la informaci�n en su �rbol JSON, cada una con sus propias ventajas, desventajas y casos de uso
recomendados. En general, el objetivo es crear colecciones grandes y documentos peque�os, ya que esto mejora el rendimiento y cumple con las limitaciones de la
plataforma [[F]](#RefF). A continuaci�n se presenta las maneras de estructurar los datos:

### Datos anidados en un documento

Los datos de los documentos se pueden guardar en objetos complejos dentro del mismo documento. Esto es muy �til, y hasta necesario, en el caso de tener informaci�n
simple, limitada y que se sabe que no va a crecer en el futuro. Pero justamente por eso es que *no* es escalable, pues si se est� guardando objetos complejos,
muy grandes o que en el futuro pueden crecer sin l�mite, esto puede afectar la eficiencia y eficacia de las consultas, adem�s de poder llegar a incumplir las
limitaciones de Firebase. Como se puede ver en el siguiente ejemplo, los datos de los usuarios son fijos y simples, o en otras parabras, pueden cambiar pero no van
a crecer, y no guardan informaci�n o estructuras complejas [[F]](#RefF).

~~~json
{
  "users": {
    "users1": {
      "name": "Omar",
      "lastName": "Sanchez",
      "country": "Costa Rica",
      "province": "Heredia",
      "canton": "San Rafael"
    }
  }
}
~~~

### Subcolecciones

En este tipo de estructura, se crean subcolecciones de los datos que se pueden expandir con el tiempo. Si se desea ver de otra manera, es como tener bolsas en las
que se guardan los datos m�s complejos por cada documento, o en terminos de Programaci�n Orientada a Objetos, como tener un objeto interno que guarda informaci�n
del objeto que lo contiene. Esto tiene la clara ventaja de permitir almacenar informaci�n que puede crecer con el tiempo y vincularla con el "nodo padre" del arbol,
adem�s de que se existen consultas que se pueden utilizar sobre las subcolecciones directamente.

Sin embargo, esta estructura puede dificultar ciertos tipos de generalizaci�n y relaciones entre documentos. Por ejemplo, requiere realizar m�s consultas,
lo que ralentiza el proceso, y requiere descargar todo el subconjunto de datos, aunque parte de esa informaci�n no se vaya a utilizar. Adem�s,
puede complicar la representaci�n de relaciones de muchos a muchos. Es por estas razones que se recomienda utilizar subcolecciones en los casos en los que
se guardar� informaci�n que puede crecer, pero que est� relacionada principalmente con el "nodo padre" y, cuando se lean los datos, tenga sentido leer
todos los datos juntos [[F]](#RefF).

Un ejemplo de la utilizaci�n de una subcolecci�n se muestra a continuaci�n:

~~~json
{
  "users": {
    "users1": {
      "name": "Omar",
      "lastName": "Sanchez",
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

### Colecciones a nivel de ra�z

Esta es la principal manera de estructurar la informaci�n, siendo que se crean colecciones al nivel de la ra�z de la base de datos que contienen conjuntos
de informaci�n. Esta es una manera f�cil de crear relaciones muchos a muchos y brinda herramientas para hacer consultas m�s complejas. Pero vuelve m�s compleja
la base de datos y puede complicar algunos tipos de consultas. Es la manera en la que se recomienda estructurar la informaci�n, pero se debe tener en consideraci�n
las necesidades y caracter�sticas del sistema, para no a�adir complejidad innecesaria al mismo [[F]](#RefF).
Un ejemplo de c�mo usar esta estructuraci�n se muestra a continuaci�n:

~~~json
{
  "users": {
    "users1": {
      "name": "Omar",
      "lastName": "Sanchez",
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

## �Y c�mo hago consultas?

**Nota:** Esta secci�n se centra en la utilizaci�n de Firebase Realtime Database en aplicaciones web

De la misma manera en la que no existe un modelo espec�fico para noSQL ni para Firebase Realtime Database, no existe un lenguaje espec�fico u homogéneo
para realizar consultas, no al menos de la manera en la que estamos acostumbrados con SQL. Adem�s, la t�cnica o c�digo utilizado para realizar consultas
puede variar dependiendo de cu�l herramienta, tecnolog�a o lenguaje de programaci�n se est� utilizando, siendo que en este caso se dar�n los ejemplo para
aplicaciones web, utilizando `javascript`.

### Obtener la referencia a la base de datos

Como esencial primer paso, debemos obtener la referencia a la base de datos que nos permita manipular y realizar consultas en la misma [[G]](#RefG).
Esto se consigue f�cilmente de la siguiente manera:

~~~js
import { getDatabase } from "firebase/database";

const MyBeautifulDatabase = getDatabase();
~~~

### Escribir datos

#### Escritura b�sica

Ahora a lo interesante, para escribir primeramente necesitaremos contar con la referencia a la base de datos y saber que la informaci�n se obtiene de manera
as�ncrona, sumado a que se obtiene la informaci�n al iniciar y cada vez que este cambia.

La manera m�s b�sica para escribir es utilizando la funci�n `set()`, la cu�l requiere saber cu�l es la ruta dentro del �rbol a la colecci�n que deseamos modificar  [[G]](#RefG).
Por ejemplo, si deseamos a�adir un nuevo usuario llamado "Omar" con su informaci�n a la base de datos, simplemente se puede utilizar el siguiente c�digo:

~~~js
import { getDatabase, ref, set } from "firebase/database";

const MyDatabase = getDatabase();

set(ref(MyDatabase, 'usuarios/1234567890'), {
  usuario: "Omar",
  apellido: "Sanchez",
  "pais": "Costa Rica"
});
~~~

Es importante destacar que el elemento `usuarios/1234567890` representa la ruta al ID del dato, donde "1234567890" es el identificador que se utiliza para
interactuar con ese dato en el programa; Siendo que podr�a ser cualquier valor generado por el programa o definido seg�n lo que se desee. 
Adem�s, se recomienda extraer esta l�gica a una funci�n que pueda ser reutilizable.

### Leer datos

#### Escuchar eventos para obtener datos

En este caso se crear� un escuchador o "*observer*" que va a obtener los datos de una direcci�n en
espec�fico; Siendo que una vez se empieza a escuchar, se realizar un lectura de los datos la primera
vez y cada vez que la informaci�n cambie, por lo que se mantiene la informaci�n actualizada.
Esto se puede realizar por medio de la operaci�n `onValue()` y lo que se obtiene es una "*snapshot*"
de los datos de la direcci�n y de todos sus hijos incluidos. En el caso de que la informaci�n no exista,
al realizar la operaci�n `val()`, esta devuelve `null` [[G]](#RefG).

~~~js
import { getDatabase, ref, onValue } from "firebase/database";

const MyDatabase = getDatabase();

const lastMesagge = ref(MyDatabase, 'messages/' + someID + '/lastMesagge');

onValue(lastMesagge, (snapshot) => {
  const data = snapshot.val();
  updateLastMesagge(someUser, data);
});
~~~

N�tese que `someID`, `someUser` y `updateLastMessage()` son valores utilizados de ejemplo, el primero
hace referencia al ID para identificar al usuario en la estructura del �rbol, el segundo al propio usuario
en la aplicaci�n web y el tercero es una funci�n para actualizar el �ltimo mensaje del usuario.

Adem�s, se recomienda obtener el nivel m�s bajo para obtener la informaci�n necesaria, dado que se obtiene
toda la informaci�n de los "nodos hijos", lo que podr�a implicar traer informaci�n que no se utilizar�
o descargar una gran cantidad de informaci�n innecesaria, por lo que siempre se debe tratar de obtener solo
la informaci�n que se requiera.

#### Leer datos una sola vez

Se puede una �nica vez la informaci�n con la operaci�n `get()`, pero esto no se recomienda,
dado que aumenta la utilizaci�n de la red y disminuye la eficiencia (Adem�s de que sale m�s caro para la billetera).
Al utilizar `get()` no se aprovecha las cualidades de respuesta a tiempo real de Firebase
y no est� tan optimizada para funcionar sin no existe conexi�n [[G]](#RefG).

~~~js
import { getDatabase, ref, child, get } from "firebase/database";

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

Por otro lado, se puede utilizar la opciones `once()` o `onlyOnce: true` para obtener informaci�n que se encuentra
en el cach�, de manera auntom�tica. Eso implica que, en lugar de obtener la informaci�n m�s reciente
de la base de datos, se utilizar�n la informaci�n que se encuentre en la cah� del disco local.

Esto puede ser de utilidad en el caso de que no se requiera los dato m�s recientes, dado que estos no cambien
tan frecuentemente o que no se necesiten [[G]](#RefG).

~~~js
import { getDatabase, ref, onValue } from "firebase/database";

const MyDatabase = getDatabase();

const lastMesagge = ref(MyDatabase, 'messages/' + someID + '/lastMesagge');

onValue(lastMesagge, (snapshot) => {
  const data = snapshot.val();
  updateLastMesagge(someUser, data);
}, {
  onlyOnce: true
});
~~~

### Modificar o eliminar datos

#### Modificar campos espec�ficos

Para esta secci�n es importante hacer una diferenciaci�n, la operaci�n `update()`
modifica el contenido de datos ya existentes, mientras que `push()` agrega nuevos
elementos a una lista ya existente y devuelve un ID �nico creado por la aplicaci�n.

Espec�ficamente, `update()` puede modificar los valores de los "nodos hijos" que pertenecen a un nivel
menor, dada una llave espec�fica que identifica al elemento. Este m�todo puede editar varios elementos
al mismo tiempo con una sola consulta [[G]](#RefG). A continuaci�n se muestra el funcionamiento de estas dos operaciones:

~~~js
import { getDatabase, ref, child, push, update } from "firebase/database";

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

Se puede eliminar alg�n dato de manera sencilla simplemente utilizando la operaci�n `remove()`.
O se puede utiliar el `set()` o `update()` con `null` para sobreescribir el valor, lo cu�l puede
ser �til si se desea eliminar varios "nodos hijos" con una sola llamada  [[G]](#RefG).

~~~js
import { getDatabase, ref, remove } from "firebase/database";

const MyDatabase = getDatabase();

const userRef = ref(MyDatabase, 'users/1234567890');

remove(userRef).then(() => {
  console.log("User remove");
}).catch((error) => {
  console.error("Can�t remove user:", error);
});
~~~

## �Existen estructuras o algoritmos?

Como se mencion� anteriormente, la estructura utilizada en Firebase Realtime Database es un *�rbol JSON*,
por lo que las relaciones que se crean son de "padre-hijo". De manera general, Firebase ofrece menos
opciones de estructuras y algoritmos en comparaci�n con una base de datos relacional, pero s� existen
opciones interesantes

### Listas

Como su nombre indica, son una colecci�n de datos relacionados a una referencia "padre"; Lo interesante
de esta estructura es que permite agregar nuevos datos utilizando la operaci�n `push()`, que genera una llave
�nica para cada uno de los "hijos" agregados a la referencia. La operaci�n `push()` permite a varios usuarios
agregar datos de forma **paralela**, por lo que estas listas se pueden utilizar para datos que son modificados
constantemente por los usuarios [[H]](#RefH).

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

De la misma manera que existen "*triggers*" en las bases de datos relacionales, Firebase ofrece la opci�n
de escuchar eventos de los nodos hijos y reaccionar ante estos. Por ejemplo, si se agrega, elimina o modifica
un nodo hijo, se puede reaccionar cambiando la interfaz gr�fica para que este cambio se refleje para el usuario [[H]](#RefH).

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

### Ordenadamiento y filtrado

#### Ordenadamiento

Se pueden realizar consultas ordenando los resultados dado una llave, un valor o hasta el valor de un nodo hijo,
utilizando las opciones de `orderByKey()`, `orderByValue()` y `orderByChild()` [[H]](#RefH).

~~~js
import { getDatabase, ref, query, orderByValue } from "firebase/database";

const MyDatabase = getDatabase();
  
const userMessages = query(ref(MyDatabase, 'user-messages/1234567890'), orderByValue('message-date'));
~~~

#### Fitrado

En este caso se puede realizar un filtrado para solamente tomar los datos que cumplan con ciertas
caracter�sticas, adem�s de que se puede combinar con el ordenamiento [[H]](#RefH). A continuaci�n se presentan
algunas de las opciones para realizar el filtrado de datos:

* `limitToFirst()`: Limita la cantidad de objetos obtenidos desde el inicio de la lista de resultados
* `limitToLast()`: Limita la cantidad de objetos obtenidos desde el final de la lista de resultados
* `startAt()` y `startAfter()`: El primero devuelve elementos mayores o iguales al valor especificado,
y el segundo devuelve elementos mayores, ambos seg�n el m�todo de ordenaci�n elegido.
* `endAt()` y `endBefore()`: El primero devuelve elementos menores o iguales al valor especificado,
y el segundo devuelve elementos menores, ambos seg�n el m�todo de ordenaci�n elegido.
* `equalTo()`: Devuelve una lista de elemento cuto valor sea igual al valor espec�fico dado.

~~~js
import { getDatabase, ref, query, orderByValue } from "firebase/database";

const MyDatabase = getDatabase();
  
const userMessages = query(ref(MyDatabase, 'user-messages/1234567890'), orderByValue('message-date'), limitToLast(25));
~~~

## Referencias

[A] S.Melendez, "*Sometimes You're Just One Hop From Something Huge*", Fast Company, May 27, 2014. [En l�nea]. Disponible en <span id="RefA"> https://www.fastcompany.com/3031109/sometimes-youre-just-one-hop-from-something-huge </span>

[B] M. Lehenbauer, "*Developers, meet Firebase!*", The Firebase Blog, Abr 12, 2014. [En l�nea]. Disponible en <span id="RefB"> https://firebase.blog/posts/2012/04/developers-meet-firebase </span>

[C] J. Tamplin, "*Firebase is Joining Google!*", The Firebase Blog, Oct 21, 2014. [En l�nea]. Disponible en <span id="RefC"> https://firebase.blog/posts/2014/10/firebase-is-joining-google </span>

[D] Firebase, "*Firebase Realtime Database*", Firebase.com. [En l�nea]. Disponible en <span id="RefD"> https://firebase.google.com/docs/database </span>. [Accedido: Sep. 1, 2024].

[E] Firebase, "*Structure Your Database*", Firebase.com. [En l�nea]. Disponible en <span id="RefE"> https://firebase.google.com/docs/database/web/structure-data </span>. [Accedido: Sep. 1, 2024].

[F] Firebase, "*Choose a data structure*", Firebase.com. [En l�nea]. Disponible en <span id="RefF"> https://firebase.google.com/docs/firestore/manage-data/structure-data </span>. [Accedido: Sep. 1, 2024].

[G] Firebase, "*Read and Write Data on the Web*", Firebase.com. [En l�nea]. Disponible en <span id="RefG"> https://firebase.google.com/docs/database/web/read-and-write#web </span>. [Accedido: Sep. 1, 2024].

[H] Firebase, "*Work with Lists of Data on the Web*", Firebase.com. [En línea]. Disponible en: [https://firebase.google.com/docs/database/web/lists-of-data#web_4](https://firebase.google.com/docs/database/web/lists-of-data#web_4). [Accedido: Sep. 4, 2024].
