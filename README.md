Plataphorma
===========

Meteor pet project created to teach my students the following Meteor functionality: 

* Collection's publish/subscribe 
* Deps.autorun 
* Meteor.methods/call 
* Integration of non-Meteor code in compatibility folder (HTML5 games Alien Invasion and Froot Wars)
* Usage of allow to control client access to collections

![ScreenShot](/screenshot.png)


Plataphorma offers the possibility to run 2 different HTML5 games: Alien Invasion and Froot Wars. 

On the right side of the screen the best players of each game are shown, updated in real time each time a signed in player finishes a game. If no game is selected, the best players overall are shown.

On the left side of the screen a chatroom for the current game is available. Only signed in users can post messages. If no game is selected a general chatroom is shown.

The original code of the two HTML5 games integrated in this project is available here:
* Alien Invasion: https://github.com/cykod/AlienInvasion
* Froot Wars: http://www.wrox.com/WileyCDA/WroxTitle/Professional-HTML5-Mobile-Game-Development.productCd-1118301323,descCd-DOWNLOAD.html

Bootstrap style (file bootstrap.min.css) provided by http://bootswatch.com


Running the project
-------------------

A live version of this code is running here: http://plataphorma.meteor.com

To run the project locally, clone the repo and run ```meteor``` inside it. You can see in .meteor/packages that this Meteor project uses these packages:
* ```meteor remove autopublish```
* ```meteor remove insecure```
* ```meteor add bootstrap```
* ```meteor add accounts-ui```
* ```meteor add accounts-password```

Preguntas
-------------------
* Click en Boton de Juego
Hay una suscripcion a la collecion de juegos en la línea 11, Meteor.subscribe("all_games");,
en la línea 21  del client.js nos encontramos, var current_game = Session.get("current_game"); dentro del Deps.autorun(), que es una fuente reactiva y que pasa un parametro que es ejecutado cada vez que la variable current_game cambia.
 en la linea 20 del index.html pinchamos en el boton del juego, guardamos el evento y en el client.js dependiendo en que juego hagamos el evento busca en la coleccion de juegos el juego con esa id, var game = Games.findOne({name:"AlienInvasion"});
	Session.set("current_game", game._id);, y nos lo muestra en el container, $('#container').show();

* Se escribe mensaje chat sin estar autenticado
 Al escribir mensajes sin estar autenticado nos aparece este mensaje: You must be signed in to post messages!, codigo que se encuentra en las lineas de la 60 a la 63 del index.html 

al darle al intro y enviar el mensaje, (if (event.which == 13)), meteor ejecuta estas lineas del client.js y busca en la coleccion de usuarios el id del usuario, si no hay usuario, no se publica ningun mensage
 messagesColl.forEach(function(m){
	var userName = Meteor.users.findOne(m.user_id).username;
	messages.push({name: userName , message: m.message});
    });


* Se escribe mensaje chat estando autenticado
 Al escribir mensajes sin estar autenticado nos aparece este mensaje: You must be signed in to post messages!, codigo que se encuentra en las lineas de la 60 a la 63 del index.html 

al darle al intro y enviar el mensaje, meteor ejecuta estas lineas del client.js y busca en la coleccion de usuarios el id del usuario, si no hay usuario, y se publica el mensage.
 messagesColl.forEach(function(m){
	var userName = Meteor.users.findOne(m.user_id).username;
	messages.push({name: userName , message: m.message});
    });
En estas lineas del client.js:
var user_id = Meteor.user()._id;	    
		var message = $('#message');
		if (message.value != '') {
		    Messages.insert({
			user_id: user_id,
			message: message.val(),
			time: Date.now(),
			game_id: Session.get("current_game")
		    });
		    message.val('')
		}
se inserta en el template de usuarios, el nombre del usuario y el mensaje que ha escrito. En la coleccion de mesnsajes guardamos ademas de lo anterior la hora y el id del juego actual. 


* Se termina partida con puntuacion alta estando autenticado.



* Se termina partida con puntuacion alta sin estar autenticado.



* ¿Qué sale en consola cuando te autenticas (Sign in)?
Al entrar, en la consola aparecen dos mensajes:
-"current user: null" 
-"current user: xKoJuboNpAMkg5o49"
Se estan ejecutando las lineas 56-61 del fichero client.js.
En la linea 57 encontramos la funcion reactiva Deps.autorun(), y en la linea 59 encontramos una búsqueda en la colección del usuarios del id del usuario, currentUser = Meteor.userId();.
Cuando hay algun cambio en el id del usuario, Deps.autorun() se hace eco de él y modifica la colección, por lo cual, primero se "pinta" el id anterior del usuario (null), y finalmente se pinta el id actual del usuario ( xKoJuboNpAMkg5o49).


* ¿ Qué sale en consola cuando sales (Sign out)?
Al salir, en la consola aparecen dos mensajes:
-"current user: xKoJuboNpAMkg5o49"
-"current user: null" 
Se estan ejecutando las lineas 56-61 del fichero client.js.
En la linea 57 encontramos la funcion reactiva Deps.autorun(), y en la linea 59 encontramos una búsqueda en la colección del usuarios del id del usuario, currentUser = Meteor.userId();.
Cuando hay algun cambio en el id del usuario, Deps.autorun() se hace eco de él y modifica la colección, por lo cual, primero se "pinta" el id anterior del usuario (xKoJuboNpAMkg5o49), y finalmente se pinta el id actual del usuario (null).
