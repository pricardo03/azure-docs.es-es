---
title: Limpiar un trabajo de Azure Stream Analytics
description: Este artículo es una guía sobre cómo eliminar trabajos de Azure Stream Analytics.
services: stream-analytics
author: mamccrea
manager: kfile
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 4773f542f12ae1773e881106bc8948c663bfd1e3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660412"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Limpiar un trabajo de Azure Stream Analytics

Los trabajos de Azure Stream Analytics se pueden eliminar fácilmente desde Azure Portal, Azure PowerShell, el SDK de Azure para .Net o la API de REST.

>[!NOTE] 
>Cuando termine su trabajo en Stream Analytics, los datos solo se conservarán en almacenaje de entrada y de salida, como Event Hubs o Azure SQL Database. Si se le solicita que elimine datos de Azure, asegúrese de seguir el proceso de supresión de los recursos de entrada y salida de su trabajo en Stream Analytics.

## <a name="stop-a-job-in-azure-portal"></a>Detener un trabajo en Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

2. Busque el trabajo de Stream Analytics que se está ejecutando y selecciónelo.

3. En la página del trabajo de Stream Analytics, seleccione **Detener** para detener el trabajo. 

   ![Detener trabajo](./media/stream-analytics-clean-up-your-job/stop-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Eliminación un trabajo en Azure Portal

1. Inicie sesión en el Portal de Azure. 

2. Busque el trabajo de Stream Analytics existente y selecciónelo.

3. En la página del trabajo de Stream Analytics, seleccione **Eliminar** para eliminar el trabajo. 

   ![Eliminar trabajo](./media/stream-analytics-clean-up-your-job/delete-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Detener o eliminar un trabajo mediante PowerShell

Para detener un trabajo con PowerShell, use el cmdlet [AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0). Para eliminar un trabajo con PowerShell, use el cmdlet [Remove-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0).

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Detener o eliminar un trabajo mediante el SDK de Azure para .NET

Para detener un trabajo mediante el SDK de Azure para. NET, use el método [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet). Para eliminar un trabajo mediante el SDK de Azure para. NET, use el método [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet).

## <a name="stop-or-delete-a-job-using-rest-api"></a>Detener o eliminar un trabajo mediante la API de REST

Para detener un trabajo mediante la API de REST, consulte el método [Stop](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#stop). Para eliminar un trabajo mediante la API de REST, consulte el método [Delete](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#delete).