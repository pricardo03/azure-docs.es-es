---
title: 'Text Analytics API con Power BI: Azure Cognitive Services | Microsoft Docs'
description: Aprenda a usar Text Analytics API para extraer las frases clave del texto almacenado en Power BI.
services: cognitive-services
author: luiscabrer
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: tutorial
ms.date: 3/07/2018
ms.author: luisca
ms.openlocfilehash: 2cdb93d44218627efdcb0360d8cf4a4eeeca177a
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2018
ms.locfileid: "42889281"
---
# <a name="text-analytics-with-power-bi"></a>Text Analytics con Power BI

Microsoft Power BI sintetiza los datos de la organización en atractivos informes, que puede distribuir en su organización para obtener una percepción más rápida y profunda. El servicio Text Analytics, parte de Cognitive Services de Microsoft Azure, puede extraer las frases más importantes del texto mediante su API de frases clave. Juntas, estas herramientas pueden ayudarle a ver rápidamente de lo que hablan sus clientes y cómo se sienten al respecto.

En este tutorial, verá cómo integrar Power BI Desktop y la API de frases clave para extraer las frases más importantes de los comentarios de clientes mediante una función personalizada de Power Query. También crearemos una nube de palabras a partir de estas frases.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, necesita:

> [!div class="checklist"]
> * Microsoft Power BI Desktop. [Descarga gratuita](https://powerbi.microsoft.com/get-started/).
> * Una cuenta de Microsoft Azure. [Inicie una evaluación gratuita](https://azure.microsoft.com/free/) o [inicie sesión](https://portal.azure.com/).
> * Una clave de acceso para Text Analytics. [Regístrese](../../cognitive-services-apis-create-account.md) y, luego, [obtenga su clave](../how-tos/text-analytics-how-to-access-key.md).
> * Comentarios de clientes. [Obtenga nuestros datos de ejemplo](https://aka.ms/cogsvc/ta) o use los suyos propios.

## <a name="loading-customer-data"></a>Carga de datos de clientes

Para empezar, abra Power BI Desktop y cargue el archivo de valores separados por comas (CSV) **FabrikamComments.csv**. Este archivo representa un día de actividad hipotética en el foro de soporte técnico de una pequeña empresa ficticia.

> [!NOTE]
> Power BI puede usar datos de una amplia variedad de orígenes, como Facebook o una base de datos SQL. Más información en [Integración de Facebook con Power BI](https://powerbi.microsoft.com/integrations/facebook/) e [Integración de SQL Server con Power BI](https://powerbi.microsoft.com/integrations/sql-server/).

En la ventana principal de Power BI Desktop, busque el grupo Datos externos de la cinta Inicio. Elija **Texto o CSV** en el menú desplegable **Obtener datos** de este grupo.

![[El botón Obtener datos]](../media/tutorials/power-bi/get-data-button.png)

Aparece el cuadro de diálogo Abrir. Vaya a la carpeta Descargas o a la carpeta que contiene el archivo de datos de ejemplo. Haga clic en el botón `FabrikamComments.csv`**Abrir**. Aparece el cuadro de diálogo de importación de CSV.

![[El cuadro de diálogo de importación de CSV]](../media/tutorials/power-bi/csv-import.png)

El cuadro de diálogo de importación de CSV le permite comprobar que Power BI Desktop ha detectado correctamente el juego de caracteres, el delimitador, las filas de encabezado y los tipos de columna. Toda esta información es correcta, por lo que hacemos clic en **Cargar**.

Para ver los datos cargados, cambie a la vista Datos con el botón Vista de datos del borde izquierdo del área de trabajo de Power BI. Se abre una tabla que contiene nuestros datos, como en Microsoft Excel.

![[La vista inicial de los datos importados]](../media/tutorials/power-bi/initial-data-view.png)

## <a name="preparing-the-data"></a>Preparación de los datos

Puede que necesite transformar los datos de Power BI Desktop antes de que estén listos para ser procesados por el servicio Frases clave.

Nuestros datos, por ejemplo, contienen un campo `subject` y un campo `comment`. Al extraer frases clave, deberemos tener en cuenta el texto de ambos campos, no solo de `comment`. La función Combinar columnas de Power BI Desktop facilita esta tarea.

Abra el Editor de consultas desde la ventana principal de Power BI Desktop; para ello, haga clic en **Editar consultas** en el grupo Datos externos de la cinta Inicio. 

![[El grupo Datos externos de la cinta Inicio]](../media/tutorials/power-bi/edit-queries.png)

Seleccione "FabrikamComments" en la lista Consultas en el lado izquierdo de la ventana si no está ya seleccionado.

Ahora seleccione las columnas `subject` y `comment` de la tabla. Es posible que deba desplazarse horizontalmente para ver estas columnas. En primer lugar, haga clic en el encabezado de columna `subject` y, a continuación, mantenga presionada la tecla Control y haga clic en el encabezado de columna `comment`.

![[Selección de campos que se van combinar]](../media/tutorials/power-bi/select-columns.png)

En el grupo Columnas de texto de la cinta Transformación, haga clic en **Combinar columnas**. Aparece el cuadro de diálogo Combinar columnas.

![[Combinación de campos con el cuadro de diálogo Combinar columnas]](../media/tutorials/power-bi/merge-columns.png)

En el cuadro de diálogo Combinar columnas, elija Tabulador como separador y haga clic en **Aceptar**. ¡Listo!

También podría considerar el filtrado de mensajes en blanco con el filtro Quitar vacíos, o quitar los caracteres no imprimibles mediante la transformación Limpiar. Si los datos contienen una columna como la columna `spamscore` de nuestro archivo de ejemplo, puede omitir los comentarios no deseados mediante un filtro de número.

## <a name="understanding-the-api"></a>Descripción de la API

La API de frases clave puede procesar hasta un millar de documentos de texto por cada solicitud HTTP. Sin embargo, Power BI prefiere trabajar con registros de uno en uno, así que nuestras llamadas a la API contendrán solo un documento. La [API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) requiere que se procesen los siguientes campos para cada documento.

| | |
| - | - |
| `id`  | Un identificador único para este documento dentro de la solicitud. La respuesta también contiene este campo. De este modo, si procesa varios documentos, puede asociar fácilmente las frases clave extraídas con el documento del que proceden. Vamos a procesar un documento por solicitud, así que ya sabemos con qué documento está asociada la respuesta y el valor `id` puede ser el mismo en cada solicitud.|
| `text`  | El texto que se va a procesar. Lo obtenemos de la columna `Merged` que creamos, que contiene la combinación de línea de asunto y texto de comentario. La API de frases clave requiere que estos datos no tengan más de 5000 caracteres.|
| `language` | El código que representa el idioma en el que está escrito el documento. Todos nuestros mensajes están disponibles en inglés, por lo que podemos codificar de forma rígida el idioma `en` de nuestra consulta.|

Es necesario combinar estos campos en un documento JSON (notación de objetos JavaScript) para su envío a la API de frases clave. La respuesta de esta solicitud es también un documento JSON, que debemos analizar y extraer de él las frases clave.

## <a name="creating-a-custom-function"></a>Creación de una función personalizada

Ahora estamos listos para crear la función personalizada que integrará Power BI y Text Analytics. Power BI Desktop invoca la función para cada fila de la tabla y crea una nueva columna con los resultados.

La función recibe el texto que se va a procesar como un parámetro. A continuación, convierte los datos a y desde la notación de objetos JavaScript (JSON) requerida y realiza la solicitud HTTP al punto de conexión de la API de frases clave. Después de analizar la respuesta, la función devuelve una cadena que contiene una lista separada por comas de las frases clave extraídas.

> [!NOTE]
> Las funciones personalizadas de Power BI Desktop se escriben en el [lenguaje de fórmulas M de Power Query](https://msdn.microsoft.com/library/mt211003.aspx), o simplemente "M" para abreviar. M es un lenguaje de programación funcional basado en [F#](https://docs.microsoft.com/dotnet/fsharp/). Para finalizar este tutorial no es necesario ser programador, si bien, se incluye el código necesario a continuación.

Aún debería estar en la ventana del Editor de consultas. En la cinta Inicio, haga clic en **Nuevo origen** (en el grupo Nueva consulta) y elija **Consulta en blanco** en el menú desplegable. 

Aparece una nueva consulta, denominada inicialmente Query1, en la lista de consultas. Haga doble clic en esta entrada y asígnele el nombre `KeyPhrases`.

Ahora, haga clic en **Editor avanzado** en el grupo Consulta de la cinta Inicio para abrir la ventana Editor avanzado. Elimine el código que ya está en esa ventana y pegue el código siguiente. 

> [!NOTE]
> En los ejemplos siguientes se da por hecho que el punto de conexión está en https://westus.api.cognitive.microsoft.com.  Text Analytics admite la creación de suscripciones en 13 regiones diferentes. Si se registró en el servicio en una región diferente, asegúrese de usar el punto de conexión correspondiente a la región seleccionada. Se mostrará en la página Introducción de Azure Portal al seleccionar la suscripción de Text Analytics.

```fsharp
// Returns key phrases from the text in a comma-separated list
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = Text.Lower(Text.Combine(jsonresp[documents]{0}[keyPhrases], ", "))
in  keyphrases
```

Pegue también la clave de Text Analytics API, obtenida en el panel de Microsoft Azure, en la línea que comienza con `apikey`. (Asegúrese de dejar las comillas antes y después de la clave). A continuación, haga clic en **Hecho**.

## <a name="using-the-function"></a>Uso de la función

Ahora podemos usar la función personalizada para obtener las frases clave contenidas en cada uno de los comentarios de nuestros clientes y almacenarlas en una nueva columna de la tabla. 

Todavía en el Editor de consultas, vuelva a la consulta FabrikamComments, cambie a la cinta Agregar columna y haga clic en el botón **Invocar función personalizada** en el grupo General.

![[Botón Invocar función personalizada]](../media/tutorials/power-bi/invoke-custom-function-button.png)<br><br>

En el cuadro de diálogo Invocar función personalizada, escriba `keyphrases` como nombre de la nueva columna. Elija la función personalizada, `KeyPhrases` como la consulta de función. 

Aparece un nuevo campo en el cuadro de diálogo que le pregunta qué campo va a pasar a nuestra función como parámetro `text`. Elija `Merged` (la columna que se creó anteriormente mediante la combinación de los campos de asunto y mensaje) en el menú desplegable.

![[Invocación de una función personalizada]](../media/tutorials/power-bi/invoke-custom-function.png)

Por último, haga clic en **Aceptar**.

Si todo está listo, Power BI llama a nuestra función una vez por cada fila de nuestra tabla, y realiza las consultas de frases clave y agrega la nueva columna a la tabla. Pero antes de que eso suceda, puede que deba especificar los valores de autenticación y privacidad.

## <a name="authentication-and-privacy"></a>Autenticación y privacidad

Después de cerrar el cuadro de diálogo Invocar función personalizada, puede aparecer un mensaje emergente que le pide que especifique cómo conectarse al punto de conexión de frases clave.

![[mensaje emergente de credenciales]](../media/tutorials/power-bi/credentials-banner.png)

Haga clic en **Editar credenciales** y asegúrese de que esté seleccionado Anónimo en el cuadro de diálogo; después, haga clic en **Conectar**. 

> [!NOTE]
> ¿Por qué anónimo? El servicio Text Analytics le autentica mediante la clave API, así Power BI no tiene que proporcionar credenciales para la propia solicitud HTTP.

![[configuración de la autenticación como anónima]](../media/tutorials/power-bi/access-web-content.png)

Si ve el mensaje emergente Editar credenciales incluso después de elegir acceso anónimo, es posible que haya olvidado pegar la clave de API. Compruebe la función personalizada `KeyPhrases` en el Editor avanzado para asegurarse de que está ahí.

A continuación, puede aparecer un mensaje emergente que le pide que proporcione información sobre la privacidad de sus orígenes de datos. 

![[mensaje emergente de privacidad]](../media/tutorials/power-bi/privacy-banner.png)

Haga clic en **Continuar** y elija Público para cada uno de los orígenes de datos del cuadro de diálogo. A continuación, haga clic en **Guardar**.

![[configuración de privacidad del origen de datos]](../media/tutorials/power-bi/privacy-dialog.png)

## <a name="creating-the-word-cloud"></a>Creación de la nube de palabras

Una vez que se haya ocupado de los mensajes emergentes que aparecen, haga clic en **Cerrar y aplicar** en la cinta Inicio para cerrar el Editor de consultas.

Power BI Desktop tarda un momento en realizar las solicitudes HTTP necesarias. Para cada fila de la tabla, la nueva columna `keyphrases` contiene las frases clave detectadas en el texto mediante la API de frases clave. 

Vamos a usar esta columna para generar una nube de palabras. Para comenzar, haga clic en el botón Informe en la ventana principal de Power BI Desktop, a la izquierda del área de trabajo.

> [!NOTE]
> ¿Por qué se usan las frases clave extraídas para generar una nube de palabras en lugar del texto completo de cada comentario? Las frases clave nos proporcionan las palabras *importantes* de los comentarios de nuestros clientes, no solo las palabras *más comunes*. Además, el ajuste de tamaño de las palabras en la nube resultante no queda sesgado por el uso frecuente de una palabra en un número relativamente pequeño de comentarios.

Si aún no tiene instalado el objeto visual personalizado Word Cloud, instálelo. En el panel Visualizaciones a la derecha del área de trabajo, haga clic en los tres puntos (**...** ) y elija **Importar del almacén**. A continuación, busque "nube" y haga clic en el botón **Agregar** junto al objeto visual Word Cloud. Power BI instala el objeto visual Word Cloud y le informa de que se ha instalado correctamente.

![[incorporación de un objeto visual personalizado]](../media/tutorials/power-bi/add-custom-visuals.png)<br><br>

Ahora, vamos a crear nuestra nube de palabras.

![[Icono Word Cloud en el panel de visualizaciones]](../media/tutorials/power-bi/visualizations-panel.png)

Primero, haga clic en el icono Word Cloud en el panel Visualizaciones. Un nuevo informe aparece en el área de trabajo. Arrastre el campo `keyphrases` desde el panel Campos hasta el campo Categoría del panel Visualizaciones. La nube de palabras aparece dentro del informe.

Ahora, cambie a la página Formato del panel Visualizaciones. En la categoría de palabras irrelevantes, active **Palabras no significativas predeterminadas** para eliminar palabras comunes cortas, como "of" de la nube. 

![[activación de palabras no significativas predeterminadas]](../media/tutorials/power-bi/default-stop-words.png)

Baje un poco más por este panel y desactive **Girar texto** y **Título**.

![[activar modo de enfoque]](../media/tutorials/power-bi/word-cloud-focus-mode.png)

Haga clic en la herramienta Modo de enfoque en el informe para obtener una mejor vista de nuestra nube de palabras. La herramienta expande la nube de palabras para rellenar el área de trabajo completa, como se muestra a continuación.

![[Una nube de palabras]](../media/tutorials/power-bi/word-cloud.png)

## <a name="more-text-analytics-services"></a>Más servicios de Text Analytics

El servicio Text Analytics, uno de los servicios de Cognitive Services que ofrece Microsoft Azure, también proporciona análisis de sentimiento y detección de idioma. La detección de idioma es especialmente útil si los comentarios de su cliente no están en inglés.

Estas otras API son muy parecidas a la API de frases clave. Así, pueden usarse funciones personalizadas casi idénticas para integrarlas con Power BI Desktop. Basta con que cree una consulta en blanco y pegue el código adecuado debajo en el Editor avanzado, como hizo anteriormente. (No olvide la clave de acceso). A continuación, igual que antes, use la función para agregar una nueva columna a la tabla.

La función Análisis de sentimiento siguiente devuelve una puntuación que indica cómo de positivo es el sentimiento expresado en el texto.

```fsharp
// Returns the sentiment score of the text, from 0.0 (least favorable) to 1.0 (most favorable)
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    sentiment   = jsonresp[documents]{0}[score]
in  sentiment
```

Estas son dos versiones de una función Detección de idioma. La primera devuelve el código de idioma ISO (por ejemplo, `en` para inglés), mientras que la segunda devuelve el nombre "descriptivo" (por ejemplo, `English`). Puede advertir que solo la última línea del cuerpo difiere entre las dos versiones.

```fsharp
// Returns the two-letter language code (e.g. en for English) of the text
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[iso6391Name]
in  language
```
```fsharp
// Returns the name (e.g. English) of the language in which the text is written
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[name]
in  language
```

Finalmente, esta es una variante de la función Frases clave ya presentada que devuelve las frases como un objeto de lista, en lugar de como una sola cadena de frases separadas por comas. 

> [!NOTE]
> La devolución de una sola cadena simplifica nuestro ejemplo de nube de palabras. Por otro lado, una lista es un formato más flexible para trabajar con las frases devueltas en Power BI. Puede manipular objetos de lista en Power BI Desktop con el grupo Columna estructurada de la cinta Transformación del Editor de consultas.

```fsharp
// Returns key phrases from the text as a list object
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = jsonresp[documents]{0}[keyPhrases]
in  keyphrases
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el servicio Text Analytics, el lenguaje de fórmulas M de Power Query o Power BI.

> [!div class="nextstepaction"]
> [Referencia de Text Analytics API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

> [!div class="nextstepaction"]
> [Referencia de Power Query M](//msdn.microsoft.com/library/mt211003.aspx)

> [!div class="nextstepaction"]
> [Documentación de Power BI](//powerbi.microsoft.com/documentation/powerbi-landing-page/)
