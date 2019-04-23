---
title: Implementación de Azure Databricks en la red virtual (versión preliminar)
description: En este artículo se describe cómo implementar Azure Databricks a la red virtual, también conocida como inserción de red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: 2db588a0cf67d7826408139e8facb43a2e897951
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003452"
---
# <a name="deploy-azure-databricks-in-your-virtual-network-preview"></a>Implementación de Azure Databricks en la red virtual (versión preliminar)

La implementación predeterminada de Azure Databricks es un servicio totalmente administrado en Azure: todos los recursos del plano de datos, incluida una red virtual (VNet), se implementan en un grupo de recursos bloqueados. Si necesita personalizar de red, sin embargo, puede implementar los recursos de Azure Databricks en su propia red virtual (también llamado inyección de red virtual), cuando le permite:

* Conectar Azure Databricks con otros servicios de Azure (por ejemplo, el almacenamiento de Azure) de forma más segura con los puntos de conexión de servicio.
* Conectarse a datos en el entorno local orígenes para su uso con Azure Databricks, que aprovecha las rutas definidas por el usuario.
* Conectar Azure Databricks a un dispositivo de red virtual para inspeccionar todo el tráfico saliente y realizar acciones según permitir y denegar las reglas.
* Configurar Azure Databricks para usar DNS personalizado.
* Configurar reglas de grupo (NSG) de seguridad de red para especificar las restricciones de tráfico de salida.
* Implementar clústeres de Azure Databricks en la red virtual existente.

Implementar recursos de Azure Databricks en su propia red virtual también le permite aprovechar las ventajas de los intervalos CIDR flexibles (en cualquier lugar entre /16-/ 24 para la red virtual y entre /18-/ 26 para las subredes).

  > [!NOTE]
  > No se puede reemplazar la red virtual para un área de trabajo. Si el área de trabajo actual no puede contener al número necesario de nodos de clúster activo, cree otra área de trabajo en una red virtual más grande. Siga [estos pasos de migración de detallados](howto-regional-disaster-recovery.md#detailed-migration-steps) para copiar recursos (blocs de notas, las configuraciones de clúster, los trabajos) de la antigua a la nueva área de trabajo.

## <a name="virtual-network-requirements"></a>Requisitos de red virtual

Puede usar la interfaz de implementación del área de trabajo de Azure Databricks en Azure portal para configurar automáticamente una red virtual existente con las subredes necesarias, el grupo de seguridad de red y configuración de creación de listas blancas, o puede usar Azure Resource Manager plantillas para configurar la red virtual e implementar el área de trabajo.

La red virtual que se implementa en el área de trabajo Azure Databricks debe cumplir los siguientes requisitos:

### <a name="location"></a>Location

La red virtual debe residir en la misma ubicación que el área de trabajo de Azure Databricks.

### <a name="subnets"></a>Subredes

La red virtual debe incluir dos subredes dedicadas a Azure Databricks:

   1. Una subred privada con un grupo de seguridad de red preconfiguradas que permite la comunicación interna del clúster

   2. Una subred pública con un grupo de seguridad de red preconfiguradas que permite la comunicación con el plano de control de Azure Databricks.

### <a name="address-space"></a>Espacio de direcciones

Un bloque CIDR entre /16-/ 24 para la red virtual y un bloque CIDR entre /18-/26 para las subredes públicas y privadas.

### <a name="whitelisting"></a>Lista blanca

Todo el tráfico entrante y saliente entre las subredes y el plano de control de Azure Databricks debe estar en la lista blanca.

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

En esta sección se describe cómo crear un área de trabajo de Azure Databricks en Azure portal e implementarlo en su propia red virtual existente. Azure Databricks actualiza la red virtual con dos nuevas subredes y grupos de seguridad de red mediante intervalos CIDR proporcionados por el usuario, las listas blancas de entrada y el tráfico de subred de salida e implementa el área de trabajo a la red virtual actualizada.

## <a name="prerequisites"></a>Requisitos previos

Debe tener una red virtual a la que se implementará el área de trabajo de Azure Databricks. Puede usar una red virtual existente o crear uno nuevo, pero la red virtual debe estar en la misma región que el área de trabajo de Azure Databricks que va a crear. Un intervalo CIDR entre /16 /24 es necesario para la red virtual.

  > [!Warning]
  > Un área de trabajo con una red virtual más pequeña: es decir, un intervalo CIDR inferior: puede ejecutar fuera de las direcciones IP (espacio de red) más rápidamente que un área de trabajo con una red virtual más grande. Por ejemplo, un área de trabajo con/24 red virtual y /26 las subredes pueden tener un máximo de 64 nodos activos a la vez, mientras que un área de trabajo con un /20 de red virtual y /22 subredes pueden alojar un máximo de nodos de 1024.

  Las subredes se crearán automáticamente al configurar el área de trabajo, y tendrá la oportunidad de proporcionar el intervalo CIDR para las subredes durante la configuración.

## <a name="configure-the-virtual-network"></a>Configuración de la red virtual

1. En el portal de Azure, seleccione **+ crear un recurso > Análisis > Azure Databricks** para abrir el cuadro de diálogo del servicio de Azure Databricks.

2. Siga los pasos de configuración se describe en el paso 2: Crear un área de trabajo de Azure Databricks en la Guía de introducción y seleccione el área de trabajo de implementación de Azure Databricks en las opciones de red Virtual.

   ![Crear servicio de Azure Databricks](./media/vnet-injection/create-databricks-service.png)

3. Seleccione la red virtual que desea usar.

   ![Opciones de red virtual](./media/vnet-injection/select-vnet.png)

4. Proporcionar intervalos CIDR en un bloque entre /18-/26 para las dos subredes, dedicadas a Azure Databricks:

   * Se creará una subred pública con un grupo de seguridad de red asociados que permite la comunicación con el plano de control de Azure Databricks.
   * Se creará una subred privada con un grupo de seguridad de red asociados que permite la comunicación interna del clúster.

5. Haga clic en **crear** para implementar el área de trabajo de Azure Databricks a la red virtual.

## <a name="advanced-resource-manager-configurations"></a>Configuraciones de administrador de recursos avanzada

Si desea más control sobre la configuración de la red virtual: por ejemplo, desea utilizar las subredes existentes, utilice grupos de seguridad de red existentes o crear sus propias reglas de seguridad: puede usar las siguientes plantillas de Azure Resource Manager en lugar de la implementación de área de trabajo y la configuración de red virtual del portal.

### <a name="all-in-one"></a>Todo en uno

Para crear una red virtual, grupos de seguridad de red y el área de trabajo de Azure Databricks todo en uno, use el [All-in-one plantilla para áreas de trabajo de Databricks VNet insertado](https://azure.microsoft.com/resources/templates/101-databricks-all-in-one-template-for-vnet-injection/).

Cuando use esta plantilla, no es necesario hacer cualquier whitelisting manual de tráfico de subred.

### <a name="network-security-groups"></a>Grupos de seguridad de red

Para crear grupos de seguridad de red con las reglas necesarias para una red virtual existente, use el [plantilla de grupo de seguridad de red para la inserción de la red virtual de Databricks](https://azure.microsoft.com/resources/templates/101-databricks-nsg-for-vnet-injection).

Cuando use esta plantilla, no es necesario hacer cualquier whitelisting manual de tráfico de subred.

### <a name="virtual-network"></a>Virtual network

Para crear una red virtual con las subredes públicas y privadas adecuadas, use la [plantilla de red Virtual para la inserción de la red virtual de Databricks](https://azure.microsoft.com/resources/templates/101-databricks-vnet-for-vnet-injection).

Si usa esta plantilla sin usar también la plantilla de grupos de seguridad de red, debe agregar manualmente reglas de inclusión para los grupos de seguridad de red que use con la red virtual.

### <a name="azure-databricks-workspace"></a>El área de trabajo de Azure Databricks

Para implementar un área de trabajo de Azure Databricks en una red virtual existente con subredes públicas y privadas y los grupos de seguridad de red configurados adecuadamente ya configurados, utilice el [plantilla del área de trabajo para la inserción de la red virtual de Databricks](https://azure.microsoft.com/resources/templates/101-databricks-workspace-with-vnet-injection).

Si usa esta plantilla sin usar también la plantilla de grupos de seguridad de red, debe agregar manualmente reglas de inclusión para los grupos de seguridad de red que use con la red virtual.

## <a name="whitelisting-subnet-traffic"></a>Inclusión en lista blanca el tráfico de subred

Si no usa el [portal Azure](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-portal) o [plantillas Azure Resource Manager](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-advanced) para crear grupos de seguridad de la red, debe manualmente tráfico de la siguiente lista de permitidos en las subredes.

|Dirección|Protocolo|Origen|Puerto de origen|Destino|Puerto de destino|
|---------|--------|------|-----------|-----------|----------------|
|Entrada|\*|VirtualNetwork|\*|\*|\*|
|Entrada|\*|Dirección IP de NAT de plano de control|\*|\*|22|
|Entrada|\*|Dirección IP de NAT de plano de control|\*|\*|5557|
|Salida|\*|\*|\*|IP de la aplicación Web|\*|
|Salida|\*|\*|\*|SQL (etiqueta de servicio)|\*|
|Salida|\*|\*|\*|Almacenamiento (etiqueta de servicio)|\*|
|Salida|\*|\*|\*|VirtualNetwork|\*|

Direcciones en la lista blanca el tráfico de subred mediante la siguiente dirección IP. (Tienda de metadatos) de SQL y Storage (almacenamiento de registro y artefacto), debe usar el Sql y Storage [etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

|Región de Azure Databricks|Servicio|Dirección IP pública|
|-----------------------|-------|---------|
|Este de EE. UU|Plano de control de NAT </br></br>Aplicación Web|23.101.152.95/32 </br></br>40.70.58.221/32|
|Este de EE. UU. 2|Plano de control de NAT </br></br>Aplicación Web|23.101.152.95/32 </br></br>40.70.58.221/32|
|Centro-Norte de EE. UU|Plano de control de NAT </br></br>Aplicación Web|23.101.152.95/32 </br></br>40.70.58.221/32|
|Centro de EE. UU.|Plano de control de NAT </br></br>Aplicación Web|23.101.152.95/32 </br></br>40.70.58.221/32|
|Centro-Sur de EE. UU|Plano de control de NAT </br></br>Aplicación Web|40.83.178.242/32 </br></br>40.118.174.12/32|
|Oeste de EE. UU.|Plano de control de NAT </br></br>Aplicación Web|40.83.178.242/32 </br></br>40.118.174.12/32|
|Oeste de EE. UU. 2|Plano de control de NAT </br></br>Aplicación Web|40.83.178.242/32 </br></br>40.118.174.12/32|
|Centro de Canadá|Plano de control de NAT </br></br>Aplicación Web|40.85.223.25/32 </br></br>13.71.184.74/32|
|Este de Canadá|Plano de control de NAT </br></br>Aplicación Web|40.85.223.25/32 </br></br>13.71.184.74/32|
|Oeste de Reino Unido|Plano de control de NAT </br></br>Aplicación Web|51.140.203.27/32 </br></br>51.140.204.4/32|
|Sur de Reino Unido 2|Plano de control de NAT </br></br>Aplicación Web|51.140.203.27/32 </br></br>51.140.204.4/32|
|Europa occidental|Plano de control de NAT </br></br>Aplicación Web|23.100.0.135/32 </br></br>52.232.19.246/32|
|Europa del Norte|Plano de control de NAT </br></br>Aplicación Web|23.100.0.135/32 </br></br>52.232.19.246/32|
|India Central|Plano de control de NAT </br></br>Aplicación Web|104.211.89.81/32 </br></br>104.211.101.14/32|
|Sur de la India|Plano de control de NAT </br></br>Aplicación Web|104.211.89.81/32 </br></br>104.211.101.14/32|
|Oeste de la India|Plano de control de NAT </br></br>Aplicación Web|104.211.89.81/32 </br></br>104.211.101.14/32|
|Sudeste de Asia|Plano de control de NAT </br></br>Aplicación Web|52.187.0.85/32 </br></br>52.187.145.107/32|
|Asia oriental|Plano de control de NAT </br></br>Aplicación Web|52.187.0.85/32 </br></br>52.187.145.107/32|
|Este de Australia|Plano de control de NAT </br></br>Aplicación Web|13.70.105.50/32 </br></br>13.75.218.172/32|
|Sudeste de Australia|Plano de control de NAT </br></br>Aplicación Web|13.70.105.50/32 </br></br>13.75.218.172/32|
|Centro de Australia|Plano de control de NAT </br></br>Aplicación Web|13.70.105.50/32 </br></br>13.75.218.172/32|
|Centro de Australia 2|Plano de control de NAT </br></br>Aplicación Web|13.70.105.50/32 </br></br>13.75.218.172/32|
|Este de Japón|Plano de control de NAT </br></br>Aplicación Web|13.78.19.235/32 </br></br>52.246.160.72/32|
|Oeste de Japón|Plano de control de NAT </br></br>Aplicación Web|13.78.19.235/32 </br></br>52.246.160.72/32|

## <a name="troubleshooting"></a>solución de problemas

### <a name="workspace-launch-errors"></a>Errores de inicio del área de trabajo

Iniciar un área de trabajo en una red virtual personalizada se produce un error en el Azure Databricks pantalla con el siguiente error de inicio de sesión: **"Nos hemos encontrado un error al crear el área de trabajo. Asegúrese de que la configuración de red personalizada es correcta y vuelva a intentarlo."**

Este error se debe a una configuración de red no cumple los requisitos. Confirme que ha seguido las instrucciones de este tema cuando creó el área de trabajo.

### <a name="cluster-creation-errors"></a>Errores de creación de clúster

**Instancias inaccesibles: Los recursos no son accesibles a través de SSH.**

Causa posible: se bloquea el tráfico del plano de control a los trabajadores. Corregir asegurándose de que las reglas de seguridad de entrada cumplen los requisitos. Si va a implementar en una red virtual existente conectada a la red local, revise la configuración con la información proporcionada en el área de trabajo de Azure Databricks al conectar a la red local.

**Error de inicio inesperado: Se detectó un error inesperado al configurar el clúster. Vuelva a intentar y póngase en contacto con Azure Databricks si el problema persiste. Mensaje de error interno: Tiempo de espera mientras se coloca el nodo.**

Causa posible: se bloquea el tráfico de los trabajadores a puntos de conexión de Azure Storage. Corregir asegurándose de que las reglas de seguridad de salida cumplen los requisitos. Si usa servidores DNS personalizados, compruebe también el estado de los servidores DNS de la red virtual.

**Error de inicio del proveedor de nube: Se encontró un error de proveedor en la nube al configurar el clúster. Consulte a la Guía de Azure Databricks para obtener más información. Código de error de Azure: AuthorizationFailed/InvalidResourceReference.**

Posible causa: la red virtual o las subredes ya no existe. Asegúrese de que existen la red virtual y subredes.

**Se terminó el clúster. Motivo: Error de inicio de Spark: Spark no pudo iniciarse en el tiempo. Este problema puede deberse a una tienda de metadatos de Hive que no funciona correctamente, las configuraciones de Spark no válidas o scripts de init no funciona correctamente. Consulte los registros de controlador de Spark para solucionar este problema y póngase en contacto con Databricks si el problema persiste. Mensaje de error interno: No se pudo iniciar Spark: El controlador no pudo iniciar en el tiempo.**

Causa posible: Contenedor no puede comunicarse con la instancia que hospeda o cuenta de almacenamiento DBFS. Corregir mediante la adición de una ruta personalizada para las subredes de la cuenta de almacenamiento DBFS con el próximo salto que se va a Internet.

### <a name="notebook-command-errors"></a>Errores del comando de Bloc de notas

**Comando no responde**

Causa posible: se bloquea la comunicación de trabajo para el trabajo. Corregir asegurándose de que las reglas de seguridad de entrada cumplen los requisitos.

**Se produce un error en el flujo de trabajo de Bloc de notas con la excepción: com.databricks.WorkflowException: org.apache.http.conn.ConnectTimeoutException**

Causa posible: se bloquea el tráfico de los trabajadores a Webapp de Azure Databricks. Corregir asegurándose de que los requisitos de las reglas de seguridad de salida.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Extracción, transformación y carga de datos mediante Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
