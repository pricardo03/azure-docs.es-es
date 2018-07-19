---
title: Introducción a las plantillas de licencias de Azure Media Services con Widevine | Microsoft Docs
description: En este tema se proporciona información general sobre una plantilla de licencia de Widevine que se usó para configurar las licencias de Widevine.
author: juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: juliako
ms.openlocfilehash: e3af5efd253458401c13f6174d9567f932482eb0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133444"
---
# <a name="widevine-license-template-overview"></a>Información general sobre las plantillas de licencias de Widevine

Puede usar Azure Media Services para configurar y solicitar licencias de Widevine de Google. Cuando el reproductor intenta reproducir contenido protegido de Widevine, se envía una solicitud al servicio de entrega de licencias para obtener una licencia. Si el servicio de licencias aprueba la solicitud, el servicio emite la licencia. A continuación, se envía al cliente y se usa para descifrar y reproducir el contenido especificado.

Una solicitud de licencia de Widevine tiene el formato de un mensaje JSON.  

>[!NOTE]
> Puede crear un mensaje vacío y sin valores usando simplemente "{}". A continuación, se crea una plantilla de licencia con los valores predeterminados. La configuración predeterminada funciona para la mayoría de los casos. Los escenarios de entrega de licencia basados en Microsoft deben utilizar siempre los valores predeterminados. Si tiene que establecer los valores de "provider" y "content_id", el proveedor debe coincidir con las credenciales de Widevine.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>Mensaje JSON

| NOMBRE | Valor | DESCRIPCIÓN |
| --- | --- | --- |
| payload |Cadena codificada en Base64 |La solicitud de licencia enviada por un cliente. |
| content_id |Cadena codificada en Base64 |Identificador utilizado para derivar el id. de la clave y la clave de contenido de cada elemento content_key_specs.track_type. |
| provider |string |Utilizado para buscar directivas y claves de contenido. En el caso de que se use la entrega de claves de Microsoft para entregar licencias de Widevine, este parámetro se omite. |
| policy_name |string |Nombre de una directiva previamente registrada. Opcional. |
| allowed_track_types |enum |SD_ONLY o SD_HD. Controla qué claves de contenido deben incluirse en una licencia. |
| content_key_specs |Matriz de estructuras JSON; consulte "Especificaciones de clave de contenido".  |Un control más preciso sobre qué claves de contenido se devolverán. Para obtener más información, consulte la sección "Especificaciones de clave de contenido". Solo se puede especificar uno de los valores de allowed_track_types y content_key_specs. |
| use_policy_overrides_exclusively |Valor booleano (true o false) |Utilice los atributos de directiva que especifica policy_overrides y omita todas las directivas almacenadas previamente. |
| policy_overrides |Estructura JSON; consulte "Invalidaciones de directivas". |Configuración de directiva para esta licencia.  En caso de que este activo tenga una directiva definida previamente, se usarán estos valores especificados. |
| session_init |Estructura JSON; consulte "Inicialización de la sesión". |Los datos opcionales se pasan a la licencia. |
| parse_only |Valor booleano (true o false) |Se analiza la solicitud de licencia, pero no se emite ninguna licencia. Sin embargo, los valores de la solicitud de licencia se devuelven en la respuesta. |

## <a name="content-key-specs"></a>Especificaciones de clave de contenido
Si existe una directiva anterior, no es necesario especificar ninguno de los valores en las especificaciones de clave de contenido. La directiva anterior que estaba asociada con este contenido se usa para determinar la protección de salida como, por ejemplo, la Protección de contenido digital de ancho de banda alto (HDCP) y Copy General Management System (CGMS). Si no hay una directiva anterior registrada con el servidor de licencias de Widevine, el proveedor de contenido puede insertar los valores en la solicitud de licencia.   

Cada valor de content_key_specs debe especificarse en todas las pistas, independientemente de la opción use_policy_overrides_exclusively. 

| NOMBRE | Valor | DESCRIPCIÓN |
| --- | --- | --- |
| content_key_specs. track_type |string |Un nombre de tipo de pista. Si se especifica content_key_specs en la solicitud de licencia, asegúrese de especificar todos los tipos de pista explícitamente. Si no lo hace, se producirán errores en la reproducción transcurridos 10 segundos. |
| content_key_specs  <br/> security_level |uint32 |Define los requisitos de solidez del cliente para la reproducción. <br/> - Se requiere criptografía de caja blanca basada en software. <br/> - Se requiere criptografía de software y un descodificador de ofuscación. <br/> - Las operaciones de criptografía y material clave deben realizarse en un entorno de ejecución de confianza con respaldo del hardware. <br/> - La criptografía y la descodificación del contenido deben realizarse dentro de un entorno de ejecución de confianza con respaldo del hardware.  <br/> - La criptografía, la descodificación y todo el tratamiento de los medios (comprimidos y descomprimidos) deben administrarse dentro de un entorno de ejecución de confianza con respaldo del hardware. |
| content_key_specs <br/> required_output_protection.hdc |cadena, una de HDCP_NONE, HDCP_V1, HDCP_V2 |Indica si se requiere HDCP. |
| content_key_specs <br/>key |Base64-<br/>cadena codificada |Clave de contenido que se utilizará para esta pista. Si se especifica, se requiere track_type o key_id. Esta opción permite que el proveedor de contenido inserte la clave de contenido para esta pista en lugar de permitir que el servidor de licencias de Widevine genere o busque una clave. |
| content_key_specs.key_id |Binario de cadena codificada en Base64, 16 bytes |Identificador único para la clave. |

## <a name="policy-overrides"></a>Invalidaciones de directivas
| NOMBRE | Valor | DESCRIPCIÓN |
| --- | --- | --- |
| policy_overrides&#46;can_play |Valor booleano (true o false) |Indica que la reproducción del contenido está permitida. El valor predeterminado es false. |
| policy_overrides&#46;can_persist |Valor booleano (true o false) |Indica que la licencia puede conservarse en el almacenamiento no volátil para su uso sin conexión. El valor predeterminado es false. |
| policy_overrides&#46;can_renew |Valor booleano (true o false) |Indica que se permite la renovación de la presente licencia. Si es true, se puede ampliar la duración de la licencia mediante latido. El valor predeterminado es false. |
| policy_overrides&#46;license_duration_seconds |int64 |Indica el período de tiempo para esta licencia específica. Un valor de 0 indica que no hay ningún límite para la duración. El valor predeterminado es 0. |
| policy_overrides&#46;rental_duration_seconds |int64 |Indica el período de tiempo en el que se permite la reproducción. Un valor de 0 indica que no hay ningún límite para la duración. El valor predeterminado es 0. |
| policy_overrides&#46;playback_duration_seconds |int64 |El período de tiempo de visualización una vez que la reproducción comienza en el plazo de duración de la licencia. Un valor de 0 indica que no hay ningún límite para la duración. El valor predeterminado es 0. |
| policy_overrides&#46;renewal_server_url |string |Todas las solicitudes de latido (renovación) de esta licencia se dirigirán a la dirección URL especificada. Este campo solo se usa si can_renew es "true". |
| policy_overrides&#46;renewal_delay_seconds |int64 |El número de segundos después de license_start_time, antes de intentar la renovación por primera vez. Este campo solo se usa si can_renew es "true". El valor predeterminado es 0. |
| policy_overrides&#46;renewal_retry_interval_seconds |int64 |Especifica el plazo en segundos entre las posteriores solicitudes de renovación de licencia, en caso de error. Este campo solo se usa si can_renew es "true". |
| policy_overrides&#46;renewal_recovery_duration_seconds |int64 |El período de tiempo en el que la reproducción puede continuar mientras se intenta la renovación, aunque no se realice correctamente debido a problemas de back-end con el servidor de licencias. Un valor de 0 indica que no hay ningún límite para la duración. Este campo solo se usa si can_renew es "true". |
| policy_overrides&#46;renew_with_usage |Valor booleano (true o false) |Indica que la licencia se enviará para renovación cuando se inicie el uso. Este campo solo se usa si can_renew es "true". |

## <a name="session-initialization"></a>Inicialización de la sesión
| NOMBRE | Valor | DESCRIPCIÓN |
| --- | --- | --- |
| provider_session_token |Cadena codificada en Base64 |Este token de sesión se pasa de nuevo en la licencia y existirá en renovaciones posteriores. El token de sesión no se conservará una vez agotadas las sesiones. |
| provider_client_token |Cadena codificada en Base64 |Token de cliente para devolver en la respuesta de licencia. Si la solicitud de licencia contiene un token de cliente, este valor se omite. El token del cliente se conservará una vez agotadas las sesiones de licencia. |
| override_provider_client_token |Valor booleano (true o false) |Si es false y la solicitud de licencia contiene un token de cliente, use el token de la solicitud incluso si se especificó un token de cliente en esta estructura. Si es true, utilice siempre el token especificado en esta estructura. |

## <a name="configure-your-widevine-license-with-net"></a>Configuración de la licencia de Widevine con .NET 

Media Services proporciona una clase que le permite configurar una licencia de Widevine. Para construir la licencia, pase el JSON a [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Para configurar la plantilla puede:

1.  Construya o codifique de forma rígida directamente una cadena JSON (que puede ser propensa a errores)

    ```csharp
        ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
    };
    ```

2.  Construya las clases necesarias con propiedades que se asignen a esos atributos JSON y que creen instancias antes de serializarlos en la cadena JSON. A continuación se muestra un ejemplo de este tipo de clases y cómo se crean instancias con ellas y se serializan.

    ```csharp
    public class policy_overrides
    {
        public bool can_play { get; set; }
        public bool can_persist { get; set; }
        public bool can_renew { get; set; }
        public int rental_duration_seconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int playback_duration_seconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int license_duration_seconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
    }

    public class content_key_spec
    {
        public string track_type { get; set; }
        public int security_level { get; set; }
        public output_protection required_output_protection { get; set; }
    }

    public class output_protection
    {
        public string hdcp { get; set; }
    }

    public class widevine_template
    {
        public string allowed_track_types { get; set; }
        public content_key_spec[] content_key_specs { get; set; }
        public policy_overrides policy_overrides { get; set; }
    }
    ```

### <a name="configure-the-license"></a>Configuración de la licencia

Utilice las clases definidas en la sección anterior para crear el archivo JSON que se usa para configurar [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate):

```csharp
void ConfigureLicense()
{
    widevine_template objwidevine_template = new widevine_template()
    {
        allowed_track_types = "SD_HD",
        content_key_specs = new content_key_spec[]
        {
            new content_key_spec()
            {
                track_type = "SD",
                security_level = 1,
                required_output_protection = new output_protection()
                {
                hdcp = "HDCP_V2"
                }
            }
        },
        policy_overrides = new policy_overrides()
        {
            can_play = true,
            can_persist = true,
            can_renew = false,
            license_duration_seconds = 2592000,
            playback_duration_seconds = 10800,
            rental_duration_seconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(objwidevine_template)
    };
}
```

## <a name="next-steps"></a>Pasos siguientes

[Información general](content-protection-overview.md)