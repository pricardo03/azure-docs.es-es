---
title: Ejecución de scripts de PowerShell en una máquina virtual Windows en Azure
description: Este tema describe cómo ejecutar scripts de PowerShell dentro de una máquina virtual Windows en Azure mediante el comando Ejecutar
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 322b21ae2273b949e9a46e0c47a9f9e4d661603e
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48267960"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar

El comando Ejecutar usa el agente de VM para ejecutar los scripts de PowerShell dentro de una VM Windows de Azure. Estos scripts pueden utilizarse para la administración de aplicaciones o máquinas generales, además de utilizarse para diagnosticar y corregir rápidamente problemas de red y de acceso a la VM, y para obtener devolver a la VM a un estado correcto.

## <a name="benefits"></a>Ventajas

Existen varias opciones que pueden utilizarse para tener acceso a las máquinas virtuales. El comando Ejecutar pueden ejecutar scripts en sus máquinas virtuales de forma remota con el agente de VM. El comando Ejecutar se puede usar mediante Azure Portal, la [API REST](/rest/api/compute/virtual%20machines%20run%20commands/runcommand) o [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) para VM con Windows.

Esta funcionalidad resulta útil en situaciones en las que desea ejecutar un script en una máquina virtual, y es una de las únicas maneras de solucionar problemas y corregir una máquina virtual que no tiene el RDP o el puerto SSH abiertos debido a una configuración de usuario administrativo o red incorrectos.

## <a name="restrictions"></a>Restricciones

Las siguientes consideraciones se aplican al usar el comando Ejecutar:

* La salida se limita a los últimos 4096 bytes
* El tiempo mínimo para ejecutar un script es de aproximadamente 20 segundos
* Los script se ejecutan como sistema en Windows
* Se puede ejecutar un script a la vez
* No se admiten los scripts que solicitan información (modo interactivo).
* No se puede cancelar un script en ejecución
* El tiempo máximo que se puede ejecutar un script es de 90 minutos después de agotarse el tiempo de espera
* La conectividad saliente de la máquina virtual es necesaria para devolver los resultados del script.

## <a name="run-a-command"></a>Ejecución de un comando

Navegue a una máquina virtual en [Azure](https://portal.azure.com) y seleccione **Ejecutar comando** en **OPERATIONS**. Se mostrará una lista de los comandos disponibles para ejecutarse en la máquina virtual.

![Lista de comandos Ejecutar](./media/run-command/run-command-list.png)

Elija un comando para ejecutar. Algunos de los comandos pueden tener parámetros de entrada obligatorios u opcionales. Para estos comandos, los parámetros se presentan como campos de texto para que pueda proporcionar los valores de entrada. Para cada comando, puede ver el script que se está ejecutando expandiendo **Ver script**. **RunPowerShellScript** es diferente de los otros comandos, ya que permite proporcionar sus propios scripts personalizados.

> [!NOTE]
> Los comandos integrados no son editables.

Una vez que se elige el comando, haga clic en **Ejecutar** para ejecutar el script. El script se ejecuta y, cuando haya finalizado, devuelve el resultado y los errores en la ventana de salida. La siguiente captura de pantalla muestra un ejemplo de salida tras ejecutar el comando **RDPSettings**.

![Salida del script del comando Ejecutar](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Comandos:

Esta tabla muestra la lista de comandos disponibles para máquinas virtuales Windows. El comando **RunPowerShellScript** se puede usar para ejecutar un script personalizado que desee.

|**Nombre**|**Descripción**|
|---|---|
|**RunPowerShellScript**|Ejecuta un script de PowerShell.|
|**EnableRemotePS**|Configura la máquina para habilitar PowerShell remoto.|
|**EnableAdminAccount**|Comprueba si la cuenta de administrador local está deshabilitada, y si lo está, la habilita.|
|**IPConfig**| Muestra información detallada de la dirección IP, la máscara de subred y la puerta de enlace predeterminada de cada adaptador enlazado a TCP/IP.|
|**RDPSettings**|Comprueba la configuración del registro y de la directiva de dominio. Recomienda acciones de directiva si la máquina forma parte de un dominio o modifica la configuración a los valores predeterminados.|
|**ResetRDPCert**|Quita el certificado SSL asociado al agente de escucha RDP y restaura la seguridad de este a los valores predeterminados. Use este script si ve algún problema con el certificado.|
|**SetRDPPort**|Establece el número de puerto especificado por el usuario o predeterminado para las conexiones del Escritorio remoto. Habilita la regla de firewall para el acceso entrante al puerto.|

## <a name="powershell"></a>PowerShell

A continuación, puede ver un ejemplo utilizando el cmdlet [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) para ejecutar un script de PowerShell en una máquina virtual de Azure.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Limitación del acceso al comando Ejecutar

Para enumerar los comandos Ejecutar o mostrar los detalles de un comando, se requieren el permiso `Microsoft.Compute/locations/runCommands/read`, que tienen el rol [Lector](../../role-based-access-control/built-in-roles.md#reader) integrado y otros superiores.

Para ejecutar un comando, se requiere el permiso `Microsoft.Compute/virtualMachines/runCommand/action`, que tienen el rol [Colaborador](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) y otros superiores.

Puede usar uno de los roles [integrados](../../role-based-access-control/built-in-roles.md) o crear uno [personalizado](../../role-based-access-control/custom-roles.md) para usar el comando Ejecutar.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Ejecución de scripts en la máquina virtual Windows](run-scripts-in-vm.md) para obtener información sobre otras maneras de ejecutar comandos y scripts de forma remota en la máquina virtual.
