---
title: DNS personalizado de Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describen las opciones de configuración para un DNS personalizado con Instancia administrada de Azure SQL Database.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 09/23/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 2d1bb7e8522da32dd33933261ea41b578f8afac1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949492"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Configuración de un DNS personalizado para Instancia administrada de Azure SQL Database

Se debe implementar una instancia administrada de Azure SQL Database dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure. Hay algunos escenarios (es decir, servidores vinculados a otras instancias de SQL en el entorno híbrido o en la nube) que requieren que los nombres de host privados se resuelvan en Instancia administrada. En este caso, debe configurar un DNS personalizado dentro de Azure. Puesto que Instancia administrada utiliza el mismo DNS para su funcionamiento interno, la configuración de DNS de la red virtual debe ser compatible con Instancia administrada. 

Para realizar una configuración personalizada de DNS compatible con Instancia administrada, debe: 
- Configurar el servidor DNS personalizado para que sea capaz de resolver los nombres de dominio público 
- Colocar la dirección IP de DNS de resolución recursiva de Azure 168.63.129.16 al final de la lista DNS de la red virtual 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Configuración de los servidores DNS personalizados

1. En Azure Portal, busque la opción DNS personalizado para la red virtual.

   ![Opción DNS personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Cambie a Personalizado y escriba la dirección IP del servidor DNS personalizado, así como la dirección IP de resoluciones recursivas de Azure 168.63.129.16. 

   ![Opción DNS personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Si no se configura la resolución recursiva de Azure en la lista de DNS, se genera un error en el estado de Instancia administrada. La recuperación a partir de un estado así puede requerir la creación de una nueva instancia en una red virtual con las directivas de red compatibles, la creación de los datos a nivel de instancia y la restauración de las bases de datos. Consulte [Configuración de VNET](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md)
- Para ver un tutorial que muestra cómo crear una instancia administrada, consulte el artículo de [creación de instancias administradas](sql-database-managed-instance-get-started.md).
- Para información sobre cómo configurar una red virtual para una instancia administrada, consulte el artículo sobre la [configuración de la red virtual para las instancias administradas](sql-database-managed-instance-vnet-configuration.md).
