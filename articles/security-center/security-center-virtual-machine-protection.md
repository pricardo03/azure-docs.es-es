---
title: Protección de las aplicaciones y las máquinas en Azure Security Center | Microsoft Docs
description: En este documento se describen las recomendaciones de Security Center que le ayudan a proteger las máquinas virtuales, equipos, aplicaciones web y los entornos de App Service.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: v-mohabe
ms.openlocfilehash: c8f381a3440d742cca880f44b73cbc22bde92ecc
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910555"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Protección de las aplicaciones y las máquinas en Azure Security Center
Azure Security Center analiza el estado de seguridad de los recursos de Azure, los servidores que no son de Azure y las máquinas virtuales. Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios. Las recomendaciones se aplican a los tipos de recursos de Azure: máquinas virtuales, equipos, aplicaciones, redes, SQL e identidad y acceso.

Este artículo trata sobre las recomendaciones aplicables a máquinas y aplicaciones.

## <a name="monitoring-security-health"></a>Supervisión del estado de seguridad
Puede supervisar el estado de seguridad de los recursos en el panel **Security Center – Información general**. La sección **Recursos** indica el número de problemas identificados y el estado de seguridad para cada tipo de recurso.

Para ver una lista de todos los problemas, puede seleccionar **Recomendaciones**. Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).

Para ver una lista completa de las recomendaciones sobre procesos y recomendaciones, consulte [Recomendaciones](security-center-virtual-machine-protection.md#compute-and-app-recommendations).

Para continuar, seleccione **Procesos y aplicaciones** en **Recursos** o en el menú principal de Security Center.
![Panel de Security Center](./media/security-center-virtual-machine-recommendations/overview.png)

## <a name="monitor-compute-and-app-services"></a>Supervisión de los servicios de procesos y aplicaciones
En **Compute & apps** (Procesos y aplicaciones), existen las siguientes pestañas:

- **Información general**: supervisión y recomendaciones identificadas por Security Center.
- **VM y equipos**: muestra el estado de seguridad actual de todas las máquinas virtuales y equipos.
- **Cloud Services**: lista de todos los roles web y de trabajo que supervisa Security Center.
- **App Services**: muestra la lista actual de los entornos de App Service y el estado de seguridad actual de cada uno.
- **Contenedores (versión preliminar)** : lista de los contenedores hospedados en equipos IaaS Linux y evaluaciones de seguridad de sus configuraciones de Docker.
- **Recursos de proceso (versión preliminar)** : enumera las recomendaciones para los recursos de proceso, como clústeres de Service Fabric y centros de eventos.

Para continuar, seleccione **Procesos y aplicaciones** en **Protección de seguridad de recursos**.

![Proceso](./media/security-center-virtual-machine-recommendations/compute.png)

Todas las pestañas pueden tener varias secciones y en cada una de ella se puede seleccionar cualquiera de las opciones para ver más detalles acerca de los pasos recomendados para abordar un problema concreto.

### Equipos y máquinas virtuales no supervisados <a name="unmonitored-vms-and-computers"></a>
Una máquina virtual o un equipo no está supervisado por Security Center si no está ejecutando la extensión de Microsoft Monitoring Agent. Una máquina virtual puede tener ya instalado un agente local, por ejemplo un agente directo de OMS o el agente System Center Operations Manager. Las máquinas virtuales con estos agentes se identifican como sin supervisar porque estos agentes no son totalmente compatibles en Security Center. Para aprovechar al máximo todas las funcionalidades de Security Center, se necesita la extensión de Microsoft Monitoring Agent.

Puede instalar la extensión en la máquina virtual o en el equipo no supervisados además del agente local ya instalado. Configure de la misma forma a ambos agentes, conectándolos a la misma área de trabajo. Esto permite que Security Center interactúe con la extensión de Microsoft Monitoring Agent y recopile datos. Consulte [Habilitar la extensión de VM](../azure-monitor/learn/quick-collect-azurevm.md) para obtener instrucciones acerca de cómo instalar la extensión de Microsoft Monitoring Agent.

Consulte [Supervisión de problemas de Agent Health](security-center-troubleshooting-guide.md#mon-agent) para saber más acerca de los motivos por los que Security Center no puede supervisar correctamente las máquinas virtuales y los equipos inicializados para el aprovisionamiento automático.

### <a name="recommendations"></a>Recomendaciones
Esta sección incluye un conjunto de recomendaciones para cada máquina virtual y equipo, para cada rol web o de trabajo, para cada entorno de Azure App Service Web Apps y Azure App Service que supervisa Security Center. La primera columna muestra la recomendación. La segunda columna muestra el número total de recursos a los que afecta dicha recomendación. La tercera columna muestra la gravedad del problema.

Cada recomendación tiene un conjunto de acciones que se podrán realizar una vez que la selecciona. Por ejemplo, si selecciona **Faltan las actualizaciones del sistema**, aparece el número de máquinas virtuales y equipos a los que les faltan revisiones y la gravedad de la actualización que falta.

**Aplicar actualizaciones del sistema** incluye un resumen de actualizaciones críticas en formato de grafo, uno para Windows y otro para Linux. La segunda parte tiene una tabla con la siguiente información:

- **NOMBRE**: nombre de la actualización que falta.
- **NÚMERO DE DE EQUIPOS Y MÁQUINAS VIRTUALES**: número total de máquinas virtuales y equipos que no tienen esta actualización.
- **ACTUALIZAR GRAVEDAD**: describe la gravedad de una recomendación concreta:

    - **Crítico**: existe una vulnerabilidad en un recurso importante (aplicación, máquina virtual o grupo de seguridad de red) y requiere atención.
    - **Importante**: para completar un proceso o eliminar una vulnerabilidad se requieren pasos adicionales o no críticos.
    - **Moderado**: es preciso abordar una vulnerabilidad, pero esta no requiere una atención inmediata. (De manera predeterminada no se muestran las recomendaciones bajas, pero si desea verlas, puede filtrar por ellas).


- **ESTADO**: el estado actual de la recomendación:

    - **Abierta**: la recomendación aún no se ha abordado.
    - **En curso**: la recomendación se está aplicando actualmente a esos recursos y no se requiere ninguna acción por su parte.
    - **Resuelta**: la recomendación ya terminó. (Si se ha resuelto el problema, la entrada aparecerá atenuada).

Para ver los detalles de recomendación, haga clic en el nombre de la actualización que falta en la lista.


> [!NOTE]
> Las recomendaciones de seguridad son las mismas que aparecen si selecciona el icono **Recomendaciones**. Para más información sobre cómo resolver las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).
>
>

### <a name="vms-and-computers"></a>Máquinas virtuales y equipos
La sección de máquinas virtuales y equipos ofrece una visión general de todas las recomendaciones de máquinas virtuales y equipos. Cada columna representa un conjunto de recomendaciones.

![Recomendaciones de máquina virtual y equipo](./media/security-center-virtual-machine-recommendations/vm-computers.png)

Hay cuatro tipos de iconos representados en esta lista:

![Equipos que no son de Azure](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Equipo que no es de Azure.

![Máquina virtual de Azure Resource Manager](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Máquina virtual de Azure Resource Manager.

![Máquina virtual de Azure clásico](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Máquina virtual de Azure clásico.


![Máquinas virtuales identificadas del área de trabajo](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) Máquinas virtuales que se identifican solo desde el área de trabajo que forma parte de la suscripción visualizada. Aquí se incluyen las máquinas virtuales de otras suscripciones que informan al área de trabajo de esta suscripción y las máquinas virtuales que se instalaron con el agente directo de Operations Manager y no tienen un identificador de recurso.

El icono que aparece en cada recomendación le ayuda a identificar rápidamente las máquinas virtuales y los equipos que necesitan atención y el tipo de recomendación. También puede usar filtros para buscar en la lista por **tipo de recurso** y **gravedad**.

Para explorar en profundidad las recomendaciones de seguridad para cada máquina virtual, haga clic en la máquina virtual.
![Cloud Services](./media/security-center-virtual-machine-recommendations/recommendation-list.png)

### <a name="cloud-services"></a>Servicios en la nube
En el caso de los servicios en la nube, se crea una recomendación cuando la versión del sistema operativo está obsoleta.

![Servicios en la nube](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

En un escenario en el que haya alguna recomendación (lo que no ocurre en el ejemplo anterior), es preciso seguir los pasos descritos en la recomendación para actualizar la versión del sistema operativo. Cuando haya alguna actualización disponible, aparecerá una alerta (su color, rojo o naranja, depende de la gravedad del problema). Al seleccionar esta alerta en las filas WebRole1 (ejecuta Windows Server con la aplicación web implementada automáticamente en IIS) o WorkerRole1 (ejecuta Windows Server con la aplicación web implementada automáticamente en IIS), puede ver más detalles sobre esta recomendación.

Para ver una explicación más preceptiva con respecto a esta recomendación, haga clic en **Actualizar versión del SO** en la columna **DESCRIPCIÓN**.



![Actualizar versión del sistema operativo](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png)

### <a name="app-services"></a>App Services
Para ver la información de App Service, debe habilitar este servicio en la suscripción. Para instrucciones sobre cómo habilitar esta característica, consulte [Protección de App Service con Azure Security Center](security-center-app-services.md).
[!NOTE]
> La supervisión de App Service se encuentra en versión preliminar y solo está disponible en el nivel estándar de Security Center.


En **App Services**, encontrará una lista de los entornos de App Service y el resumen de su estado según la evaluación que realizó Security Center.

![App Services](./media/security-center-virtual-machine-recommendations/app-services.png)

Hay tres tipos de iconos representados en esta lista:

![Entorno de App Services](./media/security-center-virtual-machine-recommendations/ase.png) Entorno de App Services.

![Aplicación web](./media/security-center-virtual-machine-recommendations/web-app.png) Aplicación web.

![Aplicación de función](./media/security-center-virtual-machine-recommendations/function-app.png) Aplicación de función.

1. Seleccione una aplicación web. Se abre una vista de resumen con tres pestañas:

   - **Recomendaciones**: en función de las evaluaciones de Security Center que mostraron errores.
   - **Evaluaciones aprobadas**: lista de las evaluaciones realizadas por Security Center que se aprobaron.
   - **Evaluaciones no disponibles**: lista de las evaluaciones que no se pudieron ejecutar debido a un error o en las que la recomendación no era adecuada para la instancia de App Service concreta.

   En **Recomendaciones** hay una lista de las recomendaciones para la aplicación web seleccionada y la gravedad de cada una.

   ![Recomendaciones de App Services](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

2. Seleccione una recomendación para ver su descripción, una lista de los recursos con estados correctos e incorrectos y una lista de los recursos sin examinar.

   - En la columna **Evaluaciones aprobadas** hay una lista de las evaluaciones que se aprobaron.  La gravedad de estas evaluaciones siempre aparece en verde.

   - Seleccione una evaluación aprobada en la lista para obtener su descripción, una lista de los recursos con estados correctos e incorrectos, y una lista de los recursos sin examinar. Hay una pestaña para los recursos con estado incorrecto, pero está siempre vacía ya que se trata de una evaluación aprobada.

     ![Corrección de App Service](./media/security-center-virtual-machine-recommendations/app-service-remediation.png)

## <a name="virtual-machine-scale-sets"></a>Conjuntos de escalado de máquinas virtuales
Security Center detecta automáticamente si tiene conjuntos de escalado y recomienda instalar a Microsoft Monitoring Agent en estos conjuntos de escalado. 

Para la instalación de Microsoft Monitoring Agent: 

1. Seleccione la recomendación **Instalar el agente de supervisión en conjuntos de escalado de máquinas virtuales**. Obtiene una lista de conjuntos de escalado no supervisados.
2. Seleccione un conjunto de escalado en mal estado. Siga las instrucciones para instalar al agente de supervisión con un área de trabajo completada existente o cree una nueva. Asegúrese de establecer el [plan de tarifa](security-center-pricing.md) del área de trabajo si aún no está establecido.

   ![Instalación de MMA](./media/security-center-virtual-machine-recommendations/install-mms.png)

Para establecer nuevos conjuntos de escalado para instalar automáticamente Microsoft Monitoring Agent:
1. Vaya a Azure Policy y haga clic en **Definiciones**.
2. Busque la directiva **Implementar el agente de Log Analytics para conjuntos de escalado de máquinas virtuales de Windows** y haga clic en ella.
3. Haga clic en **Asignar**.
4. Elija el **ámbito** y el **área de trabajo de Log Analytics** y haga clic en **Asignar**.

Si desea configurar todos los conjuntos de escalado existentes para instalar Microsoft Monitoring Agent, en Azure Policy, vaya a **Corrección** y aplique la directiva existente en conjuntos de escalado existentes.


## <a name="compute-and-app-recommendations"></a>Recomendaciones de proceso y aplicación
|Tipo de recurso|Puntuación segura|Recomendación|DESCRIPCIÓN|
|----|----|----|----|
|App Service|20|Acceso a la aplicación web solo a través de HTTPS|Limite el acceso a las aplicaciones web únicamente a HTTPS.|
|App Service|20|Acceso a Function App solo a través de HTTPS|Limitar el acceso de Function Apps únicamente a través de HTTPS|
|App Service|5|Se deben habilitar los registros de diagnósticos en App Services|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|App Service|10|Recomendación de desactivación de la depuración remota para aplicaciones web|Desactive la depuración para aplicaciones web si ya no necesita usarla. La depuración remota requiere puertos de entrada que se abran en una instancia de Function App.|
|App Service|10|Recomendación de desactivación de la depuración remota para Function App|Desactive la depuración para Function App si ya no necesita usarla. La depuración remota requiere puertos de entrada que se abran en una instancia de Function App.|
|App Service|10|No permita que todos los recursos ("*") tengan acceso a la aplicación| No permita la configuración del parámetro WEBSITE_LOAD_CERTIFICATES en "". Si establece el parámetro en ", todos los certificados se cargan en el almacén de certificados personales de su aplicación web. Esto puede dar lugar a un abuso del principio de privilegio mínimo, ya que es poco probable que el sitio necesite tener acceso a todos los certificados en tiempo de ejecución.|
|App Service|20|Recomendación de que CORS no permita que todos los recursos accedan a las aplicaciones web|Permita solo que los dominios requeridos interactúen con la aplicación web. El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación web.|
|App Service|20|CORS no debe permitir que todos los recursos accedan a Function App| Permita solo que los dominios requeridos interactúen con la aplicación de función. El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación de función.|
|Recursos de proceso (Batch)|1|Las reglas de alerta de métrica deben configurarse en las cuentas de Batch|Configure las reglas de alerta métrica en la cuenta de Batch y habilite las métricas de Eliminar grupo de eventos completos y Eliminar grupo de eventos de inicio.|
|Recursos de proceso (Service Fabric)|10|Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente|Realice la autenticación de clientes solo mediante Azure Active Directory en Service Fabric.|
|Recursos de proceso (cuenta de Automation)|5|Se deben cifrar las variables de cuenta de Automation|Habilite el cifrado de recursos de variables de cuentas de Automation al almacenar datos confidenciales.|
|Recursos de proceso (Search)|5|Permite auditar la habilitación de registros de diagnóstico para Search Service|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Service Bus)|5|Se deben habilitar los registros de diagnósticos en Service Bus|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Stream Analytics)|5|Se deben habilitar los registros de diagnóstico en Azure Stream Analytics|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Batch)|5|Habilitar los registros de diagnóstico en las cuentas de Batch|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Centro de eventos)|5|Se deben habilitar los registros de diagnósticos en el centro de eventos|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Logic Apps)|5|Habilitar los registros de diagnóstico en Logic Apps|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Service Fabric)|15|Establecer la propiedad ClusterProtectionLevel en EncryptAndSign en Service Fabric|Service Fabric proporciona tres niveles de protección (None, Sign y EncryptAndSign) para la comunicación de nodo a nodo mediante un certificado de clúster principal.  Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente. |
|Recursos de proceso (Service Bus)|1|Quitar todas las reglas de autorización excepto RootManageSharedAccessKey del espacio de nombres del Service Bus |Los clientes de Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso a nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Recursos de proceso (Centro de eventos)|1|Todas las reglas de autorización excepto RootManageSharedAccessKey se deben eliminar del espacio de nombres del centro de eventos|Los clientes del Centro de eventos no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso a nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Recursos de proceso (Centro de eventos)|5|Deben definirse las reglas de autorización en la entidad del centro de eventos|Audite las reglas de autorización en la entidad de Centro de eventos para conceder acceso con privilegios mínimos.|
|Máquina|50|Instalar agente de supervisión en las máquinas|Instale el agente de supervisión para habilitar la recopilación de datos, el análisis de actualizaciones, el análisis de línea de base y la protección de puntos de conexión en cada máquina.|
|Máquina|50|Habilitar el aprovisionamiento automático y la recopilación de datos para las suscripciones |Habilite el aprovisionamiento automático y la recopilación de datos para las máquinas en las suscripciones a fin de habilitar la recopilación de datos, el análisis de las actualizaciones, el análisis de la línea de base y la protección de los puntos de conexión en cada máquina agregada a sus suscripciones.|
|Máquina|40|Resolver problemas de estado del agente de supervisión en las máquinas|Para una protección completa de Security Center, resuelva los problemas del agente de supervisión en los equipos siguiendo las instrucciones de la guía de solución de problemas.| 
|Máquina|40|Resolver problemas de estado de protección de puntos de conexión en las máquinas|Para una protección completa de Security Center, resuelva los problemas del agente de supervisión en las máquinas siguiendo las instrucciones de la guía de solución de problemas.|
|Máquina|40|Solucionar problemas derivados de la falta de datos de examen en las máquinas|Solucione problemas derivados de datos de examen que faltan en máquinas virtuales y equipos. La falta de datos de examen en sus máquinas provoca también ausencias en las evaluaciones de seguridad, como el análisis de actualizaciones, el análisis de línea de base y la falta de análisis de soluciones de protección de puntos de conexión.|
|Máquina|40|Se deben instalar actualizaciones del sistema en las máquinas|Instale actualizaciones faltantes de seguridad y críticas para proteger sus máquinas virtuales y equipos de Windows y Linux.
|Máquina|15|Agregar un firewall de aplicaciones web| Implementar una solución de firewall de aplicaciones web para proteger las aplicaciones web. |
|Máquina|40|Actualizar la versión del SO para los roles de servicio en la nube|Actualice la versión de sistema operativo (SO) para los roles de servicio en la nube a la versión más reciente disponible para la familia de sistema operativo.|
|Máquina|35|Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas|Corrija vulnerabilidades en la configuración de seguridad en las máquinas para protegerlas de ataques.|
|Máquina|35|Corregir vulnerabilidades en la configuración de seguridad en los contenedores|Corrija vulnerabilidades en la configuración de seguridad en equipos con Docker instalado para protegerlos de ataques.|
|Máquina|25|Habilitar controles de aplicaciones adaptables|Habilite el control de aplicaciones para controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales ubicadas en Azure. Esto le ayudará a proteger las máquinas virtuales frente a malware. Security Center usa el aprendizaje automático para analizar las aplicaciones que se ejecutan en cada máquina virtual y le ayuda a aplicar reglas de permisos con esta inteligencia. Esta funcionalidad simplifica el proceso de configuración y mantenimiento de reglas de permisos de la aplicación.|
|Máquina|20|Instalar una solución de protección de punto de conexión en las máquinas|Instale una solución de protección de punto de conexión en las máquinas virtuales para protegerlas frente a amenazas y vulnerabilidades.|
|Máquina|20|Reiniciar sus máquinas para aplicar las actualizaciones del sistema|Reinicie sus máquinas para aplicar las actualizaciones del sistema y protegerlas ante vulnerabilidades.|
|Máquina|15|Se debe aplicar el cifrado de discos en las máquinas virtuales|Cifre los discos de máquina virtual mediante Azure Disk Encryption para máquinas virtuales Windows y Linux. Azure Disk Encryption (ADE) aprovecha la característica BitLocker de Windows estándar del sector y la característica DM-Crypt de Linux para proporcionar cifrado del disco de datos y del sistema operativo a fin de proteger sus datos y ayudar a asumir las responsabilidades de seguridad y cumplimiento de la organización en un almacén de claves de Azure del cliente. Cuando sus requisitos de cumplimiento y seguridad exijan que cifre los datos de extremo a extremo con sus claves de cifrado, incluido el cifrado del disco efímero (temporal adjunto localmente), utilice Azure Disk Encryption. Como alternativa, los discos de Managed Disks se cifran en reposo de forma predeterminada mediante Azure Storage Service Encryption, donde las claves de cifrado son claves administradas por Microsoft en Azure. Si esto cumple sus requisitos de seguridad y cumplimiento, puede aprovechar el cifrado predeterminado de Managed Disks para satisfacer sus necesidades.|
|Máquina|30|Instale una solución de evaluación de la vulnerabilidad en sus máquinas virtuales.|Instale una solución de evaluación de la vulnerabilidad en sus máquinas virtuales.|
|Máquina|15|Agregar un firewall de aplicaciones web| Implementar una solución de firewall de aplicaciones web para proteger las aplicaciones web. |
|Máquina|30|Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades|Las máquinas virtuales para las que se implementa una solución de evaluación de vulnerabilidades de terceros se evalúan de forma continua en relación con las vulnerabilidades de la aplicación y del sistema operativo. Cada vez que se encuentran vulnerabilidades de este tipo, es posible obtener más información sobre ellas como parte de la recomendación.|
|Máquina|30|Instale una solución de evaluación de la vulnerabilidad en sus máquinas virtuales.|Instale una solución de evaluación de la vulnerabilidad en sus máquinas virtuales.|
|Máquina|1|Se deben migrar las máquinas virtuales a nuevos recursos de AzureRM|Use Azure Resource Manager para las máquinas virtuales a fin de proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobernanza e implementación basados en Resource Manager, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de seguridad. |
|Máquina|30|Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades|Las máquinas virtuales para las que se implementa una solución de evaluación de vulnerabilidades de terceros se evalúan de forma continua en relación con las vulnerabilidades de la aplicación y del sistema operativo. Cada vez que se encuentran vulnerabilidades de este tipo, es posible obtener más información sobre ellas como parte de la recomendación.|
|Conjunto de escalado de máquina virtual |4|Los registros de diagnóstico en Virtual Machine Scale Sets deberían habilitarse|Habilite los registros y consérvelos por hasta un año. Esto le permite volver a crear seguimientos de actividad para fines de investigación. Esto es útil cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Conjunto de escalado de máquina virtual|35|Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales|Corrija vulnerabilidades en la configuración de seguridad en conjuntos de escalado de máquinas virtuales para protegerlas de ataques. |
|Conjunto de escalado de máquina virtual|5|Corregir errores de estado de protección de puntos de conexión en conjuntos de escalado de máquinas virtuales|Corrija los errores de estado de la protección de puntos de conexión en conjuntos de escalado de sus máquinas virtuales para protegerlas de amenazas y vulnerabilidades. |
|Conjunto de escalado de máquina virtual|10|Endpoint Protection debe instalarse en las máquinas virtuales|Instale una solución de protección de punto de conexión en los conjuntos de escalado de máquinas virtuales para protegerlos frente a amenazas y vulnerabilidades. |
|Conjunto de escalado de máquina virtual|40|Se debe instalar las actualizaciones del sistema en los conjuntos de escalado de máquinas virtuales|Instale actualizaciones faltantes de seguridad y críticas para proteger sus conjuntos de escalado de máquinas virtuales de Windows y Linux. |
 





## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:


* [Protección de las aplicaciones y las máquinas en Azure Security Center](security-center-virtual-machine-protection.md)
* [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.

