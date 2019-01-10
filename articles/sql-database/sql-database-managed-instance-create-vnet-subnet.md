---
title: Creación de una red virtual de Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describe cómo crear una red virtual en la que puede implementar una Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: a8000fb26ce5496a9c62ba475b862f8f80adf6b7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041771"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Configuración de una red virtual para Instancia administrada de Azure SQL Database

En este tema se explica cómo crear una red virtual y una subred válidas en las que puede implementar Instancias administradas de Azure SQL Database.

La Instancia administrada de Azure SQL Database debe implementarse dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure. Esta implementación permite los siguientes escenarios:

- Dirección IP privada segura
- Conexión a Instancia administrada directamente desde una red local
- Conexión de Instancia administrada con un servidor vinculado o con otro almacén de datos local
- Conexión de Instancia administrada a los recursos de Azure  

  > [!Note]
  > Debe [determinar el tamaño de la subred para Instancia administrada](sql-database-managed-instance-determine-size-vnet-subnet.md) antes de implementar una primera instancia porque no se puede cambiar el tamaño de la subred una vez puestos dentro los recursos.
  > Si piensa usar una red virtual existente, tiene que modificar la configuración de red para dar cabida a Instancia administrada. Para más información, consulte [Modificación de una red virtual existente para Instancia administrada](sql-database-managed-instance-configure-vnet-subnet.md).

## <a name="create-a-new-virtual-network"></a>Crear una nueva red virtual

La manera más fácil de crear y configurar la red virtual es usar la plantilla de implementación de Azure Resource Manager.

1. Inicie sesión en el Portal de Azure.

2. Use el botón **Implementar en Azure** para implementar una red virtual en la nube de Azure:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Este botón abrirá un formulario que puede usar para configurar el entorno de red, donde puede implementar Instancia administrada.

   > [!Note]
   > Esta plantilla de Azure Resource Manager implementará una red virtual con dos subredes. Una subred llamada **ManagedInstances** está reservada para instancias administradas y tiene una tabla de rutas configurada previamente, mientras que la otra subred se denomina **Default** y se usa para otros recursos que deben tener acceso a Instancia administrada (por ejemplo, Azure Virtual Machines). Puede quitar la subred **Default** si no la necesita.

3. Configure el entorno de red. En el formulario siguiente, puede configurar parámetros de su entorno de red:

![Configuración de la red de Azure](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

Puede cambiar los nombres de la red virtual y de las subredes, y ajustar los intervalos IP asociados a sus recursos de red. Una vez que presione el botón "Comprar", se creará el formulario y configurará el entorno. Si no necesita dos subredes, puede eliminar la predeterminada.

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada?](sql-database-managed-instance.md)
- Obtenga información sobre la [arquitectura de conectividad en Instancia administrada](sql-database-managed-instance-connectivity-architecture.md).
- Aprenda a [modificar la red virtual existente para Instancia administrada](sql-database-managed-instance-configure-vnet-subnet.md)
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una instancia administrada y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL Database](sql-database-managed-instance-get-started.md).
- Para problemas DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md)
