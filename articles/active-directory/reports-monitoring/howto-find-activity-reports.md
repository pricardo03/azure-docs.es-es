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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fab94088d1d54012a955b0663b078d03b13d6299
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624919"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Búsqueda de informes de actividad en Azure Portal

En este artículo, aprenderá cómo buscar informes de actividad de usuario de Azure Active Directory (Azure AD) en Azure Portal.

## <a name="audit-logs-report"></a>Informe de registros de auditoría

El informe de registros de auditoría combina varios informes en torno a las actividades de la aplicación en una sola vista para la elaboración de informes basados en contexto. Para tener acceso al informe de registros de auditoría, realice lo siguiente:

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Seleccione el directorio en la esquina superior derecha y, luego, seleccione la hoja **Azure Active Directory** en el panel de navegación izquierdo.
3. Seleccione **Registros de auditoría** en la sección **Actividad** de la hoja Azure Active Directory. 

    ![Registros de auditoría](./media/howto-find-activity-reports/482.png "Registros de auditoría")

El informe de registros de auditoría consolida los siguientes informes:

* Informe de auditoría
* Actividad de restablecimiento de contraseña
* Actividad de registro de restablecimiento de contraseñas
* Actividad de los grupos de autoservicio
* Cambios de nombre de grupo de Office365
* Actividad de aprovisionamiento de cuentas
* Estado de la sustitución de contraseña
* Errores de aprovisionamiento de cuentas

### <a name="filtering-on-audit-logs"></a>Filtrado por registros de auditoría

Puede usar los filtros avanzados en el informe de auditoría para tener acceso a una categoría específica de datos, si especifica dicha categoría en el filtro **Categoría de actividad**. Por ejemplo, para ver todas las actividades relacionadas con los restablecimientos de contraseña de autoservicio, seleccione la categoría **Administración de contraseñas de autoservicio**. 

    ![Category options on the Filter Audit Logs page](./media/howto-find-activity-reports/06.png "Category options on the Filter Audit Logs page")

Las categorías de actividad incluyen:

- Core Directory (Directorio principal)
- Self-service Password Management (Administración de contraseñas de autorservicio)
- Self-service Group Management (Administración de grupos de autoservicio)
- Account Provisioning (Aprovisionamiento de cuentas)


## <a name="sign-ins-report"></a>Informe de inicios de sesión 

La vista de **Inicios de sesión** incluye todos los inicios de sesión de usuario, así como el informe de **Uso de aplicaciones**. También puede consultar la información sobre el uso de las aplicaciones en la sección **Administrar** de la información general sobre **Aplicaciones empresariales**.

    ![Enterprise applications](./media/howto-find-activity-reports/484.png "Enterprise applications")

Para tener acceso al informe de inicios de sesión, realice lo siguiente:

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Seleccione el directorio en la esquina superior derecha y, luego, seleccione la hoja **Azure Active Directory** en el panel de navegación izquierdo.
3. Seleccione **Inicios de sesión** en la sección **Actividad** de la hoja Azure Active Directory. 

    ![Vista de inicios de sesión](./media/howto-find-activity-reports/483.png "Vista de inicios de sesión")


### <a name="filtering-on-application-name"></a>Filtrado por nombre de la aplicación

Puede utilizar el informe de inicios de sesión para ver detalles sobre el uso de la aplicación si usa el filtro de nombre de usuario o de nombre de aplicación.

![Página Filtrar eventos de inicio de sesión](./media/howto-find-activity-reports/07.png "Página Filtrar eventos de inicio de sesión")

## <a name="security-reports"></a>Informes de seguridad

### <a name="anomalous-activity-reports"></a>Informes de actividades anómalas

Los informes de actividades anómalas proporcionan información sobre eventos de riesgo relacionados con la seguridad que Azure AD puede detectar y notificar.

En la tabla siguiente aparecen los informes de seguridad de actividad anómala de Azure AD y los tipos de eventos de riesgo correspondientes en Azure Portal. Para más información, consulte [Eventos de riesgo de Azure Active Directory](concept-risk-events.md).  


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


### <a name="detected-risk-events"></a>Eventos de riesgo detectados

Puede acceder a los informes acerca de los eventos de riesgo detectados en la sección **Seguridad** de la hoja **Azure Active Directory** de [Azure Portal](https://portal.azure.com). En los informes siguientes se hace un seguimiento de los eventos de riesgo detectados:   

- [Usuarios en riesgo](concept-user-at-risk.md)
- [Inicios de sesión no seguros](concept-risky-sign-ins.md)

    ![Informes de seguridad](./media/howto-find-activity-reports/04.png "Informes de seguridad")

## <a name="next-steps"></a>Pasos siguientes

* [Información general sobre los registros de auditoría](concept-audit-logs.md)
* [Información general sobre los inicios de sesión](concept-sign-ins.md)
* [Información general sobre los eventos de riesgo](concept-risk-events.md)
