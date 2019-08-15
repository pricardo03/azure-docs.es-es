---
title: Creación de área de trabajo de Azure Databricks en una red virtual
description: En este artículo se describe cómo implementar Azure Databricks en la red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 12ac5c44a0ee479d84616b138f9e2369a195c275
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976463"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-a-virtual-network"></a>Inicio rápido: Creación de área de trabajo de Azure Databricks en una red virtual

En este inicio rápido se muestra cómo crear un área de trabajo de Azure Databricks en una red virtual. También creará un clúster de Apache Spark dentro de esa área de trabajo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

> [!Note]
> Este tutorial no puede llevarse a cabo mediante una **suscripción de evaluación gratuita de Azure**.
> Si tiene una cuenta gratuita, vaya a su perfil y cambiar la suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/). Después, [quite el límite de gasto](https://docs.microsoft.com/azure/billing/billing-spending-limit#remove-the-spending-limit-in-account-center) y [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) para las vCPU de su región. Cuando crea su área de trabajo de Azure Databricks, puede seleccionar el plan de tarifa de la **Trial (Premium - 14-Days Free DBUs)** para que el área de trabajo acceda a las DBU Premium de Azure Databricks gratis durante 14 días.

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En Azure Portal, seleccione **Crear un recurso** > **Redes** > **Red virtual**.

2. En **Creación de una red virtual**, aplique las opciones siguientes: 

    |Configuración|Valor sugerido|DESCRIPCIÓN|
    |-------|---------------|-----------|
    |NOMBRE|databricks-quickstart|Seleccione un nombre para la red virtual.|
    |Espacio de direcciones|10.1.0.0/16|El intervalo de direcciones de la red virtual en notación CIDR.|
    |Subscription|\<Su suscripción\>|Seleccione la suscripción de Azure que quiera usar.|
    |Resource group|databricks-quickstart|Seleccione **Crear nuevo** y escriba un nuevo nombre de grupo de recursos para la cuenta.|
    |Location|\<Seleccione la región más cercana a los usuarios\>|Seleccione la ubicación geográfica donde puede hospedar la red virtual. Use la ubicación más cercana a los usuarios.|
    |Nombre de subred|default|Seleccione un nombre para la subred predeterminada en la máquina virtual.|
    |Intervalo de direcciones de subred|10.1.0.0/24|El intervalo de direcciones de la subred en notación CIDR. Debe incluirse en el espacio de direcciones de la red virtual. El intervalo de direcciones de una subred que está en uso no se puede modificar.|

    ![Creación de una red virtual en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. Una vez completada la implementación, vaya a la red virtual y seleccione **Espacio de direcciones** bajo **Configuración**. En el cuadro que dice *Agregar otro intervalo de direcciones*, inserte `10.179.0.0/16` y seleccione **Guardar**.

    ![Espacios de direcciones de red virtual de Azure](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

1. En Azure Portal, seleccione **Crear un recurso** > **Analytics** > **Databricks**.

2. En **Servicio Azure Databricks**, proporcione los valores siguientes:

    |Configuración|Valor sugerido|DESCRIPCIÓN|
    |-------|---------------|-----------|
    |Nombre del área de trabajo|databricks-quickstart|Seleccione un nombre para el área de trabajo de Azure Databricks.|
    |Subscription|\<Su suscripción\>|Seleccione la suscripción de Azure que quiera usar.|
    |Resource group|databricks-quickstart|Seleccione el mismo grupo de recursos que usó para la red virtual.|
    |Location|\<Seleccione la región más cercana a los usuarios\>|Seleccione la misma ubicación que la de la red virtual.|
    |Nivel de precios|Elija entre Standard o Premium.|Para más información sobre estos planes de tarifa, consulte la [página de precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/).|
    |Implementación de un área de trabajo de Azure Databricks en una red virtual|Sí|Este parámetro le permite implementar un área de trabajo de Azure Databricks en la red virtual.|
    |Virtual Network|databricks-quickstart|Seleccione la red virtual que creó en el paso anterior.|
    |Nombre de la subred pública|public-subnet|Use el nombre de la subred pública predeterminado.|
    |Intervalo de CIDR de la subred pública|10.179.64.0/18|El intervalo CIDR para esta subred debe estar entre /18 y /26.|
    |Nombre de la subred privada|private-subnet|Use el nombre de la subred privada predeterminado.|
    |Intervalo de CIDR de la subred privada|10.179.0.0/18|El intervalo CIDR para esta subred debe estar entre /18 y /26.|

    ![Creación de un área de trabajo de Azure Databricks en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. Una vez completada la implementación, navegue hasta el recurso de Azure Databricks. Observe que el emparejamiento de redes virtuales está deshabilitado. Observe también el grupo de recursos y el grupo de recursos administrados en la página de información general. 

    ![Información general de Azure Databricks en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    El grupo de recursos administrado contiene la ubicación física de la cuenta de almacenamiento (DBFS), worker-sg (grupo de seguridad de red) y workers-vnet (red virtual). También es la ubicación donde se crearán las máquinas virtuales, el disco, la dirección IP y la interfaz de red. Este grupo de recursos está bloqueado de forma predeterminada; sin embargo, cuando se inicia un clúster en la red virtual, se crea una interfaz de red entre la red virtual workers-vnet del grupo de recursos administrados y la red virtual "hub".

    ![Grupo de recursos administrados de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

## <a name="create-a-cluster"></a>Creación de un clúster

> [!NOTE]
> Para usar una cuenta gratuita para crear el clúster de Azure Databricks, antes de crear el clúster, vaya a su perfil y cambie su suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/).

1. Vuelva al servicio de Azure Databricks y seleccione **Launch Workspace** (Iniciar área de trabajo) en la página de **información general**.

2. Seleccione **Clústeres** >  **+ Crear clúster**. A continuación, cree un nombre de clúster, como *databricks-quickstart-cluster*, y acepte el resto de valores predeterminados. Seleccione **Create cluster** (Crear clúster).

    ![Creación de un clúster de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Una vez que se esté ejecutando el clúster, vuelva al grupo de recursos administrados de Azure Portal. Observe las nuevas máquinas virtuales, discos, dirección IP e interfaces de red. Se crea una interfaz de red en cada una de las subredes públicas y privadas con direcciones IP.  

    ![Grupo de recursos administrados de Azure Databricks tras la creación del clúster](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Vuelva al área de trabajo Azure Databricks y seleccione el clúster que creó. A continuación, navegue hasta la pestaña **Executors** (Ejecutores) de la página de la **interfaz de usuario de Spark**. Tenga en cuenta que las direcciones para el controlador y los ejecutores están en el intervalo de subred privada. En este ejemplo, el controlador es 10.179.0.6 y los ejecutores son 10.179.0.4 y 10.179.0.5. Sus direcciones IP podrían ser diferentes.

    ![Ejecutores de la IU de Spark de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado el artículo, puede finalizar el clúster. Para ello, desde el área de trabajo de Azure Databricks, en el panel izquierdo, seleccione **Clusters** (Clústeres). En el clúster que desea finalizar, mueva el cursor sobre el botón de puntos suspensivos en la columna **Actions** (Acciones) y seleccione el icono **Terminate** (Finalizar). Esto detiene el clúster.

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. En tal caso, el clúster se detiene automáticamente si ha estado inactivo durante el tiempo especificado.

Si no desea volver a usar el clúster, puede eliminar el grupo de recursos que creó en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un clúster de Spark en Azure Databricks que ha implementado en una red virtual. Avance al siguiente artículo para obtener información sobre cómo consultar un contenedor de Docker de Linux de SQL Server en la red virtual mediante JDBC desde un cuaderno de Azure Databricks.  

> [!div class="nextstepaction"]
>[Query a SQL Server Linux Docker container in a virtual network from an Azure Databricks notebook](vnet-injection-sql-server.md) (Consulta de un contenedor de Docker de Linux para SQL Server en una red virtual mediante un cuaderno de Azure Databricks)
