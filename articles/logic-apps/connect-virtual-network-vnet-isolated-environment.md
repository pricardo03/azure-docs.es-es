---
title: Conexión a redes virtuales de Azure con un ISE
description: Creación de un entorno de servicio de integración (ISE) que acceda a las redes virtuales de Azure desde Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 63174e1d4950b9f18fd3693511c507ed2dd018b3
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2020
ms.locfileid: "77500363"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Conectarse a redes virtuales de Azure desde Azure Logic Apps mediante un entorno del servicio de integración (ISE)

Para escenarios donde sus cuentas de integración y las aplicaciones lógicas necesitan tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [*entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es un entorno aislado que usa almacenamiento dedicado y otros recursos que existen de forma independiente del servicio Logic Apps multiinquilino público y "global". Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Un ISE también le proporciona sus propias direcciones IP estáticas. Estas direcciones IP son independientes de las direcciones IP estáticas que comparten las aplicaciones lógicas en el servicio multiinquilino público.

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

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Una instancia de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Si no tiene una red virtual, aprenda a [crear una](../virtual-network/quick-create-portal.md).

  * La red virtual tiene que tener cuatro subredes *vacías* para crear e implementar recursos en el ISE. Cada subred admite un componente de Logic Apps diferente que se usa en el ISE. Puede crear estas subredes por adelantado o esperar a que se cree el ISE en el que pueda crear las subredes al mismo tiempo. Obtenga más información sobre los [requisitos de subredes](#create-subnet).

  * Los nombres de subred tienen que empezar con un carácter alfabético o un carácter de subrayado, y en ellos no se pueden usar los siguientes caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`. 
  
  * Si desea implementar el entorno de servicio de integración con una plantilla de Azure Resource Manager, primero asegúrese de que delega una subred vacía a Microsoft.Logic/integrationServiceEnvironment. No es necesario realizar esta delegación al realizar la implementación mediante Azure Portal.

  * Asegúrese de que la red virtual [habilita el acceso para el ISE](#enable-access) para que su ISE funcione correctamente y permanezca accesible.

  * Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), que proporciona una conexión privada a los servicios en la nube de Microsoft que es facilitada por el proveedor de conectividad, tiene que [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y vincular esa tabla a cada subred que utilice el ISE:

    **Nombre**: <*nombre de ruta*><br>
    **Prefijo de dirección**: 0.0.0.0/0<br>
    **Próximo salto**: Internet

* Si quiere usar servidores DNS personalizados para su red virtual de Azure, [configure esos servidores siguiendo estos pasos](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implementar el ISE en su red virtual. De lo contrario, cada vez que cambie el servidor DNS, tendrá que reiniciar también el ISE.

  > [!IMPORTANT]
  > Si cambia la configuración del servidor DNS después de crear un ISE, asegúrese de reiniciar el ISE. Para más información acerca de la administración de la configuración del servidor DNS, consulte [Crear, cambiar o eliminar una red virtual](../virtual-network/manage-virtual-network.md#change-dns-servers).

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>Habilitar el acceso para el ISE

Cuando se usa un ISE con una red virtual de Azure, un problema de configuración habitual es tener uno o varios puertos bloqueados. Los conectores que usa para crear conexiones entre su ISE y los sistemas de destino también pueden tener sus propios requisitos de puerto. Por ejemplo, si se comunica con un sistema FTP mediante el conector FTP, el puerto que usa en el sistema FTP debe estar disponible, por ejemplo, el puerto 21 para enviar comandos.

Para asegurarse de que el ISE sea accesible y de que las aplicaciones lógicas del ISE puedan comunicarse a través de cada subred de la red virtual, [abra los puertos descritos en esta tabla para cada subred](#network-ports-for-ise). Si alguno de los puertos necesarios no está disponible, el ISE no funcionará correctamente.

* Si tiene varias instancias de ISE que necesitan acceso a otros puntos de conexión con restricciones de IP, implemente [Azure Firewall](../firewall/overview.md) o una [aplicación virtual de red](../virtual-network/virtual-networks-overview.md#filter-network-traffic) en la red virtual y enrute el tráfico saliente a través de ese firewall o aplicación virtual de red. Después, puede [configurar una dirección IP única, de salida, pública, estática y predecible](connect-virtual-network-vnet-set-up-single-ip-address.md) que todas las instancias de ISE de la red virtual puedan usar para comunicarse con los sistemas de destino. De ese modo, no es necesario configurar aperturas adicionales del firewall en los sistemas de destino para cada ISE.

   > [!NOTE]
   > También puede usar este enfoque para un solo ISE si el escenario requiere limitar el número de direcciones IP que necesitan acceso. Considere si los costos adicionales del firewall o el dispositivo de red virtual tienen sentido para su escenario. Obtenga más información sobre los [precios de Azure Firewall](https://azure.microsoft.com/pricing/details/azure-firewall/).

* Si ha creado una nueva red virtual de Azure y subredes sin ninguna restricción, no es necesario configurar [grupos de seguridad de red (NSG)](../virtual-network/security-overview.md#network-security-groups) en la red virtual para controlar el tráfico entre las subredes.

* En una red virtual existente, puede *opcionalmente* configurar grupos de seguridad de red [filtrando el tráfico de red a través de subredes](../virtual-network/tutorial-filter-network-traffic.md). Si desea elegir esta ruta, o si ya utiliza NSG, asegúrese de que [abre los puertos de esta tabla](#network-ports-for-ise) en la red virtual donde tenga NSG o desee configurar NSG.

  > [!NOTE]
  > Si usa [reglas de seguridad de NSG](../virtual-network/security-overview.md#security-rules), debe usar *ambos* protocolos: TCP y UDP. Las reglas de seguridad de NSG describen los puertos que debe abrir para las direcciones IP que necesitan acceder a esos puertos. Asegúrese de que todo firewall, enrutador u otro elemento que exista entre estos puntos de conexión también mantengan esos puertos accesibles para esas direcciones IP.

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>Puertos de red usados por el ISE

En esta tabla se describen los puertos de la red virtual de Azure que usa el ISE y dónde se usan. La [etiqueta de servicio de Resource Manager](../virtual-network/security-overview.md#service-tags) representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad a la hora de crear reglas de seguridad.

> [!IMPORTANT]
> Los puertos de origen son efímeros, asegúrese pues de establecerlos en `*` para todas las reglas. Cuando así se indica, el ISE interno y el ISE externo hacen referencia al [punto de conexión seleccionado al crear el ISE](connect-virtual-network-vnet-isolated-environment.md#create-environment). Para más información, consulte [Acceso al punto de conexión](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). 

| Propósito | Dirección | Puertos de destino | Etiqueta de servicio de origen | Etiqueta de servicio de destino | Notas |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| Comunicación entre subredes dentro de la red virtual | Entrada y salida | * | El espacio de direcciones para la red virtual que contiene las subredes del ISE | El espacio de direcciones para la red virtual que contiene las subredes del ISE | Necesario para que el tráfico fluya *entre* las subredes de la red virtual. <p><p>**Importante**: Para que el tráfico fluya entre los *componentes* de cada subred, asegúrese de abrir todos los puertos de cada subred. |
| Comunicación con la aplicación lógica | Entrada | 443 | ISE interno: <br>VirtualNetwork <p><p>ISE externo: <br>Internet | VirtualNetwork | La dirección IP de origen del equipo o servicio que llama a los desencadenadores de solicitud o webhooks que existan en la aplicación lógica. <p><p>**Importante**: El cierre o bloqueo de este puerto impide las llamadas HTTP a aplicaciones lógicas con desencadenadores de solicitud. |
| Historial de ejecución de la aplicación lógica | Entrada | 443 | ISE interno: <br>VirtualNetwork <p><p>ISE externo: <br>Internet | VirtualNetwork | La dirección IP de origen del equipo o servicio donde desea ver el historial de ejecución de las aplicaciones lógicas. <p><p>**Importante**: Aunque cerrar o bloquear este puerto no le impide ver el historial de ejecución, no podrá ver las entradas y salidas de cada paso en ese historial de ejecución. |
| Diseñador de Logic Apps: propiedades dinámicas | Entrada | 454 | Consulte en la columna **Notas** las direcciones IP que se permiten. | VirtualNetwork | Las solicitudes proceden de las direcciones IP [entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) del punto de conexión de acceso de Logic Apps de esa región. |
| Implementación del conector | Entrada | 454 | AzureConnectors | VirtualNetwork | Necesario para implementar y actualizar conectores. Al cerrar o bloquear este puerto, las implementaciones de ISE producen un error y no se realizan las actualizaciones o correcciones del conector. |
| Comprobación del estado de la red | Entrada | 454 | Consulte en la columna **Notas** las direcciones IP que se permiten. | VirtualNetwork | Las solicitudes proceden del punto de conexión de acceso de Logic Apps Para las direcciones IP [entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) y [salientes](../logic-apps/logic-apps-limits-and-config.md#outbound) de esa región. |
| Dependencia de administración de App Service | Entrada | 454, 455 | AppServiceManagement | VirtualNetwork | |
| Comunicación de Azure Traffic Manager | Entrada | ISE interno: 454 <p><p>ISE externo: 443 | AzureTrafficManager | VirtualNetwork | |
| API Management: punto de conexión de administración | Entrada | 3443 | APIManagement | VirtualNetwork | |
| Implementación de la directiva de conectores | Entrada | 3443 | APIManagement | VirtualNetwork | Necesario para implementar y actualizar conectores. Al cerrar o bloquear este puerto, las implementaciones de ISE producen un error y no se realizan las actualizaciones o correcciones del conector. |
| Comunicación con la aplicación lógica | Salida | 80, 443 | VirtualNetwork | Varía según el destino | Los puntos de conexión del servicio externo con el que la aplicación lógica debe comunicarse. |
| Azure Active Directory | Salida | 80, 443 | VirtualNetwork | AzureActiveDirectory | |
| Administración de conexiones | Salida | 443 | VirtualNetwork  | AppService | |
| Publicación de las métricas y registros de diagnóstico | Salida | 443 | VirtualNetwork  | AzureMonitor | |
| Dependencia de Azure Storage | Salida | 80, 443, 445 | VirtualNetwork | Storage | |
| Dependencia de Azure SQL | Salida | 1433 | VirtualNetwork | SQL | |
| Azure Resource Health | Salida | 1886 | VirtualNetwork | AzureMonitor | Necesario para publicar el estado de mantenimiento en Resource Health |
| Dependencia de la directiva de registro en el centro de eventos y agente de supervisión | Salida | 5672 | VirtualNetwork | EventHub | |
| Acceso a instancias de Azure Cache for Redis entre instancias de rol | Entrada <br>Salida | 6379 - 6383 | VirtualNetwork | VirtualNetwork | Además, para que ISE funcione con la caché de Azure para Redis, debe abrir estos [puertos de entrada y salida descritos en las Preguntas frecuentes de la caché de Azure para Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Crear el ISE

1. En [Azure Portal](https://portal.azure.com), en el cuadro de búsqueda principal de Azure, escriba `integration service environments` como filtro y seleccione **Entornos del servicio de integración**.

   ![Buscar y seleccionar "Entornos del servicio de integración"](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. En el panel **Entornos del servicio de integración**, seleccione **Agregar**.

   ![Buscar y seleccionar "Entornos del servicio de integración"](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. Proporcione estos detalles para su entorno y, después, seleccione **Revisar y crear**, por ejemplo:

   ![Proporcionar detalles del entorno](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propiedad | Obligatorio | Value | Descripción |
   |----------|----------|-------|-------------|
   | **Suscripción** | Sí | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. |
   | **Grupos de recursos** | Sí | <*nombre del grupo de recursos de Azure*> | Un grupo de recursos de Azure nuevo o existente donde quiera crear el entorno. |
   | **Nombre del entorno del servicio de integración** | Sí | <*nombre del entorno*> | El nombre de ISE, que solo puede contener letras, números, guiones (`-`), caracteres de subrayado (`_`) y puntos (`.`). |
   | **Ubicación** | Sí | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se implementará el entorno. |
   | **SKU** | Sí | **Premium** o **Desarrollador (sin SLA)** | La SKU de ISE que se va a crear y a usar. Si quiere conocer las diferencias entre estas SKU, consulte las [SKU de ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). <p><p>**Importante**: Esta opción solo está disponible durante la creación del ISE y no se puede cambiar más adelante. |
   | **Capacidad adicional** | Premium: <br>Sí <p><p>Desarrollador: <br>No aplicable | Premium: <br>De 0 a 10 <p><p>Desarrollador: <br>No aplicable | Número de unidades de procesamiento adicionales que se usarán para este recurso ISE. Para agregar capacidad después de crearla, consulte [Add ISE capacity](#add-capacity) (Agregar capacidad ISE). |
   | **Punto de conexión de acceso** | Sí | **Interno** o **externo** | Tipo de puntos de conexión de acceso que se usan para el ISE. Estos puntos de conexión determinan si los desencadenadores de solicitudes o de webhooks de las aplicaciones lógicas del ISE pueden recibir llamadas desde fuera de la red virtual o no. <p><p>La selección también afecta a la manera en que puede ver y acceder a las entradas y salidas en el historial de ejecuciones de la aplicación lógica. Para obtener más información, consulte [Acceso al punto de conexión del ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). <p><p>**Importante**: Esta opción solo está disponible durante la creación del ISE y no se puede cambiar más adelante. |
   | **Red virtual** | Sí | <*Azure-virtual-network-name*> | La red virtual de Azure donde quiere insertar su entorno para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Si no tiene una red, [cree primero una red virtual de Azure](../virtual-network/quick-create-portal.md). <p><p>**Importante**: *Solo* puede realizar esta inserción cuando se crea el ISE. |
   | **Subredes** | Sí | <*subnet-resource-list*> | Una instancia de ISE requiere cuatro subredes *vacías* para crear e implementar recursos en el entorno. Para crear cada subred, [siga los pasos descritos en esta tabla](#create-subnet). |
   |||||

   <a name="create-subnet"></a>

   **Creación de una subred**

   Para crear e implementar recursos en su entorno, el ISE necesita cuatro subredes *vacías* que no estén delegadas a ningún servicio. Cada subred admite un componente de Logic Apps diferente que se usa en el ISE. *No se pueden cambiar* estas direcciones de subred una vez creado el entorno. Cada subred tiene que cumplir estos requisitos:

   * Tiene un nombre que comienza con un carácter alfabético o un guión bajo (sin números), y no tiene estos caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`.

   * \- Usa el [formato de Enrutamiento de interdominios sin clases (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) y un espacio de direcciones de clase B.

   * Usa al menos `/27` en el espacio de direcciones, porque cada subred requiere 32 direcciones como *mínimo*. Por ejemplo:

     * `10.0.0.0/28` tiene solo 16 direcciones y es demasiado pequeño porque 2<sup>(32-28)</sup> es 2<sup>4</sup> o 16.

     * `10.0.0.0/27` tiene 32 direcciones porque 2<sup>(32-27)</sup> es 2<sup>5</sup> o 32.

     * `10.0.0.0/24` tiene 256 direcciones porque 2<sup>(32-24)</sup> es 2<sup>8</sup> o 256. Sin embargo, más direcciones no proporcionan ninguna ventaja adicional.

     Para obtener más información sobre cómo calcular las direcciones, consulte [bloques de CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), tiene que [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y vincular esa tabla con cada subred que utilice el ISE:

     **Nombre**: <*nombre de ruta*><br>
     **Prefijo de dirección**: 0.0.0.0/0<br>
     **Próximo salto**: Internet

   1. En la lista **Subredes**, seleccione **Administrar configuración de subred**.

      ![Administrar configuración de subred](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. En el panel **Subredes**, seleccione **Subred**.

      ![Agregar cuatro subredes vacías](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. En el panel **Agregar subred**, proporcione esta información.

      * **Name**: nombre de la subred.
      * **Intervalo de direcciones (bloque CIDR)** : intervalo de la subred la red virtual y en formato CIDR.

      ![Agregar detalles de la subred](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. Cuando finalice, seleccione **Aceptar**.

   1. Repita estos pasos para tres subredes más.

      > [!NOTE]
      > Si las subredes que intenta crear no son válidas, Azure Portal muestra un mensaje, pero no bloquea el progreso.

   Para obtener más información sobre la creación de subredes, consulte [Add a virtual network subnet](../virtual-network/virtual-network-manage-subnet.md) (Agregar una subred de red virtual).

1. Una vez que Azure valide correctamente la información del ISE, seleccione **Crear**, por ejemplo:

   ![Después de una validación correcta, seleccionar "Crear"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure comienza a implementar el entorno, que normalmente tarda menos de dos horas en completarse. En ocasiones, la implementación puede tardar hasta cuatro horas. Para comprobar el estado de implementación, en la barra de herramientas de Azure, seleccione el icono de notificaciones, que abre el panel de notificaciones.

   ![Comprobar el estado de implementación](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Si la implementación finaliza correctamente, Azure muestra esta notificación:

   ![Implementación correcta](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   De lo contrario, siga las instrucciones de Azure Portal para solucionar problemas de implementación.

   > [!NOTE]
   > Si se produce un error en la implementación o se elimina el ISE, Azure podría tardar hasta una hora en liberar las subredes. Debido a este retraso, es posible que deba esperar antes de volver a usar esas subredes en otro ISE.
   >
   > Si elimina su red virtual, Azure generalmente tarda hasta dos horas antes de liberar las subredes, pero esta operación puede llevar más tiempo. 
   > Cuando elimine redes virtuales, asegúrese de que no haya recursos conectados. 
   > Consulte [Eliminar red virtual](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Para ver el entorno, seleccione **Ir al recurso** si Azure no va automáticamente al entorno una vez finalizada la implementación.

1. Para comprobar el estado de la red del ISE, consulte [Administración del entorno del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md#check-network-health).

1. Para empezar a crear aplicaciones lógicas y otros artefactos en el ISE, consulte [Incorporación de artefactos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md).

   > [!IMPORTANT]
   > Los conectores ISE administrados que están disponibles después de crear el ISE no aparecen automáticamente en el selector de conectores en el Diseñador de aplicación lógica. Para poder usar estos conectores ISE, tiene que [agregarlos manualmente a su ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment) para que aparezcan en el Diseñador de aplicación lógica.

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>Agregar capacidad ISE

La unidad base del ISE Premium tiene una capacidad fija, por lo que si necesita más rendimiento, puede agregar más unidades de escalado, ya sea durante el proceso de creación o posteriormente. La SKU de Desarrollador no incluye la funcionalidad para agregar unidades de escalado.

1. En Azure Portal, busque su ISE.

1. Para revisar las métricas de uso y rendimiento del ISE, seleccione **Información general**.

   ![Ver el uso de ISE](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. En **Configuración**, seleccione **Escalar horizontalmente**. En el panel **Configurar**, seleccione una de estas opciones:

   * [**Escala manual**](#manual-scale): el escalado se hace según el número de unidades de procesamiento que desee usar.
   * [**Custom autoscale**](#custom-autoscale) (Escalabilidad automática personalizada): el escalado se hace según las métricas de rendimiento, seleccionando entre varios criterios y especificando las condiciones del umbral para cumplir con esos criterios.

   ![Seleccionar el tipo de escalado deseado](./media/connect-virtual-network-vnet-isolated-environment/select-scale-out-options.png)

<a name="manual-scale"></a>

### <a name="manual-scale"></a>Escala manual

1. Después de seleccionar **Escala manual**, en **Additional capacity** (Capacidad adicional), seleccione el número de unidades de escalado que desea usar.

   ![Seleccionar el tipo de escalado deseado](./media/connect-virtual-network-vnet-isolated-environment/select-manual-scale-out-units.png)

1. Cuando finalice, seleccione **Guardar**.

<a name="custom-autoscale"></a>

### <a name="custom-autoscale"></a>Escalabilidad automática personalizada

1. Después de seleccionar **Custom autoscale** (Escalabilidad automática personalizada) en **Autoscale setting name** (Nombre de la configuración de escalabilidad automática), proporcione un nombre para el valor seleccionado y, si lo desea, seleccione el grupo de recursos de Azure al que pertenece dicho valor.

   ![Proporcionar un nombre para la configuración de escalabilidad automática y seleccionar grupo de recursos](./media/connect-virtual-network-vnet-isolated-environment/select-custom-autoscale.png)

1. En cuanto a la condición **Predeterminada**, seleccione **Scale based on a metric** (Escalado según una métrica) o **Scale to a specific instance count** (Escalado a un recuento de instancias específico).

   * Si elige el basado en instancias, escriba el número de unidades de procesamiento, entre 0 y 10.

   * Si elige en función de las métricas, siga estos pasos:

     1. En la sección **Reglas**, seleccione **Agregar una regla**.

     1. En el panel **Scale rule** (Regla de escalado), configure los criterios y la acción que se realizará cuando se active la regla.

     1. En **Instance limits** (Límites de instancias), especifique estos valores:

        * **Mínimos**: el número mínimo de unidades de procesamiento que se va a usar.
        * **Maximum**: el número máximo de unidades de procesamiento que se va a usar.
        * **Valor predeterminado**: si ocurre algún problema al leer las métricas de los recursos y la capacidad actual es inferior a la predeterminada, el escalado automático realiza el escalado horizontal hasta el número de unidades de procesamiento predeterminado. Sin embargo, si la capacidad actual es superior a la predeterminada, el escalado automático no realiza la reducción horizontal.

1. Para agregar otra condición, seleccione **Add scale condition** (Agregar condición de escalado).

1. Cuando haya terminado la configuración de escalado automático, guarde los cambios.

## <a name="delete-ise"></a>Eliminación del ISE

Antes de eliminar un ISE que ya no necesita o un grupo de recursos de Azure que contiene un ISE, compruebe que no tiene directivas ni bloqueos en el grupo de recursos de Azure que contiene estos recursos o en la red virtual de Azure porque estos elementos pueden bloquear la eliminación.

Después de eliminar el ISE, es posible que tenga que esperar hasta 9 horas antes de intentar eliminar la red virtual o las subredes de Azure.

## <a name="next-steps"></a>Pasos siguientes

* [Incorporación de artefactos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Comprobación del estado de la red para entornos del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
