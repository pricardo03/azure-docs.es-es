---
title: Informes de actividad de auditoría en el portal de Azure Active Directory | Microsoft Docs
description: Introducción a los informes de actividad de auditoría en el portal de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3da96c09026baff3965e0a90d1f461fd948a3a50
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438402"
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Informes de actividad de auditoría en el portal de Azure Active Directory 

Con los informes de Azure Active Directory (Azure AD), puede obtener toda la información que necesita para determinar cómo funciona el entorno.

Esta arquitectura de informes consta de los siguientes componentes:

- **Actividad** 
    - **Inicios de sesión**: el [informe de inicios de sesión](concept-sign-ins.md) proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
    - Los **registros de auditoría** proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.
- **Seguridad** 
    - **Inicios de sesión de riesgo**: un [inicio de sesión de riesgo](concept-risky-sign-ins.md) es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario. 
    - **Usuarios marcados en riesgo**: un [usuario en riesgo](concept-user-at-risk.md) es un indicador de una cuenta de usuario que puede haber estado en peligro.

En este artículo se proporciona información general del informe de auditoría.
 
## <a name="who-can-access-the-data"></a>¿Quién puede acceder a los datos?

* Los usuarios de la **Administrador de seguridad del**, **lector de seguridad**, **lector de informes** o **administrador Global** roles
* Además, todos los usuarios (no administradores) pueden ver sus propias actividades de auditoría

## <a name="audit-logs"></a>Registros de auditoría

Los registros de auditoría de Azure AD proporcionan registros de las actividades del sistema de cara al cumplimiento. Para acceder al informe de auditoría, seleccione **Registros de auditoría** en la sección **Actividad** de **Azure Active Directory**. Tenga en cuenta que los registros de auditoría pueden tener una latencia de hasta una hora, por lo que los datos de la actividad de auditoría pueden tardar tiempo en aparecer en el portal una vez que haya completado la tarea.

![Registros de auditoría](./media/concept-audit-logs/61.png "Registros de auditoría")

Un registro de auditoría tiene una vista de lista predeterminada que muestra:

- la fecha y hora de la repetición
- el servicio que ha iniciado la repetición
- la categoría y el nombre de la actividad (*lo*) 
- el estado de la actividad (éxito o error)
- el destino
- el iniciador o actor (quién) de una actividad

![Registros de auditoría](./media/concept-audit-logs/listview.png "Registros de auditoría")

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Registros de auditoría](./media/concept-audit-logs/columns.png "Registros de auditoría")

Esto le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.

![Registros de auditoría](./media/concept-audit-logs/columnselect.png "Registros de auditoría")

Seleccione un elemento de la vista de lista para obtener información más detallada.

![Registros de auditoría](./media/concept-audit-logs/details.png "Registros de auditoría")


## <a name="filtering-audit-logs"></a>Filtrado de registros de auditoría

Puede filtrar los datos de auditoría por los siguientes campos:

- Servicio
- Categoría
- Actividad
- Status
- Destino
- Iniciado por (actor)
- Intervalo de fechas

![Registros de auditoría](./media/concept-audit-logs/filter.png "Registros de auditoría")

El **servicio** filtro le permite seleccionar en una lista desplegable de los siguientes servicios:

- Todo
- Revisiones de acceso
- Account Provisioning (Aprovisionamiento de cuentas) 
- Aplicación de inicio de sesión único
- Métodos de autenticación
- B2C
- Acceso condicional
- Core Directory (Directorio principal)
- Administración de derechos
- Protección de identidad
- Invited Users (Usuarios invitados)
- PIM
- Self-service Group Management (Administración de grupos de autoservicio)
- Administración de autoservicio Passord
- Términos de uso

El **categoría** filtro le permite seleccionar uno de los siguientes filtros:

- Todo
- AdministrativeUnit
- ApplicationManagement
- Authentication
- Autorización
- Contacto
- Dispositivo
- DeviceConfiguration
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- Otros
- Directiva
- ResourceManagement
- RoleManagement
- UserManagement

El **actividad** filtro se basa en la selección de tipo de recurso categoría y la actividad que realice. Puede seleccionar la actividad específica que desea ver o elegir todas. 

Para obtener la lista de todas las actividades de auditoría, use Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, donde $tenantdomain = el nombre de dominio, o bien consulte el artículo sobre [eventos del informe de auditoría](reference-audit-activities.md).

El **estado** filtro le permite filtrar en función del estado de una operación de auditoría. El estado puede ser uno de los siguientes:

- Todo
- Correcto
- Error

El **destino** filtro le permite buscar un destino determinado por nombre o nombre principal de usuario (UPN). El nombre de destino y el UPN distinguen mayúsculas de minúsculas. 

El **iniciadas por** filtro le permite definir el nombre de un actor o un nombre principal universal (UPN). El nombre y el UPN distinguen mayúsculas de minúsculas.

El **intervalo de fechas** filtro permite definir un período de tiempo para los datos devueltos.  
Los valores posibles son:

- 1 mes
- 7 días
- 24 horas
- Personalizado

Cuando se selecciona un intervalo de tiempo personalizado, puede configurar una hora de inicio y una hora de finalización.

También puede descargar los datos filtrados, registros de hasta 250 000, seleccionando la **descargar** botón. Puede descargar los registros en formato CSV o JSON. El número de registros que se puede descargar también está restringido por las [directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).

![Registros de auditoría](./media/concept-audit-logs/download.png "Registros de auditoría")

## <a name="audit-logs-shortcuts"></a>Métodos abreviados de los registros de auditoría

Además de **Azure Active Directory**, Azure Portal proporciona dos puntos de entrada adicionales para auditar datos:

- Usuarios y grupos
- Aplicaciones empresariales

### <a name="users-and-groups-audit-logs"></a>Registros de auditoría de los usuarios y grupos

Con los informes de auditoría basadas en grupos y usuarios, puede obtener respuestas a preguntas como:

- ¿Qué tipos de actualizaciones se han aplicado a los usuarios?

- ¿Cuántos usuarios han cambiado?

- ¿Cuántas contraseñas han cambiado?

- ¿Qué ha hecho un administrador en un directorio?

- ¿Cuáles son los grupos que se han agregado?

- ¿Hay grupos con cambios de pertenencia?

- ¿Se han cambiado los propietarios del grupo?

- ¿Qué licencias se han asignado a un grupo o un usuario?

Si desea revisar los datos de auditorías que está relacionada con los usuarios, puede encontrar una vista filtrada en **registros de auditoría** en el **actividad** sección de la **usuarios** ficha. En este punto de entrada tiene **UserManagement** como categoría de preseleccionadas.

![Registros de auditoría](./media/concept-audit-logs/users.png "Registros de auditoría")

Si desea revisar los datos de auditorías relacionados con los grupos, puede encontrar una vista filtrada en **registros de auditoría** en el **actividad** sección de la **grupos** ficha. En este punto de entrada tiene **GroupManagement** como categoría de preseleccionadas.

![Registros de auditoría](./media/concept-audit-logs/groups.png "Registros de auditoría")

### <a name="enterprise-applications-audit-logs"></a>Registros de auditoría de aplicaciones empresariales

Con los informes de auditoría basadas en aplicaciones, puede obtener respuestas a preguntas tales como:

* ¿Qué aplicaciones se han agregado o actualizado?
* ¿Qué aplicaciones se han quitado?
* ¿Ha cambiado la entidad de servicio de una aplicación?
* ¿Se han cambiado los nombres de las aplicaciones?
* ¿Quién dio el consentimiento a una aplicación?

Si quiere revisar los datos de auditoría relacionados con las aplicaciones, puede encontrar una vista filtrada en **Registros de auditoría** en la sección **Actividad** de la hoja **Aplicaciones empresariales**. En este punto de entrada tiene **aplicaciones empresariales** preseleccionados como el **tipo de aplicación**.

![Registros de auditoría](./media/concept-audit-logs/enterpriseapplications.png "Registros de auditoría")

## <a name="office-365-activity-logs"></a>Registros de actividad de Office 365

Puede ver los registros de actividad de Office 365 desde el [centro de administración de Microsoft 365](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Aunque la actividad de Office 365 y los registros de actividad de Azure AD comparten mucho de los recursos del directorio, solo el centro de administración de Microsoft 365 proporciona una vista completa de los registros de actividad de Office 365. 

También puede tener acceso a los registros de actividad de Office 365 mediante programación con las [API de administración de Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Pasos siguientes

- [Referencia sobre actividades de auditoría de Azure AD](reference-audit-activities.md)
- [Referencia sobre la retención de informes de Azure AD](reference-reports-data-retention.md)
- [Referencia sobre las latencias de registro de Azure AD](reference-reports-latencies.md)
