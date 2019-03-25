---
title: Recolección de datos en Azure Security Center | Microsoft Docs
description: " Aprenda a habilitar la recolección de datos en Azure Security Center. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2018
ms.author: monhaber
ms.openlocfilehash: 2ce3a94b7ea4745ec3727e508cd10bb580a74c98
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401029"
---
# <a name="data-collection-in-azure-security-center"></a>Recolección de datos en Azure Security Center
Security Center recopila datos de sus máquinas virtuales (VM), conjuntos de escalado de máquinas virtuales, contenedores de IaaS y los equipos que no son de Azure (incluido local) para supervisar las amenazas y vulnerabilidades de seguridad. Los datos se recopilan con Microsoft Monitoring Agent, que lee distintas configuraciones relacionadas con la seguridad y distintos registros de eventos de la máquina y copia los datos en el área de trabajo para analizarlos. Estos son algunos ejemplos de dichos datos: tipo y versión del sistema operativo, registros del sistema operativo (registros de eventos de Windows), procesos en ejecución, nombre de la máquina, direcciones IP y usuario conectado. El agente de Microsoft Monitoring Agent también copia los archivos de volcado en el área de trabajo.

La recopilación de datos es necesaria para proporcionar visibilidad sobre actualizaciones que faltan, valores de seguridad del sistema operativo mal configurados, habilitación de la protección de punto de conexión y detecciones de amenazas y mantenimiento. 

Este artículo proporcionan instrucciones sobre cómo instalar Microsoft Monitoring Agent y establecer un área de trabajo de Log Analytics para almacenar los datos recopilados. Ambas operaciones son necesarias para habilitar la recopilación de datos. 

> [!NOTE]
> - Recopilación de datos solo es necesario para los recursos de proceso (máquinas virtuales, conjuntos de escalado de máquinas virtuales, contenedores de IaaS y los equipos que no son de Azure). Puede beneficiarse de Azure Security Center incluso si no aprovisiona a agentes; de todas formas, tendrá una seguridad limitada y no tendrá las funcionalidades enumeradas anteriormente.  
> - Consulte [Plataformas compatibles con Azure Security Center](security-center-os-coverage.md) para ver una lista de plataformas compatibles.
> - La recopilación de datos para el conjunto de escalado de máquinas virtuales no se admite actualmente.
> - Almacenar datos en Log Analytics, independientemente de si usa un área de trabajo nueva o existente, puede incurrir en cargos adicionales por almacenamiento de datos, vea la página de precios para obtener más detalles.

## Habilitar el aprovisionamiento automático de Microsoft Monitoring Agent<a name="auto-provision-mma"></a>

Para recopilar los datos de las máquinas, debe tener instalado Microsoft Monitoring Agent.  La instalación del agente puede ser automática (recomendado) o se puede optar por instalar el agente manualmente.  

>[!NOTE]
> El aprovisionamiento automático está desactivado de manera predeterminada. Para establecer Security Center para que instale el aprovisionamiento automático de forma predeterminada, establézcalo en **Activado**.
>

Cuando el aprovisionamiento automático está activado, Security Center aprovisiona a Microsoft Monitoring agent en todas las máquinas virtuales de Azure y en las que se crean. El aprovisionamiento automático está muy recomendado, pero la instalación manual del agente también está disponible. [Aprenda a instalar la extensión Microsoft Monitoring Agent](#manualagent).



Para habilitar el aprovisionamiento automático de Microsoft Monitoring Agent:
1. En el menú principal de Security Center, seleccione **Directiva de seguridad**.
2. Haga clic en **Editar configuración** en la columna Configuración de la suscripción deseada en la lista.

   ![Seleccionar suscripción][7]

3. En **Directiva de seguridad**, seleccione **Recopilación de datos**.
4. En **Aprovisionamiento automático**, seleccione **Activado** para habilitarlo.
5. Seleccione **Guardar**.

   ![Habilitar el aprovisionamiento automático][1]

>[!NOTE]
> - Para obtener instrucciones sobre cómo aprovisionar una instalación ya existente, consulte [Aprovisionamiento automático en los casos de una instalación de agente ya existente](#preexisting).
> - Para obtener instrucciones sobre aprovisionamiento manual,consulte [Instalación de la extensión de Microsoft Monitoring Agent de forma manual](#manualagent).
> - Para obtener instrucciones sobre cómo desactivar el aprovisionamiento automático, consulte [Desactivación del aprovisionamiento automático](#offprovisioning).
> - Para obtener instrucciones sobre cómo incorporar Security Center mediante PowerShell, consulte [Automatización de la incorporación de Azure Security Center mediante PowerShell](security-center-powershell-onboarding.md).
>

## <a name="workspace-configuration"></a>Configuración del área de trabajo
Los datos recopilados por Security Center se almacenan en áreas de trabajo de Log Analytics.  Puede optar por que los datos se recopilen de las máquinas virtuales de Azure almacenadas en áreas de trabajo creadas por Security Center o en un área de trabajo que haya creado. 

La configuración de área de trabajo se establece por suscripción, y muchas suscripciones pueden usar la misma área de trabajo.

### <a name="using-a-workspace-created-by-security-center"></a>Uso de un área de trabajo creada por Security Center

Security Center puede crear automáticamente un área de trabajo predeterminada para almacenar los datos. 

Para seleccionar un área de trabajo creada por Security Center:

1. En **Configuración del área de trabajo predeterminada**, seleccione Use workspace(s) created by Security center (Usar áreas de trabajo creadas por Security Center).
   ![Selección del plan de tarifa][10] 

1. Haga clic en **Guardar**.<br>
    Security Center crea un nuevo grupo de recursos y un área de trabajo predeterminada en esa geolocalización y la conecta al agente. La convención de nomenclatura del área de trabajo y el grupo de recursos es:<br>
   **Área de trabajo: DefaultWorkspace-[subscription-ID]-[geo]<br> Grupo de recursos: DefaultResourceGroup-[geo]**

   Si una suscripción contiene máquinas virtuales de varias geolocalizaciones, Security Center crea varias áreas de trabajo. Se crean varias áreas de trabajo para mantener las reglas de privacidad de los datos.
1. Security Center habilitará automáticamente una solución de Security Center en el área de trabajo de acuerdo al plan de tarifa establecido para la suscripción. 

> [!NOTE]
> El plan de tarifa de Log Analytics de áreas de trabajo creadas por Security Center no afecta a la facturación de Security Center. La facturación de Security Center siempre se basa en la directiva de seguridad de Security Center y en las soluciones instaladas en un área de trabajo. Para el nivel Gratis, Security Center habilita la solución *SecurityCenterFree* en el área de trabajo predeterminada. Para el nivel Estándar, Security Center habilita la solución *Security* en el área de trabajo predeterminada.
> El almacenamiento de datos en Log Analytics podría suponer cargos adicionales. Consulte la página de precios para más información.

Para más información, vea [Precios de Security Center ](https://azure.microsoft.com/pricing/details/security-center/).

Para obtener más información acerca de las cuentas de análisis de registro existente, consulte [clientes de análisis de registro existentes](security-center-faq.md#existingloganalyticscust).

### <a name="using-an-existing-workspace"></a>Uso de un área de trabajo existente

Si ya tiene un área de trabajo de Log Analytics, es posible que desee utilizar la misma área de trabajo.

Para usar el área de trabajo de Log Analytics existente, tiene que tener permisos de lectura y escritura en el área de trabajo.

> [!NOTE]
> Las soluciones habilitadas en el área de trabajo existente se aplicarán a las máquinas virtuales de Azure que están conectadas a él. Para las soluciones de pago, esto puede conllevar cargos adicionales. Por motivos de privacidad de datos, asegúrese de que el área de trabajo seleccionado está en la región geográfica adecuada.
> Almacenar datos en log analytics puede incurrir en cargos adicionales por almacenamiento de datos, vea la página de precios para obtener más detalles.

Para seleccionar un área de trabajo existente de Log Analytics:

1. En **Configuración del área de trabajo predeterminada**, seleccione **Use another workspace** (Usar otra área de trabajo).

   ![Seleccionar el área de trabajo existente][2]

2. En el menú desplegable, seleccione un área de trabajo para almacenar los datos recopilados.

   > [!NOTE]
   > Todas las áreas de trabajo de todas sus suscripciones están disponibles en el menú desplegable. Vea [Selección del área de trabajo entre suscripciones](security-center-enable-data-collection.md#cross-subscription-workspace-selection) para obtener más información. Tiene que tener permiso para acceder al área de trabajo.
   >
   >

3. Seleccione **Guardar**.
4. Después de hacer clic en **Guardar**, se le preguntará si quiere volver a configurar las máquinas virtuales supervisadas que estaban anteriormente conectadas a un área de trabajo predeterminada.

   - Haga clic en **No** si quiere que la nueva configuración del área de trabajo solo se aplique a las máquinas virtuales nuevas. La nueva configuración del área de trabajo solo se aplica a las nuevas instalaciones de agente, aquellas máquinas virtuales recién detectadas que no tengan instalado Microsoft Monitoring Agent.
   - Haga clic en **Sí** si quiere que la nueva configuración del área de trabajo se aplique a todas las máquinas virtuales. Además, todas las máquinas virtuales conectadas a un área de trabajo creada por Security Center se volverán a conectar a la nueva área de trabajo de destino.

   > [!NOTE]
   > Si hace clic en Sí, no debe eliminar las áreas de trabajo creadas por Security Center hasta que todas las máquinas virtuales se hayan reconectado al nuevo área de trabajo de destino. Esta operación no se lleva a cabo si se elimina un área de trabajo demasiado pronto.
   >
   >

   - Haga clic en **Cancelar** para cancelar la operación.

     ![Seleccionar el área de trabajo existente][3]

5. Seleccione el plan de tarifa del área de trabajo deseada en la que se va a configurar Microsoft Monitoring Agent. <br>Para usar un área de trabajo existente, establezca el plan de tarifa del área de trabajo. Esto instalará una solución de Security Center en el área de trabajo si todavía no está presente.

    a.  En el menú principal de Security Center, seleccione **Directiva de seguridad**.
     
    b.  Para seleccionar el área de trabajo deseada en la que quiere conectar el agente, haga clic en **Editar configuración** en la columna de Configuración de la suscripción deseada en la lista.
        ![Selección del área de trabajo][8] c. Establezca el plan de tarifa.
        ![Selección del plan de tarifa][9] 
   
   >[!NOTE]
   >Si el área de trabajo ya tiene una solución **Security** o **SecurityCenterFree** habilitada, los precios se establecerán automáticamente. 

## <a name="cross-subscription-workspace-selection"></a>Selección del área de trabajo entre suscripciones
Cuando se selecciona un área de trabajo para almacenar los datos, todas las áreas de trabajo de todas sus suscripciones están disponibles. La selección del área de trabajo entre suscripciones le permite recopilar datos de máquinas virtuales que se ejecutan en distintas suscripciones y almacenarlos en el área de trabajo que prefiera. Esta selección es útil si está usando un área de trabajo centralizada en la organización y desea usarla para la recopilación de datos de seguridad. Para más información acerca de cómo administrar las áreas de trabajo, consulte [Administración del acceso a las áreas de trabajo](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access).


## <a name="data-collection-tier"></a>Nivel de recopilación de datos
La selección de un nivel de recopilación de datos en Azure Security Center solo afecta al almacenamiento de eventos de seguridad en el área de trabajo de Log Analytics. El agente de Log Analytics aún se recopile y analice los eventos de seguridad necesarios para las detecciones de amenazas de Azure Security Center, independientemente de qué nivel de eventos de seguridad que elija almacenar en el área de trabajo de Log Analytics (si existe). Si elige almacenar los eventos de seguridad en el área de trabajo, permitirá la investigación, búsqueda y auditoría de esos eventos en el área de trabajo. 
> [!NOTE]
> Almacenar datos en log analytics puede incurrir en cargos adicionales por almacenamiento de datos, vea la página de precios para obtener más detalles.
> 
> Puede elegir la directiva de filtrado adecuada para sus suscripciones y áreas de trabajo entre cuatro conjuntos de eventos para almacenar en el área de trabajo. 

- **Ninguno**: deshabilita el almacenamiento de eventos de seguridad. Esta es la configuración predeterminada.
- **Mínimo**: Es un conjunto más pequeño de eventos para los clientes que quieren reducir el volumen de eventos.
- **Común**: Es un conjunto de eventos que satisfacen a la mayoría de los clientes y les permite efectuar un registro de auditoría completo.
- **Todos los eventos**: para clientes que quieren asegurarse de que se almacenan todos los eventos.


> [!NOTE]
> Estos conjuntos de eventos de seguridad solo están disponibles en nivel Estándar de Security Center. Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
Estos conjuntos se han diseñado para abordar escenarios típicos. Asegúrese de evaluar cuál se ajusta a sus necesidades antes de implementarlo.
>
>

Para determinar los eventos que formarán parte de los conjuntos de eventos **Común** y **Mínimo**, hemos colaborado con los clientes y los estándares del sector para obtener información sobre la frecuencia sin filtrar de cada evento y su uso. En este proceso se han empleado las siguientes directrices:

- **Mínimo**: Asegúrese de que este conjunto abarque solamente los eventos que podrían indicar una brecha correcta y eventos importantes que tengan un volumen muy bajo. Por ejemplo, este conjunto contiene un inicio de sesión de usuario correcto y uno erróneo (identificadores de evento 4624 y 4625), pero no contiene el cierre de sesión, que es importante para la auditoría pero no lo es para la detección y tiene un volumen relativamente alto. La mayor parte del volumen de datos de este conjunto son los eventos de inicio de sesión y el evento de creación de proceso (Id. de evento 4688).
- **Común**: Proporcione una pista de auditoría de usuario completa en este conjunto. Por ejemplo, este conjunto contiene los inicios de sesión y los cierres de sesión de usuario (Id. de evento 4634). Se incluyen acciones de auditoría como cambios en los grupos de seguridad, operaciones de Kerberos en los controladores de dominio de clave y otros eventos que recomiendan las organizaciones del sector.

Los eventos que tienen un volumen muy bajo se han incluido en el conjunto Común, ya que la motivación principal para elegirlo respecto de todos los eventos pasa por reducir el volumen, y no por filtrar eventos específicos.

A continuación se muestra un desglose completo de los identificadores de evento de seguridad y de AppLocker para cada conjunto:

| Capa de datos | Indicadores de eventos recopilados |
| --- | --- |
| Mínimo | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Común | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Si se usa un objeto de directiva de grupo (GPO), se recomienda habilitar el evento 4688 de creación de procesos de las directivas de auditoría y el campo *CommandLine* del evento 4688. Para más información acerca del evento 4688 de creación de procesos, consulte las [preguntas más frecuentes](security-center-faq.md#what-happens-when-data-collection-is-enabled) acerca de Security Center. Para más información acerca de estas directivas de auditoría, consulte [Recomendaciones de la directiva de auditoría](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  Para habilitar la recopilación de datos para [Controles de aplicación adaptables](security-center-adaptive-application.md), Security Center configura una directiva de AppLocker local en el modo de auditoría que permite todas las aplicaciones. Con esto, AppLocker generará eventos que luego Security Center recopilará y aprovechará. Es importante tener en cuenta que esta directiva no se configurará en las máquinas en las que ya se haya configurado una directiva de AppLocker. 
> - Para recopilar Windows Filtering Platform [Event ID 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156), tiene que habilitar [Auditar conexión de Plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) (Auditpol /set /subcategory:"Filtering Platform Connection" /Success:Enable)
>

Para elegir la directiva de filtrado:
1. En la hoja **Directiva de seguridad: Recopilación de datos** (Recopilación de datos de directivas de seguridad), seleccione la directiva de filtrado en **Eventos de seguridad**.
2. Seleccione **Guardar**.

   ![Elegir la directiva de filtrado][5]

### Aprovisionamiento automático en los casos de una instalación de agente ya existente <a name="preexisting"></a> 

Los siguientes casos de uso especifican cómo funciona el aprovisionamiento automático en los casos en los que ya hay un agente o una extensión instalados. 

- Microsoft Monitoring Agent está instalado en el equipo, pero no como una extensión (agente directo)<br>
Si Microsoft Monitoring Agent se instala directamente en la máquina virtual (no como una extensión de Azure), Security Center se instalará la extensión Microsoft Monitoring Agent y puede actualizar a Microsoft Monitoring agent a la versión más reciente.
Instalado el agente continuará informando en sus áreas de trabajo ya configurado y además informará al área de trabajo configurada en el centro de seguridad (multi-homing se admite).
Si el área de trabajo configurada es un área de trabajo de usuario (no del centro de seguridad predeterminada del área de trabajo), deberá instalar el "seguridad / solución"securityFree"en el mismo centro de seguridad iniciar el procesamiento de eventos desde máquinas virtuales y equipos de informar a esa área de trabajo.<br>
<br>
Para las máquinas existentes en suscripciones incorporado a Security Center antes de 2019-03-17, cuando se detectará un agente existente, no se instalará la extensión Microsoft Monitoring Agent y la máquina no se verán afectada. Para estos equipos, vea la recomendación "Resolver problemas de agent health en los equipos de supervisión" para resolver los problemas de instalación de agente en estas máquinas.

  
- Agente de System Center Operations Manager está instalado en el equipo<br>
Centro de seguridad instalará la extensión de Microsoft Monitoring Agent en paralelo a Operations Manager existente. El agente de Operations Manager existente continuará informando normalmente en el servidor de Operations Manager. Tenga en cuenta que el agente de Operations Manager y Microsoft Monitoring Agent compartan bibliotecas en tiempo de ejecución común, que se actualizará a la versión más reciente durante este proceso.
Nota: si está instalada la versión 2012 de agente de Operations Manager, **no** activar aprovisionamiento en automático.<br>

- Está presente una extensión de máquina virtual existente<br>
    - Cuando se instala el agente de supervisión como una extensión, la configuración de extensión permite informar a una sola área de trabajo. Security Center no invalida las conexiones existentes con áreas de trabajo de usuario. Centro de seguridad almacenará datos de seguridad de la máquina virtual en el área de trabajo ya está conectado, siempre que se instaló el "seguridad" o "securityFree" solución en él. Security Center puede actualizar la versión de extensión a la versión más reciente de este proceso.  
    - Para ver el área de trabajo, a la que la extensión existente está enviando datos, ejecute la prueba de [Validación de la conectividad con Azure Security Center](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/). Como alternativa, puede abrir las áreas de trabajo de Log Analytics, seleccione un área de trabajo, seleccione la máquina virtual y examine la conexión del agente de Log Analytics. 
    - Si tiene un entorno donde está instalado el agente de Log Analytics en estaciones de trabajo cliente y se informan a un área de trabajo de Log Analytics existente, revise la lista de [sistemas operativos compatibles con Azure Security Center](security-center-os-coverage.md) para asegurarse de que es compatible con el sistema operativo y vea [clientes de análisis de registro existentes](security-center-faq.md#existingloganalyticscust) para obtener más información.
 
### Desactivación del aprovisionamiento automático <a name="offprovisioning"></a>
Puede desactivar en cualquier momento el aprovisionamiento automático de los recursos desactivando esta opción en la directiva de seguridad. 


1. Vuelva al menú principal de Security Center y seleccione la directiva de seguridad.
2. Haga clic en **editar la configuración de** en la fila de la suscripción para el que desea deshabilitar el aprovisionamiento automático.
3. En la hoja **Directiva de seguridad: recopilación de datos**, en **Aprovisionamiento automático**, seleccione **Desactivado**.
4. Seleccione **Guardar**.

   ![Deshabilitación del aprovisionamiento automático][6]

Cuando el aprovisionamiento automático está deshabilitado (desactivado), no se muestra la sección de configuración del área de trabajo predeterminada.

Si se desactiva el aprovisionamiento automático después de que estuviera anteriormente activado:
-   No se aprovisionarán los agentes en nuevas máquinas virtuales.
-   Security Center deja de recopilar datos del área de trabajo predeterminada.
 
> [!NOTE]
>  La deshabilitación del aprovisionamiento automático no quita Microsoft Monitoring Agent de las máquinas virtuales de Azure en las que se aprovisionó el agente. Para más información acerca de cómo quitar la extensión de OMS, consulte [¿Cómo quito extensiones OMS instaladas por Security Center?](security-center-faq.md#remove-oms).
>
    
## Aprovisionamiento manual del agente <a name="manualagent"></a>
 
Hay varias formas de instalar Microsoft Monitoring Agent manualmente. Cuando vaya a realizar la instalación manual, asegúrese de que deshabilita el aprovisionamiento automático.

### <a name="operations-management-suite-vm-extension-deployment"></a>Implementación de la extensión de máquina virtual de Operations Management Suite 

Puede instalar Microsoft Monitoring Agent manualmente para que Security Center pueda recopilar los datos de seguridad de las máquinas virtuales y proporcionar recomendaciones y alertas.
1. Seleccione aprovisionamiento automático: Desactivado.
2. Cree un área de trabajo y seleccione el plan de tarifa del área de trabajo en la que se va a configurar Microsoft Monitoring Agent:

   a.  En el menú principal de Security Center, seleccione **Directiva de seguridad**.
     
   b.  Seleccione el área de trabajo deseada en la que quiere conectar al agente. Asegúrese de que el área de trabajo está en la misma suscripción que se usa en Security Center y que tiene permisos de lectura/escritura en el área de trabajo.
       ![Selección del área de trabajo][8]
3. Establezca el plan de tarifa.
   ![Selección del plan de tarifa][9] 
   >[!NOTE]
   >Si el área de trabajo ya tiene una solución **Security** o **SecurityCenterFree** habilitada, los precios se establecerán automáticamente. 
   > 

4. Si desea implementar los agentes en nuevas máquinas virtuales mediante una plantilla de Resource Manager, instale la extensión de máquina virtual de OMS:

   a.  [Instalación de la extensión de máquina virtual de OMS para Windows](../virtual-machines/extensions/oms-windows.md)
    
   b.  [Instalación de la extensión de máquina virtual de OMS para Linux](../virtual-machines/extensions/oms-linux.md)
5. Para implementar las extensiones en máquinas virtuales existentes, siga las instrucciones de [Recopilación de datos acerca de máquinas virtuales de Azure](../azure-monitor/learn/quick-collect-azurevm.md).

   > [!NOTE]
   > La sección **Recopilación de datos de eventos y rendimiento** es opcional.
   >
6. Para usar PowerShell con el fin de implementar la extensión, use el siguiente ejemplo de PowerShell:
   
   [!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
   
   1. Vaya a **Log Analytics** y haga clic en **Configuración avanzada**.
    
      ![Configuración de Log Analytics][11]

   2. Copie los valores de **WorkspaceID** y **Clave principal**.
  
      ![Copia de valores][12]

   3. Rellene la configuración pública y la configuración privada con estos valores:
     
           $PublicConf = '{
               "workspaceId": "WorkspaceID value",
           }' 
 
           $PrivateConf = '{
               "workspaceKey": "<Primary key value>”
           }' 

      - Cuando se instala en una máquina virtual Windows:
        
            Set-AzVMExtension -ResourceGroupName $vm.ResourceGroupName -VMName $vm.Name -Name "MicrosoftMonitoringAgent" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "MicrosoftMonitoringAgent" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True 
    
      - Cuando se instala en una máquina virtual Linux:
        
            Set-AzVMExtension -ResourceGroupName $vm1.ResourceGroupName -VMName $vm1.Name -Name "OmsAgentForLinux" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "OmsAgentForLinux" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True`

> [!NOTE]
> Para obtener instrucciones sobre cómo incorporar Security Center mediante PowerShell, consulte [Automatización de la incorporación de Azure Security Center mediante PowerShell](security-center-powershell-onboarding.md).

## <a name="troubleshooting"></a>Solución de problemas

-   Para identificar problemas de instalación de aprovisionamiento automático, consulte [Supervisión de problemas de Agent Health](security-center-troubleshooting-guide.md#mon-agent).

-  Para identificar los requisitos de red de agente de supervisión, consulte [Solución de problemas de los requisitos de red del agente de supervisión](security-center-troubleshooting-guide.md#mon-network-req).
-   Para identificar problemas de incorporación manual, consulte [Cómo solucionar problemas de incorporación de Operations Management Suite](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).

- Para identificar problemas de equipos y máquinas virtuales que no estén supervisados, consulte [Equipos y máquinas virtuales no supervisados](security-center-virtual-machine-protection.md#unmonitored-vms-and-computers).

## <a name="next-steps"></a>Pasos siguientes
En este artículo le hemos mostrado cómo efectuar un aprovisionamiento automático y una recopilación de datos en Security Center. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): aprenda a supervisar el estado de los recursos de Azure.



<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
[8]: ./media/security-center-enable-data-collection/manual-provision.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[10]: ./media/security-center-enable-data-collection/workspace-selection.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
