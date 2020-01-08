---
title: Administración de direcciones IP reservadas (clásica) de Azure | Microsoft Docs
description: Conozca las direcciones IP reservadas (clásica) y cómo administrarlas mediante Azure PowerShell y la CLI de Azure.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: genli
ms.openlocfilehash: c1de86c1d12109853bb5d6d1aac4143caab9199f
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75638228"
---
# <a name="reserved-ip-addresses-classic-deployment"></a>Direcciones IP reservadas (implementación clásica)

 Las direcciones IP en Azure se dividen en dos categorías: dinámicas y reservadas. Las direcciones IP públicas administradas por Azure son dinámicas de forma predeterminada. Esto significa que la dirección IP usada para un determinado servicio en la nube (VIP) o para tener acceso a una máquina virtual o instancia de rol directamente (ILPIP) puede cambiar de vez en cuando, cuando los recursos se cierran o detienen (desasignan).

Para impedir que cambien las direcciones IP, puede reservar una dirección IP. La IP reservadas únicamente puede usarse como una VIP, lo que garantiza que la dirección IP para el servicio en la nube siga siendo la misma incluso cuando se cierran o detienen (desasignan) recursos. Además, se puede convertir direcciones IP dinámicas existentes utilizadas como una VIP en una IP reservada.

> [!IMPORTANT]
> Azure tiene dos modelos de implementación diferentes para crear recursos y trabajar con ellos:  [Resource Manager y el clásico](../azure-resource-manager/resource-manager-deployment-model.md). Este artículo trata del modelo de implementación clásico. Microsoft recomienda que las implementaciones más recientes usen el modelo de Resource Manager. Obtenga información sobre cómo reservar una dirección IP pública estática con el [modelo de implementación de Resource Manager](virtual-network-ip-addresses-overview-arm.md).

Para más información sobre las direcciones IP en Azure, lea el artículo sobre [direcciones IP](virtual-network-ip-addresses-overview-classic.md).

## <a name="when-do-i-need-a-reserved-ip"></a>¿Cuándo se necesita una IP reservada?
* **Para asegurarse de que la dirección IP está reservada en su suscripción**. Si desea reservar una dirección IP que no se libera de su suscripción bajo ninguna circunstancia, debe usar una dirección IP pública reservada.  
* **Desea que su IP permanezca con el servicio en la nube incluso en el estado detenido o desasignado (máquina virtual)** . Si desea tener acceso al servicio mediante una dirección IP que no cambia incluso cuando las máquinas virtuales en el servicio en la nube se cierran o detienen (desasignan).
* **Desea estar seguro de que el tráfico saliente de Azure usa una dirección IP predecible**. Es posible que haya configurado un firewall local para permitir solo tráfico procedente de direcciones IP específicas. Al reservar una dirección IP, sabe la dirección IP de origen y no tiene que actualizar las reglas de firewall debido a un cambio de dirección IP.

## <a name="faqs"></a>Preguntas más frecuentes
- ¿Puedo usar una IP reservada para todos los servicios de Azure?
    No. Las IP reservadas solo pueden usarse para máquinas virtuales y roles de instancia del servicio en la nube que se hayan expuesto a través de una VIP.
- ¿Cuántas direcciones IP reservadas puedo tener?
    Para detalles, vea el artículo sobre [límites de Azure](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits).
- ¿Hay un cargo por las IP reservadas?
    A veces. Para detalles sobre los precios, consulte la página [Detalles de precios de las direcciones IP reservadas](https://go.microsoft.com/fwlink/?LinkID=398482).
- ¿Cómo se reserva una dirección IP?
    Puede usar PowerShell, la [API de REST de administración de Azure Management](https://msdn.microsoft.com/library/azure/dn722420.aspx) o [Azure Portal](https://portal.azure.com) para reservar una dirección IP de una región de Azure. Hay una dirección IP reservada asociada a su suscripción.
- ¿Puedo usar una IP reservada con redes virtuales basadas en grupos de afinidad?
    No. Las IP reservadas solo se admiten en redes virtuales regionales. Las IP reservadas no se admiten para redes virtuales asociadas a grupos de afinidad. Para más información sobre cómo asociar una red virtual a una región o un grupo de afinidad, consulte el artículo [Redes virtuales regionales y grupos de afinidad](virtual-networks-migrate-to-regional-vnet.md).

## <a name="manage-reserved-vips"></a>Administración de VIP reservadas

### <a name="using-azure-powershell-classic"></a>Mediante Azure PowerShell (clásica)

Para poder usar IP reservadas, debe agregarlo a su suscripción. Cree una IP reservada del grupo de direcciones IP públicas disponibles en la ubicación *Centro de EE. UU.* de la siguiente manera:

> [!NOTE]
> Con el modelo de implementación clásica, debe instalar la versión de Service Management de Azure PowerShell. Para más información, consulte [Install the Azure PowerShell Service Management module](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0) (Instalación del módulo Service Management de Azure PowerShell). 

  ```powershell
    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
  ```
Sin embargo, tenga en cuenta que no puede especificar qué IP se va a reservar. Para ver qué direcciones IP están reservadas en su suscripción, ejecute el siguiente comando de PowerShell y observe los valores de *ReservedIPName* y *Dirección*:

```powershell
Get-AzureReservedIP
```

Resultado esperado:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
>Cuando crea una dirección IP reservada con PowerShell, no puede especificar un grupo de recursos en el cual crear la IP reservada. Azure lo coloca automáticamente en un grupo de recursos llamado *Default-Networking*. Si crea la IP reservada con [Azure Portal](https://portal.azure.com), puede especificar cualquier grupo de recursos que elija. Sin embargo, si crea la IP reservada en un grupo de recursos distinto de *Default-Networking*, cada vez que haga referencia a la IP reservada con comandos como `Get-AzureReservedIP` y `Remove-AzureReservedIP`, debe hacer referencia al nombre *Group resource-group-name reserved-ip-name*.  Por ejemplo, si crea una IP reservada llamada *myReservedIP* en un grupo de recursos denominado *myResourceGroup*, debe hacer referencia al nombre de la IP reservada como *Group myResourceGroup myReservedIP*.   


Una vez reservada una IP, permanece asociada a su suscripción hasta que la elimine. Elimine una IP reservada de la siguiente manera:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

### <a name="using-azure-cli-classic"></a>Mediante la CLI de Azure (clásica)
Cree una IP reservada del grupo de direcciones IP públicas disponibles en la ubicación *Centro de EE. UU.* mediante la CLI de Azure clásica de la siguiente manera:

> [!NOTE]
> Para la implementación clásica, debe usar la CLI de Azure clásica. Para obtener información acerca de cómo instalar la CLI de Azure clásica, consulte [Instalación de la CLI clásica de Azure](https://docs.microsoft.com/cli/azure/install-classic-cli?view=azure-cli-latest).
  
 Comando:
 
```azurecli
azure network reserved-ip create <name> <location>
```
Ejemplo:
 ```azurecli
 azure network reserved-ip create MyReservedIP centralus
 ```

Puede ver qué direcciones IP están reservadas en su suscripción mediante la CLI de Azure de la siguiente manera: 

Comando:
```azurecli
azure network reserved-ip list
```
Una vez reservada una IP, permanece asociada a su suscripción hasta que la elimine. Elimine una IP reservada de la siguiente manera:

Comando:

 ```azurecli
 azure network reserved-ip delete <name>
 ```
  Ejemplo:  
 ```azurecli
 azure network reserved-ip delete MyReservedIP
 ```
## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a>Reserva de la dirección IP de un servicio en la nube existente
Puede reservar la dirección IP de un servicio en la nube existente mediante el parámetro `-ServiceName`. Reserve la dirección IP de un servicio en la nube *TestService* en la ubicación *Centro de EE. UU.* de la siguiente manera:

- Mediante Azure PowerShell (clásica):

  ```powershell
  New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
  ```

- Mediante la CLI de Azure (clásica):
  
    Comando:

    ```azurecli
     azure network reserved-ip create <name> <location> -r <service-name> -d <deployment-name>
    ```
    Ejemplo:

    ```azurecli
      azure network reserved-ip create MyReservedIP centralus -r TestService -d asmtest8942
    ```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a>Asociación de una IP reservada a un nuevo servicio en la nube
El script siguiente crea una nueva dirección IP reservada y, a continuación, la asocia a un nuevo servicio en la nube denominado *TestService*.

### <a name="using-azure-powershell-classic"></a>Mediante Azure PowerShell (clásica)
```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```
> [!NOTE]
> Cuando se crea una IP reservada para usarla con un servicio en la nube, todavía es necesario hacer referencia a la máquina virtual mediante *VIP:&lt;número de puerto>* para las comunicaciones entrantes. Reservar una dirección IP no significa que pueda conectarse directamente a la máquina virtual. La IP reservada se asigna al servicio en la nube en el que se ha implementado la máquina virtual. Si desea conectarse directamente a una máquina virtual mediante la dirección IP, tendrá que configurar una dirección IP pública a nivel de instancia. Una dirección IP pública a nivel de instancia es un tipo de dirección IP pública (denominada ILPIP) que se asigna directamente a la máquina virtual. No se puede reservar. Para más información, lea el artículo [Dirección IP pública a nivel de instancia (ILPIP)](virtual-networks-instance-level-public-ip.md).
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Eliminación de una dirección IP reservada de una implementación en ejecución

Elimine una IP reservada agregada a un nuevo servicio en la nube de la siguiente manera: 
### <a name="using-azure-powershell-classic"></a>Mediante Azure PowerShell (clásica)

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

### <a name="using-azure-cli-classic"></a>Mediante la CLI de Azure (clásica)
Comando:

```azurecli
azure network reserved-ip disassociate <name> <service-name> <deployment-name>
```

Ejemplo:

```azurecli
azure network reserved-ip disassociate MyReservedIP TestService asmtest8942
```

> [!NOTE]
> Quitar una IP reservada de una implementación en ejecución, no se elimina la reserva de la suscripción. Simplemente libera la dirección IP que será usada por otro recurso de la suscripción.
> 

Para eliminar completamente una IP reservada de una suscripción, ejecute el siguiente comando:

Comando:

```azurecli
azure network reserved-ip delete <name>
```
Ejemplo:

```azurecli
azure network reserved-ip delete MyReservedIP
```

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a>Asociación de una dirección IP reservada a una implementación en ejecución

### <a name="using-azure-powershell-classic"></a>Mediante Azure PowerShell (clásica)

Los comandos siguientes crean un servicio en la nube denominado *TestService2* con una máquina virtual nueva llamada *TestVM2*. La IP reservada existente llamada *MyReservedIP* se asocia al servicio en la nube.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

### <a name="using-azure-cli-classic"></a>Mediante la CLI de Azure (clásica)
Puede asociar una nueva dirección IP reservada a la implementación del servicio en la nube en ejecución mediante la CLI de Azure de la siguiente manera:

Comando:
```azurecli
azure network reserved-ip associate <name> <service-name> <deployment-name>
```
Ejemplo:
```azurecli
azure network reserved-ip associate MyReservedIP TestService asmtest8942
```
## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Asociación de una dirección IP reservada a un servicio en la nube mediante un archivo de configuración de servicio
También puede asociar una IP reservada a un servicio en la nube mediante un archivo de configuración de servicio (CSCFG). El xml de ejemplo siguiente muestra cómo configurar un servicio en la nube para que use una VIP reservada denominada *MyReservedIP*:
```
   <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
```
## <a name="next-steps"></a>Pasos siguientes
* Descubra cómo funcionan las [direcciones IP](virtual-network-ip-addresses-overview-classic.md) en el modelo de implementación clásica.
* Obtenga más información acerca de las [direcciones IP privadas reservadas](virtual-networks-reserved-private-ip.md).
* Obtenga más información acerca de las [direcciones IP públicas de nivel de instancia (ILPIP)](virtual-networks-instance-level-public-ip.md).

