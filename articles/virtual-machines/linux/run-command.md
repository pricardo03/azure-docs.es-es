---
title: Ejecución de scripts de PowerShell en una VM Linux de Azure
description: Este tema describe cómo ejecutar scripts dentro de una máquina virtual Linux de Azure mediante el comando Ejecutar
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/26/2019
ms.topic: article
manager: carmonm
ms.openlocfilehash: fe0547c1ddb89b8d9ab3e876e83bbdea17a58743
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64570588"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Ejecución de scripts de PowerShell en la máquina virtual Linux con el comando Ejecutar

El comando Ejecutar usa el agente de VM para ejecutar los scripts de PowerShell dentro de una VM Linux de Azure. Estos scripts pueden utilizarse para la administración de aplicaciones o máquinas generales, además de utilizarse para diagnosticar y corregir rápidamente problemas de red y de acceso a la VM, y para obtener devolver a la VM a un estado correcto.

## <a name="benefits"></a>Ventajas

Existen varias opciones que pueden utilizarse para tener acceso a las máquinas virtuales. El comando Ejecutar pueden ejecutar scripts en sus máquinas virtuales de forma remota con el agente de VM. El comando Ejecutar se puede usar mediante Azure Portal, la [API REST](/rest/api/compute/virtual%20machines%20run%20commands/runcommand) o la [CLI de Azure ](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) para VM con Linux.

Esta funcionalidad resulta útil en situaciones en las que desea ejecutar un script en una máquina virtual, y es una de las únicas maneras de solucionar problemas y corregir una máquina virtual que no tiene el RDP o el puerto SSH abiertos debido a una configuración de usuario administrativo o red incorrectos.

## <a name="restrictions"></a>Restricciones

A continuación, se muestra una lista de restricciones que están presentes cuando se usa el comando Ejecutar.

* La salida se limita a los últimos 4096 bytes
* El tiempo mínimo para ejecutar un script es de aproximadamente 20 segundos
* Los scripts se ejecutan de forma predeterminada como usuario con privilegios elevados en Linux
* Se puede ejecutar un script a la vez
* No se admiten los scripts que solicitan información (modo interactivo).
* No se puede cancelar un script en ejecución
* El tiempo máximo que se puede ejecutar un script es de 90 minutos después de agotarse el tiempo de espera
* La conectividad saliente de la máquina virtual es necesaria para devolver los resultados del script.

> [!NOTE]
> Para poder funcionar correctamente, el comando Ejecutar requiere conectividad (puerto 443) a las direcciones IP públicas de Azure. Si la extensión no tiene acceso a estos puntos de conexión, los scripts se pueden ejecutar correctamente pero no devuelven los resultados. Si va a bloquear el tráfico de la máquina virtual, puede usar las [etiquetas de servicio](../../virtual-network/security-overview.md#service-tags) para permitir el tráfico a las direcciones IP públicas de Azure mediante el uso de la etiqueta `AzureCloud`.

## <a name="azure-cli"></a>Azure CLI

A continuación, se muestra un ejemplo que utiliza el comando [az vm run-command](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) para ejecutar un script de PowerShell en una máquina virtual Linux de Azure.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Para ejecutar comandos como un usuario diferente, puede usar `sudo -u` para especificar la cuenta de usuario que se utilizará.

## <a name="azure-portal"></a>Portal de Azure

Navegue a una máquina virtual en [Azure](https://portal.azure.com) y seleccione **Ejecutar comando** en **OPERATIONS**. Se mostrará una lista de los comandos disponibles para ejecutarse en la máquina virtual.

![Lista de comandos Ejecutar](./media/run-command/run-command-list.png)

Elija un comando para ejecutar. Algunos de los comandos pueden tener parámetros de entrada obligatorios u opcionales. Para estos comandos, los parámetros se presentan como campos de texto para que pueda proporcionar los valores de entrada. Para cada comando, puede ver el script que se está ejecutando expandiendo **Ver script**. **RunShellScript** es diferente de los otros comandos, ya que permite proporcionar sus propios scripts personalizados.

> [!NOTE]
> Los comandos integrados no son editables.

Una vez que se elige el comando, haga clic en **Ejecutar** para ejecutar el script. El script se ejecuta y, cuando haya finalizado, devuelve el resultado y los errores en la ventana de salida. La siguiente captura de pantalla muestra un ejemplo de salida tras ejecutar el comando **ifconfig**.

![Salida del script del comando Ejecutar](./media/run-command/run-command-script-output.png)

## <a name="available-commands"></a>Comandos disponibles

Esta tabla muestra la lista de comandos disponibles para máquinas virtuales Linux. El comando **RunShellScript** se puede usar para ejecutar un script personalizado que desee.

|**Nombre**|**Descripción**|
|---|---|
|**RunShellScript**|Ejecuta un script de PowerShell de Linux.|
|**ifconfig**| Obtenga la configuración de todas las interfaces de red.|

## <a name="limiting-access-to-run-command"></a>Limitación del acceso al comando Ejecutar

Para enumerar los comandos Ejecutar o mostrar los detalles de un comando, se requiere el permiso `Microsoft.Compute/locations/runCommands/read` en el nivel de suscripción, que tienen el rol [Lector](../../role-based-access-control/built-in-roles.md#reader) integrado y otros superiores.

Para ejecutar un comando, se requiere el permiso `Microsoft.Compute/virtualMachines/runCommand/action` en el nivel de suscripción, que tienen el rol [Colaborador de máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) y otros superiores.

Puede usar uno de los roles [integrados](../../role-based-access-control/built-in-roles.md) o crear uno [personalizado](../../role-based-access-control/custom-roles.md) para usar el comando Ejecutar.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Ejecución de scripts en la máquina virtual Linux](run-scripts-in-vm.md) para obtener información sobre otras maneras de ejecutar comandos y scripts de forma remota en la máquina virtual.
