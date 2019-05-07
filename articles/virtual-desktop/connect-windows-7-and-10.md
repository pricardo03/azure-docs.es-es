---
title: Conectarse a la vista previa de Escritorio Virtual de Windows desde Windows 10 o Windows 7 - Azure
description: Cómo conectarse a la vista previa de Escritorio Virtual de Windows desde Windows 10 o Windows 7.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/24/2019
ms.author: helohr
ms.openlocfilehash: b7d7b25d0355f2379b90313f17e2b595234df827
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145990"
---
# <a name="connect-from-windows-10-or-windows-7"></a>Conectarse desde Windows 10 o Windows 7

> Se aplica a: Windows 7 y Windows 10.

Está disponible un cliente descargable que proporciona acceso a los recursos de la vista previa de Escritorio Virtual Windows desde dispositivos que ejecutan Windows 7 y Windows 10.

> [!IMPORTANT]
> No use **de RemoteApp y escritorio conexiones (RADC)** o **conexión de escritorio remoto (MSTSC)** para tener acceso a los recursos de Escritorio Virtual de Windows porque Escritorio Virtual de Windows no es compatible con cualquier cliente.

## <a name="install-the-client"></a>Instalar el cliente

[Descargar](https://go.microsoft.com/fwlink/?linkid=2068602) e instalar el cliente a su equipo local. La instalación requiere derechos de administrador.

## <a name="subscribe-to-a-feed"></a>Suscribirse a una fuente

Obtener la lista de recursos administrados disponibles para usted, suscríbase a la fuente proporcionada por el administrador. La suscripción hace que los recursos disponibles en su equipo local.

Para suscribirse a una fuente de distribución:

1. Iniciar el cliente en la lista de todas las aplicaciones, busque por **escritorio remoto**.
1. Seleccione **Subscribe** en la página principal para conectarse al servicio y recuperar los recursos.
1. **Inicie sesión en** con su cuenta de usuario cuando se le solicite.

Después de autenticarse correctamente, ahora debería ver una lista de recursos disponibles para usted.

Puede iniciar los recursos en uno de estos dos métodos.

- Desde la página del cliente principal, haga doble clic en un recurso para iniciarlo.
- Iniciar un recurso como lo haría normalmente en otras aplicaciones en el menú Inicio.
  - También puede buscar las aplicaciones en la barra de búsqueda.

Una vez suscrito a una fuente, se actualiza automáticamente el contenido de la fuente de forma periódica. Los recursos se pueden agregar, cambiar o eliminan de acuerdo con los cambios realizados por el administrador.

## <a name="view-the-details-of-a-feed"></a>Ver los detalles de una fuente de distribución

Después de suscribirse, puede ver información adicional acerca de la fuente mediante el acceso al panel de detalles.

1. En la página del cliente principal, seleccione los puntos suspensivos (**...** ) a la derecha del nombre de fuente.
1. En el menú desplegable, seleccione **detalles**.
1. El panel de detalles se muestra en el lado derecho del cliente.

El panel de detalles contiene información útil acerca de la fuente:

- La dirección URL y el nombre de usuario utilizado para suscribirse
- El número de aplicaciones y escritorios
- La fecha y hora de la última actualización
- El estado de la última actualización

Si es necesario, puede iniciar una actualización manual mediante la selección en **actualizar ahora**.

## <a name="unsubscribe-from-a-feed"></a>Cancelar la suscripción a una fuente de distribución

En esta sección le mostrará cómo cancelar la suscripción a una fuente. Puede cancelar la suscripción para suscribirse de nuevo con una cuenta diferente o quitar los recursos del sistema.

1. En la página del cliente principal, seleccione los puntos suspensivos (**...** ) a la derecha del nombre de fuente.
1. En el menú desplegable, seleccione **Unsubscribe**.
1. Revise y seleccione **continuar** desde el cuadro de diálogo.

## <a name="update-the-client"></a>Actualizar el cliente

Cuando hay disponible una versión nueva del cliente, se le notificará por el cliente y el centro de actividades de Windows. Seleccione la notificación para iniciar el proceso de actualización.
