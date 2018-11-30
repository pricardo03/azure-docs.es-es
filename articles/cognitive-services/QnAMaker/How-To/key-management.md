---
title: 'Administración de claves y recursos: QnA Maker'
titleSuffix: Azure Cognitive Services
description: 'El servicio QnA Maker trabaja con dos tipos de claves: claves de suscripción y claves de punto de conexión.'
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/26/2018
ms.author: tulasim
ms.openlocfilehash: 1fe39cf9e4a1e58a0d37cc8ebf9fe02c6ad7eed5
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334625"
---
# <a name="key-management"></a>Administración de claves

El servicio QnA Maker trabaja con dos tipos de claves, **claves de suscripción** y **claves de punto de conexión**.

![Administración de claves](../media/qnamaker-how-to-key-management/key-management.png)

1. **Claves de suscripción**: estas claves se usan para acceder a las [API del servicio de administración de QnA Maker](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Estas API permiten realizar diversas operaciones CRUD en la base de conocimiento.  

2. **Claves de punto de conexión**: estas claves se usan para tener acceso al punto de conexión de la base de conocimiento para obtener una respuesta para una pregunta de usuario. Normalmente se usaría este punto de conexión en el bot de chat o el código de aplicación que consume el servicio QnA Maker.
 
## <a name="subscription-keys"></a>Claves de suscripción
Puede ver y restablecer las claves de suscripción desde Azure Portal, donde creó el recurso QnA Maker. 
1. Vaya al recurso QnA Maker en Azure Portal.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Vaya a **Claves**.

    ![clave de suscripción](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Claves de punto de conexión

Las claves de punto de conexión se pueden administrar desde el [portal de QnA Maker](https://qnamaker.ai).

1. Inicie sesión en el [portal de QnA Maker](https://qnamaker.ai) y vaya a **Administrar claves**.

    ![Clave de punto de conexión](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Vea o restablezca las claves.

    ![Administrador de claves de punto de conexión](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Actualice las claves si considera que se han visto comprometidas. Esto puede requerir realizar los cambios correspondientes en el código del bot o de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear una base de conocimiento en un idioma diferente](./language-knowledge-base.md)
