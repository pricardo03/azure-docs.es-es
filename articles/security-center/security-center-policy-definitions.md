---
title: Las definiciones de directiva de Azure supervisadas en Azure Security Center | Microsoft Docs
description: Las definiciones de directiva de Azure supervisadas en Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: rkarlin
ms.openlocfilehash: e76464e294a3378bf3a275ec4fe7ccbd87c09475
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011647"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>Directivas de Azure supervisadas en Azure Security Center
Este artículo proporciona una lista de definiciones de Azure Policy que puede supervisarse en Security Center.

## <a name="available-security-policies"></a>Directivas de seguridad disponibles

Para comprender las directivas integradas que se supervisan mediante Security Center, consulte la tabla siguiente:

| Directiva | Funcionamiento de la directiva |
| --- | --- |
|Habilitación de auditoría de registros de diagnóstico en Service Fabric y Virtual Machine Scale Sets|Se recomienda habilitar los registros para que ese seguimiento de actividad se pueda volver a crear cuando se necesiten investigaciones en caso de incidente o riesgo.|
|Auditar las reglas de autorización en espacios de nombres del centro de eventos|Los clientes del Centro de eventos no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso a nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Auditar la existencia de reglas de autorización en entidades del centro de eventos|Permite auditar la existencia de reglas de autorización en entidades de Event Hub para conceder acceso con privilegios mínimos.|
|Auditar el acceso de red sin restricciones a cuentas de almacenamiento|Audite el acceso de red no restringido en la configuración de firewall de su cuenta de almacenamiento. Si se configuran reglas de red, solo las aplicaciones de redes permitidas pueden acceder a la cuenta de almacenamiento. Para permitir conexiones desde clientes específicos locales o de Internet, se puede conceder acceso al tráfico procedente de redes virtuales de Azure específicas o a intervalos de direcciones IP de Internet públicas.|
|Auditar el uso de reglas de RBAC personalizadas|Permite auditar roles integrados, como "propietario, colaborador, lector" en lugar de roles RBAC personalizados, que son propensos a errores de auditoría. El uso de roles personalizados se trata como una excepción y requiere una revisión rigurosa y el modelado de amenazas.|
|Auditar la habilitación de registros de diagnóstico en Azure Stream Analytics|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Auditar la transferencia segura a cuentas de almacenamiento|Permite auditar el requisito de transferencia segura en la cuenta de almacenamiento. La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|Auditar el aprovisionamiento de un administrador de Azure Active Directory para SQL Server|Permite aprovisionar un administrador de Azure Active Directory para SQL Server a fin de habilitar la autenticación de Azure AD. La autenticación de Azure AD permite la administración simplificada de permisos y la administración centralizada de identidades de usuarios de base de datos y otros servicios de Microsoft.|
|Auditar las reglas de autorización en espacios de nombres de Service Bus|Los clientes de Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso a nivel de entidad para que las colas y los temas proporcionen acceso solo a la entidad específica.|
|Auditar la habilitación de registros de diagnóstico en Service Bus|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Auditar la configuración de la propiedad ClusterProtectionLevel como EncryptAndSign en Service Fabric|Service Fabric proporciona tres niveles de protección (None, Sign y EncryptAndSign) para la comunicación de nodo a nodo mediante un certificado de clúster principal. Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente.| 
|Auditar el uso de Azure Active Directory para la autenticación de clientes en Service Fabric|Permite auditar el uso de la autenticación de clientes solo mediante Azure Active Directory en Service Fabric| 
|Auditar la habilitación de registros de diagnóstico para el servicio Search|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar la habilitación solo de conexiones seguras a Azure Cache for Redis|Permite auditar la habilitación solo de conexiones a Azure Cache for Redis a través de SSL. El uso de conexiones seguras garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión| 
|Auditar la habilitación de registros de diagnóstico en Logic Apps|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar la habilitación de registros de diagnóstico en Key Vault|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.|
|Auditar la habilitación de registros de diagnóstico en el centro de eventos|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar la habilitación de registros de diagnóstico en Azure Data Lake Store|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar la habilitación de registros de diagnóstico en Data Lake Analytics|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar el uso de las cuentas de almacenamiento clásicas|Use Azure Resource Manager para las cuentas de almacenamiento a fin de proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobierno e implementación basados en Azure Resource Manager, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de seguridad.| 
|Auditar el uso de máquinas virtuales clásicas|Use Azure Resource Manager para las máquinas virtuales a fin de proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobierno e implementación basados en Azure Resource Manager, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de seguridad.| 
|Auditar la configuración de reglas de alertas de métricas en cuentas de Batch|Permite auditar la configuración de reglas de alertas de métricas en una cuenta de Batch para habilitar la métrica requerida.| 
|Auditar la configuración de reglas de alertas de métricas en cuentas de Batch|Permite auditar la configuración de reglas de alertas de métricas en una cuenta de Batch para habilitar la métrica requerida.| 
|Auditar la habilitación de registros de diagnóstico en cuentas de Batch|Permite auditar la habilitación de los registros y conservarlos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar la habilitación del cifrado de variables de cuentas de Automation|Es importante habilitar el cifrado de recursos de variables de cuentas de Automation al almacenar datos confidenciales.| 
|Auditar la habilitación de registros de diagnóstico en App Services|Permite auditar la habilitación de registros de diagnóstico en la aplicación. Esto le permite volver a crear seguimientos de actividad con fines de investigación si se produce un incidente de seguridad o se pone en peligro la red.| 
|Auditar el estado del cifrado de datos transparente|Estado de cifrado de datos transparente de auditoría para bases de datos SQL.| 
|Auditar configuración de auditoría de SQL en el nivel de servidor|Audita la existencia de la auditoría de SQL en el nivel de servidor.| 
|[Versión preliminar]: Supervisión de la base de datos SQL sin cifrar en Azure Security Center|Azure Security Center supervisará las bases de datos o los servidores SQL Server sin cifrar como recomendaciones.| 
|[Versión preliminar]: Supervisión de la base de datos SQL no auditada en Azure Security Center|Azure Security Center supervisará las bases de datos y los servidores SQL Server que no tengan la auditoría de SQL activada como recomendaciones.| 
|[Versión preliminar]: Supervisión de las actualizaciones del sistema que faltan en Azure Security Center|Azure Security Center supervisará las actualizaciones del sistema de seguridad que faltan en los servidores como recomendaciones.| 
|[Versión preliminar]: Auditoría de la falta del cifrado de blob de las cuentas de almacenamiento|Esta directiva audita las cuentas de almacenamiento sin cifrado de blobs. Solo se aplica a los tipos de recursos de Microsoft.Storage, no de otros proveedores de almacenamiento. Azure Security Center supervisará el posible acceso de red Just-In-Time como recomendaciones.| 
|[Versión preliminar]: Supervisión del posible acceso de red Just-In-Time (JIT) en Azure Security Center|Azure Security Center supervisará el posible acceso de red Just-In-Time (JIT) como recomendaciones.| 
|[Versión preliminar]: Supervisión de una posible inclusión de aplicaciones en la lista de permitidos en Azure Security Center|Azure Security Center supervisará la posible inclusión de aplicaciones en la lista de permitidos.| 
|[Versión preliminar]: Supervisión del acceso de red permisivo en Azure Security Center|Azure Security Center supervisará los grupos de seguridad de red con reglas demasiado permisivas como recomendaciones.| 
|[Versión preliminar]: Supervisión de los puntos vulnerables del sistema operativo en Azure Security Center|Azure Security Center supervisará los servidores que no cumplan la base de referencia configurada como recomendaciones.| 
|[Versión preliminar]: Supervisión de la falta de Endpoint Protection en Azure Security Center|Azure Security Center supervisará los servidores sin un agente de Endpoint Protection instalado como recomendaciones.| 
|[Versión preliminar]: Supervisión de discos de máquinas virtuales sin cifrar en Azure Security Center|Azure Security Center supervisará las VM sin el cifrado de disco habilitado como recomendaciones.| 
|[Versión preliminar]: Supervisión de los puntos vulnerables de máquinas virtuales en Azure Security Center|Supervisa los puntos vulnerables detectados por la solución de evaluación de puntos vulnerables y las máquinas virtuales sin esta solución en Azure Security Center como recomendaciones.| 
|[Versión preliminar]: Supervisión de la aplicación web desprotegida en Azure Security Center|Azure Security Center supervisará las aplicaciones web sin protección de firewall de aplicaciones web como recomendaciones.| 
|[Versión preliminar]: Supervisión de puntos de conexión de red desprotegidos en Azure Security Center|Azure Security Center supervisará los puntos de conexión de red sin protección del firewall de última generación como recomendaciones.| 
|[Versión preliminar]: Supervisión de los resultados de evaluación de puntos vulnerables de SQL en Azure Security Center|Supervise los resultados del análisis de evaluación de puntos vulnerables y las recomendaciones para solucionar los de las bases de datos.| 
|[Versión preliminar]: Auditar el número máximo de propietarios de una suscripción|Se recomienda que designe a un máximo de tres propietarios de suscripción para reducir el riesgo de una brecha de seguridad por parte de un propietario en peligro.| 
|[Versión preliminar]: Auditar el número mínimo de propietarios de una suscripción|Se recomienda que designe a más de un propietario de la suscripción para tener redundancia de acceso de administrador.| 
|[Versión preliminar]: Auditar las cuentas con permisos de propietario que no tengan MFA habilitada en una suscripción|Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de propietario, a fin de evitar una brecha de seguridad en las cuentas o los recursos.| 
|[Versión preliminar]: Auditar las cuentas con permisos de escritura que no tengan MFA habilitada en una suscripción|Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de escritura, a fin de evitar una brecha de seguridad en las cuentas o los recursos.| 
|[Versión preliminar]: Auditar las cuentas con permisos de lectura que no tengan MFA habilitada en una suscripción|Multi-Factor Authentication (MFA) debe estar habilitada para todas las cuentas de la suscripción que tengan permisos de lectura, a fin de evitar una brecha de seguridad en las cuentas o los recursos.| 
|[Versión preliminar]: Auditar las cuentas en desuso de una suscripción que tengan permisos de propietario|Quitar de la suscripción las cuentas en desuso con permisos de propietario Las cuentas en desuso son cuentas en las que se ha bloqueado el inicio de sesión.| 
|[Versión preliminar]: Auditar las cuentas en desuso de una suscripción|Convendría eliminar las cuentas en desuso de las suscripciones. Las cuentas en desuso son cuentas en las que se ha bloqueado el inicio de sesión.| 
|[Versión preliminar]: Auditar las cuentas externas de una suscripción que tengan permisos de propietario|Las cuentas externas con permisos de propietario deben quitarse de la suscripción a fin de evitar el acceso no supervisado.| 
|[Versión preliminar]: Auditar las cuentas externas de una suscripción que tengan permisos de escritura|Las cuentas externas con privilegios de escritura deben quitarse de la suscripción a fin de evitar el acceso no supervisado.| 
|[Versión preliminar]: Auditar las cuentas externas de una suscripción que tengan permisos de lectura|Las cuentas externas con privilegios de lectura deben quitarse de la suscripción a fin de evitar el acceso no supervisado.| 




## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a configurar directivas de seguridad en Security Center. Para más información sobre Security Center, consulte los siguientes artículos:

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): Aprenda a planificar y entender las consideraciones de diseño sobre Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
* [Administración y respuesta a alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): Aprenda a administrar y responder a las alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): Aprenda cómo supervisar el estado de mantenimiento de las soluciones de sus asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): Obtenga respuestas para las preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/): Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.

Para más información acerca de Azure Policy, consulte [¿Qué es Azure Policy?](../azure-policy/azure-policy-introduction.md)
