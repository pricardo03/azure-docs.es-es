---
title: Diagnóstico de notificaciones eliminadas en Notification Hubs de Azure
description: Aprenda a diagnosticar problemas comunes con las notificaciones eliminadas de Azure Notification Hubs.
services: notification-hubs
documentationcenter: Mobile
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 04/04/2019
ms.author: jowargo
ms.openlocfilehash: eebf9ef63a8622c4cc431322b786fdf30f6352fe
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925816"
---
# <a name="diagnose-dropped-notifications-in-azure-notification-hubs"></a>Diagnóstico de notificaciones eliminadas en Notification Hubs de Azure

Una pregunta común sobre Azure Notification Hubs es cómo solucionar problemas cuando las notificaciones desde una aplicación no aparecen en los dispositivos cliente. Los clientes quieren saber dónde y por qué se han quitado las notificaciones y cómo solucionar el problema. En este artículo se identifica por qué las notificaciones pueden ser eliminadas o no recibidas por los dispositivos. También se explica cómo determinar la causa raíz.

Es fundamental entender primero cómo Notification Hubs inserta notificaciones en un dispositivo.

![Arquitectura de Notification Hubs][0]

En un flujo de notificaciones de envío típico, el mensaje se envía desde el *back-end de la aplicación* a Notification Hubs. Centros de notificaciones procesa todos los registros. Tiene en cuenta las etiquetas configuradas y expresiones de etiqueta para determinar los destinos. Los destinos son los registros que deben recibir la notificación de inserción. Estos registros pueden distribuir en cualquiera de nuestras plataformas compatibles: Android, Baidu (dispositivos Android de China), OS Fire (Amazon) iOS, Windows y Windows Phone.

Con los destinos establecidos, Notification Hubs inserta las notificaciones en el *servicio de notificaciones push* para la plataforma del dispositivo. Algunos ejemplos son Apple Push Notification Service (APN) de Apple y Firebase Cloud Messaging (FCM) de Google. Notification Hubs lleva a las notificaciones a dividirse en varios lotes de registros. Autentica con el servicio de notificaciones push respectivo, según las credenciales se establecen en el portal de Azure, en **Configurar centro de notificaciones**. Luego, el servicio de notificaciones de inserción reenvía las notificaciones a los respectivos *dispositivos cliente*.

Es el tramo final de la entrega de notificaciones entre el servicio de notificaciones de inserción de la plataforma y el dispositivo. Entrega de notificaciones puede producir un error en cualquiera de las cuatro fases del proceso de notificación push (cliente, back-end de aplicación, Notification Hubs y servicio de notificaciones de inserción de la plataforma). Para más información sobre la arquitectura de Notification Hubs, consulte [Introducción a Notification Hubs].

Podría producir un error al entregar notificaciones inicial durante la fase de prueba/ensayo. Las notificaciones eliminadas en esta fase podrían indicar un problema de configuración. Si se produce un error al entregar notificaciones en producción, podrían eliminarse algunas o todas las notificaciones. En este caso, se indica una aplicación más profunda o problema de patrón de mensajería.

La siguiente sección examina los escenarios en los que las notificaciones podrían eliminarse, comprendido entre común y poco frecuentes.

## <a name="notification-hubs-misconfiguration"></a>Configuración incorrecta de Notification Hubs ##

Para enviar notificaciones al servicio de notificaciones push respectivo, Notification Hubs debe autenticarse a sí mismo en el contexto de la aplicación. Debe crear una cuenta de desarrollador con el servicio de notificación de la plataforma de destino (Microsoft, Apple, Google, etcetera.). A continuación, debe registrar la aplicación con el sistema operativo donde obtener un token o una clave que se utiliza para trabajar con el PNS de destino.

Debe agregar las credenciales de plataforma en Azure Portal. Si ninguna notificación está llegando al dispositivo, el primer paso es asegurarse de que las credenciales correctas estén configuradas en Notification Hubs. Las credenciales deben coincidir con la aplicación que se crea en una cuenta de desarrollador específicas de la plataforma.

Para obtener instrucciones paso a paso para completar este proceso, consulte [Introducción a Azure Notification Hubs].

Estos son algunos errores comunes de configuración para comprobar:

### <a name="notification-hub-name-location"></a>Ubicación del nombre de concentrador de notificación

Asegúrese de que el nombre de su centro de notificaciones (escrito sin errores) sea el mismo en cada una de estas ubicaciones:
   * Donde se registra en el cliente
   * Dónde enviar notificaciones desde el back-end
   * Donde ha configurado las credenciales de servicio de notificación de inserción

Asegúrese de utilizar las cadenas de configuración de firma de acceso compartido correctas en el cliente y la aplicación back-end. Por lo general, debe usar **DefaultListenSharedAccessSignature** en el cliente y **DefaultFullSharedAccessSignature** en la aplicación back-end. Esto concede permisos para enviar notificaciones a Notification Hubs.

### <a name="apn-configuration"></a>Configuración de APN ###

Debe mantener dos centros diferentes: uno para producción y otro para las pruebas. Debe cargar el certificado que se usa en un entorno de recinto de seguridad para un centro distinto del certificado/concentrador que se pueden usar en producción. No intente cargar diferentes tipos de certificados en el mismo centro, Provocará errores de notificación.

Si carga involuntariamente diferentes tipos de certificados en el mismo centro, debe eliminar el centro y empezar de cero con un nuevo centro. Si por alguna razón no se puede eliminar el concentrador, al menos debe eliminar todos los registros existentes del concentrador.

### <a name="fcm-configuration"></a>Configuración de FCM ###

1. Asegúrese de que el *clave de servidor* obtenida coincidencias Firebase de la clave del servidor que registró en el portal de Azure.

   ![Clave de servidor de Firebase][3]

2. Asegúrese de que ha configurado el **identificador de proyecto** en el cliente. Puede obtener el valor del **identificador de proyecto** en el panel de Firebase.

   ![Identificador del proyecto de Firebase][1]

## <a name="application-issues"></a>Problemas de la aplicación ##

### <a name="tags-and-tag-expressions"></a>Las etiquetas y expresiones de etiqueta ###

Si usa etiquetas o expresiones de etiqueta para segmentar su audiencia, es posible que cuando se envía la notificación, no se encuentra ningún destino. Este error se basa en las etiquetas especificadas o expresiones de etiqueta en su llamada de envío.

Revise los registros para asegurarse de que coinciden con las etiquetas cuando se envía una notificación. A continuación, compruebe la recepción de la notificación de solo los clientes que tienen esos registros.

Por ejemplo, suponga que todos los registros con Notification Hubs usan la etiqueta "Politics". Si, a continuación, envía una notificación con la etiqueta "Sports", no se enviará la notificación a cualquier dispositivo. Un caso complejo podría incluir expresiones de etiqueta que se ha registrado mediante el uso de "Etiqueta A" *o* "Etiqueta B", pero "Etiqueta A & & etiqueta b" de destino La sección de sugerencias de autodiagnóstico más adelante en este artículo muestra cómo revisar los registros y sus etiquetas.

### <a name="template-issues"></a>Problemas de plantilla ###

Si utiliza plantillas, asegúrese de seguir las directrices descritas en [Plantillas].

### <a name="invalid-registrations"></a>Registros no válidos ###

Si el centro de notificaciones se ha configurado correctamente y las etiquetas o expresiones de etiqueta se han utilizado correctamente, se encuentran destinos válidos. Las notificaciones se enviarán a estos destinos. Después, Notification Hubs desactiva varios lotes de procesamiento en paralelo. Cada lote envía mensajes a un conjunto de registros.

> [!NOTE]
> Puesto que Notification Hubs procesa los lotes en paralelo, no se garantiza el orden en que se entregan las notificaciones.

Notification Hubs está optimizado para un modelo de entrega de mensajes de "at-mayoría-once". Intentamos una desduplicación para que ninguna notificación se entregue más de una vez a un dispositivo. Los registros se comprueban para asegurarse de que solo un mensaje se envía por identificador de dispositivo antes de enviarlos al servicio de notificaciones de inserción.

Cada lote se envía al servicio de notificaciones de inserción, que a su vez acepta y valida los registros. Durante este proceso, es posible que el servicio de notificaciones push detecte un error con uno o más registros en un lote. El servicio de notificaciones de inserción, a continuación, devuelve un error a Notification Hubs, y se detiene el proceso. El servicio de notificaciones push elimina ese lote por completo. Esto es especialmente cierto con APNs, que utiliza un protocolo de transmisión TCP.

En este caso, se quita el registro de errores de la base de datos. A continuación, volvemos a intentar la entrega de notificaciones para el resto de los dispositivos de ese lote.

Para obtener más información sobre errores sobre el intento de entrega con error en un registro, puede usar las API de REST de Notification Hubs [telemetría por mensaje: Obtener telemetría de mensaje de notificación](https://msdn.microsoft.com/library/azure/mt608135.aspx) y [comentarios de PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). Para ver un ejemplo de código, consulte el [ejemplo de REST de envío](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/SendRestExample/).

## <a name="push-notification-service-issues"></a>Problemas del servicio de notificaciones push

Una vez que el servicio de notificaciones de inserción recibe la notificación, envía la notificación al dispositivo. En este punto, Notification Hubs no tiene ningún control sobre la entrega de la notificación al dispositivo.

Dado que los servicios de notificación de plataforma son bastante sólidos, las notificaciones tienden a llegar a los dispositivos en cuestión de segundos. Si se está limitando servicio de notificaciones push, Notification Hubs aplica una estrategia de retroceso exponencial. Si el servicio de notificaciones push sigue inaccesible durante 30 minutos, hay una directiva en la que expira y quitar los mensajes de forma permanente.

Si un servicio de notificaciones push intenta entregar una notificación pero el dispositivo está desconectado, el servicio de notificaciones push almacena la notificación. Se almacena para solo un período de tiempo limitado. La notificación se entrega al dispositivo cuando este está disponible.

Cada aplicación almacena sólo una notificación reciente. Si se envían varias notificaciones mientras el dispositivo está sin conexión, cada nueva notificación provoca que la última de ellas se descarten. Mantener solo la notificación más reciente se denomina *fusión* en APNs y *contraer* de FCM. (FCM usa una clave de contracción). Cuando el dispositivo permanece sin conexión durante un tiempo prolongado, las notificaciones que se almacenaron para el dispositivo se descartan. Para obtener más información, consulte [APNs Overview] y [Acerca de los mensajes de FCM].

Con Notification Hubs, puede pasar una clave de fusión a través de un encabezado HTTP mediante el uso de SendNotification API genérica. Por ejemplo, para el SDK de .NET, debería usar `SendNotificationAsync`. SendNotification API también toma los encabezados HTTP que se pasan tal cual al servicio de notificaciones push respectivo.

## <a name="self-diagnosis-tips"></a>Sugerencias de autodiagnóstico

Estas son las rutas de acceso para diagnosticar la causa raíz de las notificaciones eliminadas en Notification Hubs.

### <a name="verify-credentials"></a>Comprobar las credenciales ###

#### <a name="push-notification-service-developer-portal"></a>Portal de desarrollador de servicios de notificaciones de inserción ####

Compruebe las credenciales en el portal para desarrolladores de los servicios de notificaciones push respectivos (APN, FCM, servicio de notificaciones de Windows, etc.). Para más información, consulte [Tutorial: Envío de notificaciones a aplicaciones de Plataforma universal de Windows mediante Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification).

#### <a name="azure-portal"></a>Azure Portal ####

Para revisar y comparar las credenciales con las que obtuvo en el portal para desarrolladores de servicios de notificaciones de inserción, vaya a la **las directivas de acceso** ficha en el portal de Azure.

![Directivas de acceso de Azure Portal][4]

### <a name="verify-registrations"></a>Verificar los registros

#### <a name="visual-studio"></a>Visual Studio ####

En Visual Studio, puede conectarse a Azure a través del explorador de servidores para ver y administrar varios servicios de Azure, incluido Notification Hubs. Este método abreviado es útil principalmente para su entorno de desarrollo y pruebas.

![Explorador de servidores de Visual Studio][9]

Puede ver y administrar todos los registros en el centro. Los registros se pueden clasificar según la plataforma, registro nativo o de plantilla, etiqueta, identificador de servicio de notificación de inserción, Id. de registro y fecha de expiración. También puede editar un registro en esta página. Resulta especialmente útil para editar etiquetas.

Haga clic en el centro de notificaciones en **Explorador de servidores**y seleccione **diagnosticar**. 

![Explorador de servidores de Visual Studio: Diagnóstico de menú](./media/notification-hubs-diagnosing/diagnose-menu.png)

Vea la siguiente página:

![Visual Studio: Página de diagnóstico](./media/notification-hubs-diagnosing/diagnose-page.png)

Cambie a la **registros de dispositivos** página:

![Visual Studio: Registros de dispositivos](./media/notification-hubs-diagnosing/VSRegistrations.png)

Puede usar **envío de prueba** página para enviar un mensaje de notificación de prueba:

![Visual Studio: Envío de prueba](./media/notification-hubs-diagnosing/test-send-vs.png)

> [!NOTE]
> Use Visual Studio para editar registros solo durante el desarrollo y pruebas y con un número limitado de registros. Si necesita modificar sus registros de forma masiva, considere el uso de la exportación e importar la funcionalidad de registro descrito en [How To: Exportar y modificar registros en masa](https://msdn.microsoft.com/library/dn790624.aspx).

#### <a name="service-bus-explorer"></a>Explorador de Service Bus ####

Muchos clientes usan [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer) para ver y administrar sus centros de notificaciones. El Explorador de Service Bus es un proyecto de código abierto. 

### <a name="verify-message-notifications"></a>Comprobar las notificaciones de mensajes

#### <a name="azure-portal"></a>Azure Portal ####

Para enviar una notificación de prueba a los clientes sin que el back-end del servicio esté en funcionamiento, en **SOPORTE TÉCNICO Y SOLUCIÓN DE PROBLEMAS**, seleccione **Envío de prueba**.

![Funcionalidad Envío de prueba en Azure][7]

#### <a name="visual-studio"></a>Visual Studio ####

También puede enviar notificaciones de prueba desde Visual Studio.

![Funcionalidad Envío de prueba en Visual Studio][10]

Para más información acerca del uso de Notification Hubs con el Explorador de servidores de Visual Studio, consulte estos artículos:

* [Cómo ver los registros de dispositivos para notification hubs](https://docs.microsoft.com/previous-versions/windows/apps/dn792122(v=win.10))
* [En profundidad: Visual Studio 2013 Update 2 RC y Azure SDK 2.3] (Análisis a fondo: Visual Studio 2013 Update 2 RC y Azure SDK 2.3)
* [Announcing release of Visual Studio 2013 Update 3 and Azure SDK 2.4] (Anuncio del lanzamiento de Visual Studio 2013 Update 3 y Azure SDK 2.4)

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Depuración de las notificaciones incorrectas y revisión del resultado de la notificación

#### <a name="enabletestsend-property"></a>Propiedad EnableTestSend ####

Cuando se envía una notificación a través de Notification Hubs, inicialmente se pone en cola la notificación. Notification Hubs determina los destinos correctos y después envía la notificación al servicio de notificaciones push. Si usa la API de REST o cualquiera de los SDK de cliente, la devolución de la llamada de envío solo implica que el mensaje se pone en cola con Notification Hubs. No ofrece información detallada sobre lo que ocurrió cuando Notification Hubs envió finalmente la notificación al servicio de notificaciones de inserción.

Si la notificación no llega al dispositivo cliente, es posible que han producido un error cuando Notification Hubs intentó entregarlo al servicio de notificaciones de inserción. Por ejemplo, el tamaño de la carga podría exceder del máximo permitido por el servicio de notificaciones push, o las credenciales configuradas en Notification Hubs podrían no ser válidas.

Para obtener información sobre los errores del servicio de notificaciones push, puede utilizar la propiedad [EnableTestSend]. Esta propiedad se habilita automáticamente cuando envía mensajes de prueba desde el portal o el cliente de Visual Studio. Puede utilizar esta propiedad para ver información de depuración detallada y también a través de API. Actualmente, se puede utilizar en .NET SDK. Se agregará a todos los SDK de cliente al final.

Para usar la propiedad `EnableTestSend` con la llamada a REST, anexe un parámetro de cadena de consulta llamado *test* al final de su llamada de envío. Por ejemplo: 

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

#### <a name="net-sdk-example"></a>Ejemplo de SDK de .NET ####

Este es un ejemplo de uso de .NET SDK para enviar una notificación emergente nativa (notificación del sistema):

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Al final de la ejecución, `result.State` sólo indica `Enqueued`. Los resultados no proporcionan ninguna información sobre qué ha ocurrido con la notificación push.

A continuación, puede usar la propiedad booleana `EnableTestSend`. Use la propiedad `EnableTestSend` mientras se inicializa `NotificationHubClient` para obtener el estado detallado de los errores del servicio de notificaciones push que se producen cuando se envía la notificación. La llamada de envío tarda más tiempo en devolver debido a que primero debe Notification Hubs para entregar la notificación al servicio de notificaciones push.

```csharp
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }
```

#### <a name="sample-output"></a>Salida de ejemplo ####

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Este mensaje indica que las credenciales configuradas en Notification Hubs son válidas o que hay un problema con los registros en el concentrador. Eliminar este registro y deje que el cliente a volver a crear el registro antes de enviar el mensaje.

> [!NOTE]
> El uso de la propiedad `EnableTestSend` está muy limitado. Use esta opción únicamente en un entorno de desarrollo/pruebas y con un conjunto limitado de registros. Depurar las notificaciones se envían únicamente a 10 dispositivos. También hay un límite de procesamiento de envíos de depuración, en el 10 por minuto.

### <a name="review-telemetry"></a>Revisar la telemetría ###

#### <a name="azure-portal"></a>Azure Portal ####

En el portal, puede obtener una rápida introducción de toda la actividad que tiene lugar en el centro de notificaciones.

1. En la pestaña **Información general**, puede ver una vista agregada de registros, notificaciones y errores por plataforma.

   ![Panel de introducción a Notification Hubs][5]

2. En la pestaña **Supervisión**, puede agregar muchas otras métricas específicas de la plataforma para obtener una visión más profunda. Puede buscar específicamente a los errores que se devuelven cuando Notification Hubs intenta enviar la notificación al servicio de notificaciones push.

   ![Registro de actividad de Azure Portal][6]

3. Comience por revisar **los mensajes entrantes**, **las operaciones de registro** y las **notificaciones correctas**. Después, vaya a la pestaña por plataforma para revisar los errores específicos del servicio de notificaciones push.

4. Si la configuración de autenticación para el centro de notificaciones no es correcta, se muestra el mensaje **Error de autenticación PNS**. Es una buena indicación para comprobar las credenciales de servicio de notificación de inserción.

#### <a name="programmatic-access"></a>Acceso mediante programación ####

Para obtener más información sobre el acceso mediante programación, vea [acceso mediante programación](https://docs.microsoft.com/previous-versions/azure/azure-services/dn458823(v=azure.100)).

> [!NOTE]
> Varias características relacionadas con la telemetría, como la exportación e importación de registros y el acceso a la telemetría a través de API, solo están disponibles en el nivel de servicio Estándar. Si intenta usar estas características de gratis o básico de nivel de servicio, obtendrá un mensaje de excepción, si usa el SDK. Si usa las características directamente desde las API de REST, obtendrá un error HTTP 403 (prohibido).
>
> Para usar las características relacionadas con la telemetría, asegúrese primero en el portal de Azure que está usando el nivel de servicio estándar.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/FCMConfigure.png
[3]: ./media/notification-hubs-diagnosing/FCMServerKey.png
[4]: ../../includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalAnalytics.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[Introducción a Notification Hubs]: notification-hubs-push-notification-overview.md
[Introducción a Azure Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Plantillas]: https://msdn.microsoft.com/library/dn530748.aspx
[APNs overview]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html (Introducción a Apple Push Notification Service)
[Acerca de los mensajes de FCM]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: https://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[View device registrations for notification hubs]: https://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[En profundidad: Visual Studio 2013 Update 2 RC y Azure SDK 2.3]: https://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs (Análisis a fondo: Visual Studio 2013 Update 2 RC y Azure SDK 2.3)
[Announcing release of Visual Studio 2013 Update 3 and Azure SDK 2.4]: https://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ (Anuncio del lanzamiento de Visual Studio 2013 Update 3 y Azure SDK 2.4)
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programmatic telemetry access]: https://msdn.microsoft.com/library/azure/dn458823.aspx
