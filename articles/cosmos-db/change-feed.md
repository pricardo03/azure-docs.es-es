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
ms.openlocfilehash: 6b0aaa075b8b2881e269d79a67e75528d0d9a86a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129865"
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

En el siguiente vídeo, Andrew Liu, administrador de programas de Azure Cosmos DB, muestra cómo funciona la fuente de cambios de Azure Cosmos DB.

> [!VIDEO https://www.youtube.com/embed/mFnxoxeXlaU]
>
>

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
            ChangeFeedOptions feedOptions = new ChangeFeedOptions();

            /* ie customize StartFromBeginning so change feed reads from beginning
                can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
            */

            feedOptions.StartFromBeginning = true;
        
            ChangeFeedProcessorOptions feedProcessorOptions = new ChangeFeedProcessorOptions();

            // ie. customizing lease renewal interval to 15 seconds
            // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
            feedProcessorOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

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

That’s it. After these few steps documents will start showing up into the **DocumentFeedObserver.ProcessChangesAsync** method.

Above code is for illustration purpose to show different kind of objects and their interaction. You have to define proper variables and initiate them with correct values. You can get the complete code used in this article from the [GitHub repo](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessorV2).

> [!NOTE]
> You should never have a master key in your code or in config file as shown in above code. Please see [how to use Key-Vault to retrive the keys](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## FAQ

### What are the different ways you can read Change Feed? and when to use each method?

There are three options for you to read change feed:

* **[Using Azure Cosmos DB SQL API .NET SDK](#sql-sdk)**
   
   By using this method, you get low level of control on change feed. You can manage the checkpoint, you can access a particular partition key etc. If you have multiple readers, you can use [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) to distribute read load to different threads or different clients. .

* **[Using the Azure Cosmos DB change feed processor library](#change-feed-processor)**

   If you want to outsource lot of complexity of change feed then you can use change feed processor library. This library hides lot of complexity, but still gives you complete control on change feed. This library follows an [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern), your processing function is called by the SDK. 

   If you have a high throughput change feed, you can instantiate multiple clients to read the change feed. Because you are using “change feed processor library”, it will automatically divide the load among different clients. You do not have to do anything. All the complexity is handled by SDK. However, if you want to have your own load balancer, then you can implement IParitionLoadBalancingStrategy for custom partition strategy. Implement IPartitionProcessor – for custom processing changes on a partition. However, with SDK, you can process a partition range but if you want to process a particular partition key then you have to use SDK for SQL API.

* **[Using Azure Functions](#azure-functions)** 
   
   The last option Azure Function is the simplest option. We recommend using this option. When you create an Azure Cosmos DB trigger in an Azure Functions app, you select the Azure Cosmos DB collection to connect to and the function is triggered whenever a change to the collection is made. watch a [screen cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) of using Azure function and change feed

   Triggers can be created in the Azure Functions portal, in the Azure Cosmos DB portal, or programmatically. Visual Studio and VS Code has great support to write Azure Function. You can write and debug the code on your desktop, and then deploy the function with one click. For more information, see [Azure Cosmos DB: Serverless database computing using Azure Functions](serverless-computing-database.md) article.

### What is the sort order of documents in change feed?

Change feed documents comes in order of their modification time. This sort order is guaranteed only per partition.

### For a multi-region account, what happens to the change feed when the write-region fails-over? Does the change feed also failover? Would the change feed still appear contiguous or would the fail-over cause change feed to reset?

Yes, change feed will work across the manual failover operation and it will be contiguous.

### How long change feed persist the changed data if I set the TTL (Time to Live) property for the document to -1?

Change feed will persist forever. If data is not deleted, it will remain in change feed.

### How can I configure Azure functions to read from a particular region, as change feed is available in all the read regions by default?

Currently it’s not possible to configure Azure Functions to read from a particular region. There is a GitHub issue in the Azure Functions repo to set the preferred regions of any Azure Cosmos DB binding and trigger.

Azure Functions uses the default connection policy. You can configure connection mode in Azure Functions and by default, it reads from the write region, so it is best to co-locate Azure Functions on the same region.

### What is the default size of batches in Azure Functions?

100 documents at every invocation of Azure Functions. However, this number is configurable within the function.json file. Here is complete [list of configuration options](../azure-functions/functions-run-local.md). If you are developing locally, update the application settings within the [local.settings.json](../azure-functions/functions-run-local.md) file.

### I am monitoring a collection and reading its change feed, however I see I am not getting all the inserted document, some documents are missing. What is going on here?

Please make sure that there is no other function reading the same collection with the same lease collection. It happened to me, and later I realized the missing documents are processed by my other Azure functions, which is also using the same lease.

Therefore, if you are creating multiple Azure Functions to read the same change feed then they must use different lease collection or use the “leasePrefix” configuration to share the same collection. However, when you use change feed processor library you can start multiple instances of your function and SDK will divide the documents between different instances automatically for you.

### My document is updated every second, and I am not getting all the changes in Azure Functions listening to change feed.

Azure Functions polls change feed for every 5 seconds, so any changes made between 5 seconds are lost. Azure Cosmos DB stores just one version for every 5 seconds so you will get the 5th change on the document. However, if you want to go below 5 second, and want to poll change Feed every second, You can configure the polling time “feedPollTime”, see [Azure Cosmos DB bindings](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). It is defined in milliseconds with a default of 5000. Below 1 second is possible but not advisable, as you will start burning more CPU.

### I inserted a document in the Mongo API collection, but when I get the document in change feed, it shows a different id value. What is wrong here?

Your collection is Mongo API collection. Remember, change feed is read using the SQL client and serializes items into JSON format. Because of the JSON formatting, MongoDB clients will experience a mismatch between BSON formatted documents and the JSON formatted change feed. You are seeing is the representation of a BSON document in JSON. If you use binary attributes in a Mongo accounts, they are converted to JSON.

### Is there a way to control change feed for updates only and not inserts?

Not today, but this functionality is on roadmap. Today, you can add a soft marker on the document for updates.

### Is there a way to get deletes in change feed?

Currently change feed doesn’t log deletes. Change feed is continuously improving, and this functionality is on roadmap. Today, you can add a soft marker on the document for delete. Add an attribute on the document called “deleted” and set it to “true” and set a TTL on the document so that it can be automatically deleted.

### Can I read change feed for historic documents(for example, documents that were added 5 years back) ?

Yes, if the document is not deleted you can read the change feed as far as the origin of your collection.

### Can I read change feed using JavaScript?

Yes, Node.js SDK initial support for change feed is recently added. It can be used as shown in the following example, please update documentdb module to current version before you run the code:

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
