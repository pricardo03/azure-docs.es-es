---
title: Conectar datos de Office 365 a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Office 365 a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: 0013540bf0ca921b2f41260dea185f6aa32567d7
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679262"
---
# <a name="connect-data-from-office-365-logs"></a>Conectar datos de registros de Office 365

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se pueden transmitir registros de auditoría desde [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) a Azure Sentinel con tan solo un clic. Se pueden transmitir registros de auditoría pertenecientes a varios inquilinos a una misma área de trabajo en Azure Sentinel. El conector de registro de actividades de Office 365 proporciona información sobre las actividades de usuario en curso. Obtendrá información sobre distintas acciones de los usuarios, los administradores, las directivas o el sistema, así como de eventos de Office 365. Al conectar registros de Office 365 a Azure Sentinel, estos datos se pueden usar para ver paneles, crear alertas personalizadas y mejorar el proceso de investigación.

> [!IMPORTANT]
> Si tiene una licencia de E3, antes de poder acceder a los datos a través de la API de administración de Office 365, debe habilitar el registro de auditoría unificado para la organización de Office 365. Para ello, active el registro de auditoría de Office 365. Para obtener instrucciones, consulte [Activar o desactivar la búsqueda de registros de auditoría de Office 365](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Para más información, consulte [Referencia de la API de Actividad de administración de Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Requisitos previos

- Debe ser administrador global o administrador de seguridad en el inquilino.
- En el equipo en el que se ha iniciado sesión en Azure Sentinel para crear la conexión, asegúrese de que el puerto 4433 está abierto al tráfico web.

## <a name="connect-to-office-365"></a>Conectarse a Office 365

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Office 365**.

2. Si aún no ha habilitado la solución de Office 365, en **Connection** (Conexión), use el botón **Enable** (Habilitar) para habilitarla. Si ya está habilitada, se identificará en la pantalla de conexión como ya habilitada.
1. Office 365 permite transmitir datos de varios inquilinos a Azure Sentinel. Agregue cada inquilino que quiera conectar a **Connect tenants to Azure Sentinel** (Conectar inquilinos a Azure Sentinel). 
1. Se abre una pantalla de Active Directory. Se le pedirá que se autentique como administrador global en cada inquilino que quiera conectar a Azure Sentinel, y deberá proporcionar permisos a Azure Sentinel para leer los registros. 
5. En Stream Office 365 activity logs (Transmitir registros de actividad de Office 365), haga clic en **Select** (Seleccionar) para elegir qué tipos de registro se van a transmitir a Azure Sentinel. Actualmente, Azure Sentinel admite Exchange y SharePoint.

4. Haga clic en **Apply changes** (Aplicar cambios).

3. Para usar el esquema correspondiente en Log Analytics para encontrar registros de Office 365, busque **OfficeActivity**.


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Office 365 a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

