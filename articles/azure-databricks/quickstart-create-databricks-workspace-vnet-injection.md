---
title: Crear un área de trabajo de Azure Databricks en una red Virtual
description: En este artículo se describe cómo implementar Azure Databricks a la red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 295b64b10f9f78ca6224d60fb84c6d1310aaa42e
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288207"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-a-virtual-network"></a>Inicio rápido: Crear un área de trabajo de Azure Databricks en una red Virtual

En este tutorial rápido se muestra cómo crear un área de trabajo de Azure Databricks en una red virtual. También creará un clúster de Apache Spark dentro de esa área de trabajo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En el portal de Azure, seleccione **crear un recurso** > **redes** > **red Virtual**.

2. En **crear red virtual**, se aplican las siguientes opciones: 

    |Configuración|Valor sugerido|DESCRIPCIÓN|
    |-------|---------------|-----------|
    |NOMBRE|databricks-Guía de inicio rápido|Seleccione un nombre de la red virtual.|
    |Espacio de direcciones|10.1.0.0/16|Intervalo de direcciones de la red virtual en notación CIDR.|
    |Subscription|\<Su suscripción\>|Seleccione la suscripción de Azure que quiera usar.|
    |Grupos de recursos|databricks-Guía de inicio rápido|Seleccione **crear nuevo** y escriba un nombre de grupo de recursos nuevo para su cuenta.|
    |Ubicación|\<Seleccione la región más cercana a los usuarios\>|Seleccione una ubicación geográfica donde puede hospedar su red virtual. Use la ubicación más cercana a los usuarios.|
    |Nombre de subred|default|Seleccione un nombre para la subred predeterminada en la red virtual.|
    |Intervalo de direcciones de subred|10.1.0.0/24|El intervalo de direcciones de la subred en notación CIDR. Debe estar incluido en el espacio de direcciones de la red virtual. No se puede editar el intervalo de direcciones de una subred que está en uso.|

    ![Crear una red virtual en Azure portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. Una vez completada la implementación, vaya a la red virtual y seleccione **espacio de direcciones** en **configuración**. En el cuadro que dice *Agregar intervalo de direcciones adicional*, insertar `10.179.0.0/16` y seleccione **guardar**.

    ![Espacio de direcciones de red virtual de Azure](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

1. En el portal de Azure, seleccione **crear un recurso** > **Analytics** > **Databricks**.

2. En **Azure Databricks Service**, se aplican las siguientes opciones:

    |Configuración|Valor sugerido|DESCRIPCIÓN|
    |-------|---------------|-----------|
    |Nombre del área de trabajo|databricks-Guía de inicio rápido|Seleccione un nombre para el área de trabajo de Azure Databricks.|
    |Subscription|\<Su suscripción\>|Seleccione la suscripción de Azure que quiera usar.|
    |Grupos de recursos|databricks-Guía de inicio rápido|Seleccione el mismo grupo de recursos que usó para la red virtual.|
    |Ubicación|\<Seleccione la región más cercana a los usuarios\>|Elija la misma ubicación que la red virtual.|
    |Nivel de precios|Elegir entre estándar o Premium.|Para obtener más información sobre los planes de tarifa, consulte el [página de precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/).|
    |Implementar el área de trabajo de Azure Databricks en la red Virtual|Sí|Esta configuración le permite implementar un área de trabajo de Azure Databricks en la red virtual.|
    |Virtual Network|databricks-Guía de inicio rápido|Seleccione la red virtual que creó en la sección anterior.|
    |Nombre de la subred pública|subred pública|Use el nombre de la subred pública predeterminada.|
    |Intervalo de CIDR de subred pública|10.179.64.0/18|Intervalo CIDR para esta subred debe estar entre /18 y /26.|
    |Nombre de la subred privada|private-subnet|Use el nombre de la subred privada predeterminada.|
    |Intervalo de CIDR de subred privada|10.179.0.0/18|Intervalo CIDR para esta subred debe estar entre /18 y /26.|

    ![Crear un área de trabajo de Azure Databricks en Azure portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. Una vez completada la implementación, navegue hasta el recurso de Azure Databricks. Tenga en cuenta que el emparejamiento de redes virtuales está deshabilitado. Tenga en cuenta el grupo de recursos y el grupo de recursos administrados en la página información general. 

    ![Información general de Azure Databricks en Azure portal](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    El grupo de recursos administrado contiene la ubicación física de la cuenta de almacenamiento (DBFS), trabajo-sg (grupo de seguridad de red), los trabajadores de la red virtual (red virtual). También es la ubicación donde se creará la interfaz de red, disco, dirección IP y las máquinas virtuales. Este grupo de recursos está bloqueado de forma predeterminada; Sin embargo cuando se inicia un clúster en la red virtual, se crea una interfaz de red entre el trabajadores de la red virtual en el grupo de recursos administrados y la red virtual "central".

    ![Grupo de recursos administrados de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

## <a name="create-a-cluster"></a>Creación de un clúster

> [!NOTE]
> Para usar una cuenta gratuita para crear el clúster de Azure Databricks, antes de crear el clúster, vaya a su perfil y cambie su suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/).

1. Vuelva a su servicio de Azure Databricks y seleccione **iniciar área de trabajo** en el **Introducción** página.

2. Seleccione **clústeres** > **+ crear clúster**. A continuación, cree un nombre de clúster, como *databricks-quickstart-cluster*y acepte los valores predeterminados restantes. Seleccione **Create cluster** (Crear clúster).

    ![Crear el clúster de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Una vez que se está ejecutando el clúster, se devuelven al grupo de recursos administrados en el portal de Azure. Tenga en cuenta las nuevas máquinas virtuales, discos, dirección IP y las interfaces de red. Se crea una interfaz de red en cada una de las subredes públicas y privadas con direcciones IP.  

    ![Grupo de recursos administrados de Azure Databricks después de la creación del clúster](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Vuelva al área de trabajo Azure Databricks y seleccione el clúster que creó. A continuación, navegue hasta el **ejecutores** ficha la **la interfaz de usuario de Spark** página. Tenga en cuenta que las direcciones para el controlador y los ejecutores son en el intervalo de subred privada. En este ejemplo, el controlador es 10.179.0.6 y ejecutores son 10.179.0.4 y 10.179.0.5. Las direcciones IP podrían ser diferentes.

    ![Ejecutores de interfaz de usuario de Databricks Spark de Azure](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado el artículo, puede finalizar el clúster. Para ello, desde el área de trabajo de Azure Databricks, en el panel izquierdo, seleccione **Clusters** (Clústeres). En el clúster que desea finalizar, mueva el cursor sobre el botón de puntos suspensivos en la columna **Actions** (Acciones) y seleccione el icono **Terminate** (Finalizar). Esto evita que el clúster.

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. En tal caso, el clúster se detiene automáticamente si ha estado inactivo durante el tiempo especificado.

Si no desea volver a usar el clúster, puede eliminar el grupo de recursos que creó en el portal de Azure.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un clúster de Spark en Azure Databricks que ha implementado en una red virtual. Avance al siguiente artículo para obtener información sobre cómo consultar un contenedor de Docker de Linux de SQL Server en la red virtual mediante JDBC desde un bloc de notas de Azure Databricks.  

> [!div class="nextstepaction"]
>[Consulta de un contenedor de Docker de Linux de SQL Server en una red virtual desde un bloc de notas de Azure Databricks](vnet-injection-sql-server.md)
