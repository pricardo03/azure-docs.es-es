---
title: Protección de su contenido con Azure Media Services | Microsoft Docs
description: En este artículo se ofrece información general de protección de contenido con Media Services.
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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 1c7454aead07c728d55ff2c309cca83a792aac88
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51238356"
---
# <a name="content-protection-overview"></a>Introducción a la protección de contenido

Puede usar Azure Media Services para proteger su contenido multimedia desde el momento en que este deja el equipo y pasa a través del almacenamiento, el procesamiento y la entrega. Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados. 

En la siguiente imagen se ilustra el flujo de trabajo de protección de contenido de Media Services: 

![Proteger contenido](./media/content-protection/content-protection.png)

*El cifrado dinámico es compatible con la "clave sin cifrado" de AES-128, CBCS y CENC. Para obtener información detallada, consulte la matriz de compatibilidad [aquí](#streaming-protocols-and-encryption-types).*

En este artículo se explican los conceptos y terminología pertinentes para conocer la protección de contenido con Media Services. El artículo también incluye la sección de [preguntas más frecuentes](#faq) y proporciona vínculos a artículos que muestran cómo proteger el contenido. 

## <a name="main-components-of-the-content-protection-system"></a>Componentes principales del sistema de protección de contenido

Para completar correctamente el diseño del sistema o las aplicaciones de "protección de contenido", debe entender completamente el ámbito del esfuerzo. En la siguiente lista se ofrece información general sobre las tres partes que necesitaría implementar. 

1. Código de Azure Media Services
  
  * Plantillas de licencia de PlayReady, Widevine y FairPlay. Las plantillas le permiten configurar los derechos y permisos para cada uno de los DRM usados
  * Una autorización de entrega de licencias que especifique la lógica de comprobación de autorizaciones en función de las notificaciones de JWT
  * Claves de contenido, protocolos de streaming y los DRM correspondientes aplicados que definan el cifrado de DRM.

  > [!NOTE]
  > Puede cifrar cada recurso con varios tipos de cifrado (AES-128, PlayReady, Widevine, FairPlay). Consulte [Streaming protocols and encryption types](#streaming-protocols-and-encryption-types) (Protocolos de streaming y tipos de cifrado) para ver lo que conviene combinar.
  
  En el artículo siguiente se muestran los pasos para cifrar contenido con AES o DRM: 
  
  * [Protección con el cifrado AES](protect-with-aes128.md)
  * [Protección con DRM](protect-with-drm.md)

2. Un reproductor con cliente de AES o DRM Un reproductor de vídeo basado en un SDK de reproductor (nativo o basado en un explorador) necesita reunir los siguientes requisitos:
  * El SDK del reproductor admite los clientes de DRM necesarios
  * El SDK del reproductor admite los protocolos necesarios de streaming: Smooth, DASH y HLS
  * El SDK del reproductor debe poder controlar el paso de un token JWT en la solicitud de adquisición de licencia
  
    Puede crear un reproductor mediante la [API de Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Utilice la [API ProtectionInfo de Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/) para especificar la tecnología DRM que se usará en distintas plataformas DRM.

    Para probar el contenido cifrado de AES o CENC (Widevine o PlayReady), puede usar [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). No olvide hacer clic en "Opciones avanzadas" y comprobar las opciones de cifrado.

    Si desea probar el contenido cifrado de FairPlay, utilice [este reproductor de prueba](https://aka.ms/amtest). El reproductor admite los DRM de Widevine, PlayReady y FairPlay, así como el cifrado de claves sin cifrado AES-128. Debe elegir el explorador adecuado para probar diferentes DRM: Chrome/Opera/Firefox para Widevine; MS Edge/IE11 para PlayReady; Safari en macOS para FairPlay.

3. Servicio de token de seguridad (STS), que emite un token JSON Web Token (JWT) como token de acceso para acceder a un recurso de back-end. Puede usar los servicios de entrega de licencias de AMS como recurso de back-end. Un STS debe definir lo siguiente:

  * Emisor y audiencia (o ámbito)
  * Notificaciones, que dependen de los requisitos empresariales de protección de contenido
  * Comprobación simétrica o asimétrica para la comprobación de firma
  * Compatibilidad con la sustitución de claves (si es necesario)

    Puede usar [esta herramienta STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) para probar el servicio de token de seguridad, que es compatible con los 3 tipos de clave de verificación: simétrico, asimétrico o AAD con sustitución de claves. 

> [!NOTE]
> Es muy recomendable probar completamente cada elemento (como se ha descrito anteriormente) antes de pasar al siguiente. Para probar el sistema de "protección de contenido", use las herramientas especificadas en la lista anterior.  

## <a name="streaming-protocols-and-encryption-types"></a>Protocolos de streaming y tipos de cifrado

Puede usar Media Services para entregar el contenido cifrado de forma dinámica con la clave sin cifrado AES o el cifrado DRM mediante PlayReady, Widevine o FairPlay. Actualmente puede cifrar los formatos de streaming HTTP Live Streaming (HLS), MPEG DASH y Smooth Streaming. Cada protocolo admite los siguientes métodos de cifrado:

|Protocolo|Formato de contenedor|Esquema de cifrado|
|---|---|---|---|
|MPEG-DASH|Todo|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Todo|AES|
||MPG2-TS |CBCS (Fairplay) |
||MPG2-TS |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Smooth Streaming|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Cifrado dinámico

En Media Services v3, se asocia una clave de contenido con StreamingLocator (consulte [este ejemplo](protect-with-aes128.md)). Si usa el servicio de entrega de claves de Media Services, debe generar automáticamente la clave de contenido. Deberá generar la clave de contenido usted mismo si usa su propio servicio de claves o si tiene que controlar un escenario de alta disponibilidad en el que necesita tener la misma clave de contenido en dos centros de datos.

Cuando un reproductor solicita una transmisión, Media Services usa la clave especificada para cifrar de forma dinámica el contenido mediante la clave sin cifrado de AES o el cifrado DRM. Para descifrar la secuencia, el reproductor solicitará la clave del servicio de entrega de claves de Media Services o la del servicio que especifique. Para determinar si el usuario tiene permiso para obtener la clave, el servicio evalúa la directiva de claves de contenido que especificó para la clave.

## <a name="aes-128-clear-key-vs-drm"></a>Clave sin cifrado AES-128 frente a DRM

Los clientes suelen preguntarse si deben usar el cifrado de AES o un sistema de DRM. La diferencia principal entre los dos sistemas es que, con el cifrado de AES, la clave de contenido se transmite al cliente en un formato sin cifrar ("fuera de peligro"). Como resultado, la clave usada para cifrar el contenido se puede ver en un seguimiento de la red en el cliente en texto sin formato. La clave sin cifrado AES-128 es adecuada para los casos de uso en los que el destinatario es una entidad de confianza (p. ej., cifrado de vídeos corporativos distribuidos dentro de una empresa para su visualización por parte de los empleados).

PlayReady, Widevine y FairPlay proporcionan un nivel de cifrado más alto en comparación con el cifrado de clave sin cifrado AES-128. La clave de contenido se transmite en un formato cifrado. Además, el descifrado se controla en un entorno seguro en el nivel de sistema operativo donde a un usuario malintencionado le resulta más difícil atacar. DRM se recomienda para los casos de uso en los que es posible que el destinatario no sea una entidad de confianza y usted requiere el nivel de seguridad más alto.

## <a name="storage-side-encryption"></a>Cifrado del lado de almacenamiento

Para proteger los recursos en reposo, estos se deben cifrar mediante el cifrado del lado de almacenamiento. En la tabla siguiente se muestra cómo funciona el cifrado del lado de almacenamiento en Media Services v3:

|Opción de cifrado|DESCRIPCIÓN|Media Services v3|
|---|---|---|---|
|Cifrado de almacenamiento en Media Services| Cifrado de AES-256, clave administrada por Media Services|Incompatible<sup>(1)</sup>|
|[Storage Service Encryption para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Cifrado en el lado del servidor que ofrece Azure Storage, clave administrada por Azure o por el cliente|Compatible|
|[Cifrado en el lado de cliente de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Cifrado en el lado de cliente que ofrece Azure Storage, clave administrada por el cliente en Key Vault|No compatible|

<sup>1</sup> En Media Services v3, el cifrado de almacenamiento (cifrado con AES-256) solo es compatible con versiones anteriores si los recursos se crearon con Media Services v2. Esto significa que la versión v3 funciona con los recursos cifrados de almacenamiento ya existentes pero no permitirá la creación de otros nuevos.

## <a name="licenses-and-keys-delivery-service"></a>Servicio de entrega de licencias y claves

Media Services proporciona un servicio de entrega de claves para proporcionar licencias DRM (PlayReady, Widevine, FairPlay) y claves AES a clientes autorizados. Puede usar la API REST o una biblioteca cliente de Media Services para configurar las directivas de autorización y autenticación de sus licencias y claves.

## <a name="control-content-access"></a>Acceso al contenido de control

Puede controlar quién tiene acceso a su contenido configurando la directiva de claves de contenido. Media Services admite varias formas de autenticar a los usuarios que realizan solicitudes de clave. Debe configurar la directiva de claves de contenido. El cliente (reproductor) debe cumplir la directiva antes de que se pueda entregar la clave al cliente. La directiva de claves de contenido puede tener una restricción de **apertura** o de **token**. 

Con una directiva de clave de contenido con restricción de token, la clave de contenido solo se enviará a un cliente que presente JSON Web Token (JWT) o Simple Web Token (SWT) válidos en la solicitud de clave o licencia. Un servicio de token seguro (STS) debe emitir este token. Puede usar Azure Active Directory como un STS o implementar un STS personalizado. Se debe configurar el STS para crear un token firmado con las notificaciones de clave y emisión que especificó en la configuración de restricción de tokens. El servicio de entrega de claves de Media Services devolverá la clave/licencia solicitada al cliente si el token es válido y las reclamaciones del token coinciden con las configuradas para la clave/licencia.

Al configurar la directiva de restricción de token, debe especificar los parámetros de clave de comprobación principal, el emisor y el público. La clave de comprobación principal contiene la clave con la que se firmó el token. El emisor es el servicio de token seguro que emite el token. El público, a veces denominado ámbito, describe la intención del token o del recurso cuyo acceso está autorizado por el token. El servicio de entrega de claves de los Media Services valida que estos valores del token coincidan con los valores de la plantilla.

## <a name="a-idfaqfrequently-asked-questions"></a><a id="faq"/>Preguntas más frecuentes

### <a name="question"></a>Pregunta

¿Cómo se implementa el sistema con DRM múltiple (PlayReady, Widevine y FairPlay) mediante Azure Media Services (AMS) v3 y cómo se usa el servicio de entrega de claves/licencias de AMS?

### <a name="answer"></a>Respuesta

Para el escenario completo, consulte el [ejemplo de código siguiente](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs). 

En el ejemplo se muestra cómo:

1. Crear y configurar ContentKeyPolicies.

  El ejemplo contiene funciones que configuran licencias de [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md) y [FairPlay](fairplay-license-overview.md).

    ```
    ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
    ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
    ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
    ```

2. Crear un objeto StreamingLocator configurado para transmitir un recurso cifrado. 

  Por ejemplo, puede establecer StreamingLocator.StreamingPolicyName en la directiva "Predefined_MultiDrmCencStreaming". Esta directiva indica que desea que se generen y establezcan dos claves de contenido (sobre y CENC) en el localizador. De esta forma, se aplican los cifrados de sobre, PlayReady y Widevine (la clave se entrega al cliente de reproducción en función de las licencias DRM configuradas). Si también quiere cifrar su transmisión con CBCS (FairPlay), utilice "Predefined_MultiDrmStreaming".

3. Crear un token de prueba.

  El método **GetTokenAsync** muestra cómo crear una prueba de token.
  
4. Crear las direcciones URL de streaming.

  El método **GetDASHStreamingUrlAsync** muestra cómo crear la dirección URL de streaming. En este caso, la dirección URL transmite el contenido de **DASH**.

### <a name="question"></a>Pregunta

¿Cómo y dónde se puede obtener el token JWT antes de usarlo para solicitar la licencia o la clave?

### <a name="answer"></a>Respuesta

1. Para entornos de producción, deberá tener servicios de token seguro (STS) (servicio web) que emite el token JWT tras una solicitud HTTPS. Para la prueba, puede usar el código que se muestra en el método **GetTokenAsync** definido en [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Una vez que se autentique un usuario, el reproductor deberá solicitar al STS dicho tipo de token y asignarlo como el valor del token. Puede usar la [API de Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/).

* Para un ejemplo de cómo ejecutar STS, con claves simétricas y asimétricas, consulte [ http://aka.ms/jwt ](https://aka.ms/jwt). 
* Para un ejemplo de un reproductor basado en Azure Media Player usando dicho token JWT, consulte [ http://aka.ms/amtest ](https://aka.ms/amtest) (expanda el vínculo "player_settings" para ver la entrada de token).

### <a name="question"></a>Pregunta

¿Cómo se autorizan las solicitudes para transmitir vídeos con cifrado AES?

### <a name="answer"></a>Respuesta

El enfoque correcto consiste en aprovechar STS (servicio de token seguro):

Según el perfil de usuario, en STS agregue notificaciones distintas (por ejemplo, "Usuario premium", "Usuario básico", "Usuario de evaluación gratuita"). Con notificaciones distintas en un token JWT, el usuario puede ver diferentes contenidos. Por supuesto, para otro contenido o recurso, la restricción ContentKeyPolicyRestriction tendrá el elemento RequiredClaims correspondiente.

Use las API de Azure Media Services para configurar la entrega de claves o licencias y cifrar los recursos (como se muestra en [este ejemplo](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

## <a name="next-steps"></a>Pasos siguientes

Consulte los artículos siguientes:

  * [Protección con el cifrado AES](protect-with-aes128.md)
  * [Protección con DRM](protect-with-drm.md)

Puede encontrar información adicional en [Diseño del sistema de protección de contenidos con drm múltiple con control de acceso](design-multi-drm-system-with-access-control.md)


