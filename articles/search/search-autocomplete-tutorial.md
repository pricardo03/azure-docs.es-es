---
title: Ejemplo de Autocompletar para incluir la escritura anticipada en un cuadro de búsqueda - Azure Search
description: Habilitar acciones de consulta de escritura anticipada en Azure Search mediante la creación de proveedores de sugerencias y formular las solicitudes que rellene un cuadro de búsqueda con completado términos o frases.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: mcarter
ms.custom: seodec2018
ms.openlocfilehash: b78fdf0c493e4631e4cdd7e26b154570b6226d1f
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369587"
---
# <a name="example-add-autocomplete-to-partial-term-inputs-in-azure-search"></a>Ejemplo: Agregar Autocompletar a entradas de término parcial en Azure Search

Esta característica de vista previa "finaliza" una entrada de término parcial proporcionando un término completado de documentos en un índice de Azure Search. Es posible que haya observado esta capacidad en los motores de búsqueda comercial. Ahora puede agregar esta característica, actualmente en versión preliminar pública, una solución de Azure Search para simplificar la ingesta de una consulta.

En este ejemplo, obtendrá información sobre cómo usar [sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions), [Autocompletar](https://docs.microsoft.com/rest/api/searchservice/autocomplete) y [facetas](search-faceted-navigation.md) en el [API de REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/) y [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) para crear un cuadro de búsqueda eficaces. 

+ Las *sugerencias* proporcionan recomendaciones de resultados reales basadas en lo que el usuario ha escrito hasta el momento. 
+ La función *Autocompletar*, [una nueva característica de versión preliminar](search-api-preview.md) de Azure Search, proporciona términos del índice para completar lo que el usuario está escribiendo. 

Compararemos varias técnicas para mejorar la productividad del usuario al trasladar la eficacia de la búsqueda directamente al usuario mientras escribe.

En este ejemplo le guiará a través de una aplicación basada en ASP.NET MVC que usa C# para llamar a la [bibliotecas de cliente .NET de Azure Search](https://aka.ms/search-sdk)y JavaScript para llamar directamente a la API de REST de Azure Search. La aplicación en este ejemplo tiene como destino un índice que rellena la [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) datos de ejemplo. Puede usar el índice ya configurado en la demostración NYCJobs o rellenar su propio índice con un cargador de datos en la solución de ejemplo NYCJobs. En el ejemplo se usan las bibliotecas [jQuery UI](https://jqueryui.com/autocomplete/) y [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) de JavaScript para crear un cuadro de búsqueda que admite la función Autocompletar. Si usa estos componentes junto con Azure Search, verá varios ejemplos de cómo admitir la función Autocompletar para anticipar la escritura en el cuadro de búsqueda.

Realizará las tareas siguientes:

> [!div class="checklist"]
> * Descargar y configurar la solución
> * Agregar información del servicio de búsqueda a la configuración de la aplicación
> * Implementar un cuadro de entrada de búsqueda
> * Agregar compatibilidad para una lista de Autocompletar que extrae información de un origen remoto 
> * Recuperar las sugerencias y Autocompletar con el SDK de .NET y API de REST
> * Admitir el almacenamiento en caché del lado cliente para mejorar el rendimiento 

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2017. Puede usar la [edición gratuita de Visual Studio 2017 Community](https://www.visualstudio.com/downloads/). 

* Descargue el ejemplo [código fuente](https://github.com/azure-samples/search-dotnet-getting-started) para el ejemplo.

* (Opcional) Una cuenta activa de Azure y un servicio Azure Search. Si no tiene una cuenta de Azure, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/). Para obtener ayuda con el proceso de aprovisionamiento de servicios, consulte [Creación de un servicio Azure Search en el portal](search-create-service-portal.md). La cuenta y servicio son opcionales porque en este ejemplo se puede realizar mediante un índice NYCJobs hospedado ya en su lugar para ver una demostración diferentes.

* (Opcional) Descargue el código de ejemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) para importar los datos NYCJobs a un índice de su propio servicio Azure Search.

> [!Note]
> Si va a usar el servicio gratuito Azure Search, está limitado a tres índices. El cargador de datos de NYCJobs crea dos índices. Asegúrese de que haya espacio en el servicio para aceptar los nuevos índices.

### <a name="set-up-azure-search-optional"></a>Configuración de Azure Search (opcional)

Siga los pasos de esta sección si desea importar los datos para la aplicación de ejemplo NYCJobs a su propio índice. Este paso es opcional.  Si quiere usar el índice de ejemplo que se proporciona, vaya directamente a la sección siguiente, Ejecución del ejemplo.

1. En la carpeta DataLoader del código de ejemplo NYCJobs, abra el archivo de solución DataLoader.sln en Visual Studio.

1. Actualice la información de la conexión del servicio Azure Search.  Abra el archivo App.config dentro del proyecto DataLoader y cambie los valores de appSettings TargetSearchServiceName y TargetSearchServiceApiKey para reflejar el servicio Azure Search y la clave de API de Azure Search Service.  Puede encontrarlos en Azure Portal.

1. Presione F5 para iniciar la aplicación.  Con esto se crearán dos índices y se importarán los datos de ejemplo de NYCJob.

1. En el ejemplo de código de ejemplo, abra el archivo de solución AutocompleteTutorial.sln en Visual Studio.  Abra Web.config dentro del proyecto AutocompleteTutorial y cambie los valores de SearchServiceName y SearchServiceApiKey a los mismos valores anteriores.

### <a name="running-the-sample"></a>Ejecución del ejemplo

Ahora está listo para ejecutar la aplicación de ejemplo de ejemplo.  Abra el archivo de solución AutocompleteTutorial.sln en Visual Studio para ejecutar el ejemplo.  La solución contiene un proyecto de ASP.NET MVC.  Presione F5 para ejecutar el proyecto y cargar la página en el explorador que prefiera.  En la parte superior, verá una opción para seleccionar C# o JavaScript.  La C# opción llama a HomeController desde el explorador y usa el SDK de .NET de Azure Search para recuperar los resultados.  La opción de JavaScript llama a la API REST de Azure Search directamente desde el explorador.  Por lo general, esta opción tendrá un rendimiento considerablemente mejor porque quita el controlador del flujo.  Puede elegir la opción que mejor se adapte a sus necesidades y a sus preferencias de lenguaje.  En la página aparecen varios ejemplos de la función Autocompletar con ayuda para cada uno de ellos.  En cada ejemplo hay texto de ejemplo recomendado que puede probar.  Para probar, empiece a escribir algunas letras en cada cuadro de búsqueda para ver lo que sucede.

## <a name="how-this-works-in-code"></a>Cómo funciona esto en el código

Ahora que vio los ejemplos en el explorador, analicemos detalladamente el primer ejemplo para revisar los componentes implicados y cómo funcionan.

### <a name="search-box"></a>Cuadro de búsqueda

Para cada opción de lenguaje, el cuadro de búsqueda es exactamente el mismo.  Abra el archivo Index.cshtml en la carpeta \Views\Home. El cuadro de búsqueda es sencillo:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Se trata de un cuadro de texto de entrada simple con una clase para establecer el estilo, un identificador para hacer referencia a JavaScript y el texto de marcador de posición.  La magia está en el código JavaScript.

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

Este código se ejecuta en el explorador al cargar la página para configurar la función Autocompletar para el cuadro de entrada de "example1a".  `minLength: 3` garantiza que la recomendación solo se mostrará cuando haya al menos tres caracteres en el cuadro de búsqueda.  El valor de origen es importante:

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

1. Lo primero que notará es un método en la parte superior de la clase con el nombre InitSearch.  Esto crea un cliente del índice HTTP autenticado en el servicio Azure Search.  Si desea obtener más información sobre cómo funciona esto, visite el siguiente ejemplo: [Cómo usar Azure Search desde una aplicación .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

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

Si lo compara con el ejemplo anterior donde se llama al controlador de inicio, verá varias similitudes.  La configuración de la función Autocompletar para `minLength` y `position` es exactamente igual.  Aquí el cambio importante es el origen.  En lugar de llamar al método sugerir en el controlador principal, una solicitud REST se crea en una función de JavaScript y ejecuta mediante Ajax.  Luego la respuesta se procesa en estado "correcto" y se usa como el origen.

## <a name="takeaways"></a>Puntos clave

En este ejemplo muestra los pasos básicos para la creación de un cuadro de búsqueda que es compatible con Autocompletar y sugerencias.  Se ha explicado cómo puede compilar una aplicación ASP.NET MVC y usar el SDK de .NET de Azure Search o la API de REST para recuperar sugerencias.

## <a name="next-steps"></a>Pasos siguientes

Integre las sugerencias y la función Autocompletar a la experiencia de búsqueda.  Tenga en cuenta cómo el SDK de .NET o la API de REST directamente pueden ayudar a trasladar la versatilidad de Azure Search a los usuarios a medida que escriben para que sean más productivos.

> [!div class="nextstepaction"]
> [Autocomplete REST API (API REST de la función Autocompletar)](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Suggestions REST API (API REST de Sugerencias)](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Facets index attribute on a Create Index REST API (Atributo de índice de facetas en una API REST de creación de índice)](https://docs.microsoft.com/rest/api/searchservice/create-index)

