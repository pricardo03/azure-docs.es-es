---
title: Conectarse a redes virtuales de Azure desde Azure Logic Apps a través de un entorno de servicio de integración (ISE)
description: Cree un entorno de servicio de integración (ISE) para que las aplicaciones lógicas y las cuentas de integración tengan acceso a redes virtuales de Azure (VNET), a la vez que se mantiene privado y aislado del servicio público o "global" de Azure.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 12/06/2018
ms.openlocfilehash: 31f3cf9bd8f83c5da32569ed370de1ed35299749
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062390"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Conectarse a redes virtuales de Azure desde Azure Logic Apps a través de un entorno de servicio de integración (ISE)

> [!NOTE]
> Esta funcionalidad está en *versión preliminar privada*. Para solicitar acceso, [cree su solicitud para unirse aquí](https://aka.ms/iseprivatepreview).

Para escenarios donde sus cuentas de integración y las aplicaciones lógicas necesitan tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [*entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es un entorno aislado y privado que usa almacenamiento especializado y otros recursos que existen de forma independiente del servicio Logic Apps público o "global". Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Puede *insertar* este ISE en su instancia de Azure Virtual Network, que luego implementa el servicio Logic Apps en la red virtual. Al crear aplicaciones lógicas o cuentas de integración, seleccionará este ISE como ubicación. Así, las aplicaciones lógicas y las cuentas de integración tienen acceso directo a recursos tales como las máquinas virtuales, los servidores, los sistemas y los servicios de la red virtual. 

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

En este artículo se muestra cómo completar estas tareas:

* Configurar los permisos en la red virtual de Azure para que la instancia privada de Logic Apps pueda acceder a la red virtual

* Crear el entorno de servicio de integración (ISE) 

* Crear una aplicación lógica que se pueda ejecutar en el ISE 

* Crear una cuenta de integración para las aplicaciones lógicas en el ISE

Para más información sobre los entornos de servicio de integración, consulte [Acceso a los recursos de Azure Virtual Network desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

  > [!IMPORTANT]
  > Las aplicaciones lógicas, las acciones integradas y los conectores que se ejecutan en la instancia de ISE utilizan un plan de tarifa diferente, no uno basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

* Una instancia de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Si no tiene una red virtual, aprenda a [crear una](../virtual-network/quick-create-portal.md). 

* Para proporcionar a las aplicaciones lógicas acceso directo a la red virtual de Azure, [configure permisos de Control de acceso basado en rol (RBAC)](#vnet-access) para que el servicio Logic Apps tenga los permisos para acceder a la red virtual. 

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Establecer permisos de red virtual

Cuando crea un entorno del servicio de integración (ISE), selecciona una red virtual de Azure donde *insertar* su entorno. Sin embargo, para poder seleccionar una red virtual para insertar su entorno, debe configurar primero los permisos de Control de acceso basado en rol (RBAC) en la red virtual. Para configurar los permisos, asigne estos roles específicos al servicio de Azure Logic Apps:

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la red virtual. 

1. En el menú de la red virtual, seleccione **Control de acceso (IAM)**. 

1. En **Control de acceso (IAM)**, elija **Add role assignment** (Agregar asignación de roles). 

   ![Agregar roles](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. En el panel **Add role assignment** (Agregar asignación de roles), agregue el rol necesario al servicio de Azure Logic Apps tal como se describe. 

   1. En **Rol**, seleccione **Colaborador de la red**. 
   
   1. En **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**.

   1. En **Seleccionar**, escriba **Azure Logic Apps**. 

   1. Cuando aparezca la lista de miembros, seleccione **Azure Logic Apps**. 

      > [!TIP]
      > Si no se encuentra este servicio, escriba el identificador de aplicación del servicio Logic Apps: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` 
   
   1. Cuando termine, seleccione **Guardar**.

   Por ejemplo: 

   ![Agregar asignación de roles](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

Para obtener más información, consulte [Permisos de acceso para la red virtual](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Crear el ISE

Para crear el entorno de servicio de integración (ISE), siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), en el menú principal de Azure, seleccione **Crear un recurso**.

   ![Crear recurso](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. En el cuadro de búsqueda, escriba "entorno de servicio de integración" como filtro.
En la lista de resultados, seleccione **Entorno de servicio de integración (versión preliminar)** y, a continuación, elija **Crear**.

   ![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Selección de "Crear"](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Proporcione estos detalles para su entorno y, después, elija **Revisión y creación**, por ejemplo:

   ![Proporcionar detalles del entorno](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propiedad | Obligatorio | Valor | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Suscripción** | SÍ | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. | 
   | **Grupos de recursos** | SÍ | <*nombre del grupo de recursos de Azure*> | El grupo de recursos de Azure donde quiere crear el entorno. |
   | **Nombre del Entorno del servicio de integración** | SÍ | <*nombre del entorno*> | El nombre de su entorno. | 
   | **Ubicación** | SÍ | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se implementará el entorno. | 
   | **Capacidad adicional** | SÍ | 0, 1, 2, 3 | Número de unidades de procesamiento que se utilizará para este recurso ISE | 
   | **Red virtual** | SÍ | <*Azure-virtual-network-name*> | La red virtual de Azure donde quiere insertar su entorno para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Si no dispone de una red, debe crear una aquí. <p>**Importante**: *Solo* puede realizar esta inserción cuando se crea el ISE. Sin embargo, para poder crear esta relación, asegúrese de [configurar el control de acceso basado en roles en la red virtual para Azure Logic Apps](#vnet-access). | 
   | **Subredes** | SÍ | <*subnet-resource-list*> | Una instancia de ISE necesita cuatro subredes *vacías* para la creación de recursos en el entorno. Por lo tanto, asegúrese de que estas subredes *no se delegan* a cualquier servicio. *No se pueden cambiar* estas direcciones de subred una vez creado el entorno. <p><p>Para crear cada subred, [siga los pasos descritos en esta tabla](#create-subnet). Cada subred debe cumplir estos criterios: <p>- No puede estar vacío. <br>- Usar un nombre que no comience con un número ni con un guión. <br>- Usar el [formato Enrutamiento de interdominios sin clases (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) y un espacio de direcciones de clase B. <br>Incluir al menos un `/27` en el espacio de direcciones de manera que la subred obtenga al menos 32 de ellas. Para más información sobre cómo calcular el número de direcciones, consulte los [bloques IPv4 CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks). Por ejemplo:  <p>- `10.0.0.0/24` tiene 256 direcciones porque 2<sup>(32-24)</sup> es 2<sup>8</sup> o 256. <br>- `10.0.0.0/27` tiene 32 direcciones porque 2<sup>(32-27)</sup> es 2<sup>5</sup> o 32. <br>- `10.0.0.0/28` tiene solo 16 direcciones porque 2<sup>(32-28)</sup> es 2<sup>4</sup> o 16. |
   |||||

   <a name="create-subnet"></a>

   **Creación de una subred**

   1. En la lista **Subredes**, elija **Administrar configuración de subred**.

      ![Administrar configuración de subred](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. En el panel **Subredes**, elija **Subred**.

      ![Subred agregada](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. En el panel **Agregar subred**, proporcione esta información.

      * **Nombre**: nombre de la subred.
      * **Intervalo de direcciones (bloque CIDR)**: intervalo de la subred la red virtual y en formato CIDR.

      ![Agregar detalles de la subred](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Cuando termine, elija **Aceptar**.

   1. Repita estos pasos para tres subredes más.

1. Una vez que Azure valida correctamente la información de ISE, elija **Crear**, por ejemplo:

   ![Después de una validación correcta, elija "Crear"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure inicia la implementación de su entorno, pero este proceso *puede* tardar hasta dos horas en finalizar. 
   Para comprobar el estado de implementación, en la barra de herramientas de Azure, elija el icono de notificaciones, que abre el panel de notificaciones.

   ![Comprobar el estado de implementación](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Si la implementación finaliza correctamente, Azure muestra esta notificación:

   ![Implementación correcta](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Si se produce un error en la implementación o se elimina el ISE, Azure *podría* tardar hasta una hora en liberar las subredes. Por lo tanto, es posible que deba esperar antes de volver a usar esas subredes en otro ISE.

1. Para ver su entorno, elija **Ir al recurso** si Azure no va automáticamente a su entorno una vez finalizada la implementación.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Crear la aplicación lógica: ISE

Para crear aplicaciones lógicas que usen el entorno de servicio de integración (ISE), siga los pasos para [crear una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) pero con estas diferencias: 

* Cuando se crea la aplicación lógica, en la propiedad **Ubicación**, seleccione el ISE de la sección **Entornos de servicio de integración**, por ejemplo:

  ![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Puede usar las mismas acciones y desencadenadores integrados, como el desencadenador HTTP, que se ejecutan en el mismo ISE que la aplicación lógica. Los conectores con la etiqueta **ISE** también se ejecutan en el mismo ISE que la aplicación lógica. Los conectores sin la etiqueta **ISE** se ejecutan en el servicio global de Logic Apps.

  ![Selección de los conectores de ISE](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Después de insertar el ISE en una red virtual de Azure, las aplicaciones lógicas del ISE pueden acceder directamente a los recursos de esa red virtual. En lo que respecta a los sistemas locales conectados a una red virtual, inserte un ISE en esa red para que las aplicaciones lógicas puedan acceder directamente a esos sistemas mediante cualquiera de estos elementos: 

  * Conector de ISE para ese sistema (por ejemplo, SQL Server)
  
  * Acción HTTP 
  
  * Conector personalizado

  Con respecto a los sistemas locales que no están en ninguna red virtual o no tienen conectores de ISE, primero debe [configurar y usar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Crear una cuenta de integración: ISE

Para usar una cuenta de integración con las aplicaciones lógicas en un entorno de servicio de integración (ISE), esa cuenta de integración debe usar el *mismo entorno* que las aplicaciones lógicas. Las aplicaciones lógicas de un ISE solo pueden hacer referencia a cuentas de integración del mismo ISE. 

Para crear una cuenta de integración que use un ISE, siga los pasos para [crear cuentas de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), excepto para la propiedad **Ubicación**, donde se muestra ahora la sección **Entornos de servicio de integración**. En su lugar, seleccione el ISE, en lugar de una región, por ejemplo:

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">foro de Azure Logic Apps</a>.
* Para enviar ideas sobre características o votar sobre ellas, visite el <a href="https://aka.ms/logicapps-wish" target="_blank">sitio de comentarios de los usuarios de Logic Apps</a>.

## <a name="next-steps"></a>Pasos siguientes

* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
