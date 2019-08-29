---
title: 'Colaboración en la base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker permite que varias personas colaboren en una base de conocimiento. Esta característica se proporciona con el Control de acceso basado en rol de Azure.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: d9c91d54fb357807682cd57f46b04454e4e2cfec
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876666"
---
# <a name="collaborate-on-your-knowledge-base"></a>Colaboración en su base de conocimiento

QnA Maker permite que varias personas colaboren en una base de conocimiento. Esta característica se proporciona con el [Control de acceso basado en rol](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) de Azure. 

Realice los pasos siguientes para compartir su servicio QnA Maker con otra persona:

1. Inicie sesión en Azure Portal y vaya al recurso QnA Maker.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

1. Vaya a la pestaña **Access Control (IAM)** .

    ![QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

1. Seleccione **Agregar**.

    ![Agregar QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

1. Seleccione los roles **Propietario** o **Colaborador**. No se puede conceder acceso de solo lectura mediante Control de acceso basado en rol. Los roles propietario y colaborador tienen permisos de acceso de lectura y escritura al servicio QnA Maker.

    ![Agregar rol QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

1. Escriba la dirección de correo electrónico del usuario y presione **Guardar**.

    ![Agregar correo electrónico de QnA Maker (IAM)](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Cuando la persona con quien comparte el servicio QnA Maker inicia sesión en el [portal de QnA Maker](https://qnamaker.ai), puede ver todas las bases de conocimiento de dicho servicio.

Recuerde que no puede compartir una base de conocimiento particular en un servicio QnA Maker. Si desea un control de acceso más pormenorizado, considere la distribución de las bases de conocimiento entre diferentes servicios QnA Maker.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Probar una base de conocimiento](./test-knowledge-base.md)
