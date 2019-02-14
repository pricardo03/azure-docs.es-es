---
title: Búsqueda de informes de actividad de usuario de Azure Active Directory en Azure Portal | Microsoft Docs
description: Obtenga información sobre dónde están los informes de actividad de usuario de Azure Active Directory en Azure Portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ff5b998222f9a7320384edea1ed69e7a65e9139
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171305"
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

Las categorías de actividad incluyen:

- Core Directory (Directorio principal)
- Self-service Password Management (Administración de contraseñas de autorservicio)
- Self-service Group Management (Administración de grupos de autoservicio)
- Account Provisioning (Aprovisionamiento de cuentas)


## <a name="sign-ins-report"></a>Informe de inicios de sesión 

La vista de **Inicios de sesión** incluye todos los inicios de sesión de usuario, así como el informe de **Uso de aplicaciones**. También puede consultar la información sobre el uso de las aplicaciones en la sección **Administrar** de la información general sobre **Aplicaciones empresariales**.

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

## <a name="troubleshoot-issues-with-activity-reports"></a>Solución de problemas con los informes de actividad

### <a name="missing-data-in-the-downloaded-activity-logs"></a>Faltan datos en los registros de actividad descargados

#### <a name="symptoms"></a>Síntomas 

Descargue los registros de actividad (auditoría o inicios de sesión) y no veo todos los registros del tiempo que elegí. ¿Por qué? 

 ![Informes](./media/troubleshoot-missing-data-download/01.png)
 
#### <a name="cause"></a>Causa

Al descargar los registros de actividad en Azure Portal, limitamos la escala a 5000 registros, con el más reciente primero. 

#### <a name="resolution"></a>Resolución

Puede sacar provecho de la [API de creación de informes de Azure AD](concept-reporting-api.md) para capturar hasta un millones de registros en cualquier momento dado. Nuestro enfoque recomendado es [ejecutar de forma programada un script](tutorial-signin-logs-download-script.md) que llame a las API de creación de informes para capturar registros de forma incremental durante un período de tiempo (por ejemplo, a diario o semanalmente). 

### <a name="missing-audit-data-for-recent-actions-in-the-azure-portal"></a>Faltan datos de auditoría para acciones recientes en Azure Portal

#### <a name="symptoms"></a>Síntomas

Realice algunas acciones en Azure Portal y esperaba ver los registros de auditoría de dichas acciones en la hoja `Activity logs > Audit Logs`, pero no los encuentro.

 ![Informes](./media/troubleshoot-missing-audit-data/01.png)
 
#### <a name="cause"></a>Causa

Las acciones no aparecen inmediatamente en los registro de actividad. En la tabla siguiente se enumeran el tiempo de latencia de los registros de actividad. 

| Informe | &nbsp; | Latencia (P95) | Latencia (P99) |
|--------|--------|---------------|---------------|
| Auditoría de directorio | &nbsp; | 2 minutos | 5 minutos |
| Actividad de inicio de sesión | &nbsp; | 2 minutos | 5 minutos | 

#### <a name="resolution"></a>Resolución

Espere entre 15 minutos y dos horas para ver si las acciones aparecen en el registro. Si no ve los registros incluso después de dos horas, [cree una incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) y la examinaremos.

### <a name="missing-logs-for-recent-user-sign-ins-in-the-azure-ad-sign-ins-activity-log"></a>Faltan registros para los inicios de sesión de usuario recientes en el registro de actividad de inicios de sesión de Azure AD

#### <a name="symptoms"></a>Síntomas

Hace poco he iniciado sesión en Azure Portal y esperaba ver los registros de inicio de sesión de dichas acciones en la hoja `Activity logs > Sign-ins`, pero no los encuentro.

 ![Informes](./media/troubleshoot-missing-audit-data/02.png)
 
#### <a name="cause"></a>Causa

Las acciones no aparecen inmediatamente en los registro de actividad. En la tabla siguiente se enumeran el tiempo de latencia de los registros de actividad. 

| Informe | &nbsp; | Latencia (P95) | Latencia (P99) |
|--------|--------|---------------|---------------|
| Auditoría de directorio | &nbsp; | 2 minutos | 5 minutos |
| Actividad de inicio de sesión | &nbsp; | 2 minutos | 5 minutos | 

#### <a name="resolution"></a>Resolución

Espere entre 15 minutos y dos horas para ver si las acciones aparecen en el registro. Si no ve los registros incluso después de dos horas, [cree una incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) y la examinaremos.

### <a name="i-cant-view-more-than-30-days-of-report-data-in-the-azure-portal"></a>No puedo ver más de 30 días de datos de informes en Azure Portal

#### <a name="symptoms"></a>Síntomas

No puedo ver más de 30 días de datos de inicio de sesión y auditoría en Azure Portal. ¿Por qué? 

 ![Informes](./media/troubleshoot-missing-audit-data/03.png)

#### <a name="cause"></a>Causa

En función de su licencia, las acciones de Azure Active Directory almacenan los informes de actividad durante el siguiente tiempo:

| Informe           | &nbsp; |  Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| ---              | ----   |  ---           | ---                 | ---                 |
| Auditoría de directorio  | &nbsp; |   7 días     | 30 días             | 30 días             |
| Actividad de inicio de sesión | &nbsp; | No disponible. Puede acceder a sus propios inicios de sesión durante 7 días en la hoja del perfil de usuario individual. | 30 días | 30 días             |

Para más información, consulte [Directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).  

#### <a name="resolution"></a>Resolución

Tiene dos opciones para conservar los datos durante más de 30 días. Puede usar las [API de generación de informes de Azure AD](concept-reporting-api.md) para recuperar los datos mediante programación y almacenarlos en una base de datos. Como alternativa, puede integrar los registros de auditoría en un sistema SIEM de terceros como Splunk o SumoLogic.

## <a name="next-steps"></a>Pasos siguientes

* [Información general sobre los registros de auditoría](concept-audit-logs.md)
* [Información general sobre los inicios de sesión](concept-sign-ins.md)
* [Información general sobre los eventos de riesgo](concept-risk-events.md)
