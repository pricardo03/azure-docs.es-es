---
title: Conecte los datos de Office 365 a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de Office 365 con Azure Sentinel.
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
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 77587b0b7506ef0ccadbeb6d1f010f5b6a72d93e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59793610"
---
# <a name="connect-data-from-office-365-logs"></a>Conectar los datos de los registros de Office 365

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de auditoría de [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) en Azure Sentinel con un solo clic. Puede transmitir los registros de auditoría de varios inquilinos a un área de trabajo en Azure Sentinel. El conector de registro de actividad de Office 365 proporciona una visión de las actividades del usuario en curso. Obtendrá información sobre las distintas usuario, admin, sistema y las acciones de directiva y los eventos de Office 365. Al conectar los registros de Office 365 en Azure Sentinel puede usar estos datos para ver los paneles, crear alertas personalizadas y mejorar el proceso de investigación.


## <a name="prerequisites"></a>Requisitos previos

- Debe ser un administrador global o administrador de seguridad en el inquilino
- En el equipo, desde el que ha iniciado sesión en Azure Centinela para crear la conexión, asegúrese de que el puerto 4433 está abierto para el tráfico web.

## <a name="connect-to-office-365"></a>Conectarse a Office 365

1. En Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **Office 365** icono.

2. Si no ya ha habilitado, en **conexión** utilizar el **habilitar** botón para habilitar la solución de Office 365. Si ya se ha habilitado, se identificarán en la pantalla de conexión que como ya se ha habilitado.
1. Office 365 le permite transmitir datos de varios inquilinos para Azure Sentinel. Para cada inquilino que desea conectarse, agregue el inquilino en **conectan los inquilinos a Azure Sentinel**. 
1. Se abre una pantalla de Active Directory. Se le solicite para autenticar con un usuario de administrador global de cada inquilino que desea conectarse a Azure Sentinel y proporcionar permisos a Azure Centinela para leer sus registros. 
5. En los registros de actividad Stream Office 365, haga clic en **seleccione** para elegir qué tipos de registro que desea transmitir a Azure Sentinel. Actualmente, Azure Sentinel es compatible con Exchange y SharePoint.

4. Haga clic en **aplicar cambios**.

3. Para usar el esquema correspondiente en Log Analytics para los registros de Office 365, busque **OfficeActivity**.


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Office 365 con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

