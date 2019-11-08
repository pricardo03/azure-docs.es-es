---
title: Implementación de Azure Databricks en la red virtual
description: En este artículo se describe cómo implementar Azure Databricks en la red virtual, también conocido como inserción de red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 10/10/2019
ms.openlocfilehash: 5eded3217e96ccc45951acae004d1424e16cb098
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605665"
---
# <a name="deploy-azure-databricks-in-your-virtual-network"></a>Implementación de Azure Databricks en la red virtual

La implementación predeterminada de Azure Databricks es un servicio totalmente administrado en Azure: todos los recursos del plano de datos, incluida una red virtual (VNet), se implementan en un grupo de recursos bloqueados. Sin embargo, si necesita personalizar la red, puede implementar los recursos de Azure Databricks en su propia red virtual (también llamado inyección VNet), cuando le permite lo siguiente:

* Conectar Azure Databricks con otros servicios de Azure (por ejemplo, Azure Storage) de forma más segura mediante puntos de conexión de servicio.
* Conectarse a orígenes de datos en el entorno local orígenes para su uso con Azure Databricks, aprovechando las rutas definidas por el usuario.
* Conectar Azure Databricks a un dispositivo de red virtual para inspeccionar todo el tráfico saliente y tomar medidas en función de reglas que permitan o denieguen el acceso.
* Configurar Azure Databricks para usar DNS personalizado.
* Configurar reglas de grupo de seguridad de red (NSG) para especificar las restricciones de tráfico de salida.
* Implementar clústeres de Azure Databricks en la red virtual existente.

Implementar recursos de Azure Databricks en su propia red virtual también le permite sacar provecho de rangos CIDR flexibles (entre /16-/24 para la red virtual y entre /18-/26 para las subredes).

  > [!NOTE]
  > No se puede reemplazar la red virtual para un área de trabajo existente. Si el área de trabajo actual no puede aceptar al número necesario de nodos de clúster activos, cree otra área de trabajo en una red virtual de mayor tamaño. Siga [estos pasos de migración detallados](howto-regional-disaster-recovery.md#detailed-migration-steps) para copiar recursos (blocs de notas, configuraciones de clúster, trabajos, etc.) del área de trabajo antigua a la nueva.

## <a name="virtual-network-requirements"></a>Requisitos de red virtual

Puede usar la interfaz de implementación del área de trabajo de Azure Databricks en Azure Portal para configurar automáticamente una red virtual existente con las subredes, grupo de seguridad de red y configuración de listas blancas necesarios o puede usar plantillas de Azure Resource Manager para configurar la red virtual e implementar el área de trabajo.

La red virtual que se implementa en el área de trabajo de Azure Databricks debe cumplir los siguientes requisitos:

### <a name="location"></a>Location

La red virtual debe residir en la misma ubicación que el área de trabajo de Azure Databricks.

### <a name="subnets"></a>Subredes

La red virtual debe incluir dos subredes dedicadas a Azure Databricks:

   1. Una subred privada con un grupo de seguridad de red preconfiguradas que permite la comunicación interna del clúster.

   2. Una subred pública con un grupo de seguridad de red preconfiguradas que permite la comunicación con el plano de control de Azure Databricks.

### <a name="address-space"></a>Espacio de direcciones

Un bloque CIDR entre /16-/24 para la red virtual y un bloque CIDR entre /18-/26 para las subredes públicas y privadas.

### <a name="whitelisting"></a>Lista blanca

Todo el tráfico entrante y saliente entre las subredes y el plano de control de Azure Databricks debe estar en la lista blanca.

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

En esta sección se describe cómo crear un área de trabajo de Azure Databricks en Azure Portal e implementarlo en su propia red virtual existente. Azure Databricks actualiza la red virtual con dos subredes y grupos de seguridad de red nuevos mediante rangos CIDR proporcionados por el usuario, pone en la lista blanca el tráfico de subred entrante y saliente, e implementa el área de trabajo en la red virtual actualizada.

## <a name="prerequisites"></a>Requisitos previos

Debe tener una red virtual a la que se implementará el área de trabajo de Azure Databricks. Puede usar una red virtual existente o crear una nueva, pero la red virtual debe estar en la misma región que el área de trabajo de Azure Databricks que va a crear. Para la red virtual se necesita un rango CIDR entre /16-/24.

  > [!Warning]
  > Un área de trabajo con una red virtual más pequeña; es decir, un rango CIDR inferior, puede quedarse sin direcciones IP (espacio de red) más rápidamente que un área de trabajo con una red virtual de mayor tamaño. Por ejemplo, un área de trabajo con una red virtual /24 red virtual y subredes /26 puede tener un máximo de 64 nodos activos a la vez, mientras que un área de trabajo con una red virtual /20 y subredes /22 puede alojar un máximo de 1024 nodos.

  Las subredes se crearán automáticamente al configurar el área de trabajo, y tendrá la oportunidad de proporcionar el rango CIDR para las subredes durante la configuración.

## <a name="configure-the-virtual-network"></a>Configuración de la red virtual

1. En Azure Portal, seleccione **+ Crear un recurso > Análisis > Azure Databricks** para abrir el cuadro de diálogo del servicio de Azure Databricks.

2. Siga las etapas de configuración que se describen en el paso 2: Cree un área de trabajo de Azure Databricks en la Guía de introducción y seleccione el área de trabajo de implementación de Azure Databricks en las opciones de red virtual.

   ![Crear un servicio de Azure Databricks](./media/vnet-injection/create-databricks-service.png)

3. Seleccione la red virtual que desee usar.

   ![Opciones de red virtual](./media/vnet-injection/select-vnet.png)

4. Proporcione los rangos CIDR en un bloque entre /18-/26 para las dos subredes, dedicadas a Azure Databricks:

   * Se creará una subred pública con un grupo de seguridad de red asociado que permite la comunicación con el plano de control de Azure Databricks.
   * Se creará una subred privada con un grupo de seguridad de red asociado que permite la comunicación interna del clúster.

5. Haga clic en **Crear** para implementar el área de trabajo de Azure Databricks a la red virtual.

## <a name="advanced-resource-manager-configurations"></a>Configuraciones avanzadas de administrador de recursos

Si quiere más control sobre la configuración de la red virtual, por ejemplo, quiere usar las subredes existentes, usar grupos de seguridad de red existentes o crear sus propias reglas de seguridad, puede usar las siguientes plantillas de Azure Resource Manager en lugar de la configuración de red virtual del portal e implementación de área de trabajo.

### <a name="all-in-one"></a>Todo en uno

Para crear una red virtual, grupos de seguridad de red y el área de trabajo de Azure Databricks todo en uno, use la plantilla disponible en [All-in-one Template for Databricks VNet Injected Workspaces](https://azure.microsoft.com/resources/templates/101-databricks-all-in-one-template-for-vnet-injection/) (Plantilla Todo en uno para áreas de trabajo inyectadas de red virtual de Azure Databricks).

Cuando use esta plantilla, no es necesario crear manualmente ninguna lista blanca de tráfico de subred.

### <a name="network-security-groups"></a>Grupos de seguridad de red

Para crear grupos de seguridad de red con las reglas necesarias para una red virtual existente, use la plantilla disponible en [Network Security Group Template for Databricks VNet Injection](https://azure.microsoft.com/resources/templates/101-databricks-all-in-one-template-for-vnet-injection/) (Plantilla de grupo de seguridad de red para la inyección de red virtual de Azure Databricks).

Cuando use esta plantilla, no es necesario crear manualmente ninguna lista blanca de tráfico de subred.

### <a name="virtual-network"></a>Virtual network

Para crear una red virtual con las subredes públicas y privadas adecuadas, use la plantilla disponible en [Virtual Network Template for Databricks VNet Injection](https://azure.microsoft.com/resources/templates/101-databricks-vnet-for-vnet-injection) (Plantilla de red virtual para la inyección de red virtual de Azure Databricks).

Si usa esta plantilla sin también usar la plantilla de grupos de seguridad de red, debe agregar manualmente reglas de inclusión en la lista blanca a los grupos de seguridad de red que use con la red virtual.

### <a name="azure-databricks-workspace"></a>Área de trabajo de Azure Databricks

Para implementar un área de trabajo de Azure Databricks en una red virtual existente con subredes públicas y privadas, y los grupos de seguridad de red correctamente configurados ya establecidos, use la plantilla disponible en [Workspace Template for Databricks VNet Injection](https://azure.microsoft.com/resources/templates/101-databricks-workspace-with-vnet-injection) (Plantilla de área de trabajo para la inyección de red virtual de Azure Databricks).

Si usa esta plantilla sin también usar la plantilla de grupos de seguridad de red, debe agregar manualmente reglas de inclusión en la lista blanca a los grupos de seguridad de red que use con la red virtual.

## <a name="whitelisting-subnet-traffic"></a>Inclusión en lista blanca del tráfico de subred

Si no usa [Azure Portal](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject#vnet-inject-portal) o las [plantillas de Azure Resource Manager](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-advanced) para crear grupos de seguridad de red, debe colocar manualmente en la lista blanca el siguiente tráfico en las subredes.

|Dirección|Protocolo|Source|Puerto de origen|Destino|Puerto de destino|
|---------|--------|------|-----------|-----------|----------------|
|Entrada|\*|VirtualNetwork|\*|\*|\*|
|Entrada|\*|Dirección IP de NAT de plano de control|\*|\*|22|
|Entrada|\*|Dirección IP de NAT del plano de control|\*|\*|5557|
|Salida|\*|\*|\*|IP de la aplicación web|\*|
|Salida|\*|\*|\*|SQL (etiqueta de servicio)|\*|
|Salida|\*|\*|\*|Almacenamiento (etiqueta de servicio)|\*|
|Salida|\*|\*|\*|VirtualNetwork|\*|

Colocar en la lista blanca el tráfico de subred con las siguientes direcciones IP. Para SQL (metastore) y Storage (almacenamiento de artefactos y registros), debe usar las [etiquetas de servicio](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) de SQL y Storage.

|Región de Azure Databricks|Servicio|Dirección IP pública|
|-----------------------|-------|---------|
|East US|NAT del plano de control </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Este de EE. UU. 2|NAT del plano de control </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Centro-Norte de EE. UU|NAT del plano de control </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Centro de EE. UU.|NAT del plano de control </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Centro-Sur de EE. UU|NAT del plano de control </br></br>Webapp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Oeste de EE. UU.|NAT del plano de control </br></br>Webapp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Oeste de EE. UU. 2|NAT del plano de control </br></br>Webapp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Centro de Canadá|NAT del plano de control </br></br>Webapp|40.85.223.25/32 </br></br>13.71.184.74/32|
|Este de Canadá|NAT del plano de control </br></br>Webapp|40.85.223.25/32 </br></br>13.71.184.74/32|
|Oeste de Reino Unido|NAT del plano de control </br></br>Webapp|51.140.203.27/32 </br></br>51.140.204.4/32|
|Sur de Reino Unido 2|NAT del plano de control </br></br>Webapp|51.140.203.27/32 </br></br>51.140.204.4/32|
|Europa occidental|NAT del plano de control </br></br>Webapp|23.100.0.135/32 </br></br>52.232.19.246/32|
|Europa del Norte|NAT del plano de control </br></br>Webapp|23.100.0.135/32 </br></br>52.232.19.246/32|
|India Central|NAT del plano de control </br></br>Webapp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Sur de la India|NAT del plano de control </br></br>Webapp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Oeste de la India|NAT del plano de control </br></br>Webapp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Sudeste de Asia|NAT del plano de control </br></br>Webapp|52.187.0.85/32 </br></br>52.187.145.107/32|
|Asia oriental|NAT del plano de control </br></br>Webapp|52.187.0.85/32 </br></br>52.187.145.107/32|
|Este de Australia|NAT del plano de control </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Sudeste de Australia|NAT del plano de control </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Centro de Australia|NAT del plano de control </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Centro de Australia 2|NAT del plano de control </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Este de Japón|NAT del plano de control </br></br>Webapp|13.78.19.235/32 </br></br>52.246.160.72/32|
|Oeste de Japón|NAT del plano de control </br></br>Webapp|13.78.19.235/32 </br></br>52.246.160.72/32|

## <a name="troubleshooting"></a>solución de problemas

### <a name="workspace-launch-errors"></a>Errores de inicio del área de trabajo

Se produce el siguiente error al iniciar un área de trabajo en una red virtual personalizada en la pantalla de inicio de sesión de Azure Databricks: **"We've encountered an error creating your workspace. Make sure the custom network configuration is correct and try again" (Hubo un error al crear el área de trabajo. Asegúrese de que la configuración de red personalizada sea correcta y pruebe otra vez).**

Este error se debe a una configuración de red que no cumple los requisitos. Confirme que siguió las instrucciones de este tema al crear el área de trabajo.

### <a name="cluster-creation-errors"></a>Errores de creación de clúster

**Instancias inaccesibles: los recursos no son accesibles a través de SSH.**

Causa posible: el tráfico del plano de control a los trabajos está bloqueado. Para corregirlo, asegúrese de que las reglas de seguridad de entrada cumplan los requisitos. Si va a implementar en una red virtual existente conectada a la red local, revise la configuración con la información que se proporciona en Connecting your Azure Databricks Workspace to your On-Premises Network.

**Error de inicio inesperado: hubo un error inesperado al configurar el clúster. Pruebe otra vez y póngase en contacto con Azure Databricks si el problema persiste. Mensaje de error interno: tiempo de espera al colocar el nodo.**

Causa posible: el tráfico de los trabajados a los puntos de conexión de Azure Storage está bloqueado. Para corregirlo, asegúrese de que las reglas de seguridad de salida cumplan los requisitos. Si usa servidores DNS personalizados, compruebe también el estado de los servidores DNS de la red virtual.

**Error de inicio del proveedor de nube: hubo un error de proveedor de nube al configurar el clúster. Consulte la guía de Azure Databricks para obtener más información. Código de error de Azure: AuthorizationFailed/InvalidResourceReference.**

Posible causa: la red virtual o las subredes ya no existen. Asegúrese de la red virtual y las subredes existen.

**Clúster terminado. Motivo: error de inicio de Spark. Spark no pudo iniciarse a tiempo. Este problema puede deberse a Hive Metastore que no funciona correctamente, configuraciones de Spark no válidas o scripts de init que no funcionan correctamente. Consulte los registros de controlador de Spark para solucionar este problema y póngase en contacto con Azure Databricks si el problema persiste. Mensaje de error interno: no se pudo iniciar Spark. El controlador no se inició a tiempo.**

Causa posible: el contenedor no puede comunicarse con la instancia que hospeda o la cuenta de almacenamiento DBFS. Para corregirlo, agregue una ruta personalizada a las subredes de la cuenta de almacenamiento DBFS con el próximo salto siendo Internet.

### <a name="notebook-command-errors"></a>Errores del comando de Notebook

**El comando no responde**

Causa posible: la comunicación de trabajo a trabajo está bloqueada. Para corregirlo, asegúrese de que las reglas de seguridad de entrada cumplan los requisitos.

**Se produce un error en el flujo de trabajo de Notebook con la excepcióncom.databricks.WorkflowException: org.apache.http.conn.ConnectTimeoutException**

Causa posible: el tráfico de los trabajos a Azure Databricks Webapp está bloqueado. Para corregirlo, asegúrese de que las reglas de seguridad de salida cumplan los requisitos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Extracción, transformación y carga de datos mediante Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
