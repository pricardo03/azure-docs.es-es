---
title: Configurar el firewall de base de datos de SQL de Azure Blockchain Workbench
description: Obtenga información sobre cómo configurar el firewall de base de datos de SQL de la versión preliminar de Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 09/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 0153065ca0ccd6cf34456d630d7437d5ea7c5b48
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70845221"
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Configurar el firewall de base de datos de Azure Blockchain Workbench

En este artículo se muestra cómo configurar una regla de firewall mediante el Azure Portal. Las reglas de firewall permiten a los clientes o aplicaciones externos conectarse a la base de datos de Azure Blockchain Workbench.

## <a name="connect-to-the-blockchain-workbench-database"></a>Conectarse a la base de datos de Blockchain Workbench

Para conectarse a la base de datos en la que desea configurar una regla:

1. Inicie sesión en Azure Portal con una cuenta que tenga permisos de **Propietario** para los recursos de Azure Blockchain Workbench.
2. En el panel de la izquierda, seleccione **Grupos de recursos**.
3. Elija el nombre del grupo de recursos para la implementación de Blockchain Workbench.
4. Seleccione **Tipo** para ordenar la lista de recursos y luego elija el servidor **SQL server**.
5. En el ejemplo de lista de recursos de la captura de pantalla siguiente se muestran dos bases de datos: *master* and *lsgn-sdk*. La regla de firewall se configura en *lsgn-sdk*.

![Enumerar recursos de Blockchain Workbench](./media/database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Crear una regla de firewall de base de datos

Para crear una regla de firewall:

1. Elija el vínculo a la base de datos de "lsgn-sdk".
2. En la barra de menús, seleccione **Establecer el firewall del servidor**.

   ![Establecer el firewall del servidor](./media/database-firewall/configure-server-firewall.png)

3. Para crear una regla para la organización:

   * Especificar un valor en **Nombre de la regla**
   * Especificar una dirección IP para el campo **IP inicial** del intervalo de direcciones
   * Especificar una dirección IP para el campo **IP final** del intervalo de direcciones

   ![Crear reglas de firewall](./media/database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Si solo desea agregar la dirección IP de su equipo, elija **+ Agregar IP de cliente**.
        
1. Para guardar la configuración del firewall, seleccione **Guardar**.
2. Conéctese a una aplicación o herramienta para probar el intervalo de direcciones IP que configuró para la base de datos. Por ejemplo, SQL Server Management Studio.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vistas de base de datos de Azure Blockchain Workbench](database-views.md)