---
title: Configuración de entornos de ensayo para aplicaciones web en Azure App Service | Microsoft Docs
description: Aprenda a utilizar la publicación de ensayo para aplicaciones web en Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: jpconnoc
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: cephalin
ms.openlocfilehash: d62632d6c28ac137095307e95dbbdab7e8573bbc
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65137875"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Configuración de entornos de ensayo en Azure App Service
<a name="Overview"></a>

> [!NOTE]
> Esta guía paso a paso muestra cómo administrar espacios mediante una nueva página de administración de versiones preliminares. Los clientes acostumbrados a la página de administración existente pueden seguir usando la página de administración de espacios existente como antes. 
>

Al implementar la aplicación web, la aplicación web en Linux, el back-end móvil y la aplicación API en [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), puede implementarlas en una ranura de implementación independiente en lugar de en la ranura de producción predeterminada si realiza la ejecución en el nivel de plan **Estándar**, **Premium** o **Aislado** de App Service. Las ranuras de implementación son realmente aplicaciones activas con sus propios nombres de host. Los elementos de contenido y configuraciones de aplicaciones se pueden intercambiar entre dos ranuras de implementación, incluida la ranura de producción. La implementación de la aplicación en un espacio que no sea de producción ofrece las siguientes ventajas:

* Puede validar los cambios en la aplicación en una ranura de implementación de ensayo antes de intercambiarla con la ranura de producción.
* La implementación de una aplicación en un espacio y su posterior paso a producción garantiza que todas las instancias del espacio están preparadas antes de dicho paso a producción. Esto elimina tiempos de inactividad a la hora de implementar la aplicación. El redireccionamiento del tráfico es perfecta y no se pierde ninguna solicitud en las operaciones de intercambio. Todo este flujo de trabajo se puede automatizar mediante la configuración de [Intercambio automático](#Auto-Swap) cuando no sea necesario realizar ninguna validación antes del intercambio.
* Después del intercambio, la ranura con la aplicación de ensayo anterior ahora ocupa la aplicación de producción anterior. Si las modificaciones que se han intercambiado en el espacio de producción no son los que esperaba, puede volver a realizar un intercambio inmediatamente para tener el "último sitio que sabe que funciona correctamente".

Cada nivel del plan de App Service admite un número distinto de ranuras de implementación. Para averiguar el número de ranuras que admite el nivel de la aplicación, consulte [Límites de App Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Para escalar la aplicación a un nivel diferente, el nivel de destino debe admitir el número de ranuras que la aplicación ya usa. Por ejemplo, si la aplicación tiene más de cinco espacios, no se puede reducir verticalmente al nivel **Estándar**, ya que dicho nivel solo admite cinco espacios de implementación.

<a name="Add"></a>

## <a name="add-slot"></a>Incorporación de espacios
Para poder habilitar varias espacios de implementación, la aplicación debe ejecutarse en los niveles **Estándar**, **Premium** o **Aislado**.

1. En [Azure Portal](https://portal.azure.com/), abra la [página de recursos](../azure-resource-manager/manage-resources-portal.md#manage-resources) de la aplicación.

2. En el panel de navegación izquierdo, elija la opción **Espacios de implementación (versión preliminar)** y haga clic en **Agregar espacio**.
   
    ![Agregar una nueva ranura de implementación](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Si la aplicación no está aún en los niveles **Estándar**, **Premium** o **Aislado**, recibirá un mensaje que indica los niveles compatibles para habilitar la publicación almacenada provisionalmente. Llegados a este punto, tiene la opción de seleccionar **Actualizar** e ir a la pestaña **Escala** de la aplicación antes de continuar.
   > 

3. En el cuadro de diálogo **Agregar espacio**, asigne un nombre al espacio y seleccione si desea que la configuración de la aplicación se clone de otro espacio de implementación existente. Haga clic en **Agregar** para continuar.
   
    ![Origen de configuración](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    La configuración se puede clonar de un espacio existente. La configuración que se puede clonar incluye los valores de la aplicación, las cadenas de conexión, versiones del marco del lenguaje, los sockets web, la versión de HTTP y el valor de bits de la plataforma.

4. Tras agregar el espacio, haga clic en **Cerrar** para cerrar el cuadro de diálogo. El nuevo espacio se muestra en la página **Espacios de implementación (versión preliminar)**. De forma predeterminada, en el espacio nuevo el valor de **Tráfico %** es 0, y todo el tráfico de los clientes se enruta al espacio de producción.

5. Haga clic en el nuevo espacio de implementación para abrir su página de recursos.
   
    ![Título de la ranura de implementación](./media/web-sites-staged-publishing/StagingTitle.png)

    Como las restantes aplicaciones de App Service, el espacio de ensayo tiene una página de administración. La configuración del espacio se puede cambiar. El nombre del espacio se muestra en la parte superior de la página para recordarle que está viendo el espacio de implementación.

6. Haga clic en la dirección URL de la aplicación en la página de recursos del espacio. El espacio de implementación tiene su propio nombre de host y es también una aplicación activa. Para limitar el acceso público al espacio de implementación, consulte [Restricciones de IP estáticas de Azure App Service](app-service-ip-restrictions.md).

El nuevo espacio de implementación no tiene contenido, aunque se clone la configuración de otro espacio. Por ejemplo, [puede publicar en esta ranura mediante Git](app-service-deploy-local-git.md). La implementación en el espacio se puede realizar desde otra rama del repositorio o desde otro repositorio. 

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>¿Qué configuración se intercambia?
Cuando crea un clon de la configuración de otro espacio de implementación, la configuración clonada se puede editar. Además, algunos elementos de configuración siguen al contenido en los intercambios (no son específicos del espacio), mientras que otros permanecen en el mismo espacio después de un intercambio (específicos del espacio). Las listas siguientes muestran la configuración que cambia cuando se intercambian las ranuras.

**Configuraciones que se intercambian**:

* Configuración general: por ejemplo, versión de framework, 32 o 64 bits, Web Sockets
* Configuración de la aplicación (puede configurarse para ajustarse a un espacio)
* Cadenas de conexión (puede configurarse para ajustarse a un espacio)
* Asignaciones de controlador
* Configuración de supervisión y diagnóstico
* Certificados públicos
* Contenido de WebJobs
* Conexiones híbridas *
* Integración con red virtual *
* Los extremos del servicio *
* Red CDN de Azure *

Las características que se marcan con un * se prevé que se realizan rápidas en la ranura. 

**Valores que no se intercambian**:

* Extremos de publicación
* Nombres de dominio personalizados
* Certificados privados y enlaces SSL
* Configuración de escala
* Programadores de WebJobs
* Restricciones de IP
* Always On
* Configuración del protocolo (HTTP**S**, versión de TLS, certificados de cliente)
* Configuración del registro de diagnóstico
* CORS

<!-- VNET and hybrid connections not yet sticky to slot -->

Para configurar que un valor de una aplicación o una cadena de conexión se quede en un espacio (que no se intercambie), vaya a la página **Configuración de la aplicación** de dicho espacio específico y seleccione el cuadro **Configuración de espacios** en los elementos de la configuración que deben permanecer en el espacio. Marcar un elemento de la configuración como específico de un espacio indica a App Service que no se puede intercambiar. 

![Configuración de espacios](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>Intercambio de dos espacios 
Los espacios de implementación se pueden intercambiar en la página **Espacios de implementación (versión preliminar)** de la aplicación. 

También se pueden intercambiar desde las páginas **Información general** y **Espacios de implementación**, pero así es como se hacía antes. En esta guía se muestra cómo usar la nueva interfaz de usuario de la página **Espacios de implementación (versión preliminar)**.

> [!IMPORTANT]
> Antes de cambiar una aplicación de un espacio de implementación a producción, asegúrese de que todos los valores están configurados exactamente como desea que estén en el destino de intercambio.
> 
> 

Para intercambiar espacios de implementación, siga estos pasos:

1. Vaya a la página **Espacios de implementación (versión preliminar)** y haga clic en **Cambiar**.
   
    ![Botón de cambio](./media/web-sites-staged-publishing/SwapButtonBar.png)

    El cuadro de diálogo **Cambiar** muestra los valores de los espacios de origen y de destino seleccionados que van a cambiar.

2. Seleccione los espacios **Origen** y **Destino**. Por lo general, el destino suele ser el espacio de producción. Además, haga clic en las pestañas **Cambios de origen** y **Cambios de destino**, y compruebe que los cambios en la configuración son los esperados. En cuanto termine, puede intercambiar los espacios. Para ello solo debe hacer clic en **Cambiar**.

    ![Intercambio completo](./media/web-sites-staged-publishing/SwapImmediately.png)

    Para ver cómo funcionaría el espacio de destino con la nueva configuración antes de que se realice el intercambio, no haga clic en **Cambiar**, siga las instrucciones de [Intercambio con vista previa](#Multi-Phase).

3. Cuando haya terminado, haga clic en **Cerrar** para cerrar el cuadro de diálogo.

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>Intercambio con vista previa (intercambio en varias fases)

> [!NOTE]
> El intercambio con vista previa no se admite en las aplicaciones web en Linux.

Antes de usar producción como espacio de destino, valide que la aplicación se ejecuta con la nueva configuración antes de que se produzca el intercambio. El espacio de origen también se ha preparado antes de que finalice el intercambio, algo que también es conveniente para aplicaciones críticas.

Al iniciar un intercambio con vista previa, App Service realiza las siguientes acciones:

- Mantiene el espacio de destino sin cambios para que su carga de trabajo (por ejemplo, producción) no se vea afectada.
- Aplica los elementos de configuración de el espacio de destino al espacio de origen, incluidas las cadenas de conexión específicas del espacio y la configuración de la aplicación.
- Reinicia los procesos de trabajo del espacio de origen, para lo que usa con estos elementos de configuración. Puede examinar el espacio de origen y ver cómo se ejecuta la aplicación con los cambios en la configuración.

Si completa el intercambio en un paso independiente, App Service mueve el espacio de origen preparado al espacio de destino y este al espacio de origen. Si cancela el intercambio, App Service vuelve a aplicar los elementos de configuración del espacio de origen en el espacio de origen.

Para realizar un intercambio con vista previa, siga estos pasos.

1. Siga los pasos que se indican en [Intercambio de espacios de implementación](#Swap), pero seleccione **Realizar intercambio con versión preliminar** .

    ![Intercambio con vista previa](./media/web-sites-staged-publishing/SwapWithPreview.png)

    El cuadro de diálogo muestra cómo cambia la configuración del espacio de origen en la fase 1 y cómo cambian tanto el espacio de origen como la de destino en la fase 2.

2. Cuando esté listo para iniciar el intercambio, haga clic en **Iniciar intercambio**.

    Cuando se complete la fase 1, se le notificará en el cuadro de diálogo. Obtenga una vista previa del intercambio en el espacio de origen, para lo que debe ir a `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Cuando esté listo para completar el intercambio pendiente, seleccione **Completar intercambio** en **Acción de intercambio** y haga clic en **Cancelar intercambio**.

    Para cancelar un intercambio pendiente, seleccione **Cancelar intercambio** y haga clic en **Cancelar intercambio**.

4. Cuando haya terminado, haga clic en **Cerrar** para cerrar el cuadro de diálogo.

Para automatizar un intercambio en varias fases, consulte Automatización con PowerShell.

<a name="Rollback"></a>

## <a name="roll-back-swap"></a>Reversión de intercambios
Si aparecen errores en el espacio de destino (por ejemplo, el espacio de producción) después de un intercambio de espacios, restaure los espacios al estado que tenían antes del intercambio. Para ello, vuelva a intercambiarlos de inmediato.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Configuración de intercambio automático

> [!NOTE]
> El intercambio automático no se admite en aplicaciones web en Linux.

El intercambio automático optimiza los escenarios de DevOps en los que se desee implementar una aplicación continuamente sin arranques en frío ni tiempos de inactividad para los clientes finales de la aplicación. Cuando un espacio se intercambia automáticamente a producción, cada vez que se insertan los cambios de código en dicho espacio, App Service cambia automáticamente la aplicación a producción después de que se haya preparado en el espacio de origen.

   > [!NOTE]
   > Antes de configurar el intercambio automático en el espacio de producción, considere la posibilidad de probarlo en un espacio de destino que no sea de producción.
   > 

Para configurar el intercambio automático, siga estos pasos:

1. Vaya a la página de recursos de la aplicación. Seleccione **Espacios de implementación (versión preliminar)** > *\<espacio de origen deseado >* > **Configuración de la aplicación**.
   
2. En **Intercambio automático**, seleccione **Activado** y, después, elija el espacio de destino que desee en **Espacio de intercambio automático** y haga clic en **Guardar** en la barra de comandos. 
   
    ![](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Ejecute una inserción de código en el espacio de origen. El intercambio automático se realiza después de un breve tiempo y la actualización se refleja en la dirección URL de la ranura de destino.

<a name="Warm-up"></a>

## <a name="custom-warm-up"></a>Preparación personalizada
Al usar [Intercambio automático](#Auto-Swap), algunas aplicaciones pueden requerir acciones de preparación personalizadas antes del intercambio. El elemento de configuración `applicationInitialization` de web.config permite especificar las acciones de inicialización personalizadas que se van a realizar. La operación espera hasta que se completa esta preparación personalizada antes de realizar el intercambio con el espacio de destino. He aquí un fragmento de ejemplo del archivo web.config.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Para obtener más información acerca de cómo personalizar el `applicationInitialization` elemento, vea [errores de intercambio de ranura de implementación más comunes y cómo corregirlos](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/).

También puede personalizar el comportamiento de la preparación con una o más de las siguientes [opciones de configuración de la aplicación](web-sites-configure.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: la ruta de acceso para hacer ping y así preparar el sitio. Agregue esta configuración de aplicación especificando una ruta de acceso personalizada que comience con una barra diagonal como valor. Por ejemplo, `/statuscheck`. El valor predeterminado es `/`. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: códigos de respuesta HTTP válidos para la operación de preparación. Agregue esta configuración de aplicación con una lista de códigos HTTP separados por comas. Por ejemplo: `200,202`. Si el código de estado devuelto no está en la lista, las operaciones de preparación e intercambio se detienen. Por defecto, todos los códigos de respuesta son válidos.

## <a name="monitor-swap"></a>Supervisión del intercambio

Si la operación de intercambio tarda mucho tiempo en completarse, puede obtener información acerca de ella en el [registro de actividad](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

En la página de recursos de la aplicación, en el panel de navegación izquierdo, seleccione **Registro de actividad**.

Una operación de intercambio aparece en la consulta de registro como `Swap Web App Slots`. Puede expandirla y seleccionar una de los suboperaciones o errores para ver los detalles.

## <a name="route-traffic"></a>Enrutamiento del tráfico

De forma predeterminada, todas las solicitudes que realiza el cliente a la dirección URL de producción de la aplicación (`http://<app_name>.azurewebsites.net`) se enrutan al espacio de producción. Una parte del tráfico se puede enrutar a otro espacio. Esta característica es útil si se necesitan los comentarios de los usuarios al respecto de una nueva actualización, pero dicha actualización aún no está lista para enviarla a producción.

### <a name="route-production-traffic-automatically"></a>Enrutamiento automático del tráfico de producción

Para enrutar el tráfico de producción de manera automática, siga estos pasos:

1. Vaya a la página de recursos de la aplicación y seleccione **Espacios de implementación (versión preliminar)**.

2. En la columna **Tráfico %** del espacio que se desea enrutar, especifique un porcentaje (entre 0 y 100) que represente la cantidad del tráfico total que desea enrutar. Haga clic en **Save**(Guardar).

    ![](./media/web-sites-staged-publishing/RouteTraffic.png)

Una vez que el valor se guarda, el porcentaje especificado de clientes se enruta aleatoriamente al espacio que no es de producción. 

Una vez un cliente se enruta automáticamente a un espacio concreto, "permanece anclado" en dicho espacio todo el tiempo que dure su sesión. En el explorador del cliente, para ver a qué espacio está anclada la sesión debe examinar la cookie `x-ms-routing-name` en los encabezados HTTP. Las solicitudes que se enrutan a al espacio "de ensayo" tienen la cookie `x-ms-routing-name=staging`. Las solicitudes que se enrutan al espacio de producción tienen la cookie `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>Enrutamiento manual del tráfico de producción

Además del enrutamiento automático del tráfico, App Service puede enrutar solicitudes a un espacio concreto. Esto es útil cuando desee que los usuarios puedan escoger el enrutamiento a la aplicación beta, o la exclusión voluntaria de la misma. Para enrutar el tráfico de producción de manera manual se usa el parámetro de consulta `x-ms-routing-name`.

Para permitir que los usuarios opten por la exclusión del enrutamiento a la aplicación beta, por ejemplo, puede poner este vínculo en la página web:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

La cadena `x-ms-routing-name=self` especifica la ranura de producción. Una vez que el explorador del cliente accede al vínculo, no solo se redirige al espacio de producción, sino también cada solicitud posterior tiene la cookie `x-ms-routing-name=self` que ancla la sesión al espacio de producción.

Para permitir a los usuarios participar en la aplicación de la versión beta, establezca el mismo parámetro de consulta con el nombre de la ranura que no es de producción, por ejemplo:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

De forma predeterminada, las ranuras de nuevo tienen una regla de enrutamiento de `0%`, como se muestra en gris. Si se establece explícitamente este valor `0%` (se muestra en texto negro), los usuarios pueden tener acceso a la ranura de ensayo manualmente mediante el `x-ms-routing-name` parámetro de consulta, pero no se enrutarán a la ranura automáticamente ya que el porcentaje de enrutamiento está establecido en 0. Se trata de un escenario avanzado, donde puede "Ocultar" su ranura de ensayo del público permitiendo que los equipos internos probar los cambios en la ranura.

<a name="Delete"></a>

## <a name="delete-slot"></a>Eliminación de espacios

Vaya a la página de recursos de la aplicación. Seleccione **Espacios de implementación (versión preliminar)** > *\<espacio que se elimina >* > **Información general**. En la barra de comandos, haga clic en **Eliminar**.  

![Eliminación de una ranura de implementación](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>Automatización con PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell es un módulo que proporciona cmdlets para administrar Azure mediante Windows PowerShell, incluida la compatibilidad para administrar ranuras de implementación de Azure App Service.

Para obtener información acerca de cómo instalar y configurar Azure PowerShell y cómo autenticar Azure PowerShell con su suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-web-app"></a>Crear una aplicación web
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-slot"></a>Creación de espacios
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Inicio de un intercambio con vista previa (intercambio en varias fases) y aplicación de la configuración del espacio de destino al espacio de origen
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Cancelación de un intercambio pendiente (intercambio con vista previa) y restauración de la configuración del espacio de origen
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Intercambio de ranuras de implementación
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Supervisión de eventos de intercambio en el registro de actividad
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-slot"></a>Eliminación de espacios
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-cli"></a>Automatización con la interfaz de la línea de comandos

Para información sobre los comandos de la [CLI de Azure](https://github.com/Azure/azure-cli) que se usan con las ranuras de implementación, consulte [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Pasos siguientes
[Bloqueo del acceso a espacios que no sean de producción](app-service-ip-restrictions.md)
