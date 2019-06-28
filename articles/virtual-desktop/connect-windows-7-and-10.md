---
title: Conectarse a Windows Virtual Desktop, versión preliminar, desde Windows 10 o Windows 7 - Azure
description: Procedimiento para conectarse a Windows Virtual Desktop, versión preliminar, desde Windows 10 o Windows 7.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/24/2019
ms.author: helohr
ms.openlocfilehash: b7d7b25d0355f2379b90313f17e2b595234df827
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65145990"
---
# <a name="connect-from-windows-10-or-windows-7"></a>Conexión desde Windows 10 o Windows 7

> Se aplica a: Windows 7 y Windows 10.

Hay disponible un cliente descargable que proporciona acceso a los recursos de Windows Virtual Desktop, versión preliminar, desde dispositivos que ejecutan Windows 7 y Windows 10.

> [!IMPORTANT]
> No use **Conexión de RemoteApp y Escritorio (RADC)** ni **Conexión a Escritorio remoto (MSTSC)** para acceder a los recursos de Windows Virtual Desktop, ya que Windows Virtual Desktop no admite ninguno de los clientes.

## <a name="install-the-client"></a>Instalar el cliente

[Descargue](https://go.microsoft.com/fwlink/?linkid=2068602) e instale el cliente en su equipo local. La instalación requiere derechos de administrador.

## <a name="subscribe-to-a-feed"></a>Suscripción a una fuente

Para obtener la lista de recursos administrados a su disposición, suscríbase a la fuente proporcionada por el administrador. La suscripción hace que los recursos estén disponibles en su equipo local.

Para suscribirse a una fuente:

1. Inicie el cliente desde la lista Todas las aplicaciones, busque **Escritorio remoto**.
1. Seleccione **Suscribirse** en la página principal para conectarse al servicio y recuperar los recursos.
1. **Inicie sesión** con su cuenta de usuario cuando se le solicite.

Después de autenticarse correctamente, debería ver una lista de recursos a su disposición.

Puede iniciar los recursos mediante uno de estos dos métodos.

- Desde la página principal del cliente, haga doble clic en un recurso para iniciarlo.
- Inicie un recurso como lo haría normalmente en otras aplicaciones desde el menú Inicio.
  - También puede buscar las aplicaciones en la barra de búsqueda.

Una vez que se haya suscrito a una fuente, el contenido de la fuente se actualiza automáticamente de forma periódica. Pueden agregarse, cambiarse o quitarse recursos de acuerdo con los cambios hechos por el administrador.

## <a name="view-the-details-of-a-feed"></a>Ver los detalles de una fuente

Después de suscribirse, puede ver información adicional acerca de la fuente si accede al panel de detalles.

1. En la página principal del cliente, seleccione los puntos suspensivos ( **…** ) a la derecha del nombre de fuente.
1. En el menú desplegable, seleccione **Detalles**.
1. El panel Detalles se muestra en el lado derecho del cliente.

El panel Detalles contiene información útil acerca de la fuente:

- La dirección URL y el nombre de usuario usados para suscribirse
- El número de aplicaciones y escritorios
- La fecha y hora de la última actualización
- El estado de la última actualización

Si es necesario, puede iniciar una actualización manual al seleccionar **Actualizar ahora**.

## <a name="unsubscribe-from-a-feed"></a>Cancelar la suscripción a una fuente

En esta sección verá cómo cancelar la suscripción a una fuente. Puede cancelar la suscripción para suscribirse de nuevo con otra cuenta o quitar los recursos del sistema.

1. En la página principal del cliente, seleccione los puntos suspensivos ( **…** ) a la derecha del nombre de fuente.
1. En el menú desplegable, seleccione **Cancelar suscripción**.
1. Revise y seleccione **Continuar** desde el cuadro de diálogo.

## <a name="update-the-client"></a>Actualizar el cliente

Cuando haya disponible una nueva versión del cliente, el cliente y el centro de actividades de Windows le notificarán. Seleccione la notificación para iniciar el proceso de actualización.
