---
title: 'Guía de inicio rápido: Descarga de un informe de inicio de sesión con Azure Portal | Microsoft Docs'
description: Obtenga información sobre cómo descargar un informe de inicio de sesión mediante Azure Portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39f3f0954a271f3abd30e99e306ebfc940e20a7f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57547816"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Inicio rápido: Descarga de un informe de inicio de sesión mediante Azure Portal

En esta guía de inicio rápido, obtendrá información sobre cómo descargar los datos de inicio de sesión para el inquilino durante las 24 últimas horas. Puede descargar hasta 250 000 registros de Azure Portal. Los registros se ordenan a partir de los más recientes, así que, de forma predeterminada, se obtienen los 250 000 últimos registros. 

## <a name="prerequisites"></a>Requisitos previos

Necesita:

* Un inquilino de Azure Active Directory, con una licencia Premium para ver el informe actividad de inicio de sesión. Consulte [Introducción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para actualizar la edición de Azure Active Directory. Tenga en cuenta que si no tuvo ningún dato de actividades antes de la actualización, los datos tardarán un par de días en mostrarse en los informes después de actualizar a una licencia premium.
* Un usuario, que tenga el rol **Administrador de seguridad**, **Lector de seguridad**, **Lector de informes** o **Administrador global** del inquilino. Además, cualquier usuario del inquilino puede acceder a sus propios inicios de sesión.

## <a name="quickstart-download-a-sign-in-report"></a>Inicio rápido: Descarga de un informe de inicio de sesión

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Seleccione **Azure Active Directory** desde el panel de navegación izquierdo y use el botón **Cambiar directorio** para seleccionar su directorio activo.
3. En el panel, seleccione **Azure Active Directory** y luego **Inicios de sesión**. 
4. Elija **Últimas 24 horas** en el menú desplegable de filtro **Fecha** y seleccione **Aplicar** para ver los inicios de sesión de las 24 últimas horas. 
5. Seleccione el botón **Descargar**, elija **CSV** como el formato de archivo y especifique un nombre de archivo para descargar un archivo CSV que contiene los registros filtrados. 

![Informes](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Pasos siguientes

* [Informes de actividad de inicio de sesión en el portal de Azure Active Directory](concept-sign-ins.md)
* [Retención de informes de Azure Active Directory](reference-reports-data-retention.md)
* [Latencias de informes de Azure Active Directory](reference-reports-latencies.md)
