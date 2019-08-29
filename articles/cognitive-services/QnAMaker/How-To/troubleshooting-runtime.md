---
title: 'Solución de problemas: QnAMaker'
titleSuffix: Azure Cognitive Services
description: QnAMaker está compuesto por componentes que se hospedan en la cuenta de usuario Azure. Para la depuración puede ser necesario que los usuarios manipulen sus recursos de QnAMaker en Azure o que proporcionen al equipo de soporte técnico de QnAMaker información adicional sobre su instalación.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b58a08c4cfa97356be4064456b2e9ec5cceccfbc
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876336"
---
# <a name="troubleshooting-tips-to-support-the-qna-maker-service-and-runtime"></a>Sugerencias para solucionar problemas con la compatibilidad del servicio QnA Maker y el entorno de ejecución

QnAMaker está compuesto por recursos que se hospedan en la suscripción de Azure del usuario. Para la depuración puede ser necesario que los usuarios manipulen sus recursos de Azure QnAMaker o que proporcionen al equipo de soporte técnico de QnAMaker información adicional sobre su instalación.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Cómo obtener las actualizaciones más recientes del sistema de tiempo de ejecución de QnAMaker

El runtime de QnAMaker forma parte de la instancia de Azure App Service implementada al [crear un servicio QnAMaker](./set-up-qnamaker-service-azure.md) en Azure Portal. El sistema de tiempo de ejecución se actualiza periódicamente. App Service de QnA Maker está en modo de actualización automática tras la versión de extensión de sitio de abril de 2019 (versión 5+). Este servicio ya está diseñado para administrar el tiempo de inactividad CERO durante las actualizaciones. 

Puede consultar su versión actual en https://www.qnamaker.ai/UserSettings. Si su versión es anterior a la versión 5.x, debe reiniciar App Service para aplicar las últimas actualizaciones.

1. Vaya a su servicio QnAMaker (grupo de recursos) en [Azure Portal](https://portal.azure.com)

    ![Grupo de recursos de QnAMaker en Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Haga clic en la instancia de App Service y abra la sección Información general

     ![QnAMaker en App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Reinicie App Service. Deberá completarse en un par de segundos. Tenga en cuenta que las aplicaciones o bots dependientes que usen este servicio QnAMaker dejan de estar disponibles para los usuarios finales durante el periodo de reinicio.

    ![Reiniciar QnAMaker en App Service](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Cómo obtener el nombre del host de servicios de QnAMaker
El nombre del host de servicios de QnAMaker es útil para fines de depuración cuando se ponga en contacto con el soporte técnico de QnAMaker o UserVoice. El nombre de host es una dirección URL con este formato: https:// *{nombre de host}* .azurewebsites.net.
    
1. Vaya a su servicio QnAMaker (grupo de recursos) en [Azure Portal](https://portal.azure.com)

    ![Grupo de recursos de Azure para QnAMaker en Azure Portal](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Seleccione la instancia de App Service asociada al recurso de QnA Maker. Normalmente, los nombres son los mismos.

     ![Selección del App Service QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. La dirección URL del nombre de host está disponible en la sección Información general

    ![Nombre de host de QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Mejora de las preguntas de la base de conocimiento con Active Learning](./improve-knowledge-base.md)
