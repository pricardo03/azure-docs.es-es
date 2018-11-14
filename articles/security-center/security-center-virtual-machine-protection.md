---
title: Protección de las aplicaciones y las máquinas en Azure Security Center | Microsoft Docs
description: En este documento se describen las recomendaciones de Security Center que le ayudan a proteger las máquinas virtuales, equipos, aplicaciones web y los entornos de App Service.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 0bd93e0732b0e822fae68b4ac2cc5e28fb246808
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016971"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Protección de las aplicaciones y las máquinas en Azure Security Center
El Centro de seguridad de Azure analiza el estado de seguridad de los recursos de Azure. Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios. Las recomendaciones se aplican a los tipos de recursos de Azure: máquinas virtuales, equipos, aplicaciones, redes, SQL e identidad y acceso.

Este artículo trata sobre las recomendaciones aplicables a máquinas y aplicaciones.

## <a name="monitoring-security-health"></a>Supervisión del estado de seguridad
Puede supervisar el estado de seguridad de los recursos en el panel **Security Center – Información general**. La sección **Recursos** indica el número de problemas identificados y el estado de seguridad para cada tipo de recurso.

Para ver una lista de todos los problemas, puede seleccionar **Recomendaciones**. Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).

Para ver una lista completa de las recomendaciones sobre procesos y recomendaciones, consulte [Recomendaciones](security-center-virtual-machine-recommendations.md).

Para continuar, seleccione **Procesos y aplicaciones** en **Recursos** o en el menú principal de Security Center.
![Panel de Security Center][1]

## <a name="monitor-compute-and-app-services"></a>Supervisión de los servicios de procesos y aplicaciones
En **Procesos**, hay cuatro pestañas:

- **Información general**: supervisión y recomendaciones identificadas por Security Center.
- **VM y equipos**: muestra el estado de seguridad actual de todas las máquinas virtuales y equipos.
- **Cloud Services**: lista de todos los roles web y de trabajo que supervisa Security Center.
- **App Services (versión preliminar)**: muestra la lista actual de los entornos de App Service y el estado de seguridad actual de cada uno.
Para continuar, seleccione **Procesos y aplicaciones** en **Recursos** o en el menú principal de Security Center.

![Compute][2]

Todas las pestañas pueden tener varias secciones y en cada una de ella se puede seleccionar cualquiera de las opciones para ver más detalles acerca de los pasos recomendados para abordar un problema concreto.

### <a name="monitoring-recommendations"></a>Supervisión de las recomendaciones
En esta sección se muestra el número total de máquinas virtuales y equipos que se inicializaron para el aprovisionamiento automático y sus estados actuales. En este ejemplo hay una recomendación, **Problemas de mantenimiento del agente de supervisión**. Seleccione esta recomendación.

![Supervisión de problemas de Agent Health][3]

Se abre **Problemas de mantenimiento del agente de supervisión**. Se enumeran las máquinas virtuales y los equipos que el Centro de seguridad no puede supervisar correctamente. Seleccione una máquina virtual o un equipo para obtener información detallada. **ESTADO DE SUPERVISIÓN** proporciona un motivo por el que Security Center no puede supervisar. Consulte la [Guía de solución de problemas de Security Center](security-center-troubleshooting-guide.md) para obtener una lista de los valores, las descripciones y los pasos de resolución de **ESTADO DE SUPERVISIÓN**.

### Equipos y máquinas virtuales no supervisados <a name="unmonitored-vms-and-computers"></a>
Una máquina virtual o un equipo no está supervisado por Security Center si no está ejecutando la extensión de Microsoft Monitoring Agent. Una máquina virtual puede tener ya instalado un agente local, por ejemplo un agente directo de OMS o el agente SCOM. Las máquinas virtuales con estos agentes se identifican como sin supervisar porque estos agentes no son totalmente compatibles en Security Center. Para aprovechar al máximo todas las funcionalidades de Security Center, se necesita la extensión de Microsoft Monitoring Agent.

Puede instalar la extensión en la máquina virtual o en el equipo no supervisados además del agente local ya instalado. Configure de la misma forma a ambos agentes, conectándolos a la misma área de trabajo. Esto permite que Security Center interactúe con la extensión de Microsoft Monitoring Agent y recopile datos. Consulte [Habilitar la extensión de VM](../log-analytics/log-analytics-quick-collect-azurevm.md) para obtener instrucciones acerca de cómo instalar la extensión de Microsoft Monitoring Agent.

Consulte [Supervisión de problemas de Agent Health](security-center-troubleshooting-guide.md#mon-agent) para saber más acerca de los motivos por los que Security Center no puede supervisar correctamente las máquinas virtuales y los equipos inicializados para el aprovisionamiento automático.

### <a name="recommendations"></a>Recomendaciones
Esta sección incluye un conjunto de recomendaciones para cada máquina virtual y equipo, para cada rol web o de trabajo, para cada entorno de Azure App Service Web Apps y Azure App Service que supervisa Security Center. La primera columna muestra la recomendación. La segunda columna muestra el número total de recursos a los que afecta dicha recomendación. La tercera columna muestra la gravedad del problema como se muestra en la captura de pantalla siguiente:

![Recomendaciones][4]

Cada recomendación tiene un conjunto de acciones que se podrán realizar una vez que la selecciona. Por ejemplo, si selecciona **Faltan las actualizaciones del sistema**, aparece el número de máquinas virtuales y equipos a los que les faltan revisiones y la gravedad de la actualización que falta, como se muestra en la captura de pantalla siguiente:

![Aplicar actualizaciones del sistema][5]

**Aplicar actualizaciones del sistema** incluye un resumen de actualizaciones críticas en formato de grafo, uno para Windows y otro para Linux. La segunda parte tiene una tabla con la siguiente información:

- **NOMBRE**: nombre de la actualización que falta.
- **NÚMERO DE MÁQUINAS VIRTUALES Y EQUIPOS**: número total de máquinas virtuales y equipos que no tienen esta actualización.
- **GRAVEDAD DE LA ACTUALIZACIÓN**: describe la gravedad de una recomendación concreta:

    - **Crítica**: existe una vulnerabilidad en un recurso importante (aplicación, máquina virtual o grupo de seguridad de red) y requiere atención.
    - **Importante**: para completar un proceso o eliminar una vulnerabilidad se requieren pasos adicionales o no críticos.
    - **Moderada**: es preciso abordar una vulnerabilidad, pero esta no requiere una atención inmediata. (De manera predeterminada no se muestran las recomendaciones bajas, pero si desea verlas, puede filtrar por ellas).


- **ESTADO**: el estado actual de la recomendación:

    - **Abierta**: la recomendación aún no se ha abordado.
    - **En curso**: la recomendación se está aplicando actualmente a esos recursos y no se requiere ninguna acción por su parte.
    - **Resuelta**: la recomendación ya terminó. (Si se ha resuelto el problema, la entrada aparecerá atenuada).

Para ver los detalles de recomendación, haga clic en el nombre de la actualización que falta en la lista.

![Detalles de recomendación][6]

> [!NOTE]
> Las recomendaciones de seguridad son las mismas que aparecen si selecciona el icono **Recomendaciones**. Para más información sobre cómo resolver las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).
>
>

### <a name="vms-and-computers"></a>Máquinas virtuales y equipos
La sección de máquinas virtuales y equipos ofrece una visión general de todas las recomendaciones de máquinas virtuales y equipos. Cada columna representa un conjunto de recomendaciones tal como se muestra en la siguiente captura de pantalla:

![Recomendaciones de máquina virtual y equipo][7]

Hay cuatro tipos de iconos representados en esta lista:

![Equipos que no son de Azure][8] Equipo que no es de Azure.

![Máquina virtual de Azure Resource Manager][9] Máquina virtual de Azure Resource Manager.

![Máquina virtual de Azure clásico][10] Máquina virtual de Azure clásico.

![Máquinas virtuales identificadas del área de trabajo][11] Máquinas virtuales que se identifican solo desde el área de trabajo que forma parte de la suscripción visualizada. Aquí se incluyen máquinas virtuales de otras suscripciones que informan al área de trabajo de esta suscripción y máquinas virtuales que se instalaron con el agente directo de SCOM y no tienen id. de recurso.

El icono que aparece en cada recomendación le ayuda a identificar rápidamente las máquinas virtuales y los equipos que necesitan atención y el tipo de recomendación. También puede usar la opción Filtro para seleccionar las opciones que verá en esta pantalla.

![Filtrar][12]

En el ejemplo anterior, una máquina virtual tiene una recomendación crítica sobre la protección del punto de conexión. Seleccione la máquina virtual para obtener más información sobre ella:

![Recomendación crítica][13]

Aquí verá los detalles de seguridad de la máquina virtual o del equipo. En la parte inferior puede ver la acción recomendada y la gravedad de cada problema.

### <a name="cloud-services"></a>Servicios en la nube
En el caso de los servicios en la nube, se crea una recomendación cuando la versión del sistema operativo está obsoleta, como se muestra en la siguiente captura de pantalla:

![Servicios en la nube][14]

En un escenario en el que haya alguna recomendación (lo que no ocurre en el ejemplo anterior), es preciso seguir los pasos descritos en la recomendación para actualizar la versión del sistema operativo. Cuando haya alguna actualización disponible, aparecerá una alerta (su color, rojo o naranja, depende de la gravedad del problema). Al seleccionar esta alerta en las filas WebRole1 (ejecuta Windows Server con la aplicación web implementada automáticamente en IIS) o WorkerRole1 (ejecuta Windows Server con la aplicación web implementada automáticamente en IIS), puede ver más detalles sobre esta recomendación, como se muestra en la siguiente captura de pantalla:

![WorkerRole1][15]

Para ver una explicación más preceptiva con respecto a esta recomendación, haga clic en **Actualizar versión del SO** en la columna **DESCRIPCIÓN**.

![Actualizar versión del sistema operativo][16]

### <a name="app-services-preview"></a>App Services (versión preliminar)

> [!NOTE]
> La supervisión de App Service se encuentra en versión preliminar y solo está disponible en el nivel estándar de Security Center. Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
>
>

En **App Services**, encontrará una lista de los entornos de App Service y el resumen de su estado según la evaluación que realizó Security Center.

![App Services][17]

Hay tres tipos de iconos representados en esta lista:

![Entorno de App Services][18] Entorno de App Services.

![Aplicación web][19] Aplicación web.

![Aplicación de función][24] Aplicación de función.

1. Seleccione una aplicación web. Se abre una vista de resumen con tres pestañas:

  - **Recomendaciones**: en función de las evaluaciones de Security Center que mostraron errores.
  - **Evaluaciones aprobadas**: lista de las evaluaciones realizadas por Security Center que se aprobaron.
  - **Evaluaciones no disponibles**: lista de las evaluaciones que no se pudieron ejecutar debido a un error o en las que la recomendación no era adecuada para la instancia de App Service concreta.

  En **Recomendaciones** hay una lista de las recomendaciones para la aplicación web seleccionada y la gravedad de cada una.

  ![Vista de resumen][20]

2. Seleccione una recomendación para obtener su descripción, una lista de los recursos con estados correctos e incorrectos, y una lista de los recursos sin examinar.

  ![Descripción de la recomendación][21]

  En **Evaluaciones aprobadas** hay una lista de las evaluaciones que se consideraron superadas.  La gravedad de estas evaluaciones siempre aparece en verde.

  ![Evaluaciones aprobadas][22]

3. Seleccione una evaluación aprobada en la lista para obtener su descripción, una lista de los recursos con estados correctos e incorrectos, y una lista de los recursos sin examinar. Hay una pestaña para los recursos con estado incorrecto, pero está siempre vacía ya que se trata de una evaluación aprobada.

    ![Recursos con estado correcto][23]

## <a name="compute-and-app-recommendations"></a>Recomendaciones de proceso y aplicación
|Tipo de recurso|Puntuación segura|Recomendación|DESCRIPCIÓN|
|----|----|----|----|
|Máquina|50|Instalar agente de supervisión en las máquinas|Instale el agente de supervisión para habilitar la recopilación de datos, el análisis de actualizaciones, el análisis de línea de base y la protección de puntos de conexión en cada máquina.|
|Máquina|50|Habilitar el aprovisionamiento automático y la recopilación de datos para las suscripciones |Habilite el aprovisionamiento automático y la recopilación de datos para las máquinas en las suscripciones a fin de habilitar la recopilación de datos, el análisis de las actualizaciones, el análisis de la línea de base y la protección de los puntos de conexión en cada máquina agregada a sus suscripciones.|
|Máquina|40|Resolver problemas de estado del agente de supervisión en las máquinas|Para una protección completa de Security Center, resuelva los problemas del agente de supervisión en los equipos siguiendo las instrucciones de la guía de solución de problemas.| 
|Máquina|40|Resolver problemas de estado de protección de puntos de conexión en las máquinas|Para una protección completa de Security Center, resuelva los problemas del agente de supervisión en las máquinas siguiendo las instrucciones de la guía de solución de problemas.|
|Máquina|40|Solucionar problemas derivados de la falta de datos de examen en las máquinas|Solucione problemas derivados de datos de examen que faltan en máquinas virtuales y equipos. La falta de datos de examen en sus máquinas provoca también ausencias en las evaluaciones de seguridad, como el análisis de actualizaciones, el análisis de línea de base y la falta de análisis de soluciones de protección de puntos de conexión.|
|Máquina|40|Instalar actualizaciones del sistema en las máquinas|Instale actualizaciones faltantes de seguridad y críticas para proteger sus máquinas virtuales y equipos de Windows y Linux.
|Máquina|40|Actualizar la versión del SO para los roles de servicio en la nube|Actualice la versión de sistema operativo (SO) para los roles de servicio en la nube a la versión más reciente disponible para la familia de sistema operativo.|
|Máquina|35|Corregir vulnerabilidades en la configuración de seguridad en las máquinas|Corrija vulnerabilidades en la configuración de seguridad en las máquinas para protegerlas de ataques. |
|Máquina|35|Corregir vulnerabilidades en la configuración de seguridad en los contenedores|Corrija vulnerabilidades en la configuración de seguridad en equipos con Docker instalado para protegerlos de ataques.|
|Máquina|25|Habilitar controles de aplicaciones adaptables|Habilite el control de aplicaciones para controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales ubicadas en Azure. Esto le ayudará a proteger las máquinas virtuales frente a malware. Security Center usa el aprendizaje automático para analizar las aplicaciones que se ejecutan en cada máquina virtual y le ayuda a aplicar reglas de permisos con esta inteligencia. Esta funcionalidad simplifica el proceso de configuración y mantenimiento de reglas de permisos de la aplicación.|
|Máquina|20|Instalar una solución de protección de punto de conexión en las máquinas|Instale una solución de protección de punto de conexión en las máquinas virtuales para protegerlas frente a amenazas y vulnerabilidades.|
|Máquina|20|Reiniciar sus máquinas para aplicar las actualizaciones del sistema|Reinicie sus máquinas para aplicar las actualizaciones del sistema y protegerlas ante vulnerabilidades.|
|App Service|20|Acceso a la aplicación web solo a través de HTTPS|Limite el acceso a las aplicaciones web únicamente a HTTPS.|
|App Service|20|Acceso a Function App solo a través de HTTPS|Limitar el acceso de Function Apps únicamente a través de HTTPS|
|Máquina|15|Aplicar Disk Encryption a las máquinas virtuales|Cifre los discos de máquina virtual mediante Azure Disk Encryption para máquinas virtuales Windows y Linux. Azure Disk Encryption (ADE) aprovecha la característica BitLocker de Windows estándar del sector y la característica DM-Crypt de Linux para proporcionar cifrado del disco de datos y del sistema operativo a fin de proteger sus datos y ayudar a asumir las responsabilidades de seguridad y cumplimiento de la organización en un almacén de claves de Azure del cliente. Cuando sus requisitos de cumplimiento y seguridad exijan que cifre los datos de extremo a extremo con sus claves de cifrado, incluido el cifrado del disco efímero (temporal adjunto localmente), utilice Azure Disk Encryption. Como alternativa, los discos de Managed Disks se cifran en reposo de forma predeterminada mediante Azure Storage Service Encryption, donde las claves de cifrado son claves administradas por Microsoft en Azure. Si esto cumple sus requisitos de seguridad y cumplimiento, puede aprovechar el cifrado predeterminado de Managed Disks para satisfacer sus necesidades.|
|Recursos de proceso (Service Fabric)|10|Usar Azure Active Directory para la autenticación de clientes en Service Fabric|Realice la autenticación de clientes solo mediante Azure Active Directory en Service Fabric.|
|Recursos de proceso (cuenta de Automation)|5| Habilitar el cifrado de la cuenta de Automation|Habilite el cifrado de recursos de variables de cuentas de Automation al almacenar datos confidenciales.|
|App Service|5|Habilitar los registros de diagnósticos de App Service|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Load Balancer)|5|Habilitar los registros de diagnóstico en Load Balancer|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Search)|5|Habilitar registros de diagnóstico en el servicio Search|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Service Bus)|5|Habilitar los registros de diagnósticos en Service Bus|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Stream Analytics)|5|Habilitar los registros de diagnóstico en Azure Stream Analytics|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Service Fabric)|5|Habilitar los registros de diagnósticos en Service Fabric|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Batch)|5|Habilitar los registros de diagnóstico en las cuentas de Batch|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Centro de eventos)|5|Habilitar los registros de diagnósticos en el Centro de eventos|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Recursos de proceso (Logic Apps)|5|Habilitar los registros de diagnóstico en Logic Apps|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Máquina|30|Instalar una solución de evaluación de la vulnerabilidad en sus máquinas virtuales|Instale una solución de evaluación de la vulnerabilidad en sus máquinas virtuales.|
|Máquina|15|Agregar un firewall de aplicaciones web| Implementar una solución de firewall de aplicaciones web para proteger las aplicaciones web. |
|Máquina|30|Corregir vulnerabilidades mediante una solución de evaluación de vulnerabilidades|Las máquinas virtuales para las que se implementa una solución de evaluación de vulnerabilidades de terceros se evalúan de forma continua en relación con las vulnerabilidades de la aplicación y del sistema operativo. Cada vez que se encuentran vulnerabilidades de este tipo, es posible obtener más información sobre ellas como parte de la recomendación.|
|Recursos de proceso (Service Fabric)|15|Establecer la propiedad ClusterProtectionLevel en EncryptAndSign en Service Fabric|Service Fabric proporciona tres niveles de protección (None, Sign y EncryptAndSign) para la comunicación de nodo a nodo mediante un certificado de clúster principal.  Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente. |
|App Service|10|Recomendación de desactivación de la depuración remota para aplicaciones web|Desactive la depuración para aplicaciones web si ya no necesita usarla. La depuración remota requiere puertos de entrada que se abran en una instancia de Function App.|
|App Service|10|Recomendación de desactivación de la depuración remota para Function App|Desactive la depuración para Function App si ya no necesita usarla. La depuración remota requiere puertos de entrada que se abran en una instancia de Function App.|
|App Service|10|Configuración de las restricciones de IP de las aplicaciones web|Defina una lista de direcciones IP que tienen permiso para acceder a su aplicación. El uso de restricciones de IP protege una aplicación web frente a ataques comunes.|
|App Service|10|Configuración de las restricciones de IP para Function App| Defina una lista de direcciones IP que tienen permiso para acceder a su aplicación. El uso de restricciones de IP protege una aplicación de función frente a ataques comunes.|
|App Service|10|No permita que todos los recursos ("*") tengan acceso a la aplicación| No permita la configuración del parámetro WEBSITE_LOAD_CERTIFICATES en "". Si establece el parámetro en ", todos los certificados se cargan en el almacén de certificados personales de su aplicación web. Esto puede dar lugar a un abuso del principio de privilegio mínimo, ya que es poco probable que el sitio necesite tener acceso a todos los certificados en tiempo de ejecución.|
|App Service|5|Recomendación de desactivación de Web Sockets para aplicaciones web|Revise el uso de Web Sockets en las aplicaciones web. El protocolo Web Sockets es vulnerable a distintos tipos de amenazas de seguridad.|
|App Service|5|Recomendación de desactivación de Web Sockets para Function App|Revise el uso de Web Sockets en las instancias de Function App. El protocolo Web Sockets es vulnerable a distintos tipos de amenazas de seguridad.|
|App Service|5|Uso de dominios personalizados para su aplicación web|Use dominios personalizados para proteger una aplicación web frente a ataques comunes, como la suplantación de identidad y otros ataques relacionados con DNS.|
|App Service|5|Uso de dominios personalizados para Function App|Use dominios personalizados para proteger una aplicación de función frente a ataques comunes, como la suplantación de identidad y otros ataques relacionados con DNS.|
|Recursos de proceso (Batch)|1|Configurar las reglas de alerta de métrica en la cuenta de Batch|Configure las reglas de alerta métrica en la cuenta de Batch y habilite las métricas de Eliminar grupo de eventos completos y Eliminar grupo de eventos de inicio.|
|Recursos de proceso (Service Bus)|1|Quitar todas las reglas de autorización excepto RootManageSharedAccessKey del espacio de nombres del Service Bus |Los clientes de Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso a nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Recursos de proceso (Centro de eventos)|1|Quitar todas las reglas de autorización excepto RootManageSharedAccessKey del espacio de nombres del Centro de eventos |Los clientes del Centro de eventos no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso a nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Recursos de proceso (Centro de eventos)|5|Definir las reglas de autorización en la entidad de Centro de eventos|Audite las reglas de autorización en la entidad de Centro de eventos para conceder acceso con privilegios mínimos.|
|Máquina|30|Instalar una solución de evaluación de la vulnerabilidad en sus máquinas virtuales|Instale una solución de evaluación de la vulnerabilidad en sus máquinas virtuales.|
|App Service|20|Recomendación de que CORS no permita que todos los recursos accedan a las aplicaciones web|Permita solo que los dominios requeridos interactúen con la aplicación web. El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación web.|
|App Service|20|CORS no debe permitir que todos los recursos accedan a Function App| Permita solo que los dominios requeridos interactúen con la aplicación de función. El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación de función.|
|Máquina|15|Agregar un firewall de aplicaciones web| Implementar una solución de firewall de aplicaciones web para proteger las aplicaciones web. |
|App Service|10|Uso de la última versión compatible de .NET Framework para aplicaciones web|Utilice la versión más reciente de .NET Framework para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta.|
|App Service|10|Uso de la última versión compatible de Java para aplicaciones web|Utilice la versión más reciente de Java para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta.|
|App Service|10|Uso de la última versión compatible de Java para aplicaciones web|Utilice la versión más reciente de PHP para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta.|
|App Service|10|Uso de la última versión compatible de Node.js para aplicaciones web|Utilice la versión más reciente de Node.js para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta.|
|App Service|10|Usar la última versión compatible de Python para aplicaciones web|Utilice la versión más reciente de Python para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta.|
|Máquinas virtuales y equipos|1|Migrar las máquinas virtuales a nuevos recursos de AzureRM|Use el nuevo Azure Resource Manager v2 para sus máquinas virtuales para proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobierno e implementación basados en ARM, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de seguridad. |
|Máquina|30|Corregir vulnerabilidades mediante una solución de evaluación de vulnerabilidades|Las máquinas virtuales para las que se implementa una solución de evaluación de vulnerabilidades de terceros se evalúan de forma continua en relación con las vulnerabilidades de la aplicación y del sistema operativo. Cada vez que se encuentran vulnerabilidades de este tipo, es posible obtener más información sobre ellas como parte de la recomendación.|




## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:


* [Descripción de las recomendaciones de Azure Security Center para máquinas virtuales](security-center-virtual-machine-recommendations.md)
* [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](security-center-policies.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
