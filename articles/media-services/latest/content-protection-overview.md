---
title: Proteger su contenido mediante cifrado dinámico de Media Services - Azure | Microsoft Docs
description: En este artículo se proporciona información general de protección de contenido con cifrado dinámico. También se habla sobre el streaming de protocolos y tipos de cifrado.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 3ce24100a0780f313a00b80129601f4e8f344bde
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189774"
---
# <a name="content-protection-with-dynamic-encryption"></a>Protección de contenido con cifrado dinámico

Puede usar Azure Media Services para proteger su contenido multimedia desde el momento en que este deja el equipo y pasa a través del almacenamiento, el procesamiento y la entrega. Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con el Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados. 

En la siguiente imagen se ilustra el flujo de trabajo de protección de contenido de Media Services: 

![Proteger contenido](./media/content-protection/content-protection.svg)

*El cifrado dinámico es compatible con la "clave sin cifrado" de AES-128, CBCS y CENC. Para obtener información detallada, consulte la matriz de compatibilidad [aquí](#streaming-protocols-and-encryption-types).*

En este artículo se explican los conceptos y terminología pertinentes para conocer la protección de contenido con Media Services.

## <a name="main-components-of-a-content-protection-system"></a>Componentes principales de un sistema de protección de contenido

Para completar correctamente el diseño del sistema o las aplicaciones de "protección de contenido", debe entender completamente el ámbito del esfuerzo. En la siguiente lista se ofrece información general sobre las tres partes que necesitaría implementar. 

1. Código de Azure Media Services
  
   El [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) ejemplo muestra cómo implementar el sistema de varios DRM con Media Services v3 y también usar el servicio de entrega de clave/licencia de Media Services. Puede cifrar cada recurso con varios tipos de cifrado (AES-128, PlayReady, Widevine, FairPlay). Consulte [Streaming protocols and encryption types](#streaming-protocols-and-encryption-types) (Protocolos de streaming y tipos de cifrado) para ver lo que conviene combinar.
  
   En el ejemplo se muestra cómo:

   1. Crear y configurar [las directivas de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

      * Defina una autorización de entrega de licencias que especifique la lógica de comprobación de autorizaciones en función de las notificaciones de JWT.
      * Configurar el cifrado de DRM mediante la especificación de la clave de contenido.
      * Configurar [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md), o [FairPlay](fairplay-license-overview.md) licencias. Las plantillas le permiten configurar los derechos y permisos para cada uno de los DRM usados.

        ```
        ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
        ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
        ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
        ```
   2. Crear un [localizador de Streaming](https://docs.microsoft.com/rest/api/media/streaminglocators) que está configurado para transmitir el recurso cifrado. 
  
      El **localizador de Streaming** tiene que estar asociado con un [directiva Streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies). En el ejemplo, establecemos StreamingLocator.StreamingPolicyName a la directiva "Predefined_MultiDrmCencStreaming". Esta directiva indica que desea para dos claves de contenido (envoltura y CENC) para obtener generado y establecer en el localizador. De esta forma, se aplican los cifrados de sobre, PlayReady y Widevine (la clave se entrega al cliente de reproducción en función de las licencias DRM configuradas). Si también quiere cifrar su transmisión con CBCS (FairPlay), utilice "Predefined_MultiDrmStreaming".
    
      Puesto que deseamos cifrar el vídeo, el **directiva de clave de contenido** también se configuró anteriormente tiene que estar asociado con el **localizador de Streaming**. 
    
   3. Crear un token de prueba.

      El método **GetTokenAsync** muestra cómo crear una prueba de token.
   4. Crear las direcciones URL de streaming.

      El método **GetDASHStreamingUrlAsync** muestra cómo crear la dirección URL de streaming. En este caso, la dirección URL transmite el contenido de **DASH**.

2. Un reproductor con cliente de AES o DRM Un reproductor de vídeo basado en un SDK de reproductor (nativo o basado en un explorador) necesita reunir los siguientes requisitos:
   * El SDK del reproductor admite los clientes de DRM necesarios
   * El SDK del reproductor admite los protocolos necesarios de streaming: Smooth, DASH o HLS
   * El SDK del reproductor debe poder controlar el paso de un token JWT en la solicitud de adquisición de licencia
  
     Puede crear un reproductor mediante la [API de Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/). Utilice la [API ProtectionInfo de Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/) para especificar la tecnología DRM que se usará en distintas plataformas DRM.

     Para probar el contenido cifrado de AES o CENC (Widevine o PlayReady), puede usar [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). No olvide hacer clic en "Opciones avanzadas" y comprobar las opciones de cifrado.

     Si desea probar el contenido cifrado de FairPlay, utilice [este reproductor de prueba](https://aka.ms/amtest). El reproductor admite los DRM de Widevine, PlayReady y FairPlay, así como el cifrado de claves sin cifrado AES-128. 
    
     Debe elegir el explorador adecuado para probar diferentes DRM: Chrome/Opera/Firefox para Widevine, Microsoft Edge/IE11 para PlayReady, Safari en macOS para FairPlay.

3. Servicio de token de seguridad (STS), que emite un token JSON Web Token (JWT) como token de acceso para acceder a un recurso de back-end. Puede usar los servicios de entrega de licencias de AMS como recurso de back-end. Un STS debe definir lo siguiente:

   * Emisor y audiencia (o ámbito)
   * Notificaciones, que dependen de los requisitos empresariales de protección de contenido
   * Comprobación simétrica o asimétrica para la comprobación de firma
   * Compatibilidad con la sustitución de claves (si es necesario)

     Puede usar [esta herramienta STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) para probar el servicio de token de seguridad, que es compatible con los 3 tipos de clave de verificación: simétrico, asimétrico o Azure AD con sustitución de claves. 

> [!NOTE]
> Es muy recomendable probar completamente cada elemento (como se ha descrito anteriormente) antes de pasar al siguiente. Para probar el sistema de "protección de contenido", use las herramientas especificadas en la lista anterior.  

## <a name="streaming-protocols-and-encryption-types"></a>Protocolos de streaming y tipos de cifrado

Puede usar Media Services para entregar el contenido cifrado de forma dinámica con la clave sin cifrado AES o el cifrado DRM mediante PlayReady, Widevine o FairPlay. Actualmente puede cifrar los formatos de streaming HTTP Live Streaming (HLS), MPEG DASH y Smooth Streaming. Cada protocolo admite los siguientes métodos de cifrado:

### <a name="hls"></a>HLS

El protocolo HLS admite los siguientes formatos de contenedor y esquemas de cifrado.

|Formato de contenedor|Esquema de cifrado|Ejemplo de dirección URL|
|---|---|---|
|Todo|AES|`https://amsv3account-usw22.streaming.media.azure.net/<id>/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|MPG2-TS |CBCS (FairPlay) ||
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/<id>/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|MPG2-TS |CENC (PlayReady) ||
|CMAF(fmp4) |CENC (PlayReady) ||

CMAF/HLS más FairPlay (incluidos HEVC / H.265) es compatible con los siguientes dispositivos:

* v11 de iOS o superior 
* iPhone 8 o superior
* MacOS high Sierra con Intel tendrá lugar el 7 de uso de CPU

### <a name="mpeg-dash"></a>MPEG-DASH

El protocolo de MPEG-DASH admite los siguientes formatos de contenedor y esquemas de cifrado.

|Formato de contenedor|Esquema de cifrado|Ejemplos de direcciones URL
|---|---|---|
|Todo|AES|`https://amsv3account-usw22.streaming.media.azure.net/<id>/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/<id>/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)||

### <a name="smooth-streaming"></a>Smooth Streaming

El protocolo de transmisión por secuencias suave admite los siguientes formatos de contenedor y esquemas de cifrado.

|Protocolo|Formato de contenedor|Esquema de cifrado|
|---|---|---|
|fMP4|AES||
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/<id>/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Exploradores

Common exploradores admiten a los siguientes clientes DRM:

|Browser|Cifrado|
|---|---|
|Chrome|Widevine|
|Edge, IE 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="aes-128-clear-key-vs-drm"></a>Clave sin cifrado AES-128 frente a DRM

Los clientes suelen preguntarse si deben usar el cifrado de AES o un sistema de DRM. La diferencia principal entre los dos sistemas es que con cifrado AES de la clave de contenido se transmite al cliente a través de TLS para que la clave se cifra en tránsito, pero sin cifrado adicional ("en claro"). Como resultado, la clave utilizada para descifrar el contenido sea accesible para el Reproductor del cliente y se puede ver en un seguimiento de red en el cliente en texto sin formato. La clave sin cifrado AES-128 es adecuada para los casos de uso en los que el destinatario es una entidad de confianza (p. ej., cifrado de vídeos corporativos distribuidos dentro de una empresa para su visualización por parte de los empleados).

Los sistemas DRM como PlayReady, Widevine y FairPlay proporcionan un nivel adicional de cifrado en la clave utilizada para descifrar el contenido en comparación con la clave sin cifrado AES-128. Se cifra la clave de contenido en una clave protegida por el tiempo de ejecución DRM en adicional a cualquier cifrado de nivel de transporte proporcionado mediante TLS. Además, el descifrado se controla en un entorno seguro en el nivel de sistema operativo donde a un usuario malintencionado le resulta más difícil atacar. DRM se recomienda para los casos de uso en los que es posible que el destinatario no sea una entidad de confianza y usted requiere el nivel de seguridad más alto.

## <a name="dynamic-encryption-and-key-delivery-service"></a>Cifrado dinámico y el servicio de entrega de claves

En Media Services v3, una clave de contenido está asociada con el localizador de Streaming (vea [en este ejemplo](protect-with-aes128.md)). Si usa el servicio de entrega de claves de Media Services, puede dejar que Azure Media Services genere automáticamente la clave de contenido. Deberá generar la clave de contenido usted mismo si usa su propio servicio de claves o si tiene que controlar un escenario de alta disponibilidad en el que necesita tener la misma clave de contenido en dos centros de datos.

Cuando un reproductor solicita una transmisión, Media Services usa la clave especificada para cifrar de forma dinámica el contenido mediante la clave sin cifrado de AES o el cifrado DRM. Para descifrar la secuencia, el reproductor solicitará la clave del servicio de entrega de claves de Media Services o la del servicio que especifique. Para determinar si el usuario tiene permiso para obtener la clave, el servicio evalúa la directiva de claves de contenido que especificó para la clave.

Media Services proporciona un servicio de entrega de claves para proporcionar licencias DRM (PlayReady, Widevine, FairPlay) y claves AES a clientes autorizados. Puede usar la API REST o una biblioteca cliente de Media Services para configurar las directivas de autorización y autenticación de sus licencias y claves.

### <a name="custom-key-and-license-acquisition-url"></a>URL de adquisición de claves y licencias personalizado

Utilice las siguientes plantillas si desea especificar un servicio de entrega diferentes de claves y licencias (no de Media Services). Los dos campos reemplazables en las plantillas sirven para que puedan compartir la directiva de transmisión por secuencias a través de muchos recursos en lugar de crear una directiva de Streaming por activo. 

* EnvelopeEncryption.CustomKeyAcquisitionUrlTemplate - plantilla para la dirección URL del servicio personalizado en la entrega de claves a los jugadores del usuario final. No se requiere cuando se usa Azure Media Services para la emisión de claves. La plantilla admite tokens reemplazables que el servicio se actualizará en tiempo de ejecución con el valor específico para la solicitud.  Los valores de token admitidos actualmente son {AlternativeMediaId}, que se reemplaza con el valor de StreamingLocatorId.AlternativeMediaId y {ContentKeyId}, que se reemplaza con el valor del identificador de la clave que se solicita.
* StreamingPolicyPlayReadyConfiguration.CustomLicenseAcquisitionUrlTemplate - plantilla para la dirección URL del servicio personalizado en la entrega de licencias a los jugadores del usuario final. No se requiere cuando se usa Azure Media Services para emitir las licencias. La plantilla admite tokens reemplazables que el servicio se actualizará en tiempo de ejecución con el valor específico para la solicitud. Los valores de token admitidos actualmente son {AlternativeMediaId}, que se reemplaza con el valor de StreamingLocatorId.AlternativeMediaId y {ContentKeyId}, que se reemplaza con el valor del identificador de la clave que se solicita. 
* StreamingPolicyWidevineConfiguration.CustomLicenseAcquisitionUrlTemplate - igual al anterior, solo para Widevine. 
* StreamingPolicyFairPlayConfiguration.CustomLicenseAcquisitionUrlTemplate - igual al anterior, solo para FairPlay.  

Por ejemplo: 

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

El `ContentKeyId` tiene un valor de la clave que se solicita y `AlternativeMediaId` puede usarse si desea asignar la solicitud a una entidad por su parte. Por ejemplo, el `AlternativeMediaId` puede usarse para ayudarle a buscar los permisos.

Para ejemplos de REST que use custom clave y direcciones URL de adquisición de licencias, vea [crear directivas de Streaming:](https://docs.microsoft.com/rest/api/media/streamingpolicies/create)

## <a name="control-content-access"></a>Acceso al contenido de control

Puede controlar quién tiene acceso a su contenido configurando la directiva de claves de contenido. Media Services admite varias formas de autorizar a los usuarios que realizan solicitudes de clave. Debe configurar la directiva de claves de contenido. El cliente (reproductor) debe cumplir la directiva antes de que se pueda entregar la clave al cliente. La directiva de claves de contenido puede tener una restricción de **apertura** o de **token**. 

Con una directiva de clave de contenido con restricción de token, la clave de contenido solo se enviará a un cliente que presente JSON Web Token (JWT) o Simple Web Token (SWT) válidos en la solicitud de clave o licencia. Un servicio de token seguro (STS) debe emitir este token. Puede usar Azure Active Directory como un STS o implementar un STS personalizado. Se debe configurar el STS para crear un token firmado con las notificaciones de clave y emisión que especificó en la configuración de restricción de tokens. El servicio de entrega de claves de Media Services devolverá la clave/licencia solicitada al cliente si el token es válido y las reclamaciones del token coinciden con las configuradas para la clave/licencia.

Al configurar la directiva de restricción de token, debe especificar los parámetros de clave de comprobación principal, el emisor y el público. La clave de comprobación principal contiene la clave con la que se firmó el token. El emisor es el servicio de token seguro que emite el token. El público, a veces denominado ámbito, describe la intención del token o del recurso cuyo acceso está autorizado por el token. El servicio de entrega de claves de los Media Services valida que estos valores del token coincidan con los valores de la plantilla.

Los clientes suelen usar a un STS personalizado para incluir las notificaciones personalizadas en el símbolo (token) para seleccionar entre diferentes ContentKeyPolicyOptions con distintos parámetros de licencias DRM (una licencia de suscripción frente a una licencia de alquiler) o para incluir una notificación que representa la clave de contenido identificador de la clave que el token concede acceso.
 
## <a name="storage-side-encryption"></a>Cifrado del lado de almacenamiento

Para proteger los recursos en reposo, estos se deben cifrar mediante el cifrado del lado de almacenamiento. En la tabla siguiente se muestra cómo funciona el cifrado del lado de almacenamiento en Media Services v3:

|Opción de cifrado|DESCRIPCIÓN|Media Services v3|
|---|---|---|
|Cifrado de almacenamiento en Media Services| Cifrado de AES-256, clave administrada por Media Services|Incompatible<sup>(1)</sup>|
|[Storage Service Encryption para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Cifrado en el lado del servidor que ofrece Azure Storage, clave administrada por Azure o por el cliente|Compatible|
|[Cifrado en el lado de cliente de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Cifrado en el lado de cliente que ofrece Azure Storage, clave administrada por el cliente en Key Vault|No compatible|

<sup>1</sup> En Media Services v3, el cifrado de almacenamiento (cifrado con AES-256) solo es compatible con versiones anteriores si los recursos se crearon con Media Services v2. Esto significa que la versión v3 funciona con los recursos cifrados de almacenamiento ya existentes pero no permitirá la creación de otros nuevos.

## <a name="next-steps"></a>Pasos siguientes

* [Protección con el cifrado AES](protect-with-aes128.md)
* [Protección con DRM](protect-with-drm.md)
* [Diseñar el sistema de protección de contenido con DRM múltiple con control de acceso](design-multi-drm-system-with-access-control.md)
* [Preguntas más frecuentes](frequently-asked-questions.md)

