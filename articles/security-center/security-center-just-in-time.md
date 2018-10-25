---
title: Acceso a máquina virtual Just-In-Time en Azure Security Center | Microsoft Docs
description: En este documento se explica cómo el acceso a VM Just-In-Time en Azure Security Center ayuda a controlar el acceso a máquinas virtuales de Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 98533e3c1454867ff09c53902f0f575d198452a3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320346"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Administrar el acceso a máquina virtual mediante Just-In-Time

El acceso a máquina virtual Just-In-Time se puede usar para bloquear el tráfico entrante a las máquinas virtuales de Azure. Para ello, se reduce la exposición a ataques al mismo tiempo que se proporciona un acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario.

> [!NOTE]
> La característica Just-In-Time está disponible en el nivel estándar de Security Center.  Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
>
>

## <a name="attack-scenario"></a>Escenario de ataque

Normalmente, los ataques por fuerza bruta tienen como destino los puertos de administración para obtener acceso a una máquina virtual. Si un atacante tiene éxito, puede tomar el control de la máquina virtual y establecer un punto de apoyo en su entorno.

Una manera de reducir el riesgo de sufrir un ataque por fuerza bruta consiste en limitar el tiempo que está abierto un puerto. No es necesario que los puertos de administración estén abiertos en todo momento. Solo deben estar abiertos mientras se está conectado a la máquina virtual, por ejemplo, para realizar tareas de administración o mantenimiento. Cuando se habilita Just-In-Time, Security Center usa las reglas del [grupo de seguridad de red](../virtual-network/security-overview.md#security-rules) (NSG), que restringen el acceso a los puertos de administración para que no puedan ser objeto de ataques.

![Escenario Just-In-Time][1]

## <a name="how-does-just-in-time-access-work"></a>¿Cómo funciona el acceso Just-In-Time?

Cuando se habilita Just-In-Time, Security Center bloquea el tráfico entrante a las máquinas virtuales de Azure mediante la creación de una regla de NSG. Se deben seleccionar los puertos de la máquina virtual para la que se bloqueará el tráfico entrante. Estos puertos están controlados mediante la solución Just-In-Time.

Cuando un usuario solicita acceso a una máquina virtual, Security Center comprueba que el usuario tenga permisos de [control de acceso basado en rol (RBAC)](../role-based-access-control/role-assignments-portal.md) que conceden acceso de escritura para la máquina virtual. Si tiene permisos de escritura, se aprueba la solicitud y Security Center configura automáticamente los grupos de seguridad de red (NSG) para permitir el tráfico entrante a los puertos seleccionados durante el tiempo especificado. Una vez transcurrido ese tiempo, Security Center restaura los NSG a su estado anterior. Pero las conexiones que ya están establecidas no se interrumpen.

> [!NOTE]
> En la actualidad, el acceso a máquina virtual Just-In-Time de Security Center solo admite las máquinas virtuales que se han implementado mediante Azure Resource Manager. Para obtener más información sobre los modelos de implementación clásico y de Resource Manager, vea [La implementación de Azure Resource Manager frente a la implementación clásica](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Uso del acceso Just-In-Time

1. Abra el panel **Security Center**.

2. En el panel izquierdo, seleccione **Acceso a VM Just-In-Time**.

![Icono Acceso a VM Just-In-Time][2]

La ventana **Acceso a VM Just-In-Time** se abre.

![Icono Acceso a VM Just-In-Time][10]

La hoja **Acceso a VM Just-In-Time** proporciona información sobre el estado de las máquinas virtuales:

- **Configurado**: máquinas virtuales que se han configurado para admitir el acceso a máquina virtual Just-In-Time. Los datos que se muestran son de la última semana e incluyen, para cada máquina virtual, el número de solicitudes aprobadas, la fecha y hora del último acceso y el último usuario.
- **Recomendado**: máquinas virtuales que pueden admitir el acceso a máquina virtual Just-In-Time pero que no se han configurado para ello. Se recomienda habilitar el control de acceso a máquina virtual Just-In-Time para estas máquinas virtuales. Consulte [Configurar una directiva de acceso Just-In-Time](#configuring-a-just-in-time-access-policy).
- **Ninguna recomendación**: una máquina virtual podría no recomendarse por diversas razones:
  - Falta el NSG: la solución Just-In-Time requiere que exista un NSG.
  - Máquina virtual clásica: en la actualidad, el acceso a máquina virtual Just-In-Time de Security Center solo admite las máquinas virtuales que se han implementado mediante Azure Resource Manager. La solución Just-In-Time no admite las implementaciones clásicas.
  - Otros: se incluirá una máquina virtual en esta categoría si la solución Just-In-Time está desactivada en la directiva de seguridad de la suscripción o del grupo de recursos, o si la máquina virtual no tiene una IP pública y no existe un NSG.

## <a name="configuring-a-just-in-time-access-policy"></a>Configurar una directiva de acceso Just-In-Time

Para seleccionar las máquinas virtuales que quiere habilitar:

1. En **Acceso a VM Just-In-Time**, seleccione la pestaña **Recomendado**.

  ![Habilitar el acceso Just-In-Time][3]

2. En **MÁQUINA VIRTUAL**, seleccione las máquinas virtuales que quiere habilitar. De este modo, se coloca una marca de verificación junto a una máquina virtual.
3. Seleccione **Habilitar JIT en VM**.
4. Seleccione **Guardar**.

### <a name="default-ports"></a>Puertos predeterminados

Puede ver los puertos predeterminados que Security Center recomienda al habilitar Just-In-Time.

1. En **Acceso a VM Just-In-Time**, seleccione la pestaña **Recomendado**.

  ![Mostrar los puertos predeterminados][6]

2. En **Máquinas virtuales**, seleccione una máquina virtual. De este modo, se coloca una marca de verificación junto a la máquina virtual y se abre la hoja **JIT VM access configuration** (Configuración de acceso a máquina virtual Just-In-Time). En esta hoja se muestran los puertos predeterminados.

### <a name="add-ports"></a>Agregar puertos

En **JIT VM access configuration** (Configuración de acceso a máquina virtual Just-In-Time), también puede agregar y configurar un puerto nuevo en el que habilitar la solución Just-In-Time.

1. En **JIT VM access configuration** (Configuración de acceso a máquina virtual Just-In-Time), seleccione **Agregar**. Se abre la hoja **Add port configuration** (Agregar configuración de puerto).

  ![Configuración de puerto][7]

2. En **Add port configuration** (Agregar configuración de puerto), debe identificar el puerto, el tipo de protocolo, las direcciones IP de origen permitidas y el tiempo de solicitud máximo.

  Las direcciones IP de origen permitidas son los intervalos IP que pueden obtener acceso una vez que se ha aprobado una solicitud.

  El tiempo de solicitud máximo es el período de tiempo máximo durante el que puede estar abierto un puerto específico.

3. Seleccione **Aceptar**.

> [!NOTE]
>Cuando el acceso a la máquina virtual JIT se habilita en una máquina virtual, Azure Security Center crea la opción para denegar todas las reglas de tráfico de entrada para los puertos seleccionados en los grupos de seguridad de red asociados a ella. Las reglas serán la prioridad principal de sus grupos de seguridad de red, o tendrán una prioridad más baja que las reglas existentes. Esto depende del análisis que realice Azure Security Center, que determinará si una regla es segura o no.
>


## <a name="set-just-in-time-within-a-vm"></a>Establecer Just-In-Time en una VM

Para facilitar el despliegue del acceso Just-In-Time en las VM, puede establecer una VM para permitir solo el acceso Just-In-Time directamente desde dentro de la VM.

1. En Azure Portal, seleccione **Máquinas virtuales**.
2. Haga clic en la máquina virtual a la que quiere limitar el acceso Just-In-Time.
3. En el menú, haga clic en **Configuración**.
4. En **Acceso Just-In-Time** haga clic en **Habilitar directiva Just-In-Time**. 

Esto habilita el acceso Just-In-Time para la VM con las siguientes opciones:

- Servidores Windows:
    - Puerto RDP 3389
    - 3 horas de acceso
    - Las direcciones IP de origen permitidas se establecen en Por solicitud
- Servidores Linux:
    - Puerto SSH 22
    - 3 horas de acceso
    - Las direcciones IP de origen permitidas se establecen en Por solicitud
     
Si una VM ya tiene Just-In-Time habilitado, cuando va a su página de configuración podrá ver que Just-In-Time está habilitado y que puede usar el vínculo para abrir la directiva en Azure Security Center para ver y cambiar la configuración.

![jit config in vm](./media/security-center-just-in-time/jit-vm-config.png)


## <a name="requesting-access-to-a-vm"></a>Solicitar acceso a una máquina virtual

Para solicitar acceso a una máquina virtual:

1. En **Acceso a VM Just-In-Time**, seleccione la pestaña **Configurado**.
2. En **Máquinas virtuales**, seleccione las máquinas virtuales para las que quiere habilitar el acceso. De este modo, se coloca una marca de verificación junto a una máquina virtual.
3. Seleccione **Solicitar acceso**. Se abre la hoja **Solicitar acceso**.

  ![Solicitar acceso a una máquina virtual][4]

4. En la hoja **Solicitar acceso**, configure para cada máquina virtual los puertos que deben abrirse, junto con la dirección IP de origen en la que se abre el puerto y el tiempo durante el que permanece abierto. Solo puede solicitar acceso a los puertos que están configurados en la directiva Just-In-Time. Cada puerto tiene un tiempo máximo permitido que se basa en la directiva Just-In-Time.
5. Seleccione **Open ports** (Abrir puertos).

> [!NOTE]
> Cuando un usuario solicita acceso a una máquina virtual, Security Center comprueba que el usuario tenga permisos de [control de acceso basado en rol (RBAC)](../role-based-access-control/role-assignments-portal.md) que conceden acceso de escritura para la máquina virtual. Si tienen permisos de escritura, se aprueba la solicitud.
>
>

> [!NOTE]
> Si un usuario que solicita acceso está detrás de un proxy, la opción de "Mi IP" podría no funcionar. Puede haber una necesidad de definir el intervalo completo de la organización.
>
>

## <a name="editing-a-just-in-time-access-policy"></a>Editar una directiva de acceso Just-In-Time

Puede cambiar la directiva Just-In-Time de una máquina virtual. Para ello, agregue y configure un puerto nuevo que se abra para esa máquina virtual, o bien cambie otro parámetro relacionado con un puerto ya protegido.

Para editar una directiva Just-In-Time de una máquina virtual, use la pestaña **Configurado**:

1. En **Máquinas virtuales**, seleccione una máquina virtual para agregarle un puerto. Para ello, haga clic en los tres puntos situados en la fila de esa máquina virtual. Se abrirá un menú.
2. Seleccione **Editar** en el menú. Se abre la hoja **JIT VM access configuration** (Configuración de acceso a máquina virtual Just-In-Time).

  ![Editar directiva][8]

3. En **JIT VM access configuration** (Configuración de acceso a máquina virtual Just-In-Time), puede hacer clic en un puerto ya protegido para modificar la configuración existente, o bien puede seleccionar **Agregar**. Se abre la hoja **Add port configuration** (Agregar configuración de puerto).

  ![Agregar un puerto][7]

4. En **Add port configuration** (Agregar configuración de puerto), identifique el puerto, el tipo de protocolo, las direcciones IP de origen permitidas y el tiempo de solicitud máximo.
5. Seleccione **Aceptar**.
6. Seleccione **Guardar**.

## <a name="auditing-just-in-time-access-activity"></a>Auditar la actividad del acceso Just-In-Time

Puede usar la búsqueda de registros para obtener información sobre las actividades de las máquinas virtuales. Para ver los registros:

1. En **Acceso a VM Just-In-Time**, seleccione la pestaña **Configurado**.
2. En **Máquinas virtuales**, seleccione una máquina virtual cuya información quiera ver. Para ello, haga clic en los tres puntos situados en la fila de esa máquina virtual. Se abrirá un menú.
3. Seleccione **Registro de actividades** en el menú. Se abre la hoja **Registro de actividad**.

  ![Seleccionar el registro de actividades][9]

  En **Registro de actividades** se proporciona una vista filtrada de las operaciones anteriores para esa máquina virtual, junto con el tiempo, la fecha y la suscripción.

Para descargar la información del registro, seleccione **Haga clic aquí para descargar todos los elementos como archivo .csv**.

Modifique los filtros y seleccione **Aplicar** para crear una búsqueda y un registro.

## <a name="using-just-in-time-vm-access-via-rest-apis"></a>Uso del acceso a máquina virtual Just-In-Time mediante las API REST

La característica de acceso a máquina virtual Just-in-Time se puede usar mediante la API de Azure Security Center. Use esta API para obtener más información acerca de las máquinas virtuales configuradas, cómo agregar otras nuevas o solicitar acceso a una máquina virtual, entre otras cosas. Consulte [Jit Network Access Policies](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies) (Directivas de acceso a red JIT), para más información sobre la API REST Just-in-Time.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Usar el acceso a máquina virtual Just-In-Time mediante PowerShell 

Para usar la solución de acceso de la máquina virtual Just-In-Time con de PowerShell, use los cmdlets de PowerShell de Azure Security Center oficiales; específicamente `Set-AzureRmJitNetworkAccessPolicy`.

En el siguiente ejemplo se establece una directiva de acceso a la máquina virtual de tipo Just-in-Time en una máquina virtual específica, y se establece lo siguiente:
1.  Cierre los puertos 22 y 3389.
2.  Establezca una ventana de tiempo máximo de 3 horas para cada una, para que así puedan abrirse según la solicitud aprobada.
3.  Permite que el usuario que solicita el acceso controle las direcciones IP de origen; asimismo, le permite establecer una sesión correcta tras una solicitud de acceso Just-In-Time aprobada.

Ejecute lo siguiente en PowerShell, para poder lograr lo que se ha detallado:

1.  Asigne una variable que contenga la directiva de acceso a la máquina virtual Just-in-Time para una máquina virtual:

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

2.  Inserte la directiva de acceso de la máquina virtual Just-in-Time en una matriz:
    
        $JitPolicyArr=@($JitPolicy)

3.  Configure la directiva de acceso a la máquina virtual Just-in-Time en la máquina virtual seleccionada:
    
        Set-AzureRmJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="requesting-access-to-a-vm"></a>Solicitar acceso a una máquina virtual

En el siguiente ejemplo, puede ver una solicitud de acceso a la máquina virtual Just-In-Time para una máquina virtual específica, en la cual se solicita que se abra el puerto 22 para una dirección IP específica y por un período de tiempo específico:

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

        Start-AzureRmJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Para obtener más información, consulte la documentación de cmdlet de PowerShell.


## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido cómo el acceso a máquina virtual Just-In-Time en Security Center ayuda a controlar el acceso a máquinas virtuales de Azure.

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

- [Establecimiento de directivas de seguridad](security-center-policies.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
- [Administración de recomendaciones de seguridad](security-center-recommendations.md): conozca una serie de recomendaciones que le ayudarán a proteger los recursos de Azure.
- [Supervisión del estado de seguridad](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
- [Administración y respuesta a las alertas de seguridad](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
- [Supervisión de las soluciones de asociados](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
- [Preguntas más frecuentes sobre Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
- [Blog de seguridad de Azure](https://blogs.msdn.microsoft.com/azuresecurity/) :  encuentre entradas de blog sobre el cumplimiento y la seguridad en Azure.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[10]: ./media/security-center-just-in-time/just-in-time-access.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
