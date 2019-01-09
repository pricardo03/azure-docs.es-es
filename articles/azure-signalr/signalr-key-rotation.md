---
title: Rotación de claves de acceso para Azure SignalR Service
description: Información general sobre por qué el cliente debe rotar periódicamente las claves de acceso y cómo hacerlo con la GUI del portal y la CLI.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 2c0f60b0ef3a90372fc4a095c830f39bc148f354
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636087"
---
# <a name="access-key-rotation-for-azure-signalr-service"></a>Rotación de claves de acceso para Azure SignalR Service

Cada instancia de Azure SignalR Service tiene un par de claves de acceso: claves principales y secundarias. Se usan para autenticar a los clientes de SignalR al realizar solicitudes al servicio. Las claves están asociadas con la dirección URL del punto de conexión de instancia. Mantenga seguras las claves y realice su rotación de forma periódica. Como se le proporcionan dos claves de acceso, puede mantener las conexiones con una mientras se regenera la otra.

## <a name="why-rotate-access-keys"></a>¿Por qué se deben rotar las claves de acceso?

Por motivos de seguridad y para satisfacer los requisitos de cumplimiento, se recomienda a los desarrolladores rotar periódicamente las claves de acceso.

## <a name="how-to-regenerate-access-keys"></a>¿Cómo se regeneran las claves de acceso?

1. Vaya a [Azure Portal](https://portal.azure.com/) e inicie sesión con sus credenciales.

1. Busque la sección **Claves** en la instancia de Azure SignalR Service para la que desee regenerar las claves.

1. Haga clic en **Claves** en el menú de navegación.

1. Haga clic en **Regenerar clave principal** o **Regenerar clave secundaria**.

Se creará y se mostrará una nueva clave y la cadena de conexión correspondiente.

 ![Regeneración de claves](media/signalr-key-rotation/regenerate-keys.png)

También puede regenerar claves mediante la [CLI de Azure](/cli/azure/ext/signalr/signalr/key?view=azure-cli-latest#ext-signalr-az-signalr-key-renew).

## <a name="update-configurations-with-new-connection-strings"></a>Actualización de configuraciones con nuevas cadenas de conexión

1. Copie la cadena de conexión recién generada.

1. Actualice todas las configuraciones para usar la nueva cadena de conexión.

1. Reinicie la aplicación según sea necesario.

## <a name="forced-access-key-regeneration"></a>Regeneración forzada de claves de acceso

En determinadas situaciones, Azure SignalR Service puede exigir una regeneración obligatoria de las claves de acceso. El servicio lo notificará a los clientes por correo electrónico y el portal. Si recibe este tipo de comunicación o encuentra errores del servicio debido a la clave de acceso, siga esta guía para rotar las claves.

## <a name="next-steps"></a>Pasos siguientes

Como buena práctica de seguridad, se recomienda rotar las claves de acceso con frecuencia.

En esta guía ha aprendido sobre cómo volver a generar las claves de acceso. Continúe con los siguientes tutoriales sobre la autenticación con OAuth o Azure Functions.

> [!div class="nextstepaction"]
> [Integración con la identidad de ASP.NET Core](./signalr-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Creación de una aplicación en tiempo real sin servidor con autenticación](./signalr-authenticate-azure-functions.md)