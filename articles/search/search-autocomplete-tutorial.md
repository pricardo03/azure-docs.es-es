---
title: Tutorial para agregar la función Autocompletar al cuadro de búsqueda con Azure Search | Microsoft Docs
description: Ejemplos de cómo mejorar la experiencia del usuario final de las aplicaciones centradas en los datos con las API de sugerencias y la función Autocompletar de Azure Search.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: mcarter
ms.openlocfilehash: 7120080bfdc188c150c7065e1c0639ab8c04f173
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989693"
---
# <a name="tutorial-add-auto-complete-to-your-search-box-using-azure-search"></a>Tutorial: Incorporación de la función Autocompletar el cuadro de búsqueda con Azure Search

En este tutorial, obtendrá información sobre cómo usar las [sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions), la función [Autocompletar](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete) y las [facetas](search-faceted-navigation.md) en la [API de REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/) y el [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) para crear un cuadro de búsqueda eficaz. Las *sugerencias* proporcionan recomendaciones de resultados reales que se basen en lo que el usuario ha escrito hasta el momento. La función *Autocompletar*, [una característica de versión preliminar nueva](search-api-preview.md) de Azure Search, proporciona términos del índice para completar lo que el usuario está escribiendo actualmente. Compararemos varias técnicas para mejorar la productividad del usuario y encontraremos rápida y fácilmente lo que buscan mediante la incorporación de la eficacia de la búsqueda directamente al usuario mientras escribe.

Este tutorial lo guiará por una aplicación basada en ASP.NET MVC que usa C# para llamar a las [bibliotecas cliente de SDK de Azure Search](https://aka.ms/search-sdk) y JavaScript para llamar directamente la API de REST de Azure Search. La aplicación de este tutorial está dirigida a un índice que se rellena con los datos de ejemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). Puede usar el índice ya configurado en la demostración NYCJobs o rellenar su propio índice con un cargador de datos en la solución de ejemplo NYCJobs. En el ejemplo se usan las bibliotecas [jQuery UI](https://jqueryui.com/autocomplete/) y [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) de JavaScript para crear un cuadro de búsqueda que admite la función Autocompletar. Si usa estos componentes junto con Azure Search, verá varios ejemplos de cómo admitir la función Autocompletar para anticipar la escritura en el cuadro de búsqueda.

Realizará las tareas siguientes:

> [!div class="checklist"]
> * Descargar y configurar la solución
> * Agregar información del servicio de búsqueda a la configuración de la aplicación
> * Implementar un cuadro de entrada de búsqueda
> * Agregar compatibilidad para una lista de Autocompletar que extrae información de un origen remoto 
> * Recuperar las sugerencias y la función Autocompletar con la API de REST y el SDK de .NET
> * Admitir el almacenamiento en caché del lado cliente para mejorar el rendimiento 

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2017. Puede usar la [edición gratuita de Visual Studio 2017 Community](https://www.visualstudio.com/downloads/). 

* Descargue el [código fuente](https://github.com/azure-samples/search-dotnet-getting-started) de ejemplo para el tutorial.

* (Opcional) Una cuenta activa de Azure y un servicio de Azure Search. Si no tiene una cuenta de Azure, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/). Para obtener ayuda con el proceso de aprovisionamiento de servicios, consulte [Creación de un servicio Azure Search en el portal](search-create-service-portal.md). La cuenta y el servicio son opcionales, porque este tutorial se puede completar con un índice de NYCJobs hospedado que ya existe para otra demostración.

* (Opcional) Descargue el código de ejemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) para importar los datos NYCJobs a un índice de su propio servicio de Azure Search.

> [!Note]
> Si va a usar el servicio gratuito de Azure Search, está limitado a tres índices. El cargador de datos de NYCJobs crea dos índices. Asegúrese de que haya espacio en el servicio para aceptar los nuevos índices.

### <a name="set-up-azure-search-optional"></a>Configuración de Azure Search (opcional)

Siga los pasos de esta sección si desea importar los datos para la aplicación de ejemplo NYCJobs a su propio índice. Este paso es opcional.  Si quiere usar el índice de ejemplo que se proporciona, vaya directamente a la sección siguiente al ejecutar el ejemplo.

1. En la carpeta DataLoader del código de ejemplo NYCJobs, abra el archivo de solución DataLoader.sln en Visual Studio.

1. Actualice la información de la conexión del servicio Azure Search.  Abra el archivo App.config dentro del proyecto DataLoader y cambie los valores de appSettings TargetSearchServiceName y TargetSearchServiceApiKey para reflejar el servicio Azure Search y la clave de API de Azure Search Service.  Puede encontrarlos en Azure Portal.

1. Presione F5 para iniciar la aplicación.  Con esto se crearán dos índices y se importarán los datos de ejemplo de NYCJob.

1. En el código de ejemplo del tutorial, abra el archivo de solución AutocompleteTutorial.sln en Visual Studio.  Abra Web.config dentro del proyecto AutocompleteTutorial y cambie los valores de SearchServiceName y SearchServiceApiKey a los mismos valores anteriores.

### <a name="running-the-sample"></a>Ejecución del ejemplo

Ahora está preparado para ejecutar la aplicación de ejemplo del tutorial.  Abra el archivo de solución AutocompleteTutorial.sln en Visual Studio para ejecutar el tutorial.  La solución contiene un proyecto de ASP.NET MVC.  Presione F5 para ejecutar el proyecto y cargar la página en el explorador de su preferencia.  En la parte superior, verá una opción para seleccionar C# o JavaScript.  La opción de C# llama a HomeController desde el explorador y usa el SDK de .NET de Azure Search para recuperar los resultados.  La opción de JavaScript llama a la API de REST de Azure Search directamente desde el explorador.  Por lo general, esta opción tendrá un rendimiento considerablemente mejor porque quita el controlador del flujo.  Puede elegir la opción que mejor se adapte a sus necesidades y a sus preferencias de lenguaje.  En la página aparecen varios ejemplos de la función Autocompletar con ayuda para cada uno de ellos.  En cada ejemplo hay texto de ejemplo recomendado que puede probar.  Para probar, empiece a escribir algunas letras en cada cuadro de búsqueda para ver lo que sucede.

## <a name="how-this-works-in-code"></a>Cómo funciona esto en el código

Ahora que vio los ejemplos en el explorador, analicemos detalladamente el primer ejemplo para revisar los componentes implicados y cómo funcionan.

### <a name="search-box"></a>Cuadro de búsqueda

Para cada opción de lenguaje, el cuadro de búsqueda es exactamente el mismo.  Abra el archivo Index.cshtml en la carpeta \Views\Home. El cuadro de búsqueda es sencillo:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Se trata de un cuadro de texto de entrada simple con una clase para establecer el estilo y un identificador al que JavaScript hará referencia, además de un texto de marcador de posición.  La magia está en el código JavaScript.

### <a name="javascript-code-c"></a>Código JavaScript (C#)

El ejemplo del lenguaje C# usa JavaScript en Index.cshtml para aprovechar la biblioteca de la función Autocompletar de jQuery UI.  Esta biblioteca agrega la experiencia de la función Autocompletar al cuadro de búsqueda mediante la realización de llamadas asincrónicas al controlador de MVC para recuperar las recomendaciones.  Veamos el código de JavaScript del primer ejemplo:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Este código se ejecuta en el explorador en la carga de página para configurar la función Autocompletar para el cuadro de entrada de "example1a".  `minLength: 3` garantiza que la recomendación solo se mostrará cuando haya al menos tres caracteres en el cuadro de búsqueda.  El valor de origen es importante:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Esta línea indica a la API de la función Autocompletar dónde obtener la lista de elementos para mostrar debajo del cuadro de búsqueda.  Debido a que se trata de un proyecto de MVC, llama a la función Sugerir en HomeController.cs.  Lo examinaremos más detalladamente en la sección siguiente.  También pasa algunos parámetros para controlar un resaltado, una coincidencia aproximada o un término.  La API de JavaScript de la función Autocompletar agrega el parámetro de término.

#### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Ampliación del ejemplo para admitir la coincidencia aproximada

La búsqueda aproximada permite obtener resultados basados en coincidencias cercanas incluso si el usuario escribe incorrectamente una palabra en el cuadro de búsqueda.  Para probar esto, cambiemos la línea de asunto para habilitar la coincidencia aproximada.

Cambie la línea siguiente de esto:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

a esto:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

Presione F5 para iniciar la aplicación.

Pruebe escribir algo como "ejecativo" y observe cómo aparecen resultados para "ejecutivo", incluso si no son coincidencia exacta de las letras que escribió.

### <a name="homecontrollercs-c"></a>HomeController.cs (C#)

Ahora que ya revisamos el código de JavaScript del ejemplo, veamos el código de control de C# que recupera realmente las recomendaciones mediante el SDK de .NET de Azure Search.

1. Abra el archivo HomeController.cs en el directorio Controllers (Controladores). 

1. Lo primero que notará es un método en la parte superior de la clase con el nombre InitSearch.  Esto crea un cliente del índice HTTP autenticado en el servicio Azure Search.  Si desea más información sobre cómo funciona esto, consulte el tutorial siguiente: [Cómo usar Azure Search desde una aplicación .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

1. Vaya a la función Sugerir.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

La función Sugerir toma dos parámetros que determinar si se devuelven resaltados de aciertos o si se usa la coincidencia aproximada además de la entrada del término de búsqueda.  El método crea un objeto SuggestParameters que luego se pasa a la API de la función Sugerir. Luego el resultado se convierte en JSON para poder mostrarlo en el cliente.
(Opcional) Agregue un punto de interrupción al inicio de la función Sugerir y recorra el código.  Observe la respuesta que devuelve el SDK y cómo se convierte en el resultado que se devuelve desde el método.

Los otros ejemplos de la página siguen el mismo patrón para agregar resaltado de aciertos, la anticipación de la escritura para las recomendaciones de la función Autocompletar y las facetas para admitir el almacenamiento en caché del lado cliente de los resultados de la función Autocompletar.  Revise cada uno de estos elementos para saber cómo funcionan y cómo puede aprovecharlos en la experiencia de búsqueda.

### <a name="javascript-language-example"></a>Ejemplo de lenguaje JavaScript

Para el ejemplo de lenguaje JavaScript, el código JavaScript en la página IndexJavaScript.cshtml aprovecha la función Autocompletar de jQuery UI.  Se trata de una biblioteca que realiza la mayor parte del trabajo pesado que implica presentar un cuadro de búsqueda de aspecto interesante y facilita realizar llamadas asincrónicas a Azure Search para recuperar las recomendaciones.  Veamos el código de JavaScript del primer ejemplo:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Si lo compara con el ejemplo anterior donde se llama al controlador de inicio, verá varias similitudes.  La configuración de la función Autocompletar para `minLength` y `position` es exactamente igual.  Aquí el cambio importante es el origen.  En lugar de llamar al método Sugerir en el controlador de inicio, se crea una solicitud REST en una función de JavaScript y se ejecuta mediante ajax.  Luego la respuesta se procesa en estado "correcto" y se usa como el origen.

## <a name="takeaways"></a>Puntos clave

En este tutorial se muestran los pasos básicos para crear un cuadro de búsqueda que admita las sugerencias y la función Autocompletar.  Se explicó cómo compilar una aplicación ASP.NET MVC y usar la API de REST o el SDK de .NET de Azure Search para recuperar las sugerencias.

## <a name="next-steps"></a>Pasos siguientes

Integre las sugerencias y la función Autocompletar a la experiencia de búsqueda.  Tenga en cuenta cómo el uso directo del SDK de .NET y la API de REST puede ayudar a llevar la eficacia de Azure Search a los usuarios cuando escriben para permitirles ser más productivos.

> [!div class="nextstepaction"]
> [Autocomplete REST API (API de REST de la función Autocompletar)](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete)
> [Suggestions REST API (API de REST de Sugerencias)](https://docs.microsoft.com/en-us/rest/api/searchservice/suggestions)
> [Facets index attribute on a Create Index REST API (Atributo de índice de facetas en una API de REST de creación de índice)](https://docs.microsoft.com/en-us/rest/api/searchservice/create-index)

