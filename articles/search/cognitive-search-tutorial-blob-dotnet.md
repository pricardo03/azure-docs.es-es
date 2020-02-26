---
title: 'Tutorial: Creación de conjunto de aptitudes en C# mediante .NET'
titleSuffix: Azure Cognitive Search
description: Siga paso a paso el código de ejemplo que muestra la extracción de datos, el lenguaje natural y el procesamiento de inteligencia artificial de imágenes en una canalización de indexación de enriquecimiento de Azure Cognitive Search.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: efd4a9333b5fb02c18b2f6a6d0f8ce58bfb8f220
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472390"
---
# <a name="tutorial-create-an-ai-enrichment-pipeline-using-c-and-the-net-sdk"></a>Tutorial: Creación de una canalización de enriquecimiento de inteligencia artificial mediante C# y el SDK de .NET

En este tutorial, aprenderá la mecánica para programar el enriquecimiento de datos en Azure Cognitive Search mediante *aptitudes cognitivas*. Las aptitudes tienen el respaldo de las funcionalidades del procesamiento de lenguaje natural (NLP) y del análisis de imágenes de Cognitive Services. A través de la composición y configuración del conjunto de aptitudes puede extraer texto y representaciones de texto de un archivo con una imagen o con un documento digitalizado. También puede detectar el idioma, las entidades, las frases clave, etc. El resultado final es un contenido adicional abundante en un índice de búsqueda, creado por una canalización de indexación alimentado por inteligencia artificial.

En este tutorial, usará el SDK de .NET para realizar las tareas siguientes:

> [!div class="checklist"]
> * Crear una canalización de indexación para enriquecer los datos de ejemplo en la ruta a un índice.
> * Aplicar aptitudes integradas: reconocimiento óptico de caracteres, combinación de texto, detección de idioma, división de texto, reconocimiento de entidades, extracción de frases clave
> * Aprender a encadenar aptitudes mediante la asignación de las entradas a las salidas en conjunto de aptitudes.
> * Ejecutar solicitudes y revisar los resultados.
> * Restablecer el índice y los indexadores para su posterior desarrollo.

La salida es un índice que permite búsquedas de texto completo en Azure Cognitive Search. Puede mejorar el índice con otras funciones estándares, como [sinónimos](search-synonyms.md), [perfiles de puntuación](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [analizadores](search-analyzers.md) y [filtros](search-filters.md).

Este tutorial se ejecuta en el servicio Gratis, pero el número de transacciones gratuitas está limitado a 20 documentos por día. Si quiere ejecutar este tutorial más de una vez en el mismo día, elimine el indexador para restablecer el contador.

> [!NOTE]
> A medida que expande el ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregando más algoritmos de inteligencia artificial, tendrá que asociar un recurso facturable de Cognitive Services. Los cargos se acumulan cuando se llama a las API de Cognitive Services y por la extracción de imágenes como parte de la fase de descifrado de documentos de Azure Cognitive Search. No hay ningún cargo por la extracción de texto de documentos.
>
> La ejecución de aptitudes integradas se cobra a los [precios de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. Los precios de la extracción de imágenes se describen en la [página de precios de Búsqueda cognitiva de Azure](https://go.microsoft.com/fwlink/?linkid=2042400).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

En este tutorial se usan los siguientes servicios, herramientas y datos. 

+ [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de ejemplo. Asegúrese de que la cuenta de almacenamiento se encuentra en la misma región que Azure Cognitive Search.

+ Los [datos de ejemplo](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) están formados por un pequeño conjunto de archivos de tipos diferentes. 

+ [Instale Visual Studio](https://visualstudio.microsoft.com/) para usarlo como IDE.

+ [Cree un servicio Azure Cognitive Search](search-create-service-portal.md) o [busque uno existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Para interactuar con el servicio Azure Cognitive Search, necesitará la dirección URL del servicio y una clave de acceso. Con ambos se crea un servicio de búsqueda, por lo que, si ha agregado Azure Cognitive Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

   ![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-get-started-postman/get-url-key.png "Obtención de una clave de acceso y un punto de conexión HTTP")

Tener una clave válida genera la confianza, solicitud a solicitud, entre la aplicación que envía la solicitud y el servicio que se encarga de ella.

## <a name="prepare-sample-data"></a>Preparación de datos de ejemplo

La canalización de enriquecimiento extrae los orígenes de datos de Azure. Los datos de origen deben proceder de un tipo de origen de datos compatible de un [indexador de Azure Cognitive Search](search-indexer-overview.md). Para realizar este ejercicio, usaremos Blob Storage para mostrar varios tipos de contenido.

1. [Inicie sesión en Azure Portal](https://portal.azure.com), vaya a su cuenta de Azure Storage, haga clic en **Blobs** y, después, en **+Contenedor**.

1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) que contenga los datos de ejemplo. Puede establecer el nivel de acceso público a cualquiera de sus valores válidos. En este tutorial se supone que el nombre del contenedor es "basic-demo-data-pr".

1. Una vez creado el contenedor, ábralo y seleccione **Cargar** en la barra de comandos para cargar los [datos de ejemplo](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4).

   ![Archivos de origen en Azure Blob Storage](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Una vez cargados los archivos de ejemplo, obtenga el nombre del contenedor y una cadena de conexión de Blob Storage. Para ello, puede ir a su cuenta de almacenamiento en Azure Portal, seleccionar **Claves de acceso** y, luego, copiar el campo **Cadena de conexión**.

   La cadena de conexión debe ser una URL similar a la del ejemplo siguiente:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Hay otras maneras de especificar la cadena de conexión, como proporcionar una firma de acceso compartido. Para obtener más información acerca de las credenciales del origen de datos, consulte [Indexación de Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="set-up-your-environment"></a>Configurar el entorno

Para comenzar, abra Visual Studio y cree un nuevo proyecto de Aplicación de consola que pueda ejecutarse en .NET Core.

### <a name="install-nuget-packages"></a>Instalación de paquetes NuGet

El [SDK de .NET de Azure Cognitive Search](https://aka.ms/search-sdk) consta de algunas bibliotecas de cliente que le permiten administrar los índices, los orígenes de datos, los indexadores y los conjuntos de aptitudes, así como cargar y administrar documentos y ejecutar consultas. Todo ello sin tener que encargarse de los detalles de HTTP y JSON. Esas bibliotecas de cliente se distribuyen como paquetes de NuGet.

Para este proyecto, deberá instalar la versión 9 del paquete NuGet `Microsoft.Azure.Search` y la versión más reciente del paquete NuGet `Microsoft.Extensions.Configuration.Json`.

Instale el paquete NuGet `Microsoft.Azure.Search` con la consola del Administrador de paquetes en Visual Studio. Para abrir la consola del Administrador de paquetes, seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Para que el comando se ejecute, vaya a la [página del paquete NuGet Microsoft.Azure.Search](https://www.nuget.org/packages/Microsoft.Azure.Search), seleccione la versión 9 y copie el comando del Administrador de paquetes. En la consola del Administrador de paquetes, ejecute este comando.

Para instalar el paquete NuGet `Microsoft.Extensions.Configuration.Json` en Visual Studio, seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Administrar paquetes NuGet para la solución...** . Seleccione Examinar y busque el paquete NuGet `Microsoft.Extensions.Configuration.Json`. Una vez que lo encuentre, seleccione el paquete, seleccione el proyecto, confirme que la versión sea la versión estable más reciente y, luego, seleccione Instalar.

## <a name="add-azure-cognitive-search-service-information"></a>Incorporación de información del servicio Azure Cognitive Search

Para poder conectarse al servicio Azure Cognitive Search, deberá agregar la información del servicio de búsqueda a su proyecto. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Agregar** > **Nuevo elemento...** Asigne el nombre `appsettings.json` al archivo y seleccione **Agregar**. 

Este archivo deberá incluirse en el directorio de salida. Para ello, haga clic con el botón derecho en `appsettings.json` y seleccione **Propiedades**. Cambie el valor de **Copiar en el directorio de salida** a **Copiar si es posterior**.

Copie el siguiente código JSON en el nuevo archivo JSON.

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "SearchServiceQueryApiKey": "Put your query API key here",
  "AzureBlobConnectionString": "Put your Azure Blob connection string here",
}
```

Agregue la información del servicio de búsqueda y de la cuenta de almacenamiento de blobs.

Puede obtener la información del servicio de búsqueda en la página de su cuenta de búsqueda en Azure Portal. El nombre de la cuenta estará en la página principal, y las claves pueden encontrarse en **Claves**.

Para obtener la cadena de conexión de blobs, vaya a la cuenta de almacenamiento en Azure Portal, seleccione **Claves de acceso** y, luego, copie el campo **Cadena de conexión**.

## <a name="add-namespaces"></a>Agregar espacios de nombres

En este tutorial se usan muchos tipos diferentes de varios espacios de nombres. Para poder usar esos tipos, agregue lo siguiente a `Program.cs`.

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Extensions.Configuration;
```

## <a name="create-a-client"></a>Creación de un cliente

Cree una instancia de la clase `SearchServiceClient`.

```csharp
IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
IConfigurationRoot configuration = builder.Build();
SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);
```

`CreateSearchServiceClient` crea un nuevo `SearchServiceClient` mediante valores que se almacenan en el archivo de configuración de la aplicación (appsettings.json).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string adminApiKey = configuration["SearchServiceAdminApiKey"];

   SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
   return serviceClient;
}
```

> [!NOTE]
> La clase `SearchServiceClient` administra las conexiones con el servicio de búsqueda. Para evitar la apertura de un número excesivo de conexiones, debe intentar, si es posible, compartir una única instancia de `SearchServiceClient` en la aplicación. Sus métodos son seguros para subprocesos lo que permite habilitar este tipo de uso compartido.
> 
> 

## <a name="create-a-data-source"></a>Creación de un origen de datos

Cree una nueva instancia de `DataSource` mediante una llamada a `DataSource.AzureBlobStorage`. `DataSource.AzureBlobStorage` requiere que especifique el nombre del origen de datos, la cadena de conexión y el nombre del contenedor de blobs.

Aunque no se usa en este tutorial, también se define una directiva de eliminación temporal, que se usa para identificar los blobs eliminados en función del valor de una columna de eliminación temporal. La siguiente directiva considera que un blob se ha eliminado si tiene una propiedad de metadatos `IsDeleted` con el valor `true`.

```csharp
DataSource dataSource = DataSource.AzureBlobStorage(
    name: "demodata",
    storageConnectionString: configuration["AzureBlobConnectionString"],
    containerName: "basic-demo-data-pr",
    deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
        softDeleteColumnName: "IsDeleted",
        softDeleteMarkerValue: "true"),
    description: "Demo files to demonstrate cognitive search capabilities.");
```

Ahora que ha inicializado el objeto `DataSource`, cree el origen de datos. `SearchServiceClient` tiene una propiedad `DataSources`. Esta propiedad proporciona todos los métodos que necesita para crear, enumerar, actualizar o eliminar orígenes de datos de Azure Cognitive Search.

Para una solicitud correcta, el método devolverá el origen de datos que se creó. Si hay un problema con la solicitud, como un parámetro no válido, el método producirá una excepción.

```csharp
try
{
    serviceClient.DataSources.CreateOrUpdate(dataSource);
}
catch (Exception e)
{
    // Handle the exception
}
```

Puesto que se trata de su primera solicitud, consulte Azure Portal para confirmar el origen de datos se creó en Azure Cognitive Search. En la página del panel del servicio de búsqueda, compruebe que el icono de Data Sources tiene un nuevo elemento. Debe esperar unos minutos a que la página del portal se actualice.

  ![Icono de orígenes de datos en el portal](./media/cognitive-search-tutorial-blob/data-source-tile.png "Icono de orígenes de datos en el portal")

## <a name="create-a-skillset"></a>Creación de un conjunto de aptitudes

En esta sección, definirá un conjunto de pasos de enriquecimiento que quiere aplicar en los datos. Cada paso de enriquecimiento se denomina una *aptitud*, y el conjunto de pasos de enriquecimiento, un *conjunto de aptitudes*. Este tutorial usa un conjunto de [aptitudes cognitivas integradas](cognitive-search-predefined-skills.md):

+ [Reconocimiento óptico de caracteres](cognitive-search-skill-ocr.md) para reconocer texto escrito a mano e impreso en los archivos de imagen.

+ [Combinación de texto](cognitive-search-skill-textmerger.md) para consolidar el texto de una colección de campos en un solo campo.

+ [Detección de idioma](cognitive-search-skill-language-detection.md) para identificar el idioma del contenido.

+ [División de texto](cognitive-search-skill-textsplit.md) para dividir contenido grande en fragmentos más pequeños antes de llamar a la aptitud de extracción de frases clave y a la aptitud de reconocimiento de entidades. La extracción de frases clave y el reconocimiento de entidades aceptan entradas de 50 000 caracteres, o menos. Algunos de los archivos de ejemplo deben dividirse para no superar este límite.

+ [Reconocimiento de entidades](cognitive-search-skill-entity-recognition.md) para extraer los nombres de las organizaciones del contenido del contenedor de blobs.

+ [Extracción de frases clave](cognitive-search-skill-keyphrases.md) para extraer las frases clave principales.

Durante el procesamiento inicial, Azure Cognitive Search descifra cada documento para leer el contenido de distintos formatos de archivo. El texto encontrado procedente del archivo de origen se coloca en un campo ```content``` generado, uno para cada documento. Por lo tanto, establezca la entrada en ```"/document/content"``` para usar este texto. 

Las salidas se pueden asignar a un índice, usar como entrada para una aptitud descendente, o ambas cosas como sucede con el código de idioma. En el índice, un código de idioma es útil para el filtrado. Como entrada, el código de idioma se usa en las aptitudes de análisis de texto para informar a las reglas lingüísticas de la separación de palabras.

Para obtener más información sobre los conceptos básicos del conjunto de aptitudes, consulte el tema sobre la [definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md).

### <a name="ocr-skill"></a>Habilidad de OCR

La aptitud **OCR** extrae el texto de las imágenes. Esta aptitud supone que existe un campo normalized_images. Para generar este campo, más adelante en el tutorial también estableceremos la configuración de ```"imageAction"``` en la definición del indexador en ```"generateNormalizedImages"```.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "image",
    source: "/document/normalized_images/*"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "text",
    targetName: "text"));

OcrSkill ocrSkill = new OcrSkill(
    description: "Extract text (plain and structured) from image",
    context: "/document/normalized_images/*",
    inputs: inputMappings,
    outputs: outputMappings,
    defaultLanguageCode: OcrSkillLanguage.En,
    shouldDetectOrientation: true);
```

### <a name="merge-skill"></a>Aptitud Combinar

En esta sección, creará una aptitud **Combinar** que combina el campo de contenido del documento con el texto que se ha producido con la aptitud de OCR.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/content"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "itemsToInsert",
    source: "/document/normalized_images/*/text"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "offsets",
    source: "/document/normalized_images/*/contentOffset"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "mergedText",
    targetName: "merged_text"));

MergeSkill mergeSkill = new MergeSkill(
    description: "Create merged_text which includes all the textual representation of each image inserted at the right location in the content field.",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings,
    insertPreTag: " ",
    insertPostTag: " ");
```

### <a name="language-detection-skill"></a>Aptitud Detección de idioma

La aptitud **Detección de idioma** detecta el idioma del texto de entrada e informa de un único código de idioma para cada documento enviado en la solicitud. Vamos a usar la salida de la aptitud **Detección de idioma** como parte de la entrada de la aptitud **División de texto**.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/merged_text"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "languageCode",
    targetName: "languageCode"));

LanguageDetectionSkill languageDetectionSkill = new LanguageDetectionSkill(
    description: "Detect the language used in the document",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings);
```

### <a name="text-split-skill"></a>Aptitud División de texto

La aptitud **División** siguiente divide el texto por páginas y limita la longitud de la página a 4000 caracteres, según lo medido por `String.Length`. El algoritmo intentará dividir el texto en fragmentos que tengan un tamaño de `maximumPageLength` como máximo. En este caso, el algoritmo hará todo lo posible por dividir la frase en un límite de oración, por lo que el tamaño del fragmento puede ser ligeramente inferior a `maximumPageLength`.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/merged_text"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "languageCode",
    source: "/document/languageCode"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "textItems",
    targetName: "pages"));

SplitSkill splitSkill = new SplitSkill(
    description: "Split content into pages",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings,
    textSplitMode: TextSplitMode.Pages,
    maximumPageLength: 4000);
```

### <a name="entity-recognition-skill"></a>Aptitud Reconocimiento de entidades

Esta instancia `EntityRecognitionSkill` está configurada para reconocer el tipo de categoría `organization`. La aptitud **Reconocimiento de entidades** también puede reconocer los tipos de categoría `person` y `location`.

Tenga en cuenta que el campo "context" se establece en ```"/document/pages/*"``` con un asterisco, lo que significa que se llama al paso de enriquecimiento para cada página en ```"/document/pages"```.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/pages/*"));
    
List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "organizations",
    targetName: "organizations"));

List<EntityCategory> entityCategory = new List<EntityCategory>();
entityCategory.Add(EntityCategory.Organization);
    
EntityRecognitionSkill entityRecognitionSkill = new EntityRecognitionSkill(
    description: "Recognize organizations",
    context: "/document/pages/*",
    inputs: inputMappings,
    outputs: outputMappings,
    categories: entityCategory,
    defaultLanguageCode: EntityRecognitionSkillLanguage.En);
```

### <a name="key-phrase-extraction-skill"></a>Aptitud Extracción de frases clave

Al igual que la instancia `EntityRecognitionSkill` recién creada, se llama a la aptitud **Extracción de frases clave** para cada página del documento.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/pages/*"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "languageCode",
    source: "/document/languageCode"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "keyPhrases",
    targetName: "keyPhrases"));

KeyPhraseExtractionSkill keyPhraseExtractionSkill = new KeyPhraseExtractionSkill(
    description: "Extract the key phrases",
    context: "/document/pages/*",
    inputs: inputMappings,
    outputs: outputMappings);
```

### <a name="build-and-create-the-skillset"></a>Compilar y crear el conjunto de aptitudes

Compile `Skillset` con las aptitudes que ha creado.

```csharp
List<Skill> skills = new List<Skill>();
skills.Add(ocrSkill);
skills.Add(mergeSkill);
skills.Add(languageDetectionSkill);
skills.Add(splitSkill);
skills.Add(entityRecognitionSkill);
skills.Add(keyPhraseExtractionSkill);

Skillset skillset = new Skillset(
    name: "demoskillset",
    description: "Demo skillset",
    skills: skills);
```

Cree el conjunto de aptitudes en el servicio de búsqueda.

```csharp
try
{
    serviceClient.Skillsets.CreateOrUpdate(skillset);
}
catch (Exception e)
{
    // Handle exception
}
```

## <a name="create-an-index"></a>Creación de un índice

En esta sección, se define el esquema de índice. Para ello, se especifican los campos que se incluirán en el índice que permite búsquedas y los atributos de búsqueda de cada campo. Los campos tienen un tipo y pueden tener atributos que determinen cómo se utiliza el campo (si permite búsquedas, se puede ordenar, etc.). Los nombres de campos de un índice no tienen que coincidir exactamente con los nombres de campo del origen. En un paso posterior, agregará asignaciones de campos en un indexador para conectar los campos de origen y destino. Para este paso, defina el índice con convenciones de nomenclatura de campos adecuadas para la aplicación de búsqueda.

En este ejercicio se utilizan los siguientes campos y tipos de campos:

| field-names: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="create-demoindex-class"></a>Crear la clase DemoIndex

Los campos de este índice se definen mediante una clase de modelo. Cada propiedad de la clase de modelo tiene atributos que determinan los comportamientos relacionados con la búsqueda del campo de índice correspondiente. 

Vamos a agregar la clase del modelo a un nuevo archivo de C#. Haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Nuevo elemento...** , seleccione "Class" y el nombre del archivo `DemoIndex.cs`. Luego, seleccione **Agregar**.

Asegúrese de indicar que quiere usar tipos de los espacios de nombres `Microsoft.Azure.Search` y `Microsoft.Azure.Search.Models`.

```csharp
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
```

Agregue la siguiente definición de la clase de modelo en `DemoIndex.cs` e inclúyala en el mismo espacio de nombres donde creará el índice.

```csharp
// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Cognitive Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public class DemoIndex
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsSearchable, IsSortable]
    public string Id { get; set; }

    [IsSearchable]
    public string Content { get; set; }

    [IsSearchable]
    public string LanguageCode { get; set; }

    [IsSearchable]
    public string[] KeyPhrases { get; set; }

    [IsSearchable]
    public string[] Organizations { get; set; }
}
```

Ahora que ha definido una clase de modelo, de nuevo en `Program.cs`, puede crear una definición de índice con bastante facilidad. El nombre de este índice será "demoindex".

```csharp
var index = new Index()
{
    Name = "demoindex",
    Fields = FieldBuilder.BuildForType<DemoIndex>()
};
```

Durante las pruebas, es posible que intente crear el índice de más de una vez. Por este motivo, compruebe si el índice que está por crear ya existe antes de intentar crearlo.

```csharp
try
{
    bool exists = serviceClient.Indexes.Exists(index.Name);

    if (exists)
    {
        serviceClient.Indexes.Delete(index.Name);
    }

    serviceClient.Indexes.Create(index);
}
catch (Exception e)
{
    // Handle exception
}
```

Para más información acerca de cómo definir un índice, consulte [Creación de un índice (API REST de Azure Cognitive Search)](https://docs.microsoft.com/rest/api/searchservice/create-index).

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Creación de un indexador, asignación de campos y ejecución de transformaciones

Hasta ahora, ha creado un origen de datos, un conjunto de aptitudes y un índice. Estos tres componentes pasan a formar parte de un [indexador](search-indexer-overview.md) que extrae todas las piezas juntas en una sola operación de varias fases. Para unirlas en un indexador, debe definir las asignaciones de campos.

+ Los campos fieldMappings se procesan antes que el conjunto de aptitudes, lo que asigna los campos de origen del origen de datos a los campos de destino de un índice. Si los tipos y nombres de campo son los mismos en ambos extremos, no se requiere ninguna asignación.

+ Los campos outputFieldMappings se procesan después que el conjunto de aptitudes y hacen referencia a las asignaciones sourceFieldNames que no existen hasta que el descifrado o enriquecimiento de documentos las crean. targetFieldName es un campo de un índice.

Además de enlazar las entradas con las salidas, también puede usar las asignaciones de campos para aplanar las estructuras de datos. Para más información, consulte [Asignación de campos enriquecidos a un índice de búsqueda](cognitive-search-output-field-mapping.md).

```csharp
IDictionary<string, object> config = new Dictionary<string, object>();
config.Add(
    key: "dataToExtract",
    value: "contentAndMetadata");
config.Add(
    key: "imageAction",
    value: "generateNormalizedImages");

List<FieldMapping> fieldMappings = new List<FieldMapping>();
fieldMappings.Add(new FieldMapping(
    sourceFieldName: "metadata_storage_path",
    targetFieldName: "id",
    mappingFunction: new FieldMappingFunction(
        name: "base64Encode")));
fieldMappings.Add(new FieldMapping(
    sourceFieldName: "content",
    targetFieldName: "content"));

List<FieldMapping> outputMappings = new List<FieldMapping>();
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/pages/*/organizations/*",
    targetFieldName: "organizations"));
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/pages/*/keyPhrases/*",
    targetFieldName: "keyPhrases"));
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/languageCode",
    targetFieldName: "languageCode"));

Indexer indexer = new Indexer(
    name: "demoindexer",
    dataSourceName: dataSource.Name,
    targetIndexName: index.Name,
    description: "Demo Indexer",
    skillsetName: skillSet.Name,
    parameters: new IndexingParameters(
        maxFailedItems: -1,
        maxFailedItemsPerBatch: -1,
        configuration: config),
    fieldMappings: fieldMappings,
    outputFieldMappings: outputMappings);

try
{
    bool exists = serviceClient.Indexers.Exists(indexer.Name);

    if (exists)
    {
        serviceClient.Indexers.Delete(indexer.Name);
    }

    serviceClient.Indexers.Create(indexer);
}
catch (Exception e)
{
    // Handle exception
}
```

Es de esperar que la creación del indexador tarde algo de tiempo en completarse. Aunque el conjunto de datos es pequeño, las aptitudes analíticas realiza un uso intensivo de los recursos. Algunas aptitudes, como el análisis de imágenes, son de larga ejecución.

> [!TIP]
> La creación de un indexador invoca la canalización. Si hay problemas para conectar con los datos, las entradas y salidas de asignación o el orden de las operaciones, se muestran en esta fase.

### <a name="explore-creating-the-indexer"></a>Explorar la creación del indexador

El código establece ```"maxFailedItems"``` en -1, que indica al motor de indexación que omita los errores durante la importación de datos. Esto es útil porque hay muy pocos documentos en el origen de datos de demostración. Para un origen de datos mayor, debería establecer un valor mayor que 0.

Observe también que ```"dataToExtract"``` se estable en ```"contentAndMetadata"```. Esta instrucción indica al indexador que extraiga automáticamente el contenido de diferentes formatos de archivo, así como los metadatos relacionados con cada archivo.

Una vez extraído el contenido, puede establecer `imageAction` para que se extraiga el texto de las imágenes que se encuentran en el origen de datos. La configuración de ```"imageAction"``` establecido en ```"generateNormalizedImages"```, combinada con la aptitud de OCR y la aptitud de Combinación de texto, indica al indexador que extraiga el texto de las imágenes (por ejemplo, la palabra "stop" de la señal de tráfico Stop) y lo inserte como parte del campo de contenido. Este comportamiento se aplica a las imágenes incrustadas en los documentos (piense en una imagen de un archivo PDF), así como a las imágenes que se encuentran en el origen de datos, como un archivo JPG.

## <a name="check-indexer-status"></a>Comprobación del estado del indexador

Una vez definido el indexador, se ejecuta automáticamente cuando se envía la solicitud. Dependiendo de las aptitudes cognitivas definidas, la indexación puede tardar más de lo esperado. Para averiguar si el indexador todavía se está ejecutando, use el método `GetStatus`.

```csharp
try
{
    IndexerExecutionInfo demoIndexerExecutionInfo = serviceClient.Indexers.GetStatus(indexer.Name);

    switch (demoIndexerExecutionInfo.Status)
    {
        case IndexerStatus.Error:
            Console.WriteLine("Indexer has error status");
            break;
        case IndexerStatus.Running:
            Console.WriteLine("Indexer is running");
            break;
        case IndexerStatus.Unknown:
            Console.WriteLine("Indexer status is unknown");
            break;
        default:
            Console.WriteLine("No indexer information");
            break;
    }
}
catch (Exception e)
{
    // Handle exception
}
```

`IndexerExecutionInfo` representa el estado actual y el historial de ejecución de un indexador.

Las advertencias son comunes con algunas combinaciones de aptitudes y archivos de origen y no siempre indican un problema. En este tutorial, las advertencias son benignas (por ejemplo, no hay entradas de texto de archivos JPEG).
 
## <a name="query-your-index"></a>Consulta del índice

Una vez finalizada la indexación, puede ejecutar consultas que devuelvan el contenido de los campos individuales. De forma predeterminada, Azure Cognitive Search devuelve los primeros 50 resultados. Los datos de ejemplo tiene un tamaño reducido, por lo que la configuración predeterminada funciona bien. Sin embargo, si trabaja con conjuntos de datos más grandes, deberá incluir parámetros en la cadena de consulta para que se devuelvan más resultados. Para ver las instrucciones, consulte [Cómo paginar los resultados de la búsqueda en Azure Cognitive Search](search-pagination-page-layout.md).

Como paso de verificación, consulte el índice para todos los campos.

```csharp
DocumentSearchResult<DemoIndex> results;

ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*");
}
catch (Exception e)
{
    // Handle exception
}
```

`CreateSearchIndexClient` crea un nuevo `SearchIndexClient` mediante valores que se almacenan en el archivo de configuración de la aplicación (appsettings.json). Tenga en cuenta que se usa la clave de API de consulta del servicio de búsqueda, no la clave de administrador.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string queryApiKey = configuration["SearchServiceQueryApiKey"];

   SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "demoindex", new SearchCredentials(queryApiKey));
   return indexClient;
}
```

La salida es el esquema de índice, con el nombre, el tipo y los atributos de cada campo.

Envíe una segunda consulta para `"*"` a fin de devolver todo el contenido de un campo único, como `organizations`.

```csharp
SearchParameters parameters =
    new SearchParameters
    {
        Select = new[] { "organizations" }
    };

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*", parameters);
}
catch (Exception e)
{
    // Handle exception
}
```

Repita el proceso para otros campos: contenido, languageCode, keyPhrases y organizaciones del ejercicio. Puede devolver varios campos a través de `$select` con una lista delimitada por comas.

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Restablecer y volver a ejecutar

En las primeras etapas experimentales de desarrollo, el enfoque más práctico para las iteraciones del diseño es eliminar los objetos de Azure Cognitive Search y permitir que el código vuelva a generarlos. Los nombres de los recursos son únicos. La eliminación de un objeto permite volver a crearlo con el mismo nombre.

En este tutorial se encargó de comprobar los indexadores e índices existentes y eliminarlos, si ya existían, para poder volver a ejecutar el código.

También puede usar el portal para eliminar los índices, indexadores y conjuntos de aptitudes.

A medida que el código evoluciona, puede refinar una estrategia de regeneración. Para más información, consulte el artículo sobre cómo [recompilar un índice](search-howto-reindex.md).

## <a name="takeaways"></a>Puntos clave

En este tutorial se han mostrado los pasos básicos para la creación de una canalización de indexación enriquecida a través de la creación de componentes: un origen de datos, un conjunto de aptitudes, un índice y un indexador.

Se presentaron las [aptitudes integradas](cognitive-search-predefined-skills.md), junto con la definición del conjunto de aptitudes y los mecanismos de encadenamiento de aptitudes, mediante entradas y salidas. También aprendió que `outputFieldMappings` en la definición del indexador es necesario para enrutar los valores enriquecidos de la canalización a un índice que permita búsquedas en un servicio Azure Cognitive Search.

Por último, ha aprendido cómo probar los resultados y restablecer el sistema para otras iteraciones. Ha aprendido que emitir consultas en el índice devuelve la salida creada por la canalización de indexación enriquecida. También ha aprendido cómo comprobar el estado del indexador y qué objetos se deben eliminar antes de volver a ejecutar una canalización.

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un tutorial es eliminar el grupo de recursos que contenga el servicio de Azure Cognitive Search y Azure Blob service. Si decide colocar ambos servicios en el mismo grupo, elimine el grupo de recursos para eliminar de manera permanente todo lo que contiene, incluyendo los servicios y cualquier contenido almacenado que haya creado para este tutorial. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio.

## <a name="next-steps"></a>Pasos siguientes

Personalice o extienda la canalización con aptitudes personalizadas. Al crear una aptitud personalizada y agregarla a un conjunto de aptitudes, puede incorporar texto o análisis de imágenes que escriba usted mismo.

> [!div class="nextstepaction"]
> [Ejemplo: Creación de una aptitud personalizada para el enriquecimiento con inteligencia artificial](cognitive-search-create-custom-skill-example.md)
