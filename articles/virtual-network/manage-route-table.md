---
title: Creación, modificación o eliminación de una tabla de rutas de Azure
titlesuffix: Azure Virtual Network
description: Obtenga información sobre cómo crear, modificar o eliminar una tabla de rutas.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: kumud
ms.openlocfilehash: a39d9f9c5a138ece5d40cc5afe1d1dcdd8e7a41a
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849797"
---
# <a name="create-change-or-delete-a-route-table"></a>Creación, modificación o eliminación de una tabla de rutas

Azure enruta automáticamente el tráfico entre redes locales, las redes virtuales y las subredes de Azure. Si desea cambiar algún enrutamiento predeterminado de Azure, debe crear una tabla de rutas. Si no está familiarizado con el enrutamiento en redes virtuales, puede obtener más información sobre él en la [información general sobre enrutamiento](virtual-networks-udr-overview.md) o siguiendo un [tutorial](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Complete las tareas siguientes antes de seguir los pasos de las secciones de este artículo:

* Si todavía no tiene una cuenta de Azure, regístrese para obtener una [cuenta de evaluación gratuita](https://azure.microsoft.com/free).<br>
* Si usa el portal, abra https://portal.azure.come inicie sesión con su cuenta de Azure.<br>
* Si usa comandos de PowerShell para completar las tareas de este artículo, ejecute los comandos que se encuentran en [Azure Cloud Shell](https://shell.azure.com/powershell) o ejecute PowerShell en el equipo. Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. Para realizar este tutorial, se necesita la versión 1.0.0 del módulo de Azure PowerShell u otra posterior. Ejecute `Get-Module -ListAvailable Az` para buscar la versión instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.<br>
* Si usa la interfaz de la línea de comandos (CLI) de Azure para completar las tareas de este artículo, ejecute los comandos que se encuentran en [Azure Cloud Shell](https://shell.azure.com/bash) o ejecute la CLI en el equipo. Para realizar este tutorial, es necesaria la versión 2.0.31 de la CLI de Azure o una versión posterior. Ejecute `az --version` para buscar la versión instalada. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). Si ejecuta de forma local la CLI de Azure, también debe ejecutar `az login` para crear una conexión con Azure.

La cuenta que inicie sesión en o conectarse a Azure, debe asignarse a la [colaborador de la red](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol o a un [rol personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que se asigna a las acciones adecuadas que se muestran en [permisos ](#permissions).

## <a name="create-a-route-table"></a>Creación de una tabla de rutas

Existe un límite para la cantidad de tablas de rutas que puede crear por suscripción y ubicación de Azure. Para más información, consulte el artículo acerca de los [límites de Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. En la esquina superior izquierda del portal, seleccione **+ Crear un recurso**.
1. Seleccione **Redes** y, luego, **Tabla de rutas**.
1. Escriba un **nombre** para la tabla de rutas, seleccione la **suscripción**, cree un **grupo de recursos** nuevo o seleccione uno existe, seleccione una **ubicación** y, luego, seleccione **Crear**. Si va a asociar la tabla de rutas a una subred de una red virtual que está conectada a la red local a través de una puerta de enlace VPN y deshabilita **propagación de rutas de puerta de enlace de red Virtual**, no lo son las rutas locales se propaga a las interfaces de red en la subred.

### <a name="create-route-table---commands"></a>Crear tabla de rutas - comandos

* CLI de Azure: [az network route-table create](/cli/azure/network/route-table/route)<br>
* PowerShell: [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)

## <a name="view-route-tables"></a>Vista de las tablas de rutas

En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela. Aparecen las tablas de rutas que existen en la suscripción.

### <a name="view-route-table---commands"></a>Vista de tabla de rutas - comandos

* CLI de Azure: [az network route-table list](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="view-details-of-a-route-table"></a>Vista de los detalles de una tabla de rutas

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione en la lista la tabla de rutas cuyos detalles desea ver. En **CONFIGURACIÓN**, puede ver las **rutas** de la tabla de rutas y las **subredes** con las que está asociada la tabla de rutas.
1. Para más información sobre la configuración común de Azure, consulte la información siguiente:

    * [Registro de actividad](../azure-monitor/platform/activity-logs-overview.md)<br>
    * [Control de acceso (IAM)](../role-based-access-control/overview.md)<br>
    * [Etiquetas](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Bloqueos](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Script de Automation](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)

### <a name="view-details-of-route-table---commands"></a>Ver detalles de la tabla de rutas - comandos

* CLI de Azure: [az network route-table show](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="change-a-route-table"></a>Modificación de una tabla de rutas

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione la tabla de rutas que desea modificar. Los cambios más comunes son [agregar](#create-a-route) o [quitar](#delete-a-route) rutas y [asociar](#associate-a-route-table-to-a-subnet) o [desasociar](#dissociate-a-route-table-from-a-subnet) tablas de rutas de las subredes.

### <a name="change-a-route-table---commands"></a>Cambiar una tabla de rutas - comandos

* CLI de Azure: [az network route-table update](/cli/azure/network/route-table/route)<br>
* PowerShell: [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Asociación de una tabla de rutas a una subred

Una subred puede tener una tabla de ruta asociada a ella o ninguna. Una tabla de rutas se puede asociar a varias subredes o a ninguna. Como las tablas de rutas no se asocian a las redes virtuales, debe asociar una tabla de rutas a cada subred con la que desea asociarla. Todo el tráfico sale de la subred se enruta en función de las rutas que se ha creado dentro de las tablas de rutas, [rutas predeterminadas](virtual-networks-udr-overview.md#default), y las rutas propagadas desde una red local, si la red virtual está conectada a una puerta de enlace de red virtual de Azure () ExpressRoute o VPN). Solo puede asociar una tabla de rutas a las subredes de las redes virtuales que existen en la misma suscripción y ubicación de Azure de la tabla de rutas.

1. En el cuadro de búsqueda de la parte superior del portal, escriba *redes virtuales*. Cuando aparezca la opción **Redes virtuales** en los resultados de la búsqueda, selecciónela.
1. Seleccione la red virtual en la lista que contiene la subred a la que desea asociar una tabla de rutas.
1. Seleccione **Subredes** en **CONFIGURACIÓN**.
1. Seleccione la subred a la que desea asociar la tabla de rutas.
1. Seleccione **Tabla de rutas**, la tabla de rutas a la que desea asociar la subred y, luego, seleccione **Guardar**.

Si la red virtual está conectada a una instancia de Azure VPN Gateway, no asocie una tabla de rutas a la [subred de la puerta de enlace](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) que incluya una ruta con un destino 0.0.0.0/0. Si lo hace, puede que la puerta de enlace no funcione correctamente. Para más información acerca del uso de 0.0.0.0/0 en una ruta, consulte [Enrutamiento del tráfico de redes virtuales](virtual-networks-udr-overview.md#default-route).

### <a name="associate-a-route-table---commands"></a>Asociar una tabla de rutas - comandos

* CLI de Azure: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Desasociación de una tabla de rutas de una subred

Cuando desasocia una tabla de rutas de una subred, Azure enruta el tráfico según sus [rutas predeterminadas](virtual-networks-udr-overview.md#default).

1. En el cuadro de búsqueda de la parte superior del portal, escriba *redes virtuales*. Cuando aparezca la opción **Redes virtuales** en los resultados de la búsqueda, selecciónela.
1. Seleccione la red virtual que contiene la subred de la que desea desasociar una tabla de rutas.
1. Seleccione **Subredes** en **CONFIGURACIÓN**.
1. Seleccione la subred de la que desea desasociar la tabla de rutas.
1. Seleccione **Tabla de rutas**, **Ninguna** y, luego, seleccione **Guardar**.

### <a name="dissociate-a-route-table---commands"></a>Desasociar una tabla de rutas - comandos

* CLI de Azure: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="delete-a-route-table"></a>Eliminación de una tabla de rutas

Una tabla de rutas no se puede eliminar si está asociada a alguna subred. [Desasocie](#dissociate-a-route-table-from-a-subnet) una tabla de rutas de todas las subredes antes de intentar eliminarla.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione **...** en el lado derecho de la tabla de rutas que desea eliminar.
1. Seleccione **Eliminar** y, luego, seleccione **Sí**.

### <a name="delete-a-route-table---commands"></a>Eliminar una tabla de rutas - comandos

* CLI de Azure: [az network route-table delete](/cli/azure/network/route-table/route)<br>
* PowerShell: [Remove-AzRouteTable](/powershell/module/az.network/remove-azroutetable)

## <a name="create-a-route"></a>Creación de una ruta

Existe un límite para la cantidad de rutas por tabla de rutas que puede crear por suscripción y ubicación de Azure. Para más información, consulte el artículo acerca de los [límites de Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione en la lista la tabla de rutas a la que desea agregar una ruta.
1. Seleccione **Rutas** en **CONFIGURACIÓN**.
1. Seleccione **+ agregar**.
1. Escriba un **nombre** único para la ruta dentro de la tabla de rutas.
1. Escriba el **prefijo de dirección**, en notación CIDR, al que desea enrutar el tráfico. El prefijo no se puede duplicar en más de una ruta dentro de la tabla de rutas, aunque puede estar dentro de otro prefijo. Por ejemplo, si definió 10.0.0.0/16 como prefijo en una ruta, de todos modos puede definir otra ruta con el prefijo de dirección 10.0.0.0/24. Azure selecciona una ruta para el tráfico en función de la coincidencia de prefijo más larga. Para más información sobre cómo Azure selecciona las rutas, consulte [Selección de rutas por parte de Azure](virtual-networks-udr-overview.md#how-azure-selects-a-route).
1. Seleccione un **tipo de próximo salto**. Para una descripción detallada de todos los tipos de próximo salto, consulte [Enrutamiento del tráfico de redes virtuales](virtual-networks-udr-overview.md).
1. Escriba una dirección IP en la **dirección del próximo salto**. Solo puede escribir una dirección si seleccionó *Aplicación virtual* en **Tipo de próximo salto**.
1. Seleccione **Aceptar**.

### <a name="create-a-route---commands"></a>Creación de una ruta - comandos

* CLI de Azure: [az network route-table route create](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)

## <a name="view-routes"></a>Vista de las rutas

Una tabla de rutas contiene varias rutas o ninguna. Para más detalles sobre la información que aparece al ver las rutas, consulte [Enrutamiento del tráfico de redes virtuales](virtual-networks-udr-overview.md).

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione en la lista la tabla de rutas cuyas rutas quiere ver.
1. Seleccione **Rutas** en **CONFIGURACIÓN**.

### <a name="view-routes---commands"></a>Visualización de las rutas - comandos

* CLI de Azure: [az network route-table route list](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="view-details-of-a-route"></a>Vista de los detalles de una ruta

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione la tabla de rutas de la que desea conocer los detalles de una ruta.
1. Seleccione **Rutas**.
1. Seleccione la ruta cuyos detalles quiere ver.

### <a name="view-details-of-a-route---commands"></a>Ver detalles de una ruta - comandos

* CLI de Azure: [az network route-table route show](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="change-a-route"></a>Modificación de una ruta

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione la tabla de rutas de la que desea modificar una ruta.
1. Seleccione **Rutas**.
1. Seleccione la ruta que desea modificar.
1. Cambie la configuración existente a la configuración nueva y, luego, seleccione **Guardar**.

### <a name="change-a-route---commands"></a>Cambiar una ruta - comandos

* CLI de Azure: [az network route-table route update](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Set-AzRouteConfig](/powershell/module/az.network/set-azrouteconfig)

## <a name="delete-a-route"></a>Eliminación de una ruta

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *tablas de rutas*. Cuando la opción **Tablas de rutas** aparezca en los resultados de la búsqueda, selecciónela.
1. Seleccione la tabla de rutas de la que desea eliminar una ruta.
1. Seleccione **Rutas**.
1. En la lista de rutas, seleccione **...** en el lado derecho de la ruta que desea eliminar.
1. Seleccione **Eliminar** y, luego, seleccione **Sí**.

### <a name="delete-a-route---commands"></a>Eliminar una ruta - comandos

* CLI de Azure: [az network route-table route delete](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Remove-AzRouteConfig](/powershell/module/az.network/remove-azrouteconfig)

## <a name="view-effective-routes"></a>Vista de las rutas eficaces

Las rutas eficaces de cada interfaz de red conectada a una máquina virtual son una combinación de las tablas de rutas que creó, las rutas predeterminadas de Azure y cualquier ruta propagada desde las redes locales vía BGP a través de una puerta de enlace de red virtual de Azure. Conocer las rutas eficaces de una interfaz de red resulta útil para solucionar los problemas con el enrutamiento. Puede ver las rutas eficaces de cualquier interfaz de red conectada a una máquina virtual en ejecución.

1. En el cuadro de búsqueda que se encuentra en la parte superior del portal, escriba el nombre de una máquina virtual cuyas rutas eficaces desea ver. Si no conoce el nombre de una máquina virtual, escriba *máquinas virtuales* en el cuadro de búsqueda. Cuando la opción **Máquinas virtuales** aparezca en los resultados de la búsqueda, selecciónela y seleccione una máquina virtual en la lista.
1. Seleccione **Redes** en **CONFIGURACIÓN**.
1. Seleccione el nombre de una interfaz de red.
1. Seleccione **Rutas eficaces** en **SOPORTE Y SOLUCIÓN DE PROBLEMAS**.
1. Revise la lista de las rutas eficaces para determinar si existe la ruta correcta a la que desea enrutar el tráfico. Obtenga más información sobre los tipos de próximo salto que ve en esta lista en [Enrutamiento del tráfico de redes virtuales](virtual-networks-udr-overview.md).

### <a name="view-effective-routes---commands"></a>Vista de las rutas eficaces: comandos

* CLI de Azure: [az network nic show-effective-route-table](/cli/azure/network/nic?view=azure-cli-latest)<br>
* PowerShell: [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)

## <a name="validate-routing-between-two-endpoints"></a>Validación del enrutamiento entre dos puntos de conexión

Puede determinar el tipo de próximo paso entre una máquina virtual y la dirección IP de otro recurso de Azure, un recurso local o un recurso de Internet. Determinar el enrutamiento de Azure resulta útil para solucionar los problemas con el enrutamiento. Para completar esta tarea, debe haber una instancia de Network Watcher. Si no tiene una instancia de Network Watcher, complete los pasos que aparecen en [Creación de una instancia de Azure Network Watcher](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para crear una.

1. En el cuadro de búsqueda que aparece en la parte superior del portal, escriba *network watcher*. Cuando aparezca la opción **Network Watcher** en los resultados de búsqueda, selecciónela.
1. Seleccione **Próximo paso** en **HERRAMIENTAS DE DIAGNÓSTICO DE RED**.
1. Seleccione la **suscripción** y el **grupo de recursos** de la máquina virtual de origen desde la que desea validar el enrutamiento.
1. Seleccione la **máquina virtual**, la **interfaz de red** conectada a la máquina virtual y la **dirección IP de origen** asignada a la interfaz de red desde la que desea validar el enrutamiento.
1. Escriba la **dirección IP de destino** a la que desea validar el enrutamiento.
1. Seleccione **Próximo paso**.
1. Tras una breve espera, se devuelve información que le indica el tipo de próximo paso y el identificador de la ruta que enrutó el tráfico. Obtenga más información sobre los tipos de próximo salto que aquí se devuelven en [Enrutamiento del tráfico de redes virtuales](virtual-networks-udr-overview.md).

### <a name="validate-routing-between-two-endpoints---commands"></a>Validar el enrutamiento entre dos puntos de conexión, comandos

* CLI de Azure: [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest)<br>
* PowerShell: [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop)

## <a name="permissions"></a>Permisos

Para realizar tareas en rutas y en tablas de rutas, la cuenta debe estar asignada al rol de [colaborador de red](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) o a un rol [personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que tenga asignadas las acciones adecuadas que se muestran en la tabla siguiente:

| .                                                          |   NOMBRE                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Lectura de una tabla de rutas                                    |
| Microsoft.Network/routeTables/write                             |   Creación o actualización de una tabla de rutas                        |
| Microsoft.Network/routeTables/delete                            |   Eliminación de una tabla de rutas                                  |
| Microsoft.Network/routeTables/join/action                       |   Asociación de una tabla de rutas a una subred                   |
| Microsoft.Network/routeTables/routes/read                       |   Lectura de una ruta                                          |
| Microsoft.Network/routeTables/routes/write                      |   Creación o actualización de una ruta                              |
| Microsoft.Network/routeTables/routes/delete                     |   Eliminación de una ruta                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Obtención de la tabla de rutas eficaces de una interfaz de red |
| Microsoft.Network/networkWatchers/nextHop/action                |   Obtención del próximo salto de una VM                           |

## <a name="next-steps"></a>Pasos siguientes

* Crear una tabla de rutas con scripts de ejemplo de [PowerShell](powershell-samples.md) o de la [CLI de Azure](cli-samples.md) o con [plantillas de Azure Resource Manager](template-samples.md)<br>
* Crear y aplicar una [directiva de Azure](policy-samples.md) para redes virtuales
