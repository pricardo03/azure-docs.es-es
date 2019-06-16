---
title: 'Colaboración en la base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker permite que varias personas colaboren en una base de conocimiento. Esta característica se proporciona con el Control de acceso basado en rol de Azure.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.openlocfilehash: ca754f197a46fc41b6f1b432611a2177ec0afafa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61374859"
---
# <a name="collaborate-on-your-knowledge-base"></a>Colaboración en su base de conocimiento

QnA Maker permite que varias personas colaboren en una base de conocimiento. Esta característica se proporciona con el [Control de acceso basado en rol](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) de Azure. 

Realice los pasos siguientes para compartir su servicio QnA Maker con otra persona:

1. Inicie sesión en Azure Portal y vaya al recurso QnA Maker.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Vaya a la pestaña **Access Control (IAM)** .

    ![QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Seleccione **Agregar**.

    ![Agregar QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Seleccione los roles **Propietario** o **Colaborador**. No se puede conceder acceso de solo lectura mediante Control de acceso basado en rol. El rol de colaborador y propietario tiene derecho de acceso de lectura y escritura al servicio QnA Maker.

    ![Agregar rol QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Escriba el correo electrónico de la persona con quien desea compartirlo y presione Guardar.

    ![Agregar correo electrónico de QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Ahora, cuando la persona con quien comparte el servicio QnA Maker inicia sesión en el [portal de QnA Maker](https://qnamaker.ai), puede ver todas las bases de conocimiento de dicho servicio.

Recuerde que no puede compartir una base de conocimiento particular en un servicio QnA Maker. Si desea un control de acceso más pormenorizado, considere la distribución de las bases de conocimiento entre diferentes servicios QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Probar una base de conocimiento](./test-knowledge-base.md)
