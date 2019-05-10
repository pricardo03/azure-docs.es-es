---
title: La depuración local de PowerShell de Azure Functions
description: Descubra cómo desarrollar funciones mediante el uso de PowerShell.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha, glenga
ms.openlocfilehash: b699379448863c8df84fda0e059fc10846c09931
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230077"
---
# <a name="debug-powershell-azure-functions-locally"></a>La depuración local de PowerShell de Azure Functions

Azure Functions permite desarrollar sus funciones como scripts de PowerShell.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

Puede depurar localmente las funciones de PowerShell como lo haría con cualquier script de PowerShell con las siguientes herramientas de desarrollo estándar:

* [Visual Studio Code](https://code.visualstudio.com/): Editor de texto gratuitas, ligeras y código abierto de Microsoft con la extensión de PowerShell que ofrece una completa experiencia de desarrollo de PowerShell.
* Una consola de PowerShell: Depurar con los mismos comandos que utilizaría para depurar cualquier otro proceso de PowerShell.

[Azure Functions Core Tools](functions-run-local.md) admite la depuración local de Azure Functions, incluidas las funciones de PowerShell.

## <a name="example-function-app"></a>Aplicación de función de ejemplo

La aplicación de función usada en este artículo tiene una sola función desencadenada por HTTP y tiene los siguientes archivos:

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

Esta aplicación de función es similar a la que se obtiene al completar el [inicio rápido de PowerShell](functions-create-first-function-powershell.md).

El código de función en `run.ps1` similar a la secuencia de comandos siguiente:

```powershell
param($Request)

$name = $Request.Query.Name

if($name) {
    $status = 200
    $body = "Hello $name"
}
else {
    $status = 400
    $body = "Please pass a name on the query string or in the request body."
}

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```

## <a name="set-the-attach-point"></a>Establecer el punto de adición

Para depurar cualquier función de PowerShell, la función se debe detener para que se puede adjuntar el depurador. El `Wait-Debugger` cmdlet detiene la ejecución y espera a que el depurador.

Todo lo que necesita hacer es agregar una llamada a la `Wait-Debugger` cmdlet justo encima el `if` instrucción, como se indica a continuación:

```powershell
param($Request)

$name = $Request.Query.Name

# This is where we will wait for the debugger to attach
Wait-Debugger

if($name) {
    $status = 200
    $body = "Hello $name"
}
# ...
```

Se inicia en la depuración el `if` instrucción. 

Con `Wait-Debugger` en su lugar, ahora puede depurar las funciones con Visual Studio Code o una consola de PowerShell.

## <a name="debug-in-visual-studio-code"></a>Depurar en Visual Studio Code

Para depurar sus funciones de PowerShell en Visual Studio Code, debe tener las siguientes extensiones para Visual Studio Code:

* [PowerShell](/powershell/scripting/components/vscode/using-vscode)
* [Azure Functions](functions-create-first-function-vs-code.md)

Después de instalar las extensiones de PowerShell y Azure Functions, cargar un proyecto de aplicación de función existente. También puede [crear un proyecto de Functions](functions-create-first-function-vs-code.md).

>[!NOTE]
> Debe tener el proyecto no los archivos de configuración necesarios, deberá agregarlos.

### <a name="start-the-function-app"></a>Iniciar la aplicación de función

Compruebe que `Wait-Debugger` está establecido en la función en la que desee asociar el depurador.  Con `Wait-Debugger` agregado, puede depurar la aplicación de función mediante Visual Studio Code.

Elija la **depurar** panel y, a continuación, **adjuntar a la función de PowerShell**.

![Depurador](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

También puede presionar la tecla F5 para iniciar la depuración.

El inicio de depuración de la operación realiza las siguientes tareas:

* Ejecuciones `func extensions install` en el terminal para instalar las extensiones de Azure Functions requeridas por la aplicación de función.
* Ejecuciones `func host start` en el terminal para iniciar la aplicación de función en el host de Functions.
* Asociar al depurador de PowerShell para el proceso de PowerShell en el tiempo de ejecución de funciones.

Con la aplicación de función se ejecute, necesita una consola de PowerShell independiente para llamar a la función desencadenada por HTTP.

En este caso, la consola de PowerShell es el cliente. El `Invoke-RestMethod` se usa para desencadenar la función.

En una consola de PowerShell, ejecute el siguiente comando:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Observará que no se devuelve inmediatamente una respuesta. Eso es porque `Wait-Debugger` ha adjuntado el depurador y PowerShell ejecución entró en modo de interrupción tan pronto como se haría. Esto es debido el [BreakAll concepto](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place), que se explica más adelante. Después de presionar el `continue` botón, el depurador ahora se interrumpe en la línea inmediatamente después `Wait-Debugger`.

En este momento, el depurador está conectado y puede hacer a todo el depurador normal de operaciones. Para obtener más información sobre cómo utilizar el depurador en Visual Studio Code, consulte [la documentación oficial](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions).

Después de seguir y totalmente invocar el script, observará:

* La consola de PowerShell que hizo el `Invoke-RestMethod` ha devuelto un resultado
* La consola de PowerShell integrado en Visual Studio Code está esperando a que se ejecutará una secuencia de comandos

Posteriormente, cuando se invoca la misma función, el depurador de PowerShell de extensión se interrumpe inmediatamente después del `Wait-Debugger`.

## <a name="debugging-in-a-powershell-console"></a>Depuración en una consola de PowerShell

>[!NOTE]
> En esta sección se supone que ha leído el [docs de Azure Functions Core Tools](functions-run-local.md) y saber cómo usar el `func host start` comando para iniciar la aplicación de función.

Abra una consola, `cd` en el directorio de la aplicación de función y ejecute el comando siguiente:

```sh
func host start
```

Con la aplicación de función que se ejecuta y el `Wait-Debugger` en su lugar, puede asociar al proceso. Necesita dos más consolas de PowerShell.

Una de las consolas actúa como el cliente. De este modo, se llama a `Invoke-RestMethod` para desencadenar la función. Por ejemplo, puede ejecutar el comando siguiente:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Observará que no devuelve una respuesta, que es el resultado de la `Wait-Debugger`. El proceso de PowerShell ahora está esperando un depurador adjuntarlo. Vamos a poner que adjunta.

En la consola de PowerShell, ejecute el siguiente comando:

```powershell
Get-PSHostProcessInfo
```

Este cmdlet devuelve una tabla que es similar a la siguiente salida:

```output
ProcessName ProcessId AppDomainName
----------- --------- -------------
dotnet          49988 None
pwsh            43796 None
pwsh            49970 None
pwsh             3533 None
pwsh            79544 None
pwsh            34881 None
pwsh            32071 None
pwsh            88785 None
```

Tome nota de la `ProcessId` para el elemento en la tabla con el `ProcessName` como `dotnet`. Este proceso es la aplicación de función.

A continuación, ejecute el siguiente fragmento de código:

```powershell
# This enters into the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

Una vez iniciado, el depurador se interrumpe y muestra algo parecido a lo siguiente:

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

En este momento, se ha detenido en un punto de interrupción en el [depurador de PowerShell](/powershell/module/microsoft.powershell.core/about/about_debuggers). Desde aquí, puede realizar todas las operaciones de depuración usuales, paso a paso, paso a paso, continuar, salir y otros. Para ver el conjunto completo de comandos de depuración disponibles en la consola, ejecute el `h` o `?` comandos.

También puede establecer puntos de interrupción en este nivel con el `Set-PSBreakpoint` cmdlet.

Una vez que continúa y totalmente invocar el script, observará:

* La consola de PowerShell que ejecutó `Invoke-RestMethod` ahora se ha devuelto un resultado.
* La consola de PowerShell que ejecutó `Debug-Runspace` está esperando un script que se ejecutará.

Puede invocar la misma función nuevo (con `Invoke-RestMethod` por ejemplo) y el depurador se interrumpe justo después del `Wait-Debugger` comando.

## <a name="considerations-for-debugging"></a>Consideraciones para la depuración

Tenga en cuenta los siguientes problemas al depurar el código de funciones.

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll` puede provocar que el depurador se interrumpa en un lugar inesperado

Los usos de la extensión de PowerShell `Debug-Runspace`, que a su vez se basa en PowerShell `BreakAll` característica. Esta característica indica a PowerShell para detenerse en el primer comando que se ejecuta. Este comportamiento le ofrece la oportunidad de establecer puntos de interrupción en el proceso depurado.

El tiempo de ejecución de Azure Functions ejecuta algunos comandos antes de invocar realmente la `run.ps1` de script, por lo que es posible que el depurador termina importantes dentro de la `Microsoft.Azure.Functions.PowerShellWorker.psm1` o `Microsoft.Azure.Functions.PowerShellWorker.psd1`.

¿Debe suceder esta interrupción, ejecute el `continue` o `c` comando saltarse hasta este punto de interrupción. A continuación, detenga el punto de interrupción esperada.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo desarrollar funciones mediante PowerShell, consulte [Guía del desarrollador de Azure PowerShell de funciones](functions-reference-powershell.md).
