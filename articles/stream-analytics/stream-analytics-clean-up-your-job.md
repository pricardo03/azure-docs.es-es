---
title: Limpiar un trabajo de Azure Stream Analytics
description: En este artículo se muestran distintos métodos para eliminar los trabajos de Azure Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: e43e1034abe4bbe3d31a46ab3b98b0efe612b852
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588727"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Limpiar un trabajo de Azure Stream Analytics

Los trabajos de Azure Stream Analytics se pueden eliminar fácilmente desde Azure Portal, Azure PowerShell, el SDK de Azure para .Net o la API de REST. Un trabajo de Stream Analytics no puede recuperarse una vez que se ha eliminado.

>[!NOTE] 
>Cuando termine su trabajo en Stream Analytics, los datos solo se conservarán en almacenaje de entrada y de salida, como Event Hubs o Azure SQL Database. Si se le solicita que elimine datos de Azure, asegúrese de seguir el proceso de supresión de los recursos de entrada y salida de su trabajo en Stream Analytics.

## <a name="stop-a-job-in-azure-portal"></a>Detener un trabajo en Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

2. Busque el trabajo de Stream Analytics que se está ejecutando y selecciónelo.

3. En la página del trabajo de Stream Analytics, seleccione **Detener** para detener el trabajo. 

   ![Detención de un trabajo de Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Eliminación un trabajo en Azure Portal

1. Inicie sesión en el Portal de Azure. 

2. Busque el trabajo de Stream Analytics existente y selecciónelo.

3. En la página del trabajo de Stream Analytics, seleccione **Eliminar** para eliminar el trabajo. 

   ![Eliminación de un trabajo de Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Detener o eliminar un trabajo mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para detener un trabajo mediante PowerShell, use el [Stop AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet. Para eliminar un trabajo mediante PowerShell, use el [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Detener o eliminar un trabajo mediante el SDK de Azure para .NET

Para detener un trabajo mediante el SDK de Azure para. NET, use el método [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet). Para eliminar un trabajo mediante el SDK de Azure para. NET, use el método [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet).

## <a name="stop-or-delete-a-job-using-rest-api"></a>Detener o eliminar un trabajo mediante la API de REST

Para detener un trabajo mediante la API de REST, consulte el método [Stop](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop). Para eliminar un trabajo mediante la API de REST, consulte el método [Delete](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete).
