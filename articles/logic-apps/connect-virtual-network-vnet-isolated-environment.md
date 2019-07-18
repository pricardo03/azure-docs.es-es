---
title: Conectarse a redes virtuales de Azure desde Azure Logic Apps a través de un entorno de servicio de integración (ISE)
description: Cree un entorno de servicio de integración (ISE) para que las aplicaciones lógicas y las cuentas de integración tengan acceso a redes virtuales de Azure (VNET), a la vez que se mantiene privado y aislado del servicio público o "global" de Azure.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: b48257cc8e10deb1ec922806f62a6c435069f66f
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467091"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Conectarse a redes virtuales de Azure desde Azure Logic Apps mediante un entorno del servicio de integración (ISE)

Para escenarios donde sus cuentas de integración y las aplicaciones lógicas necesitan tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [*entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es un entorno aislado y privado que usa almacenamiento especializado y otros recursos que existen de forma independiente del servicio Logic Apps público o "global". Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Puede *insertar* este ISE en su instancia de Azure Virtual Network, que luego implementa el servicio Logic Apps en la red virtual. Al crear aplicaciones lógicas o cuentas de integración, seleccionará este ISE como ubicación. Así, las aplicaciones lógicas y las cuentas de integración tienen acceso directo a recursos tales como las máquinas virtuales, los servidores, los sistemas y los servicios de la red virtual.

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

En este artículo se muestra cómo completar estas tareas:

* Asegúrese de que todos los puertos necesarios en una red virtual estén abiertos para que el tráfico pueda moverse a través del entorno del servicio de integración (ISE) mediante las subredes de esa red virtual.

* Crear el entorno de servicio de integración (ISE)

* Crear una aplicación lógica que se pueda ejecutar en el ISE

* Crear una cuenta de integración para las aplicaciones lógicas en el ISE

Para más información sobre los entornos de servicio de integración, consulte [Acceso a los recursos de Azure Virtual Network desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

  > [!IMPORTANT]
  > Las aplicaciones lógicas, los desencadenadores integrados, las acciones integradas y los conectores que se ejecutan en el ISE usan un plan de tarifa diferente al plan de tarifa basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

* Una instancia de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Si no tiene una red virtual, aprenda a [crear una](../virtual-network/quick-create-portal.md). 

  * Su red virtual debe tener cuatro subredes *vacías* para poder implementar y crear recursos en el ISE. Puede crear estas subredes por adelantado o esperar a que se cree el ISE en el que pueda crear las subredes al mismo tiempo. Obtenga más información sobre los [requisitos de subredes](#create-subnet). 
  
    > [!NOTE]
    > Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), que proporciona una conexión privada a los servicios en la nube de Microsoft, debe [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y vincular esa tabla con cada subred que use el ISE:
    > 
    > **Nombre**: <*nombre de ruta*><br>
    > **Prefijo de dirección**: 0.0.0.0/0<br>
    > **Próximo salto**: Internet

  * Además, debe asegurarse de que la red virtual [habilita esos puertos](#ports) y de que el ISE funciona correctamente y permanece accesible.

* Si quiere usar servidores DNS personalizados para su red virtual de Azure, [configure esos servidores siguiendo estos pasos](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implementar el ISE en su red virtual. En caso contrario, cada vez que cambie el servidor DNS, también tendrá que reiniciar el ISE, que es una funcionalidad que está disponible con la versión preliminar pública del ISE.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="check-network-ports"></a>Comprobación de los puertos de red

Cuando usa un entorno del servicio de integración (ISE) con una red virtual, un problema de configuración habitual es tener uno o más puertos bloqueados. Los conectores que usa para crear conexiones entre su ISE y el sistema de destino también pueden tener sus propios requisitos de puerto. Por ejemplo, si se comunica con un sistema FTP mediante el conector FTP, asegúrese de que el puerto que usa en ese sistema FTP como, por ejemplo, el puerto 21 para enviar comandos, está disponible.

Para controlar el tráfico a través de las subredes de la red virtual donde implementa su ISE, puede configurar de forma opcional [grupos de seguridad de red (NSG)](../virtual-network/security-overview.md) en su red virtual [si filtra el tráfico de red a través de las subredes](../virtual-network/tutorial-filter-network-traffic.md). Si elige esta opción, asegúrese de que el ISE abre los puertos adecuados, tal como se describe en la siguiente tabla, en la red virtual que usa los NSG. Si tiene NSG o firewalls existentes en su red virtual, asegúrese de que abran esos puertos. De esa manera, el ISE permanecerá accesible y podrá funcionar correctamente para que no pierda el acceso al mismo. De lo contrario, si alguno de los puertos necesarios no está disponible, el ISE dejará de funcionar.

En estas tablas se describen los puertos de la red virtual que usa el ISE y dónde se usan. La [etiqueta de servicio de Resource Manager](../virtual-network/security-overview.md#service-tags) representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad a la hora de crear reglas de seguridad.

> [!IMPORTANT]
> En cuanto a la comunicación interna dentro de las subredes, ISE requiere que se abran todos los puertos dentro de esas subredes.

| Propósito | Dirección | Puertos | Etiqueta de servicio de origen | Etiqueta de servicio de destino | Notas |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Comunicación desde Azure Logic Apps | Salida | 80 y 443 | VirtualNetwork | Internet | El puerto depende del servicio externo con el que se comunica el servicio Logic Apps. |
| Azure Active Directory | Salida | 80 y 443 | VirtualNetwork | AzureActiveDirectory | |
| Dependencia de Azure Storage | Salida | 80 y 443 | VirtualNetwork | Storage | |
| Comunicación entre subredes | Entrada y salida | 80 y 443 | VirtualNetwork | VirtualNetwork | Para la comunicación entre subredes |
| Comunicación con Azure Logic Apps | Entrada | 443 | Internet  | VirtualNetwork | La dirección IP del equipo o el servicio que llama a cualquier desencadenador de solicitud o webhook que existe en su aplicación lógica. El cierre o bloqueo de este puerto evita las llamadas HTTP a aplicaciones lógicas con desencadenadores de solicitud.  |
| Historial de ejecución de la aplicación lógica | Entrada | 443 | Internet  | VirtualNetwork | La dirección IP del equipo desde la que ve el historial de ejecución de las aplicaciones lógicas. Aunque cerrar o bloquear este puerto no le impide ver el historial de ejecución, no podrá ver las entradas y salidas de cada paso en ese historial de ejecución. |
| Administración de conexiones | Salida | 443 | VirtualNetwork  | Internet | |
| Publicación de las métricas y registros de diagnóstico | Salida | 443 | VirtualNetwork  | AzureMonitor | |
| Comunicación de Azure Traffic Manager | Entrada | 443 | AzureTrafficManager | VirtualNetwork | |
| Diseñador de Logic Apps: propiedades dinámicas | Entrada | 454 | Internet  | VirtualNetwork | Las solicitudes proceden de las aplicaciones lógicas [de las direcciones IP entrantes del punto de conexión de acceso de esa región ](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Dependencia de administración de App Service | Entrada | 454 y 455 | AppServiceManagement | VirtualNetwork | |
| Implementación del conector | Entrada | 454 y 3443 | Internet  | VirtualNetwork | Necesario para implementar y actualizar conectores. Al cerrar o bloquear este puerto, las implementaciones de ISE producen un error y no se realizan las actualizaciones o correcciones del conector. |
| Dependencia de Azure SQL | Salida | 1433 | VirtualNetwork | SQL |
| Azure Resource Health | Salida | 1886 | VirtualNetwork | Internet | Se necesita para publicar el estado de mantenimiento en Resource Health |
| API Management: punto de conexión de administración | Entrada | 3443 | APIManagement  | VirtualNetwork | |
| Dependencia de la directiva de registro en el centro de eventos y agente de supervisión | Salida | 5672 | VirtualNetwork  | EventHub | |
| Acceso a instancias de Azure Cache for Redis entre instancias de rol | Entrada <br>Salida | 6379-6383 | VirtualNetwork  | VirtualNetwork | Además, para que ISE funcione con la caché de Azure para Redis, debe abrir estos [puertos de entrada y salida descritos en las Preguntas frecuentes de la caché de Azure para Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Entrada | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Crear el ISE

Para crear el entorno de servicio de integración (ISE), siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), en el menú principal de Azure, seleccione **Crear un recurso**.
En el cuadro de búsqueda, escriba "entorno de servicio de integración" como filtro.

   ![Crear recurso](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. En el panel de creación del entorno del servicio de integración, elija **Create** (Crear).

   ![Selección de "Crear"](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Proporcione estos detalles para su entorno y, después, elija **Revisión y creación**, por ejemplo:

   ![Proporcionar detalles del entorno](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Suscripción** | Sí | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. |
   | **Grupos de recursos** | Sí | <*nombre del grupo de recursos de Azure*> | El grupo de recursos de Azure donde quiere crear el entorno. |
   | **Nombre del Entorno del servicio de integración** | Sí | <*nombre del entorno*> | El nombre de su entorno. |
   | **Ubicación** | Sí | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se implementará el entorno. |
   | **Capacidad adicional** | Sí | De 0 a 10 | Número de unidades de procesamiento adicionales que se usarán para este recurso ISE. Para agregar capacidad después de crearla, consulte [Add ISE capacity](#add-capacity) (Agregar capacidad ISE). |
   | **Red virtual** | Sí | <*Azure-virtual-network-name*> | La red virtual de Azure donde quiere insertar su entorno para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Si no tiene una red, [cree primero una red virtual de Azure](../virtual-network/quick-create-portal.md). <p>**Importante**: *Solo* puede realizar esta inserción cuando se crea el ISE. |
   | **Subredes** | Sí | <*subnet-resource-list*> | Una instancia de ISE necesita cuatro subredes *vacías* para la creación de recursos en el entorno. Para crear cada subred, [siga los pasos descritos en esta tabla](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Creación de una subred**

   Para crear recursos en su entorno, el ISE necesita cuatro subredes *vacías* que no estén delegadas a ningún servicio. 
   *No se pueden cambiar* estas direcciones de subred una vez creado el entorno. Cada subred debe cumplir estos criterios:

   * Tiene un nombre que comienza con un carácter alfabético o un guión bajo, y no tiene estos caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * \- Usa el [formato de Enrutamiento de interdominios sin clases (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) y un espacio de direcciones de clase B.

   * Usa al menos un `/27` en el espacio de direcciones porque cada subred debe tener 32 direcciones como *mínimo*. Por ejemplo:

     * `10.0.0.0/27` tiene 32 direcciones porque 2<sup>(32-27)</sup> es 2<sup>5</sup> o 32.

     * `10.0.0.0/24` tiene 256 direcciones porque 2<sup>(32-24)</sup> es 2<sup>8</sup> o 256.

     * `10.0.0.0/28` tiene solo 16 direcciones y es demasiado pequeño porque 2<sup>(32-28)</sup> es 2<sup>4</sup> o 16.

     Para obtener más información sobre cómo calcular las direcciones, consulte [bloques de CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), recuerde que debe [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y que debe vincular esa tabla con cada subred que use el ISE:

     **Nombre**: <*nombre de ruta*><br>
     **Prefijo de dirección**: 0.0.0.0/0<br>
     **Próximo salto**: Internet

   1. En la lista **Subredes**, elija **Administrar configuración de subred**.

      ![Administrar configuración de subred](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. En el panel **Subredes**, elija **Subred**.

      ![Subred agregada](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. En el panel **Agregar subred**, proporcione esta información.

      * **Nombre**: nombre de la subred.
      * **Intervalo de direcciones (bloque CIDR)** : intervalo de la subred la red virtual y en formato CIDR.

      ![Agregar detalles de la subred](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Cuando termine, elija **Aceptar**.

   1. Repita estos pasos para tres subredes más.

      > [!NOTE]
      > Si las subredes que intenta crear no son válidas, Azure Portal muestra un mensaje, pero no bloquea el progreso.

1. Una vez que Azure valida correctamente la información de ISE, elija **Crear**, por ejemplo:

   ![Después de una validación correcta, elija "Crear"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure inicia la implementación de su entorno, pero este proceso *puede* tardar hasta dos horas en finalizar. 
   Para comprobar el estado de implementación, en la barra de herramientas de Azure, elija el icono de notificaciones, que abre el panel de notificaciones.

   ![Comprobar el estado de implementación](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Si la implementación finaliza correctamente, Azure muestra esta notificación:

   ![Implementación correcta](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   De lo contrario, siga las instrucciones de Azure Portal para solucionar problemas de implementación.

   > [!NOTE]
   > Si se produce un error en la implementación o se elimina el ISE, Azure podría tardar hasta una hora en liberar las subredes. Debido a este retraso, es posible que deba esperar antes de volver a usar esas subredes en otro ISE. 
   >
   > Si elimina su red virtual, Azure generalmente tarda hasta dos horas antes de liberar las subredes, pero esta operación puede llevar más tiempo. 
   > Cuando elimine redes virtuales, asegúrese de que no haya recursos conectados. Consulte [Eliminar red virtual](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Para ver su entorno, elija **Ir al recurso** si Azure no va automáticamente a su entorno una vez finalizada la implementación.  

Para obtener más información sobre la creación de subredes, consulte [Add a virtual network subnet](../virtual-network/virtual-network-manage-subnet.md) (Agregar una subred de red virtual).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Crear la aplicación lógica: ISE

Para crear aplicaciones lógicas que se ejecuten en el entorno del servicio de integración (ISE), [cree sus aplicaciones lógicas de la manera habitual](../logic-apps/quickstart-create-first-logic-app-workflow.md), excepto cuando configure la propiedad **Ubicación**; para ello, seleccione el ISE en la sección de los **entornos del servicio de integración**, por ejemplo:

  ![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

Para conocer las diferencias de funcionamiento de los desencadenadores y las acciones y saber cómo se etiquetan cuando usa un ISE en comparación con el servicio global de Logic Apps, consulte [Isolated versus global in the ISE overview](connect-virtual-network-vnet-isolated-environment-overview.md#difference) (Información general del ISE: aislado vs. global).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Crear una cuenta de integración: ISE

Para usar una cuenta de integración con las aplicaciones lógicas en un entorno de servicio de integración (ISE), esa cuenta de integración debe usar el *mismo entorno* que las aplicaciones lógicas. Las aplicaciones lógicas de un ISE solo pueden hacer referencia a cuentas de integración del mismo ISE.

Para crear una cuenta de integración que use un ISE, [cree sus cuentas de integración de la manera habitual](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), excepto cuando configure la propiedad **Ubicación**; para ello, seleccione el ISE en la sección de los **entornos del servicio de integración**, por ejemplo:

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>Agregar capacidad ISE

Su unidad base del ISE tiene una capacidad fija, por lo que si necesita más rendimiento, puede agregar más unidades de escalado. Puede realizar un escalado automático en función de las métricas de rendimiento o en función de varias unidades de procesamiento adicionales. Si va a realizar el escalado automático basado en las métricas, puede elegir entre varios criterios y especificar las condiciones del umbral para cumplir con esos criterios.

1. En Azure Portal, busque su ISE.

1. Para revisar las métricas de uso y el rendimiento del ISE, en el menú principal del mismo seleccione **Overview** (Información general).

   ![Ver el uso de ISE](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Para configurar la escala automática, en **Settings** (Configuración), seleccione **Scale out** (Escalar horizontalmente). En la pestaña **Configure** (Configurar), elija **Enable autoscale** (Habilitar escalado automático).

   ![Activar el escalado automático](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. Para el **nombre de configuración de escalado automático**, proporcione un nombre para su configuración.

1. En la sección **Default** (Valor predeterminado), elija **Scale based on a metric** (Escala basada en una métrica) o **Scale to a specific instance count** (Escalar a un recuento de instancia específico).

   * Si elige en función de la instancia, escriba el número de unidades de procesamiento entre 0 y 10 inclusive.

   * Si elige en función de las métricas, siga estos pasos:

     1. En la sección **Rules** (Reglas), elija **Add a rule** (Agregar una regla).

     1. En el panel **Scale rule** (Regla de escalado), configure los criterios y la acción que se realizará cuando se active la regla.

     1. Cuando termine, elija **Add** (Agregar).

1. Cuando haya terminado la configuración de escalado automático, guarde los cambios.

## <a name="next-steps"></a>Pasos siguientes

* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
