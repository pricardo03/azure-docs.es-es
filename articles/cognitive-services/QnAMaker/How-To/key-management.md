---
title: 'Administración de claves: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Cómo administrar las claves de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b402187f4949dac34fa476648c81b980ba3efc96
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381810"
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
