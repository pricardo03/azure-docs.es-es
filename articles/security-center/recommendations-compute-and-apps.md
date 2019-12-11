---
title: Recomendaciones de Azure Security Center para las máquinas y las aplicaciones
description: Recomendaciones de seguridad de Azure Security Center que ayudan a proteger las máquinas virtuales, los equipos, las aplicaciones web y los entornos de App Service.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2019
ms.author: memildin
ms.openlocfilehash: c0cf5951daf004a0c805b688f08d1ccfa4679615
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781904"
---
# <a name="compute-and-app-recommendations---reference-guide"></a>Recomendaciones de proceso y aplicación: guía de referencia

En este artículo se proporciona la lista completa de las recomendaciones que puede ver en Azure Security Center con respecto a los siguientes tipos de recursos:

* Máquinas virtuales y equipos
* VM Scale Sets
* Cloud Services
* App Services
* Contenedores
* Recursos de proceso

Para una explicación de cómo buscarlos y resolverlos, consulte [esto](security-center-virtual-machine-protection.md).

## Recomendaciones de proceso y aplicación <a name="compute-and-app-recs"></a>
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
|Recursos de proceso (Service Fabric)|15|Establecer la propiedad ClusterProtectionLevel en EncryptAndSign en Service Fabric|Service Fabric proporciona tres niveles de protección (None, Sign y EncryptAndSign) para la comunicación de nodo a nodo mediante un certificado de clúster principal. Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente. |
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
|Máquina|1|Se deben migrar las máquinas virtuales a nuevos recursos de AzureRM|Use Azure Resource Manager para ofrecer en las máquinas virtuales mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobernanza e implementación basados en Resource Manager, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de la seguridad. |
|Máquina|30|Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades|Las máquinas virtuales para las que se implementa una solución de evaluación de vulnerabilidades de terceros se evalúan de forma continua en relación con las vulnerabilidades de la aplicación y del sistema operativo. Cada vez que se encuentran vulnerabilidades de este tipo, es posible obtener más información sobre ellas como parte de la recomendación.|
|Conjunto de escalado de máquina virtual |4|Los registros de diagnóstico en Virtual Machine Scale Sets deberían habilitarse|Habilite los registros y consérvelos por hasta un año. Esto le permite volver a crear seguimientos de actividad para fines de investigación. Esto es útil cuando se produce un incidente de seguridad o se pone en peligro su red.|
|Conjunto de escalado de máquina virtual|35|Se deben corregir las vulnerabilidades en la configuración de seguridad de los conjuntos de escalado de máquinas virtuales|Corrija vulnerabilidades en la configuración de seguridad en conjuntos de escalado de máquinas virtuales para protegerlas de ataques. |
|Conjunto de escalado de máquina virtual|5|Corregir errores de estado de protección de puntos de conexión en conjuntos de escalado de máquinas virtuales|Corrija los errores de estado de la protección de puntos de conexión en conjuntos de escalado de sus máquinas virtuales para protegerlas de amenazas y vulnerabilidades. |
|Conjunto de escalado de máquina virtual|10|Endpoint Protection debe instalarse en las máquinas virtuales|Instale una solución de protección de punto de conexión en los conjuntos de escalado de máquinas virtuales para protegerlos frente a amenazas y vulnerabilidades. |
|Conjunto de escalado de máquina virtual|40|Se debe instalar las actualizaciones del sistema en los conjuntos de escalado de máquinas virtuales|Instale actualizaciones faltantes de seguridad y críticas para proteger sus conjuntos de escalado de máquinas virtuales de Windows y Linux. |
|


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)
