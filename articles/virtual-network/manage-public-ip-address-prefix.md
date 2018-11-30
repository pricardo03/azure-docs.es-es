---
title: Creación, modificación o eliminación del prefijo de una dirección IP pública de Azure | Microsoft Docs
description: Obtenga información sobre cómo crear, cambiar o eliminar el prefijo de una dirección IP pública.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: a0ae1f3fbf9189068cae4b18ac92f0bea0498f67
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427580"
---
# <a name="create-change-or-delete-a-public-ip-address-prefix"></a>Creación, modificación o eliminación del prefijo de una dirección IP pública

Obtenga información sobre el prefijo de una dirección IP pública y cómo crearlo, modificarlo y eliminarlo. El prefijo de una dirección IP pública es un intervalo contiguo de direcciones basado en el número de direcciones IP públicas que especifique. Las direcciones se asignan a su suscripción. Al crear un recurso de dirección IP pública, puede asignar una dirección IP pública estática a partir del prefijo y asociarla a máquinas virtuales, equilibradores de carga u otros recursos para habilitar la conectividad a Internet. Si no está familiarizado con los prefijos de las direcciones IP públicas, consulte [Información general sobre el prefijo de una dirección IP pública](public-ip-address-prefix.md)

## <a name="before-you-begin"></a>Antes de empezar

> [!IMPORTANT]
> El prefijo de dirección IP pública está en versión preliminar pública en regiones limitadas. Puede [obtener información sobre qué significa estar en versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). El prefijo de dirección IP pública está actualmente disponible en: Centro-oeste de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Centro de EE. UU., Europa del Norte, Europa Occidental y Sudeste Asiático. Para obtener una lista actualizada de regiones, consulte [Actualizaciones de Azure](https://azure.microsoft.com/updates/?product=virtual-network).

Complete las tareas siguientes antes de seguir los pasos de las secciones de este artículo:

- Si todavía no tiene una cuenta de Azure, regístrese para obtener una [cuenta de evaluación gratuita](https://azure.microsoft.com/free).
- Si usa el portal, abra https://aka.ms/publicipprefixportal e inicie sesión con la cuenta de Azure.
- Si usa comandos de PowerShell para completar las tareas de este artículo, ejecute los comandos que se encuentran en [Azure Cloud Shell](https://shell.azure.com/powershell) o ejecute PowerShell en el equipo. Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. Para realizar este tutorial, es necesaria la versión 6.3.1 del módulo de AzureRm.Network PowerShell u otra posterior. Ejecute `Get-Module -ListAvailable AzureRM.Network` para buscar la versión instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/AzureRm.Network.6.3.1). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.
- Si usa la interfaz de la línea de comandos (CLI) de Azure para completar las tareas de este artículo, ejecute los comandos que se encuentran en [Azure Cloud Shell](https://shell.azure.com/bash) o ejecute la CLI en el equipo. Para realizar este tutorial, es necesaria la versión 2.0.41 de la CLI de Azure o una versión posterior. Ejecute `az --version` para buscar la versión instalada. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli). Si ejecuta de forma local la CLI de Azure, también debe ejecutar `az login` para crear una conexión con Azure.

La cuenta en la que inicia sesión o con la que se conecta a Azure debe tener asignado el rol de [colaborador de red](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) o un [rol personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que tenga asignadas las acciones apropiadas en [Permisos](#permissions).

Los prefijos de las direcciones IP públicas tienen un cargo. Para obtener información detallada, consulte [Precios](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="create-a-public-ip-address-prefix"></a>Creación del prefijo de una dirección IP pública

1. En la esquina superior izquierda del portal, seleccione **+ Crear un recurso**.
2. Escriba *Prefijo de dirección IP pública* en el cuadro *Búsqueda en Marketplace*. Seleccione **Prefijo de dirección IP pública** cuando aparezca en los resultados de búsqueda.
3. En **Prefijo de dirección IP pública**, seleccione **Crear**.
4. Escriba o seleccione valores para las siguientes opciones, en **Crear prefijo de dirección IP pública**, y seleccione **Crear**:

   |Configuración|¿Necesario?|Detalles|
   |---|---|---|
   |Subscription|SÍ|Debe existir en la misma [suscripción](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) que el recurso al cual desee asociar la dirección IP pública.|
   |Grupos de recursos|SÍ|Puede existir en la misma suscripción que el [grupo de recursos](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) al cual desee asociar la dirección IP pública o en otra diferente.|
   |NOMBRE|SÍ|El nombre debe ser único dentro del grupo de recursos que seleccione.|
   |Region|SÍ|Debe existir en la misma [región](https://azure.microsoft.com/regions) que las direcciones IP públicas que asignará a direcciones del intervalo. El prefijo está actualmente en versión preliminar en centro-oeste de EE. UU., oeste de EE. UU., oeste de EE. UU. 2, centro de EE. UU., Europa del norte, Europa occidental y sudeste asiático.|
   |Tamaño del prefijo|SÍ| El tamaño del prefijo que necesita. A/28 o 16 direcciones IP es el valor predeterminado. 

**Comandos**


|Herramienta|Get-Help|
|---|---|
|CLI|[az network public-ip prefix create](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create)|
|PowerShell|[New-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/new-azurermpublicipprefix)|

## <a name="create-a-static-public-ip-address-from-a-prefix"></a>Creación de una dirección IP pública estática a partir de un prefijo
Una vez que cree un prefijo, debe crear las direcciones IP estáticas a partir del prefijo. Para ello, siga estos pasos.

1. En el cuadro que contiene el texto *Buscar recursos*, en la parte superior de Azure Portal, escriba *Prefijo de dirección IP pública*. Seleccione **Prefijos de direcciones IP públicas** cuando aparezca en los resultados de búsqueda.
2. Seleccione el prefijo a partir del cual quiere crear IP públicas.
3. Cuando aparezca en los resultados de búsqueda, selecciónelo y haga clic en **+Agregar dirección IP** en la sección Información general. En caso de que no lo vea, asegúrese de que está usando el vínculo correcto para la versión preliminar: https://aka.ms/publicipprefixportal
4. Escriba o seleccione valores para las siguientes opciones, en **Crear dirección IP pública**. Puesto que un prefijo es para SKU estándar, IPv4 y estático, solo deberá proporcionar la información siguiente:

   |Configuración|¿Necesario?|Detalles|
    |---|---|---|
    |NOMBRE|SÍ|El nombre de la dirección IP pública debe ser único dentro del grupo de recursos que seleccione.|
   |Tiempo de espera de inactividad (minutos)|Sin |Cantidad de minutos para mantener una conexión TCP o HTTP abierta sin que dependa del envío de mensajes de mantenimiento de los clientes. |
   |Etiqueta de nombre DNS|Sin |Debe ser único dentro de la región de Azure en la que cree el nombre (entre todas las suscripciones y todos los clientes). Azure registra automáticamente el nombre y la dirección IP en el DNS para que pueda conectarse a un recurso con el nombre. Azure anexa una subred predeterminada, como *location.cloudapp.azure.com* (en que location es la ubicación seleccionada), al nombre que proporcione para crear el nombre DNS cualificado completo. Para obtener más información, consulte [Utilizar Azure DNS con una dirección IP pública de Azure](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|

## <a name="view-or-delete-a-prefix"></a>Ver o eliminar un prefijo

1. En el cuadro que contiene el texto *Buscar recursos*, en la parte superior de Azure Portal, escriba *Prefijo de dirección IP pública*. Seleccione **Prefijos de direcciones IP públicas** cuando aparezca en los resultados de búsqueda.
2. Seleccione el nombre del prefijo de la dirección IP pública que quiera ver, cambiar la configuración o eliminar de la lista.
3. Complete una de las siguientes opciones en función de si desea ver, eliminar o cambiar el prefijo de la dirección IP pública.
    - **Ver**: en la sección **Información general** se muestra la configuración clave del prefijo de una dirección IP pública, como prefijo.
    - **Eliminar**: para eliminar el prefijo de una dirección IP pública, seleccione **Eliminar** en la sección **Información general**. Si las direcciones dentro del prefijo están asociadas a recursos de dirección IP pública, primero debe eliminar dichos recursos de dirección IP pública. Consulte [Eliminar una dirección IP pública](virtual-network-public-ip-address.md#view-change-settings-for-or-delete-a-public-ip-address).

**Comandos**

|Herramienta|Get-Help|
|---|---|
|CLI|[az network public-ip prefix list](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-list) para mostrar las direcciones IP públicas, [az network public-ip prefix show](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-show) para mostrar la configuración, [az network public-ip prefix update](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-update) para actualizar, [az network public-ip prefix delete](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-delete) para eliminar|
|PowerShell|[Get-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/get-azurermpublicipprefix) para recuperar un objeto de dirección IP pública y ver su configuración, [Set-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/set-azurermpublicipprefix) para actualizar la configuración, [Remove-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/remove-azurermpublicipprefix) para eliminar|

## <a name="permissions"></a>Permisos

Para realizar tareas en prefijos de direcciones IP públicas, su cuenta debe estar asignada al rol de [colaborador de red](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) o a un rol [personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que tenga asignadas las acciones adecuadas que se muestran en la tabla siguiente:

| .                                                                   | NOMBRE                                                           |
| ---------                                                                | -------------                                                  |
| Microsoft.Network/publicIPPrefixes/read                           | Leer el prefijo de una dirección IP pública                                |
| Microsoft.Network/publicIPPrefixes/write                          | Crear o actualizar el prefijo de una dirección IP pública                    |
| Microsoft.Network/publicIPPrefixes/delete                         | Eliminación del prefijo de una dirección IP pública                              |
|Microsoft.Network/publicIPPrefixes/join/action | Creación de una dirección IP pública a partir de un prefijo |

## <a name="next-steps"></a>Pasos siguientes

- Obtener información sobre los escenarios y las ventajas de usar un [prefijo de IP pública](public-ip-address-prefix.md)
