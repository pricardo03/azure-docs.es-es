---
title: Introducción a Knowledge Exploration Service API | Microsoft Docs
description: Use Knowledge Exploration Service (KES) para crear un motor para obtener una experiencia de búsqueda interactiva en distintas publicaciones académicas en Microsoft Cognitive Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 02dc9368eef02d6fa507335ef3171e923412acca
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380766"
---
<a name="getting-started"></a>
# <a name="get-started-with-the-knowledge-exploration-service"></a>Introducción a Knowledge Exploration Service API
En este tutorial, se usa Knowledge Exploration Service (KES) para crear un motor para obtener una experiencia de búsqueda interactiva en distintas publicaciones académicas. Puede instalar la herramienta de línea de comandos, [`kes.exe`](CommandLine.md), y todos los archivos de ejemplo desde el [SDK de Knowledge Exploration Service](https://www.microsoft.com/en-us/download/details.aspx?id=51488).

En el ejemplo de publicaciones académicas contiene una muestra de 1000 artículos académico publicados por investigadores de Microsoft.  Cada artículo está asociado con un título, un año de publicación, los autores y palabras clave. Cada autor se representa mediante un identificador, el nombre y la afiliación en el momento de la publicación. Cada palabra clave puede asociarse con un conjunto de sinónimos (por ejemplo, la palabra clave "máquina de vectores de soporte" puede asociarse al sinónimo "svm").

<a name="defining-schema"></a>
## <a name="define-the-schema"></a>Definición del esquema
El esquema describe la estructura de atributos de los objetos en el dominio. Especifica el nombre y el tipo de datos para cada atributo en un formato de archivo JSON. El ejemplo siguiente es el contenido del archivo *Academic.schema*.

```json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

En este caso, defina *Title*, *Year* y *Keyword* como una cadena, un entero y un atributo de cadena, respectivamente. Dado que los autores se representan por el identificador, el nombre y la afiliación, defina *Author* como un atributo compuesto con tres atributos secundarios: *Author.Id*, *Author.Name* y *Author.Affiliation*.

De manera predeterminada, los atributos admiten todas las operaciones disponibles para su tipo de datos, incluidos *equals*, *starts_with* e *is_between*. Dado que el id. de autor solo se utiliza internamente como identificador, reemplace el valor predeterminado y especifique *equals* como la única operación indexada.

Para el atributo *Keyword*, permita que los sinónimos coincidan con los valores canónicos de palabra clave al especificar el archivo de sinónimos *Keyword.syn* en la definición del atributo. Este archivo contiene una lista de pares de valores canónico y de sinónimo:

```json
...
["support vector machine","support vector machines"]
["support vector machine","support vector networks"]
["support vector machine","support vector regression"]
["support vector machine","support vector"]
["support vector machine","svm machine learning"]
["support vector machine","svm"]
["support vector machine","svms"]
["support vector machine","vector machine"]
...
```

Para obtener información adicional sobre la definición del esquema, consulte [Schema Format](SchemaFormat.md) (Formato de esquema).

<a name="generating-data"></a>
## <a name="generate-data"></a>Generación de datos
El archivo de datos describe la lista de las publicaciones hasta el índice, donde cada línea especifica los valores del atributo de un artículo en [formato JSON](http://json.org/).  El ejemplo siguiente es una sola línea del archivo de datos *Academic.data*, con un formato que permite su legibilidad:

```
...
{
    "logprob": -16.714,
    "Title": "the world wide telescope",
    "Year": 2001,
    "Author": [
        {
            "Id": 717694024,
            "Name": "alexander s szalay",
            "Affiliation": "microsoft"
        },
        {
            "Id": 2131537204,
            "Name": "jim gray",
            "Affiliation": "microsoft"
        }
    ]
}
...
```

En este fragmento de código, debe especificar los atributos *Title* e *Year* del artículo como una cadena JSON y un número, respectivamente. Varios valores se representan a través de matrices JSON. Dado que *Author* es un atributo compuesto, cada valor se representa mediante un objeto JSON que consta de sus atributos secundarios. Los atributos a los que falten valores, como *Keyword* en este caso, pueden excluirse de la representación JSON.

Para diferenciar la probabilidad de diferentes artículos, especifique la probabilidad logarítmica relativa con el atributo *logprob* integrado. Dada una probabilidad *p* entre 0 y 1, calcule la probabilidad logarítmica como log(*p*), donde log() es la función de logaritmo natural.

Para obtener más información, consulte [Data Format](DataFormat.md) (Formato de datos).

<a name="building-index"></a>
## <a name="build-a-compressed-binary-index"></a>Compilación de un índice binario comprimido
Una vez que tenga un archivo de esquemas y un archivo de datos, puede compilar un índice binario comprimido de los objetos de datos mediante [`kes.exe build_index`](CommandLine.md#build_index-command). En este ejemplo, va a compilar el archivo de índice *Academic.index* desde el archivo de esquema de entrada *Academic.schema* y el archivo de datos *Academic.data*. Use el comando siguiente:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Para la rápida creación de prototipos fuera de Azure, [`kes.exe build_index`](CommandLine.md#build_index-command) puede generar índices pequeños localmente, a partir de archivos de datos que contengan hasta 10 000 objetos. Para archivos de datos más grandes, puede ejecutar el comando desde una [VM de Windows en Azure](../../../articles/virtual-machines/windows/quick-create-portal.md) o realizar una compilación remota en Azure. Para obtener más información, consulte [Escalado vertical](#scaling-up).

<a name="authoring-grammar"></a>
## <a name="use-an-xml-grammar-specification"></a>Uso de una especificación de gramática XML
La gramática especifica el conjunto de consultas en lenguaje natural que puede interpretar el servicio, así como la manera en que estas consultas en lenguaje natural se convierten en expresiones de consulta semántica. En este ejemplo, se utiliza la gramática especificada en *academic.xml*:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="Academic.schema" name="academic"/>

  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>

    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>

    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>

      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>

        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>

        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>

  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="lt" name="year"/>
          </item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="gt" name="year"/>
          </item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```

Para obtener más información sobre la sintaxis de especificación de la gramática, consulte [Grammar Format](GrammarFormat.md) (Formato de gramática).

<a name="compiling-grammar"></a>
## <a name="compile-the-grammar"></a>Compilación de la gramática
Después de tener una especificación de la gramática XML, se puede compilar en una gramática binaria mediante [`kes.exe build_grammar`](CommandLine.md#build_grammar-command). Tenga en cuenta que si la gramática importa un esquema, el archivo de esquema debe estar ubicado en la misma ruta de acceso que la gramática XML. En este ejemplo, va a compilar el archivo binario de gramática *Academic.grammar* a partir del archivo de gramática XML de entrada *Academic.xml*. Use el comando siguiente:

`kes.exe build_grammar Academic.xml Academic.grammar`

<a name="hosting-index"></a>
## <a name="host-the-grammar-and-index-in-a-web-service"></a>Hospedaje de la gramática y el índice en un servicio web
Para una rápida creación de prototipos, puede hospedar la gramática y el índice en un servicio web en el equipo local, mediante [`kes.exe host_service`](CommandLine.md#host_service-command). A continuación, puede acceder al servicio a través de [API web](WebAPI.md) para validar la corrección de los datos y el diseño de la gramática. En este ejemplo, hospeda el archivo de gramática *Academic.grammar* y el archivo de índice *Academic.index* en http://localhost:8000/. Use el comando siguiente:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Esto inicia una instancia local del servicio web. Para comprobar de forma interactiva el servicio, puede visitar `http::localhost:<port>` desde un explorador. Para más información, consulte [Probar el servicio](#testing-service).

Además, puede llamar directamente a varias [API web](WebAPI.md) para probar la interpretación en lenguaje natural, la finalización de las consultas, la evaluación de consultas estructuradas y el cálculo de histogramas. Para detener el servicio, escriba "quit" en el símbolo del sistema `kes.exe host_service` o presione Ctrl+C. Estos son algunos ejemplos:

* [http://localhost:8000/interpret?query=papers by susan t dumais](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers by susan t d&complete=1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='susan t dumais')&attributes=Title,Year,Author.Name,Author.Id&count=2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='susan t dumais'),Year>=2013)&attributes=Year,Keyword&count=4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Fuera de Azure, [`kes.exe host_service`](CommandLine.md#host_service-command) se limita a índices de hasta 10 000 objetos. Otros límites incluyen una tasa de API de 10 solicitudes por segundo y un total de 1000 solicitudes antes de que el proceso finalice automáticamente. Para pasar por alto estas restricciones, ejecute el comando desde una [VM de Windows en Azure](../../../articles/virtual-machines/windows/quick-create-portal.md) o implemente en un servicio de nube de Azure mediante el comando [`kes.exe deploy_service`](CommandLine.md#deploy_service-command). Para obtener más información, consulte [Implementación del servicio](#deploying-service).

<a name="scaling-up"></a>
## <a name="scale-up-to-host-larger-indices"></a>Escalado vertical de los índices de host más grandes
Cuando se ejecuta `kes.exe` fuera de Azure, el índice se limita a 10 000 objetos. Puede compilar y hospedar índices mayores mediante Azure. Regístrese para una [evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/). Como alternativa, si se suscribe a Visual Studio, o MSDN, puede [activar las ventajas para suscriptores](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Ofrecen algunos créditos de Azure cada mes.

Para permitir que `kes.exe` acceda a una cuenta de Azure, [descargue el archivo de configuración de publicación de Azure](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) desde Azure Portal. Si se le solicita, inicie sesión en la cuenta de Azure deseada. Guarde el archivo como *AzurePublishSettings.xml* en el directorio de trabajo desde el que se ejecuta `kes.exe`.

Hay dos maneras de compilar y hospedar índices de gran tamaño. La primera consiste en preparar los archivos de esquema y de datos en una VM de Windows en Azure. A continuación, ejecute [`kes.exe build_index`](#building-index) para compilar el índice de forma local en la VM, sin ninguna restricción de tamaño. El índice resultante se puede hospedar localmente en la VM mediante [`kes.exe host_service`](#hosting-service) para la rápida creación de prototipos, nuevamente sin restricciones. Para obtener instrucciones detalladas, consulte el [tutorial de VM en Azure](../../../articles/virtual-machines/windows/quick-create-portal.md).

El segundo método consiste en realizar una compilación remota en Azure, mediante [`kes.exe build_index`](CommandLine.md#build_index-command) con el parámetro `--remote`. Especifica un tamaño de VM de Azure. Cuando se especifica el parámetro `--remote`, el comando crea una VM temporal de Azure de ese tamaño. A continuación, compila el índice en la VM, carga el índice en el almacenamiento de blobs de destino y elimina la VM al finalizar. El costo de la VM mientras se está compilando el índice se cobra a su suscripción de Azure.

Esta funcionalidad de compilación remota en Azure permite que [`kes.exe build_index`](CommandLine.md#build_index-command) se ejecute en cualquier entorno. Al realizar una compilación remota, el esquema de entrada y los argumentos de datos pueden ser rutas de acceso de archivos locales o direcciones URL de [almacenamiento de blobs de Azure](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). El argumento del índice de salida debe ser una dirección URL de almacenamiento de blobs. Para crear una cuenta de almacenamiento de Azure, consulte [Acerca de las cuentas de Azure Storage](../../storage/common/storage-create-storage-account.md). Para copiar archivos de forma eficaz hacia y desde almacenamiento de blobs, use la utilidad [AzCopy](../../storage/common/storage-use-azcopy.md).

En este ejemplo, puede suponer que ya se ha creado el siguiente contenedor de almacenamiento de blobs: http://&lt;*cuenta*&gt;.blob.core.windows.net/&lt;*contenedor*&gt;/. Contiene el esquema *Academic.schema*, el archivo de sinónimo de referencia *Keywords.syn*y el archivo de datos a escala completa *Academic.full.data*. Puede compilar el índice completo de forma remota mediante el comando siguiente:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Tenga en cuenta que puede tardar entre 5 y 10 minutos en aprovisionar una VM temporal para compilar el índice. Para la rápida creación de prototipos, haga lo siguiente:
- Desarrolle con un conjunto de datos más pequeño establecido localmente en cualquier máquina.
- Manualmente [cree una VM de Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), [conéctese a ella](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) a través de Escritorio remoto, instale el [SDK de Knowledge Exploration Service](https://www.microsoft.com/en-us/download/details.aspx?id=51488) y ejecute [`kes.exe`](CommandLine.md) desde la VM.

La paginación ralentiza el proceso de compilación. Para evitar la paginación, use una VM con el triple de RAM como el tamaño del archivo de datos de entrada para la compilación del índice. Use una VM con 1 GB más de RAM que el tamaño del índice para el hospedaje. Para obtener una lista de tamaños de VM disponibles, consulte [Tamaños de máquinas virtuales](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

<a name="deploying-service"></a>
## <a name="deploy-the-service"></a>Implementación del servicio
Una vez tenga una gramática y un índice, estará listo para implementar el servicio en la nube de Azure. Para crear un servicio en la nube de Azure, consulte [Creación e implementación de un servicio en la nube](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). No especifique un paquete de implementación en este momento.  

Una vez que haya creado el servicio en la nube, puede usar [`kes.exe deploy_service`](CommandLine.md#deploy_service-command) para implementar el servicio. Un servicio en la nube de Azure tiene dos ranuras de implementación: producción y almacenamiento ensayo. Para un servicio que recibe tráfico de usuarios en vivo, debe implementar inicialmente en el espacio de ensayo. Espere a que el servicio se inicie y se inicialice a sí mismo. A continuación, puede enviar algunas solicitudes para validar la implementación y comprobar que pase las pruebas básicas.

[Intercambie](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md) el contenido del espacio de ensayo con el espacio de producción, de modo que el tráfico en vivo ahora se dirija al servicio recién implementado. Puede repetir este proceso al implementar una versión actualizada del servicio con nuevos datos. Al igual que con todos los demás servicios en la nube de Azure, también tiene la opción de usar Azure Portal para configurar el [escalado automático](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md).

En este ejemplo, se implementa el índice *Academic* en el espacio de ensayo de un servicio en la nube existente con VM *< vm_size >*. Use el comando siguiente:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Para obtener una lista de tamaños de VM disponibles, consulte [Tamaños de máquinas virtuales](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Después de implementar el servicio, puede llamar a las distintas [API web](WebAPI.md) para probar la interpretación en lenguaje natural, la finalización de las consultas, la evaluación de consultas estructuradas y el cálculo de histogramas.  

<a name="testing-service"></a>
## <a name="test-the-service"></a>Probar el servicio
Para depurar un servicio en vivo, vaya a la máquina host desde un explorador web. Para un servicio local implementado mediante [host_service](#hosting-service), visite `http://localhost:<port>/`.  Para un servicio en la nube de Azure implementado mediante [deploy_service](#deploying-service), visite `http://<serviceName>.cloudapp.net/`.

Esta página contiene un vínculo a información sobre estadísticas básicas de llamadas API, así como la gramática y el índice hospedados en este servicio. Esta página también contiene una interfaz de búsqueda interactiva que muestra el uso de las API web. Escriba las consultas en el cuadro de búsqueda para ver los resultados de las llamadas API [interpret](interpretMethod.md), [evaluate](evaluateMethod.md) y [calchistogram](calchistogramMethod.md). El código fuente HTML subyacente de esta página también sirve como ejemplo de cómo integrar las API web en una aplicación, para crear una experiencia de búsqueda enriquecida e interactiva.


