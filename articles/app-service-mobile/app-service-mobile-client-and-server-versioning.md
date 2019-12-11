---
title: Control de versiones del SDK de cliente y servidor
description: Lista de SDK de cliente y compatibilidad con las versiones del SDK de servidor para Mobile Services y Azure Mobile Apps.
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.openlocfilehash: 4d0b301dee363c2338cb13a9fc09ee17549467eb
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668842"
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Control de versiones de cliente y servidor en Mobile Apps y Mobile Services
> [!NOTE]
> Visual Studio App Center admite servicios integrados de un extremo a otro fundamentales para el desarrollo de aplicaciones móviles. Los desarrolladores pueden usar los servicios de **compilación**, **prueba** y **distribución** para configurar la canalización de integración y entrega continuas. Una vez que se ha implementado la aplicación, los desarrolladores pueden supervisar el estado y el uso de su aplicación con los servicios de **análisis** y **diagnóstico**, e interactuar con los usuarios que utilizan el servicio de **Push** (inserción). Además, los desarrolladores pueden aprovechar **Auth** para autenticar a los usuarios y el servicio de **datos** para almacenar y sincronizar los datos de la aplicación en la nube.
>
> Si está pensando en integrar servicios en la nube en su aplicación para dispositivos móviles, regístrese en [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) hoy mismo.

La versión más reciente de Azure Mobile Services es la característica **Mobile Apps** de Azure App Service.

Los SDK de cliente y servidor de Mobile Apps originalmente se basaban en los de Mobile Services, pero *no* son compatibles entre sí.
Es decir, debe usar el SDK de cliente de *Mobile Apps* con un SDK de servidor de *Mobile Apps* y algo parecido sucede con *Mobile Services*. Este contrato se aplica a través de un valor de encabezado especial usado por los SDK de cliente y servidor, `ZUMO-API-VERSION`.

Nota: cada vez que este documento hace referencia a un back-end de *Mobile Services* , no es necesario que esté hospedado en Mobile Services. Ahora es posible migrar un servicio móvil para que se ejecute en App Service sin realizar ningún cambio en el código, pero el servicio seguirá usando versiones de SDK de *Mobile Services*.

## <a name="header-specification"></a>Especificación del encabezado
La clave `ZUMO-API-VERSION` se puede especificar en el encabezado HTTP o en la cadena de consulta. El valor es una cadena de versión con el formato **x.y.z**.

Por ejemplo:

GET https://service.azurewebsites.net/tables/TodoItem

HEADERS: ZUMO-API-VERSION: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Anulación de la comprobación de la versión
Para anular la comprobación de la versión, establezca el valor **true** en la configuración de la aplicación **MS_SkipVersionCheck**. Especifique esto en el archivo web.config o en la sección Configuración de la aplicación del Portal de Azure.

> [!NOTE]
> Hay una serie de cambios de comportamiento entre Mobile Services y Mobile Apps, especialmente en las áreas de sincronización sin conexión, autenticación y notificaciones push. Solo debe anular la comprobación de versión después de realizar pruebas exhaustivas para asegurarse de que estos cambios de comportamiento no impiden la funcionalidad de la aplicación.

## <a name="2.0.0"></a>Cliente y servidor de Azure Mobile Apps
### <a name="MobileAppsClients"></a> SDK de cliente de *Aplicaciones* móviles
La comprobación de versión se introdujo a partir de las siguientes versiones del SDK de cliente de **Azure Mobile Apps**:

| Plataforma de cliente | Versión | Valor de encabezado de versión |
| --- | --- | --- |
| Cliente administrado (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](https://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

### <a name="mobile-apps-server-sdks"></a>SDK de servidor de *Mobile Apps*
La comprobación de versión se incluye en las siguientes versiones del SDK de servidor:

| Plataforma de servidor | SDK | Encabezado de versión aceptado |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[azure-mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Comportamiento de back-ends de Mobile Apps
| ZUMO-API-VERSION | Valor de MS_SkipVersionCheck | Response |
| --- | --- | --- |
| x.y.z o Null |True |200 - CORRECTO |
| Null |False/Sin especificar |400 - Solicitud incorrecta |
| 1.x.y |False/Sin especificar |400 - Solicitud incorrecta |
| 2.0.0-2.x.y |False/Sin especificar |200 - CORRECTO |
| 3.0.0-3.x.y |False/Sin especificar |400 - Solicitud incorrecta |

[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients
[Mobile App Server SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
