---
title: Puntos de conexión de streaming en Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son los puntos de conexión de streaming y cómo los usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/30/2019
ms.author: juliako
ms.openlocfilehash: 8cd6a68f6593a5b746a19e42e4835deb05e112b6
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757180"
---
# <a name="streaming-endpoints"></a>Extremos de streaming

En Microsoft Azure Media Services (AMS), la entidad [Puntos de conexión de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints) representa un servicio de streaming que puede entregar contenido directamente a una aplicación de reproducción cliente o a una red Content Delivery Network (CDN) para la distribución posterior. La secuencia de salida de un servicio de **punto de conexión de streaming** puede ser un streaming en vivo o un recurso de vídeo a petición en la cuenta de Media Services. Cuando se crea una cuenta de Media Services, se genera automáticamente un punto de conexión de streaming **predeterminado** en estado detenido. No se puede eliminar el punto de conexión de streaming **predeterminado**. Es posible crear puntos de conexión de streaming adicionales en la cuenta. 

> [!NOTE]
> Para iniciar la transmisión de vídeos, es preciso que inicie el **punto de conexión de streaming** desde el que desee transmitir en secuencias el vídeo. 

## <a name="naming-convention"></a>Convención de nomenclatura

Para el punto de conexión predeterminado: `{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

Para los puntos de conexión adicionales: `{EndpointName}-{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

## <a name="types"></a>Tipos  

Existen dos tipos de **puntos de conexión de streaming**: Estándar y **Premium**. El tipo se define por el número de unidades de escalado (`scaleUnits`) que se asignan para el punto de conexión de streaming. 

En la tabla se describen los tipos:  

|Type|Unidades de escalado|DESCRIPCIÓN|
|--------|--------|--------|  
|**Punto de conexión de streaming estándar** (recomendado)|0|El valor predeterminado es el punto de conexión de Streaming un **estándar** escriba, pero puede cambiarse al tipo Premium.<br/> El tipo estándar es la opción recomendada para prácticamente todos los escenarios de streaming y tamaños de audiencia. El tipo **estándar** escala el ancho de banda de salida automáticamente. El rendimiento de este tipo de extremo de Streaming es hasta 600 Mbps. Fragmentos de vídeo almacenados en caché en la red CDN, no utilice el ancho de banda de punto de conexión de Streaming.<br/>Para los clientes con requisitos muy exigentes, Media Services ofrece puntos de conexión de streaming **prémium**, que pueden utilizarse para escalar horizontalmente la capacidad para las audiencias de Internet más grandes. Si espera un público amplio y usuarios simultáneos, póngase en contacto con nosotros en amsstreaming\@microsoft.com para obtener instrucciones sobre si necesita mover a la **Premium** tipo. |
|**Punto de conexión de streaming premium**|>0|Los puntos de conexión de streaming **Premium** son adecuados para cargas de trabajo avanzadas y proporcionan una capacidad de ancho de banda dedicada y escalable. Para pasar a un tipo **premium** se ajusta la propiedad `scaleUnits`. La propiedad `scaleUnits` proporciona capacidad de salida dedicada que puede adquirirse en incrementos de 200 Mbps. Cuando se usa el tipo **premium**, cada unidad habilitada proporciona capacidad de ancho de banda adicional a la aplicación. |
 
## <a name="comparing-streaming-types"></a>Comparación de tipos de streaming

### <a name="features"></a>Características

Característica|Estándar|Premium
---|---|---
Gratis los primeros 15 días| Sí |Sin 
Throughput |Hasta 600 Mbps cuando no se utiliza Azure CDN. Se puede ampliar con la red CDN.|200 Mbps por unidad de streaming. Se puede ampliar con la red CDN.
Contrato de nivel de servicio | 99,9|99,9 (200 Mbps por unidad de streaming).
CDN|Azure CDN, red de entrega de contenido de terceros o ninguna red de entrega de contenido.|Azure CDN, red de entrega de contenido de terceros o ninguna red de entrega de contenido.
La facturación se prorratea| Diario|Diario
Cifrado dinámico|Sí|Sí
Empaquetado dinámico|Sí|Sí
Escala|Se amplía automáticamente hasta el rendimiento objetivo.|Unidades de streaming adicionales
IP filtrado/G20/host personalizado <sup>1</sup>|Sí|Sí
Descarga progresiva|Sí|Sí
Uso recomendado |Se recomienda para la gran mayoría de escenarios de streaming.|Uso profesional.<br/>Si considera que puede tener necesidades más allá del nivel Estándar. Si espera un tamaño de audiencia simultánea superior a 50 000 espectadores, póngase en contacto con nosotros (amsstreaming@microsoft.com).

<sup>1</sup> solo se usa directamente en el extremo de transmisión por secuencias cuando la red CDN no está habilitada en el punto de conexión.

## <a name="working-with-cdn"></a>Trabajo con la red CDN

En la mayoría de los casos, tendrá la red CDN habilitada. Sin embargo, si prevé una simultaneidad máxima inferior a 500 visores, se recomienda deshabilitar la red CDN, ya que esta escala mejor con simultaneidad.

> [!NOTE]
> El punto de conexión de streaming `hostname` y la dirección URL de streaming permanecen igual habilite o no la red CDN.

### <a name="detailed-explanation-of-how-caching-works"></a>Explicación detallada de cómo funciona el almacenamiento en caché

No hay ningún valor de ancho de banda específico al agregar la red CDN porque la cantidad de ancho de banda necesaria para un punto de conexión de streaming con red CDN habilitada varía. Un lote depende del tipo de contenido, su popularidad, la velocidad de bits y los protocolos. La red CDN solo almacena en caché lo que se solicita. Esto significa que se servirá contenido popular directamente desde la red CDN: siempre que el fragmento de vídeo esté almacenado en caché. Es probable que el contenido en vivo se almacene en caché porque suele haber muchas personas mirando exactamente lo mismo. El contenido a petición puede ser un poco más complejo porque podría haber parte del contenido que fuera popular y otra que no. Si hay millones de recursos de vídeo que no son populares (solo 1 o 2 visores a la semana), pero hay miles de personas que miran vídeos diferentes, la red CDN pasa a ser mucho menos efectiva. Con estos errores de caché, se aumenta la carga en el punto de conexión de streaming.
 
También debe tener en cuenta cómo funciona el streaming adaptable. Cada fragmento de vídeo individual se almacena en caché como entidad propia. Por ejemplo, si la primera vez que una persona mira un determinado vídeo, omite parte y mira solo unos segundos aquí y allá, solo los fragmentos de vídeo asociados con lo que miró esa persona se almacenan en caché de la red CDN. Con el streaming adaptable, se suelen tener de 5 a 7 velocidades de bits de vídeo distintas. Si una persona está mirando a una velocidad de bits y otra persona a una velocidad de bits diferente, se almacenan en caché cada uno por separado en la red CDN. Incluso si dos personas están mirando a la misma velocidad de bits podrían transmitir en streaming a través de protocolos diferentes. Cada protocolo (HLS, MPEG-DASH, Smooth Streaming) se almacena en caché por separado. Por lo tanto, cada velocidad de bits y protocolo se almacenan en caché por separado y solo se almacenan en caché los fragmentos de vídeo que se han solicitado.
 
## <a name="properties"></a>Properties (Propiedades) 

En esta sección se proporciona detalles sobre algunas de las propiedades del extremo de Streaming. Para obtener ejemplos de cómo crear un nuevo punto de conexión de streaming y las descripciones de todas las propiedades, vea [Streaming Endpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints/create) (Punto de conexión de streaming). 

- `accessControl` -Se usa para configurar las siguientes opciones de seguridad para este extremo de streaming: Claves de autenticación de encabezado de firma de Akamai y direcciones IP que puedan conectarse a este punto de conexión.<br />Se puede establecer esta propiedad cuando `cdnEnabled` está establecida en false.
- `cdnEnabled` : Indica si la integración de Azure CDN para este extremo de streaming está habilitada (deshabilitado de forma predeterminada). Si establece la propiedad `cdnEnabled` en true, se deshabilitarán las siguientes configuraciones: `customHostNames` y `accessControl`.
  
    No todos los centros de datos admiten la integración de Azure CDN. Para comprobar si su centro de datos tiene la integración de CDN de Azure disponible, haga lo siguiente:
 
  - Intenta establecer el `cdnEnabled` en true.
  - Compruebe el resultado devuelto para un `HTTP Error Code 412` (PreconditionFailed) con el mensaje "Propiedad CdnEnabled de extremo de Streaming no se puede establecer como verdadero, como la capacidad de red CDN no está disponible en la región actual." 

    Si recibe este error, el centro de datos no la admite. Debe probar otro centro de datos.
- `cdnProfile` -Cuando `cdnEnabled` está establecido en true, también puede pasar `cdnProfile` valores. El valor `cdnProfile` es el nombre del perfil CDN donde se creará el punto de conexión CDN. Puede proporcionar un cdnProfile existente o usar uno nuevo. Si el valor es NULL y `cdnEnabled` es true, se utiliza el valor predeterminado "AzureMediaStreamingPlatformCdnProfile". Si el valor `cdnProfile` proporcionado ya existe, se crea un punto de conexión debajo de él. Si el perfil no existe, automáticamente se crea un nuevo perfil.
- `cdnProvider` -Cuando CDN esté habilitado, también se puede pasar `cdnProvider` valores. El valor `cdnProvider` controla qué proveedor se utilizará. Actualmente, se admiten tres valores: "StandardVerizon", "PremiumVerizon" y "StandardAkamai". Si se proporciona ningún valor y `cdnEnabled` es true, se utiliza "StandardVerizon" (es decir, el valor predeterminado).
- `crossSiteAccessPolicies` : Se usa para especificar las directivas de acceso entre sitios para varios clientes. Para obtener más información, consulte [Cross-domain policy file specification](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) (Especificación del archivo de directivas entre dominios) y [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx) (Hacer que un servicio esté disponible a través de los límites del dominio).<br/>La configuración se aplica solo a Smooth Streaming.
- `customHostNames` : Se usa para configurar un extremo de Streaming para aceptar tráfico dirigido a un nombre de host personalizado.  Esta propiedad es válida para Standard y Premium de extremos de Streaming y se puede establecer cuando `cdnEnabled`: false.
    
    La propiedad del nombre de dominio debe ser confirmada por Media Services. Media Services comprueba la propiedad de nombre de dominio solicitando un `CName` registro que contiene el identificador de cuenta de Media Services como un componente para agregarlo al dominio en uso. Por ejemplo, para que "sports.contoso.com" se use como nombre de host personalizado para el punto de conexión de streaming, se debe configurar un registro para `<accountId>.contoso.com` que apunte a uno de los nombres de host de comprobación de Media Services. El nombre de host de comprobación está formado por verifydns.\<zona-dns-mediaservices>. 

    Las siguientes son las zonas DNS esperadas que se usará en el registro de comprobación para diferentes regiones de Azure.
  
  - Norteamérica, Europa, Singapur, Hong Kong, Japón:
      
    - `media.azure.net`
    - `verifydns.media.azure.net`
      
  - China:
        
    - `mediaservices.chinacloudapi.cn`
    - `verifydns.mediaservices.chinacloudapi.cn`
        
    Por ejemplo, un `CName` registro que se asigna "945a4c4e-28ea-45cd-8ccb-a519f6b700ad.contoso.com" a "verifydns.media.azure.net" demuestra que el 945a4c4e-28ea-45cd-8ccb-a519f6b700ad de identificador de Media Services tiene la propiedad del dominio contoso.com, por lo tanto Habilitación de cualquier nombre de contoso.com para usarse como un nombre de host personalizado para un extremo de streaming en esa cuenta. Para buscar el valor de id. de Media Services, vaya a [Azure Portal](https://portal.azure.com/) y seleccione su cuenta de Media Services. El **Id. de cuenta** aparece en la parte superior derecha de la página.
        
    Si se intenta establecer un nombre de host personalizado sin una comprobación adecuada del registro `CName`, se producirá un error de respuesta de DNS y se almacenará en caché durante algún tiempo. Una vez que haya un registro adecuado, podría tardar un tiempo hasta que se vuelva a validar la respuesta almacenada en caché. En función del proveedor de DNS para el dominio personalizado, podría tardar desde unos minutos a una hora en volver a validar el registro.
        
    Además el `CName` que asigna `<accountId>.<parent domain>` a `verifydns.<mediaservices-dns-zone>`, debe crear otro `CName` que asigna el nombre de host personalizado (por ejemplo, `sports.contoso.com`) al nombre de host de su Media Services de transmisión por secuencias del punto de conexión (por ejemplo, `amstest-usea.streaming.media.azure.net`).
 
    > [!NOTE]
    > Los puntos de conexión que se encuentra en el mismo centro de datos de transmisión por secuencias no pueden compartir el mismo nombre de host personalizado.

    Actualmente, Media Services no admite SSL con dominios personalizados. 
    
- `maxCacheAge` -Invalida la caché HTTP predeterminada max-age controlar encabezado establecido por el extremo de streaming de fragmentos multimedia y los manifiestos y a petición. El valor se establece en segundos.
- `resourceState` -

    - Detenido: el estado inicial de un extremo de Streaming después de la creación
    - Iniciando - está realizando la transición al estado en ejecución
    - Es capaz de transmitir contenido a los clientes de ejecución:
    - Escalado - la escala de las unidades que se aumenta o disminuye
    - Deteniendo - está realizando la transición al estado detenido
    - Eliminando – se va a eliminar
    
- `scaleUnits ` -Proporcionan capacidad de salida dedicada puede adquirirse en incrementos de 200 Mbps. Si tiene que pasar a un tipo **premium**, ajuste la propiedad `scaleUnits`.

## <a name="next-steps"></a>Pasos siguientes

En el ejemplo [de este repositorio](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) se muestra cómo iniciar el punto de conexión de streaming predeterminado con .NET.

