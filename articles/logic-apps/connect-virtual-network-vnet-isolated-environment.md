---
title: Conexión a redes virtuales de Azure con un ISE
description: Creación de un entorno de servicio de integración (ISE) que acceda a las redes virtuales de Azure desde Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: c87ff5384c53f53cdec4a6c0f560d2ab40e43a96
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614974"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Conectarse a redes virtuales de Azure desde Azure Logic Apps mediante un entorno del servicio de integración (ISE)

Para escenarios donde sus cuentas de integración y las aplicaciones lógicas necesitan tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [*entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es un entorno aislado y privado que usa almacenamiento especializado y otros recursos que existen de forma independiente del servicio Logic Apps multiinquilino público y "global". Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Un ISE también le proporciona sus propias direcciones IP estáticas. Estas direcciones IP son independientes de las direcciones IP estáticas que comparten las aplicaciones lógicas en el servicio multiinquilino público.

Cuando crea un ISE, Azure *inserta* ese ISE en la red virtual de Azure que, luego, implementa el servicio Logic Apps en la red virtual. Al crear aplicaciones lógicas o cuentas de integración, seleccionará el ISE como ubicación. Así, las aplicaciones lógicas y las cuentas de integración tienen acceso directo a recursos tales como las máquinas virtuales, los servidores, los sistemas y los servicios de la red virtual.

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> Para que las aplicaciones lógicas y las cuentas de integración funcionen juntas en un ISE, ambas deben usar *el mismo ISE* como ubicación.

Un ISE ha aumentado los límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes del conector personalizado. Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](logic-apps-limits-and-config.md). Para más información sobre los ISE, consulte [Acceso a recursos de Azure Virtual Network desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

En este artículo se muestra cómo completar estas tareas:

* Habilite el acceso para el ISE.
* Cree el ISE.
* Agregue capacidad adicional al ISE.

> [!IMPORTANT]
> Las aplicaciones lógicas, los desencadenadores integrados, las acciones integradas y los conectores que se ejecutan en el ISE usan un plan de tarifa diferente al plan de tarifa basado en el consumo. Para saber cómo funcionan los precios y la facturación para los ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Para ver las tarifas de precios, consulte los [precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Prerequisites

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una instancia de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Si no tiene una red virtual, aprenda a [crear una](../virtual-network/quick-create-portal.md).

  * La red virtual tiene que tener cuatro subredes *vacías* para crear e implementar recursos en el ISE. Puede crear estas subredes por adelantado o esperar a que se cree el ISE en el que pueda crear las subredes al mismo tiempo. Obtenga más información sobre los [requisitos de subredes](#create-subnet).

  * Los nombres de subred tienen que empezar con un carácter alfabético o un carácter de subrayado, y en ellos no se pueden usar los siguientes caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`. 
  
  * Si desea implementar el entorno de servicio de integración con una plantilla de Azure Resource Manager, primero asegúrese de que delega una subred vacía a Microsoft.Logic/integrationServiceEnvironment. No es necesario realizar esta delegación al realizar la implementación mediante Azure Portal.

  * Asegúrese de que la red virtual [habilita el acceso para el ISE](#enable-access) para que su ISE funcione correctamente y permanezca accesible.

  * Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), que proporciona una conexión privada a los servicios en la nube de Microsoft, tiene que [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y vincular esa tabla con cada subred que utilice el ISE:

    **Nombre**: <*nombre de ruta*><br>
    **Prefijo de dirección**: 0.0.0.0/0<br>
    **Próximo salto**: Internet

* Si quiere usar servidores DNS personalizados para su red virtual de Azure, [configure esos servidores siguiendo estos pasos](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implementar el ISE en su red virtual. De lo contrario, cada vez que cambie el servidor DNS, tendrá que reiniciar también el ISE.

  > [!IMPORTANT]
  > Si cambia la configuración del servidor DNS después de crear un ISE, asegúrese de reiniciar el ISE. Para más información acerca de la administración de la configuración del servidor DNS, consulte [Crear, cambiar o eliminar una red virtual](../virtual-network/manage-virtual-network.md#change-dns-servers).

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>Habilitar el acceso para el ISE

Cuando se usa un ISE con una red virtual de Azure, un problema de configuración habitual es tener uno o varios puertos bloqueados. Los conectores que usa para crear conexiones entre su ISE y los sistemas de destino también pueden tener sus propios requisitos de puerto. Por ejemplo, si se comunica con un sistema FTP mediante el conector FTP, el puerto que usa en el sistema FTP debe estar disponible, por ejemplo, el puerto 21 para enviar comandos.

Para asegurarse de que el ISE sea accesible y de que las aplicaciones lógicas del ISE pueden comunicarse a través de las subredes de la red virtual, [abra los puertos de esta tabla](#network-ports-for-ise). Si alguno de los puertos necesarios no está disponible, el ISE no funcionará correctamente.

* Si tiene varias instancias de ISE que necesitan acceso a otros puntos de conexión con restricciones de IP, implemente [Azure Firewall](../firewall/overview.md) o una [aplicación virtual de red](../virtual-network/virtual-networks-overview.md#filter-network-traffic) en la red virtual y enrute el tráfico saliente a través de ese firewall o aplicación virtual de red. Después, puede [configurar una dirección IP única, de salida, pública, estática y predecible](connect-virtual-network-vnet-set-up-single-ip-address.md) que todas las instancias de ISE de la red virtual puedan usar para comunicarse con los sistemas de destino. De ese modo, no es necesario configurar aperturas adicionales del firewall en los sistemas de destino para cada ISE.

   > [!NOTE]
   > También puede usar este enfoque para un solo ISE si el escenario requiere limitar el número de direcciones IP que necesitan acceso. Considere si los costos adicionales del firewall o el dispositivo de red virtual tienen sentido para su escenario. Obtenga más información sobre los [precios de Azure Firewall](https://azure.microsoft.com/pricing/details/azure-firewall/).

* Si ha creado una nueva red virtual de Azure y subredes sin ninguna restricción, no es necesario configurar [grupos de seguridad de red (NSG)](../virtual-network/security-overview.md#network-security-groups) en la red virtual para controlar el tráfico entre las subredes.

* En una red virtual existente, puede *opcionalmente* configurar grupos de seguridad de red [filtrando el tráfico de red a través de subredes](../virtual-network/tutorial-filter-network-traffic.md). Si elige esta ruta, en la red virtual en la que desea configurar los grupos de seguridad de red, asegúrese de que [abre los puertos de esta tabla](#network-ports-for-ise). Si usa las [reglas de seguridad de NSG](../virtual-network/security-overview.md#security-rules), necesita los protocolos TCP y UDP.

* Si ya tenía grupos de seguridad de red, asegúrese de [abrir los puertos de esta tabla](#network-ports-for-ise). Si usa las [reglas de seguridad de NSG](../virtual-network/security-overview.md#security-rules), necesita los protocolos TCP y UDP.

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>Puertos de red usados por el ISE

En esta tabla se describen los puertos de la red virtual de Azure que usa el ISE y dónde se usan. La [etiqueta de servicio de Resource Manager](../virtual-network/security-overview.md#service-tags) representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad a la hora de crear reglas de seguridad.

> [!IMPORTANT]
> Los puertos de origen son efímeros, asegúrese pues de establecerlos en `*` para todas las reglas.

| Propósito | Dirección | Puertos de destino | Etiqueta de servicio de origen | Etiqueta de servicio de destino | Notas |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| Comunicación entre subredes | Entrada y salida | * | - | - | **Importante**: Para la comunicación entre componentes dentro de subredes, asegúrese de que abre todos los puertos dentro de esas subredes. |
| Comunicación entre subredes | Entrada y salida | 80, 443 | VirtualNetwork | VirtualNetwork | Para la comunicación entre subredes |
| Comunicación desde Azure Logic Apps | Salida | 80, 443 | VirtualNetwork | Internet | El puerto depende del servicio externo con el que se comunica el servicio Logic Apps. |
| Azure Active Directory | Salida | 80, 443 | VirtualNetwork | AzureActiveDirectory | |
| Dependencia de Azure Storage | Salida | 80, 443 | VirtualNetwork | Storage | |
| Comunicación con Azure Logic Apps | Entrada | 443 | Puntos de conexión de acceso interno: <br>VirtualNetwork <p><p>Puntos de conexión de acceso externo: <br>Internet <p><p>**Nota**: Estos puntos de conexión hacen referencia a la configuración del punto de conexión que se [seleccionó en la creación del ISE](connect-virtual-network-vnet-isolated-environment.md#create-environment). Para más información, consulte [Acceso al punto de conexión](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). | VirtualNetwork | La dirección IP del equipo o el servicio que llama a cualquier desencadenador de solicitud o webhook que existe en su aplicación lógica. El cierre o bloqueo de este puerto evita las llamadas HTTP a aplicaciones lógicas con desencadenadores de solicitud. |
| Historial de ejecución de la aplicación lógica | Entrada | 443 | Puntos de conexión de acceso interno: <br>VirtualNetwork <p><p>Puntos de conexión de acceso externo: <br>Internet <p><p>**Nota**: Estos puntos de conexión hacen referencia a la configuración del punto de conexión que se [seleccionó en la creación del ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#create-environment). Para más información, consulte [Acceso al punto de conexión](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). | VirtualNetwork | La dirección IP del equipo desde la que ve el historial de ejecución de las aplicaciones lógicas. Aunque cerrar o bloquear este puerto no le impide ver el historial de ejecución, no podrá ver las entradas y salidas de cada paso en ese historial de ejecución. |
| Administración de conexiones | Salida | 443 | VirtualNetwork  | AppService | |
| Publicación de las métricas y registros de diagnóstico | Salida | 443 | VirtualNetwork  | AzureMonitor | |
| Comunicación de Azure Traffic Manager | Entrada | 443 | AzureTrafficManager | VirtualNetwork | |
| Diseñador de Logic Apps: propiedades dinámicas | Entrada | 454 | Consulte las direcciones IP que se permiten en la columna Notas. | VirtualNetwork | Las solicitudes proceden de las direcciones IP [entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) del punto de conexión de acceso de Logic Apps de esa región. |
| Comprobación del estado de la red | Entrada | 454 | Consulte las direcciones IP que se permiten en la columna Notas. | VirtualNetwork | Las solicitudes proceden del punto de conexión de acceso de Logic Apps Para las direcciones IP [entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) y [salientes](../logic-apps/logic-apps-limits-and-config.md#outbound) de esa región. |
| Dependencia de administración de App Service | Entrada | 454, 455 | AppServiceManagement | VirtualNetwork | |
| Implementación del conector | Entrada | 454 | AzureConnectors | VirtualNetwork | Necesario para implementar y actualizar conectores. Al cerrar o bloquear este puerto, las implementaciones de ISE producen un error y no se realizan las actualizaciones o correcciones del conector. |
| Implementación de la directiva de conectores | Entrada | 3443 | Internet | VirtualNetwork | Necesario para implementar y actualizar conectores. Al cerrar o bloquear este puerto, las implementaciones de ISE producen un error y no se realizan las actualizaciones o correcciones del conector. |
| Dependencia de Azure SQL | Salida | 1433 | VirtualNetwork | SQL | |
| Azure Resource Health | Salida | 1886 | VirtualNetwork | AzureMonitor | Se necesita para publicar el estado de mantenimiento en Resource Health |
| API Management: punto de conexión de administración | Entrada | 3443 | APIManagement | VirtualNetwork | |
| Dependencia de la directiva de registro en el centro de eventos y agente de supervisión | Salida | 5672 | VirtualNetwork | EventHub | |
| Acceso a instancias de Azure Cache for Redis entre instancias de rol | Entrada <br>Salida | 6379-6383 | VirtualNetwork | VirtualNetwork | Además, para que ISE funcione con la caché de Azure para Redis, debe abrir estos [puertos de entrada y salida descritos en las Preguntas frecuentes de la caché de Azure para Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Entrada | * | AzureLoadBalancer | VirtualNetwork | |
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

   | Propiedad | Obligatorio | Value | Descripción |
   |----------|----------|-------|-------------|
   | **Suscripción** | Sí | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. |
   | **Grupos de recursos** | Sí | <*nombre del grupo de recursos de Azure*> | El grupo de recursos de Azure donde quiere crear el entorno. |
   | **Nombre del entorno del servicio de integración** | Sí | <*nombre del entorno*> | El nombre de ISE, que solo puede contener letras, números, guiones (`-`), caracteres de subrayado (`_`) y puntos (`.`). |
   | **Ubicación** | Sí | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se implementará el entorno. |
   | **SKU** | Sí | **Premium** o **Desarrollador (sin SLA)** | La SKU de ISE que se va a crear y a usar. Si quiere conocer las diferencias entre estas SKU, consulte las [SKU de ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). <p><p>**Importante**: Esta opción solo está disponible durante la creación del ISE y no se puede cambiar más adelante. |
   | **Capacidad adicional** | Premium: <br>Sí <p><p>Desarrollador: <br>No aplicable | Premium: <br>De 0 a 10 <p><p>Desarrollador: <br>No aplicable | Número de unidades de procesamiento adicionales que se usarán para este recurso ISE. Para agregar capacidad después de crearla, consulte [Add ISE capacity](#add-capacity) (Agregar capacidad ISE). |
   | **Punto de conexión de acceso** | Sí | **Interno** o **externo** | Tipo de puntos de conexión de acceso que se usan para el ISE. Estos puntos de conexión determinan si los desencadenadores de solicitudes o de webhooks de las aplicaciones lógicas del ISE pueden recibir llamadas desde fuera de la red virtual o no. <p><p>La selección también afecta a la manera en que puede ver y acceder a las entradas y salidas en el historial de ejecuciones de la aplicación lógica. Para obtener más información, consulte [Acceso al punto de conexión del ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). <p><p>**Importante**: Esta opción solo está disponible durante la creación del ISE y no se puede cambiar más adelante. |
   | **Red virtual** | Sí | <*Azure-virtual-network-name*> | La red virtual de Azure donde quiere insertar su entorno para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Si no tiene una red, [cree primero una red virtual de Azure](../virtual-network/quick-create-portal.md). <p>**Importante**: *Solo* puede realizar esta inserción cuando se crea el ISE. |
   | **Subredes** | Sí | <*subnet-resource-list*> | Una instancia de ISE requiere cuatro subredes *vacías* para crear e implementar recursos en el entorno. Para crear cada subred, [siga los pasos descritos en esta tabla](#create-subnet). |
   |||||

   <a name="create-subnet"></a>

   **Creación de una subred**

   Para crear e implementar recursos en su entorno, el ISE necesita cuatro subredes *vacías* que no estén delegadas a ningún servicio. *No se pueden cambiar* estas direcciones de subred una vez creado el entorno.

   > [!IMPORTANT]
   > 
   > Los nombres de subred tienen que empezar con un carácter alfabético o un carácter de subrayado (sin números), y en ellos no se pueden usar los siguientes caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`.

   Además, cada subred tiene que cumplir estos requisitos:

   * \- Usa el [formato de Enrutamiento de interdominios sin clases (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) y un espacio de direcciones de clase B.

   * Usa al menos un `/27` en el espacio de direcciones, porque cada subred debe tener *al menos* 32 direcciones como *mínimo*. Por ejemplo:

     * `10.0.0.0/27` tiene 32 direcciones porque 2<sup>(32-27)</sup> es 2<sup>5</sup> o 32.

     * `10.0.0.0/24` tiene 256 direcciones porque 2<sup>(32-24)</sup> es 2<sup>8</sup> o 256.

     * `10.0.0.0/28` tiene solo 16 direcciones y es demasiado pequeño porque 2<sup>(32-28)</sup> es 2<sup>4</sup> o 16.

     Para obtener más información sobre cómo calcular las direcciones, consulte [bloques de CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), tiene que [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y vincular esa tabla con cada subred que utilice el ISE:

     **Nombre**: <*nombre de ruta*><br>
     **Prefijo de dirección**: 0.0.0.0/0<br>
     **Próximo salto**: Internet

   1. En la lista **Subredes**, elija **Administrar configuración de subred**.

      ![Administrar configuración de subred](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. En el panel **Subredes**, elija **Subred**.

      ![Subred agregada](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. En el panel **Agregar subred**, proporcione esta información.

      * **Name**: nombre de la subred.
      * **Intervalo de direcciones (bloque CIDR)** : intervalo de la subred la red virtual y en formato CIDR.

      ![Agregar detalles de la subred](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Cuando termine, elija **Aceptar**.

   1. Repita estos pasos para tres subredes más.

      > [!NOTE]
      > Si las subredes que intenta crear no son válidas, Azure Portal muestra un mensaje, pero no bloquea el progreso.

   Para obtener más información sobre la creación de subredes, consulte [Add a virtual network subnet](../virtual-network/virtual-network-manage-subnet.md) (Agregar una subred de red virtual).

1. Una vez que Azure valida correctamente la información de ISE, elija **Crear**, por ejemplo:

   ![Después de una validación correcta, elija "Crear"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure comienza a implementar el entorno, que normalmente tarda menos de dos horas en completarse. En ocasiones, la implementación puede tardar hasta cuatro horas. Para comprobar el estado de implementación, en la barra de herramientas de Azure, elija el icono de notificaciones, que abre el panel de notificaciones.

   ![Comprobar el estado de implementación](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Si la implementación finaliza correctamente, Azure muestra esta notificación:

   ![Implementación correcta](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   De lo contrario, siga las instrucciones de Azure Portal para solucionar problemas de implementación.

   > [!NOTE]
   > Si se produce un error en la implementación o se elimina el ISE, Azure podría tardar hasta una hora en liberar las subredes. Debido a este retraso, es posible que deba esperar antes de volver a usar esas subredes en otro ISE.
   >
   > Si elimina su red virtual, Azure generalmente tarda hasta dos horas antes de liberar las subredes, pero esta operación puede llevar más tiempo. 
   > Cuando elimine redes virtuales, asegúrese de que no haya recursos conectados. 
   > Consulte [Eliminar red virtual](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Para ver su entorno, elija **Ir al recurso** si Azure no va automáticamente a su entorno una vez finalizada la implementación.

1. Para comprobar el estado de la red del ISE, consulte [Administración del entorno del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md#check-network-health).

1. Para empezar a crear aplicaciones lógicas y otros artefactos en el ISE, consulte [Incorporación de artefactos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md).

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>Agregar capacidad ISE

La unidad base del ISE Premium tiene una capacidad fija, por lo que si necesita más rendimiento, puede agregar más unidades de escalado, ya sea durante el proceso de creación o posteriormente. Puede realizar un escalado automático en función de las métricas de rendimiento o en función de varias unidades de procesamiento adicionales. Si va a realizar el escalado automático basado en las métricas, puede elegir entre varios criterios y especificar las condiciones del umbral para cumplir con esos criterios. La SKU de Desarrollador no incluye la funcionalidad para agregar unidades de escalado.

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

## <a name="delete-ise"></a>Eliminación del ISE

Antes de eliminar un ISE que ya no necesita o un grupo de recursos de Azure que contiene un ISE, compruebe que no tiene directivas ni bloqueos en el grupo de recursos de Azure que contiene estos recursos o en la red virtual de Azure porque estos elementos pueden bloquear la eliminación.

Después de eliminar el ISE, es posible que tenga que esperar hasta 9 horas antes de intentar eliminar la red virtual o las subredes de Azure.

## <a name="next-steps"></a>Pasos siguientes

* [Incorporación de artefactos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Comprobación del estado de la red para entornos del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
