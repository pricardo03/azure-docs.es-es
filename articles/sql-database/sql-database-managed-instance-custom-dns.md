---
title: DNS personalizado de Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describen las opciones de configuración para un DNS personalizado con Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 12/13/2018
ms.openlocfilehash: bb5890b883b6062d834b928bff28a26a3664fb64
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787907"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Configuración de un DNS personalizado para Instancia administrada de Azure SQL Database

Se debe implementar una instancia administrada de Azure SQL Database dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure. Hay algunos escenarios (por ejemplo, correo electrónico de base de datos o servidores vinculados a otras instancias de SQL en el entorno híbrido o en la nube) que requieren que los nombres de host privados se resuelvan en Instancia administrada. En este caso, debe configurar un DNS personalizado dentro de Azure. Puesto que Instancia administrada utiliza el mismo DNS para su funcionamiento interno, la configuración de DNS de la red virtual debe ser compatible con Instancia administrada.

   > [!IMPORTANT]
   > Utilice siempre nombres de dominio completos (FQDN) para los servidores de correo electrónico, servidores SQL Server y otros servicios, incluso si están dentro de la zona DNS privada. Por ejemplo, use `smtp.contoso.com` para el servidor de correo electrónico porque simplemente `smtp` no se resolverá correctamente.

Para realizar una configuración personalizada de DNS compatible con Instancia administrada, debe:

- Configurar el servidor DNS personalizado para que sea capaz de resolver los nombres de dominio público
- Colocar la dirección IP de DNS de resolución recursiva de Azure 168.63.129.16 al final de la lista DNS de la red virtual

## <a name="setting-up-custom-dns-servers-configuration"></a>Configuración de los servidores DNS personalizados

1. En Azure Portal, busque la opción DNS personalizado para la red virtual.

   ![Opción DNS personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png)

2. Cambie a Personalizado y escriba la dirección IP del servidor DNS personalizado, así como la dirección IP de resoluciones recursivas de Azure 168.63.129.16.

   ![Opción DNS personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png)

   > [!IMPORTANT]
   > No establecer una resolución recursiva de Azure en la lista de DNS puede provocar que la Instancia administrada entre en un estado defectuoso cuando los servidores DNS personalizados no estén disponibles por algún motivo. La recuperación a partir de un estado así puede requerir la creación de una nueva instancia en una red virtual con las directivas de red compatibles, la creación de los datos a nivel de instancia y la restauración de las bases de datos. El establecimiento de la resolución recursiva de Azure como la última entrada de la lista de DNS garantiza, incluso cuando se produce un error en todos los servidores DNS personalizados, que los nombres públicos todavía se puedan resolver.

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada de SQL Database (versión preliminar)?](sql-database-managed-instance.md)
- Para ver un tutorial que muestra cómo crear una instancia administrada, consulte el artículo de [creación de instancias administradas](sql-database-managed-instance-get-started.md).
- Para información sobre cómo configurar una red virtual para una instancia administrada, consulte el artículo sobre la [configuración de la red virtual para las instancias administradas](sql-database-managed-instance-connectivity-architecture.md).
