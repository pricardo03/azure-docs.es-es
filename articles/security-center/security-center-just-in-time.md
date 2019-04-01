---
title: Acceso a máquina virtual Just-In-Time en Azure Security Center | Microsoft Docs
description: En este documento se explica la forma en que el acceso a máquina virtual del tipo Just-In-Time en Azure Security Center le ayuda a controlar el acceso a máquinas virtuales de Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: monhaber
ms.openlocfilehash: 66a7171aff7b9bab5f320df1d71e9cab4ce0477d
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758295"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Administración del acceso a máquina virtual mediante Just-In-Time

El acceso a máquina virtual (VM) Just-In-Time (JIT) se puede usar para bloquear el tráfico entrante a las máquinas virtuales de Azure, lo que reduce la exposición a ataques y, al mismo tiempo, se proporciona acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario.

> [!NOTE]
> La característica Just-In-Time está disponible en el nivel Estándar de Security Center.  Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
>
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="attack-scenario"></a>Escenario de ataque

Normalmente, los ataques por fuerza bruta tienen como destino los puertos de administración para obtener acceso a una máquina virtual. Si un atacante tiene éxito, puede tomar el control de la máquina virtual y establecer un punto de apoyo en su entorno.

Una manera de reducir el riesgo de sufrir un ataque por fuerza bruta consiste en limitar el tiempo que está abierto un puerto. No es necesario que los puertos de administración estén abiertos en todo momento. Solo deben estar abiertos mientras se está conectado a la máquina virtual, por ejemplo, para realizar tareas de administración o mantenimiento. Cuando se habilita Just-In-Time, Security Center usa las reglas del [grupo de seguridad de red](../virtual-network/security-overview.md#security-rules) (NSG), que restringen el acceso a los puertos de administración para que no puedan ser objeto de ataques.

![Escenario de Just-in-time](./media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>¿Cómo funciona el acceso JIT?

Cuando se habilita Just-In-Time, Security Center bloquea el tráfico entrante a las máquinas virtuales de Azure mediante la creación de una regla de NSG. Se deben seleccionar los puertos de la máquina virtual para la que se bloqueará el tráfico entrante. Estos puertos los controla la solución Just-In-Time.

Cuando un usuario solicita acceso a una máquina virtual, Security Center comprueba que el usuario tiene permisos de [control de acceso basado en rol (RBAC)](../role-based-access-control/role-assignments-portal.md) que le permite solicitar correctamente acceso a una máquina virtual. Si la solicitud se aprueba, Security Center configura automáticamente los grupos de seguridad de red (NSG) para permitir el tráfico entrante a los puertos seleccionados y a las direcciones IP o a los rangos de origen durante el periodo especificado. Una vez transcurrido ese tiempo, Security Center restaura los NSG a su estado anterior. Pero las conexiones que ya están establecidas no se interrumpen.

> [!NOTE]
> Actualmente, el acceso a máquina virtual del tipo Just-In-Time de Security Center solo admite las máquinas virtuales implementadas a través de Azure Resource Manager. Para obtener más información sobre los modelos de implementación clásico y de Resource Manager, vea [La implementación de Azure Resource Manager frente a la implementación clásica](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

A JIT se puede acceder a través de:
- [Uso del acceso JIT en Azure Security Center](#jit-asc)
- [Uso del acceso JIT en una hoja de máquina virtual de Azure](#jit-vm)

## Uso del acceso JIT en Azure Security Center<a name="jit-asc"></a>

1. Abra el panel **Security Center**.

2. En el panel izquierdo, seleccione **Just-in-time VM access** (Acceso a máquina virtual del tipo Just-In-Time).

![Icono de acceso a máquina virtual del tipo Just-In-Time](./media/security-center-just-in-time/just-in-time.png)

Se abre la ventana **Just-in-time VM access** (Acceso a máquina virtual del tipo Just-In-Time).

![Icono de acceso a máquina virtual del tipo Just-In-Time](./media/security-center-just-in-time/just-in-time-access.png)

**Just-in-time VM access** (Acceso a máquina virtual del tipo Just-In-Time) proporciona información acerca del estado de las máquinas virtuales:

- **Configurado**: máquinas virtuales que se han configurado para admitir el acceso a máquina virtual Just-In-Time. Los datos que se muestran son de la última semana e incluyen, para cada máquina virtual, el número de solicitudes aprobadas, la fecha y hora del último acceso y el último usuario.
- **Recomendado**: máquinas virtuales que pueden admitir el acceso a máquina virtual Just-In-Time, pero que no se han configurado para ello. Se recomienda habilitar el control de acceso a máquina virtual Just-In-Time para estas máquinas virtuales. Consulte [Configuración de una directiva de acceso Just-In-Time](#jit-config).
- **Ninguna recomendación**: una máquina virtual podría no recomendarse por diversas razones:
  - Falta grupo de seguridad de red: la solución Just-In-Time requiere que exista un grupo de seguridad de red.
  - Máquina virtual clásica: en la actualidad, el acceso a máquina virtual Just-In-Time de Security Center solo admite las máquinas virtuales implementadas mediante Azure Resource Manager. La solución Just-In-Time no admite las implementaciones clásicas.
  - Otros: una máquina virtual de esta categoría si la solución Just-In-Time está desactivada en la directiva de seguridad de la suscripción o del grupo de recursos, o si la máquina virtual no tiene una IP pública y no existe ningún grupo de seguridad de red.

### Configuración de una directiva de acceso JIT<a name="jit-config"></a>

Para seleccionar las máquinas virtuales que quiere habilitar:

1. En **Just-in-time VM access** (Acceso a máquina virtual Just-In-Time), seleccione la pestaña **Recomendado**.

   ![Habilitar acceso Just-In-Time](./media/security-center-just-in-time/enable-just-in-time-access.png)

2. En **MÁQUINA VIRTUAL**, seleccione las máquinas virtuales que quiere habilitar. De este modo, se coloca una marca de verificación junto a una máquina virtual.
3. Seleccione **Habilitar JIT en VM**.
   1. Esta hoja muestran los puertos predeterminados que recomienda Azure Security Center:
      - 22 - SSH
      - 3389 - RDP
      - 5985 - WinRM 
      - 5986 - WinRM
   2. También puede configurar puertos personalizados. Para ello, seleccione **Agregar**. 
   3. En **Agregar configuración de puerto**, para cada puerto que elija configurar, tanto el predeterminado como el configurado, puede personalizar los valores siguientes:
      - **Tipo de protocolo**: el protocolo que se permite en este puerto cuando se aprueba una solicitud.
      - **Direcciones IP de origen permitidas**: los intervalos de IP que se permiten en este puerto cuando se aprueba una solicitud.
      - **Tiempo de solicitud máximo**: el período de tiempo máximo durante el que puede estar abierto un puerto específico.

4. Seleccione **Guardar**.


> [!NOTE]
>Cuando se habilita JIT VM Access para una máquina virtual, Azure Security Center crea las reglas de "Denegar todo el tráfico entrante" para los puertos seleccionados en los grupos de seguridad de red asociados con él. Si otras reglas se había creados para los puertos seleccionados, las reglas existentes tienen prioridad sobre las nuevas reglas de "Denegar todo el tráfico entrante". Si no hay ninguna regla existente en los puertos seleccionados, a continuación, las nuevas reglas de "Denegar todo el tráfico entrante" tienen una prioridad superior en los grupos de seguridad de red.
>

### <a name="request-jit-access-to-a-vm"></a>Solicitud de acceso Just-In-Time a una máquina virtual

Para solicitar acceso a una máquina virtual:
1.  En **Just in time VM access** (Acceso a máquina virtual Just-In-Time), seleccione **Configurado**.
2.  En **Máquinas virtuales**, seleccione las máquinas virtuales para las que desea habilitar el acceso Just-In-Time.
3.  Seleccione **Solicitar acceso**. 
  ![solicitar acceso a una máquina virtual](./media/security-center-just-in-time/request-access-to-a-vm.png)
4.  En **Solicitar acceso**, configure para cada máquina virtual los puertos que desee que se abran y las direcciones IP de origen en que se abre el puerto y el tiempo durante el que permanecerá abierto. Solo se podrá solicitar acceso a los puertos que estén configurados en la directiva Just-In-Time. Cada puerto tiene un tiempo máximo permitido que se basa en la directiva Just-In-Time.
5.  Seleccione **Open ports** (Abrir puertos).

> [!NOTE]
> Si un usuario que solicita acceso está detrás de un proxy, es posible que la opción de **Mi IP** no funcione. Es posible que necesite definir todo el intervalo de direcciones IP de la organización.

### <a name="editing-a-jit-access-policy"></a>Edición de una directiva de acceso JIT

Para cambiar la directiva Just-In-Time de una máquina virtual, agregue y configure un puerto nuevo que proteja dicha máquina virtual, o bien cambie otro valor relacionado con un puerto ya protegido.

Para editar una directiva Just-In-Time existente de una máquina virtual:
1. En la ficha **Configurado**, en **Máquinas virtuales**, seleccione la máquina virtual a la que desea agregar un puerto. Para ello, haga clic en los tres puntos situados en la fila de dicha máquina. 
2. Seleccione **Editar**.
3. En **Configuración de acceso a máquina virtual del tipo JIT** , puede modificar la configuración existente de un puerto protegido, o bien agregar un puerto personalizado. Para más información, consulte [Configuración de una directiva de acceso Just-In-Time](#jit-config). 
  ![acceso a vm de tipo jit](./media/security-center-just-in-time/edit-policy.png)

## Uso del acceso JIT en una hoja de máquina virtual de Azure<a name="jit-vm"></a>

Para su comodidad, puede conectarse a una máquina virtual con JIT directamente desde dentro la hoja de máquina virtual de Azure.

### <a name="configuring-a-just-in-time-access-policy"></a>Configuración de una directiva de acceso Just-In-Time 
Para facilitar el despliegue del acceso Just-In-Time en las VM, puede establecer una VM para permitir solo el acceso Just-In-Time directamente desde dentro de la VM.

1. En Azure Portal, seleccione **Máquinas virtuales**.
2. Haga clic en la máquina virtual a la que quiere limitar el acceso Just-In-Time.
3. En el menú, haga clic en **Configuración**.
4. En **Acceso Just-In-Time** haga clic en **Habilitar directiva Just-In-Time**. 

Esto habilita el acceso Just-In-Time para la VM con las siguientes opciones:

- Servidores Windows:
    - Puerto RDP 3389
    - Se permite un tiempo de acceso máximo de tres horas
    - Las direcciones IP de origen permitidas se establece en uno
- Servidores Linux:
    - Puerto SSH 22
    - Se permite un tiempo de acceso máximo de tres horas
    - Las direcciones IP de origen permitidas se establece en uno
     
Si una VM ya tiene Just-In-Time habilitado, cuando va a su página de configuración podrá ver que Just-In-Time está habilitado y que puede usar el vínculo para abrir la directiva en Azure Security Center para ver y cambiar la configuración.

![jit config in vm](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="requesting-jit-access-to-a-vm"></a>Solicitud de acceso JIT a una máquina virtual

En Azure Portal, al intentar conectarse a una máquina virtual, Azure comprueba si hay alguna directiva de acceso Just-In-Time configurada en ella.

- Si hay alguna máquina virtual en la que no esté configurado Just-In-Time, se le solicitará que la configure.

  ![solicitud de jit](./media/security-center-just-in-time/jit-prompt.png)

- Si tiene una directiva JIT configurada en la máquina virtual, puede hacer clic en **Solicitar acceso** para poder tener acceso de acuerdo con la directiva JIT establecida para la máquina virtual. Se solicita el acceso con los parámetros predeterminados siguientes:
    - **IP de origen**: 'Any' (*) (no se puede cambiar)
    - **intervalo de tiempo**: 3 horas (no se puede cambiar)
    - **número de puerto** RDP el puerto 3389 para Windows / puerto 22 para Linux (puede cambiar el número de puerto en el **conectar a máquina virtual** cuadro de diálogo.)


  >![acceso a solicitud de jit](./media/security-center-just-in-time/jit-request-access.png)

## <a name="auditing-jit-access-activity"></a>Auditoría de la actividad de acceso JIT

Puede usar la búsqueda de registros para obtener información sobre las actividades de las máquinas virtuales. Para ver los registros:

1. En **Just in time VM access** (Acceso a máquina virtual del tipo Just-In-Time), seleccione la pestaña **Configurado**.
2. En **Máquinas virtuales**, seleccione una máquina virtual cuya información quiera ver. Para ello, haga clic en los tres puntos situados en la fila de esa máquina virtual. Se abrirá un menú.
3. Seleccione **Registro de actividades** en el menú. Se abre la hoja **Registro de actividad**.

   ![Seleccionar el registro de actividades](./media/security-center-just-in-time/select-activity-log.png)

   En **Registro de actividades** se proporciona una vista filtrada de las operaciones anteriores para esa máquina virtual, junto con el tiempo, la fecha y la suscripción.

Para descargar la información del registro, seleccione **Haga clic aquí para descargar todos los elementos como archivo .csv**.

Modifique los filtros y seleccione **Aplicar** para crear una búsqueda y un registro.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Permisos necesarios para configurar y usar JIT

Establezca estos privilegios necesarios para que los usuarios puedan configurar o editar directivas JIT para máquinas virtuales.

Asigne estas *acciones* al rol: 
- En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:
  - Microsoft.Security/locations/jitNetworkAccessPolicies/write
- En el ámbito de una suscripción, un grupo de recursos o una máquina virtual:
  - Microsoft.Compute/virtualMachines/write 

Establezca estos privilegios para que los usuarios puedan solicitar correctamente acceso Just-In-Time a máquinas virtuales: Asigne estas *acciones* al usuario:
- En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:
  - Microsoft.Security/locations/{the_location_of_the_VM}/jitNetworkAccessPolicies/ initiate/action
- En el ámbito de una suscripción, un grupo de recursos o una máquina virtual:
  - Microsoft.Compute/virtualMachines/read



## <a name="use-jit-programmatically"></a>Uso de JIT mediante programación
Just-In-Time se puede configurar y usar tanto a través de las API REST como de PowerShell.

### <a name="using-just-in-time-vm-access-via-rest-apis"></a>Uso del acceso a máquina virtual Just-In-Time mediante las API REST

La característica de acceso a máquina virtual Just-in-Time se puede usar mediante la API de Azure Security Center. Use esta API para obtener más información acerca de las máquinas virtuales configuradas, cómo agregar otras nuevas o solicitar acceso a una máquina virtual, entre otras cosas. Consulte [Jit Network Access Policies](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies) (Directivas de acceso a red JIT), para más información acerca de la API REST Just-in-Time.

### <a name="using-jit-vm-access-via-powershell"></a>Uso del acceso a máquina virtual del tipo Just-In-Time mediante PowerShell 

Para utilizar la solución de acceso a máquina virtual Just-In-Time mediante PowerShell, use los cmdlets de PowerShell de Azure Security Center oficiales; específicamente `Set-AzJitNetworkAccessPolicy`.

En el siguiente ejemplo se establece una directiva de acceso a la máquina virtual del tipo Just-in-Time en una máquina virtual específica, y se establece lo siguiente:
1.  Cierre los puertos 22 y 3389.
2.  Establezca una ventana de tiempo máximo de 3 horas para cada una, para que así puedan abrirse según la solicitud aprobada.
3.  Permite que el usuario que solicita el acceso controle las direcciones IP de origen; asimismo, le permite establecer una sesión correcta tras una solicitud de acceso del tipo Just-In-Time aprobada.

Ejecute lo siguiente en PowerShell, para poder lograr lo que se ha detallado:

1.  Asigne una variable que contenga la directiva de acceso a máquina virtual del tipo Just-in-Time para una máquina virtual:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  Inserte la directiva de acceso a máquina virtual del tipo Just-in-Time de la máquina virtual en una matriz:
    
        $JitPolicyArr=@($JitPolicy)

3.  Configure la directiva de acceso a máquina virtual del tipo Just-in-Time en la máquina virtual seleccionada:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

#### <a name="requesting-access-to-a-vm"></a>Solicitar acceso a una máquina virtual

En el siguiente ejemplo, puede ver una solicitud de acceso a máquina virtual del tipo Just-In-Time para una máquina virtual específica, en la que se solicita que se abra el puerto 22 para una dirección IP específica y durante un período concreto:

Ejecute lo siguiente en PowerShell:
1.  Configure las propiedades de acceso de solicitud de la máquina virtual.

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  Inserte los parámetros de solicitud de acceso a la máquina virtual en una matriz:

        $JitPolicyArr=@($JitPolicyVm1)
3.  Envíe el acceso de solicitud (use el id. de recurso que obtuvo en el paso 1).

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Para obtener más información, consulte la documentación de cmdlet de PowerShell.


## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido la forma en que el acceso a máquina virtual del tipo Just-In-Time en Security Center ayuda a controlar el acceso a máquinas virtuales de Azure.

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

- [Establecimiento de directivas de seguridad](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
- [Administración de recomendaciones de seguridad](security-center-recommendations.md): conozca una serie de recomendaciones que le ayudarán a proteger los recursos de Azure.
- [Supervisión del estado de seguridad](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
- [Administración y respuesta a las alertas de seguridad](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
- [Supervisión de las soluciones de asociados](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
- [Preguntas más frecuentes sobre Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
- [Blog de seguridad de Azure](https://blogs.msdn.microsoft.com/azuresecurity/) :  encuentre entradas de blog sobre el cumplimiento y la seguridad en Azure.

