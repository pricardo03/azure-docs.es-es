---
title: 'Tutorial: adición de una instancia administrada de SQL Database a un grupo de conmutación por error'
description: Obtenga información para configurar un grupo de conmutación por error para su instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sashan, carlrab
manager: jroth
ms.date: 06/27/2019
ms.openlocfilehash: 3e5b96cf4227e933aa99b37469410276a775dbed
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103087"
---
# <a name="tutorial-add-a-sql-database-managed-instance-to-a-failover-group"></a>Tutorial: adición de una instancia administrada de SQL Database a un grupo de conmutación por error

Agregue una instancia administrada de SQL Database a un grupo de conmutación por error. En este artículo, aprenderá a:

> [!div class="checklist"]
> - Crear una instancia administrada principal
> - Crear una instancia administrada secundaria como parte de [un grupo de conmutación por error](sql-database-auto-failover-group.md). 
> - Conmutación por error de prueba

  > [!NOTE]
  > - Al completar este tutorial, asegúrese de que está configurando los recursos con los [requisitos previos para configurar grupos de conmutación por error para la instancia administrada](sql-database-auto-failover-group.md#enabling-geo-replication-between-managed-instances-and-their-vnets). 
  > - La creación de una instancia administrada puede tardar bastante tiempo. Como resultado, este tutorial podría tardar varias horas en completarse. Para obtener más información sobre los tiempos de aprovisionamiento, consulte [Operaciones de administración de instancia administrada](sql-database-managed-instance.md#managed-instance-management-operations). 
  > - El uso de grupos de conmutación por error con instancias administradas actualmente se encuentra en versión preliminar. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de disponer de los siguientes elementos: 

- Una suscripción a Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una. 


## <a name="1---create-resource-group-and-primary-managed-instance"></a>1\. Creación de un grupo de recursos y una instancia administrada principal
En este paso, creará el grupo de recursos y la instancia administrada principal del grupo de conmutación por error mediante Azure Portal. 

1. Seleccione **Azure SQL** en el menú izquierdo de Azure Portal. Si **Azure SQL** no está en la lista, seleccione **Todos los servicios** y escriba Azure SQL en el cuadro de búsqueda. (Opcional) Seleccione la estrella junto a **Azure SQL** para marcarlo como favorito y agréguelo como un elemento en el panel de navegación izquierdo. 
1. Seleccione **+ Agregar** para abrir la página **Select SQL deployment option** (Seleccionar la opción de implementación de SQL). Para ver más información sobre las distintas bases de datos, seleccione Mostrar detalles en el icono Bases de datos.
1. Seleccione **Crear** en el icono **Instancias administradas de SQL**. 

    ![Selección de la instancia administrada](media/sql-database-managed-instance-failover-group-tutorial/select-managed-instance.png)

1. En la página **Crear instancia administrada de Azure SQL Database**, desde la pestaña **Aspectos básicos**
    1. En **Detalles del proyecto**, seleccione su **Suscripción** en la lista desplegable y, a continuación, elija **Crear nuevo** en grupo de recursos. Escriba un nombre para el grupo de recursos, como `myResourceGroup`. 
    1. En **Detalles de la instancia administrada**, proporcione el nombre de la instancia administrada y la región en la que quiere implementar la instancia administrada. Deje el campo **Proceso y almacenamiento** con el valor predeterminado. 
    1. En **Cuenta de administrador** proporcione un inicio de sesión de administrador (como `azureuser`) y una contraseña de administrador compleja. 

    ![Creación de una instancia administrada principal](media/sql-database-managed-instance-failover-group-tutorial/primary-sql-mi-values.png)

1. Deje el resto de la configuración en sus valores predeterminados y seleccione **Revisar y crear** para revisar la configuración de la instancia administrada. 
1. Seleccione **Crear** para crear la instancia administrada principal. 


## <a name="2---create-a-virtual-network"></a>2\. Creación de una red virtual
En este paso, creará una red virtual para la instancia administrada secundaria. Este paso es necesario porque un requisito establece que la subred de las instancias administradas principales y secundarias debe tener intervalos de direcciones que no se superpongan. 

Para comprobar el intervalo de subred de la red virtual principal, siga estos pasos:
1. En [Azure Portal](https://portal.azure.com), vaya a su grupo de recursos y seleccione la red virtual para la instancia principal. 
1. Seleccione **Subredes** en **Configuración** y anote el **Intervalo de direcciones**. El intervalo de direcciones de subred perteneciente a la red virtual para la instancia administrada secundaria no puede superponerse con el intervalo anotado. 


   ![Subred principal](media/sql-database-managed-instance-failover-group-tutorial/verify-primary-subnet-range.png)

Para crear una red virtual, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** y busque *red virtual*. 
1. Seleccione la opción **Red virtual** publicada por Microsoft y, a continuación, seleccione **Crear** en la página siguiente. 
1. Rellene los campos obligatorios para configurar la red virtual para la instancia administrada secundaria y después seleccione **Crear**. 

   En la tabla siguiente se muestran los valores necesarios para la red virtual secundaria:

    | **Campo** | Valor |
    | --- | --- |
    | **Nombre** |  Nombre de la red virtual que va a usar la instancia administrada secundaria, como `vnet-sql-mi-secondary`. |
    | **Espacio de direcciones** | Espacio de direcciones de la red virtual, como `10.128.0.0/16`. | 
    | **Suscripción** | Suscripción en la que residen la instancia administrada principal y el grupo de recursos. |
    | **Región** | Ubicación en la que implementará la instancia administrada secundaria. |
    | **Subred** | Nombre de la subred. De forma predeterminada, se proporciona `default`. |
    | **Intervalo de direcciones**| Intervalo de direcciones para la subred. Debe ser distinto al intervalo de direcciones de subred que usó la red virtual de la instancia administrada principal, como `10.128.0.0/24`.  |
    | &nbsp; | &nbsp; |

    ![Valores de red virtual secundaria](media/sql-database-managed-instance-failover-group-tutorial/secondary-virtual-network.png)


## <a name="3---create-a-secondary-managed-instance"></a>3\. Creación de una instancia administrada secundaria
En este paso, creará una instancia administrada secundaria en Azure Portal, que también configurará las redes entre las dos instancias administradas. 

La segunda instancia administrada debe:
- Estar vacía. 
- Tener una subred y un intervalo de direcciones IP distintos de los de la instancia administrada principal. 

Para crear la instancia administrada secundaria, siga estos pasos: 

1. Seleccione **Azure SQL** en el menú izquierdo de Azure Portal. Si **Azure SQL** no está en la lista, seleccione **Todos los servicios** y escriba Azure SQL en el cuadro de búsqueda. (Opcional) Seleccione la estrella junto a **Azure SQL** para marcarlo como favorito y agréguelo como un elemento en el panel de navegación izquierdo. 
1. Seleccione **+ Agregar** para abrir la página **Select SQL deployment option** (Seleccionar la opción de implementación de SQL). Para ver más información sobre las distintas bases de datos, seleccione Mostrar detalles en el icono Bases de datos.
1. Seleccione **Crear** en el icono **Instancias administradas de SQL**. 

    ![Selección de la instancia administrada](media/sql-database-managed-instance-failover-group-tutorial/select-managed-instance.png)

1. En la pestaña **Aspectos básicos** de la página **Crear instancia administrada de Azure SQL Database**, rellene los campos obligatorios para configurar la instancia administrada secundaria. 

   En la tabla siguiente se muestran los valores necesarios para la instancia administrada secundaria:
 
    | **Campo** | Valor |
    | --- | --- |
    | **Suscripción** |  Suscripción en la que reside la instancia administrada principal. |
    | **Grupos de recursos**| Grupo de recursos en el que reside la instancia administrada principal. |
    | **Nombre de la instancia administrada** | Nombre de la nueva instancia administrada secundaria, como `sql-mi-secondary`  | 
    | **Región**| Ubicación de la instancia administrada secundaria.  |
    | **Inicio de sesión de administrador de la instancia administrada** | Inicio de sesión que quiere usar para la nueva instancia administrada secundaria, como `azureuser`. |
    | **Contraseña** | Contraseña compleja que usará el inicio de sesión del administrador para la nueva instancia administrada secundaria.  |
    | &nbsp; | &nbsp; |

1. En la pestaña **Redes**, como **Red virtual**, seleccione la red virtual que creó para la instancia administrada secundaria de la lista desplegable.

   ![Redes de instancia administrada secundaria](media/sql-database-managed-instance-failover-group-tutorial/networking-settings-for-secondary-mi.png)

1. En la pestaña **Configuración adicional**, para la **Replicación geográfica** elija **Sí** en _Use as failover secondary_ (Usar como conmutación por error secundaria). Seleccione la instancia administrada principal en la lista desplegable. 
    1. Asegúrese de que la intercalación y la zona horaria coinciden con las de la instancia administrada principal. La instancia administrada principal creada en este tutorial usó el valor predeterminado de intercalación `SQL_Latin1_General_CP1_CI_AS` y la zona horaria `(UTC) Coordinated Universal Time`. 

   ![Redes de instancia administrada secundaria](media/sql-database-managed-instance-failover-group-tutorial/secondary-mi-failover.png)

1. Seleccione **Revisar y crear** para revisar la configuración de la instancia administrada secundaria. 
1. Seleccione **Crear** para crear la instancia administrada secundaria. 


## <a name="4---create-primary-virtual-network-gateway"></a>4\. Creación de la puerta de enlace de red virtual 

Para que dos instancias administradas participen en un grupo de conmutación por error, debe haber una puerta de enlace configurada entre las redes virtuales de las dos instancias administradas para permitir la comunicación en red. Puede crear la puerta de enlace para la instancia administrada principal mediante Azure Portal:

1. En [Azure Portal](https://portal.azure.com), navegue al grupo de recursos y seleccione el recurso **Red virtual** para la instancia administrada principal. 
1. Seleccione **Subredes** en **Configuración** y, a continuación, seleccione para agregar una nueva **Subred de puerta de enlace**. Deje los valores predeterminados. 

   ![Adición de una puerta de enlace para la instancia administrada principal](media/sql-database-managed-instance-failover-group-tutorial/add-subnet-gateway-primary-vnet.png)

1. Una vez creada la puerta de enlace de subred, seleccione **Crear un recurso** en el panel de navegación izquierdo y, a continuación, escriba `Virtual network gateway` en el cuadro de búsqueda. Seleccione el recurso **Puerta de enlace de red virtual** publicado por **Microsoft**. 

   ![Creación de una nueva puerta de enlace de red virtual](media/sql-database-managed-instance-failover-group-tutorial/create-virtual-network-gateway.png)

1. Rellene los campos obligatorios para configurar la puerta de enlace de la instancia administrada principal. 

   En la tabla siguiente se muestran los valores necesarios para la puerta de enlace de la instancia administrada principal:
 
    | **Campo** | Valor |
    | --- | --- |
    | **Suscripción** |  Suscripción en la que reside la instancia administrada principal. |
    | **Nombre** | Nombre de la puerta de enlace de red virtual, como `primary-mi-gateway`. | 
    | **Región** | Región donde se encuentra la instancia administrada secundaria. |
    | **Tipo de puerta de enlace** | Seleccione **VPN**. |
    | **Tipo de VPN** | Seleccione **Basada en rutas**. |
    | **SKU**| Deje el valor predeterminado de `VpnGw1`. |
    | **Ubicación**| Ubicación donde se encuentran la instancia administrada principal y la red virtual principal.   |
    | **Red virtual**| Seleccione la red virtual que se creó en la sección 2, como `vnet-sql-mi-primary`. |
    | **Dirección IP pública**| Seleccione **Crear nuevo**. |
    | **Nombre de la dirección IP pública**| Escriba un nombre para la dirección IP, como `primary-gateway-IP`. |
    | &nbsp; | &nbsp; |
1. Deje los demás valores predeterminados y, a continuación, seleccione **Revisar y crear** para revisar la configuración de la puerta de enlace de red virtual.

   ![Configuración de la puerta de enlace principal](media/sql-database-managed-instance-failover-group-tutorial/settings-for-primary-gateway.png)

1. Seleccione **Crear** para crear la nueva puerta de enlace de red virtual. 

## <a name="5---configure-secondary-virtual-network-gateway"></a>5\. Configuración de una puerta de enlace de red virtual secundaria 

Repita los pasos de la sección anterior para crear la subred y la puerta de enlace de la red virtual para la instancia administrada secundaria. Rellene los campos obligatorios para configurar la puerta de enlace para la instancia administrada secundaria. 

   En la tabla siguiente se muestran los valores necesarios para la puerta de enlace de la instancia administrada secundaria:

   | **Campo** | Valor |
   | --- | --- |
   | **Suscripción** |  Suscripción en la que reside la instancia administrada secundaria. |
   | **Nombre** | Nombre de la puerta de enlace de red virtual, como `secondary-mi-gateway`. | 
   | **Región** | Región donde se encuentra la instancia administrada secundaria. |
   | **Tipo de puerta de enlace** | Seleccione **VPN**. |
   | **Tipo de VPN** | Seleccione **Basada en rutas**. |
   | **SKU**| Deje el valor predeterminado de `VpnGw1`. |
   | **Ubicación**| Ubicación donde se encuentran la instancia administrada secundaria y la red virtual secundaria.   |
   | **Red virtual**| Seleccione la red virtual que se creó en la sección 2, como `vnet-sql-mi-secondary`. |
   | **Dirección IP pública**| Seleccione **Crear nuevo**. |
   | **Nombre de la dirección IP pública**| Escriba un nombre para la dirección IP, como `secondary-gateway-IP`. |
   | &nbsp; | &nbsp; |

   ![Configuración de la puerta de enlace secundaria](media/sql-database-managed-instance-failover-group-tutorial/settings-for-secondary-gateway.png)

## <a name="6---connect-the-gateways"></a>6\. Conexión de las puertas de enlace
En este paso, se creará una conexión entre las puertas de enlace. Se debe establecer una conexión de la puerta de enlace primaria a la secundaria. Después, se debe establecer una conexión independiente de la puerta de enlace secundaria a la principal. Asegúrese de usar la misma **clave compartida** al configurar la conectividad entre ambas puertas de enlace. 

Para configurar la conectividad, siga estos pasos:

1. Navegue hasta el grupo de recursos en [Azure Portal](https://portal.azure.com) y seleccione la puerta de enlace principal que creó en el paso 4. 
1. Seleccione **Conexiones** en **Configuración** y luego **Agregar** para crear una nueva conexión. 

   ![Adición de una conexión a la puerta de enlace principal](media/sql-database-managed-instance-failover-group-tutorial/add-primary-gateway-connection.png)

1. Escriba un nombre para la conexión, como `Primary-connection`, y escriba un valor para la **clave compartida**, como `mi1mi2psk`. 
1. Seleccione la **segunda puerta de enlace de red virtual** y, a continuación, seleccione la puerta de enlace para la instancia administrada secundaria, como `secondary-mi-gateway`. 

   ![Creación de la conexión principal a secundaria](media/sql-database-managed-instance-failover-group-tutorial/create-primary-to-secondary-connection.png)

1. Seleccione **Aceptar** para agregar la nueva conexión de la puerta de enlace principal a la secundaria.
1. Repita estos pasos para crear una conexión desde la puerta de enlace de la instancia administrada secundaria a la puerta de enlace de la instancia administrada principal. 

   ![Creación de la conexión secundaria a principal](media/sql-database-managed-instance-failover-group-tutorial/create-secondary-to-primary-connection.png)


## <a name="7---create-a-failover-group"></a>7\. Creación de un grupo de conmutación por error
En este paso, creará el grupo de conmutación por error y le agregará ambas instancias administradas. 

1. Seleccione **Azure SQL** en el menú izquierdo de [Azure Portal](https://portal.azure.com). Si **Azure SQL** no está en la lista, seleccione **Todos los servicios** y escriba Azure SQL en el cuadro de búsqueda. (Opcional) Seleccione la estrella junto a **Azure SQL** para marcarlo como favorito y agréguelo como un elemento en el panel de navegación izquierdo. 
1. Seleccione la instancia administrada principal que creó en la primera sección, por ejemplo, `sql-mi-primary`. 
1. En **Configuración**, navegue a **Grupos de conmutación por error de instancias** y, después, elija **Agregar grupo** para abrir la página **Grupo de conmutación por error de instancias**. 

   ![Adición de un grupo de conmutación por error](media/sql-database-managed-instance-failover-group-tutorial/add-failover-group.png)

1. En la página **Grupo de conmutación por error de instancias**, escriba el nombre del grupo de conmutación por error, como `failovergrouptutorial`, y después elija la instancia administrada secundaria, como `sql-mi-secondary`, en la lista desplegable. Seleccione **Crear** para crear el grupo de conmutación por error. 

   ![Creación de un grupo de conmutación por error](media/sql-database-managed-instance-failover-group-tutorial/create-failover-group.png)

1. Una vez completada la implementación del grupo de conmutación por error, volverá a mostrarse la página **Grupo de conmutación por error**. 

## <a name="8---test-failover"></a>8\. Prueba de la conmutación por error
En este paso, se producirá un error en el grupo de conmutación por error en el servidor secundario y, a continuación, se realizará la conmutación por recuperación mediante Azure Portal. 

1. Navegue a la instancia administrada en [Azure Portal](https://portal.azure.com) y seleccione **Grupos de conmutación por error de instancias** en las opciones de configuración. 
1. Revise cuál instancia administrada es la principal y cuál es la secundaria. 
1. Seleccione **Conmutación por error** y, a continuación, seleccione **Sí** en la advertencia acerca de la desconexión de las sesiones TDS. 

   ![Conmutación por error del grupo de conmutación por error](media/sql-database-managed-instance-failover-group-tutorial/failover-mi-failover-group.png)

1. Revise cuál instancia administrada es la principal y cuál la secundaria. Si la conmutación por error se realiza correctamente, las dos instancias deben tener los roles cambiados. 

   ![Las instancias administradas cambian roles después de la conmutación por error](media/sql-database-managed-instance-failover-group-tutorial/mi-switched-after-failover.png)

1. Vuelva a seleccionar **Conmutación por error** para que la instancia principal vuelva a tener el rol principal. 


## <a name="clean-up-resources"></a>Limpieza de recursos
Para limpiar los recursos, elimine primero la instancia administrada, después el clúster virtual, a continuación el resto de los recursos y, por último, el grupo de recursos. 

1. Vaya a su grupo de recursos en [Azure Portal](https://portal.azure.com). 
1. Seleccione la instancia administrada y después seleccione **Eliminar**: Escriba `yes` en el cuadro de texto para confirmar que quiere eliminar el recurso y después seleccione **Eliminar**. Este proceso puede tardar algún tiempo en completarse en segundo plano y, hasta que finalice, no podrá eliminar el *Clúster virtual* ni ningún otro recurso dependiente. Supervise la eliminación en la pestaña Actividad para confirmar que la instancia administrada se ha eliminado. 
1. Una vez que se elimine la instancia administrada, puede eliminar el *Clúster virtual* al seleccionarlo en el grupo de recursos y elegir **Eliminar**. Escriba `yes` en el cuadro de texto para confirmar que quiere eliminar el recurso y después seleccione **Eliminar**. 
1. Elimine todos los recursos restantes. Escriba `yes` en el cuadro de texto para confirmar que quiere eliminar el recurso y después seleccione **Eliminar**. 
1. Para eliminar el grupo de recursos, seleccione **Eliminar grupo de recursos**, escriba el nombre del grupo de recursos (`myResourceGroup`) y, a continuación, seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, configuró un grupo de conmutación por error entre dos instancias administradas. Ha aprendido a:

> [!div class="checklist"]
> - Crear una instancia administrada principal
> - Crear una instancia administrada secundaria como parte de [un grupo de conmutación por error](sql-database-auto-failover-group.md). 
> - Conmutación por error de prueba

Continúe al siguiente inicio rápido sobre cómo conectarse a la instancia administrada y cómo restaurar una base de datos en la instancia administrada: 

> [!div class="nextstepaction"]
> [Conectarse a la instancia administrada](sql-database-managed-instance-configure-vm.md)
> [Restaurar una base de datos en una instancia administrada](sql-database-managed-instance-get-started-restore.md)


