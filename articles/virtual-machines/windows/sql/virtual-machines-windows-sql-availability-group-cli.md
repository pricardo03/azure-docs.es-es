---
title: Usar la CLI de máquina virtual de Azure SQL para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure
description: 'Use la CLI de Azure para crear el clúster de conmutación por error de Windows, la escucha del grupo de disponibilidad y la instancia de Load Balancer interno en una máquina virtual de SQL Server en Azure. '
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 5efbe874bbf3c1c4081eb7a2c76c1be5a3358ec8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65518978"
---
# <a name="use-azure-sql-vm-cli-to-configure-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Usar la CLI de máquina virtual de Azure SQL para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure
En este artículo se describe cómo usar la [CLI de máquina virtual de Azure SQL](/cli/azure/sql/vm?view=azure-cli-latest/) para implementar un clúster de conmutación por error de Windows (WSFC) y agregar a él máquinas virtuales de SQL Server, así como crear la instancia de Load Balancer interno y la escucha de un grupo de disponibilidad Always On.  La implementación real del grupo de disponibilidad Always On se sigue realizando manualmente a través de SQL Server Management Studio (SSMS). 

## <a name="prerequisites"></a>Requisitos previos
Para automatizar la configuración de un grupo de disponibilidad Always On mediante la CLI de máquina virtual de Azure SQL, ya debe tener los siguientes requisitos previos: 
- Una [suscripción a Azure](https://azure.microsoft.com/free/).
- Un grupo de recursos con un controlador de dominio. 
- Una o varias [máquinas virtuales en Azure, unidas a un dominio, con SQL Server 2016 (o superior) Enterprise Edition](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) en el *mismo conjunto o zona de disponibilidad o en otro distinto* que se haya [registrado con el proveedor de recursos de máquina virtual de SQL](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).  
- [Azure CLI](/cli/azure/install-azure-cli). 
- Dos direcciones IP disponibles (que no use ninguna entidad), una para la instancia de Load Balancer interno y otra para la escucha del grupo de disponibilidad, dentro de la misma subred que el grupo de disponibilidad. Si se utiliza un equilibrador de carga existente, solo será necesaria una dirección IP disponible para la escucha del grupo de disponibilidad. 

## <a name="permissions"></a>Permisos
Los siguientes permisos de cuenta son necesarios para configurar el grupo de disponibilidad Always On mediante la CLI de máquina virtual de Azure SQL. 

- Una cuenta de usuario de dominio existente que tenga permiso para crear objetos de equipo en el dominio.  Por ejemplo, una cuenta de administrador de dominio normalmente tiene permisos suficientes (p. ej.: account@domain.com). _Esta cuenta también debe formar parte del grupo de administradores local en cada máquina virtual para crear el clúster._
- La cuenta de usuario del dominio que controla el servicio SQL Server. 
 
## <a name="step-1---create-storage-account-as-a-cloud-witness"></a>Paso 1: Crear una cuenta de almacenamiento como testigo en la nube
El clúster necesita una cuenta de almacenamiento para que actúe como el testigo en la nube. Puede usar una cuenta de almacenamiento existente o crear una desde cero. Si quiere utilizar una cuenta de almacenamiento existente, avance a la siguiente sección. 

Con el siguiente fragmento de código se crea una cuenta de almacenamiento: 
```azurecli-interactive
# Create the storage account
# example: az storage account create -n 'cloudwitness' -g SQLVM-RG -l 'West US' `
#  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true

az storage account create -n <name> -g <resource group name> -l <region ex:eastus> `
  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true
```

   >[!TIP]
   > Puede aparecer el error `az sql: 'vm' is not in the 'az sql' command group` si usa una versión obsoleta de la CLI de Azure. Descargue la [versión más reciente de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) para solucionar este error.

## <a name="step-2---define-windows-failover-cluster-metadata"></a>Paso 2: Definir los metadatos del clúster de conmutación por error de Windows
El grupo de comandos [az sql vm group](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest) de la CLI de máquina virtual de Azure SQL administra los metadatos del servicio de clúster de conmutación por error de Windows (WSFC) que hospedan el grupo de disponibilidad. Los metadatos del clúster engloban el dominio de Active Directory, las cuentas de clúster, las cuentas de almacenamiento que se van a usar como testigo en la nube y la versión de SQL Server. Use [az sql vm group create](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest#az-sql-vm-group-create) para definir los metadatos del WSFC de forma que, cuando se agregue la primera máquina virtual de SQL Server, el clúster se cree tal como esté establecido. 

Con el siguiente fragmento de código se definen los metadatos del clúster:
```azurecli-interactive
# Define the cluster metadata
# example: az sql vm group create -n Cluster -l 'West US' -g SQLVM-RG `
#  --image-offer SQL2017-WS2016 --image-sku Enterprise --domain-fqdn domain.com `
#  --operator-acc vmadmin@domain.com --bootstrap-acc vmadmin@domain.com --service-acc sqlservice@domain.com `
#  --sa-key '4Z4/i1Dn8/bpbseyWX' `
#  --storage-account 'https://cloudwitness.blob.core.windows.net/'

az sql vm group create -n <cluster name> -l <region ex:eastus> -g <resource group name> `
  --image-offer <SQL2016-WS2016 or SQL2017-WS2016> --image-sku Enterprise --domain-fqdn <FQDN ex: domain.com> `
  --operator-acc <domain account ex: testop@domain.com> --bootstrap-acc <domain account ex:bootacc@domain.com> `
  --service-acc <service account ex: testservice@domain.com> `
  --sa-key '<PublicKey>' `
  --storage-account '<ex:https://cloudwitness.blob.core.windows.net/>'
```

## <a name="step-3---add-sql-server-vms-to-cluster"></a>Paso 3: Agregar máquinas virtuales de SQL Server al clúster
Al agregar la primera máquina virtual de SQL Server al clúster, se crea el clúster. El comando [az sql vm add-to-group](https://docs.microsoft.com/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-add-to-group) crea el clúster con el nombre que se le haya dado, instala el rol de clúster en las máquinas virtuales de SQL Server y las agrega al clúster. Los usos posteriores del comando `az sql vm add-to-group` hacen que se agreguen más máquinas virtuales de SQL Server al clúster recién creado. 

Con el siguiente fragmento de código se crea el clúster y se agrega a él la primera máquina virtual de SQL Server: 

```azurecli-interactive
# Add SQL Server VMs to cluster
# example: az sql vm add-to-group -n SQLVM1 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!
# example: az sql vm add-to-group -n SQLVM2 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

az sql vm add-to-group -n <VM1 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
az sql vm add-to-group -n <VM2 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
```
Use este comando para agregar al clúster cualquier otra máquina virtual de SQL Server, modificando únicamente el parámetro `-n` en el nombre de la máquina virtual de SQL Server. 

## <a name="step-4---create-availability-group"></a>Paso 4: Crear el grupo de disponibilidad
Cree el grupo de disponibilidad manualmente del modo habitual, ya sea mediante [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell) o [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

  >[!IMPORTANT]
  > **No** cree una escucha en este momento, ya que esto se realiza a través de la CLI de Azure en las secciones siguientes.  

## <a name="step-5---create-internal-load-balancer"></a>Paso 5: Crear la instancia de Load Balancer interno

La escucha de grupo de disponibilidad Always On requiere una instancia de Azure Load Balancer interno (ILB). El ILB proporciona una dirección IP "flotante" para la escucha de grupo de disponibilidad que permite conmutar por error y volver a conectarse de manera más rápida. Si las máquinas virtuales con SQL Server de un grupo de disponibilidad forman parte del mismo conjunto de disponibilidad, puede usar una instancia de Load Balancer básico; en caso contrario, deberá usar Standard Load Balancer.  **El ILB debe estar en la misma red virtual que las instancias de VM con SQL Server.** 

Con el siguiente fragmento de código se crea la instancia de Load Balancer interno:

```azurecli-interactive
# Create the Internal Load Balancer
# example: az network lb create --name sqlILB -g SQLVM-RG --sku Standard `
# --vnet-name SQLVMvNet --subnet default

az network lb create --name sqlILB -g <resource group name> --sku Standard `
  --vnet-name <VNet Name> --subnet <subnet name>
```

  >[!IMPORTANT]
  > El recurso de IP pública de cada VM con SQL Server debe tener una SKU estándar para que sea compatible con Standard Load Balancer. Para determinar la SKU del recurso de IP pública de la máquina virtual, vaya a su **grupo de recursos**, seleccione su recurso de **dirección IP pública** para la VM con SQL Server deseada y busque el valor que aparece debajo de **SKU** en el panel **Información general**.  

## <a name="step-6---create-availability-group-listener"></a>Paso 6: Crear la escucha del grupo de disponibilidad
Después de haber creado el grupo de disponibilidad manualmente, puede crear la escucha mediante [az sql vm ag-listener](/cli/azure/sql/vm/group/ag-listener?view=azure-cli-latest#az-sql-vm-group-ag-listener-create). 


- El **identificador de recurso de subred** es el valor de `/subnets/<subnetname>` anexado al identificador de recurso del recurso de red virtual. Para detectar el identificador de recurso de subred, realice lo siguiente:
   1. Vaya a su grupo de recursos en [Azure Portal](https://portal.azure.com). 
   1. Seleccione el recurso de red virtual. 
   1. Seleccione **Propiedades** en el panel **Configuración**. 
   1. Encuentre el identificador de recurso de la red virtual y anéxele `/subnets/<subnetname>` al final para crear el identificador de recurso de subred. Por ejemplo:
        - Mi identificador de recurso de red virtual es: `/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet`
        - Mi nombre de subred es `default`.
        - Por lo tanto, mi identificador de recurso de subred es: `/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default`.


Con el siguiente fragmento de código se creará la escucha del grupo de disponibilidad:

```azurecli-interactive
# Create the AG listener
# example: az sql vm group ag-listener create -n AGListener -g SQLVM-RG `
#  --ag-name SQLAG --group-name Cluster --ip-address 10.0.0.27 `
#  --load-balancer sqlilb --probe-port 59999  `
#  --subnet /subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#  --sqlvms sqlvm1 sqlvm2

az sql vm group ag-listener create -n <listener name> -g <resource group name> `
  --ag-name <availability group name> --group-name <cluster name> --ip-address <ag listener IP address> `
  --load-balancer <lbname> --probe-port <Load Balancer probe port, default 59999>  `
  --subnet <subnet resource id> `
  --sqlvms <names of SQL VM's hosting AG replicas ex: sqlvm1 sqlvm2>
```

## <a name="modify-number-of-replicas-in-availability-group"></a>Modificar el número de réplicas en el grupo de disponibilidad
Implementar un grupo de disponibilidad en máquinas virtuales de SQL Server hospedadas en Azure supone una dificultad añadida, ya que ahora los recursos se administran mediante el proveedor de recursos y mediante `virtual machine group`. En este sentido, al agregar o quitar réplicas en el grupo de disponibilidad, hay un paso más consistente en actualizar los metadatos de la escucha con información relativa a las máquinas virtuales de SQL Server. Por lo tanto, cuando se modifica el número de réplicas del grupo de disponibilidad, hay que usar también el comando [az sql vm group ag-listener update](/cli/azure/sql/vm/group/ag-listener?view=azure-cli-2018-03-01-hybrid#az-sql-vm-group-ag-listener-update) para actualizar la escucha con los metadatos de las máquinas virtuales de SQL Server. 


### <a name="add-a-replica"></a>Adición de una réplica

Para agregar una réplica nueva al grupo de disponibilidad, haga lo siguiente:

1. Agregar la máquina virtual de SQL Server al clúster:
   ```azurecli-interactive
   # Add SQL Server VM to the Cluster
   # example: az sql vm add-to-group -n SQLVM3 -g SQLVM-RG --sqlvm-group Cluster `
   # -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

   az sql vm add-to-group -n <VM3 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
   -b <bootstrap account password> -p <operator account password> -s <service account password>
   ```
1. Use SQL Server Management Studio (SSMS) para agregar la instancia de SQL Server como una réplica dentro del grupo de disponibilidad.
1. Agregue a la escucha los metadatos de la máquina virtual de SQL Server:
   ```azurecli-interactive
   # Update the listener metadata with the new VM
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2 sqlvm3

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs, along with new SQL VM>
   ```

### <a name="remove-a-replica"></a>Eliminación de una réplica

Para quitar una réplica del grupo de disponibilidad, haga lo siguiente:

1. Quite la réplica del grupo de disponibilidad mediante SQL Server Management Studio (SSMS). 
1. Quite los metadatos de máquina virtual de SQL Server de la escucha:
   ```azurecli-interactive
   # Update the listener metadata by removing the VM from the SQLVMs list
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs that remain>
   ```
1. Quite la máquina virtual de SQL Server del clúster:
   ```azurecli-interactive
   # Remove SQL VM from cluster
   # example: az sql vm remove-from-group --name SQLVM3 --resource-group SQLVM-RG

   az sql vm remove-from-group --name <SQL VM name> --resource-group <RG name> 
   ```

## <a name="remove-availability-group-listener"></a>Eliminación de la escucha de grupo de disponibilidad
Si, posteriormente, necesita quitar la escucha de grupo de disponibilidad configurada con la CLI de Azure, deberá pasar por el proveedor de recursos de máquina virtual de SQL. Puesto que la escucha se registra mediante este proveedor, no basta con eliminarla mediante SQL Server Management Studio. Realmente, se debe eliminar a través de dicho proveedor, pero usando la CLI de Azure. De esta manera, se eliminan los metadatos de la escucha de grupo de disponibilidad del proveedor y se elimina físicamente la escucha del grupo de disponibilidad. 

El fragmento de código siguiente elimina la escucha de grupo de disponibilidad de SQL del proveedor de recursos de SQL y del grupo de disponibilidad: 

```azurecli-interactive
# Remove the AG listener
# example: az sql vm group ag-listener delete --group-name Cluster --name AGListener --resource-group SQLVM-RG

az sql vm group ag-listener delete --group-name <cluster name> --name <listener name > --resource-group <resource group name>
```

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas frecuentes sobre las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [Notas de la versión de las máquinas virtuales con SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Cambio de los modelos de licencia para una VM con SQL Server](virtual-machines-windows-sql-ahb.md)
* [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)   
* [Configurar una instancia del servidor para admitir grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server)   
* [Administración de un grupo de disponibilidad &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server)   
* [Herramientas para supervisar grupos de disponibilidad Always On &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server)
* [Instrucciones Transact-SQL para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups)   
* [Información general de los cmdlets de PowerShell para grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](/sql/database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server)  
