---
title: 'Conexión a Windows Virtual Desktop desde iOS: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente iOS.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: helohr
ms.openlocfilehash: 764ed4fefd1a3aba1f0b7812fa2965505aa34161
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338687"
---
# <a name="connect-with-the-ios-client"></a>Conexión con el cliente iOS

> Se aplica a: iOS 8.0, o cualquier versión posterior. Compatible con iPhone, iPad y iPod touch.

>[!NOTE]
> El cliente iOS está actualmente en versión preliminar.

Puede acceder a los recursos de Windows Virtual Desktop desde su dispositivo iOS con nuestro cliente, que se puede descargar. En esta guía se le indicará cómo configurar el cliente iOS.

## <a name="install-the-ios-beta-client"></a>Instalación del cliente de la versión beta de iOS
Para instalar el cliente de la versión beta de iOS:

1. Instale la aplicación [Apple TestFlight](https://apps.apple.com/us/app/testflight/id899247664) en el dispositivo iOS.
2. En el dispositivo iOS, abra un explorador y vaya a [aka.ms/rdiosbeta](https://aka.ms/rdiosbeta).
3. En la etiqueta **Step 2: Join the Beta** (Paso 2: participar en la versión beta), seleccione **Start Testing** (Iniciar la prueba).
4. Cuando se le redireccione a la aplicación TestFlight, seleccione **Accept** (Aceptar) y, después, **Install** (Instalar).

## <a name="subscribe-to-a-feed"></a>Suscripción a una fuente

Suscríbase a la fuente que proporciona su administrador para obtener la lista de recursos administrados a los que puede acceder en el dispositivo iOS.

Para suscribirse a una fuente:

1. En el centro de conexiones, pulse **+** y, después, pulse **Add Workspace** (Agregar área de trabajo).
2. Escriba la dirección URL de la fuente en el campo **Feed URL** (Dirección URL de la fuente). La dirección URL de la fuente puede ser una dirección URL o una dirección de correo electrónico.
   - Si usa una dirección URL, use la que le proporcionó el administrador. Normalmente, la dirección URL es <https://rdweb.wvd.microsoft.com>.
   - Para usar el correo electrónico, escriba su dirección de correo electrónico. Esto indica al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador configuró el servidor de esa forma.
3. Pulse **Next** (Siguiente).
4. Escriba sus credenciales cuando se le solicite.
   - En **User Name** (Nombre de usuario), asigne al nombre de usuario permiso para acceder a los recursos.
   - En **Password** (Contraseña), escriba la contraseña asociada al nombre de usuario.
   - También se le puede solicitar que proporcione otra información si el administrador configuró la autenticación de esa forma.
5. Pulse **Save** (Guardar).

Después, el centro de conexiones debería mostrar los recursos remotos.

Una vez que se haya suscrito a una fuente, el contenido de la misma se actualizará automáticamente de forma periódica. Pueden agregarse, cambiarse o quitarse recursos de acuerdo con los cambios hechos por el administrador.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el cliente de la versión beta de iOS, consulte el documento [Introducción al cliente de iOS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios).
