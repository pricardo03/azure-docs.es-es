---
title: Información general de punto de conexión de streaming de Azure Media Services | Microsoft Docs
description: En este tema se proporciona información general sobre los puntos de conexión de streaming de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: c5979fa7ff67c5acda9ab653bc4ee52d8b5129a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60544966"
---
# <a name="streaming-endpoints-overview"></a>Información general de los puntos de conexión de streaming  

## <a name="overview"></a>Información general

En Microsoft Azure Media Services (AMS), un **punto de conexión de streaming** representa un servicio de streaming que puede entregar contenido directamente a una aplicación de reproducción de cliente o a una red Content Delivery Network (CDN) para la distribución posterior. Media Services también proporciona integración sin problemas de Azure CDN. La secuencia de salida de un servicio de punto de conexión de streaming puede ser una secuencia en vivo, un vídeo bajo demanda o una descarga progresiva de su recurso en su cuenta de Media Services. Cada cuenta de Azure Media Services incluye un punto de conexión de streaming predeterminado. Es posible crear puntos de conexión de streaming adicionales en la cuenta. Existen dos versiones de puntos de conexión de streaming: 1.0 y 2.0. A partir del 10 de enero de 2017, las cuentas recién creadas de AMS incluirán de manera **predeterminada** la versión 2.0 del punto de conexión de streaming. Los puntos de conexión de streaming adicionales que agregue a esta cuenta también se generarán en la versión 2.0. Este cambio no afectará a las cuentas existentes; los puntos de conexión de streaming actuales mantendrán la versión 1.0, aunque es posible actualizarlos a la versión 2.0. Este cambio implicará modificaciones en cuanto al comportamiento, la facturación y las características (para obtener más información, consulte la sección **Tipos y versiones de streaming** a continuación).

En Azure Media Services se agregaron las siguientes propiedades a la entidad punto de conexión de streaming: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime** y **StreamingEndpointVersion**. Para obtener información general detallada de estas propiedades, consulte [aquí](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Cuando se crea una cuenta de Azure Media Services, se genera automáticamente un punto de conexión de streaming estándar predeterminado en el estado **Stopped** (Detenido). No se puede eliminar el punto de conexión de streaming predeterminado. Dependiendo de la disponibilidad de Azure CDN en la región de destino, el punto de conexión de streaming recién creado incluye también de manera predeterminada la integración con el proveedor de red CDN "StandardVerizon". 
                
> [!NOTE]
> Es posible deshabilitar la integración de Azure CDN antes de iniciar el punto de conexión de streaming. `hostname` y la dirección URL de streaming permanecen igual habilite o no la red CDN.

Este tema proporciona información general de las principales funcionalidades proporcionadas por los puntos de conexión de streaming.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Para el punto de conexión predeterminado: `{AccountName}.streaming.mediaservices.windows.net`

Para los puntos de conexión adicionales: `{EndpointName}-{AccountName}.streaming.mediaservices.windows.net`

## <a name="streaming-types-and-versions"></a>Tipos y versiones de streaming

### <a name="standardpremium-types-version-20"></a>Tipos estándar o premium (versión 2.0)

A partir de la versión de enero de 2017 de Media Services, existen dos tipos de streaming: **Estándar** y **Premium**. Estos tipos forman parte de la versión "2.0" de los puntos de conexión de streaming.

Type|DESCRIPCIÓN
---|---
**Estándar** |Se trata de la opción predeterminada, que probablemente funcione para la mayoría de los escenarios.<br/>Con esta opción conseguirá un Acuerdo de Nivel de Servicio fijo o limitado, los 15 días siguientes al inicio del punto de conexión de streaming serán gratuitos.<br/>Si crea más de un punto de conexión de streaming, solo el primero de ellos será gratuito durante los primeros 15 días; los demás se cobrarán tan pronto los inicie. <br/>Tenga en cuenta que la prueba gratuita solo se aplica a las cuentas de Media Services recién creadas y los puntos de conexión de streaming predeterminados. Los puntos de conexión de streaming ya existentes y los puntos de conexión de streaming creados adicionalmente no incluyen el período de evaluación gratuita, incluso si se actualizan a la versión 2.0 o se crean como versión 2.0.
**Premium** |Esta opción es la preferible para escenarios profesionales en los que se requiere mayor escala o control.<br/>Con un Acuerdo de Nivel de Servicio variable que se basa en la capacidad adquirida de la unidad de streaming premium, los puntos de conexión de streaming dedicados residen en un entorno aislado y no compiten por los recursos.

Para obtener más información, consulte a continuación la sección **Comparar tipos de streaming**.

### <a name="classic-type-version-10"></a>Tipo clásico (versión 1.0)

Los usuarios que disponen de cuentas de AMS anteriores a la versión del 10 de enero de 2017 tienen un punto de conexión de streaming de tipo **clásico**. Este tipo forma parte de la versión "1.0" de los puntos de conexión de streaming.

Si el punto de conexión de streaming de la **versión "1.0"** tiene al menos 1 unidad de streaming premium, será un punto de conexión de streaming premium y ofrecerá todas las características de AMS (al igual que el tipo **Estándar o Premium**) sin ningún paso de configuración adicional.

>[!NOTE]
>Los puntos de conexión de streaming **clásicos** (versión "1.0" y 0 unidades de streaming) ofrecen características limitadas y no incluyen un Acuerdo de Nivel de Servicio. Se recomienda migrar al tipo **Estándar** para disfrutar de una mejor experiencia y usar características como el empaquetado dinámico, el cifrado y otras que se ofrecen con **dicho** tipo.  Para migrar al tipo **Estándar**, vaya a [Azure Portal](https://portal.azure.com/) y seleccione **Opt-in to Standard** (Participar en Estándar). Para obtener más información acerca de la migración, consulte la sección [Migración](#migration-between-types).
>
>Tenga en cuenta que esta operación no se puede revertir y afectará al precio.
>
 
## <a name="comparing-streaming-types"></a>Comparación de tipos de streaming

### <a name="versions"></a>Versiones

|Type|Versión de punto de conexión de streaming|Unidades de escalado|CDN|Facturación|Contrato de nivel de servicio| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Clásico|1.0|0|N/D|Gratuito|N/D|
|Punto de conexión de streaming estándar|2.0|0|Sí|De pago|Sí|
|Unidades de streaming premium|1.0|>0|Sí|De pago|Sí|
|Unidades de streaming premium|2.0|>0|Sí|De pago|Sí|

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
Filtrado de IP/G20/host personalizado|Sí|Sí
Descarga progresiva|Sí|Sí
Uso recomendado |Se recomienda para la gran mayoría de escenarios de streaming.|Uso profesional.<br/>Si considera que puede tener necesidades más allá del nivel Estándar. Si espera un tamaño de audiencia simultánea superior a 50 000 espectadores, póngase en contacto con nosotros (amsstreaming@microsoft.com).


## <a name="migration-between-types"></a>Migración entre tipos

De | Para | .
---|---|---
Clásico|Estándar|Necesidad de solicitar la participación
Clásico|Premium| Escala (unidades de streaming adicionales)
Estándar/Premium|Clásico|No está disponible (si la versión de punto de conexión de streaming es 1.0. Se permite cambiar al modelo clásico con la configuración de unidades de escalado en "0")
Estándar (con o sin red CDN)|Premium con la misma configuración|Permitido en el estado **started** (iniciado). (mediante Azure Portal)
Premium (con o sin red CDN)|Estándar, con la misma configuración|Permitido en el estado **started** (iniciado) (a través de Azure Portal)
Estándar (con o sin red CDN)|Premium con diferente configuración|Permitido en el estado **stopped** (detenido) (a través de Azure Portal). No se permite en el estado en ejecución.
Premium (con o sin red CDN)|Estándar con diferente configuración|Permitido en el estado **stopped** (detenido) (a través de Azure Portal). No se permite en el estado en ejecución.
Versión 1.0 con al menos una unidad de streaming en red CDN|Estándar o Premium sin ninguna red CDN|Permitido en el estado **stopped** (detenido). No se permite en el estado **started** (iniciado).
Versión 1.0 con al menos una unidad de streaming en red CDN|Estándar con o sin red CDN|Permitido en el estado **stopped** (detenido). No se permite en el estado **started** (iniciado). La red CDN de la versión 1.0 se eliminará y se creará e iniciará una nueva.
Versión 1.0 con al menos una unidad de streaming en red CDN|Premium con o sin red CDN|Permitido en el estado **stopped** (detenido). No se permite en el estado **started** (iniciado). La red CDN clásica se eliminará y se creará e iniciará una nueva.

## <a name="next-steps"></a>Pasos siguientes
Consulte las rutas de aprendizaje de Media Services.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

