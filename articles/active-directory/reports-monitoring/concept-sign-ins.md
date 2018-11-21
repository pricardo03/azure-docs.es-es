---
title: Informes de actividad de inicio de sesión en el portal de Azure Active Directory | Microsoft Docs
description: Introducción a los informes de actividad de inicio de sesión en el portal de Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ae962cba5e3d08661eb1c93edfc2feb221a9367e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623803"
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
* El inquilino debe tener una licencia de Azure AD Premium asociada para ver el informe de actividades de inicio de sesión activas

## <a name="sign-ins-report"></a>Informe de inicios de sesión

El informe de inicios de sesión de usuario proporciona respuestas a las preguntas siguientes:

* ¿Cuál es el patrón de inicio de sesión de un usuario?
* ¿Cuántos usuarios han iniciado sesión en una semana?
* ¿Cuál es el estado de estos inicios de sesión?

Se puede tener acceso al informe de inicios de sesión si, desde [Azure Portal](https://portal.azure.com), selecciona **Azure Active Directory** > **Actividad** > **Inicios de sesión**.

![Actividad de inicio de sesión](./media/concept-sign-ins/61.png "Actividad de inicio de sesión")

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

Puede [descargar los datos de inicios de sesión](quickstart-download-sign-in-report.md) si quiere trabajar con ellos fuera de Azure Portal. Si hace clic en **Descargar** se crea un archivo CSV con los 5000 registros más recientes.  Además de un botón de descarga, Azure Portal también proporciona una opción para [generar un script para descargar los datos](tutorial-signin-logs-download-script.md).  

![Descargar](./media/concept-sign-ins/71.png "Descargar")

Si necesita más flexibilidad, puede usar la solución de script. Si hace clic en **Script** se crea un script de PowerShell que incluye todos los filtros que ha establecido. Descargue y ejecute este script en **modo de administrador** para generar el archivo CSV. 

> [!IMPORTANT]
> El número de registros que se puede descargar también está restringido por las [directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).  

### <a name="running-the-script-on-a-windows-10-machine"></a>Ejecución del script en un equipo Windows 10

Si desea ejecutar el script en un equipo **Windows 10**, primero deberá realizar algunos pasos adicionales. 

1. Instale el [módulo AzureRM](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0l).
2. Importe el módulo; abra un símbolo del sistema de PowerShell y ejecute el comando **Import-Module AzureRM**.
3. Ejecute **Set-ExecutionPolicy unrestricted** y seleccione **Sí a todo**. 
4. Ahora puede ejecutar el script de PowerShell descargado en modo de administrador para generar el archivo CSV.

## <a name="sign-ins-data-shortcuts"></a>Accesos directos a los datos de inicios de sesión

Además de Azure AD, Azure Portal proporciona puntos de entrada adicionales para los datos de inicios de sesión:

- Información general sobre la protección de la seguridad de la identidad
- Usuarios
- Grupos
- Aplicaciones empresariales

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Datos de inicio de sesión de usuarios en la protección de la identidad de la seguridad

El gráfico de inicio de sesión de usuario en la página de información general de la **protección de la seguridad de identidad** muestra agregaciones semanales de inicios de sesión para todos los usuarios en un período determinado. El valor predeterminado para el período es 30 días.

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
- Ubicación
- Dirección IP
- Date
- Se requiere MFA
- Estado de inicio de sesión
 
En la página **Usuarios**, puede obtener una vista general completa de todos los inicios de sesión del usuario haciendo clic en **Inicios de sesión** en la sección **Actividad**.

![Actividad de inicio de sesión](./media/concept-sign-ins/08.png "Actividad de inicio de sesión")

## <a name="usage-of-managed-applications"></a>Uso de las aplicaciones administradas

Con una vista centrada en la aplicación de los datos de inicio de sesión, puede responder a preguntas tales como:

* ¿Quién está usando mis aplicaciones?
* ¿Cuáles son las tres aplicaciones principales en su organización?
* Recientemente he implementado una aplicación. ¿Cómo sigue?

El punto de entrada a los datos son las tres aplicaciones principales de su organización en el informe de los últimos 30 días en la sección **Introducción** en **Aplicaciones empresariales**.

![Actividad de inicio de sesión](./media/concept-sign-ins/10.png "Actividad de inicio de sesión")

Agregaciones semanales del gráfico de uso de la aplicación de inicios de sesión para las tres aplicaciones principales en un período determinado. El valor predeterminado para el período es 30 días.

![Actividad de inicio de sesión](./media/concept-sign-ins/47.png "Actividad de inicio de sesión")

Si lo desea, puede establecer el foco en una aplicación específica.

![Informes](./media/concept-sign-ins/single_spp_usage_graph.png "Informes")

Al hacer clic en un día del gráfico de uso de la aplicación, obtendrá una lista detallada de las actividades de inicio de sesión.

La opción **Inicios de sesión** ofrece una descripción completa de todos los eventos de inicio de sesión para sus aplicaciones.

![Actividad de inicio de sesión](./media/concept-sign-ins/11.png "Actividad de inicio de sesión")

## <a name="next-steps"></a>Pasos siguientes

* [Códigos de error de los informes de actividad de inicio de sesión](reference-sign-ins-error-codes.md)
* [Directivas de retención de datos de Azure AD](reference-reports-data-retention.md)
* [Latencias de informes de Azure AD](reference-reports-latencies.md)

