---
title: Caja de seguridad de cliente de Microsoft Azure
description: Introducción técnica de caja de seguridad de cliente de Microsoft Azure, que proporciona control sobre el acceso de proveedor en la nube cuando Microsoft necesite tener acceso a los datos del cliente.
author: cabailey
ms.service: security
ms.topic: article
ms.author: cabailey
manager: barbkess
ms.date: 05/07/2019
ms.openlocfilehash: 468e392cd2c45d79cbb24f8d737a6e83fbcd2725
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079276"
---
# <a name="customer-lockbox-for-microsoft-azure"></a>Caja de seguridad de cliente de Microsoft Azure

> [!NOTE]
> Para usar esta característica, su organización debe tener un [plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/) con un nivel mínimo de **Developer**.

Caja de seguridad de cliente de Microsoft Azure proporciona una interfaz para los clientes a revisar y aprobar o rechazar las solicitudes de acceso de datos de cliente. Se utiliza en casos donde un ingeniero de Microsoft necesita tener acceso a los datos del cliente durante una solicitud de soporte técnico.

En este artículo trata cómo las solicitudes de caja de seguridad de cliente son iniciado por, realiza un seguimiento y almacenadas para las auditorías y las revisiones más adelante.

Caja de seguridad de cliente ya está disponible con carácter general y actualmente está habilitado para acceso de escritorio remoto a máquinas virtuales.

## <a name="workflow"></a>Flujo de trabajo

Los siguientes pasos describen un flujo de trabajo típico para una solicitud de caja de seguridad de cliente.

1. Un usuario en una organización tiene un problema con su carga de trabajo de Azure.

2. Después de que esta persona soluciona el problema, pero no puede corregirlo, abran una incidencia de soporte técnico desde el [portal Azure](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac). El vale se asigna a un ingeniero de soporte técnico al cliente de Azure.

3. Un ingeniero de soporte técnico de Azure revisa la solicitud de servicio y determina los pasos siguientes para resolver el problema.

4. Si el ingeniero de soporte técnico no puede solucionar el problema mediante el uso de telemetría y las herramientas estándar, el siguiente paso es solicitar permisos con privilegios elevados mediante el uso de un servicio de acceso Just In Time (JIT). Esta solicitud puede ser desde el ingeniero de soporte técnico original. O bien, puede ser de un ingeniero de diferentes porque el problema se remite al equipo de DevOps de Azure.

5. Después el acceso de solicitud se envía el técnico de Azure, Just-In-Time service evalúa la solicitud teniendo en cuenta factores tales como:
    - El ámbito del recurso
    - Si el solicitante es una identidad aislada o mediante la autenticación multifactor
    - Niveles de permisos
    
    Según la regla JIT, esta solicitud puede incluir también una aprobación de aprobadores interno de Microsoft. Por ejemplo, el aprobador puede ser el responsable de soporte técnico al cliente o el director de DevOps.

6. Cuando la solicitud requiere acceso directo a los datos del cliente, se inicia una solicitud de caja de seguridad de cliente. Por ejemplo, acceso de escritorio remoto para máquina virtual de un cliente.
    
    La solicitud está ahora en un **una notificación de cliente** estado, esperando la aprobación del cliente antes de conceder acceso.

7. En la organización del cliente, el usuario que tiene el [rol propietario](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-rbac-roles) para Azure suscripción recibe un correo electrónico de Microsoft, para informarles sobre la solicitud de acceso pendientes. Para las solicitudes de caja de seguridad de cliente, esta persona es el aprobador designado.
    
    Correo electrónico de ejemplo:
    
    ![Caja de seguridad de cliente Azure - notificación por correo electrónico](./media/azure-customer-lockbox/customer-lockbox-email-notification.png)

8. La notificación por correo electrónico proporciona un vínculo a la **caja de seguridad de cliente** hoja en el portal de Azure. Mediante este vínculo, el aprobador designado inicia sesión en el portal de Azure para verlas solicitudes de su organización tiene para la caja de seguridad de cliente pendientes:
    
    ![Caja de seguridad de cliente Azure - página de aterrizaje](./media/azure-customer-lockbox/customer-lockbox-landing-page.png)
    
   La solicitud permanece en la cola de cliente durante cuatro días. Transcurrido ese tiempo, la solicitud de acceso expira automáticamente y no se concede acceso a los ingenieros de Microsoft.

9. Para obtener los detalles de la solicitud pendiente, el aprobador designado puede seleccionar la solicitud de caja de seguridad de **solicitudes pendientes**:
    
    ![Caja de seguridad de cliente Azure - ver la solicitud pendiente](./media/azure-customer-lockbox/customer-lockbox-pending-requests.png)

10. También puede seleccionar el aprobador designado la **Id. de solicitud de servicio** para ver la solicitud de vale de soporte técnico que se creó el usuario original. Esta información proporciona el contexto de por qué se activa Microsoft Support y el historial del problema notificado. Por ejemplo: 
    
    ![Caja de seguridad de cliente Azure - ver la solicitud de vale de soporte técnico](./media/azure-customer-lockbox/customer-lockbox-support-ticket.png)

11. Después de revisar la solicitud, se selecciona el aprobador designado **aprobar** o **Deny**:
    
    ![Liquidación de cliente Azure - seleccione aprobar o denegar](./media/azure-customer-lockbox/customer-lockbox-approval.png)
    
    Como resultado de la selección:
    - **Aprobar**:  Se concede acceso al ingeniero de Microsoft. Se concede el acceso para un período predeterminado de ocho horas.
    - **Denegar**: Se rechaza la solicitud de acceso con privilegios elevados mediante el ingeniero de Microsoft y se realiza ninguna otra acción.

Con fines de auditoría, se registran las acciones realizadas en este flujo de trabajo en [los registros de solicitudes de caja de seguridad de cliente](#auditing-logs).

## <a name="auditing-logs"></a>Registros de auditoría

Registros de caja de seguridad de cliente se almacenan en los registros de actividad. En el portal de Azure, seleccione **los registros de actividad** para ver información de auditoría relacionados con las solicitudes de caja de seguridad de cliente. Puede filtrar para acciones específicas, tales como:
- **Denegar la solicitud de caja de seguridad**
- **Crear solicitud de caja de seguridad**
- **Aprobar solicitud de caja de seguridad**
- **Expiración de solicitud de caja de seguridad**

Por ejemplo:

![Caja de seguridad de cliente Azure - registros de actividad](./media/azure-customer-lockbox/customer-lockbox-activitylogs.png)

## <a name="supported-services-and-scenarios"></a>Escenarios y servicios admitidos

Los siguientes servicios y escenarios son actualmente en disponibilidad general para la caja de seguridad de cliente.

### <a name="remote-desktop-access-to-virtual-machines"></a>Acceso a Escritorio remoto a máquinas virtuales

Caja de seguridad de cliente está habilitada actualmente para las solicitudes de acceso de escritorio remoto a máquinas virtuales. Se admiten las cargas de trabajo siguientes:
- Plataforma como servicio (PaaS): versión 1
- Infraestructura como servicio (IaaS) - Windows y Linux (solo Azure Resource Manager)
- Conjunto de escalado de máquinas virtuales: Windows y Linux

> [!NOTE]
> No se admiten las instancias de la IaaS clásica mediante la caja de seguridad de cliente. Si tiene cargas de trabajo que se ejecutan en instancias de la IaaS clásica, se recomienda que migrar del modelo clásico a modelos de implementación de Resource Manager. Para obtener instrucciones, consulte [migración compatible con la plataforma de recursos de IaaS del modelo clásico a Azure Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

#### <a name="detailed-audit-logs"></a>Registros de auditoría detallados

Para escenarios que implican acceso a Escritorio remoto, puede usar los registros de eventos de Windows para revisar las acciones realizadas por el ingeniero de Microsoft. Considere el uso de Azure Security Center para recopilar los registros de eventos y copiar los datos en el área de trabajo para el análisis. Para obtener más información, consulte [recopilación de datos en Azure Security Center](../security-center/security-center-enable-data-collection.md).

## <a name="exclusions"></a>Exclusiones

No se desencadenan las solicitudes de caja de seguridad de cliente en los siguientes escenarios de soporte técnico de ingeniería:

- Un ingeniero de Microsoft necesita realizar una actividad que se encuentre fuera de los procedimientos operativos estándar. Por ejemplo, para recuperar o restaurar los servicios en escenarios impredecibles o inesperados.

- Un ingeniero de Microsoft tiene acceso a la plataforma Windows Azure como parte de la solución de problemas y sin darse cuenta tiene acceso a los datos del cliente. Por ejemplo, el equipo de red de Azure realiza la solución de problemas que da como resultado una captura de paquetes en un dispositivo de red. Sin embargo, si el cliente cifra los datos mientras estaba en tránsito, el ingeniero no puede leer los datos.

## <a name="next-steps"></a>Pasos siguientes

Caja de seguridad de cliente está disponible automáticamente para todos los clientes que tienen un [plan de soporte técnico de Azure](https://azure.microsoft.com/support/plans/) con un nivel mínimo de **Developer**.

Cuando tenga un plan de soporte elegibles, se requiere ninguna acción por el usuario para habilitar la caja de seguridad de cliente. Las solicitudes de caja de seguridad de cliente iniciadas por un ingeniero de Microsoft si esta acción es necesaria para avanzar una incidencia de soporte técnico que se archiva de alguien de su organización.
