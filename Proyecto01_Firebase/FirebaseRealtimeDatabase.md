# Firebase Realtime Database

## Actividad para compartir conocimiento

### Autores

Geancarlo Rivera Hernandez C06516

Julio Alejandro Rodr�guez Salguera C16717

## �Y qu� es Firebase?

### Contexto

Firebase Realtime Database es una base de datos NoSQL basada en la nube que sincroniza datos en tiempo real, permitiendo que la informaci�n de los usuarios 
se mantenga actualizada incluso cuando la aplicaci�n se desconecta. Esta base de datos est� alojada en la nube y almacena la informaci�n como documentos JSON, 
adem�s de permitir compartir informaci�n en multiplataforma y realizar actualizaciones de los datos m�s recientes a tiempo real [[D]](#RefD).

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

==**Nota: Esta secci�n se centra en la utilizaci�n de Firebase Realtime Database en aplicaciones web**==

A diferencia de SQL, las bases de datos noSQL no cuentan con una manera estandarizada de modelar, siendo que Firebase Realime Database se basa en un **arbol JSON** [[E]](#RefE).

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
	"usuarios": {
        "usuario1": {
		    "nombre": "Omar",
		    "apellido": "Sanchez"
		    "pais": "Costa Rica",
		    "provincia": "Heredia",
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
  "usuarios": {
    "usuario1": {
      "nombre": "Omar",
		"apellido": "Sanchez"
		"pais": "Costa Rica",
		"provincia": "Heredia",
		"canton": "San Rafael"
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

### Colecciones a nivel de ra�z

Esta es la principal manera de estructurar la informaci�n, siendo que se crean colecciones al nivel de la ra�z de la base de datos que contienen conjuntos 
de informaci�n. Esta es una manera f�cil de crear relaciones muchos a muchos y brinda herramientas para hacer consultas m�s complejas. Pero vuelve m�s compleja 
la base de datos y puede complicar algunos tipos de consultas. Es la manera en la que se recomienda estructurar la informaci�n, pero se debe tener en consideraci�n 
las necesidades y caracter�sticas del sistema, para no a�adir complejidad innecesaria al mismo [[F]](#RefF). 
Un ejemplo de c�mo usar esta estructuraci�n se muestra a continuaci�n:

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

## Referencias

[A] S.Melendez, "*Sometimes You�re Just One Hop From Something Huge*", Fast Company, May 27, 2014. [En l�nea]. Disponible en <span id="RefA"> https://www.fastcompany.com/3031109/sometimes-youre-just-one-hop-from-something-huge </span>

[B] M. Lehenbauer, "*Developers, meet Firebase!*", The Firebase Blog, Abr 12, 2014. [En l�nea]. Disponible en <span id="RefB"> https://firebase.blog/posts/2012/04/developers-meet-firebase </span>

[C] J. Tamplin, "*Firebase is Joining Google!*", The Firebase Blog, Oct 21, 2014. [En l�nea]. Disponible en <span id="RefC"> https://firebase.blog/posts/2014/10/firebase-is-joining-google </span>

[D] Firebase, "*Firebase Realtime Database*", Firebase.com. [En l�nea]. Disponible en <span id="RefD"> https://firebase.google.com/docs/database </span>. [Accedido: Sep. 1, 2024].

[E] Firebase, "*Structure Your Database*", Firebase.com. [En l�nea]. Disponible en <span id="RefE"> https://firebase.google.com/docs/database/web/structure-data </span>. [Accedido: Sep. 1, 2024].

[F] Firebase, "*Choose a data structure*", Firebase.com. [En l�nea]. Disponible en <span id="RefF"> https://firebase.google.com/docs/firestore/manage-data/structure-data </span>. [Accedido: Sep. 1, 2024].