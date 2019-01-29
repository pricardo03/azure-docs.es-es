---
title: 'Guía de inicio rápido: Descarga de un informe de auditoría con Azure Portal | Microsoft Docs'
description: Obtenga información sobre cómo descargar un informe de auditoría mediante Azure Portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: afa92c40bd3cebea1024cf783a746450d3430a8d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821846"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Inicio rápido: Descarga de un informe de auditoría mediante Azure Portal

En esta guía de inicio rápido, obtendrá información sobre cómo descargar registros de auditoría para el inquilino durante las 24 últimas horas. Puede descargar hasta 5000 registros de Azure Portal. Los registros se ordenan a partir de los más recientes, así que, de forma predeterminada, se obtienen los últimos 5000 registros. 

## <a name="prerequisites"></a>Requisitos previos

Necesita:

* Un inquilino de Azure Active Directory. 
* Un usuario, que tenga el rol **Administrador de seguridad**, **Lector de seguridad** o **Administrador global** del inquilino. Además, cualquier usuario del inquilino puede acceder a sus propios registros de auditoría.

## <a name="quickstart-download-an-audit-report"></a>Inicio rápido: Descarga de un informe de auditoría

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Seleccione **Azure Active Directory** desde el panel de navegación izquierdo y use el botón **Cambiar directorio** para seleccionar su directorio activo.
3. En el panel, seleccione **Azure Active Directory** y luego **Registros de auditoría**. 
4. Elija **Últimas 24 horas** en el menú desplegable de filtro **Intervalo de fechas** y seleccione **Aplicar** para ver los registros de auditoría de las 24 últimas horas. 
5. Seleccione el botón **Descargar** para descargar un archivo CSV que contiene los registros filtrados. 

![Informes](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Pasos siguientes

* [Informes de actividad de inicio de sesión en el portal de Azure Active Directory](concept-sign-ins.md)
* [Retención de informes de Azure Active Directory](reference-reports-data-retention.md)
* [Latencias de informes de Azure Active Directory](reference-reports-latencies.md)
