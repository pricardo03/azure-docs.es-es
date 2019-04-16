---
title: Mi primer runbook de flujo de trabajo de PowerShell en Azure Automation
description: Tutorial que le guiará a través de la creación, prueba y publicación de un runbook de texto simple con Flujo de trabajo de PowerShell.
keywords: flujo de trabajo de powershell, ejemplos de flujo de trabajo de powershell, powershell para flujos de trabajo
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/24/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1697f479cf013f2ef94dd5a8a2fc637d72e6e18a
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549253"
---
# <a name="my-first-powershell-workflow-runbook"></a>Mi primer runbook de flujo de trabajo de PowerShell

> [!div class="op_single_selector"]
> * [Gráfico](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Flujo de trabajo de PowerShell](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Este tutorial le guiará para crear un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) en Azure Automation. Comenzará con un runbook simple que probará y publicará, mientras se explica cómo hacer un seguimiento del estado del trabajo del runbook. A continuación, puede modificar el runbook para administrar recursos de Azure, en este caso, iniciar una máquina virtual de Azure. Para finalizar, agregará parámetros de runbook para que el runbook sea más sólido.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará lo siguiente:

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Cuenta de Automation](automation-offering-get-started.md) para contener el Runbook y autenticarse en recursos de Azure.  Esta cuenta debe tener permiso para iniciar y detener la máquina virtual.
* Una máquina virtual de Azure. Para detener e iniciar esta máquina por lo que no debe ser una máquina virtual de producción.

## <a name="step-1---create-new-runbook"></a>Paso 1: crear nuevo runbook

Empieza creando un runbook simple cuya salida sea el texto *Hola mundo*.

1. En Azure Portal, abra su cuenta de Automation.

   La página de la cuenta de Automation proporciona una vista rápida de los recursos que hay en esa cuenta. Ya debería tener algunos recursos. Muchas de esos recursos son los módulos que se incluyen automáticamente en una cuenta nueva de Automation. También debe tener el recurso de credencial que se menciona en los [requisitos previos](#prerequisites).

1. Haga clic en **Runbooks** (en **Automatización de procesos**) para abrir la lista de runbooks.
1. Para crear un nuevo runbook, haga clic en el botón **+ Agregar un runbook** y luego en **Crear un nuevo runbook**.
1. Asigne al runbook el nombre *MyFirstRunbook-Workflow*.
1. En este caso, va a crear un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks), por lo tanto, seleccione **Flujo de trabajo de Powershell** en **Tipo de runbook**.
1. Haga clic en **Crear** para crear el runbook y abra el editor de texto.

## <a name="step-2---add-code-to-the-runbook"></a>Paso 2: Incorporación de código al runbook

Puede escribir el código directamente en el runbook o seleccionar los cmdlets, runbooks y recursos desde el control Biblioteca y agregarlos al runbook con los parámetros relacionados. En este tutorial, escribirá directamente en el runbook.

1. Su runbook está vacío actualmente, solo con la palabra clave *workflow*, el nombre del runbook y las llaves que encerrarán el flujo de trabajo completo.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. Escriba *Write-Output "Hello World."*  entre las llaves.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. Guarde el Runbook, para lo que debe hacer clic en **Guardar**.

## <a name="step-3---test-the-runbook"></a>Paso 3: probar el runbook

Antes de publicar el runbook para que esté disponible en producción, puede que quiera probarlo para asegurarse de que funciona correctamente. Cuando se prueba un runbook, se ejecuta su versión **Borrador** y se visualizan sus resultados de forma interactiva.

1. Haga clic en **Panel Prueba** para abrir el panel de prueba.
1. Haga clic en **Iniciar** para iniciar la prueba. Esta opción debe ser la única opción habilitada.
1. Se crea un [trabajo de runbook](automation-runbook-execution.md) y se muestra su estado.

   El estado del trabajo se iniciará como *en cola* que indica que está esperando un runbook worker en la nube disponible. Su estado cambia a *Iniciando* cuando un trabajo de runbook solicita el trabajo. Cuando el runbook comienza a ejecutarse realmente, el estado es *En ejecución*.  

1. Cuando se complete el trabajo del runbook, se mostrará su resultado. En este caso, debería ver *Hola mundo*.

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. Cierre el panel Prueba para volver al lienzo.

## <a name="step-4---publish-and-start-the-runbook"></a>Paso 4: publicar e iniciar el runbook

El runbook que acaba de crear aún está en modo de borrador. Antes de ejecutarlo en producción, debe publicarla. Al publicar un runbook, se sobrescribe la versión publicada existente con la versión de borrador. En este caso, no tiene una versión publicada aún porque acaba de crear el runbook.

1. Haga clic en **Publicar** para publicar el runbook y en **Sí** cuando se le solicite.
1. Si se desplaza a la izquierda para ver el runbook en el panel **Runbooks**, se muestra **Publicado** en **Estado de creación**.
1. Desplácese de nuevo a la derecha para ver el panel de **MyFirstRunbook-Workflow**.  
   Las opciones en la parte superior nos permiten iniciar el runbook, programarlo para que se inicie en algún momento en el futuro o crear un [webhook](automation-webhooks.md) para que se inicie a través de una llamada HTTP.
1. Solo tiene que iniciar el runbook, para ello debe hacer clic en **Iniciar** y en **Sí** cuando se le solicite.

   ![Iniciar runbook](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. Se abre un panel de trabajo para el trabajo de runbook que acaba de crear. Puede cerrar este panel, pero en este caso déjelo abierto para que pueda ver el progreso del trabajo.
1. El estado del trabajo se muestra en **Resumen del trabajo** y coincide con los estados que vio cuando probó el runbook.

   ![Resumen del trabajo](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. Cuando el estado del runbook aparezca como *Completado*, haga clic en **Salida**. Se abre el panel Salida y puede ver *Hola mundo*.

   ![Resumen del trabajo](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  

1. Cierre el panel Salida.
1. Haga clic en **Todos los registros** para abrir el panel Transmisiones para el trabajo de Runbook. solo debería ver *Hello World* en la salida stream, pero esta vista puede mostrar otras transmisiones de un trabajo de runbook como detallado y Error si el runbook escribe en ellas.

   ![Resumen del trabajo](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. Cierre la página de secuencias y la página de trabajo para volver a la página MyFirstRunbook.
1. Haga clic en **trabajos** para abrir la página de trabajos para este runbook. Esta página enumera todos los trabajos creados por este runbook. Solo debería ver un trabajo en la lista ya que solo ejecutó el trabajo una vez.

   ![Trabajos](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. Puede hacer clic en esta tarea para abrir la misma página de trabajo que vio cuando se inició el runbook. Esta acción permite volver atrás en el tiempo y ver los detalles de cualquier trabajo que se creó para un runbook determinado.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Paso 5: agregar autenticación para administrar recursos de Azure

Ha probado y publicado su runbook, pero hasta ahora no hace nada útil. Quiere que administre recursos de Azure. No puede hacer que sin embargo, a menos que se ha autenticado con las credenciales que se hace referencia en el [requisitos previos](#prerequisites). Esto se hace con el cmdlet **Connect-AzureRmAccount**.

1. Abra el editor de texto haciendo clic en **Editar** en el panel MyFirstRunbook-Workflow.
2. Ya no necesita la línea **Write-Output**, así que elimínela.
3. Coloque el cursor en una línea en blanco entre las llaves.
4. Escriba o copie y pegue el siguiente código que controlará la autenticación con la cuenta de ejecución de Automation:

   ```powershell-interactive
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $Conn.SubscriptionID
   ```

   > [!IMPORTANT]
   > **Add-AzureRmAccount** y **Login-AzureRmAccount** ahora son alias de **Connect-AzureRMAccount**. Si el cmdlet **Connect-AzureRMAccount** no existe, puede usar **Add-AzureRmAccount** o **Login-AzureRmAccount**, o bien puede [actualizar los módulos](automation-update-azure-modules.md) en su cuenta de Automation a las versiones más recientes.

> [!NOTE]
> Es posible que deba [actualizar los módulos](automation-update-azure-modules.md) incluso si acaba de crear una nueva cuenta de Automation.

1. Haga clic en el **panel de prueba** para poder probar el runbook.
1. Haga clic en **Iniciar** para iniciar la prueba. Cuando termine, recibirá unos resultados similares a los siguientes, que muestran información básica de su cuenta. Esta acción confirma que la credencial es válida.

   ![Autenticar](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Paso 6: Incorporación de una actividad para iniciar una máquina virtual

Ahora que el runbook está autenticado en la suscripción a Azure, puede administrar los recursos. Agregue un comando para iniciar una máquina virtual. Puede seleccionar cualquier máquina virtual en su suscripción de Azure y, por ahora podrá codificar ese nombre en el runbook. Si está administrando los recursos en varias suscripciones, deberá usar el **- AzureRmContext** parámetro junto con [Get-AzureRmContext](/powershell/module/azurerm.profile/get-azurermcontext).

1. Tras *Connect-AzureRmAccount*, escriba *Start-AzureRmVM -Name "NombreVM" -ResourceGroupName "NombreGrupoRecursos"* y proporcione el nombre y el nombre del grupo de recursos de la máquina virtual que quiere iniciar.  

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $Conn.SubscriptionID

   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName' -AzureRmContext $AzureContext
   }
   ```

1. Guarde el runbook y haga clic en **Panel de prueba** para probarlo.
1. Haga clic en **Iniciar** para iniciar la prueba. Cuando haya terminado, compruebe que la máquina virtual se ha iniciado.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Paso 7: agregar un parámetro de entrada al runbook

Actualmente, el runbook inicia la máquina virtual que codificó en el runbook, pero sería más útil si especificara la máquina virtual al iniciar el runbook. Agregue parámetros de entrada al runbook para proporcionar esa funcionalidad.

1. Agregue parámetros para *NombreVM* y *NombreGrupoRecursos* al runbook y use estas variables con el cmdlet **Start-AzureRmVM** como se muestra en el siguiente ejemplo.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```

2. Guarde el runbook y abra el panel Prueba. Ahora puede proporcionar valores para las dos variables de entrada que se usarán en la prueba.
3. Cierre el panel Prueba.
4. Haga clic en **Publicar** para publicar la nueva versión del runbook.
5. Detenga la máquina virtual que inició en el paso anterior.
6. Haga clic en **Iniciar** para iniciar el runbook. Escriba el valor de **VMName** y **ResourceGroupName** para la máquina virtual que va a iniciar.

   ![Inicio del runbook](media/automation-first-runbook-textual/automation-pass-params.png)

7. Cuando se complete el runbook, compruebe que la máquina virtual se ha iniciado.  

## <a name="next-steps"></a>Pasos siguientes

* Para empezar a trabajar con runbooks gráficos, consulte [Mi primer runbook gráfico](automation-first-runbook-graphical.md)
* Para empezar a trabajar con runbooks de PowerShell, consulte [Mi primer runbook de PowerShell](automation-first-runbook-textual-powershell.md)
* Para más información sobre los tipos de Runbook, sus ventajas y sus limitaciones, consulte [Tipos de runbooks de Azure Automation](automation-runbook-types.md)
* Para más información sobre la característica de compatibilidad con scripts de PowerShell, consulte [Announcing Native PowerShell Script Support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
