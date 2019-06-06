---
title: Extremos de streaming (origen) en servicios multimedia de Azure | Microsoft Docs
description: En Azure Media Services, un extremo de Streaming (Origin) representa un empaquetado dinámico y el servicio de streaming que puede entregar contenido directamente a una aplicación de Reproductor de cliente o a una Content Delivery Network (CDN) para su posterior distribución.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/27/2019
ms.author: juliako
ms.openlocfilehash: 3e1a5d8ba8b6d0ec8e3ec1ba9506a88ee87d8ed1
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515742"
---
# <a name="streaming-endpoints"></a>Extremos de streaming 

En Microsoft Azure Media Services, un [Streamingendpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) representa un dinámico (just-in-time) empaquetado y el origen de servicio que puede entregar el contenido en directo y a petición directamente a una aplicación de Reproductor de cliente, mediante uno de los media protocolos de streaming comunes (HLS o DASH). Además, el **Streamingendpoint** proporciona el cifrado dinámico (just-in-time) para DRM de líderes del sector.

Cuando se crea una cuenta de Media Services, se genera automáticamente un punto de conexión de streaming **predeterminado** en estado detenido. No se puede eliminar el punto de conexión de streaming **predeterminado**. Se pueden crear puntos de conexión de Streaming adicionales en la cuenta (consulte [cuotas y limitaciones](limits-quotas-constraints.md)). 

> [!NOTE]
> Para iniciar la transmisión de vídeos, es preciso que inicie el **punto de conexión de streaming** desde el que desee transmitir en secuencias el vídeo. 
>  
> Solo se le cobrará cuando StreamingEndpoint esté en estado en ejecución.

## <a name="naming-convention"></a>Convención de nomenclatura

Para el punto de conexión predeterminado: `{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

Para los puntos de conexión adicionales: `{EndpointName}-{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

## <a name="types"></a>Tipos  

Existen dos tipos de **puntos de conexión de streaming**: **Estándar** (versión preliminar) y **Premium**. El tipo se define por el número de unidades de escalado (`scaleUnits`) que se asignan para el punto de conexión de streaming. 

En la tabla se describen los tipos:  

|Escriba|Unidades de escalado|DESCRIPCIÓN|
|--------|--------|--------|  
|**Estándar**|0|El valor predeterminado es el punto de conexión de Streaming un **estándar** escriba, se puede cambiar el tipo Premium ajustando `scaleUnits`.|
|**Premium**|>0|**Premium** extremos de Streaming son adecuados para cargas de trabajo avanzadas, proporcionan una capacidad de ancho de banda dedicada y escalable. Mover a un **Premium** tipo ajustando `scaleUnits` (unidades de streaming). La propiedad `scaleUnits` proporciona capacidad de salida dedicada que puede adquirirse en incrementos de 200 Mbps. Cuando se usa el tipo **premium**, cada unidad habilitada proporciona capacidad de ancho de banda adicional a la aplicación. |

> [!NOTE]
> Los clientes que desean para entregar contenido a grandes audiencias de internet, se recomienda habilitar CDN en el punto de conexión de Streaming.

Para obtener información de SLA, consulte [precios y SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="comparing-streaming-types"></a>Comparación de tipos de streaming

Característica|Estándar|Premium
---|---|---
Gratis los primeros 15 días <sup>1</sup>| Sí |No
Throughput |Hasta 600 Mbps y puede proporcionar un rendimiento eficaz mucho mayor cuando se usa una red CDN.|200 Mbps por unidad de streaming. Puede proporcionar un rendimiento eficaz mucho mayor cuando se usa una red CDN.
CDN|Azure CDN, red de entrega de contenido de terceros o ninguna red de entrega de contenido.|Azure CDN, red de entrega de contenido de terceros o ninguna red de entrega de contenido.
La facturación se prorratea| Cada día|Cada día
Cifrado dinámico|Sí|Sí
Empaquetado dinámico|Sí|Sí
Escala|Se amplía automáticamente hasta el rendimiento objetivo.|Unidades de streaming adicionales
IP filtrado/G20/host personalizado <sup>2</sup>|Sí|Sí
Descarga progresiva|Sí|Sí
Uso recomendado |Se recomienda para la gran mayoría de escenarios de streaming.|Uso profesional.

<sup>1</sup> la evaluación gratuita solo aplica a las cuentas de servicios multimedia recién creado y el valor predeterminado el punto de conexión de Streaming.<br/>
<sup>2</sup> solo se usa directamente en el extremo de transmisión por secuencias cuando la red CDN no está habilitada en el punto de conexión.<br/>

## <a name="properties"></a>Properties (Propiedades) 

En esta sección se proporciona detalles sobre algunas de las propiedades del extremo de Streaming. Para obtener ejemplos de cómo crear un nuevo punto de conexión de streaming y las descripciones de todas las propiedades, vea [Streaming Endpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints/create) (Punto de conexión de streaming). 

- `accessControl` -Se usa para configurar las siguientes opciones de seguridad para este extremo de streaming: Claves de autenticación de encabezado de firma de Akamai y direcciones IP que puedan conectarse a este punto de conexión.<br />Esta propiedad solo puede establecerse cuando `cdnEnabled` está establecida en false.
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
  
  - América del Norte, Europa, Singapur, RAE de Hong Kong, Japón:
      
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
    
- `scaleUnits` -Proporcionan capacidad de salida dedicada puede adquirirse en incrementos de 200 Mbps. Si tiene que pasar a un tipo **premium**, ajuste la propiedad `scaleUnits`.

## <a name="working-with-cdn"></a>Trabajo con la red CDN

En la mayoría de los casos, tendrá la red CDN habilitada. Sin embargo, si prevé una simultaneidad máxima inferior a 500 visores, se recomienda deshabilitar la red CDN, ya que esta escala mejor con simultaneidad.

### <a name="considerations"></a>Consideraciones

* El punto de conexión de streaming `hostname` y la dirección URL de streaming permanecen igual habilite o no la red CDN.
* Si necesita la capacidad de probar el contenido con o sin red CDN, puede crear otro extremo de Streaming que no está habilitado de CDN.

### <a name="detailed-explanation-of-how-caching-works"></a>Explicación detallada de cómo funciona el almacenamiento en caché

No hay ningún valor de ancho de banda específico al agregar la red CDN porque la cantidad de ancho de banda necesaria para un punto de conexión de streaming con red CDN habilitada varía. Un lote depende del tipo de contenido, su popularidad, la velocidad de bits y los protocolos. La red CDN solo almacena en caché lo que se solicita. Esto significa que se servirá contenido popular directamente desde la red CDN: siempre que el fragmento de vídeo esté almacenado en caché. Es probable que el contenido en vivo se almacene en caché porque suele haber muchas personas mirando exactamente lo mismo. El contenido a petición puede ser un poco más complejo porque podría haber parte del contenido que fuera popular y otra que no. Si hay millones de recursos de vídeo que no son populares (solo 1 o 2 visores a la semana), pero hay miles de personas que miran vídeos diferentes, la red CDN pasa a ser mucho menos efectiva. Con estos errores de caché, se aumenta la carga en el punto de conexión de streaming.
 
También debe tener en cuenta cómo funciona el streaming adaptable. Cada fragmento de vídeo individual se almacena en caché como entidad propia. Por ejemplo, si la primera vez que una persona mira un determinado vídeo, omite parte y mira solo unos segundos aquí y allá, solo los fragmentos de vídeo asociados con lo que miró esa persona se almacenan en caché de la red CDN. Con el streaming adaptable, se suelen tener de 5 a 7 velocidades de bits de vídeo distintas. Si una persona está mirando a una velocidad de bits y otra persona a una velocidad de bits diferente, se almacenan en caché cada uno por separado en la red CDN. Incluso si dos personas están mirando a la misma velocidad de bits podrían transmitir en streaming a través de protocolos diferentes. Cada protocolo (HLS, MPEG-DASH, Smooth Streaming) se almacena en caché por separado. Por lo tanto, cada velocidad de bits y protocolo se almacenan en caché por separado y solo se almacenan en caché los fragmentos de vídeo que se han solicitado.

### <a name="enable-azure-cdn-integration"></a>Habilitar la integración de la red CDN de Azure

Después de un extremo de Streaming se aprovisiona con CDN haya habilitado es un tiempo de espera definido en Media Services antes de realiza la actualización de DNS para asignar el punto de conexión de Streaming al punto de conexión CDN.

Si más adelante desea volver a habilitar o deshabilitar la red CDN, punto de conexión de streaming debe estar en estado **stopped** (detenido). Es posible que transcurran hasta dos horas hasta que la integración de Azure CDN se habilite y los cambios se activen en todos los POP de la red CDN. Sin embargo, puede iniciar el punto de conexión de streaming y transmitir sin interrupciones desde ahí y, una vez que la integración esté completa, la transmisión se efectúa desde la red CDN. Durante el período de aprovisionamiento, el punto de conexión de streaming estará en estado **starting** (iniciando) y es posible que note una reducción en el rendimiento.

Cuando se crea el extremo de streaming estándar, se configura de forma predeterminada con Verizon estándar. Puede configurar proveedores de Verizon Premium o estándar de Akamai mediante las API de REST. 

La integración de la red CDN está habilitada en todos los centros de datos de Azure excepto las regiones de China y el Gobierno Federal.

> [!IMPORTANT]
> La integración de Azure Media Services con Azure CDN se implementa en **Azure CDN de Verizon** para puntos de conexión de streaming estándar. Los puntos de conexión de streaming premium pueden configurarse con todos los **proveedores y planes de tarifa de Azure CDN**. Para obtener más información sobre las características de Azure CDN, consulte la [información general de la red CDN](../../cdn/cdn-overview.md).

### <a name="determine-if-dns-change-has-been-made"></a>Determinar si se ha realizado el cambio DNS

Puede determinar si se ha realizado el cambio DNS en un extremo de Streaming (el tráfico se dirige a la red CDN de Azure) mediante https://www.digwebinterface.com. Si los resultados tiene nombres de dominio azureedge.net en los resultados, el tráfico que apuntan ahora a la red CDN.

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

En el ejemplo [de este repositorio](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) se muestra cómo iniciar el punto de conexión de streaming predeterminado con .NET.

