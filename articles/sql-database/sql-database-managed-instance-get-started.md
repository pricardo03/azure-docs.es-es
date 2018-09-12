---
title: 'Azure Portal: creación de una instancia administrada de SQL | Microsoft Docs'
description: Cree una instancia administrada de SQL, un entorno de red y una máquina virtual cliente para el acceso.
keywords: tutorial de sql database, crear una instancia administrada de sql
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: quickstart
ms.date: 08/31/2018
ms.author: jovanpop-msft
ms.openlocfilehash: 4271f0cef31b0e028ed1f9408166c37d4cbbe109
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382005"
---
# <a name="create-an-azure-sql-managed-instance"></a>Creación de una instancia administrada de Azure SQL

En este inicio rápido se explica cómo crear una instancia administrada de SQL en Azure. Instancia administrada de Azure SQL Database es una instancia del motor de base de datos de SQL Server PaaS (plataforma como-servicio) que permite ejecutar y escalar bases de datos de SQL Server de alta disponibilidad en la nube de Azure. En esta guía de inicio rápido se muestra cómo comenzar mediante la creación de una instancia administrada de SQL.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="prepare-network-environment"></a>Preparación del entorno de red

Instancia administrada de SQL es un servicio seguro que se coloca en su propia instancia de Azure Virtual Network (VNet). Para crear una instancia administrada, es preciso preparar el entorno de red de Azure, lo que incluye:
 - Una red virtual de Azure, en la que se colocará su instancia administrada.
 - Una subred de la red virtual de Azure, en la que se colocarán las instancias administradas.
 - Ruta definida por el usuario que permitirá a la instancia administrada comunicarse con los servicios de Azure que controlan y administran la instancia.

La subred está dedicada a las instancias administradas y no se pueden crear otros recursos (por ejemplo Azure Virtual Machines) en esa subred. En esta guía de inicio rápido se crearán dos subredes en una red virtual de Azure, con el fin de que pueda colocar instancias administradas en la subred dedicada a las instancias administradas y otros recursos en la subred predeterminada.

1. Para implementar un entorno de red de Azure preparado para la instancia administrada de Azure SQL Database, haga clic en el botón siguiente:

    <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener"> <img src="http://azuredeploy.net/deploybutton.png"> </a>

    Este botón abrirá un formulario en Azure Portal en el que se puede configurar un entorno de red antes de implementarlo.

2. Opcionalmente, cambie los nombres de la red virtual y de las subredes, y ajuste los intervalos IP asociados a sus recursos de red. Después, presione el botón "Comprar" para crear y configurar el entorno:

    ![crear un entorno de una instancia administrada](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

    > [!Note]
    > Esta implementación de Azure Resource Manager creará dos subredes en la red virtual: una para las instancias administradas llamada **ManagedInstances** y otra llamada **Default** para otros recursos, como la máquina virtual cliente que se pueden usar para conectarse a la instancia administrada. Si no necesita dos subredes, puede eliminar la valor predeterminado más adelante; sin embargo, en ese caso no podría completar el paso 3 en esta guía de inicio rápido: [Preparación de la máquina cliente](#prepare-client-machine).

    > [!Note]
    > Si cambia los nombres de la red virtual y las subredes, asegúrese de que recuerda los nombres nuevos, ya que los necesitará en las secciones siguientes. En el resto del tutorial se asumirá que ha creado una red virtual denominada **MyNewVNet**, una subred **ManagedInstances** para instancias administradas de SQL y una subred **Default** para las máquinas virtuales y otros recursos.

## <a name="create-a-managed-instance"></a>Creación de una instancia administrada

Los pasos siguientes muestran cómo crear la instancia administrada cuando se haya aprobado la versión preliminar.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Busque **Instancia administrada** y seleccione **Instancia administrada Azure SQL Database (versión preliminar)**.
3. Haga clic en **Create**(Crear).

   ![Crear una instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Seleccione la suscripción y compruebe que los términos de la versión preliminar muestran **Aceptado**.

   ![versión preliminar de la instancia administrada aceptada](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Rellene el formulario de la instancia administrada con la información solicitada, utilizando los datos de la siguiente tabla:

   | Configuración| Valor sugerido | DESCRIPCIÓN |
   | ------ | --------------- | ----------- |
   |**Nombre de la instancia administrada**|Cualquier nombre válido|Para conocer cuáles son los nombres válidos, consulte el artículo [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Inicio de sesión de administrador de la instancia administrada**|Cualquier nombre de usuario válido|Para conocer cuáles son los nombres válidos, consulte el artículo [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). No utilice "serveradmin", ya es un rol de nivel de servidor reservado.| 
   |**Contraseña**|Cualquier contraseña válida|La contraseña debe tener al menos 16 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Grupo de recursos**|El grupo de recursos que ha creado anteriormente||
   |**Ubicación**|La ubicación que ha seleccionado anteriormente|Para obtener información acerca de las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).|
   |**Red virtual**|La red virtual que ha creado anteriormente| Elija el elemento **MyNewVNet/ManagedInstances** si no ha cambiado los nombres en el paso anterior. De lo contrario, elija el nombre de la red virtual y la subred de la instancia administrada que especificó en la sección anterior. **No utilice la subred predeterminada porque no está configurada para hospedar instancias administradas**. |

   ![formulario de creación de instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

6. Haga clic en **Plan de tarifa** para cambiar el tamaño de los recursos de almacenamiento y de proceso, así como para revisar las opciones del plan de tarifa. De forma predeterminada, la instancia obtiene 32 GB de espacio de almacenamiento de forma gratuita, **lo que puede que no sea suficiente para sus aplicaciones**.
7. Utilice los controles deslizantes o cuadros de texto para especificar la cantidad de almacenamiento y el número de núcleos virtuales. 
   ![plan de tarifa de instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-pricing-tier.png)

8. Cuando haya terminado, haga clic en **Aplicar** para guardar la selección.  
9. Haga clic en **Crear** para implementar la instancia administrada.
10. Haga clic en el icono de **notificaciones** para ver el estado de la implementación.
11. Haga clic en **Deployment in progress** (Implementación en curso) para abrir la ventana de la instancia administrada y seguir la supervisión del progreso de la implementación.

Mientras se realiza la implementación, continúe con el siguiente procedimiento.

> [!IMPORTANT]
> Para la primera instancia de una subred, el tiempo de implementación es típicamente mucho mayor que en las instancias siguientes. No cancele la operación de implementación porque dure más de lo previsto. La creación de la segunda instancia administrada en la subred llevará un par de minutos.

## <a name="prepare-client-machine"></a>Preparación de la máquina cliente

Dado que la instancia administrada de SQL está colocada en su red virtual privada, es preciso que cree una máquina virtual de Azure con una herramienta de cliente SQL como SQL Server Management Studio o SQL Operations Studio para conectarse a la instancia administrada y ejecutar consultas.

> [!Note]
> En lugar de la máquina virtual de Azure del cliente, puede configurar la conexión de [punto a sitio](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) y conectarse a la instancia administrada desde un equipo local.

La forma más fácil de crear una máquina virtual cliente con todas las herramientas necesarias es usar las plantillas de Azure Resource Manager.

1. Haga clic en el siguiente botón (asegúrese de que ha iniciado sesión en Azure Portal en otra pestaña del explorador):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. En el formulario que se abrirá, escriba el nombre de la máquina virtual, el nombre de usuario del administrador y la contraseña que usará para conectarse a la máquina virtual.

    ![crear máquina virtual cliente](./media/sql-database-managed-instance-get-started/create-client-sql-vm.png)

    Si no ha cambiado el nombre de la red virtual y la subred predeterminada, no es necesario cambiar los dos últimos parámetros; en caso contrario, debe cambiar estos valores por los que especificó al configurar el entorno de red.

3. Haga clic en el botón "Comprar" y se implementará una máquina virtual de Azure en la red que preparó.

4. Conéctese a su máquina virtual mediante la conexión al Escritorio remoto y busque SQL Server Management Studio o SQL Operation Studio, que se instalan automáticamente en la máquina virtual.

5. Abra SQL Server Management Studio y escriba el **nombre de host** de la instancia administrada en el cuadro **Nombre del servidor**, seleccione **Autenticación de SQL Server**, especifique el inicio de sesión y la contraseña en el cuadro de diálogo **Conectar con el servidor** y haga clic en **Conectar**.

    ![conexión ssms](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Después de conectarse, puede ver las bases de datos del sistema y de los usuarios en el nodo Bases de datos, así como varios objetos en los nodos Seguridad, Objetos de servidor, Replicación, Administración, Agente SQL Server y XEvent Profiler.

## <a name="next-steps"></a>Pasos siguientes

 - [Conexión de aplicaciones a instancia administrada](sql-database-managed-instance-connect-app.md).
 - [Migración de bases de datos de un entorno local a instancia administrada](sql-database-managed-instance-migrate.md).


