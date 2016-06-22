# Estándares de programación en iOS
Arquitectura de código
--------------------
La estructura de directorios sigue la siguiente forma:

> |--AppName
> |.......|--AppDelegate.swift<br>
> |.......|--UI<br>
> |.............|--NameOfSection<br>
> |......................|--____ViewController.swift<br>
> |......................|--____View.swift<br>
> |......................|--____Cell.swift<br>
> |.............|--Common<br>
> |......................|--____ViewController.swift<br>
> |......................|--____View.swift<br>
> |......................|--____Cell.swift<br>
>  |.......|--Storyboards<br>
>  |..............|--**.storyboard<br>
>  |.......|--Networking<br>
>  |..............|--____NetworkingManager.swift<br>
>  |.......|--Model<br>
>  |.......|--Resources<br>
>  |.......|--Util<br>
>  |--AppNameTests
>  |--AppNameUITests

|  Directorio  |  Descripción  |
|---|---|
|AppName|Contiene todos los archivos del proyecto.|
|UI|Contiene Todas las secciones del proyecto y dentro de cada sección se encuentran los archivos que tienen que ver con la vista (ViewControllers, Views, Cells, etc.). <br>Si existen clases que se ocupan en diferentes secciones se colocan en la caperta Common. <br>Estos archivos siguen la siguiente convención:<br>* xxxxViewController<br>* xxxxView <br>*xxxxCell|
|Storyboards|Contiene los storyboards del proyecto.|
|Networking|Contiene una clase que expone métodos estáticos para realizar las llamadas a servicios REST. Todos los servicios se consumen con la librería Alamofire.|
|Model|Contiene los modelos y schemas, la convención de nombres es siempre en singular.|
|Util| Contienen archivos de fuentes, assets, archivos de texto y cualquier recurso que utilice el proyecto.|
|AppNameTests|Contiene las pruebas unitarias de código.|
|AppNameUITests|Contiene las pruebas unitarias de Interfaz gráfica.|
Dónde xxx es un nombre en inglés que describe el contenido del código. 


## Estándares de codificación ##

**Convención de nombres**

- Para propiedades, variables y constantes. Utilizar el prefijo de acuerdo al tipo de componente. Por ejemplo:
 - lbl para Lables
 - tv para TextViews
 - tf para TextFields
 - btn para Buttons
- Usar nombres descriptivos en camel case para métodos, variables, clases, etc. Las clases, estructuras, enumeraciones y protocolos deben tener capitalizada la primera letra.

Buena idea:

    private let maximumWidgetCount = 100
    class WidgetContainer {
	 var widgetButton: UIButton
	 let widgetHeightPercentage = 0.85
	}

Mala idea:

    private let MAX_WIDGET_COUNT = 100
    class app_widgetContainer {
	 var wBut: UIButton
	 let wHeightPctg = 0.85
	}

Las abreviaciones deben ser evitadas y en caso de existir deben estar todas en mayúsculas o minúsculas según corresponda.

Buena idea:

    let urlString: URLString
    let userId: UserID

Mala idea:

    let uRlSTring: UrlString
    let userId: UserId

Para funciones y métodos init utilizar parámetros nombrados al menos que el contexto sea muy claro. Incluir nombres de parámetros externos si esto hace que la función sea más legible.

    func dateFromString(date: String) -> NSDate
    func convertPointAt(column column: Int, row: Int) -> CGPoint
    
    //would be called like this
    dateFromString("2014-03-08")
    convertPointAt(column:42, row:12)

Para los métodos seguir el estándar de Apple de hacer referencia al primer parámetro en el nombre del método.

    class Counter {
	    func combineWith(otherCounter: Counter, options: Dictionary?) { ... }
	    func incrementBy(amount: Int) { ... }

Usar lowerCamelCase para enums.

    enum Shape {
	    case rectangle
	    case triangle
	    case circle
	    case square
    }

 Organiza el código con: //MARK:  -
 Utiliza las extensiones para implementar los protocolos

Buena idea:

    class MyViewController: UIViewController {
	    //class stuff here
    }
    
    // MARK: - UITableViewDataSource
    extension MyViewController: UITableViewDataSource {
	    //table view data source methods
	}
	
	// MARK: - UIScrollViewDelegate
    extension MyViewController: UIScrollViewDelegate {
	    //scroll view delegate methods
	}

Mala idea:

    class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
	    //all methods
    }

 - Utilizar 2 espacios para identar el código. 
 - No utilizar self al menos que sirva para diferenciar una propiedad de una variable. 
 - Marcar clase como final cuando no se planea heredar de ella.

**UI**
<br>Para el manejo de interfaces en iOS, se utilizará una combinación de Storyboards y vistas completamente programáticas. La estructura inicial se realizará en storyboards y para vistas reusables o complejas se codificarán las vistas.
Se utilizará Autolayout para el acomodo de los elementos.

**Manejo de dependencias**
<br>Se utilizará cocoapods como manejador de dependencias

**Librerías**

 - Alamofire para realizar llamadas a los servicios web. 
 - DateTools para manipular fechas.
 - SwiftyJSON para parsear y manejar json en swift.

**Patrones de comunicación**
<br>En iOS existen varias maneras de notificar a otros componentes:

 - Delegados: Se utiliza para comunicar información hacia atrás,   
   normalmente de vistas a controladores. Sirve para comunicación uno a 
   uno.
 - Bloques: Permite un código más desacoplado  y mayor legibilidad. Sirve para comunicación uno a uno.
 - Notification Center: Permite comunicarte de uno a muchos, la ventaja de esta forma de comunicación es que es altamente   
   desacoplado.

**Convención de nombres**
 - Capitalizar las constantes: CONSTANT 
 - Para los acrónimos seguir de manera estricta la notación de camello, es decir  UnitId en lugar de UnitID
 - Anteponer el tipo de objeto, por ejemplo para un TextView
   poner tvName

**Constantes**
<br>El número de constantes en el proyecto se tiene que mantener lo más corto posible, cuando son variables a utilizar solo dentro de una clase, deben vivir en dicha clase. Utilizar un archivo llamado Constants.swift y dentro del archivo declarar estructuras. Como se muestra a continuación:

    struct Config {
	    static let baseURL = NSURL(string: "http://google.com")!
	    static let name = "Sergio"
	}
	
	struct Color {
		static primaryColor = UIColor.redColor()
		static secondaryColor = UIColor.whiteColor()
	}


**Assets**
<br>Utilizar el archivo de assets para administrar los assets del proyecto, proporcionar la medida más grande de la imagen (@3x) y dejar que el sistema escale a la imagen requerida. En caso de que la imagen no se ajuste bien, proporcionar los tamaños requeridos.
En medida de lo posible incorporar imágenes como vectores gráficos (PDF)

**Let en lugar de Var**
<br>Utilizar siempre let si el valor no cambiará, cuando se sabe que el valor va a cambiar utilizar var. Es una práctica común el definir todo como let y cambiarlo a var cuando el compilador mande error.

**Sintaxis**
<br>Es preferible utilizar la sintaxis recortada en lugar de la genérica.

Buena idea:

    var deviceModels: [Stirng]
    var employees: [Int: String]
    var faxNumber: Int?

Mala idea:

    var deviceModels: Array<Stirng>
    var employees: Dictionary<Int: String>
    var faxNumber: Optional<Int>

**Paréntesis**
<br>En condicionales, evitar el uso de paréntesis puesto que ya no son necesarios.

Buena idea:

    if name == "Hello" {
	    print("Hello!")
	}

Mala idea:

    if (name == "Hello") {
	    print("Hello!")
	}
