---
title: Instalación del paquete de contenido de Power Bi para Azure AD | Microsoft Docs
description: Aprenda a instalar el paquete de contenido de Power BI para Azure AD
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: fd5604eb-1334-4bd8-bfb5-41280883e2b5
ms.service: active-directory
ms.workload: identity
ms.component: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: cd5da2a3c5fa3d9cd67715493eb649926badb694
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622541"
---
# <a name="quickstart-install-azure-active-directory-power-bi-content-pack"></a>Guía de inicio rápido: Instalación del paquete de contenido de Power BI para Azure Active Directory

|  |
|--|
|Actualmente, el paquete de contenido de Power BI de Azure AD usa las Graph API de Azure AD para recuperar datos del inquilino de Azure AD. Como resultado, es posible que vea alguna discrepancia entre los datos disponibles en el paquete de contenido y los datos recuperados con la [API de Microsoft Graph para crear informes](concept-reporting-api.md). |
|  |

El paquete de contenido de Power BI para Azure Active Directory (Azure AD) le ofrece la posibilidad de visualizar los datos de informes de su entorno. Puede descargar el paquete de contenido precompilado y usarlo para informar de todas las actividades dentro de su directorio mediante la rica experiencia de visualización que ofrece Power BI. También puede crear su propio panel y compartirlo fácilmente con cualquier persona de su organización. 

En este tutorial, aprenderá a instalar el paquete de contenido.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

* Una cuenta de Power BI Esta es la misma cuenta que la cuenta de Office 365 o Azure AD. 
* El identificador de inquilino de Azure AD. Es el **identificador del directorio** de la [página de propiedades](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) de Azure Portal.
* Una licencia de Azure AD Premium (P1 y P2). 

## <a name="install-azure-ad-power-bi-content-pack"></a>Instalación del paquete de contenido de Power BI para Azure AD 

1. Inicie sesión en [Power BI](https://app.powerbi.com/groups/me/getdata/services) con su cuenta de Power BI. Esta es la misma cuenta que la cuenta de Office 365 o Azure AD.

2. Busque **Azure Active Directory Activity Logs** (Registros de actividad de Azure Active Directory) en la página **Aplicaciones** y seleccione **Obtenerla ahora**. 

   ![Paquete de contenido de Power BI de Azure Active Directory](./media/quickstart-install-power-bi-content-pack/getitnow.png) 
    
3. En la ventana emergente, escriba el identificador de inquilino de Azure AD, escriba **7** como el número de días para consultar y, luego, seleccione **Siguiente**.
    
   ![Paquete de contenido de Power BI de Azure Active Directory](./media/quickstart-install-power-bi-content-pack/connect.png) 

4. Cuando se haya creado el panel de registros de actividad de Azure Active Directory, selecciónelo.

   ![Paquete de contenido de Power BI de Azure Active Directory](./media/quickstart-install-power-bi-content-pack/dashboard.png) 
    
## <a name="next-steps"></a>Pasos siguientes

* [Uso del paquete de contenido de Power BI](howto-power-bi-content-pack.md)
* [Solución de errores de los paquetes de contenido](troubleshoot-content-pack.md)
