---
title: 'Azure Event Hubs: Habilitación de la integración y los firewalls de redes virtuales'
description: En este tutorial, aprenderá a integrar Event Hubs con redes virtuales y firewalls para habilitar el acceso seguro.
services: event-hubs
author: axisc
manager: darosa
ms.author: aschhab
ms.date: 12/20/2019
ms.topic: tutorial
ms.service: event-hubs
ms.custom: mvc
ms.openlocfilehash: f911a1513c6f89180ea51cc0de96dc8a475c7fc8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75437118"
---
# <a name="tutorial-enable-virtual-networks-integration-and-firewalls-on-event-hubs-namespace"></a>Tutorial: Habilitar la integración y los firewalls de redes virtuales en el espacio de nombres de Event Hubs

Los [puntos de conexión de servicio de redes virtuales](../virtual-network/virtual-network-service-endpoints-overview.md) extienden el espacio de direcciones privadas de la red virtual y la identidad de la red virtual a los servicios de Azure mediante una conexión directa. Los puntos de conexión permiten proteger los recursos de servicio de Azure críticos únicamente para las redes virtuales. El tráfico desde la red virtual al servicio de Azure siempre permanece en la red troncal de Microsoft Azure.

Los firewalls le permiten limitar el acceso al espacio de nombres de Event Hubs desde determinadas direcciones IP o intervalos de direcciones IP.

Este tutorial muestra cómo integrar los puntos de conexión de servicio de redes virtuales y cómo configurar los firewalls (filtrado de IP) con el espacio de nombres de Azure Event Hubs existente, mediante Azure Portal.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Integrar los puntos de conexión de servicio de redes virtuales con el espacio de nombres de Event Hubs.
> * Configurar el firewall (filtrado de IP) con el espacio de nombres de Event Hubs.

>[!WARNING]
> La implementación de la integración de instancias de Virtual Network puede evitar que otros servicios de Azure interactúen con Event Hubs.
>
> Las integraciones propias no se admiten si las redes virtuales están habilitadas.
> Escenarios comunes de Azure que no funcionan con redes virtuales:
> * Azure Diagnostics y registro
> * Azure Stream Analytics
> * Integración con Event Grid
> * Web Apps y Functions son necesarios en una red virtual.
> * Rutas de IoT Hub
> * IoT Device Explorer


> [!IMPORTANT]
> Las redes virtuales se admiten en los niveles **estándar** y **dedicado** de Event Hubs. No se admiten en el nivel básico.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita][] antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Se aprovechará un espacio de nombres de Event Hubs existente, así que asegúrese de que haya uno disponible. Si no hay ninguno, consulte [este tutorial](./event-hubs-create.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Primero, vaya a [Azure Portal][Azure portal] e inicie sesión con su suscripción de Azure.

## <a name="select-event-hubs-namespace"></a>Selección de un espacio de nombres de Event Hubs

Para este tutorial, se ha creado un espacio de nombres de Event Hubs y se accederá a él.

## <a name="navigate-to-firewalls-and-virtual-networks-experience"></a>Vaya a la experiencia de firewalls y redes virtuales

Use el menú de navegación en el panel izquierdo de Azure Portal para seleccionar la opción **"Firewalls y redes virtuales"** .

  ![Navegar al menú](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-landing-page.png)

  La primera vez que visite esta página, debe seleccionar el botón de radio **Todas las redes**. De esta forma, el espacio de nombres de Event Hubs permitirá todas las conexiones entrantes.

## <a name="add-virtual-network-service-endpoint"></a>Adición de un punto de conexión de servicio de red virtual

Para limitar el acceso, debe integrar el punto de conexión de servicio de red virtual para este espacio de nombres de Event Hubs.

1. Haga clic en el botón de radio **Redes seleccionadas** en la parte superior de la página para habilitar las opciones de menú en el resto de la página.
  ![Redes seleccionadas](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-selecting-selected-networks.png)
2. En la sección Red virtual de la página, seleccione la opción para ***+Agregar red virtual existente***. Se abrirá el panel donde podrá seleccionar una red virtual que ya haya creado.
  ![Agregar red virtual existente](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane.png)
3. Seleccione la red virtual en la lista y también la subred.
   ![Seleccionar una subred](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-with-subnet-query.png)
4. Debe habilitar el punto de conexión de servicio antes de agregar la red virtual a la lista. Si no está habilitado el punto de conexión de servicio, el portal le pedirá que lo habilite.
  ![Seleccionar subred y habilitar punto de conexión](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-after-enabling.png)
    > [!NOTE]
    > Si no puede habilitar el punto de conexión de servicio, puede ignorar el punto de conexión de servicio de red virtual que falta mediante la plantilla de ARM. Esta funcionalidad no está disponible en Azure Portal.

5. Después de habilitar el punto de conexión de servicio en la subred seleccionada, puede agregarlo a la lista de redes virtuales permitidas.
  ![Agregar subred después de habilitar el punto de conexión](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-after-adding.png)

6. Pulse el botón **Guardar** en la barra de herramientas superior para guardar la configuración de red virtual en el servicio. Espere unos minutos hasta que la confirmación se muestre en las notificaciones de Azure Portal.

## <a name="add-firewall-for-specified-ip"></a>Adición de firewall para la dirección IP especificada

Se puede restringir el acceso al espacio de nombres de Event Hubs para un intervalo limitado de direcciones IP o una dirección IP específica mediante el uso de reglas de firewall.

1. Haga clic en el botón de radio **Redes seleccionadas** en la parte superior de la página para habilitar las opciones de menú en el resto de la página.
  ![Redes seleccionadas](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-selecting-selected-networks.png)
2. En la sección **Firewall**, en la cuadrícula ***Intervalo de direcciones***, puede agregar una o muchas direcciones IP específicas o intervalos de direcciones IP.
  ![Agregar direcciones IP](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-firewall.png)
3. Una vez que haya agregado varias direcciones IP o intervalos de direcciones IP, pulse el botón **Guardar** en la barra de herramientas superior para asegurarse de que la configuración se guarda en el servicio. Espere unos minutos hasta que la confirmación se muestre en las notificaciones de Azure Portal.
  ![Agregar direcciones IP y pulsar Guardar](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-firewall-hitting-save.png)

## <a name="adding-your-current-ip-address-to-the-firewall-rules"></a>Adición de la dirección IP actual a las reglas de firewall

1. También puede agregar la dirección IP actual rápidamente; para ello, marque la casilla ***Agregar su dirección IP de cliente (SU DIRECCIÓN IP ACTUAL)*** justo encima de la cuadrícula ***Intervalo de direcciones***.
  ![Agregar dirección IP actual](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-current-ip-hitting-save.png)
2. Una vez que haya agregado la dirección IP actual a las reglas de firewall, pulse el botón **Guardar** en la barra de herramientas superior para asegurarse de que la configuración se guarda en el servicio. Espere unos minutos hasta que la confirmación se muestre en las notificaciones de Azure Portal.
  ![Agregar dirección IP actual y pulsar Guardar](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-current-ip-hitting-save-after-saving.png)

## <a name="conclusion"></a>Conclusión

En este tutorial, se han integrado puntos de conexión de redes virtuales y reglas de firewall en un espacio de nombres existente de Event Hubs. Ha aprendido a:
> [!div class="checklist"]
> * Integrar los puntos de conexión de servicio de redes virtuales con el espacio de nombres de Event Hubs.
> * Configurar el firewall (filtrado de IP) con el espacio de nombres de Event Hubs.


[Azure portal]: https://portal.azure.com/
