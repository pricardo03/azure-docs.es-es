---
title: Búsqueda de informes de actividad de usuario de Azure Active Directory en Azure Portal | Microsoft Docs
description: Obtenga información sobre dónde están los informes de actividad de usuario de Azure Active Directory en Azure Portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: report-monitor
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 182537d6f07b624f2395f591681ed4596579bde0
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42145904"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Búsqueda de informes de actividad en Azure Portal

En este artículo, se describe cómo buscar informes de actividad de usuario de Azure Active Directory en Azure Portal.

## <a name="activity-and-integrated-app-reports"></a>Informes de actividad y de aplicaciones integradas

En el caso de los informes basados en contexto en Azure Portal, los informes existentes se combinan en una sola vista. Una API subyacente única proporciona los datos a la vista.

Para ver esta vista, vaya a la hoja **Azure Active Directory** y, en **ACTIVIDAD**, seleccione **Registros de auditoría**.

![Registros de auditoría](./media/howto-find-activity-reports/482.png "Registros de auditoría")

Los informes siguientes están consolidados en esta vista:

* Informe de auditoría
* Actividad de restablecimiento de contraseña
* Actividad de registro de restablecimiento de contraseñas
* Actividad de los grupos de autoservicio
* Cambios de nombre de grupo de Office365
* Actividad de aprovisionamiento de cuentas
* Estado de la sustitución de contraseña
* Errores de aprovisionamiento de cuentas


El informe Uso de la aplicación se mejoró y se incluyó en la vista **Inicios de sesión**. Para ver esta vista, vaya a la hoja **Azure Active Directory** y, en **ACTIVIDAD**, seleccione **Inicios de sesión**.

![Vista de inicios de sesión](./media/howto-find-activity-reports/483.png "Vista de inicios de sesión")

La vista **Inicios de sesión** incluye todos los inicios de sesión del usuario. Puede usar esta información para obtener información sobre el uso de las aplicaciones. También puede consultar la información sobre el uso de las aplicaciones en la información general sobre **Aplicaciones empresariales** en la sección **ADMINISTRAR**.

![Aplicaciones empresariales](./media/howto-find-activity-reports/484.png "Aplicaciones empresariales")

## <a name="access-a-specific-report"></a>Acceso a un informe específico

Si bien Azure Portal ofrece una vista única, también se pueden consultar informes específicos.

### <a name="audit-logs"></a>Registros de auditoría

En respuesta a los comentarios de los clientes, en Azure Portal puede usar el filtrado avanzado para tener acceso a los datos que desea. Un filtro que puede usar es una *categoría de actividad*, que muestra los distintos tipos de registros de actividad en Azure AD. Para restringir los resultados a lo que busca, puede seleccionar una categoría.

Por ejemplo, si solo le interesan las actividades relacionadas con los restablecimientos de contraseña de autoservicio, puede elegir la categoría **Administración de contraseñas de autoservicio**. Las categorías que ve se basan en el recurso en el que trabaja.  

![Opciones de categoría en la página Filtrar registros de auditoría](./media/howto-find-activity-reports/06.png "Opciones de categoría en la página Filtrar registros de auditoría")

Las categorías de actividad incluyen:

- Core Directory (Directorio principal)
- Self-service Password Management (Administración de contraseñas de autorservicio)
- Self-service Group Management (Administración de grupos de autoservicio)
- Account Provisioning (Aprovisionamiento de cuentas)

### <a name="application-usage"></a>Uso de la aplicación

Para detalles sobre el uso de todas las aplicaciones o de una sola aplicación, en **ACTIVIDAD**, seleccione **Inicios de sesión**. Para restringir los resultados, puede filtrar según el nombre del usuario o el nombre de la aplicación.

![Página Filtrar eventos de inicio de sesión](./media/howto-find-activity-reports/07.png "Página Filtrar eventos de inicio de sesión")

### <a name="security-reports"></a>Informes de seguridad

#### <a name="azure-ad-anomalous-activity-reports"></a>Informes de actividades anómalas de Azure AD

Los informes de seguridad de actividad anómala en Azure AD se consolidaron para brindarle una vista una y centralizada. Esta vista muestra todos los eventos de riesgo relacionados con la seguridad que Azure AD puede detectar e informar.

En la tabla siguiente aparecen los informes de seguridad de actividad anómala de Azure AD y los tipos de eventos de riesgo correspondientes en Azure Portal.

| Informe de actividad anómala de Azure AD |  Tipo de evento de riesgo de Identity Protection|
| :--- | :--- |
| Usuarios con credenciales perdidas | Credenciales con fugas |
| Actividad de inicio de sesión irregular | Viaje imposible a ubicaciones inusuales |
| Inicios de sesión desde dispositivos posiblemente infectados | Inicios de sesión desde dispositivos infectados|
| Inicios de sesión desde orígenes desconocidos | Inicios de sesión desde direcciones IP anónimas |
| Inicios de sesión desde direcciones IP con actividad sospechosa | Inicios de sesión desde direcciones IP con actividad sospechosa |
| - | Inicios de sesión desde ubicaciones desconocidas |

Los informes de seguridad de actividad anómala de Azure AD siguientes no se incluyen como eventos de riesgo en Azure Portal:

* Inicios de sesión tras varios errores
* Inicios de sesión desde varias ubicaciones geográficas

Para más información, consulte [Eventos de riesgo de Azure Active Directory](concept-risk-events.md).  


#### <a name="detected-risk-events"></a>Eventos de riesgo detectados

En Azure Portal, puede tener acceso a los informes sobre eventos de riesgo detectados en la hoja **Azure Active Directory**, en **SEGURIDAD**. En los informes siguientes se hace un seguimiento de los eventos de riesgo detectados:   

- Usuarios en riesgo
- Inicios de sesión no seguros

![Informes de seguridad](./media/howto-find-activity-reports/04.png "Informes de seguridad")

Para más información sobre los informes de seguridad, consulte:

- [Informe de seguridad de usuarios en riesgo en el portal de Azure Active Directory](concept-user-at-risk.md)
- [Informe de inicios de sesión poco seguros del portal de Azure Active Directory](concept-risky-sign-ins.md)


Para ver el informe **Uso de la aplicación**, vaya a la hoja **Azure Active Directory** y, en **ADMINISTRAR**, seleccione **Aplicaciones empresariales** y, luego, seleccione **Inicios de sesión**.


![Informe de inicios de sesión de aplicaciones empresariales](./media/howto-find-activity-reports/199.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información general sobre los informes, consulte [Informes de Azure Active Directory](overview-reports.md).
