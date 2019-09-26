---
title: Las definiciones de directiva de Azure supervisadas en Azure Security Center | Microsoft Docs
description: Las definiciones de directiva de Azure supervisadas en Azure Security Center.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 8/22/2019
ms.author: memildin
ms.openlocfilehash: 892b33f52788bd37d430f221f14ba4c98f764cfb
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201670"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>Directivas de seguridad de Azure supervisadas por Security Center
Este artículo proporciona una lista de definiciones de Azure Policy que puede supervisar en Azure Security Center. Para más información acerca de las directivas de seguridad, consulte [Uso de directivas de seguridad](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Directivas de seguridad disponibles

Para comprender las directivas integradas que Security Center supervisa, consulte la tabla siguiente:

| Directiva | Funcionamiento de la directiva |
| --- | --- |
|Los registros de diagnóstico en Virtual Machine Scale Sets deberían habilitarse|Se recomienda habilitar los registros para que haya disponible una traza de actividad para una posible investigación después de un incidente o un peligro.|
|Todas las reglas de autorización excepto RootManageSharedAccessKey se deben eliminar del espacio de nombres del centro de eventos|Los clientes de Azure Event Hubs no deben usar una directiva de acceso del nivel de espacio de nombres que proporcione acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso en las entidades para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Deben definirse las reglas de autorización en la entidad del centro de eventos|Audite la existencia de reglas de autorización en las entidades de Event Hubs para conceder acceso con privilegios mínimos.|
|Se debe restringir el acceso a las cuentas de almacenamiento con configuraciones de red virtual y firewall|Audite el acceso de red no restringido en la configuración de firewall de su cuenta de almacenamiento. Configure reglas de red de modo que solo las aplicaciones de redes permitidas puedan acceder a la cuenta de almacenamiento. Para permitir las conexiones desde clientes específicos en entornos locales o de Internet, conceda acceso al tráfico procedente de redes virtuales de Azure específicas o a intervalos de direcciones IP de Internet públicas.|
|Auditar el uso de reglas de RBAC personalizadas|Audite roles integrados, como "propietario, colaborador o lector", en lugar de roles de control de acceso basado en rol (RBAC) personalizados, que son propensos a errores de auditoría. El uso de roles personalizados se trata como una excepción y requiere una revisión rigurosa y el modelado de amenazas.|
|Se deben habilitar los registros de diagnóstico en Azure Stream Analytics|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se debe habilitar la transferencia segura a las cuentas de almacenamiento|Audite los requisitos de transferencia segura en su cuenta de almacenamiento. La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio. También protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|Se debe aprovisionar un administrador de Azure AD para SQL Server|Audite el aprovisionamiento de un administrador de Azure Active Directory (Azure AD) para SQL Server a fin de habilitar la autenticación de Azure AD. La autenticación de Azure AD permite la administración simplificada de los permisos y la administración centralizada de las identidades de los usuarios de base de datos y otros servicios de Microsoft.|
|Todas las reglas de autorización excepto RootManageSharedAccessKey se deben eliminar del espacio de nombres de Service Bus|Los clientes de Azure Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporcione acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, cree directivas de acceso en el nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Se deben habilitar los registros de diagnósticos en Service Bus|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se debe establecer la propiedad ClusterProtectionLevel en EncryptAndSign en Service Fabric|Service Fabric proporciona tres niveles de protección para la comunicación de nodo a nodo mediante un certificado de clúster principal: None, Sign y EncryptAndSign. Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente.|
|La autenticación de cliente debe usar Azure Active Directory|Audite el uso de la autenticación de clientes solo mediante Azure AD en Service Fabric.|
|Se deben habilitar los registros de diagnósticos en los servicios Search|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Solo se deben habilitar las conexiones seguras a Redis Cache|Audite la habilitación solo de las conexiones a través de SSL a Azure Redis Cache. El uso de conexiones seguras garantiza la autenticación entre el servidor y el servicio. También protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|Se deben habilitar los registros de diagnósticos en Logic Apps|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se deben habilitar los registros de diagnósticos en Key Vault|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se deben habilitar los registros de diagnósticos en el centro de eventos|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se deben habilitar los registros de diagnósticos en Azure Data Lake Storage|Permite auditar la habilitación de los registros y conservarlos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se deben permitir los registros de diagnóstico en Data Lake Analytics|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se deben migrar las cuentas de almacenamiento a nuevos recursos de AzureRM|Utilice Azure Resource Manager para las cuentas de almacenamiento a fin de proporcionar mejoras de seguridad. Entre ellas se incluyen las siguientes: <br>- Control de acceso más seguro (RBAC)<br>- Mejor auditoría<br>- Gobierno e implementación basada en Azure Resource Manager<br>- Acceso a identidades administradas<br>- Acceso a Azure Key Vault para los secretos<br>- Autenticación basada en Azure AD<br>- Compatibilidad con las etiquetas y los grupos de recursos para facilitar la administración de seguridad|
|Se deben migrar las máquinas virtuales a nuevos recursos de AzureRM|Utilice Azure Resource Manager para las máquinas virtuales a fin de proporcionar mejoras de seguridad. Entre ellas se incluyen las siguientes: <br>- Control de acceso más seguro (RBAC)<br>- Mejor auditoría<br>- Gobierno e implementación basada en Azure Resource Manager<br>- Acceso a identidades administradas<br>- Acceso a Azure Key Vault para los secretos<br>- Autenticación basada en Azure AD<br>- Compatibilidad con las etiquetas y los grupos de recursos para facilitar la administración de seguridad|
|Las reglas de alerta de métrica deben configurarse en las cuentas de Batch|Permite auditar la configuración de reglas de alertas de métricas en una cuenta de Azure Batch para habilitar la métrica requerida.|
|Se deben habilitar los registros de diagnóstico en las cuentas de Batch|Audite la habilitación de los registros y consérvelos hasta un año. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|El cifrado debe habilitarse en las variables de cuentas de Automation|Es importante habilitar el cifrado de recursos de variables de cuentas de Azure Automation al almacenar datos confidenciales.|
|Se deben habilitar los registros de diagnósticos en App Services|Permite auditar la habilitación de registros de diagnóstico en la aplicación. De esta forma se crean seguimientos de actividad que pueden investigarse cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Se debe habilitar el Cifrado de datos transparente en bases de datos SQL|Estado de cifrado de datos transparente de auditoría para bases de datos SQL.|
|Se debe habilitar la auditoría de SQL Server|Audite la existencia de la auditoría de SQL en el nivel de servidor.|
|\[Versión preliminar]: Supervisión de la base de datos SQL sin cifrar en Azure Security Center|Azure Security Center supervisa las bases de datos o los servidores SQL Server sin cifrar tal como se recomienda.|
|\[Versión preliminar]: Supervisión de la base de datos SQL no auditada en Azure Security Center|Azure Security Center supervisa las bases de datos y los servidores SQL Server que no tengan la auditoría de SQL activada tal como se recomienda.|
|\[Versión preliminar]: Se deben instalar actualizaciones del sistema en las máquinas|Azure Security Center supervisa las actualizaciones del sistema de seguridad que faltan en los servidores tal como se recomienda.|
|\[Versión preliminar]: Auditoría de la falta del cifrado de blob de las cuentas de almacenamiento|Se auditan las cuentas de almacenamiento que no usan el cifrado de blobs. Solo se aplica a los tipos de recursos de almacenamiento de Microsoft, no a los de otros proveedores. Azure Security Center supervisa el posible acceso de red Just-In-Time tal como se recomienda.|
|\[Versión preliminar]: El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales|Azure Security Center supervisa el posible acceso de red Just-In-Time tal como se recomienda.|
|\[Versión preliminar]: Deben habilitarse los controles de aplicaciones adaptables en las máquinas virtuales|Azure Security Center supervisa la posible configuración de la inclusión de aplicaciones en la lista de permitidos.|
|\[Versión preliminar]: Se debe configurar los grupos de seguridad de red que faltan para las máquinas virtuales|Azure Security Center supervisa los grupos de seguridad de red con reglas demasiado permisivas, tal como se recomienda.|
|\[Versión preliminar]: Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas|Azure Security Center supervisa los servidores que no cumplen la línea de base configurada tal como se recomienda.| 
|\[Versión preliminar]: Endpoint Protection debe instalarse en las máquinas virtuales|Azure Security Center supervisa los servidores que no tienen instalado un agente de Microsoft System Center Endpoint Protection tal como se recomienda.|
|\[Versión preliminar]: Se debe aplicar el cifrado de discos en las máquinas virtuales|Azure Security Center supervisa las máquinas virtuales que no tienen habilitado el cifrado de disco tal como se recomienda.|
|\[Versión preliminar]: Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades|Supervise las vulnerabilidades detectadas por la solución de evaluación de vulnerabilidades y las máquinas virtuales sin esta solución en Azure Security Center tal como se recomienda.|
|\[Versión preliminar]: Supervisión de la aplicación web desprotegida en Azure Security Center|Azure Security Center supervisa las aplicaciones web que carecen de la protección de firewall de aplicaciones web tal como se recomienda.|
|\[Versión preliminar]: La solución Endpoint Protection debe instalarse en las máquinas virtuales|Azure Security Center supervisa los puntos de conexión de red que no tienen la protección de firewall de próxima generación tal como se recomienda.|
|\[Versión preliminar]: Se deben corregir las vulnerabilidades en las bases de datos SQL|Supervise los resultados del análisis de evaluación de vulnerabilidades y las recomendaciones sobre cómo solucionar las de las bases de datos.|
|\[Versión preliminar]: Se debe designar un máximo de tres propietarios para la suscripción|Se recomienda que designe a un máximo de tres propietarios de la suscripción para reducir el riesgo de vulneración por parte de un propietario en peligro.|
|\[Versión preliminar]: Debe haber más de un propietario asignado a su suscripción|Se recomienda que designe a más de un propietario de la suscripción para asegurarse la redundancia del acceso de administrador.|
|\[Versión preliminar]: MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción |Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de propietario, a fin de evitar una brecha de seguridad en las cuentas o los recursos.|
|\[Versión preliminar]: MFA debe estar habilitada en las cuentas de la suscripción con permisos de escritura|Multi-Factor Authentication debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de escritura, a fin de evitar una brecha de seguridad en las cuentas o los recursos.|
|\[Versión preliminar]: MFA debe estar habilitada en las cuentas de la suscripción con permisos de lectura|Multi-Factor Authentication debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de lectura, a fin de evitar una brecha de seguridad en las cuentas o los recursos.|
|\[Versión preliminar]: Se deben quitar de la suscripción las cuentas en desuso con permisos de propietario|Se deben quitar de la suscripción las cuentas en desuso con permisos de propietario. Se ha bloqueado el inicio de sesión en las cuentas en desuso.|
|\[Versión preliminar]: Las cuentas en desuso deben eliminarse de la suscripción|Convendría eliminar las cuentas en desuso de las suscripciones. Se ha bloqueado el inicio de sesión en las cuentas en desuso.|
|\[Versión preliminar]: Las cuentas externas con permisos de propietario deben quitarse de la suscripción|Las cuentas externas con permisos de propietario deben quitarse de la suscripción a fin de evitar el acceso a los permisos.|
|\[Versión preliminar]: Las cuentas externas con permisos de escritura deben quitarse de la suscripción|Las cuentas externas con permisos de escritura deben quitarse de la suscripción a fin de evitar el acceso no supervisado.|
|\[Versión preliminar]: Las cuentas externas con permisos de lectura deben quitarse de la suscripción|Las cuentas externas con permisos de lectura deben quitarse de la suscripción a fin de evitar el acceso no supervisado.|




## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a configurar directivas de seguridad en Security Center. Para más información sobre Security Center, consulte los siguientes artículos:

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): Aprenda a planear y entender las consideraciones de diseño en Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
* [Administración y respuesta a alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): Aprenda a administrar y responder a las alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): Aprenda cómo supervisar el estado de mantenimiento de las soluciones de sus asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): Obtenga respuestas para las preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.

Para más información acerca de Azure Policy, consulte [¿Qué es Azure Policy?](../governance/policy/overview.md).
