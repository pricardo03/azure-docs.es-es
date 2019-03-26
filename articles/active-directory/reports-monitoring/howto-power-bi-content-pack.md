---
title: Uso del paquete de contenido de Power BI de Azure Active Directory | Microsoft Docs
description: Aprenda a usar el paquete de contenido de Power BI de Azure Active Directory.
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 934562147fedcc81b16fd1ad2534af5662ef4b78
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436836"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Uso del paquete de contenido de Power BI de Azure Active Directory

|  |
|--|
|Actualmente, el paquete de contenido de Power BI de Azure AD usa las Graph API de Azure AD para recuperar datos del inquilino de Azure AD. Como resultado, es posible que vea alguna discrepancia entre los datos disponibles en el paquete de contenido y los datos recuperados con la [API de Microsoft Graph para crear informes](concept-reporting-api.md). |
|  |

El paquete de contenido de Power BI para Azure Active Directory (Azure AD) contiene informes precompilados para ayudarle a comprender cómo los usuarios adoptan y usan características de Azure AD. La rica experiencia de visualización de Power BI le va a permitir entender todas las actividades que tienen lugar en el directorio. También puede crear su propio panel y compartirlo con cualquier persona de su organización. 

## <a name="prerequisites"></a>Requisitos previos

Para usar el paquete de contenido necesita una licencia Premium de Azure AD (P1 y P2). Consulte [Introducción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para actualizar la edición de Azure Active Directory.

## <a name="install-the-content-pack"></a>Instalación del paquete de contenido

Consulte la [guía de inicio rápido](quickstart-install-power-bi-content-pack.md) para instalar el paquete de contenido de Azure AD Power BI.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Los informes incluidos en esta versión del paquete de contenido de Azure AD

Los informes siguientes se incluyen en el paquete de contenido de Power BI de Azure AD. Los informes contienen datos de los **últimos 30 días**.

**Informe de uso y tendencias de las aplicaciones**:  este informe le proporciona información detallada sobre las aplicaciones que se usan en su organización. Puede obtener una lista de las aplicaciones más populares, o comprender cómo se usa una aplicación que recientemente se ha implementado en su organización. Esto le permite realizar un seguimiento del uso y mejorarlo con el tiempo.

**Inicios de sesión por ubicación y usuarios**: este informe proporciona datos sobre todos los inicios de sesión realizados mediante la identidad de Azure. Con este informe, puede explorar en profundidad los inicios de sesión individuales y responder a preguntas como:

- ¿Desde dónde inició sesión este usuario?
- ¿Qué usuario tiene la mayor cantidad de inicios de sesión y desde dónde inicia sesión? 
- ¿Fue el inicio de sesión correcto?  
 
También puede filtrar los resultados mediante la selección de una fecha o ubicación específicas.

**Usuarios únicos por aplicación**:  este informe proporciona una vista de todos los usuarios únicos que usan una aplicación determinada. Solo se incluyen los usuarios que han iniciado sesión "*correctamente*" en una aplicación.

**Inicios de sesión de dispositivo**: este informe le ayuda a comprender los diversos perfiles de dispositivo usados dentro de su organización y a determinar las directivas de dispositivo según el uso. Se proporcionan datos sobre el tipo de sistema operativo y los exploradores usados para iniciar sesión en las aplicaciones, junto con información detallada sobre los usuarios, por ejemplo:

- User Name
- Dirección IP
- Ubicación 
- Estado de inicio de sesión 

**Embudo de SSPR**: este informe le ayuda a comprender cómo se usa la herramienta SSPR dentro de su organización. Puede ver cuántos restablecimientos de contraseña se intentaron con la herramienta SSPR y cuántos de ellos se realizaron correctamente. También puede profundizar más en los errores de restablecimiento de contraseña y comprender por qué se produjeron determinados errores. 

## <a name="customize-azure-ad-activity-content-pack"></a>Personalizar el paquete de contenido de actividad de Azure AD

**Cambio de la visualización**:  para cambiar la visualización de un informe, haga clic en **Editar informe** y seleccione la visualización que desee.
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/09.png) 
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/10.png) 

**Inclusión de campos adicionales**:  puede agregar un campo al informe o quitarlo seleccionando el objeto visual que desea agregar o quitar en el campo. Por ejemplo, puede agregar el campo "estado de inicio de sesión" a la vista de tabla, tal como se muestra a continuación. 
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/11.png) 

**Anclado de visualizaciones al panel**:  puede personalizar el panel. Así, puede incluir sus propias visualizaciones en el informe y anclar este informe al panel. 

![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/13.png) 
 
**Compartir el panel**: también puede compartir el panel con los usuarios de su organización. Una vez que comparta el informe, los usuarios pueden ver los campos que ha seleccionado en el informe.
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Programar una actualización diaria de su informe de Power BI

Para programar una actualización diaria de su informe de Power BI, vaya a **Conjuntos de datos** > **Configuración** > **Programar actualización** y configúrela como se muestra a continuación.
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>Actualización a una versión más reciente del paquete de contenido

Si quiere actualizar el paquete de contenido a una versión más reciente, siga estos pasos:

- Descargue el nuevo paquete de contenido y configúrelo según las instrucciones de este artículo.

- Una vez configurado, vaya a **Origen de datos** > **Configuración** > **Credenciales de origen de datos** y escriba sus credenciales.

    ![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/16.png) 

Una vez que compruebe que la nueva versión del paquete de contenido funciona según lo esperado, puede quitar la versión anterior, si es necesario; para ello, elimine los informes subyacentes y los conjuntos de datos asociados con ese paquete de contenido.

## <a name="troubleshoot-content-pack-errors"></a>Solución de errores de los paquetes de contenido

Mientras trabaja con el paquete de contenido, es posible que se encuentre con los siguientes errores: 

- [Error al actualizar](#refresh-failed) 
- [Error al actualizar las credenciales del origen de datos ](#failed-to-update-data-source-credentials) 
- [La importación de datos tarda demasiado](#data-import-is-too-slow) 

Para obtener ayuda general con Power BI, visite estos [artículos de Ayuda](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>Error al actualizar 
 
**¿Cómo aparece este error?**: correo electrónico desde Power BI o estado de error en el historial de actualización. 


| Causa | Solución |
| ---   | ---        |
| Los errores de actualización pueden producirse cuando se han restablecido las credenciales de los usuarios que se conectan al paquete de contenido, pero no se actualizan en la configuración de conexión del paquete de contenido. | En Power BI, busque el conjunto de datos correspondiente al panel de registros de actividad de Azure AD (**registros de actividad de Azure Active Directory**), elija programar la actualización y, luego, escriba sus credenciales de Azure AD. |
| Una actualización puede producir un error debido a problemas de datos en el paquete de contenido subyacente. | [Abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>Error al actualizar las credenciales del origen de datos 
 
**¿Cómo aparece este error?**: en Power BI, cuando se conecta al paquete de contenido de registros de actividad de Azure AD. 

| Causa | Solución |
| ---   | ---        |
| El usuario que se conecta no es un administrador global, un lector de seguridad o un administrador de seguridad. | Use una cuenta que sea administrador global, lector de seguridad o administrador de seguridad para acceder a los paquetes de contenido. |
| El inquilino no es Premium o no tiene al menos un usuario con un archivo de licencia Premium. | [Abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>La importación de datos es demasiado lenta 
 
**¿Cómo aparece este error?**: en Power BI, después de conectar el paquete de contenido, el proceso de importación de datos comienza a preparar su panel para los registros de actividad de Azure AD. Verá el mensaje: **Importando datos...**  sin ningún progreso adicional.  

| Causa | Solución |
| ---   | ---        |
| Según el tamaño de su inquilino, este paso puede tardar desde algunos minutos hasta media hora. | Si el mensaje no cambia para mostrar el panel dentro de una hora, [abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md).|
  
## <a name="next-steps"></a>Pasos siguientes

* [Instalación del paquete de contenido de Power BI](quickstart-install-power-bi-content-pack.md).
* [¿Qué son los informes de Azure AD?](overview-reports.md)
