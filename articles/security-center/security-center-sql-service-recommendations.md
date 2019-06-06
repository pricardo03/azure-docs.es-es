---
title: Protección de los datos y el servicio de SQL de Azure en Azure Security Center | Microsoft Docs
description: En este documento se explica cómo las recomendaciones de Azure Security Center ayudan a proteger el servicio SQL de Azure y los datos y a cumplir las directivas de seguridad.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2019
ms.author: monhaber
ms.openlocfilehash: bbba5f380fddb4fdec43a7414e59778135c4e0ef
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428302"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Protección del servicio SQL de Azure en Azure Security Center
El Centro de seguridad de Azure analiza el estado de seguridad de los recursos de Azure. Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios.  Las recomendaciones se aplican a los tipos de recursos de Azure: máquinas virtuales, redes, SQL y datos y aplicaciones.


### <a name="monitor-data-security"></a>Supervisión de seguridad de datos

Al hacer clic en **Seguridad de los datos** en la sección **Prevención**, se abre la hoja **Recursos de datos** con las recomendaciones para SQL y Storage. También tiene [recomendaciones](security-center-sql-service-recommendations.md) sobre el estado general de la base de datos. Para más información acerca del cifrado de almacenamiento, consulte [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Habilitación del cifrado para la cuenta de almacenamiento de Azure en Azure Security Center).

![Recursos de datos](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

En **SQL Recommendations** (Recomendaciones de SQL) puede hacer clic en cualquier recomendación y obtener más detalles acerca de cómo realizar más acciones para resolver un problema. El ejemplo siguiente muestra la expansión de la recomendación **Auditoría de la base de datos y detección de amenazas en bases de datos de SQL**.

![Detalles acerca de una recomendación de SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

La hoja **Habilitar la auditoría y la detección de amenazas en bases de datos de SQL** contiene la siguiente información:

* Una lista de bases de datos SQL.
* El servidor en el que se encuentran.
* Información sobre si esta configuración se heredó del servidor o si es única en la base de datos.
* El estado actual.
* La gravedad del problema.

Al hacer clic en la base de datos para abordar esta recomendación, se abre la hoja **Auditoría y detección de amenazas**, como se muestra en la siguiente pantalla.

![Auditoría y detección de amenazas](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Para habilitar la auditoría, seleccione **Activar** en la opción **Auditoría**.

## <a name="data-and-storage-recommendations"></a>Recomendaciones de disco y almacenamiento

|Tipo de recurso|Puntuación segura|Recomendación|DESCRIPCIÓN|
|----|----|----|----|
|Cuenta de almacenamiento|20|Debe habilitarse la transferencia segura a las cuentas de almacenamiento|La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|Redis|20|Deben habilitarse solo conexiones seguras con Redis Cache|Habilitación solo de conexiones a Azure Cache for Redis a través de SSL. El uso de conexiones seguras garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|SQL|15|Debe habilitarse el cifrado de datos transparente en bases de datos SQL|Habilite el cifrado de datos transparente para proteger los datos en reposo y satisfacer los requisitos de cumplimiento.|
|SQL|15|Se debe habilitar la auditoría de SQL server|Habilite la auditoría de servidores SQL de Azure. (solo el servicio SQL de Azure. No incluye los servidores SQL que se ejecutan en las máquinas virtuales).|
|Data Lake Analytics|5|Se deben habilitar los registros de diagnóstico en Data Lake Analytics|Habilite los registros y consérvelos durante un período de hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Data Lake Store|5|Se deben habilitar los registros de diagnóstico en Azure Data Lake Store|Habilite los registros y consérvelos durante un período de hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|SQL|30|Vulnerabilidades en las bases de datos SQL deben corregirse.|La evaluación de vulnerabilidades de SQL examina la base de datos en busca de vulnerabilidades de seguridad y expone las posibles desviaciones de los procedimientos recomendados, como errores de configuración, permisos excesivos y datos confidenciales sin protección. La corrección de las vulnerabilidades encontradas puede mejorar considerablemente el estado de seguridad de la base de datos.|
|SQL|20|Aprovisionar un administrador de Azure AD para SQL Server|Aprovisione un administrador de Azure AD para SQL Server a fin de habilitar la autenticación de Azure AD. La autenticación de Azure AD permite la administración simplificada de permisos y la administración centralizada de identidades de usuarios de base de datos y otros servicios de Microsoft.|
|Cuenta de almacenamiento|15|Se debe restringir el acceso a las cuentas de almacenamiento con firewall y las configuraciones de red virtual|Audite el acceso de red no restringido en la configuración de firewall de su cuenta de almacenamiento. Si se configuran reglas de red, solo las aplicaciones de redes permitidas pueden acceder a la cuenta de almacenamiento. Para permitir conexiones desde Internet específico o los clientes locales, puede conceder acceso al tráfico de redes virtuales de Azure específicos o a intervalos de direcciones IP de Internet públicos.|
|Cuenta de almacenamiento|1|Se deben migrar las cuentas de almacenamiento a los nuevos recursos de Azure Resource Manager|Usar nueva v2 de Azure Resource Manager para las cuentas de almacenamiento para proporcionar mejoras de seguridad, como: control de acceso más fuerte (RBAC), auditoría mejor, implementación basada en Resource Manager y gobierno, el acceso a las identidades administradas, acceso al almacén de claves para los secretos y autenticación basada en AD Azure y compatibilidad con etiquetas de recursos y grupos para facilitar la administración de seguridad.|

## <a name="see-also"></a>Vea también
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Protección de las máquinas virtuales en Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Protección de las aplicaciones en Azure Security Center](security-center-application-recommendations.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
