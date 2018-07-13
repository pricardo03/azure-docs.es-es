---
title: 'Colaboración en su base de conocimiento: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Colaboración en la base de conocimiento de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e18d656236276595fc5186a6656349bf28974ead
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381726"
---
# <a name="collaborate-on-your-knowledge-base"></a>Colaboración en su base de conocimiento

QnA Maker permite que varias personas colaboren en una base de conocimiento. Esta característica se proporciona con el [Control de acceso basado en rol](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) de Azure. 

Realice los pasos siguientes para compartir su servicio QnA Maker con otra persona:

1. Inicie sesión en Azure Portal y vaya al recurso QnA Maker.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Vaya a la pestaña **Access Control (IAM)**.

    ![QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Seleccione **Agregar**.

    ![Agregar QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Seleccione los roles **Propietario** o **Colaborador**.

    ![Agregar rol QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Escriba el correo electrónico de la persona con quien desea compartirlo y presione Guardar.

    ![Agregar correo electrónico de QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Ahora, cuando la persona con quien comparte el servicio QnA Maker inicia sesión en el [portal de QnA Maker](https://qnamaker.ai), puede ver todas las bases de conocimiento de dicho servicio.

Recuerde que no puede compartir una base de conocimiento particular en un servicio QnA Maker. Si desea un control de acceso más pormenorizado, considere la distribución de las bases de conocimiento entre diferentes servicios QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Probar una base de conocimiento](./test-knowledge-base.md)
