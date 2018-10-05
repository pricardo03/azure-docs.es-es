---
title: 'Solución de problemas: QnAMaker'
titlesuffix: Azure Cognitive Services
description: QnAMaker está compuesto por componentes que se hospedan en la cuenta de usuario Azure. Para la depuración puede ser necesario que los usuarios manipulen sus recursos de QnAMaker en Azure o que proporcionen al equipo de soporte técnico de QnAMaker información adicional sobre su instalación.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: ad3c01ebf37c8b544830b281144090694eeadfcd
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033741"
---
# <a name="qnamaker-troubleshooting"></a>Solución de problemas de QnAMaker
QnAMaker está compuesto por componentes que se hospedan en la cuenta de usuario Azure. Para la depuración puede ser necesario que los usuarios manipulen sus recursos de QnAMaker en Azure o que proporcionen al equipo de soporte técnico de QnAMaker información adicional sobre su instalación.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Cómo obtener las actualizaciones más recientes del sistema de tiempo de ejecución de QnAMaker
El sistema de tiempo de ejecución de QnAMaker forma parte del servicio Azure App Service, que se implementa cuando se crea [un servicio de QnAMaker](./set-up-qnamaker-service-azure.md) en Azure Portal. El sistema de tiempo de ejecución se actualiza periódicamente. Para aplicar las actualizaciones más recientes a su instalación de QnAMaker, debe reiniciar App Service.
1. Vaya a su servicio QnAMaker (grupo de recursos) en [Azure Portal](https://portal.azure.com)

    ![Grupo de recursos de QnAMaker en Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Haga clic en la instancia de App Service y abra la sección Información general

     ![QnAMaker en App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Reinicie App Service. Deberá completarse en un par de segundos. Tenga en cuenta que las aplicaciones o bots de niveles inferiores que utilicen este servicio QnAMaker dejarán de estar disponibles para los usuarios finales durante el reinicio.

    ![Reiniciar QnAMaker en App Service](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Cómo obtener el nombre del host de servicios de QnAMaker
El nombre del host de servicios de QnAMaker es útil para fines de depuración cuando se ponga en contacto con el soporte técnico de QnAMaker o UserVoice. El nombre de host es una dirección URL con este formato: https://*{nombre de host}*.azurewebsites.net.
    
1. Vaya a su servicio QnAMaker (grupo de recursos) en [Azure Portal](https://portal.azure.com)

    ![Grupo de recursos de QnAMaker en Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Haga clic en la instancia de App Service.

     ![QnAMaker en App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. La dirección URL del nombre de host está disponible en la sección Información general

    ![Nombre de host de QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Uso de QnAMaker API](./upgrade-qnamaker-service.md)
