---
title: Sugerencias de rendimiento de Azure Cosmos DB para .NET
description: Conozca las opciones de configuración de cliente para mejorar el rendimiento de las bases de datos de Azure Cosmos.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: sngun
ms.openlocfilehash: dc9d10a6539c7fc3a7c5c8b3db290cc951c24883
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623315"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Sugerencias de rendimiento para Azure Cosmos DB y .NET

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB es una base de datos distribuida rápida y flexible que se escala sin problemas con una latencia y un rendimiento garantizados. No es necesario realizar cambios de arquitectura importantes ni escribir código complejo para escalar la base de datos con Azure Cosmos DB. Escalar o reducir verticalmente es tan sencillo como realizar una única llamada API. Para más información, consulte [cómo aprovisionar el rendimiento del contenedor](how-to-provision-container-throughput.md) o [cómo aprovisionar el rendimiento de la base de datos](how-to-provision-database-throughput.md). Sin embargo, como el acceso a Azure Cosmos DB se realiza mediante llamadas de red, puede realizar optimizaciones en el lado cliente para conseguir un rendimiento máximo al usar el [SDK de .NET para SQL](sql-api-sdk-dotnet-standard.md).

Así que si se está preguntando "¿Cómo puedo mejorar el rendimiento de la base de datos?", tenga en cuenta las opciones siguientes:

## <a name="hosting-recommendations"></a>Hospedando recomendaciones

1.  **Para las cargas de trabajo de consulta intensiva, use Windows de 64 bits en lugar del proceso de host de Linux o Windows de 32 bits**

    El proceso de host de Windows de 64 bits se recomienda para mejorar el rendimiento. El SDK de SQL incluye un ServiceInterop.dll nativo para analizar y optimizar consultas localmente, y solo se admite en la plataforma Windows x64. En el caso de Linux y otras plataformas no compatibles donde el ServiceInterop.dll no está disponible, realizará una llamada de red adicional a la puerta de enlace para obtener la consulta optimizada. De forma predeterminada, los siguientes tipos de aplicaciones tienen un proceso de host de 32 bits; para cambiar a 64 bits, siga estos pasos según el tipo de aplicación:

    - Para aplicaciones ejecutables, esto puede hacerse estableciendo el [destino de la plataforma](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) en **x64** en la ventana **Propiedades del proyecto**, en la pestaña **Generar**.

    - Para proyectos de prueba basados en VSTest, puede hacerlo seleccionando **Prueba**->**Configuración de pruebas**->**Arquitectura del procesador predeterminada como X64**, en la opción de menú **Prueba de Visual Studio**.

    - Para aplicaciones web de ASP.NET implementadas localmente, se puede hacer seleccionando **Usar la versión de 64 bits de IIS Express para proyectos y sitios web**, en **Herramientas**->**Opciones**->**Proyectos y soluciones**->**Proyectos Web**.

    - Para aplicaciones web de ASP.NET implementadas en Azure, se puede hacer eligiendo la **Plataforma de 64 bits** en **Configuración de la aplicación** en Azure Portal.

    > [!NOTE] 
    > Visual Studio tiene como valores predeterminados nuevos proyectos en Cualquier CPU. Se recomienda establecer el proyecto en x64 para evitar su cambio a x86. Puede cambiarse cualquier proyecto de CPU fácilmente a x86 si se agrega alguna dependencia que sea solo x86.<br/>
    > El ServiceInterop.dll debe estar en la misma carpeta que el archivo dll de SDK en el que se ejecuta. Solo es necesario hacer esto para usuarios que copian manualmente archivos dll o tienen sistemas de compilación/implementación personalizados.
    
2. **Activación de la recolección de elementos no utilizados (GC) del lado servidor**

    En algunos casos puede ser de ayuda reducir la frecuencia de recopilación de elementos no utilizados. En .NET, establezca [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) en True.

3. **Escalado horizontal de la carga de trabajo de cliente**

    Si va a realizar pruebas en niveles de alto rendimiento (>50 000 RU/s), la aplicación cliente puede volverse un cuello de botella debido a que la máquina limita el uso de CPU o de la red. Si llega a este punto, puede seguir insertando la cuenta de Azure Cosmos DB mediante la escala horizontal de las aplicaciones cliente en varios servidores.

    > [!NOTE] 
    > Un uso elevado de CPU puede provocar el aumento de la latencia, así como excepciones de tiempo de espera de solicitud.

## <a name="networking"></a>Redes
<a id="direct-connection"></a>

1. **Directiva de conexión: uso del modo de conexión directa**

    El modo en que un cliente se conecta a Azure Cosmos DB tiene implicaciones importantes sobre el rendimiento, especialmente en los términos de la latencia observada en el lado cliente. Existen dos opciones de configuración clave disponibles para configurar la directiva de conexión de cliente: el *modo* de conexión y el *protocolo* de conexión.  Los dos modos disponibles son:

   * Modo de puerta de enlace
      
     El modo de puerta de enlace se admite en todas las plataformas de SDK y es el valor predeterminado configurado para el [SDK de Microsoft.Azure.DocumentDB](sql-api-sdk-dotnet.md). Si la aplicación se ejecuta dentro de una red corporativa con restricciones de firewall estrictas, el modo de puerta de enlace es la mejor opción, ya que utiliza el puerto HTTPS estándar y un único punto de conexión. La desventaja para el rendimiento, sin embargo, es que el modo de puerta de enlace implica un salto de red adicional cada vez que se leen o escriben datos en Azure Cosmos DB. Por este motivo, el modo directo ofrece mejor rendimiento debido al número menor de saltos de red. También se recomienda el modo de conexión de puerta de enlace para ejecutar aplicaciones en entornos con un número limitado de conexiones de socket. 

     Al usar el SDK en Azure Functions, especialmente en el [plan de consumo](../azure-functions/functions-scale.md#consumption-plan), tenga en cuenta los [límites de conexión](../azure-functions/manage-connections.md) actuales. En ese caso, es posible que se recomiende el modo de puerta de enlace si también trabaja con otros clientes basados en HTTP dentro de la aplicación Azure Functions.

   * Modo directo

     El modo directo admite la conectividad mediante el protocolo TCP y es el modo de conectividad predeterminado si usa el [SDK Microsoft.Azure.Cosmos/.Net V3](sql-api-sdk-dotnet-standard.md).

     Cuando se usa el modo de puerta de enlace, Cosmos DB usa el puerto 443. Cuando se usa la API de Azure Cosmos DB para MongoDB se usan los puertos 10250, 10255 y 10256. El puerto 10250 se asigna a una instancia de MongoDB predeterminada sin replicación geográfica y los puertos 10255 y 10256 se asignan a la instancia de MongoDB con funcionalidad de replicación geográfica. Al utilizar TCP en modo directo, además de los puertos de la puerta de enlace, debe garantizar que los puertos entre el 10000 y el 20000 estén abiertos, ya que Azure Cosmos DB utiliza puertos TCP dinámicos. Si estos puertos no están abiertos y trata de utilizar TCP, recibirá un error 503 de servicio no disponible. En la tabla siguiente se muestran los modos de conectividad disponibles para distintas API y el usuario de puertos de servicio para cada API:

     |Modo de conexión  |Protocolo admitido  |SDK admitidos  |API o puerto de servicio  |
     |---------|---------|---------|---------|
     |Puerta de enlace  |   HTTPS    |  Todos los SDK    |   SQL(443), Mongo(10250, 10255, 10256), Table(443), Cassandra(10350), Graph(443)    |
     |Directo    |     TCP    |  .NET SDK    | Puertos incluidos en el intervalo 10 000-20 000 |

     Azure Cosmos DB ofrece un modelo de programación RESTful sencillo y abierto sobre HTTPS. Además, ofrece un protocolo TCP eficaz que también es RESTful en su modelo de comunicación y está disponible a través del SDK de cliente de .NET. El protocolo TCP usa SSL para la autenticación inicial y el cifrado del tráfico. Para obtener el mejor rendimiento, utilice el protocolo TCP cuando sea posible.

     En el caso del SDK V3, el modo de conectividad se configura al crear la instancia de CosmosClient, como parte de CosmosClientOptions; recuerde que el modo directo es el predeterminado.

     ```csharp
     var serviceEndpoint = new Uri("https://contoso.documents.net");
     var authKey = "your authKey from the Azure portal";
     CosmosClient client = new CosmosClient(serviceEndpoint, authKey,
     new CosmosClientOptions
     {
        ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
     });
     ```

     Para el SDK de Microsoft.Azure.DocumentDB, el modo de conectividad se configura durante la construcción de la instancia de DocumentClient con el parámetro ConnectionPolicy. Si se utiliza el modo directo, el protocolo también puede establecerse en el parámetro ConnectionPolicy.

     ```csharp
     var serviceEndpoint = new Uri("https://contoso.documents.net");
     var authKey = "your authKey from the Azure portal";
     DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
     new ConnectionPolicy
     {
        ConnectionMode = ConnectionMode.Direct, //ConnectionMode.Gateway is the default
        ConnectionProtocol = Protocol.Tcp
     });
     ```

     Como TCP solo se admite en modo directo, si se utiliza el modo de puerta de enlace, siempre se usa el protocolo HTTPS para comunicarse con la puerta de enlace y se omite el valor de protocolo en el parámetro ConnectionPolicy.

     ![Ilustración de la directiva de conexión de Azure Cosmos DB](./media/performance-tips/connection-policy.png)

2. **Llamada a OpenAsync para evitar la latencia de inicio en la primera solicitud**

    De forma predeterminada, la primera solicitud tendrá una latencia mayor porque debe capturar la tabla de enrutamiento de direcciones. Al usar el [SDK V2](sql-api-sdk-dotnet.md), para evitar esta latencia de inicio en la primera solicitud, se debe llamar a OpenAsync() una vez durante la inicialización de esta manera.

        await client.OpenAsync();

    > [!NOTE] 
    > El método OpenAsync generará solicitudes para obtener la tabla de enrutamiento de direcciones para todos los contenedores de la cuenta. Para las cuentas con numerosos contenedores pero cuya aplicación acceda solo a un subconjunto de estos, generaría una cantidad innecesaria de tráfico que ralentizaría la inicialización. Por tanto, el método OpenAsync podría ser inútil en este escenario, ya que ralentiza el inicio de la aplicación.

   <a id="same-region"></a>
3. **Colocación de los clientes en la misma región de Azure para aumentar el rendimiento**

    Cuando sea posible, coloque las aplicaciones que llaman a Azure Cosmos DB en la misma región que la base de datos de Azure Cosmos. Para obtener una comparación aproximada, las llamadas a Azure Cosmos DB en la misma región se realizan en menos de 1 o 2 ms, pero la latencia entre las costas este y oeste de Estados Unidos es mayor de 50 ms. Esta latencia podría variar de una solicitud a otra, según la ruta tomada por la solicitud cuando pasa del cliente al límite del centro de datos de Azure. Para conseguir la menor latencia posible, asegúrese de que la aplicación que llama se encuentra en la misma región de Azure que el punto de conexión de Azure Cosmos DB aprovisionado. Para obtener una lista de regiones disponibles, consulte [Regiones de Azure](https://azure.microsoft.com/regions/#services).

    ![Ilustración de la directiva de conexión de Azure Cosmos DB](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>

4. **Aumento del número de subprocesos y tareas**

    Dado que las llamadas a Azure Cosmos DB se realizan a través de la red, puede que tenga que variar el grado de paralelismo de las solicitudes para reducir todo lo posible el tiempo que la aplicación cliente espera entre una solicitud y otra. Por ejemplo, si usa la [biblioteca TPL](https://msdn.microsoft.com//library/dd460717.aspx) de .NET, cree del orden de 100 tareas de lectura o escritura en Azure Cosmos DB.

5. **Habilitación de la redes aceleradas**

   Con el fin de reducir la latencia y la vibración de la CPU, se recomienda habilitar las redes aceleradas en las máquinas virtuales de cliente. Consulte los artículos [Creación de una máquina virtual Windows con redes aceleradas](../virtual-network/create-vm-accelerated-networking-powershell.md) o [Creación de una máquina virtual Linux con redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md) para habilitar las redes aceleradas.


## <a name="sdk-usage"></a>Uso del SDK
1. **Instalación del SDK más reciente**

    Los SDK de Azure Cosmos DB se mejoran constantemente para proporcionar el mejor rendimiento. Consulte las páginas del [SDK de Azure Cosmos DB](sql-api-sdk-dotnet-standard.md) para determinar las mejoras de los SDK y las revisiones más recientes.

2. **Uso de las API de transmisión**

    El [SDK V3 de .NET](sql-api-sdk-dotnet-standard.md) contiene las API de transmisión que reciben y devuelven datos sin serialización. 

    Las aplicaciones de nivel intermedio que no consumen las respuestas del SDK directamente, sino que las retransmiten a otros niveles de aplicación, pueden beneficiarse de las API de transmisión. Consulte los ejemplos de [administración de elementos](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement) sobre el control de la transmisión.

3. **Uso de un cliente de Azure Cosmos DB singleton para aumentar la duración de la aplicación**

    Las instancias de DocumentClient y CosmosClient están protegidas frente a amenazas y realizan una administración de conexiones y un almacenamiento en caché de las direcciones de manera eficiente cuando funcionan en modo directo. Para permitir la administración eficiente de las conexiones y un rendimiento mejor mediante el cliente de SDK, se recomienda usar una sola instancia por AppDomain durante la vigencia de la aplicación.

   <a id="max-connection"></a>

4. **Aumento de System.Net MaxConnections por host al usar el modo de puerta de enlace**

    Las solicitudes de Azure Cosmos DB se realizan a través de HTTPS o REST cuando se usa el modo de puerta de enlace, y están condicionadas por los límites de conexión predeterminados por nombre de host o dirección IP. Puede que deba establecer MaxConnections en un valor mayor (100-1000) para que la biblioteca del cliente pueda utilizar varias conexiones simultáneas para Azure Cosmos DB. En el SDK 1.8.0 de .NET o versiones superiores, el valor predeterminado de [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) es 50 y para cambiar el valor, debe establecer [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) en un valor más alto.

5. **Ajuste de consultas paralelas en colecciones particionadas**

     A partir de la versión 1.9.0 del SDK de .NET para SQL se admiten consultas paralelas que permiten consultar una colección con particiones en paralelo. Para obtener más información, consulte [ejemplos de código](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) relacionados para trabajar con los SDK. Las consultas paralelas están diseñadas para mejorar la latencia y el rendimiento de la consulta en todos sus homólogos seriales. Las consultas paralelas proporcionan dos parámetros que los usuarios pueden adaptar según sus necesidades: (a) MaxDegreeOfParallelism, para controlar el número máximo de particiones que se pueden consultar en paralelo, y (b) MaxBufferedItemCount, para controlar el número de resultados recuperados previamente.

    (a) ***Ajuste del grado de paralelismo\:*** La consulta paralela sirve para realizar consultas en varias particiones en paralelo. Sin embargo, los datos de una partición individual se capturan en serie con respecto a la consulta. Establecer `MaxDegreeOfParallelism` en el [SDK V2](sql-api-sdk-dotnet.md) o `MaxConcurrency` en el [SDK V3](sql-api-sdk-dotnet-standard.md) en el número de particiones tiene la máxima probabilidad de conseguir el mejor rendimiento de consulta, siempre y cuando el resto de condiciones del sistema permanezcan invariables. Si no conoce el número de particiones, puede establecer el grado de paralelismo en un número alto y el sistema elegirá el mínimo (número de particiones, entrada proporcionada por el usuario) como valor del grado de paralelismo.

    Es importante tener en cuenta que las consultas en paralelo producen los mejores beneficios si los datos se distribuyen uniformemente entre todas las particiones con respecto a la consulta. Si la colección con particiones está dividida de tal forma que todos, o la mayoría de los datos, devueltos por una consulta se concentran en algunas particiones (una partición en el peor de los casos), entonces el rendimiento de la consulta se vería afectada por cuellos de botella debido a esas particiones.

    (b) La consulta paralela ***Tuning MaxBufferedItemCount\:*** está diseñada para capturar previamente los resultados mientras el cliente procesa el lote actual de resultados. La captura previa ayuda a mejorar la latencia general de una consulta. MaxBufferedItemCount es el parámetro para limitar el número de resultados capturados previamente. Establecer MaxBufferedItemCount en el número esperado de resultados devueltos (o un número más alto) permite que la consulta reciba el máximo beneficio de la captura previa.

    La captura previa funciona de la misma manera con independencia del grado de paralelismo y hay un único búfer para los datos de todas las particiones.  

6. **Implementación del retroceso según intervalos RetryAfter**

    Durante las pruebas de rendimiento, debe aumentar la carga hasta que se limite una tasa de solicitudes pequeña. Si se limita, la aplicación del cliente debe retroceder de acuerdo con la limitación para el intervalo de reintento que el servidor especificó. Respetar el retroceso garantiza que dedica una cantidad de tiempo mínima de espera entre reintentos. Se incluye compatibilidad con la directiva de reintentos en la versión 1.8.0 y superior de [.NET](sql-api-sdk-dotnet.md) y [Java](sql-api-sdk-java.md) de SQL, y en la versión 1.9.0 y superior de [Node.js](sql-api-sdk-node.md) y [Python](sql-api-sdk-python.md), así como en todas las versiones compatibles de los SDK de [.NET Core](sql-api-sdk-dotnet-core.md). Para más información, vea [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
    
    Con la versión 1.19 y versiones posterior del SDK de. NET, hay un mecanismo para registrar información adicional de diagnóstico y soluciones de problemas de latencia como se muestra en el ejemplo siguiente. Puede registrar la cadena de diagnóstico para las solicitudes que tienen una mayor latencia de lectura. La cadena de diagnóstico capturada le ayudará a comprender el número de veces que se ha observado la aparición de códigos de estado 429 para una solicitud determinada.
    ```csharp
    ResourceResponse<Document> readDocument = await this.readClient.ReadDocumentAsync(oldDocuments[i].SelfLink);
    readDocument.RequestDiagnosticsString 
    ```

7. **Almacenamiento en caché de los identificadores URI de documentos para una latencia menor en las operaciones de lectura**

    Siempre que sea posible, almacene en caché los identificadores URI para obtener el mejor rendimiento en las operaciones de lectura. Tendrá que definir la lógica para almacenar en caché el valor del identificador del recurso al crear el recurso. Las búsquedas basadas en el identificador del recurso son más rápidas que las búsquedas basadas en el nombre, por lo que almacenar en caché estos valores mejora el rendimiento. 

   <a id="tune-page-size"></a>
8. **Ajuste del tamaño de página en consultas y fuentes de lectura para aumentar el rendimiento**

   Al realizar una lectura masiva de documentos mediante la funcionalidad de fuentes de lectura (por ejemplo, ReadDocumentFeedAsync) o al emitir una consulta SQL, los resultados se devuelven de forma segmentada si el conjunto de resultados es demasiado grande. De forma predeterminada, se devuelven resultados en fragmentos de 1 MB o de 100 artículos, el límite que se alcance primero.

   Para reducir el número de recorridos de ida y vuelta de red necesarios para recuperar todos los resultados aplicables, puede aumentar el tamaño de página con el encabezado de solicitud [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) hasta a 1000. En aquellos casos en los que solo sea necesario mostrar unos cuantos resultados, por ejemplo, si la interfaz de usuario o la API de aplicación solo devuelven 10 resultados de una vez, también puede reducir el tamaño de página a 10 a fin de reducir el rendimiento consumido en las lecturas y consultas.

   > [!NOTE] 
   > La propiedad maxItemCount no debe usarse únicamente con fines de paginación. Sirve principalmente para mejorar el rendimiento de las consultas al reducir el número máximo de elementos que se devuelven en una página.  

   También puede establecer el tamaño de página mediante los SDK de Azure Cosmos DB disponibles. La propiedad [MaxItemCount](/dotnet/api/microsoft.azure.documents.client.feedoptions.maxitemcount?view=azure-dotnet) de FeedOptions permite establecer el número máximo de elementos que se devolverán en la enumeración. Cuando `maxItemCount` se establece en -1, el SDK busca automáticamente el valor óptimo según el tamaño del documento. Por ejemplo:
    
   ```csharp
    IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
   ```
    
   Cuando se ejecuta una consulta, se envían los datos resultantes en un paquete TCP. Si especifica un valor demasiado bajo para `maxItemCount`, el número de viajes requeridos para enviar los datos dentro del paquete TCP es alto, lo que afecta al rendimiento. Por tanto, si no está seguro de qué valor establecer para la propiedad `maxItemCount`, es mejor establecerla en -1 y dejar que el SDK elija el valor predeterminado. 

9. **Aumento del número de subprocesos y tareas**

    Consulte [Aumento del número de subprocesos y tareas](#increase-threads) en la sección Redes.

## <a name="indexing-policy"></a>Directiva de indexación
 
1. **Exclusión de rutas de acceso sin utilizar de la indexación para acelerar las escrituras**

    La directiva de indexación de Cosmos DB también le permite especificar las rutas de acceso de documentos que se incluirán en la indexación o se excluirán de esta mediante el aprovechamiento de las rutas de acceso de indexación (IndexingPolicy.IncludedPaths y IndexingPolicy.ExcludedPaths). El uso de rutas de acceso de indexación puede ofrecer un rendimiento de escritura mejorado y un almacenamiento de índices reducido en escenarios en los que los patrones de consulta se conocen de antemano, dado que los costos de indexación están directamente correlacionados con el número de rutas de acceso únicas indexadas.  Por ejemplo, en el siguiente código se muestra cómo excluir una sección completa de los documentos (un subárbol) de la indexación mediante el comodín "*".

    ```csharp
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    Para más información, consulte [Directivas de indexación de Azure Cosmos DB](index-policy.md).

## <a name="throughput"></a>Throughput
<a id="measure-rus"></a>

1. **Medición y optimización del uso menor de unidades de solicitud por segundo**

    Azure Cosmos DB ofrece un amplio conjunto de operaciones de base de datos, incluidas consultas relacionales y jerárquicas con funciones definidas por el usuario, procedimientos almacenados y desencadenadores. Todo funciona con los documentos dentro de una colección de base de datos. El costo asociado a cada una de estas operaciones variará en función de la CPU, la E/S y la memoria necesarias para completar la operación. En lugar de administrar y pensar sobre los recursos de hardware, puede pensar en una unidad de solicitud (RU) como una medida única para los recursos necesarios para realizar varias operaciones de la base de datos y dar servicio a una solicitud de la aplicación.

    El rendimiento se aprovisiona en función del número de [unidades de solicitud](request-units.md) establecido para cada contenedor. El consumo de la unidad de solicitud se evalúa como frecuencia por segundo. Las aplicaciones que superan la frecuencia de unidad de solicitud aprovisionada para su contenedor están limitadas hasta que la frecuencia cae por debajo del nivel aprovisionado del contenedor. Si la aplicación requiere un mayor nivel de rendimiento, puede aumentar el rendimiento mediante el aprovisionamiento de unidades de solicitud adicionales. 

    La complejidad de una consulta afecta a la cantidad de unidades de solicitud consumidas para una operación. El número de predicados, la naturaleza de los predicados, el número de UDF y el tamaño del conjunto de datos de origen influyen en el costo de operaciones de consulta.

    Para medir la sobrecarga de cualquier operación (crear, actualizar o eliminar), inspeccione el encabezado [x-ms-request-charge](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) (o la propiedad RequestCharge equivalente en ResourceResponse\<T> o FeedResponse\<T> del SDK de .NET) para medir el número de unidades de solicitud que usan estas operaciones.

    ```csharp
    // Measure the performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure the performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    El cargo de solicitud devuelto en este encabezado es una fracción de la capacidad de proceso aprovisionada (es decir, 2000 RU/segundo). Por ejemplo, si la consulta anterior devuelve 1000 documentos de 1 KB, el costo de la operación será 1000. Por lo tanto, al cabo de un segundo, el servidor atenderá solo dos de estas solicitudes antes de limitar la velocidad de las solicitudes posteriores. Para más información, consulte [Unidades de solicitud](request-units.md) y la [calculadora de unidades de solicitud](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Administración de la limitación de velocidad y la tasa de solicitudes demasiado grande**

    Cuando un cliente intenta superar la capacidad de proceso reservada para una cuenta, no habrá ninguna degradación del rendimiento en el servidor y no se utilizará ninguna capacidad de proceso más allá del nivel reservado. El servidor finalizará de forma preventiva la solicitud con RequestRateTooLarge (código de estado HTTP 429) y devolverá el encabezado [x-ms-retry-after-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) para indicar la cantidad de tiempo, en milisegundos, que el usuario debe esperar antes de volver a intentar realizar la solicitud.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Los SDK capturan implícitamente esta respuesta, respetan el encabezado retry-after especificado por el servidor y reintentan la solicitud. A menos que varios clientes obtengan acceso a la cuenta al mismo tiempo, el siguiente reintento se realizará correctamente.

    Si tiene más de un cliente que funciona acumulativamente de forma coherente por encima de la tasa de solicitudes, puede que el número de reintentos predeterminado establecido actualmente en 9 de manera interna por el cliente no sea suficiente; en este caso, el cliente producirá una excepción DocumentClientException con el código de estado 429 para la aplicación. El número de reintentos predeterminado se puede cambiar estableciendo RetryOptions en la instancia ConnectionPolicy. De forma predeterminada, la excepción DocumentClientException con el código de estado 429 se devuelve tras un tiempo de espera acumulativo de 30 segundos si la solicitud sigue funcionando por encima de la tasa de solicitudes. Esto sucede incluso cuando el número de reintentos actual es inferior al número de reintentos máximo de 9, el valor predeterminado, o un valor definido por el usuario.

    Aunque el comportamiento de reintento automático ayuda a mejorar la resistencia y la usabilidad en la mayoría de las aplicaciones, podría no resultar ventajoso al realizar comparativas de rendimiento, en especial al medir la latencia.  La latencia observada del cliente aumentará si el experimento llega a la limitación del servidor y hace que el SDK del cliente realice reintentos de forma silenciosa. Para evitar aumentos de latencia durante los experimentos de rendimiento, mida el gasto devuelto por cada operación y asegúrese de que las solicitudes funcionan por debajo de la tasa de solicitudes observada. Para más información, consulte [Unidades de solicitud](request-units.md).
3. **Diseño de documentos más pequeños para un mayor rendimiento**

    El gasto de solicitud (es decir, el costo de procesamiento de solicitudes) de una operación dada está directamente correlacionado con el tamaño del documento. Las operaciones con documentos grandes cuestan más que las operaciones con documentos pequeños.

## <a name="next-steps"></a>Pasos siguientes
Si quiere ver una aplicación de ejemplo usada para evaluar Azure Cosmos DB en escenarios de alto rendimiento en un pequeño número de máquinas cliente, consulte [Pruebas de escala y rendimiento con Azure Cosmos DB](performance-testing.md).

Para más información sobre cómo diseñar la aplicación para escalarla y obtener un alto rendimiento, consulte [Partición y escalado en Azure Cosmos DB](partition-data.md).
