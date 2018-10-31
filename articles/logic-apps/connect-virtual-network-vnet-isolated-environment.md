---
title: Conectarse a redes virtuales de Azure desde Azure Logic Apps a través de un entorno de servicio de integración (ISE)
description: Cree un entorno de servicio de integración (ISE) para que las aplicaciones lógicas y las cuentas de integración tengan acceso a redes virtuales de Azure, a la vez que se mantiene privado y aislado del servicio público o "global" de Azure.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/25/2018
ms.openlocfilehash: b4e4e801c3c54b635f2f13b319257018ea544c03
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404127"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Conectarse a redes virtuales de Azure desde Azure Logic Apps a través de un entorno de servicio de integración (ISE)

> [!NOTE]
> Esta funcionalidad está en *versión preliminar privada*. Para solicitar acceso, [cree su solicitud para unirse aquí](https://aka.ms/iseprivatepreview).

En escenarios de integración donde las aplicaciones lógicas y las cuentas de integración deban tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [ *entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), que es un entorno aislado y privado que usa almacenamiento dedicado y donde otros recursos se mantienen separados del servicio Logic Apps público o *global*. Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Puede vincular este ISE a su instancia de Azure Virtual Network, que luego implementa el servicio Logic Apps en la red virtual. Al crear aplicaciones lógicas o cuentas de integración, seleccionará este ISE como ubicación. Así, las aplicaciones lógicas y las cuentas de integración tienen acceso directo a recursos tales como las máquinas virtuales, los servidores, los sistemas y los servicios de la red virtual. 

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

En este artículo se muestra cómo realizar estas tareas:

* Configurar los permisos en la red virtual de Azure para que la instancia privada de Logic Apps pueda acceder a la red virtual

* Crear el entorno de servicio de integración (ISE) 

* Crear una aplicación lógica que se pueda ejecutar en el ISE 

* Crear una cuenta de integración para las aplicaciones lógicas en el ISE

Para más información sobre los entornos de servicio de integración, consulte [Acceso a los recursos de Azure Virtual Network desde aplicaciones lógicas de Azure aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* Si no tiene una red virtual de Azure, aprenda cómo [crear una](../virtual-network/quick-create-portal.md). 

  > [!IMPORTANT]
  > Aunque no necesita una red virtual de Azure para crear su entorno, *solo* puede seleccionar una red virtual como elemento del mismo nivel de su entorno al crear ese entorno. 

* Para proporcionar a las aplicaciones lógicas acceso directo a la red virtual de Azure, [configure permisos de Control de acceso basado en rol (RBAC)](#vnet-access) para que el servicio Logic Apps tenga los permisos para acceder a la red virtual. 

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Establecer permisos de red virtual

Cuando se crea un entorno de servicio de integración, puede seleccionar una red virtual de Azure como *elemento del mismo nivel* para el entorno. Sin embargo, solo puede realizar este paso, o *emparejamiento*, al crear el entorno. Esta relación permite al servicio Logic Apps conectarse directamente a los recursos de esa red virtual y proporciona al entorno acceso a esos recursos. 

Para poder seleccionar la red virtual, debe configurar primero los permisos de Control de acceso basado en rol (RBAC) en la red virtual. Para realizar esta tarea, debe asignar roles específicos al servicio Azure Logic Apps.

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la red virtual. 

1. En el menú de la red virtual, seleccione **Control de acceso (IAM)**. 

1. En **Control de acceso**, seleccione **Asignación de roles** si aún no está seleccionado. En la barra de herramientas **Asignación de roles**, elija **Agregar**. 

   ![Agregar asignación de roles](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. En el panel **Agregar permisos**, configure cada rol de esta tabla para el servicio Azure Logic Apps. Asegúrese de elegir **Guardar** después de finalizar cada rol:

   | Rol | Asignar acceso a | Seleccionar | 
   |------|------------------|--------|
   | **Colaborador de la red** | **Usuario, grupo o aplicación de Azure AD** | Escriba **Azure Logic Apps**. Cuando aparezca la lista de miembros, seleccione el mismo valor. <p>**Sugerencia**: si no se encuentra este servicio, escriba el identificador de aplicación del servicio Logic Apps: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Colaborador de la red virtual clásica** | **Usuario, grupo o aplicación de Azure AD** | Escriba **Azure Logic Apps**. Cuando aparezca la lista de miembros, seleccione el mismo valor. <p>**Sugerencia**: si no se encuentra este servicio, escriba el identificador de aplicación del servicio Logic Apps: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

   Por ejemplo: 

   ![Adición de permisos](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

   Para obtener más información sobre los permisos de rol requeridos para el emparejamiento, consulte la [sección Permisos de Crear, cambiar o eliminar un emparejamiento de red virtual](../virtual-network/virtual-network-manage-peering.md#permissions). 

Si la red virtual está conectada mediante Azure ExpressRoute, una VPN de punto a sitio de Azure o una VPN de sitio a sitio de Azure, continúe con la siguiente sección para que pueda agregar la subred de puerta de enlace necesaria. De lo contrario, siga con la [creación del entorno](#create-environment).

<a name="add-gateway-subnet"></a>

## <a name="add-gateway-subnet-for-virtual-networks-with-expressroute-or-vpns"></a>Agregar la subred de puerta de enlace para las redes virtuales con ExpressRoute o VPN

Después de finalizar los pasos anteriores, para proporcionar a su entorno de servicio de integración (ISE) acceso a una red virtual de Azure que se ha conectado mediante [Azure ExpressRoute](../expressroute/expressroute-introduction.md), [VPN de punto a sitio de Azure](../vpn-gateway/point-to-site-about.md) o [VPN de sitio a sitio de Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), también debe agregar una [*subred de puerta de enlace*](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsub) a la red virtual:

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la red virtual. En el menú de la red virtual, seleccione **Subredes** y, luego, elija **Subred de puerta de enlace** > **Aceptar**.

   ![Agregación de subred de puerta de enlace](./media/connect-virtual-network-vnet-isolated-environment/add-gateway-subnet.png)

1. Ahora, cree una [*tabla de rutas*](../virtual-network/manage-route-table.md), que se asociará a la subred de puerta de enlace que creó anteriormente.

   1. En el menú principal de Azure, seleccione **Crear un recurso** > 
    **Redes** > **Tabla de rutas**.

      ![Creación de una tabla de rutas](./media/connect-virtual-network-vnet-isolated-environment/create-route-table.png)

   1. Proporcione información sobre la tabla de rutas, como el nombre, la suscripción de Azure que se usará, el grupo de recursos de Azure y la ubicación. Asegúrese de que la propiedad **Propagación de rutas BGP** esté establecida en **Habilitado** y, luego, elija **Crear**.

      ![Proporcionar detalles de la tabla de rutas](./media/connect-virtual-network-vnet-isolated-environment/enter-route-table-information.png)

   1. En el menú de la tabla de rutas, seleccione **Subredes** y, luego, elija **Asociar**. 

      ![Conectar la tabla de rutas a la subred](./media/connect-virtual-network-vnet-isolated-environment/associate-route-table.png)

   1. Seleccione **Red virtual** y, luego, seleccione la red virtual.
   
   1. Seleccione **Subred** y, luego, seleccione la subred de puerta de enlace creada anteriormente.

   1. Cuando termine, elija **Aceptar**.

1. Si tiene una VPN de punto a sitio, realice también estos pasos:

   1. En Azure, busque y seleccione el recurso de puerta de enlace de red virtual.

   1. En el menú de la puerta de enlace, seleccione **Configuración de punto a sitio**. 
   A continuación, elija **Descargar cliente VPN** para tener la configuración de cliente VPN más reciente.

      ![Descargar el cliente VPN más reciente](./media/connect-virtual-network-vnet-isolated-environment/download-vpn-client.png)

Ahora ya ha terminado con la configuración de una subred de puerta de enlace para redes virtuales que usan ExpressRoute, VPN de punto a sitio o VPN de sitio a sitio. Para continuar con la creación del entorno de servicio de integración, siga estos pasos.

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Crear el ISE

Para crear el entorno de servicio de integración (ISE), siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), en el menú principal de Azure, seleccione **Crear un recurso**.

   ![Crear recurso](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. En el cuadro de búsqueda, escriba "entorno de servicio de integración" como filtro.
En la lista de resultados, seleccione **Entorno de servicio de integración (versión preliminar)** y, a continuación, elija **Crear**.

   ![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Selección de "Crear"](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Proporcione estos detalles para su entorno:

   ![Proporcionar detalles del entorno](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propiedad | Obligatorio | Valor | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Nombre** | SÍ | <*nombre del entorno*> | El nombre de su entorno. | 
   | **Suscripción** | SÍ | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. | 
   | **Grupos de recursos** | SÍ | <*nombre del grupo de recursos de Azure*> | El grupo de recursos de Azure donde quiere crear el entorno. |
   | **Ubicación** | SÍ | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se almacenará la información sobre su entorno. |
   | **Red virtual de mismo nivel** | Sin  | <*nombre de la red virtual de Azure*> | La red virtual de Azure para asociar con el entorno como del *mismo nivel* para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Para poder crear esta relación, asegúrese de [configurar el control de acceso basado en rol en la red virtual para Azure Logic Apps](#vnet-access). <p>**Importante**: Aunque una red virtual no es necesaria, puede seleccionar una *solo* al crear el entorno. | 
   | **Nombre del emparejamiento** | Sí, con la red virtual seleccionada. | <*nombre de emparejamiento*> | El nombre dado a la relación del mismo nivel. | 
   | **Intervalo IP de red virtual** | Sí, con la red virtual seleccionada. | <*intervalo de direcciones IP*> | El intervalo de direcciones IP que se usará para crear recursos en el entorno. Este intervalo debe usar el [formato de enrutamiento de interdominios sin clase (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing), por ejemplo, 10.0.0.1/16 y requiere un espacio de direcciones de clase B. El intervalo no debe existir en el espacio de direcciones de la red virtual seleccionada en la propiedad **Red virtual del mismo nivel**, ni dentro de ninguna otra dirección IP privada donde esté conectada la red del mismo nivel, bien mediante emparejamiento o por medio de puertas de enlace. <p><p>**Importante**: *No se puede cambiar* este intervalo de direcciones una vez creado el entorno. |
   |||||
   
1. Cuando termine, seleccione **Crear**. 

   Azure inicia la implementación de su entorno, pero este proceso puede tardar *hasta dos horas* en finalizar. 
   Para comprobar el estado de implementación, en la barra de herramientas de Azure, elija el icono de notificaciones, que abre el panel de notificaciones.

   ![Comprobar el estado de implementación](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Cuando finaliza correctamente la implementación, Azure muestra esta notificación:

   ![Implementación correcta](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Para ver su entorno, elija **Ir al recurso** si Azure no va automáticamente a su entorno una vez finalizada la implementación.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Crear la aplicación lógica: ISE

Para crear aplicaciones lógicas que usen el entorno de servicio de integración (ISE), siga los pasos habituales para [crear una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) pero con estas diferencias y consideraciones: 

* Cuando se crea la aplicación lógica, la propiedad **Ubicación** muestra los entornos ISE debajo de **Entornos de servicio de integración** junto con las regiones disponibles. Seleccione el ISE, en lugar de una región, por ejemplo:

  ![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Puede usar los mismos elementos integrados, como el desencadenador HTTP o la acción, que se ejecutan en el mismo ISE que la aplicación lógica principal. Los conectores con la etiqueta **ISE** también se ejecutan en el mismo ISE que la aplicación lógica principal. Los conectores sin la etiqueta **ISE** se ejecutan en el servicio global de Logic Apps.

  ![Selección de los conectores de ISE](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Si configuró previamente el ISE con una red virtual de Azure como elemento del mismo nivel, las aplicaciones lógicas del ISE pueden acceder directamente a los recursos de esa red virtual. En lo que respecta a los sistemas locales de una red virtual que está vinculada a un ISE, las aplicaciones lógicas pueden acceder directamente a esos sistemas mediante cualquiera de estos elementos: 

  * Conector de ISE para ese sistema (por ejemplo, SQL Server)

  * Acción HTTP 

  * Conector personalizado

  Con respecto a los sistemas locales que no están en ninguna red virtual o no tienen conectores de ISE, todavía pueden conectarse después de [configurar y usar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Crear una cuenta de integración: ISE

Para usar una cuenta de integración con las aplicaciones lógicas en un entorno de servicio de integración (ISE), esa cuenta de integración debe usar el *mismo entorno* que las aplicaciones lógicas. Las aplicaciones lógicas de un ISE solo pueden hacer referencia a cuentas de integración del mismo ISE. 

Para crear una cuenta de integración que use un ISE, siga los pasos habituales para [crear cuentas de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), excepto para la propiedad **Ubicación**, que ahora muestra los ISE en **Entornos de servicio de integración** junto con las regiones disponibles. Seleccione el ISE, en lugar de una región, por ejemplo:

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">foro de Azure Logic Apps</a>.
* Para enviar ideas sobre características o votar sobre ellas, visite el <a href="http://aka.ms/logicapps-wish" target="_blank">sitio de comentarios de los usuarios de Logic Apps</a>.

## <a name="next-steps"></a>Pasos siguientes

* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
