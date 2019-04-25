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
ms.date: 03/12/2019
ms.openlocfilehash: 8cbc02f80244b02b397162309fa5ae047f3f460a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60511309"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Conectarse a redes virtuales de Azure desde Azure Logic Apps mediante un entorno del servicio de integración (ISE)

> [!NOTE]
> Esta funcionalidad está en [ *versión preliminar pública*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para escenarios donde sus cuentas de integración y las aplicaciones lógicas necesitan tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [*entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es un entorno aislado y privado que usa almacenamiento dedicado y otros recursos que se mantienen separadas del servicio de Logic Apps "global" o pública. Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Puede *insertar* este ISE en su instancia de Azure Virtual Network, que luego implementa el servicio Logic Apps en la red virtual. Al crear aplicaciones lógicas o cuentas de integración, seleccionará este ISE como ubicación. Así, las aplicaciones lógicas y las cuentas de integración tienen acceso directo a recursos tales como las máquinas virtuales, los servidores, los sistemas y los servicios de la red virtual.

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

En este artículo se muestra cómo completar estas tareas:

* Configurar los puertos en la red virtual de Azure para que el tráfico pueda transcurrir a través de su entorno de servicio de integración (ISE) entre las subredes de la red virtual.

* Crear el entorno de servicio de integración (ISE)

* Crear una aplicación lógica que se pueda ejecutar en el ISE

* Crear una cuenta de integración para las aplicaciones lógicas en el ISE

Para más información sobre los entornos de servicio de integración, consulte [Acceso a los recursos de Azure Virtual Network desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

  > [!IMPORTANT]
  > Las aplicaciones lógicas, las acciones integradas y los conectores que se ejecutan en la instancia de ISE utilizan un plan de tarifa diferente, no uno basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

* Una instancia de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Si no tiene una red virtual, aprenda a [crear una](../virtual-network/quick-create-portal.md). 

  * La red virtual debe tener cuatro *vacía* subredes para la implementación y creación de recursos en el ISE. Puede crear de antemano estas subredes, o puede esperar hasta que se cree el ISE donde puede crear subredes al mismo tiempo. Obtenga más información sobre [los requisitos de la subred](#create-subnet). 
  
    > [!NOTE]
    > Si usas [ExpressRoute](../expressroute/expressroute-introduction.md), que proporciona una conexión privada con los servicios de nube de Microsoft, debe [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tiene las siguientes enrutar y vincular esa tabla con cada subred usada por el ISE:
    > 
    > **Nombre**: <*nombre de ruta*><br>
    > **Prefijo de dirección**: 0.0.0.0/0<br>
    > **Próximo salto**: Internet

  * Asegúrese de que la red virtual [pone a disposición estos puertos](#ports) por lo que el ISE funciona correctamente y permanece accesible.

* Si desea usar servidores DNS personalizados para su red virtual de Azure, [configurar esos servidores, siga estos pasos](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implementar el ISE en la red virtual. En caso contrario, cada vez que cambie el servidor DNS, también tendrá que reiniciar el ISE, que es una funcionalidad que está disponible con la versión preliminar pública del ISE.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Configuración de los puertos de red

Para que funcione correctamente y permanezca accesible, el entorno del servicio de integración (ISE) debe tener puertos específicos disponibles en la red virtual. En caso contrario, si alguno de estos puertos no está disponible, puede perder el acceso al ISE, el cual podría dejar de funcionar. Cuando se usa un ISE en una red virtual, un problema de configuración habitual es tener uno o varios puertos bloqueados. Para las conexiones entre el ISE y el sistema de destino, el conector que utilice también podría tener sus propios requisitos de puertos. Por ejemplo, si se comunica con un sistema FTP mediante el conector FTP, asegúrese de que el puerto que usa en ese sistema FTP como, por ejemplo, el puerto 21 para enviar comandos, está disponible.

Para controlar el tráfico entre subredes de la red virtual en el que implementar el ISE, puede configurar [grupos de seguridad de red](../virtual-network/security-overview.md) para estas subredes por [filtrado del tráfico de red entre subredes](../virtual-network/tutorial-filter-network-traffic.md). En estas tablas se describen los puertos de la red virtual que usa el ISE y dónde se usan. El [etiquetas de servicio de administrador de recursos](../virtual-network/security-overview.md#service-tags) representa un grupo de prefijos de direcciones IP que ayudan a minimizar la complejidad al crear reglas de seguridad.

> [!IMPORTANT]
> Para la comunicación interna dentro de las subredes, ISE requiere que abra todos los puertos dentro de esas subredes.

| Propósito | Dirección | Puertos | Etiqueta de servicio de origen | Etiqueta de servicio de destino | Notas |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Comunicación desde Azure Logic Apps | Salida | 80 y 443 | VirtualNetwork | Internet | El puerto depende el servicio externo con el que se comunica el servicio Logic Apps |
| Azure Active Directory | Salida | 80 y 443 | VirtualNetwork | AzureActiveDirectory | |
| Dependencia de Azure Storage | Salida | 80 y 443 | VirtualNetwork | Almacenamiento | |
| Comunicación intersubnet | Entrada y salida | 80 y 443 | VirtualNetwork | VirtualNetwork | Para la comunicación entre subredes |
| Comunicación con Azure Logic Apps | Entrada | 443 | Internet  | VirtualNetwork | La dirección IP para el equipo o servicio que llama a cualquier desencadenador de solicitud o un webhook que existe en la aplicación lógica. Cerrar o bloqueando este puerto evita que las llamadas HTTP a logic apps con desencadenadores de solicitud.  |
| Historial de ejecución de aplicación lógica | Entrada | 443 | Internet  | VirtualNetwork | Historial de ejecución de la dirección IP del equipo en el que ve la aplicación lógica. Aunque cierre o bloqueando este puerto no le impide ver el historial de ejecución, no podrá ver las entradas y salidas de cada paso en el el historial de ejecución. |
| Administración de conexiones | Salida | 443 | VirtualNetwork  | Internet | |
| Publicación de las métricas y registros de diagnóstico | Salida | 443 | VirtualNetwork  | AzureMonitor | |
| Comunicación de Azure Traffic Manager | Entrada | 443 | AzureTrafficManager | VirtualNetwork | |
| Diseñador de Logic Apps: propiedades dinámicas | Entrada | 454 | Internet  | VirtualNetwork | Las solicitudes proceden de las aplicaciones lógicas [tener acceso a punto de conexión entrante a las direcciones IP en esa región](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Dependencia de administración de App Service | Entrada | 454 y 455 | AppServiceManagement | VirtualNetwork | |
| Implementación del conector | Entrada | 454 & 3443 | Internet  | VirtualNetwork | Es necesario para implementar y actualizar los conectores. Cerrar o bloqueando este puerto hace que las implementaciones de ISE producir un error e impide que las actualizaciones de conector o correcciones. |
| Dependencia de SQL Azure | Salida | 1433 | VirtualNetwork | SQL |
| Azure Resource Health | Salida | 1886 | VirtualNetwork | Internet | Para publicar el estado de mantenimiento en Resource Health |
| API Management: punto de conexión de administración | Entrada | 3443 | APIManagement  | VirtualNetwork | |
| Dependencia de la directiva de registro en el centro de eventos y agente de supervisión | Salida | 5672 | VirtualNetwork  | EventHub | |
| Acceso a instancias de Azure Cache for Redis entre instancias de rol | Entrada <br>Salida | 6379-6383 | VirtualNetwork  | VirtualNetwork | Además, ISE trabajar con caché de Azure para Redis, debe abrir estos [puertos de entrada y salidos se describe en la memoria caché de Azure para Redis preguntas más frecuentes sobre](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Entrada | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

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

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Suscripción** | Sí | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. |
   | **Grupos de recursos** | Sí | <*nombre del grupo de recursos de Azure*> | El grupo de recursos de Azure donde quiere crear el entorno. |
   | **Nombre del Entorno del servicio de integración** | Sí | <*nombre del entorno*> | El nombre de su entorno. |
   | **Ubicación** | Sí | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se implementará el entorno. |
   | **Capacidad adicional** | Sí | 0, 1, 2, 3 | El número de unidades de procesamiento que se utilizará para este recurso ISE. Para agregar capacidad después de crearla, consulte [agregar capacidad](#add-capacity). |
   | **Red virtual** | Sí | <*Azure-virtual-network-name*> | La red virtual de Azure donde quiere insertar su entorno para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Si no dispone de una red, debe crear una aquí. <p>**Importante**: *Solo* puede realizar esta inserción cuando se crea el ISE. Sin embargo, antes de poder crear esta relación, asegúrese de que ya ha configurado el control de acceso basado en roles en la red virtual para Azure Logic Apps. |
   | **Subredes** | Sí | <*subnet-resource-list*> | Una instancia de ISE necesita cuatro subredes *vacías* para la creación de recursos en el entorno. Para crear cada subred, [siga los pasos descritos en esta tabla](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Creación de una subred**

   Para crear recursos en su entorno, necesita el ISE cuatro *vacía* subredes que no se delegan a cualquier servicio. 
   Le *no* cambiar estas direcciones de subred después de crear el entorno. Cada subred debe cumplir estos criterios:

   * Tiene un nombre que comienza con un carácter alfabético o un carácter de subrayado y no tiene estos caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Usa el [formato de enrutamiento entre dominios sin clase (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) y un espacio de direcciones de clase B.

   * Utiliza al menos un `/27` en la dirección de espacio porque cada subred debe tener 32 direcciones como la *mínimo*. Por ejemplo: 

     * `10.0.0.0/27` tiene 32 direcciones porque 2<sup>(32-27)</sup> es 2<sup>5</sup> o 32.

     * `10.0.0.0/24` tiene 256 direcciones porque 2<sup>(32-24)</sup> es 2<sup>8</sup> o 256.

     * `10.0.0.0/28` tiene solo 16 direcciones y es demasiado pequeño porque 2<sup>(32-28)</sup> es 2<sup>4</sup> o 16.

     Para más información sobre cómo calcular las direcciones, consulte [bloques CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si usas [ExpressRoute](../expressroute/expressroute-introduction.md), no olvide [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tiene las siguientes enrutar y vincular esa tabla con cada subred usada por el ISE:

     **Nombre**: <*nombre de ruta*><br>
     **Prefijo de dirección**: 0.0.0.0/0<br>
     **Próximo salto**: Internet

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

<a name="add-capacity"></a>

### <a name="add-capacity"></a>Ampliación de capacidad

La unidad básica de ISE ha se ha corregido la capacidad, por lo que si necesita más rendimiento, puede agregar más unidades de escalado. Puede que el escalado automático basado en métricas de rendimiento o en un número de unidades de procesamiento. Si elige el escalado automático basado en métricas, puede elegir entre varios criterios y especificar las condiciones de umbral para satisfacer ese criterio.

1. En Azure portal, busque el ISE.

1. Para ver las métricas de rendimiento para el ISE, en el menú principal del ISE, elija **Introducción**.

1. Para configurar el escalado automático, en **configuración**, seleccione **escalar horizontalmente**. En el **configurar** ficha, elija **habilitar escalado automático**.

1. En el **predeterminado** sección, elija **escalado según una métrica** o **escala a un número específico de instancias**.

1. Si elige basados en instancias, escriba el número de unidades de procesamiento entre 0 y 3, ambos inclusive. En caso contrario, para basadas en métricas, siga estos pasos:

   1. En el **predeterminado** sección, elija **agregar una regla**.

   1. En el **regla de escalado** panel, configurar los criterios y la acción que se realizará cuando se desencadena la regla.

   1. Cuando haya terminado, elija **agregar**.

1. Cuando haya terminado, no olvide guardar los cambios.

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Crear la aplicación lógica: ISE

Para crear aplicaciones lógicas que usen el entorno de servicio de integración (ISE), siga los pasos para [crear una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) pero con estas diferencias: 

* Cuando se crea la aplicación lógica, en la propiedad **Ubicación**, seleccione el ISE de la sección **Entornos de servicio de integración**, por ejemplo:

  ![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Puede usar el mismos desencadenadores integrados y acciones, como HTTP, que se ejecutan en el ISE mismo que la aplicación lógica. Los conectores con la etiqueta **ISE** también se ejecutan en el mismo ISE que la aplicación lógica. Los conectores sin la etiqueta **ISE** se ejecutan en el servicio global de Logic Apps.

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
