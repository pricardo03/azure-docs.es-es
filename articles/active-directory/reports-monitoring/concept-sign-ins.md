---
title: Informes de actividad de inicio de sesión en el portal de Azure Active Directory | Microsoft Docs
description: Introducción a los informes de actividad de inicio de sesión en el portal de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac65a9ac81bca942f9fcbe802fdbf8a0aa3f8248
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997672"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Informes de actividad de inicio de sesión en el portal de Azure Active Directory

La arquitectura de los informes de Azure Active Directory (Azure AD) consta de los siguientes componentes:

- **Actividad** 
    - **Inicios de sesión**: información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
    - **Registros de auditoría**: los [registros de auditoría](concept-audit-logs.md) proporcionan información de la actividad del sistema sobre la administración de usuarios y grupos, aplicaciones administradas y actividades de directorio. - 
- **Seguridad** 
    - **Inicios de sesión de riesgo**: un [inicio de sesión de riesgo](concept-risky-sign-ins.md) es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
    - **Usuarios marcados en riesgo**: un [usuario en riesgo](concept-user-at-risk.md) es un indicador de una cuenta de usuario que puede haber estado en peligro.

Este tema ofrece una visión general del reporte de inicios de sesión.

## <a name="prerequisites"></a>Requisitos previos

### <a name="who-can-access-the-data"></a>¿Quién puede acceder a los datos?
* Usuarios de los roles Administrador de seguridad, Lector de seguridad o Lector de informes
* Administradores globales
* Además, cualquier usuario (no administradores) puede acceder a sus propios inicios de sesión 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>¿Qué licencia de Azure AD se necesita para acceder a la actividad de inicio de sesión?
* El inquilino debe tener una licencia de Azure AD Premium asociada para ver el informe de actividades de inicio de sesión activas. Consulte [Introducción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para actualizar la edición de Azure Active Directory. Tenga en cuenta que si no tuvo ningún dato de las actividades antes de la actualización, los datos tardarán un par de días en mostrarse en los informes después de actualizar a una licencia prémium.

## <a name="sign-ins-report"></a>Informe de inicios de sesión

El informe de inicios de sesión de usuario proporciona respuestas a las preguntas siguientes:

* ¿Cuál es el patrón de inicio de sesión de un usuario?
* ¿Cuántos usuarios han iniciado sesión en una semana?
* ¿Cuál es el estado de estos inicios de sesión?

Se puede tener acceso al informe de inicios de sesión si, desde [Azure Portal](https://portal.azure.com), selecciona **Azure Active Directory** > **Actividad** > **Inicios de sesión**. Tenga en cuenta que algunos registros de inicio de sesión pueden tardar hasta dos horas antes de aparecer en el portal.

![Actividad de inicio de sesión](./media/concept-sign-ins/61.png "Actividad de inicio de sesión")

> [!IMPORTANT]
> El informe de inicios de sesión solo muestra los inicios de sesión **interactivos**, es decir, los inicios de sesión donde un usuario inicia sesión manualmente con su nombre de usuario y contraseña. Los inicios de sesión no interactivos, como la autenticación de servicio a servicio, no se muestran en el informe de inicios de sesión. 

Un registro de inicios de sesión tiene una vista de lista predeterminada que muestra:

- La fecha de inicio de sesión
- El usuario relacionado
- La aplicación en la que el usuario ha iniciado sesión
- El estado de inicio de sesión
- El estado de la detección de riesgos
- El estado del requisito de la autenticación multifactor (MFA)

![Actividad de inicio de sesión](./media/concept-sign-ins/01.png "Actividad de inicio de sesión")

Puede personalizar la vista de lista; para ello, haga clic en **Columnas** en la barra de herramientas.

![Actividad de inicio de sesión](./media/concept-sign-ins/19.png "Actividad de inicio de sesión")

Esto le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.

![Actividad de inicio de sesión](./media/concept-sign-ins/02.png "Actividad de inicio de sesión")

Seleccione un elemento de la vista de lista para obtener información más detallada.

![Actividad de inicio de sesión](./media/concept-sign-ins/03.png "Actividad de inicio de sesión")

> [!NOTE]
> Los clientes ahora pueden solucionar problemas de directivas de acceso condicional con todos los informes de inicios de sesión. Al hacer clic en la pestaña **Acceso condicional** de un registro de inicio de sesión, los clientes pueden revisar el estado de acceso condicional y profundizar en los detalles de las directivas que se aplican al inicio de sesión y del resultado de cada directiva.
> Para más información, vea las [preguntas más frecuentes sobre la información de acceso condicional en todos los inicios de sesión](reports-faq.md#conditional-access).

![Actividad de inicio de sesión](./media/concept-sign-ins/ConditionalAccess.png "Actividad de inicio de sesión")


## <a name="filter-sign-in-activities"></a>Filtrado de las actividades de inicio de sesión

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de inicio de sesión con los siguientes campos predeterminados:

- Usuario
- Application
- Estado de inicio de sesión
- Acceso condicional
- Date

![Actividad de inicio de sesión](./media/concept-sign-ins/04.png "Actividad de inicio de sesión")

El filtro **usuario** permite especificar el nombre o el nombre principal de usuario (UPN) del usuario que le interesa.

El filtro **aplicación** permite especificar el nombre de la aplicación que le interesa.

El filtro **estado de inicio de sesión** le permite seleccionar:

- Todo
- Correcto
- Error

El filtro de **acceso condicional** le permite seleccionar el estado de la directiva de entidad emisora de certificados para el inicio de sesión:

- Todo
- No aplicado
- Correcto
- Error

El filtro **Fecha** le permite definir un período de tiempo para los datos devueltos.  
Los valores posibles son:

- 1 mes
- 7 días
- 24 horas
- Intervalo de tiempo personalizado

Cuando se selecciona un intervalo de tiempo personalizado, puede configurar una hora de inicio y una hora de finalización.

Si agrega campos adicionales a la vista de inicios de sesión, estos campos se agregarán automáticamente a la lista de filtros. Por ejemplo, si agrega el campo **Aplicación cliente** a la lista, también obtendrá otra opción de filtro que le permitirá establecer los siguientes filtros:

- Browser      
- Exchange ActiveSync (compatible)               
- Exchange ActiveSync (no compatible)
- Otros clientes               
    - IMAP
    - MAPI
    - Clientes antiguos de Office
    - POP
    - SMTP


![Actividad de inicio de sesión](./media/concept-sign-ins/12.png "Actividad de inicio de sesión")


## <a name="download-sign-in-activities"></a>Descarga de actividades de inicio de sesión

Puede [descargar los datos de inicios de sesión](quickstart-download-sign-in-report.md) si quiere trabajar con ellos fuera de Azure Portal. Al hacer clic en **descargar** le ofrece la opción para crear un archivo CSV o JSON de los 250 000 registros más recientes.  

![Descargar](./media/concept-sign-ins/71.png "Descargar")

> [!IMPORTANT]
> El número de registros que se puede descargar también está restringido por las [directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).  


## <a name="sign-ins-data-shortcuts"></a>Accesos directos a los datos de inicios de sesión

Además de Azure AD, Azure Portal proporciona puntos de entrada adicionales para los datos de inicios de sesión:

- Información general sobre la protección de la seguridad de la identidad
- Usuarios
- Grupos
- Aplicaciones empresariales

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Datos de inicio de sesión de usuarios en la protección de la identidad de la seguridad

El gráfico de inicio de sesión de usuario en el **protección de identidad de seguridad** página de información general muestra agregaciones semanales de inicios de sesión para todos los usuarios en un período de tiempo determinado. El valor predeterminado para el período es 30 días.

![Actividad de inicio de sesión](./media/concept-sign-ins/06.png "Actividad de inicio de sesión")

Al hacer clic en un día del gráfico de inicio de sesión, obtiene una vista general de las actividades de inicio de sesión de ese día.

Cada fila de la lista de actividades de inicio de sesión muestra:

* ¿Quién ha iniciado sesión?
* ¿Qué aplicación era el destino del inicio de sesión?
* ¿Cuál es el estado del inicio de sesión?
* ¿Cuál es el estado de MFA del inicio de sesión?

Si hace clic en un elemento, obtendrá más detalles sobre la operación de inicio de sesión:

- Id. de usuario
- Usuario
- Nombre de usuario
- Identificador de aplicación
- Application
- Cliente
- Location
- Dirección IP
- Date
- Se requiere MFA
- Estado de inicio de sesión

> [!NOTE]
> Las direcciones IP se emiten de forma que no haya ninguna conexión definitiva entre una dirección IP y donde se encuentre físicamente el equipo con esa dirección. La asignación de direcciones IP se complica por el hecho de que los proveedores de dispositivos móviles y las VPN emiten direcciones IP desde grupos centrales que, a menudo, están muy lejos de donde el dispositivo cliente se usa realmente. Actualmente, la conversión de la dirección IP en una ubicación física en los informes de Azure AD es un esfuerzo notable basado en seguimientos, datos del Registro, búsquedas inversas y otra información.

En la página **Usuarios**, puede obtener una vista general completa de todos los inicios de sesión del usuario haciendo clic en **Inicios de sesión** en la sección **Actividad**.

![Actividad de inicio de sesión](./media/concept-sign-ins/08.png "Actividad de inicio de sesión")

## <a name="usage-of-managed-applications"></a>Uso de las aplicaciones administradas

Con una vista centrada en la aplicación de los datos de inicio de sesión, puede responder a preguntas tales como:

* ¿Quién está usando mis aplicaciones?
* ¿Cuáles son las tres aplicaciones principales en su organización?
* Recientemente he implementado una aplicación. ¿Cómo sigue?

El punto de entrada a los datos son las tres aplicaciones principales de su organización en el informe de los últimos 30 días en la sección **Introducción** en **Aplicaciones empresariales**.

![Actividad de inicio de sesión](./media/concept-sign-ins/10.png "Actividad de inicio de sesión")

La aplicación uso graph agregaciones semanales de inicios de sesión para sus aplicaciones 3 principales en un período de tiempo determinado. El valor predeterminado para el período es 30 días.

![Actividad de inicio de sesión](./media/concept-sign-ins/47.png "Actividad de inicio de sesión")

Si lo desea, puede establecer el foco en una aplicación específica.

![Informes](./media/concept-sign-ins/single_spp_usage_graph.png "Informes")

Al hacer clic en un día del gráfico de uso de la aplicación, obtendrá una lista detallada de las actividades de inicio de sesión.

La opción **Inicios de sesión** ofrece una descripción completa de todos los eventos de inicio de sesión para sus aplicaciones.

![Actividad de inicio de sesión](./media/concept-sign-ins/11.png "Actividad de inicio de sesión")

## <a name="office-365-activity-logs"></a>Registros de actividad de Office 365

Puede ver los registros de actividad de Office 365 desde el [centro de administración de Microsoft 365](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Aunque la actividad de Office 365 y los registros de actividad de Azure AD comparten mucho de los recursos del directorio, solo el centro de administración de Microsoft 365 proporciona una vista completa de los registros de actividad de Office 365. 

También puede tener acceso a los registros de actividad de Office 365 mediante programación con las [API de administración de Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Pasos siguientes

* [Códigos de error de los informes de actividad de inicio de sesión](reference-sign-ins-error-codes.md)
* [Directivas de retención de datos de Azure AD](reference-reports-data-retention.md)
* [Latencias de informes de Azure AD](reference-reports-latencies.md)

