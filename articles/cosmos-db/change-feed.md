---
title: Compatibilidad con la fuente de cambios en Azure Cosmos DB | Microsoft Docs
description: Use la compatibilidad con la fuente de cambios de Azure Cosmos DB para controlar los cambios en documentos y realizar el procesamiento basado en eventos tales como desencadenadores y mantener actualizados las cachés y los sistemas de análisis.
keywords: fuente de cambios
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: b6d05c5e9bc59df9df7ef8840b70ab027b6e2f74
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269503"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Compatibilidad con la fuente de cambios en Azure Cosmos DB

[Azure Cosmos DB](../cosmos-db/introduction.md) es una base de datos replicada globalmente, rápida y flexible, ideal para aplicaciones IoT, juegos, comerciales y de registro operativo. Un modelo de diseño común en estas aplicaciones es usar los cambios en los datos para iniciar acciones adicionales. Estas acciones adicionales podrían ser cualquiera de las siguientes: 

* Desencadenamiento de una notificación o una llamada a una API cuando se inserta o modifica un documento.
* Procesamiento del flujo para IoT o análisis.
* Movimiento de datos adicionales mediante la sincronización con una memoria caché, el motor de búsqueda o el almacén de datos, o archivar los datos en un almacenamiento en frío.

La **compatibilidad con la fuente de cambios**  en Azure Cosmos DB le permite crear soluciones eficientes y escalables para cada uno de estos patrones, como se muestra en la imagen siguiente:

![Uso de la fuente de cambios de Azure Cosmos DB para aumentar la eficacia de los escenarios de informática orientada a eventos y análisis en tiempo real](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> La compatibilidad con la fuente de cambios se proporciona para todos los contenedores y modelos de datos de Azure Cosmos DB. Sin embargo, la fuente de cambios se lee mediante el cliente de SQL y serializa los elementos en formato JSON. Debido al formato JSON, los clientes de MongoDB experimentarán una discrepancia entre los documentos con formato BSON y la fuente de cambios con formato JSON.

## <a name="how-does-change-feed-work"></a>¿Cómo funciona la fuente de cambios?

La compatibilidad con la fuente de cambios en Azure Cosmos DB se proporciona al observar si hay algún cambio en una colección de Azure Cosmos DB. A continuación, muestra la lista ordenada de los documentos que han cambiado en el orden en el que se modificaron. Los cambios se conservan y se pueden procesar de manera asincrónica. El resultado puede distribuirse entre uno o varios clientes para procesarse en paralelo. 

Puede leer la fuente de cambios de tres maneras diferentes, como se describe más adelante en este artículo:

*   [Uso de Azure Functions](#azure-functions)
*   [Uso del SDK de Azure Cosmos DB](#sql-sdk)
*   [Uso de la biblioteca de procesador de fuente de cambios de Azure Cosmos DB](#change-feed-processor)

La fuente de cambios está disponible para cada intervalo de claves de partición dentro de la colección de documentos y, por tanto, se puede distribuir entre uno o varios consumidores para el procesamiento en paralelo, tal y como se muestra en la imagen siguiente.

![Procesamiento distribuido de la fuente de cambios de Azure Cosmos DB](./media/change-feed/changefeedvisual.png)

Detalles adicionales:
* La fuente de cambios está habilitada de forma predeterminada para todas las cuentas.
* Puede usar el [procesamiento aprovisionado](request-units.md) en su región de escritura o en cualquier [región de lectura](distribute-data-globally.md) para leer la fuente de cambios, igual que con cualquier otra operación de Azure Cosmos DB.
* La fuente de cambios incluye inserciones y operaciones de actualización realizadas en los documentos dentro de la colección. Puede capturar eliminaciones estableciendo un indicador "eliminación temporal" dentro de los documentos en lugar de eliminaciones. Como alternativa, puede establecer un período finito de caducidad para los documentos mediante la [funcionalidad TTL](time-to-live.md), por ejemplo, 24 horas, y usar el valor de esa propiedad para capturar las eliminaciones. Con esta solución, tiene que procesar los cambios dentro de un intervalo de tiempo más corto que el período de expiración de TTL.
* Cada cambio realizado en un documento aparece exactamente una vez en la fuente de cambios y los clientes administran su lógica de puntos de comprobación. La biblioteca de procesadores de fuentes de cambio proporciona semántica de puntos de comprobación automáticos y "al menos una vez".
* Solo el cambio más reciente en un documento determinado se incluye en el registro de cambios. Puede que los cambios intermedios no estén disponibles.
* La fuente de cambios está clasificada por orden de modificación dentro de cada valor de clave de partición. No hay ningún orden garantizado entre valores de clave de partición.
* Los cambios se pueden sincronizar desde cualquier punto en el tiempo, es decir, no hay ningún período fijo de retención de datos en el que los cambios estén disponibles.
* Los cambios están disponibles en fragmentos de intervalos de claves de partición. Esta funcionalidad permite que los cambios de colecciones grandes se procesen en paralelo por medio de varios consumidores/servidores.
* Las aplicaciones pueden solicitar varias fuentes de cambios a la vez en la misma colección.
* ChangeFeedOptions.StartTime se puede utilizar para proporcionar un punto de partida inicial, por ejemplo, para encontrar el token de continuación correspondiente a la hora de reloj facilitada. ContinuationToken, si se especifica, tiene prioridad sobre los valores StartTime y StartFromBeginning. La precisión de ChangeFeedOptions.StartTime es aproximadamente de 5 segundos. 

## <a name="use-cases-and-scenarios"></a>Casos de uso y escenarios

La fuente de cambios permite procesar con eficacia grandes conjuntos de datos con un elevado volumen de escrituras, y ofrece una alternativa a la consulta de conjuntos de datos enteros para identificar lo que ha cambiado. 

Por ejemplo, con una fuente de cambios, puede realizar las siguientes tareas de manera eficaz:

* Actualizar una caché, un índice de búsqueda o un almacenamiento de datos con los datos almacenados en Azure Cosmos DB.
* Implementar archivado y niveles de datos de aplicación, es decir, almacenar "datos activos" en Azure Cosmos DB y "jubilar datos inactivos" en [Azure Blob Storage](../storage/common/storage-introduction.md) o [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Realizar migraciones con cero tiempo de inactividad a otra cuenta de Azure Cosmos DB con otro esquema de partición.
* Implementar [canalizaciones Lambda en Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) con Azure Cosmos DB. Azure Cosmos DB proporciona una solución de base de datos escalable que puede hacer frente tanto a la ingesta como a la consulta, e implementar arquitecturas Lambda con un bajo TCO. 
* Recibir y almacenar datos de eventos de dispositivos, sensores, infraestructura y aplicaciones, y luego procesarlos en tiempo real con [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md) o [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

La imagen siguiente muestra cómo las canalizaciones lambda que tanto ingieren como consultan Azure Cosmos DB pueden usar la compatibilidad con fuentes de cambios: 

![Canalización Lambda basada en Azure Cosmos DB para ingesta y consulta](./media/change-feed/lambda.png)

Además, en las aplicaciones web y móviles [sin servidor](http://azure.com/serverless), puede realizar el seguimiento de eventos, como cambios en el perfil del cliente, las preferencias o la ubicación para desencadenar determinadas acciones como enviar notificaciones push a sus dispositivos mediante [Azure Functions](#azure-functions). Si usa Azure Cosmos DB para compilar un juego, puede usar la fuente de cambios para, por ejemplo, implementar marcadores en tiempo real basados en las puntuaciones de los juegos completados.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Uso de Azure Functions 

Si usa Azure Functions, la manera más sencilla de conectarse a una fuente de cambios de Azure Cosmos DB es agregar un desencadenador de Azure Cosmos DB a la aplicación Azure Functions. Cuando cree un desencadenador de Azure Cosmos DB en una aplicación de Azure Functions, seleccione la colección de Azure Cosmos DB a la que conectarse, y la función se desencadena siempre que se realiza un cambio en la colección. 

Los desencadenadores pueden crearse en el portal de Azure Functions, en el portal de Azure Cosmos DB o mediante programación. Para más información, vea [Azure Cosmos DB: informática de base de datos sin servidor con Azure Functions](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>Uso del SDK

El [SDK de SQL](sql-api-sdk-dotnet.md) para Azure Cosmos DB le otorga la capacidad de leer y administrar una fuente de cambios. Pero con la eficacia, también vienen una gran cantidad de responsabilidades, demasiadas. Si desea administrar puntos de control, tratar con números de secuencia de documentos y tener un control específico sobre las claves de las particiones, el SDK puede ser la solución correcta.

En esta sección se explica cómo usar el SDK de SQL para trabajar con una fuente de cambios.

1. Empiece por leer los siguientes recursos desde appconfig. Las instrucciones sobre cómo recuperar la clave de autorización y puntos de conexión están disponibles en [Actualización de la cadena de conexión](create-sql-api-dotnet.md#update-your-connection-string).

    ``` csharp
    DocumentClient client;
    string DatabaseName = ConfigurationManager.AppSettings["database"];
    string CollectionName = ConfigurationManager.AppSettings["collection"];
    string endpointUrl = ConfigurationManager.AppSettings["endpoint"];
    string authorizationKey = ConfigurationManager.AppSettings["authKey"];
    ```

2. Cree el cliente como se indica a continuación:

    ```csharp
    using (client = new DocumentClient(new Uri(endpointUrl), authorizationKey,
    new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    {
    }
    ```

3. Obtenga los intervalos de claves de partición:

    ```csharp
    FeedResponse pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri,
        new FeedOptions
            {RequestContinuation = pkRangesResponseContinuation });
     
    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    ```

4. Llame a ExecuteNextAsync para cada intervalo de claves de partición:

    ```csharp
    foreach (PartitionKeyRange pkRange in partitionKeyRanges){
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);
        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collectionUri,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = -1,
                // Set reading time: only show change feed results modified since StartTime
                StartTime = DateTime.Now - TimeSpan.FromSeconds(30)
            });
        while (query.HasMoreResults)
            {
                FeedResponse<dynamic> readChangesResponse = query.ExecuteNextAsync<dynamic>().Result;
    
                foreach (dynamic changedDocument in readChangesResponse)
                    {
                         Console.WriteLine("document: {0}", changedDocument);
                    }
                checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
            }
    }
    ```

> [!NOTE]
> En lugar de `ChangeFeedOptions.PartitionKeyRangeId`, puede usar `ChangeFeedOptions.PartitionKey` para especificar una clave de partición única para la que obtener una fuente de cambios. Por ejemplo, `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Si tiene varios lectores, puede usar **ChangeFeedOptions** para distribuir la carga de lectura en subprocesos o clientes distintos.

Y eso es todo, con estas pocas líneas de código puede empezar a leer la fuente de cambios. Puede obtener el código completo que se usa en este artículo desde el [repositorio de GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

En el código del paso 4 anterior, el fragmento **ResponseContinuation** de la última línea tiene el último número de secuencia lógica (LSN) del documento, que se usará la próxima vez que lea documentos nuevos tras este número de secuencia. Mediante el uso del valor **StartTime** de **ChangeFeedOption** puede ampliar la red para obtener los documentos. Por lo tanto, si el valor de **ResponseContinuation** es null, pero **StartTime** vuelve atrás en el tiempo, se le devolverán todos los documentos que cambiaron desde el momento que indica **StartTime**. Pero, si **ResponseContinuation** tiene un valor, el sistema le ayudará a obtener todos los documentos desde ese LSN.

Por lo tanto, la matriz de puntos de control solo mantiene el LSN para cada partición. Pero si no desea ocuparse de las particiones, los puntos de control, LSN, las horas de inicio, etc., la opción más sencilla consiste en usar la biblioteca de procesadores de fuente de cambios.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Uso de la biblioteca de procesadores de fuente de cambios 

La [biblioteca de procesadores de fuente de cambios de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) puede ayudarle a distribuir fácilmente el procesamiento de eventos entre varios consumidores. Esta biblioteca simplifica la lectura de los cambios a través de las particiones y varios subprocesos que trabajan en paralelo.

La ventaja principal de la biblioteca de procesadores de fuente de cambios es que no tiene que administrar cada partición y token de continuación, ni que sondear manualmente cada colección.

La biblioteca de procesadores de fuente de cambios simplifica la lectura de los cambios a través de las particiones y varios subprocesos que trabajan en paralelo.  Administra automáticamente la lectura de los cambios a través de las particiones que usan un mecanismo de concesión. Como puede ver en la imagen siguiente, si inicia dos clientes que usan la biblioteca de procesadores de fuente de cambios, dividen el trabajo entre ellos. A medida que continúe aumentando los clientes, siguen dividiéndose el trabajo entre ellos.

![Procesamiento distribuido de la fuente de cambios de Azure Cosmos DB](./media/change-feed/change-feed-output.png)

El cliente de la izquierda se inició en primer lugar y comenzó la supervisión de todas las particiones; a continuación, se inició el segundo cliente y el primero dejó ir a algunas de las concesiones para el segundo cliente. Como puede ver, esta es la forma "agradable" de distribuir el trabajo entre distintas máquinas y clientes.

Tenga en cuenta que, si tiene dos funciones de Azure sin servidor supervisando la misma colección y usando la misma concesión, las dos funciones pueden obtener documentos diferentes, en función del modo en que la biblioteca de procesadores decida procesar las particiones.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Información de la biblioteca de procesadores de fuente de cambios

Hay cuatro componentes principales en la implementación de la biblioteca de procesadores de fuente de cambios: la colección supervisada, la colección de concesión, el host de procesador y los consumidores. 

> [!WARNING]
> Crear una colección implica precios, debido a que reserva rendimiento para que la aplicación se comunique con Azure Cosmos DB. Para más detalles, visite la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

**Colección supervisada:** La colección supervisada consiste en los datos desde los que se genera la fuente de cambios. Todas las inserciones y cambios realizados en la colección supervisada se reflejan en la fuente de cambios de la colección. 

**Colección de concesión:** La colección de concesión coordina el procesamiento de la fuente de cambios entre varios trabajadores. Se utiliza una colección independiente para almacenar las concesiones con una concesión por partición. Resulta ventajoso almacenar esta colección de concesión en una cuenta diferente con la región de escritura más cerca de donde se está ejecutando el procesador de fuente de cambios. Un objeto de concesión contiene los siguientes atributos: 
* Propietario: Especifica el host que posee la concesión.
* Continuación: Especifica la posición (token de continuación) en la fuente de cambios para una partición determinada.
* Marca de tiempo: Última vez que se actualizó la concesión; la marca de tiempo se puede utilizar para comprobar si la concesión se considera expirada. 

**Host de procesador:** Cada host determina cuántas particiones se van a procesar según la cantidad de otras instancias de host que tienen concesiones activas. 
1.  Cuando se inicia un host, adquiere concesiones para equilibrar la carga de trabajo en todos los hosts. Un host renueva periódicamente concesiones, por lo que las concesiones permanecen activas. 
2.  Un host establece puntos de control en el último token de continuación para su concesión para cada lectura. Para garantizar la seguridad de simultaneidad, un host comprueba ETag para cada actualización de concesión. También se admiten otras estrategias de puntos de control.  
3.  Tras el cierre, un host libera todas las concesiones pero mantiene la información de continuación, por lo que puede reanudar la lectura más adelante desde el punto de control almacenado. 

En este momento, el número de hosts no puede ser mayor que el número de particiones (concesiones).

**Consumidores:** Los consumidores, o trabajadores, son subprocesos que realizan el procesamiento de fuentes de cambios iniciado por cada host. Cada host de procesador puede tener varios consumidores. Cada consumidor lee la fuente de cambios de la partición a la que se ha asignado y notifica a su host los cambios y las concesiones expiradas.

Para comprender mejor cómo funcionan estos cuatro elementos del procesador de fuente de cambios juntos, echemos un vistazo a un ejemplo en el diagrama siguiente. La colección supervisada almacena documentos y utiliza "city" como la clave de partición. Vemos que la partición azul contiene documentos con el campo "city" de "A-e", etc. Hay dos hosts, cada uno con dos consumidores, que leen las cuatro particiones en paralelo. Las flechas muestran a los consumidores leyendo un punto específico de la fuente de cambios. En la primera partición, el azul más oscuro representa los cambios no leídos, mientras que el azul claro representa los cambios ya leídos de la fuente de cambios. Los hosts utilizan la colección de concesión para almacenar un valor de "continuación" para realizar un seguimiento de la posición de lectura actual para cada consumidor. 

![Uso del host de procesador de fuente de cambios de Azure Cosmos DB](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Trabajo con la biblioteca de procesadores de fuente de cambios

Antes de instalar el paquete NuGet del procesador de fuente de cambios, instale primero: 

* Microsoft.Azure.DocumentDB, versión más reciente.
* Newtonsoft.Json, versión más reciente

A continuación, instale el [paquete Nuget Microsoft.Azure.DocumentDB.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) e inclúyalo como una referencia.

Para implementar la biblioteca de procesadores de fuente de cambios, tiene que hacer lo siguiente:

1. Implemente un objeto **DocumentFeedObserver**, que implementa **IChangeFeedObserver**.
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;
    using Microsoft.Azure.Documents.Client;

    /// <summary>
    /// This class implements the IChangeFeedObserver interface and is used to observe 
    /// changes on change feed. ChangeFeedEventHost will create as many instances of 
    /// this class as needed. 
    /// </summary>
    public class DocumentFeedObserver : IChangeFeedObserver
    {
    private static int totalDocs = 0;

        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserver" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        /// <param name="client"> Client connected to destination collection </param>
        /// <param name="destCollInfo"> Destination collection information </param>
        public DocumentFeedObserver()
        {
            
        }

        /// <summary>
        /// Called when change feed observer is opened; 
        /// this function prints out observer partition key id. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task OpenAsync(IChangeFeedObserverContext context)
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Observer opened for partition Key Range: {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;
        }

        /// <summary>
        /// Called when change feed observer is closed; 
        /// this function prints out observer partition key id and reason for shut down. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <param name="reason">Specifies the reason the observer is closed.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task CloseAsync(IChangeFeedObserverContext context, ChangeFeedObserverCloseReason reason)
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("Observer closed, {0}", context.PartitionKeyRangeId);
            Console.WriteLine("Reason for shutdown, {0}", reason);
            return Task.CompletedTask;
        }

        public Task ProcessChangesAsync(IChangeFeedObserverContext context, IReadOnlyList<Document> docs, CancellationToken cancellationToken)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Change feed: PartitionId {0} total {1} doc(s)", context.PartitionKeyRangeId, Interlocked.Add(ref totalDocs, docs.Count));
            foreach (Document doc in docs)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(doc.Id.ToString());
            }

            return Task.CompletedTask;
        }
    }
    ```

2. Implemente un objeto **DocumentFeedObserverFactory**, que implementa **IChangeFeedObserverFactory**.
    ```csharp
     using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;

    /// <summary>
    /// Factory class to create instance of document feed observer. 
    /// </summary>
    public class DocumentFeedObserverFactory : IChangeFeedObserverFactory
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserverFactory" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        public DocumentFeedObserverFactory()
        {
        }

        /// <summary>
        /// Creates document observer instance with client and destination collection information
        /// </summary>
        /// <returns>DocumentFeedObserver with client and destination collection information</returns>
        public IChangeFeedObserver CreateObserver()
        {
            DocumentFeedObserver newObserver = new DocumentFeedObserver();
            return newObserver as IChangeFeedObserver;
        }
    }
    ```

3. Defina *CancellationTokenSource* y *ChangeFeedProcessorBuilder*.

    ```csharp
    private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
    private readonly ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorBuilder();
    ```

5. Cree el objeto **ChangeFeedProcessorBuilder** después de definir los objetos pertinentes. 

    ```csharp
            string hostName = Guid.NewGuid().ToString();
      
            // monitored collection info 
            DocumentCollectionInfo documentCollectionInfo = new DocumentCollectionInfo
            {
                Uri = new Uri(this.monitoredUri),
                MasterKey = this.monitoredSecretKey,
                DatabaseName = this.monitoredDbName,
                CollectionName = this.monitoredCollectionName
            };
            
            DocumentCollectionInfo leaseCollectionInfo = new DocumentCollectionInfo
                {
                    Uri = new Uri(this.leaseUri),
                    MasterKey = this.leaseSecretKey,
                    DatabaseName = this.leaseDbName,
                    CollectionName = this.leaseCollectionName
                };
            DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory();
       
            ChangeFeedProcessorOptions feedProcessorOptions = new ChangeFeedProcessorOptions();

            // ie. customizing lease renewal interval to 15 seconds
            // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
            feedProcessorOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);
            feedProcessorOptions.StartFromBeginning = true;

            this.builder
                .WithHostName(hostName)
                .WithFeedCollection(documentCollectionInfo)
                .WithLeaseCollection(leaseCollectionInfo)
                .WithProcessorOptions (feedProcessorOptions)
                .WithObserverFactory(new DocumentFeedObserverFactory());               
                //.WithObserver<DocumentFeedObserver>();  If no factory then just pass an observer

            var result =  await this.builder.BuildAsync();
            await result.StartAsync();
            Console.Read();
            await result.StopAsync();    
    ```

¡Ya está! Después de estos pasos, los documentos empezarán a mostrarse en el método **DocumentFeedObserver.ProcessChangesAsync**.

El código anterior es para fines de ilustración para mostrar diferentes tipos de objetos y su interacción. Tendrá que definir las variables adecuadas e iniciarlas con los valores correctos. Puede obtener el código completo que se usa en este artículo desde el [repositorio de GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessorV2).

> [!NOTE]
> Nunca debe tener una clave maestra en el código o en el archivo de configuración como se muestra en el código anterior. Consulte [cómo usar Key-Vault para recuperar las claves](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## <a name="faq"></a>Preguntas más frecuentes

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>¿Cuáles son las distintas formas en que puede leer la fuente de cambios? ¿Cuándo debe usar cada método?

Hay tres opciones para leer la fuente de cambios:

* **[Mediante el SDK de .NET de la API de SQL de Azure Cosmos DB](#sql-sdk)**
   
   Con este método, obtendrá un bajo nivel de control sobre la fuente de cambios. Puede administrar el punto de control, puede acceder a una clave de partición determinada, etc. Si tiene varios lectores, puede usar [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) para distribuir la carga de lectura en subprocesos o clientes distintos. .

* **[Mediante la biblioteca de procesadores de fuente de cambios de Azure Cosmos DB](#change-feed-processor)**

   Si quiere externalizar gran parte de la complejidad de la fuente de cambios, puede usar la biblioteca de procesadores de fuente de cambios. Esta biblioteca oculta gran parte de la complejidad, pero de todos modos le permite controlar completamente la fuente de cambios. Esta biblioteca sigue un [patrón de observador](https://en.wikipedia.org/wiki/Observer_pattern) en el que el SDK llama a la función de procesamiento. 

   Si tiene una fuente de cambios de alto rendimiento, puede crear instancias de varios clientes para leer la fuente de cambios. Como usa la "biblioteca de procesadores de fuente de cambios", dividirá automáticamente la carga en distintos clientes. No tiene que hacer nada. El SDK controla toda la complejidad. Sin embargo, si quiere tener su propio equilibrador de carga, puede implementar IParitionLoadBalancingStrategy para una estrategia de partición personalizada. Implemente IPartitionProcessor para procesar de manera personalizada los cambios de una partición. Sin embargo, con el SDK puede procesar un intervalo de partición, pero si quiere procesar una clave de partición determinada, tendrá que usar el SDK para la API de SQL.

* **[Mediante Azure Functions](#azure-functions)** 
   
   La última opción, Azure Functions, es la opción más simple. Se recomienda usar esta opción. Cuando cree un desencadenador de Azure Cosmos DB en una aplicación de Azure Functions, seleccione la colección de Azure Cosmos DB a la que conectarse, y la función se desencadenará siempre que se realice un cambio en la colección. Vea un [presentación en pantalla](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) sobre cómo usar Azure Functions y la fuente de cambios.

   Los desencadenadores pueden crearse en el portal de Azure Functions, en el portal de Azure Cosmos DB o mediante programación. Visual Studio y VS Code cuentan con un elevado nivel de compatibilidad para escribir Azure Functions. Puede escribir y depurar el código en el escritorio y, luego, implementar la función con un solo clic. Para más información, consulte el artículo [Azure Cosmos DB: informática de base de datos sin servidor con Azure Functions](serverless-computing-database.md).

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>¿Cuál es el criterio de ordenación de los documentos en la fuente de cambios?

Los documentos de la fuente de cambios están ordenados según su hora de modificación. Este criterio de ordenación solo se garantiza por partición.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>En una cuenta de varias regiones, ¿qué sucede con la fuente de cambios cuando se realiza la conmutación por error de la región de escritura? ¿También se realiza la conmutación por error de la fuente de cambios? ¿La fuente de cambios seguiría apareciendo como contigua o la conmutación por error reiniciaría la fuente de cambios?

Sí, la fuente de cambios funcionará entre la operación de conmutación por error manual y será contigua.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>¿Durante cuánto tiempo la fuente de cambios conserva los datos modificados si la propiedad TTL (período de vida) se establece en -1 para el documento?

La fuente de cambios se conservará para siempre. Si los datos no se eliminan, se mantendrán en la fuente de cambios.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>¿Cómo puedo configurar Azure Functions para leer de una región determinada si la fuente de cambios está disponible en todas las regiones de lectura de manera predeterminada?

Actualmente no es posible configurar Azure Functions para leer desde una región determinada. Hay un problema de GitHub en el repositorio de Azure Functions para establecer las regiones de preferencia de cualquier desencadenador y enlace de Azure Cosmos DB.

Azure Functions usa la directiva de conexión predeterminada. Puede configurar el modo de conexión en Azure Functions y, de manera predeterminada, lee desde la región de escritura, por lo que se recomienda reubicar Azure Functions en la misma región.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>¿Cuál es el tamaño predeterminado de lotes en Azure Functions?

100 documentos en cada invocación de Azure Functions. Sin embargo, este número se puede configurar dentro del archivo function.json. Esta es una [lista completa de las opciones de configuración](../azure-functions/functions-run-local.md). Si está desarrollando de manera local, actualice la configuración de la aplicación dentro del archivo [local.settings.json](../azure-functions/functions-run-local.md).

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Estoy configurando una colección y leyendo su fuente de cambios, pero veo que no recibo todos los documentos insertados y que faltan algunos documentos. ¿Qué está pasando?

Asegúrese de que ninguna otra función lee la misma colección con la misma colección de concesión. Esto me pasó a mí y me di cuenta de que otra instancia de Azure Functions procesa los documentos faltantes, que también usa la misma concesión.

Por lo tanto, si crea varias instancias de Azure Functions para leer la misma fuente de cambios, deben usar distintas colecciones de concesión o usar la configuración "leasePrefix" para compartir la misma colección. Sin embargo, cuando usa la biblioteca de procesadores de fuente de cambios, puede iniciar varias instancias de la función y el SDK dividirá los documentos en distintas instancias de manera automática.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>El documento se actualiza a cada segundo y no recibo todos los cambios en Azure Functions donde se escucha la fuente de cambios.

Azure Functions sondea la fuente de cambios cada 5 segundos, por lo que se pierde cualquier cambio hecho entre esos 5 segundos. Azure Cosmos DB almacena solo una versión cada 5 segundos, por lo que obtendrá el quinto cambio en el documento. Sin embargo, si quiere bajar de los 5 segundos y sondear la fuente de cambios cada segundo, puede configurar el tiempo de sondeo "feedPollTime". Para saber cómo hacerlo, consulte [Enlaces de Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Se define en milisegundos con un valor predeterminado de 5000. Es posible bajar del segundo, pero no lo recomendamos, porque empezará a consumir más CPU.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Inserté un documento en la colección de la API de Mongo, pero cuando veo el documento en la fuente de cambios, muestra un valor de identificador distinto. ¿Qué está pasando?

Su colección es la colección de la API de Mongo. Recuerde que la fuente de cambios se lee mediante el cliente de SQL y serializa los elementos en formato JSON. Debido al formato JSON, los clientes de MongoDB experimentarán una discrepancia entre los documentos con formato BSON y la fuente de cambios con formato JSON. Lo que ve es la representación de un documento BSON en JSON. Si usa atributos binarios en cuentas de Mongo, se convierten a JSON.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>¿Hay alguna manera de controlar la fuente de cambios solo para actualizaciones y no inserciones?

No en la actualidad, pero esta funcionalidad está pensada para el futuro. Hoy en día, puede agregar un marcador parcial en los documentos para las actualizaciones.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>¿Hay alguna manera de obtener eliminaciones en la fuente de cambios?

Actualmente, la fuente de cambios no registra las eliminaciones. La fuente de cambios mejora constantemente y esta funcionalidad está pensada para el futuro. Hoy en día, puede agregar un marcador parcial en los documentos para las eliminaciones. Agregue un atributo en el documento llamado "deleted" (eliminado), establézcalo en "true" y establezca un TTL en el documento para que se pueda eliminar automáticamente.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>¿Puedo leer la fuente de cambios de documentos históricos (por ejemplo, documentos que se agregaron hace 5 años)?

Sí, si no se eliminó el documento puede leer la fuente de cambios hasta el origen de la colección.

### <a name="can-i-read-change-feed-using-javascript"></a>¿Puedo leer la fuente de cambios con JavaScript?

Sí, recientemente se agregó la compatibilidad inicial del SDK de Node.js con la fuente de cambios. Se puede usar como se muestra en el ejemplo siguiente. Actualice el módulo documentdb a la versión actual antes de ejecutar el código:

```js

var DocumentDBClient = require('documentdb').DocumentClient;
const host = "https://your_host:443/";
const masterKey = "your_master_key==";
const databaseId = "db";
const collectionId = "c1";
const dbLink = 'dbs/' + databaseId;
const collLink = dbLink + '/colls/' + collectionId;
var client = new DocumentDBClient(host, { masterKey: masterKey });
let options = {
    a_im: "Incremental feed",
    accessCondition: {
        type: "IfNoneMatch",        // Use: - empty condition (or remove accessCondition entirely) to start from beginning.
        //      - '*' to start from current.
        //      - specific etag value to start from specific continuation.
        condition: ""
    }
};
 
var query = client.readDocuments(collLink, options);
query.executeNext((err, results, headers) =&gt; {
    // Now we have headers.etag, which can be used in next readDocuments in accessCondition option.
    console.log(results);
    console.log(headers.etag);
    console.log(results.length);
    options.accessCondition = { type: "IfNoneMatch", condition: headers.etag };
    var query = client.readDocuments(collLink, options);
    query.executeNext((err, results, headers) =&gt; {
        console.log("next one:", results[0]);
    });
});<span id="mce_SELREST_start" style="overflow:hidden;line-height:0;"></span>

```

### <a name="can-i-read-change-feed-using-java"></a>¿Puedo leer la fuente de cambios con Java?

La biblioteca de Java para leer la fuente de cambios está disponible en el [repositorio de Github](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). Sin embargo, actualmente las versiones de la biblioteca de Java son algo anteriores a las versiones de la biblioteca de .NET. Ambas bibliotecas pronto estarán sincronizadas.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>¿Puedo usar _etag, _lsn o _ts para contabilidad interna? ¿Qué obtengo como respuesta?

El formato _etag es interno y no debe depender de él (no lo analice), porque puede cambiar en cualquier momento.
_ts es la marca de tiempo de la modificación o de la creación. Puede usar _ts para la comparación cronológica.
_lsn es un identificador de lote que solo se agrega para la fuente de cambios y representa el identificador de transacción del almacén. Muchos documentos pueden tener el mismo _lsn.
Otro aspecto que se debe tener en cuenta es que ETag en FeedResponse es distinto de _etag que ve en el documento. _etag es un identificador interno que se usa para la simultaneidad e indica la versión del documento y el ETag que se usa para secuenciar la fuente.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>¿Leer la fuente de cambios agrega algún costo adicional?

Se le cobrará por las unidades de solicitud consumidas, es decir, el movimiento de datos dentro y fuera de las colecciones de Azure Cosmos DB siempre consume RU. A los usuarios se les cobrará por las unidades de solicitud que consume la colección de concesión.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>¿Varias instancias de Azure Functions pueden leer la fuente de cambios de una colección?

Sí. Varias instancias de Azure Functions pueden leer la fuente de cambios de la misma colección. Sin embargo, las instancias de Azure Functions deben tener definido un leaseCollectionPrefix independiente.

### <a name="should-the-lease-collection-be-partitioned"></a>¿Es necesario particionar la colección de concesión?

No, la colección de concesión se puede corregir. No es necesario particionar la colección de concesión, un proceso que no es compatible actualmente.

### <a name="can-i-read-change-feed-from-spark"></a>¿Puedo leer la fuente de cambios desde Spark?

Sí, puede hacerlo. Consulte el artículo sobre el [conector de Spark para Azure Cosmos DB](spark-connector.md). Esta es una [presentación en pantalla](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) que muestra cómo se puede procesar la fuente de cambios como un flujo estructurado.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Si estoy procesando una fuente de cambios con Azure Functions, como un lote de 10 documentos, y recibo un error en el séptimo documento. En ese caso, no se procesan los últimos tres documentos. ¿Cómo puedo empezar el procesamiento desde el documento con errores (es decir, el séptimo documento) en la próxima fuente?

Para controlar el error, el patrón recomendado es encapsular el código con el bloque try-catch. Detecte el error y coloque ese documento en una cola (de mensajes fallidos) y defina la lógica para manejar los documentos que generaron el error. Con este método, si tiene un lote de 200 documentos y solo un documento con errores, no tiene que deshacerse de todo el lote.

Si se produce un error, no debe rebobinar al punto de control para comenzar otra acción. Puede seguir obteniendo esos documentos de la fuente de cambios. Recuerde que la fuente de cambios conserva la última instantánea final de los documentos, por lo que puede perder la instantánea anterior en el documento. La fuente de cambios solo conserva una última versión del documento y, entremedio, pueden venir otros procesos y cambiar el documento.

Mientras sigue corrigiendo el código, pronto no encontrará documentos en la cola de mensajes fallidos.
La fuente de cambios llama automáticamente a Azure Functions y Azure Functions mantiene de manera interna el punto de control, el sistema, etc. Si quiere revertir el punto de control y controlar cada uno de sus aspectos, debe considerar la posibilidad de usar el SDK del procesador de la fuente de cambios.


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca Azure Cosmos DB con Azure Functions, consulte [Azure Cosmos DB: informática de base de datos sin servidor con Azure Functions](serverless-computing-database.md).

Para más información sobre cómo usar la biblioteca de procesadores de fuente de cambios, use los siguientes recursos:

* [Página de información](sql-api-sdk-dotnet-changefeed.md) 
* [Paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Código de ejemplo que muestra los pasos del 1 al 6 anterior](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Ejemplos adicionales en GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Para más información sobre cómo utilizar la fuente de cambios mediante el SDK, use los siguientes recursos:

* [Página de información de SDK](sql-api-sdk-dotnet.md)
