---
title: Servidores de hospedaje MySQL en Azure Stack | Microsoft Docs
description: Procedimientos para agregar instancias MySQL para el aprovisionamiento mediante el proveedor de recursos de adaptador de MySQL.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 5522eb1b8b0398aeb6f1b0dd8578b906880b4e89
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939664"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Incorporación de servidores de hospedaje para el proveedor de recursos MySQL

Puede hospedar una instancia MySQL en una máquina virtual en [Azure Stack](azure-stack-poc.md), o en una máquina virtual fuera de su entorno de Azure Stack, siempre y cuando el proveedor de recursos MySQL pueda conectarse a la instancia.

## <a name="connect-to-a-mysql-hosting-server"></a>Conexión a un servidor de hospedaje MySQL

Asegúrese de que tiene las credenciales de una cuenta con privilegios de administrador del sistema. Para agregar un servidor de hospedaje, siga estos pasos:

1. Inicie sesión en el portal del operador de Azure Stack como administrador de servicios.
2. Seleccione **Más servicios**.
3. Vaya a **RECURSOS ADMINISTRATIVOS** > **MySQL Hosting Servers** (Servidores de hospedaje MySQL) > **+Agregar**. Esta acción abre el cuadro de diálogo **Add a MySQL Hosting Server** (Agregar un servidor de hospedaje MySQL), que se muestra en la siguiente captura de pantalla.

   ![Configurar un servidor host](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Proporcione los detalles de conexión de la instancia del servidor MySQL.

   * En el caso de **MySQL Hosting Server Name** (Nombre del servidor de hospedaje MySQL), proporcione el nombre de dominio completo (FQDN) o una dirección IPv4 válida. No use el nombre corto de la máquina virtual.
   * No se ha proporcionado ninguna instancia de MySQL, por lo que debe especificar el valor de **Size of Hosting Server in GB** (Tamaño del servidor de hospedaje en GB). Debe ser similar a la capacidad del servidor de bases de datos.
   * Mantenga la configuración predeterminada para **Subscrición**.
   * En el caso de **Grupo de recursos**, cree uno o use un grupo ya existente.

   > [!NOTE]
   > Si Azure Resource Manager del inquilino y del administrador tiene acceso a la instancia de MySQL, puede ponerla bajo el control del proveedor de recursos. Sin embargo, la instancia de MySQL **debe** asignarse exclusivamente al proveedor de recursos.

5. Seleccione las **SKU** para abrir el cuadro de diálogo **Create SKU** (Crear SKU).

   ![Crear una SKU de MySQL](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   El valor de **Nombre** de la SKU debe reflejar sus propiedades para que los usuarios puedan implementar sus bases de datos en la SKU adecuada.

   >[!IMPORTANT]
   >Los caracteres especiales, entre los que se incluyen espacios y puntos, no se admiten en el **nombre** ni en el **nivel** al crear una SKU para el proveedor de recursos de MySQL.

6. Seleccione **Aceptar** para crear la SKU.
7. En **Add a MySQL Hosting Server** (Agregar un servidor de hospedaje MySQL), seleccione **Crear**.

A medida que agregue servidores, asígnelos a una SKU nueva o existente para diferenciar las ofertas de servicio. Por ejemplo, puede tener una instancia empresarial de MySQL que proporcione copias de seguridad automáticas y bases de datos mayores. Puede reservar este servidor de alto rendimiento para diferentes departamentos de su organización.

## <a name="increase-backend-database-capacity"></a>Aumento de la capacidad de la base de datos back-end

Puede aumentar la capacidad de la base de datos back-end mediante la implementación de más servidores de MySQL en el portal de Azure Stack. Agregue dichos servidores a una SKU nueva o existente. Si agrega un servidor a una SKU existente, asegúrese de que las características del servidor son las mismas que las de otros servidores de la SKU.

## <a name="make-mysql-database-servers-available-to-your-users"></a>Disposición de los servidores de base de datos MySQL para los usuarios

Cree planes y ofertas para poner los servidores de bases de datos MySQL a disposición de los usuarios. Agregue el servicio Microsoft.MySqlAdapter al plan y, después, agregue la cuota predeterminada o cree una nueva.

![Creación de planes y ofertas para incluir bases de datos](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="next-steps"></a>Pasos siguientes

[Creación de una base de datos MySQL](azure-stack-mysql-resource-provider-databases.md)