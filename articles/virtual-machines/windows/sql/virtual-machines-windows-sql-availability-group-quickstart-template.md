---
title: Uso de plantillas de inicio rápido de Azure para crear un clúster WSFC y una escucha y configurar ILB para un grupo de disponibilidad Always On en una VM con SQL Server
description: Use plantillas de inicio rápido de Azure para simplificar el proceso de creación de grupos de disponibilidad para máquinas virtuales con SQL Server en Azure y crear el clúster, unir a él máquinas virtuales SQL, crear la escucha y configurar el ILB.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 9be8717bc9b1d15a59486edf206dd0657a711c06
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360560"
---
# <a name="create-wsfc-listener-and-configure-ilb-for-an-always-on-availability-group-on-a-sql-server-vm-with-azure-quickstart-template"></a>Uso de plantillas de inicio rápido de Azure para crear un clúster WSFC y una escucha y configurar ILB para un grupo de disponibilidad Always On en una VM con SQL Server
En este artículo se describe cómo usar las plantillas de inicio rápido de Azure para automatizar parcialmente la implementación de una configuración de grupo de disponibilidad Always On para SQL Server Virtual Machines en Azure. Son dos las plantillas de inicio rápido de Azure que se usan en este proceso. 

   | Plantilla | DESCRIPCIÓN |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Crea el clúster de conmutación por error de Windows y une a él las máquinas virtuales con SQL Server. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | Crea la escucha de grupo de disponibilidad y configura la instancia de Load Balancer interno. |
   | &nbsp; | &nbsp; |

Otras partes de la configuración del grupo de disponibilidad deben realizarse manualmente, como la creación del grupo de disponibilidad y de la instancia de Load Balancer interno. En este artículo se proporciona la secuencia de pasos manuales y automatizados.
 

## <a name="prerequisites"></a>Requisitos previos 
Para automatizar la configuración de un grupo de disponibilidad Always On mediante plantillas de inicio rápido, ya debe tener los siguientes requisitos previos: 
- Una [suscripción a Azure](https://azure.microsoft.com/free/).
- Un grupo de recursos con un [controlador de dominio](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-forest). 
- Una o varias [máquinas virtuales en Azure, unidas a un dominio, con SQL Server 2016 (o superior) Enterprise Edition](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) en el mismo conjunto o zona de disponibilidad que se hayan [registrado con el proveedor de recursos de máquina virtual de SQL](#register-existing-sql-vm-with-new-resource-provider).  

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Registro de máquinas virtuales de SQL existentes con el nuevo proveedor de recursos
Dado que estas plantillas de inicio rápido de Azure de grupo de disponibilidad se basan en el proveedor de recursos de máquina virtual de Azure (Microsoft.SqlVirtualMachine), las máquinas virtuales con SQL Server existentes deben registrarse con el proveedor de recursos de máquina virtual de SQL. Si ha creado la VM con SQL Server después de diciembre de 2018, omita este paso ya que todas las máquinas virtuales con SQL Server creadas con posterioridad a esta fecha se registran automáticamente. En esta sección se proporcionan los pasos necesarios para registrarse con el proveedor mediante Azure Portal, pero también puede usar [PowerShell](virtual-machines-windows-sql-ahb.md#powershell). 

  >[!IMPORTANT]
  > Si quita el recurso de VM con SQL Server, volverá a la configuración de licencia codificada de forma rígida de la imagen. 

1. Abra Azure Portal y vaya a **Todos los servicios**. 
1. Vaya a **Suscripciones** y seleccione la suscripción que le interese.  
1. En la hoja **Suscripciones**, vaya a **Proveedor de recursos**. 
1. Escriba `sql` en el filtro para mostrar los proveedores de recursos relacionados con SQL. 
1. Seleccione *Registrar*, *Volver a registrar* o *Anular registro* para el proveedor **Microsoft.SqlVirtualMachine**, según la acción que quiera realizar. 

  ![Modificación del proveedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>Paso 1: Creación del clúster WSFC y unión a él de las máquinas virtuales con SQL Server mediante una plantilla de inicio rápido 
Una vez que se han registrado sus máquinas virtuales con SQL Server con el nuevo proveedor de recursos de VM con SQL, puede unirlas a *SqlVirtualMachineGroup*. Este recurso define los metadatos del clúster de conmutación por error de Windows, lo que incluye la versión, la edición, el nombre de dominio completo, las cuentas de AD para administrar el clúster y la cuenta de almacenamiento como el testigo en la nube. Al agregar la VM con SQL Server a *SqlVirtualMachineGroup*, arranca el servicio de clúster de conmutación por error de Windows y se unen las máquinas virtuales con SQL Server al clúster. Este paso se automatiza con la plantilla de inicio rápido **101-sql-vm-ag-setup** y se puede implementar mediante estos pasos:

1. Vaya a la plantilla de inicio rápido [**101-sql-vm-ag-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) y seleccione **Deploy to Azure** (Implementar en Azure) para iniciar la plantilla de inicio rápido en Azure Portal.
1. Rellene los campos obligatorios para configurar los metadatos del clúster de conmutación por error de Windows. Los campos opcionales pueden dejarse en blanco.

    En la tabla siguiente se muestran los valores necesarios para la plantilla: 

   | **Campo** | Valor |
   | --- | --- |
   | **Suscripción** |  La suscripción donde se encuentran sus máquinas virtuales con SQL Server. |
   |**Grupos de recursos** | El grupo de recursos donde residen sus máquinas virtuales con SQL Server. | 
   |**Failover Cluster Name** (Nombre del clúster de conmutación por error) | El nombre deseado para el nuevo clúster de conmutación por error de Windows. |
   | **Existing Vm List** (Lista de máquinas virtuales existentes) | Las máquinas virtuales con SQL Server que quiere que participen en el grupo de disponibilidad y que, por lo tanto, formen parte de este nuevo clúster. Separe estos valores con una coma y un espacio (por ejemplo: SQLVM1, SQLVM2). |
   | **SQL Server Version** (Versión de SQL Server) | En la lista desplegable, seleccione la versión de SQL Server de las máquinas virtuales con SQL Server. Actualmente solo se admiten imágenes de SQL 2016 y 2017. |
   | **Existing Fully Qualified Domain Name** (Nombre de dominio completo existente) | El FQDN existente del dominio en el que residen sus máquinas virtuales con SQL Server. |
   | **Existing Domain Account** (Cuenta de dominio existente) | Una cuenta de dominio existente que tiene acceso de administrador del sistema a SQL Server. | 
   | **Domain Account Password** (Contraseña de la cuenta de dominio) | La contraseña de la cuenta de dominio mencionada anteriormente. | 
   | **Existing Sql Service Account** (Cuenta del servicio SQL existente) | La cuenta de usuario del dominio que se usa para controlar el servicio SQL Server. Para encontrar esta información, puede usar el [**Administrador de configuración de SQL Server**](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager?view=sql-server-2017). |
   | **Sql Service Password** (Contraseña del servicio SQL) | La contraseña usada por la cuenta de usuario del dominio que controla el servicio SQL Server. |
   | &nbsp; | &nbsp; |

1. Si acepta los términos y condiciones, active la casilla junto a **I Agree to the terms and conditions stated above** (Acepto los términos y condiciones indicados anteriormente) y seleccione **Purchase** (Comprar) para finalizar la implementación de la plantilla de inicio rápido. 
1. Para supervisar la implementación, seleccione la implementación en el icono de campana **Notificaciones** en el banner del panel de navegación izquierdo o vaya a su **grupo de recursos** en Azure Portal, seleccione **Implementaciones** en el campo **Configuración** y elija la implementación "Microsoft.Template". 

## <a name="step-2---manually-create-the-availability-group"></a>Paso 2: Creación del grupo de disponibilidad de forma manual 
Cree el grupo de disponibilidad como haría normalmente, mediante [PowerShell](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell?view=sql-server-2017), [SQL Server Management Studio](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-2017) o [Transact-SQL](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql?view=sql-server-2017). 

  >[!IMPORTANT]
  > **No** cree una escucha en este momento porque lo hace automáticamente la plantilla de inicio rápido **101-sql-vm-aglistener-setup** en el paso 4. 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>Paso 3: Creación de la instancia de Load Balancer interno (ILB) de forma manual
La escucha de grupo de disponibilidad (AG) Always On requiere una instancia de Azure Load Balancer interno (ILB). El ILB proporciona una dirección IP "flotante" para la escucha de grupo de disponibilidad que permite conmutar por error y volver a conectarse de manera más rápida. Si las máquinas virtuales con SQL Server de un grupo de disponibilidad forman parte del mismo conjunto de disponibilidad, puede usar una instancia de Load Balancer básico; en caso contrario, deberá usar Standard Load Balancer.  **El ILB debe estar en la misma red virtual que las instancias de VM con SQL Server.** Solo es necesario crear el ILB; el resto de la configuración (como las reglas para el grupo de servidores back-end, el sondeo de estado y el equilibrio de carga) se gestiona mediante la plantilla de inicio rápido **101-sql-vm-aglistener-setup** en el paso 4. 

1. En el Portal de Azure, abra el grupo de recursos que contiene las máquinas virtuales de SQL Server. 
2. En el grupo de recursos, haga clic en **Agregar**.
3. Busque el **equilibrador de carga** y, a continuación, en los resultados de la búsqueda, seleccione el **equilibrador de carga** publicado por **Microsoft**.
4. En la hoja **Load Balancer**, haga clic en **Crear**.
5. En el cuadro de diálogo **Crear equilibrador de carga**, configure el equilibrador de carga tal y como se explica a continuación:

   | Configuración | Valor |
   | --- | --- |
   | **Nombre** |Nombre de texto que representa el equilibrador de carga; Por ejemplo, **sqlLB**. |
   | **Tipo** |**Internas**: en la mayoría de las implementaciones se usa un equilibrador de carga interno que permite que las aplicaciones dentro de la misma red virtual se conecten al grupo de disponibilidad.  </br> **Externas**: permite que las aplicaciones se conecten al grupo de disponibilidad mediante una conexión a Internet pública. |
   | **Red virtual** |Seleccione la red virtual en la que se encuentran las instancias de SQL Server. |
   | **Subred** |Seleccione la subred en la que se encuentran las instancias de SQL Server. |
   | **Asignación de dirección IP** |**Estática** |
   | **Dirección IP privada** |Especifique una dirección IP disponible de la subred. Utilícela cuando cree un agente de escucha en el clúster.|
   | **Suscripción** |Si tiene varias suscripciones, puede aparecer este campo. Seleccione la suscripción que quiere asociar con este recurso. Normalmente, será la misma suscripción que se utiliza con todos los recursos del grupo de disponibilidad. |
   | **Grupos de recursos** |Seleccione el grupo de recursos en el que se encuentran las instancias de SQL Server. |
   | **Ubicación** |Seleccione la ubicación de Azure en la que se encuentran las instancias de SQL Server. |
   | &nbsp; | &nbsp; |

6. Seleccione **Crear**. 


  >[!NOTE]
  > El recurso de IP pública de cada VM con SQL Server debe tener una SKU estándar para que sea compatible con Standard Load Balancer. Para determinar la SKU del recurso de IP pública de la máquina virtual, vaya a su **grupo de recursos**, seleccione su recurso de **dirección IP pública** para la VM con SQL Server deseada y busque el valor que aparece debajo de **SKU** en el panel **Información general**. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>Paso 4: Creación de la escucha de grupo de disponibilidad y configuración del ILB con la plantilla de inicio rápido

Cree la escucha del grupo de disponibilidad y configure la instancia de Load Balancer interno (ILB) automáticamente con la plantilla de inicio rápido **101-sql-vm-aglistener-setup** que aprovisiona el recurso Microsoft.SqlVirtualMachine, Grupos de máquinas virtuales de SQL o Escucha de grupo de disponibilidad. La plantilla de inicio rápido  **101-sql-vm-aglistener-setup** realiza las siguientes acciones mediante el proveedor de recursos de máquina virtual con SQL:

 - Configura la red del clúster y del ILB. 
 - Configura las reglas para el grupo de servidores back-end, el sondeo de estado y el equilibrio de carga del ILB.
 - Crea la escucha de grupo de disponibilidad con el nombre y la dirección IP especificados.

Para configurar el ILB y crear la escucha de grupo de disponibilidad, haga lo siguiente:
1. Vaya a la plantilla de inicio rápido [**101-sql-vm-aglistener-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) y seleccione **Deploy to Azure** (Implementar en Azure) para iniciar la plantilla de inicio rápido en Azure Portal.
1. Rellene los campos obligatorios para configurar el ILB y crear la escucha de grupo de disponibilidad. Los campos opcionales pueden dejarse en blanco. 

    En la tabla siguiente se muestran los valores necesarios para la plantilla: 

   | **Campo** | Valor |
   | --- | --- |
   |**Grupos de recursos** | El grupo de recursos donde residen sus máquinas virtuales con SQL Server y el grupo de disponibilidad. | 
   |**Failover Cluster Name** (Nombre del clúster de conmutación por error) | El nombre del clúster al que se han unido sus máquinas virtuales con SQL Server. |
   | **Existing Sql Availability Group** (Grupo de disponibilidad de SQL existente)| El nombre del grupo de disponibilidad del que forman parte las máquinas virtuales con SQL Server. |
   | **Existing Vm List** (Lista de máquinas virtuales existentes) | Los nombres de las máquinas virtuales con SQL Server que forman parte del grupo de disponibilidad mencionado anteriormente. Los nombres deben estar separados por una coma y un espacio (por ejemplo: SQLVM1, SQLVM2). |
   | **Existing Fully Qualified Domain Name** (Nombre de dominio completo existente) | El FQDN existente del dominio en el que residen sus máquinas virtuales con SQL Server. |
   | **Agente de escucha** | El nombre DNS que desea asignar a la escucha. De forma predeterminada, esta plantilla especifica el nombre "aglistener", pero puede cambiarse. |
   | **Listener Port** (Puerto de escucha) | El puerto que quiere que use la escucha. Normalmente, este puerto debe ser el puerto predeterminado 1433 y, como tal, es el número de puerto especificado por esta plantilla. Sin embargo, si el puerto predeterminado ha cambiado, el puerto de escucha debe usar en su lugar ese valor. | 
   | **Existing Vnet** (Red virtual existente) | El nombre de la red virtual donde residen sus máquinas virtuales con SQL Server y el ILB. |
   | **Existing Subnet** (Subred existente) | El *nombre* de la subred interna de sus máquinas virtuales con SQL Server (p. ej.: el predeterminado). Para determinar este valor, vaya a su **grupo de recursos**, seleccione su **red virtual**, seleccione **Subredes** en el panel **Configuración** y copie el valor que aparece en **Nombre**. |
   | **Existing Internal Load Balancer** (Load Balancer interno existente) | El nombre del ILB que creó en el paso 3. |
   | **Puerto de sondeo** | El puerto de sondeo que quiere que use el ILB. La plantilla usa 59999 de forma predeterminada, pero este valor se puede cambiar. |
   | &nbsp; | &nbsp; |

1. Si acepta los términos y condiciones, active la casilla junto a **I Agree to the terms and conditions stated above** (Acepto los términos y condiciones indicados anteriormente) y seleccione **Purchase** (Comprar) para finalizar la implementación de la plantilla de inicio rápido. 
1. Para supervisar la implementación, seleccione la implementación en el icono de campana **Notificaciones** en el banner del panel de navegación izquierdo o vaya a su **grupo de recursos** en Azure Portal, seleccione **Implementaciones** en el campo **Configuración** y elija la implementación "Microsoft.Template". 

  >[!NOTE]
  >Si la implementación genera errores a mitad del proceso, deberá [quitar la escucha recién creada](#remove-availability-group-listener) de forma manual mediante PowerShell antes de volver a implementar la plantilla de inicio rápido **101-sql-vm-aglistener-setup**. 

## <a name="remove-availability-group-listener"></a>Eliminación de la escucha de grupo de disponibilidad
Si posteriormente necesita quitar la escucha de grupo de disponibilidad configurada por la plantilla, debe pasar por el proveedor de recursos de máquina virtual de SQL. Puesto que la escucha se registra mediante este proveedor, no basta con eliminarla mediante SQL Server Management Studio. En realidad se debe eliminar a través de él pero con PowerShell. De esta manera, se eliminan los metadatos de la escucha de grupo de disponibilidad del proveedor y se elimina físicamente la escucha del grupo de disponibilidad. 

El fragmento de código siguiente elimina la escucha de grupo de disponibilidad de SQL del proveedor de recursos de SQL y del grupo de disponibilidad: 

```PowerShell
# Remove the AG listener
# example: Remove-AzureRmResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzureRmResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="known-issues-and-errors"></a>Problemas y errores conocidos
En esta sección se describen algunos problemas conocidos y sus posibles soluciones. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>La escucha del grupo de disponibilidad "\<AG-Name>" ya existe
El grupo de disponibilidad seleccionado usado en la plantilla de inicio rápido de Azure de la escucha de grupo de disponibilidad ya contiene una escucha, o bien físicamente dentro del grupo de disponibilidad o como metadatos dentro del proveedor de recursos de máquina virtual de SQL.  Quite la escucha mediante [PowerShell](#remove-availability-group-listener) antes de volver a implementar la plantilla de inicio rápido **101-sql-vm-aglistener-setup**. 

### <a name="connection-only-works-from-primary-replica"></a>La conexión solo funciona desde la réplica principal
Este comportamiento puede deberse a una implementación incorrecta de la plantilla **101-sql-vm-aglistener-setup** que deja la configuración del ILB en un estado incoherente. Compruebe que el grupo de servidores back-end muestra el conjunto de disponibilidad y que existen reglas para el sondeo de estado y el equilibrio de carga. Si falta algo, la configuración del ILB se queda en un estado incoherente. 

Para resolver este comportamiento, quite la escucha mediante [PowerShell](#remove-availability-group-listener), elimine la instancia de Load Balancer interno mediante Azure Portal y comience de nuevo en el [paso 3](#step-3---manually-create-the-internal-load-balanced-ilb). 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest: solo se puede actualizar la lista de máquinas virtuales de SQL
Este error puede producirse al implementar la plantilla **101-sql-vm-aglistener-setup** si la escucha se eliminó mediante SQL Server Management Studio (SSMS), pero no se ha eliminado del proveedor de recursos de máquina virtual de SQL. Al eliminar la escucha mediante SSMS, no se quitan sus metadatos del proveedor de recursos de máquina virtual de SQL; para ello, es necesario usar [PowerShell](#remove-availability-group-listener). 


## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas frecuentes sobre las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [Guía de precios de las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Notas de la versión de las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Cambio de los modelos de licencia para una VM con SQL Server](virtual-machines-windows-sql-ahb.md)



