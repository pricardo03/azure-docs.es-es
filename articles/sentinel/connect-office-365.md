---
title: Recopilar datos de Office 365 en Azure Sentinel Preview | Microsoft Docs
description: Obtenga información sobre cómo recopilar datos de Office 365 en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 7cd90b987550dc60b30d3aa0cd1016b681eec85f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57547042"
---
# <a name="collect-data-from-office-365-logs"></a>Recopilar datos de los registros de Office 365

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de auditoría de [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) en Azure Sentinel con un solo clic. Puede transmitir los registros de auditoría de varios inquilinos a un área de trabajo en Azure Sentinel. El conector de registro de actividad de Office 365 proporciona una visión de las actividades del usuario en curso. Obtendrá información sobre las distintas usuario, admin, sistema y las acciones de directiva y los eventos de Office 365. Al conectar los registros de Office 365 en Azure Sentinel puede usar estos datos para ver los paneles, crear alertas personalizadas y mejorar el proceso de investigación.


## <a name="prerequisites"></a>Requisitos previos

- Debe ser un administrador global o administrador de seguridad en el inquilino
- Para agregar a un inquilino, abrir el puerto 4433 al tráfico web en el equipo desde el que se realice la conexión.

## <a name="connect-to-office-365"></a>Conectarse a Office 365

1. En Azure Sentinel, seleccione **la recopilación de datos** y, a continuación, haga clic en el **Office 365** icono.

2. Si no ya ha habilitado, en **conexión** utilizar el **habilitar** botón para habilitar la solución de Office 365. Si ya se ha habilitado, se identificarán en la pantalla de conexión que como ya se ha habilitado.
1. Office 365 le permite transmitir datos de varios inquilinos para Azure Sentinel. Para cada inquilino que desea conectarse, agregue el inquilino en **conectan los inquilinos a Azure Sentinel**. 
1. Se abre una pantalla de Active Directory. Se le solicite para autenticar con un usuario de administrador global de cada inquilino que desea conectarse a Azure Sentinel y proporcionar permisos a Azure Centinela para leer sus registros. 
5. En los registros de actividad Stream Office 365, haga clic en **seleccione** para elegir qué tipos de registro que desea transmitir a Azure Sentinel. Actualmente, Azure Sentinel es compatible con Exchange y SharePoint.

4. Haga clic en **aplicar cambios**.

3. Para usar el esquema correspondiente en Log Analytics para los registros de Office 365, busque **OfficeActivity**.


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Office 365 con Azure Sentinel. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

