---
title: Uso del paquete de contenido de Power BI de Azure Active Directory | Microsoft Docs
description: Aprenda a usar el paquete de contenido de Power BI de Azure Active Directory.
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c0326a6b611d5f3d5633db2d2b64b8cdc15e10a7
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816691"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Uso del paquete de contenido de Power BI de Azure Active Directory

|  |
|--|
|Actualmente, el paquete de contenido de Power BI de Azure AD usa las Graph API de Azure AD para recuperar datos del inquilino de Azure AD. Como resultado, es posible que vea alguna discrepancia entre los datos disponibles en el paquete de contenido y los datos recuperados con la [API de Microsoft Graph para crear informes](concept-reporting-api.md). |
|  |

Los administradores de TI deben conocer la forma en que los usuarios adoptan y usan las características de Azure Active Directory. Esto permite planear la infraestructura de TI y la comunicación para aumentar la utilización y sacar el máximo partido de las características de Azure AD. El paquete de contenido de Power BI para Azure Active Directory le brinda la posibilidad de analizar en mayor profundidad los datos para obtener más información sobre lo que pasa en su directorio. Con la integración de las API de Azure Active Directory en Power BI, puede descargar fácilmente el paquete de contenido generado previamente y obtener información de todas las actividades de Azure Active Directory gracias a las magnificas características de visualización que ofrece Power BI. Puede crear su propio panel y compartirlo fácilmente con cualquier persona de su organización. 

## <a name="prerequisites"></a>Requisitos previos

Para usar el paquete de contenido necesita una licencia Premium de Azure AD (P1 y P2). 

## <a name="install-the-content-pack"></a>Instalación del paquete de contenido

Consulte la [guía de inicio rápido](quickstart-install-power-bi-content-pack.md) para instalar el paquete de contenido de Azure AD Power BI.

## <a name="what-can-i-do-with-this-content-pack"></a>¿Qué puedo hacer con este paquete de contenido?

Antes de explicar qué puede hacer con este paquete de contenido, aquí tiene un breve resumen de los distintos informes que incluye el paquete de contenido. Los datos de los informes cubren los **últimos 30 días**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Informes incluidos en esta versión del paquete de contenido de registros de Azure Active Directory

**Informe de utilización y tendencias de aplicaciones**: obtenga información sobre las aplicaciones que se usan en su organización y cuáles se usan más y cuándo. Este informe se puede usar para recopilar información sobre cómo se usa una aplicación que acabe de implementar en su organización o para averiguar qué aplicaciones son populares. Al hacerlo, puede mejorar la utilización si ve si no se usa la aplicación.

**Inicios de sesión por ubicación y usuarios**: obtenga información sobre todos los inicios de sesión llevados a cabo con la identidad de Azure y sobre la identidad de los usuarios. Con esto, puede profundizar en los inicios de sesión individuales y responder a preguntas como:

- ¿Desde dónde inició sesión este usuario?
- ¿Qué usuario tiene la mayor cantidad de inicios de sesión y desde dónde inicia sesión? 
- ¿Fue el inicio de sesión correcto?  
 
Para profundizar en los detalles, haga clic en una fecha o ubicación concretas.

**Usuarios únicos por aplicación**: obtenga una vista de todos los usuarios únicos de una aplicación determinada. Esto incluye solo los usuarios que hayan iniciado sesión "*correctamente*" en una aplicación.

**Inicios de sesión de dispositivo**: obtenga una vista del tipo de sistema operativo y los exploradores que los usuarios están utilizando en su organización con información detallada sobre los usuarios, lo que incluye:

- User Name
- Dirección IP
- Ubicación 
- Estado de inicio de sesión 

Con este informe, puede comprender los diversos perfiles de dispositivo usados dentro de su organización y determinar directivas de dispositivo en función de lo que se use.

**Embudo de SSPR**: obtenga una descripción de cómo se llevan a cabo los restablecimientos de contraseña en su organización. Eche un vistazo a cuántos restablecimientos de contraseña se intentaron con la herramienta SSPR y cuántos se realizaron correctamente. Profundice más en los restablecimientos de contraseña con error mediante el embudo de SSPR y comprenda por qué se produjeron determinados errores. Este informe ofrece una visión más detallada de cómo se usa la herramienta SSPR dentro de su organización para que pueda tomar las decisiones correctas.

## <a name="customizing-azure-ad-activity-content-pack"></a>Personalización del paquete de contenido de actividad de Azure AD

**Cambio de la visualización**: para cambiar la visualización de un informe, haga clic en **Editar informe** y seleccione la visualización que desee.
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/09.png) 
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/10.png) 

**Inclusión de campos adicionales**: puede agregar un campo al informe o quitarlo seleccionando el objeto visual que desea agregar o quitar en el campo. En el ejemplo siguiente, se agrega el campo "Estado de inicio de sesión" a la vista de tabla. 
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/11.png) 

**Anclado de visualizaciones al panel**: puede personalizar el panel e incluir sus propias visualizaciones en el informe y anclarlo al panel. En el ejemplo siguiente, se agrega un filtro nuevo denominado "Estado de inicio de sesión" y se incluye en el informe. También se cambia la visualización de gráfico de barras a gráfico de líneas y se puede anclar este nuevo objeto visual al panel.

![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/12.png) 

![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/13.png) 
 

 


**Compartir su panel**: una vez que haya creado el contenido que desea, puede compartir el panel con los usuarios de su organización. Recuerde que, una vez que comparta el informe, pueden ver los campos que haya seleccionado en el informe.
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Programación de una actualización diaria de su informe de Power BI

Para programar una actualización diaria de su informe de Power BI, vaya a **Conjuntos de datos > Configuración > Programar actualización** y configúrelo como se muestra a continuación.
 
![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>Actualización a una versión más reciente del paquete de contenido

Si desea actualizar el paquete de contenido para obtener una versión más reciente:

- Descargue el nuevo paquete de contenido y configúrelo según las instrucciones de este artículo.

- Una vez configurado, vaya a **Origen de datos > Configuración > Credenciales de origen de datos** y vuelva a escribir sus credenciales, tal y como se muestra a continuación.

    ![Paquete de contenido de Power BI de Azure Active Directory](./media/howto-power-bi-content-pack/16.png) 

Tan pronto como la nueva versión del paquete de contenido esté funcionando, puede quitar la versión anterior, si es necesario, eliminando los informes subyacentes y los conjuntos de datos asociados con ese paquete de contenido.

## <a name="still-having-issues"></a>¿Sigue teniendo problemas? 

Consulte nuestra [guía de solución de problemas](troubleshoot-content-pack.md). Para obtener ayuda general con Power BI, visite estos [artículos de Ayuda](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Pasos siguientes

* [Instalación del paquete de contenido de Power BI](quickstart-install-power-bi-content-pack.md).
* [Solución de errores de los paquetes de contenido](troubleshoot-content-pack.md)
* [¿Qué son los informes de Azure AD?](overview-reports.md)
