#ios-parse

#Frameworks Utilizados:

Fabric / Crashlytics.

FBSDKCoreKit.

FBSDKLoginKit.

Alamofire.

CryptoSwift.

Parse.

Google Analytics.


#Uso de Parse Server
Ya que Parse dejará de dar soporte a su servidor en la nube, es necesario contar con nuestro propio Parse Server, para ello, se puede descargar un ejemplo totalmente configurable de:

https://github.com/ParsePlatform/parse-server-example

Este servidor está montado con Node.js y Express, por lo tanto para correrlo localmente se necesitará npm.

El archivo principal de configuración se llama "index.js", los parámetros importantes a modificar son:

Sustituir esta línea:
`var databaseUri = process.env.DATABASE_URI || process.env.MONGODB_URI;`
Por:
`var databaseUri = <RUTA-A-BASE-DE-DATOS>`

*(Ejemplo: "mongodb://username:password@hostname.com:3130/database")*

Para configurar el sevidor se deben de indicar los siguientes valores:

`var api = new ParseServer({

  databaseURI: databaseUri || 'mongodb://localhost:27017/dev',

  cloud: process.env.CLOUD_CODE_MAIN || __dirname + '/cloud/main.js',

  appId: process.env.APP_ID || 'myAppId',

  masterKey: process.env.MASTER_KEY || '', //Add your master key here. Keep it secret!

  serverURL: process.env.SERVER_URL || 'http://localhost:1337/parse',  // Don't forget to change to https if needed

  push: {

		android: {

			senderId: '', // The Sender ID of GCM

			apiKey: '' // The Server API Key of GCM

		},

		ios: {

			pdx: 'certs/mycert.p12', // the path and filename to the .p12 file you exported earlier.

			bundleId: '', // The bundle identifier associated with your app

			production: true

		}

	}
  
});`

donde:

**appId, masterKey, clientKey:** Es el App ID de la aplicación, se genera junto con el Master Key y Client Key.

**serverURL:** Es la URL del servidor Parse.

Para habilitar Push notifications en el servidor se debe de indicar en el Parse Server en la sección "push". Para iOS se debe generar el certificado p12 e indicar el BundleID de la aplicación que indica XCode. El atributo "production" debe estar en false en caso de estar en desarrollo.

El servidor Parse se puede montar en Heroku para hacerlo público, se crea un proyecto en Heroku y se envía el código de Parse Server hacia el repositorio GIT que genera Heroku, al momento de realizar PUSH al repositorio, Heroku automaticamente reconoce que es una aplicación Node.js e inicia el deployment.

Para la base de datos se puede optar por tener un servidor propio o hay alternativas en la nube como mLab.

#Inicio del proyecto con Cocoapods
Al crear un proyecto nuevo, es necesario cerrar XCode, ir a la terminal en el folder de la aplicación y realizar pod init (Si es necesario, realizar gem install cocoapods para instalar Cocoapods). "pod init" genera el archivo Podfile, en ese archivo se debe de indicar los pods que queramos usar, por ejemplo:

`pod 'Alamofire'
pod 'Parse'
pod 'AFNetworking'`

Al guardar nuestro archivo, podemos usar "pod install" o "pod update" en caso de querer usar las últimas versiones de los Pods.

#Estructura del proyecto en XCode

Al abrir el proyecto se pueden ver los siguentes grupos:

* Util: Incluye el archivo Constants.swift donde se indican los valores de la constantes usadas en todo el proyecto.

* Frameworks: Incluye los Frameworks importados manualmente (sin haber sido incluídos en el Podfile).

* Controller: En este grupo tenemos los archivos de los controllers, en este caso ViewController y MainViewController.

* View: En este grupo tenemos los archivos de la vista, las Custom Views, en este caso MaterialTextField, MaterialButton, MaterialView, PostTableViewCell.

* Model: En este grupo se indican los modelos de datos, en este caso Post.swift.

* Plist: Se necesitan dos archivos PList, GoogleService-Info.plist, el cual se baja al momento de crear un proyecto de Google Analytics. Info.plist debe incluír los siguientes parámetros:

Para usar Facebook SDK:
`<key>FacebookAppID</key>
	<string>1073325816125490</string>
	<key>FacebookDisplayName</key>
	<string>InterwareParse</string>
	<key>LSApplicationQueriesSchemes</key>
	<array>
		<string>fbapi</string>
		<string>fb-messenger-api</string>
		<string>fbauth2</string>
		<string>fbshareextension</string>
	</array>`

  Para permitir conexiones inseguras (no https, no recomendable, solo para desarrollo):
  `<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>`

  Para usar Fabric/Crashlytics:
  `<key>Fabric</key>
	<dict>
		<key>APIKey</key>
		<string>97228c34327e77776525c812c5f1dee2d03102d7</string>
		<key>Kits</key>
		<array>
			<dict>
				<key>KitInfo</key>
				<dict/>
				<key>KitName</key>
				<string>Crashlytics</string>
			</dict>
		</array>
	</dict>`

  * Google Analytics es una librería de Objective-C que no puede ser usado directamente con Swift. Para esto se debe tener un archivo "Bridging-Header". La forma más fácil de crear esto es crear un Nuevo Archivo en XCode y seleccionar Objective-C, XCode abrirá una ventana indicando que se va a crear el archivo Bridging-Header, dentro de este archivo se debe indicar:

  `#import <Google/Analytics.h>`
