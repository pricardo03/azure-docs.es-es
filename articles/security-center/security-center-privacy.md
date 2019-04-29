---
title: Administración de datos de usuario en Azure Security Center | Microsoft Docs
description: " Obtenga información sobre cómo administrar los datos de usuario en Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: rkarlin
ms.openlocfilehash: fcec410df631a58b76878a4cb327ca2fb04a2105
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60703479"
---
# <a name="manage-user-data-in-azure-security-center"></a>Administrar datos de usuario en Azure Security Center
En este artículo se proporciona información sobre cómo administrar los datos de usuario en Azure Security Center. La administración de los datos de usuario incluye la capacidad de acceder, eliminar o exportar datos.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede tener acceso a los datos del cliente dentro de la herramienta. Vea [Roles integrados para el control de acceso basado en roles de Azure](../role-based-access-control/built-in-roles.md) para obtener más información sobre los roles Lector, Propietario y Colaborador. Vea [Administradores de la suscripción de Azure](../billing/billing-add-change-azure-subscription-administrator.md) para obtener más información sobre el rol Administrador de cuenta.

## <a name="searching-for-and-identifying-personal-data"></a>Búsqueda e identificación de datos personales
Un usuario de Security Center puede ver sus datos personales a través de Azure Portal. En Security Center solo se almacenan los detalles de contacto de seguridad como direcciones de correo electrónico y números de teléfono. Vea [Provisión de detalles de contacto de seguridad en Azure Security Center](security-center-provide-security-contact-details.md) para obtener más información.

En Azure Portal, un usuario puede ver las configuraciones de IP permitidas mediante el la característica de acceso Just-In-Time a la máquina virtual de Security Center. Para más información, consulte [Administración del acceso a máquina virtual mediante Just-In-Time](security-center-just-in-time.md).

En Azure Portal, un usuario puede ver las alertas de seguridad proporcionadas por Security Center, incluidos los detalles de atacantes y direcciones IP. Vea [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) para obtener más información.

## <a name="classifying-personal-data"></a>Clasificación de datos personales
No es necesario clasificar los datos personales que se encuentran en la característica de contactos de seguridad de Security Center. Los datos guardados son una dirección de correo electrónico (o varias) y un número de teléfono. Security Center valida los [datos de contacto](security-center-provide-security-contact-details.md).

No es necesario clasificar las direcciones IP y los números de puerto guardados por la característica [Just-In-Time](security-center-just-in-time.md) de Security Center.

Solo un usuario asignado al rol de Administrador puede clasificar los datos personales mediante la [visualización de alertas](security-center-managing-and-responding-alerts.md) en Security Center.

## <a name="securing-and-controlling-access-to-personal-data"></a>Protección y control del acceso a los datos personales
Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede tener acceso a los [datos de contacto de seguridad](security-center-provide-security-contact-details.md).

Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede tener acceso a las directivas [Just-In-Time](security-center-just-in-time.md).

Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede ver las [alertas](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Actualización de datos personales
Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede actualizar los [datos de contacto de seguridad](security-center-provide-security-contact-details.md) a través de Azure Portal.

Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede actualizar las [directivas Just-In-Time](security-center-just-in-time.md).

Un Administrador de cuenta no puede editar incidentes de alertas. Un [incidente de alerta](security-center-managing-and-responding-alerts.md) se considera datos de seguridad y es de solo lectura.

## <a name="deleting-personal-data"></a>Eliminación de datos personales
Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede eliminar los [datos de contacto de seguridad](security-center-provide-security-contact-details.md) a través de Azure Portal.

Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede eliminar las [directivas Just-In-Time](security-center-just-in-time.md) a través de Azure Portal.

Un usuario de Security Center no puede eliminar los incidentes de alerta. Debido a las necesidades de seguridad, un [incidente de alerta](security-center-managing-and-responding-alerts.md) se considera datos de solo lectura.

## <a name="exporting-personal-data"></a>Exportación de datos personales
Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede exportar los [datos de contacto de seguridad](security-center-provide-security-contact-details.md) si:

- Realiza una copia desde Azure Portal.
- Ejecuta la llamada de API de REST de Azure, GET HTTP:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
  ```

Un usuario de Security Center que tenga asignado el rol Administrador de cuenta puede exportar las [directivas Just-In-Time](security-center-just-in-time.md) que contienen las direcciones IP si:

- Realiza una copia desde Azure Portal.
- Ejecuta la llamada de API de REST de Azure, GET HTTP:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
  ```

Un Administrador de cuenta puede exportar los detalles de alerta si:

- Realiza una copia desde Azure Portal.
- Ejecuta la llamada de API de REST de Azure, GET HTTP:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
  ```

Vea [Get Security Alerts (GET Collection)](https://msdn.microsoft.com/library/mt704050.aspx) (Obtener alertas de seguridad [colección GET]) para obtener más información.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Restricción del uso de datos personales para la generación de perfiles o marketing sin consentimiento
Un usuario de Security Center puede elegir no participar mediante la eliminación de sus [datos de contacto de seguridad](security-center-provide-security-contact-details.md).

Los [datos Just-In-Time](security-center-just-in-time.md) se consideran datos no identificables y se conservan durante un período de 30 días.

Los [datos de alertas](security-center-managing-and-responding-alerts.md) se consideran datos de seguridad y se conservan durante un período de dos años.

## <a name="auditing-and-reporting"></a>Informes y auditoría
Los registros de auditoría de contacto de seguridad, Just-In-Time y las actualizaciones de alertas se mantienen en [los registros de actividad de Azure](../azure-monitor/platform/activity-logs-overview.md).

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre la administración de datos de usuario, vea [Manage user data found in an Azure Security Center investigation](security-center-investigation-user-data.md) (Administración de los datos de usuario encontrados en una investigación de Azure Security Center).
