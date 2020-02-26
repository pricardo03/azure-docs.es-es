---
title: Puntos de conexión de streaming (origen)
titleSuffix: Azure Media Services
description: Obtenga información sobre los puntos de conexión de streaming (origen), un servicio dinámico de empaquetado y streaming que entrega contenido directamente a una aplicación de reproducción cliente o a una instancia de Content Delivery Network (CDN).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2020
ms.author: juliako
ms.openlocfilehash: c1e9be605a6f01695f2472ae76a9e5a786388aa0
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77206113"
---
# <a name="streaming-endpoints-origin-in-azure-media-services"></a>Puntos de conexión de streaming (origen) en Azure Media Services

En Microsoft Azure Media Services, un [punto de conexión de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints) representa un servicio dinámico (Just-In-Time) de empaquetado y origen que puede entregar directamente el contenido en directo y a petición a una aplicación de reproducción cliente, mediante uno de los protocolos de streaming multimedia habituales (HLS o DASH). Además, el **punto de conexión de streaming** proporciona cifrado dinámico (Just-In-Time) para los sistemas de DRM líderes del sector.

Cuando se crea una cuenta de Media Services, se genera automáticamente un punto de conexión de streaming **predeterminado** en estado detenido. No se puede eliminar el punto de conexión de streaming **predeterminado**. Es posible crear puntos de conexión de streaming adicionales en la cuenta (consulte [Cuotas y limitaciones](limits-quotas-constraints.md)).

> [!NOTE]
> Para iniciar la transmisión de vídeos, es preciso que inicie el **punto de conexión de streaming** desde el que desee transmitir en secuencias el vídeo.
>
> Solo se le cobrará cuando el punto de conexión de streaming esté en estado de ejecución.

## <a name="naming-convention"></a>Convención de nomenclatura

El formato de nombre de host de la dirección URL de streaming es: `{servicename}-{accountname}-{regionname}.streaming.media.azure.net`, donde `servicename` = el nombre del punto de conexión de streaming o el nombre del evento en directo.

Al usar el punto de conexión de streaming predeterminado, se omite `servicename`, por lo que la dirección URL es: `{accountname}-{regionname}.streaming.azure.net`.

### <a name="limitations"></a>Limitaciones

* El nombre del punto de conexión de streaming tiene un valor máximo de 24 caracteres.
* El nombre debe seguir este patrón [de expresión regular](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference): `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$`.

## <a name="types"></a>Tipos

Existen dos tipos de **puntos de conexión de streaming**: **Estándar** (versión preliminar) y **Premium**. El tipo se define por el número de unidades de escalado (`scaleUnits`) que se asignan para el punto de conexión de streaming.

En la tabla se describen los tipos:

|Tipo|Unidades de escalado|Descripción|
|--------|--------|--------|  
|**Estándar**|0|El punto de conexión de streaming predeterminado es de tipo **Estándar** y se puede cambiar al tipo Premium mediante el ajuste de `scaleUnits`.|
|**Premium**|>0|Los puntos de conexión de streaming **Premium** son adecuados para cargas de trabajo avanzadas y proporcionan una capacidad de ancho de banda dedicada y escalable. Para pasar a un tipo **Premium** se ajusta la propiedad `scaleUnits` (unidades de streaming). La propiedad `scaleUnits` proporciona capacidad de salida dedicada que puede adquirirse en incrementos de 200 Mbps. Cuando se usa el tipo **Premium**, cada unidad habilitada proporciona capacidad de ancho de banda adicional a la aplicación. |

> [!NOTE]
> Para los clientes que quieren entregar contenido a grandes audiencias de Internet, se recomienda habilitar CDN en el punto de conexión de streaming.

Para obtener información del contrato de nivel de servicio, consulte [Precios y SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="comparing-streaming-types"></a>Comparación de tipos de streaming

Característica|Estándar|Premium
---|---|---
Throughput |Hasta 600 Mbps y puede proporcionar un rendimiento eficaz mucho mayor cuando se usa una red CDN.|200 Mbps por unidad de streaming. Puede proporcionar un rendimiento eficaz mucho mayor cuando se usa una red CDN.
CDN|Azure CDN, CDN de terceros o ninguna CDN.|Azure CDN, CDN de terceros o ninguna CDN.
La facturación se prorratea| Diario|Diario
Cifrado dinámico|Sí|Sí
Empaquetado dinámico|Sí|Sí
Escala|Se amplía automáticamente hasta el rendimiento objetivo.|Unidades de streaming adicionales
Filtrado de direcciones IP/G20/host personalizado <sup>1</sup>|Sí|Sí
Descarga progresiva|Sí|Sí
Uso recomendado |Se recomienda para la gran mayoría de escenarios de streaming.|Uso profesional.

<sup>1</sup> Solo se usa directamente en el punto de conexión de streaming cuando la red CDN no está habilitada en el punto de conexión.<br/>

## <a name="properties"></a>Propiedades

En esta sección se proporcionan detalles sobre algunas de las propiedades del punto de conexión de streaming. Para obtener ejemplos de cómo crear un nuevo punto de conexión de streaming y las descripciones de todas las propiedades, vea [Streaming Endpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints/create) (Punto de conexión de streaming).

- `accessControl`: Se usa para configurar las siguientes opciones de seguridad para este punto de conexión de streaming: claves de autenticación de encabezado de firma de Akamai y direcciones IP que puedan conectarse a este punto de conexión. Esta propiedad solo se puede establecer cuando la propiedad `cdnEnabled` se establece en false.

- `cdnEnabled`: indica si la integración de la red Azure CDN para este punto de conexión de streaming está habilitada (está deshabilitada de forma predeterminada). Si establece la propiedad `cdnEnabled` en true, se deshabilitarán las siguientes configuraciones: `customHostNames` y `accessControl`.

    No todos los centros de datos admiten la integración de Azure CDN. Para comprobar si su centro de datos tiene disponible la integración de la red Azure CDN, realice los pasos siguientes:

  - Intente establecer el valor de `cdnEnabled` en true.
  - Compruebe el resultado devuelto de `HTTP Error Code 412` (error de condición previa) con el mensaje de que la propiedad CdnEnabled del punto de conexión de streaming no se puede establecer en true ya que la funcionalidad de la red CDN no está disponible en la región actual.

    Si recibe este error, el centro de datos no la admite. Pruebe otro centro de datos.

- `cdnProfile`: Cuando la propiedad `cdnEnabled` está establecida en true, también puede pasar valores `cdnProfile`. El valor `cdnProfile` es el nombre del perfil CDN donde se creará el punto de conexión CDN. Puede proporcionar un cdnProfile existente o usar uno nuevo. Si el valor es NULL y `cdnEnabled` es true, se utiliza el valor predeterminado "AzureMediaStreamingPlatformCdnProfile". Si el valor `cdnProfile` proporcionado ya existe, se crea un punto de conexión debajo de él. Si el perfil no existe, se crea un nuevo perfil automáticamente.
- `cdnProvider`: Cuando la red CDN está habilitada, también se pueden pasar valores `cdnProvider`. El valor `cdnProvider` controla qué proveedor se utilizará. Actualmente, se admiten tres valores: "StandardVerizon", "PremiumVerizon" y "StandardAkamai". Si no se proporciona ningún valor y la propiedad `cdnEnabled` es true, se utiliza "StandardVerizon" (es decir, el valor predeterminado).
- `crossSiteAccessPolicies`: Se usa para especificar las directivas de acceso entre sitios para varios clientes. Para obtener más información, consulte [Cross-domain policy file specification](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) (Especificación del archivo de directivas entre dominios) y [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx) (Hacer que un servicio esté disponible a través de los límites del dominio). La configuración se aplica solo a Smooth Streaming.
- `customHostNames`: se usa para configurar un punto de conexión de streaming para que acepte el tráfico dirigido a un nombre de host personalizado. Esta propiedad es válida para los puntos de conexión de streaming Estándar y Premium y se puede establecer cuando `cdnEnabled`: false.

    Media Services debe confirmar la propiedad del nombre de dominio. Media Services comprueba la propiedad del nombre de dominio mediante la solicitud de un registro `CName` que contenga el identificador de cuenta de Media Services como un componente para agregarlo al dominio en uso. Por ejemplo, para que "sports.contoso.com" se use como nombre de host personalizado para el punto de conexión de streaming, se debe configurar un registro para `<accountId>.contoso.com` que apunte a uno de los nombres de host de comprobación de Media Services. El nombre de host de comprobación está formado por verifydns.\<zona-dns-mediaservices>.

    Las siguientes son las zonas DNS esperadas que se van a usar en el registro de comprobación para diferentes regiones de Azure.
  
  - Norteamérica, Europa, Singapur, RAE de Hong Kong, Japón:

    - `media.azure.net`
    - `verifydns.media.azure.net`

  - China:

    - `mediaservices.chinacloudapi.cn`
    - `verifydns.mediaservices.chinacloudapi.cn`

    Por ejemplo, un registro `CName` que asigna "945a4c4e-28ea-45cd-8ccb-a519f6b700ad.contoso.com" a "verifydns.media.azure.net" prueba que el identificador de Media Services 945a4c4e-28ea-45cd-8ccb-a519f6b700ad tiene la propiedad del dominio contoso.com, lo que permite usar cualquier nombre de contoso.com como nombre de host personalizado para un punto de conexión de streaming en esa cuenta. Para buscar el valor de id. de Media Services, vaya a [Azure Portal](https://portal.azure.com/) y seleccione su cuenta de Media Services. El **identificador de cuenta** aparece en la parte superior derecha de la página.

    Si se intenta establecer un nombre de host personalizado sin una comprobación adecuada del registro `CName`, se producirá un error de respuesta de DNS y se almacenará en caché durante algún tiempo. Una vez que haya un registro adecuado, podría tardar un tiempo hasta que se vuelva a validar la respuesta almacenada en caché. En función del proveedor de DNS para el dominio personalizado, puede tardar desde unos minutos a una hora en volver a validar el registro.

    Además del registro `CName` que asigna `<accountId>.<parent domain>` a `verifydns.<mediaservices-dns-zone>`, debe crear otro registro `CName` que asigne el nombre de host personalizado (por ejemplo, `sports.contoso.com`) al nombre de host del punto de conexión de streaming de Media Services (por ejemplo, `amstest-usea.streaming.media.azure.net`).

    > [!NOTE]
    > Los puntos de conexión de streaming ubicados en el mismo centro de datos no pueden compartir el mismo nombre de host personalizado.

    Actualmente, Media Services no admite SSL con dominios personalizados.

- `maxCacheAge`: invalida el encabezado max-age de control de caché HTTP predeterminado establecido por el punto de conexión de streaming en fragmentos multimedia y manifiestos a petición. El valor se establece en segundos.
- `resourceState` -

    - Detenido: el estado inicial de un punto de conexión de streaming después de su creación.
    - Iniciando: está realizando la transición al estado En ejecución.
    - En ejecución: capaz de hacer streaming del contenido a clientes.
    - Escalando: las unidades de escalado se están aumentando o reduciendo.
    - Deteniendo: está realizando la transición al estado Detenido.
    - Eliminando: se está eliminando.

- `scaleUnits`: proporciona capacidad de salida dedicada que se puede adquirir en incrementos de 200 Mbps. Si tiene que pasar a un tipo **premium**, ajuste la propiedad `scaleUnits`.

## <a name="working-with-cdn"></a>Trabajo con la red CDN

En la mayoría de los casos, tendrá la red CDN habilitada. Sin embargo, si prevé una simultaneidad máxima inferior a 500 visores, se recomienda deshabilitar la red CDN, ya que esta se escala mejor con simultaneidad.

### <a name="considerations"></a>Consideraciones

* El punto de conexión de streaming `hostname` y la dirección URL de streaming permanecen igual habilite o no la red CDN.
* Si necesita poder probar el contenido con o sin red CDN, puede crear otro punto de conexión de streaming que no tenga habilitada la red CDN.

### <a name="detailed-explanation-of-how-caching-works"></a>Explicación detallada de cómo funciona el almacenamiento en caché

No hay ningún valor de ancho de banda específico al agregar la red CDN porque la cantidad de ancho de banda necesaria para un punto de conexión de streaming con la red CDN habilitada varía. Un lote depende del tipo de contenido, su popularidad, la velocidad de bits y los protocolos. La red CDN solo almacena en caché lo que se solicita. Esto significa que se servirá contenido popular directamente desde la red CDN: siempre que el fragmento de vídeo esté almacenado en caché. Es probable que el contenido en vivo se almacene en caché porque suele haber muchas personas mirando exactamente lo mismo. El contenido a petición puede ser un poco más complejo porque podría haber parte del contenido que fuera popular y otra que no. Si hay millones de recursos de vídeo que no son populares (solo uno o dos visores a la semana), pero hay miles de personas que miran vídeos diferentes, la red CDN pasa a ser mucho menos efectiva. Con estos errores de caché, se aumenta la carga en el punto de conexión de streaming.

También debe tener en cuenta cómo funciona el streaming adaptable. Cada fragmento de vídeo individual se almacena en caché como entidad propia. Por ejemplo, imagine la primera vez que se mira un vídeo determinado. Si el visor omite parte y mira solo unos segundos aquí y allá, solo los fragmentos de vídeo asociados con lo que miró esa persona se almacenan en caché de la red CDN. Con el streaming adaptable, se suelen tener de 5 a 7 velocidades de bits de vídeo distintas. Si una persona está mirando a una velocidad de bits y otra persona a una velocidad de bits diferente, se almacenan en caché cada uno por separado en la red CDN. Incluso si dos personas están mirando a la misma velocidad de bits podrían hacer streaming a través de protocolos diferentes. Cada protocolo (HLS, MPEG-DASH, Smooth Streaming) se almacena en caché por separado. Por lo tanto, cada velocidad de bits y protocolo se almacenan en caché por separado y solo se almacenan en caché los fragmentos de vídeo que se han solicitado.

### <a name="enable-azure-cdn-integration"></a>Habilitación de la integración de Azure CDN

> [!IMPORTANT]
> No puede habilitar CDN con cuentas de prueba o cuentas educativas de Azure.
>
> La integración de la red CDN está habilitada en todos los centros de datos de Azure excepto para las regiones Gobierno Federal y China.

Después de aprovisionar un punto de conexión de streaming con la red CDN habilitada, hay un tiempo de espera definido en Media Services antes de realizar la actualización de DNS para asignar el punto de conexión de streaming al punto de conexión de la red CDN.

Si más adelante desea volver a habilitar o deshabilitar la red CDN, punto de conexión de streaming debe estar en estado **stopped** (detenido). Es posible que transcurran hasta dos horas hasta que la integración de Azure CDN se habilite y los cambios se activen en todos los POP de la red CDN. Sin embargo, puede iniciar el punto de conexión de streaming y transmitir sin interrupciones desde ahí y, una vez que la integración esté completa, la transmisión se efectúa desde la red CDN. Durante el período de aprovisionamiento, el punto de conexión de streaming estará en estado **iniciando** y es posible que note una reducción en el rendimiento.

Cuando se crea el punto de conexión de streaming Estándar, se configura de forma predeterminada con Standard Verizon. Puede configurar los proveedores Premium Verizon o Standard Akamai mediante las API REST.

La integración de Azure Media Services con Azure CDN se implementa en **Azure CDN de Verizon** para puntos de conexión de streaming estándar. Los puntos de conexión de streaming premium pueden configurarse con todos los **proveedores y planes de tarifa de Azure CDN**. 

> [!NOTE]
> Para más información sobre Azure CDN, consulte [Información general de la red CDN](../../cdn/cdn-overview.md).

### <a name="determine-if-dns-change-was-made"></a>Determinar si se realizó el cambio de DNS

Puede determinar si se ha realizado el cambio de DNS en un punto de conexión de streaming (el tráfico se dirige a la red Azure CDN) mediante https://www.digwebinterface.com. Si los resultados tienen los nombres de dominio azureedge.net, el tráfico apunta ahora a la red CDN.

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="see-also"></a>Consulte también

[Información general de la red CDN](../../cdn/cdn-overview.md)

## <a name="next-steps"></a>Pasos siguientes

En el ejemplo [de este repositorio](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) se muestra cómo iniciar el punto de conexión de streaming predeterminado con .NET.
