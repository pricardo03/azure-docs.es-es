---
title: 'Tutorial: Archivar registros de Azure Active Directory en una cuenta de Azure Storage (versión preliminar) | Microsoft Docs'
description: Aprenda a configurar Azure Diagnostics para insertar los registros de Azure Active Directory en una cuenta de Azure Storage (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ea26da91e2a0b88e8afa54a4210b7a2365e4949b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240289"
---
# <a name="tutorial-archive-azure-active-directory-logs-to-an-azure-storage-account-preview"></a>Tutorial: Archivar registros de Azure Active Directory en una cuenta de Azure Storage (versión preliminar)

En este tutorial, aprenderá a configurar los valores de diagnóstico de Azure Monitor para enrutar los registros de Azure Active Directory en una cuenta de Azure Storage.

## <a name="prerequisites"></a>Requisitos previos 

Necesita:

* Una suscripción de Azure con una cuenta de Azure Storage. Si no tiene ninguna suscripción de Azure, puede [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/free/).
* Un inquilino de Azure Active Directory.
* Un usuario, que sea administrador global o administrador de seguridad en dicho inquilino.

## <a name="archive-logs-to-an-azure-storage-account"></a>Archivado de registros en una cuenta de Azure Storage

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 
2. Haga clic en **Azure Active Directory** -> **Actividad** -> **Registros de auditoría**. 
3. Haga clic en **Exportar configuración** para abrir la hoja Configuración de diagnóstico. Haga clic en **Editar configuración** si desea cambiar la configuración existente o en **Add diagnostic setting** (Agregar configuración de diagnóstico) si lo que desea es agregar una nueva. Puede tener un máximo de tres valores. 
    ![Exportar configuración](./media/reporting-azure-monitor-diagnostics-azure-storage-account/ExportSettings.png "Export settings")

4. Agregue un nombre descriptivo al valor que recuerde su finalidad. Por ejemplo, "Enviar a la cuenta de Azure Storage". 
5. Seleccione la casilla **Archive to a storage account** (Archivar en una cuenta de almacenamiento) y haga clic en **Cuenta de almacenamiento** para elegir la cuenta de Azure Storage. 
6. Seleccione la suscripción de Azure y la cuenta de almacenamiento a la que desea enrutar los registros a y haga clic en **Aceptar** para cerrar la configuración.
7. Seleccione la casilla **AuditLogs** para enviar los registros de auditoría a la cuenta de almacenamiento. 
8. Seleccione la casilla **SignInLogs** para enviar los registros de inicio de sesión a la cuenta de almacenamiento.
9. Use el control deslizante para establecer la retención de los datos del registro. De forma predeterminada, este valor es "0" y los registros se conservarán en la cuenta de almacenamiento de forma indefinida. De lo contrario, puede establecer un valor y se eliminarán todos los eventos que tengan más días de los seleccionados.
10. Haga clic en **Guardar** para guardar el valor.
    ![Configuración de diagnóstico](./media/reporting-azure-monitor-diagnostics-azure-storage-account/DiagnosticSettings.png "Diagnostic settings")

11. A los 15 minutos compruebe que los registros se han insertado en la cuenta de almacenamiento. Vaya a Azure Portal, haga clic en **Cuentas de almacenamiento**, elija la cuenta de almacenamiento que usó anteriormente y haga clic en **Blobs**. 
12. En **Registros de auditoría**, haga clic en **insights-log-audit**. En **Registros de inicio de sesión**, haga clic en **insights-log-audit**.
    ![Cuenta de almacenamiento](./media/reporting-azure-monitor-diagnostics-azure-storage-account/StorageAccount.png "Storage account")

## <a name="next-steps"></a>Pasos siguientes

* [Interpretación de un esquema de registros de auditoría en Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretación de un esquema de registros de inicio de sesión en Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Preguntas frecuentes y problemas conocidos](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)