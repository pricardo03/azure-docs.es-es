---
title: 'Inicio rápido: Creación de una zona y un registro de Azure DNS mediante Azure PowerShell'
description: Obtenga información sobre cómo crear una zona y un registro DNS en Azure DNS. Este es un inicio rápido paso a paso para crear y administrar su primera zona y su primer registro DNS con Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 07/16/2018
ms.author: victorh
ms.openlocfilehash: e5801e9ed512a32d793f7b4b71be86174f656ab0
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089985"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Inicio rápido: Creación de una zona y un registro de Azure DNS mediante Azure PowerShell

En este inicio rápido, creará su primera zona y su primer registro DNS mediante Azure PowerShell. También puede llevar a cabo estos pasos con [Azure Portal](dns-getstarted-portal.md) o la [CLI de Azure](dns-getstarted-cli.md). 

Una zona DNS se usa para hospedar los registros DNS de un dominio concreto. Para iniciar el hospedaje de su dominio en DNS de Azure, debe crear una zona DNS para ese nombre de dominio. Cada registro DNS del dominio se crea luego en esta zona DNS. Finalmente, para publicar la zona DNS en Internet, debe configurar los servidores de nombres para el dominio. A continuación, se describen cada uno de estos pasos.

Azure DNS también admite la creación de dominios privados. Para obtener instrucciones paso a paso sobre cómo crear la primera zona DNS privada y registrarla, consulte [Introducción al uso de PowerShell con las zonas privadas de Azure DNS](private-dns-getstarted-powershell.md).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-the-resource-group"></a>Creación del grupo de recursos

Antes de crear la zona DNS, cree un grupo de recursos que la contenga:

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>Creación de una zona DNS

Una zona DNS se crea mediante el cmdlet `New-AzureRmDnsZone` . En el ejemplo siguiente, se crea una zona DNS llamada *contoso.com* en el grupo de recursos *MyResourceGroup*. Utilice el ejemplo para crear una zona DNS, sustituyendo los valores por los suyos.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Creación de un registro de DNS

Los conjuntos de registros se crean mediante el cmdlet `New-AzureRmDnsRecordSet`. En el ejemplo siguiente se crea un conjunto de registros con el nombre relativo "www" en la zona DNS "contoso.com" del grupo de recursos "MyResourceGroup". El nombre completo del conjunto de registros es "www.contoso.com". El tipo de registro es "A", con la dirección IP 1.2.3.4 y el valor de TTL es de 3600 segundos.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

## <a name="view-records"></a>Visualización de los registros

Para enumerar los registros DNS de su zona, use lo siguiente:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Actualización de los servidores de nombres

Una vez haya comprobado que la zona y los registros DNS se han configurado correctamente, tiene que configurar el nombre de dominio para usar los servidores de nombres DNS de Azure. De esta forma, otros usuarios en Internet podrán encontrar los registros DNS.

Los servidores de nombres de su zona se proporcionan con el cmdlet `Get-AzureRmDnsZone`:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Estos servidores de nombres deben configurarse con el registrador de nombres de dominio (donde adquirió el nombre de dominio). El registrador ofrece la opción de configurar los servidores de nombres para el dominio. Si quiere obtener más información, consulte [Delegación del dominio en DNS de Azure](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Eliminación de todos los recursos

Cuando ya no los necesite, puede eliminar todos los recursos creados en este inicio rápido mediante la eliminación del grupo de recursos:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado su primera zona y su primer registro DNS con Azure PowerShell, puede crear registros para una aplicación web de un dominio personalizado.

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)

