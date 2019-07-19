---
title: Protección de los servicios de datos y almacenamiento de Azure en Azure Security Center | Microsoft Docs
description: En este documento se explica cómo las recomendaciones de Azure Security Center ayudan a proteger el servicio SQL de Azure y los datos y a cumplir las directivas de seguridad.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2019
ms.author: monhaber
ms.openlocfilehash: 2ac0e4ebaafb8b0c9c79e885cecbefc5a65c1823
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275335"
---
# <a name="protect-azure-data-and-storage-services-in-azure-security-center"></a>Protección de los servicios de datos y almacenamiento de Azure en Azure Security Center
En este tema se muestra cómo ver e implementar recomendaciones de seguridad para los recursos de datos y almacenamiento. Azure Security Center encontró estas recomendaciones al analizar el estado de seguridad de los recursos de Azure.

## <a name="view-your-data-security-information"></a>Visualización de la información de seguridad de los datos

1. En la sección **Resource security hygiene** (Protección de seguridad de recursos), seleccione **Data and storage resources** (Recursos de datos y almacenamiento).

   ![Recursos de datos y almacenamiento](./media/security-center-monitoring/click-data.png)

    Se abre la página **Data security** (Seguridad de los datos) con recomendaciones para los recursos de datos.

     ![Recursos de datos](./media/security-center-monitoring/sql-overview.png)

En esta página puede realizar las siguientes acciones:

* Haga clic en la pestaña **Overview** (Información general) para mostrar todas las recomendaciones de recursos de datos que deben corregirse. 
* Haga clic en cada pestaña y vea las recomendaciones por tipo de recurso.

    > [!NOTE]
    > Para más información acerca del cifrado de almacenamiento, consulte [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Habilitación del cifrado para la cuenta de almacenamiento de Azure en Azure Security Center).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Corrección de una recomendación en un recurso de datos

1. En cualquiera de las pestañas de recursos, haga clic en un recurso. Se abre la página de información con las recomendaciones que deben corregirse.

    ![Información de recursos](./media/security-center-monitoring/sql-recommendations.png)

2. Haga clic en una recomendación. Se abre la página de la recomendación que muestra los **pasos de corrección** para implementar la recomendación.

   ![Pasos de corrección](./media/security-center-monitoring/remediate1.png)

3. Haga clic en **Take action** (Realizar acción). Aparece la página de configuración de recursos.

    ![Habilitación de la recomendación](./media/security-center-monitoring/remediate2.png)

4. Siga los **pasos de corrección** y haga clic en **Save** (Guardar).

## <a name="data-and-storage-recommendations"></a>Recomendaciones de disco y almacenamiento

|Tipo de recurso|Puntuación segura|Recomendación|DESCRIPCIÓN|
|----|----|----|----|
|Cuenta de almacenamiento|20|Se debe permitir la transferencia segura a las cuentas de almacenamiento.|La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de capa de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|Redis|20|Solo se deben permitir las conexiones seguras a Redis Cache|Habilitación solo de conexiones a Azure Cache for Redis a través de SSL. El uso de conexiones seguras garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|
|SQL|15|Se debe permitir el cifrado de datos transparente en bases de datos SQL|Habilite el cifrado de datos transparente para proteger los datos en reposo y satisfacer los requisitos de cumplimiento.|
|SQL|15|Se debe permitir la auditoría de SQL Server|Habilite la auditoría de servidores SQL de Azure. (solo el servicio SQL de Azure. No incluye los servidores SQL que se ejecutan en las máquinas virtuales).|
|Data Lake Analytics|5|Se deben permitir los registros de diagnóstico en Data Lake Analytics|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|Data Lake Store|5|Se deben permitir los registros de diagnóstico en Azure Data Lake Store|Habilite los registros y consérvelos hasta un año. Esto le permite volver a crear seguimientos de actividad con fines de investigación cuando se produce un incidente de seguridad o se pone en peligro la red. |
|SQL|30|Se deben corregir las vulnerabilidades de las bases de datos SQL|La evaluación de vulnerabilidades de SQL examina la base de datos en busca de vulnerabilidades de seguridad y expone las posibles desviaciones de los procedimientos recomendados, como errores de configuración, permisos excesivos y datos confidenciales sin protección. La corrección de las vulnerabilidades encontradas puede mejorar considerablemente el estado de seguridad de la base de datos.|
|SQL|20|Aprovisionar un administrador de Azure AD para SQL Server|Aprovisione un administrador de Azure AD para SQL Server a fin de habilitar la autenticación de Azure AD. La autenticación de Azure AD permite la administración simplificada de permisos y la administración centralizada de identidades de usuarios de base de datos y otros servicios de Microsoft.|
|Cuenta de almacenamiento|15|Se debe restringir el acceso a las cuentas de almacenamiento con configuraciones de firewall y red virtual|Audite el acceso de red no restringido en la configuración de firewall de su cuenta de almacenamiento. Si se configuran reglas de red, solo las aplicaciones de redes permitidas pueden acceder a la cuenta de almacenamiento. Para permitir conexiones desde clientes específicos locales o de Internet, puede conceder acceso al tráfico procedente de redes virtuales de Azure específicas o a intervalos de direcciones IP de Internet públicas.|
|Cuenta de almacenamiento|1|Se deben migrar las cuentas de almacenamiento a los nuevos recursos de Azure Resource Manager|Use el nuevo Azure Resource Manager v2 para las cuentas de almacenamiento a fin de proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobernanza e implementación basados en Resource Manager, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de seguridad.|

## <a name="see-also"></a>Otras referencias
Para más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes temas:

* [Protección de las máquinas virtuales en Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Protección de las aplicaciones en Azure Security Center](security-center-application-recommendations.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)

Para más información sobre Security Center, consulte los siguientes temas:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
