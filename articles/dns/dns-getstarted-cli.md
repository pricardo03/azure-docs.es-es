---
title: 'Inicio rápido: Creación de una zona y un registro de Azure DNS mediante la CLI de Azure'
description: 'Inicio rápido: Aprender a crear una zona y un registro DNS en Azure DNS. Esta es una guía paso a paso para crear y administrar su primera zona y su primer registro DNS con la CLI de Azure.'
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: e61975d81fd5920feb5fd47845c67d0aa5293ae6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962018"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Inicio rápido: Creación de una zona y un registro de Azure DNS mediante la CLI de Azure

Este artículo lo guía por los pasos necesarios para crear su primera zona y su primer registro DNS mediante la CLI de Azure, que está disponible para Windows, Mac y Linux. También puede llevar a cabo estos pasos con [Azure Portal](dns-getstarted-portal.md) o [Azure PowerShell](dns-getstarted-powershell.md).

Una zona DNS se usa para hospedar los registros DNS de un dominio concreto. Para iniciar el hospedaje de su dominio en DNS de Azure, debe crear una zona DNS para ese nombre de dominio. Cada registro DNS del dominio se crea luego en esta zona DNS. Finalmente, para publicar la zona DNS en Internet, debe configurar los servidores de nombres para el dominio. A continuación, se describen cada uno de estos pasos.

Azure DNS admite zonas DNS privadas (actualmente en versión preliminar pública). Para más información acerca de las zonas DNS privadas, consulte [Using Azure DNS for private domains](private-dns-overview.md) (Uso de Azure DNS para dominios privados). Para ver un ejemplo de cómo crear una zona DNS privada, consulte [Get started with Azure DNS Private Zones using CLI](./private-dns-getstarted-cli.md) (Introducción al uso de la CLI de Azure 2.0 con las zonas privadas de Azure DNS).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-the-resource-group"></a>Creación del grupo de recursos

Antes de crear la zona DNS, cree un grupo de recursos que la contenga:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>Creación de una zona DNS

Una zona DNS se crea con el comando `az network dns zone create` . Para ver la ayuda sobre este comando, escriba `az network dns zone create -h`.

En el ejemplo siguiente, se crea una zona DNS llamada *contoso.com* en el grupo de recursos *MyResourceGroup*. Utilice el ejemplo para crear una zona DNS, sustituyendo los valores por los suyos.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```

## <a name="create-a-dns-record"></a>Creación de un registro de DNS

Para crear un registro de DNS, use el comando `az network dns record-set [record type] add-record`. Para obtener ayuda sobre los registros A, consulte `azure network dns record-set A add-record -h`.

En el ejemplo siguiente se crea un registro con el nombre relativo "www" en la zona DNS "contoso.com" del grupo de recursos "MyResourceGroup". El nombre completo del conjunto de registros es www.contoso.com. El tipo de registro es "A", con la dirección IP "1.2.3.4" y un valor predeterminado TTL de 3600 segundos (1 hora).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

## <a name="view-records"></a>Visualización de los registros

Para enumerar los registros DNS de su zona, ejecute:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="update-name-servers"></a>Actualización de los servidores de nombres

Una vez haya comprobado que la zona y los registros DNS se han configurado correctamente, tiene que configurar el nombre de dominio para usar los servidores de nombres DNS de Azure para permitir que otros usuarios de Internet encuentren sus registros DNS.

Los servidores de nombres de su zona se proporcionan con el comando `az network dns zone show`. Para ver los nombres del servidor de nombres, utilice la salida JSON, como se muestra en el ejemplo siguiente.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Estos servidores de nombres deben configurarse con el registrador de nombres de dominio (donde adquirió el nombre de dominio). El registrador ofrece la opción de configurar los servidores de nombres para el dominio. Para más información, vea [Tutorial: Hospedaje del dominio en Azure DNS](dns-delegate-domain-azure-dns.md#delegate-the-domain).

## <a name="delete-all-resources"></a>Eliminación de todos los recursos
 
Cuando ya no los necesite, puede eliminar todos los recursos creados en este inicio rápido mediante la eliminación del grupo de recursos:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado su primera zona y su primer registro DNS mediante la CLI de Azure, puede crear registros para una aplicación web de un dominio personalizado.

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)
