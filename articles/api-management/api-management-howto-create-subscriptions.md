---
title: Cómo crear suscripciones en Azure API Management | Microsoft Docs
description: Más información acerca de cómo crear suscripciones en Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 3f4e113125ec9644aac974e47996afe290e57cee
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621735"
---
# <a name="how-to-create-subscriptions-in-azure-api-management"></a>Cómo crear suscripciones en Azure API Management

Al publicar las API a través de Azure API Management (APIM), la forma más fácil y habitual de proteger el acceso a esas API es mediante el uso de claves de suscripción. Es decir, las aplicaciones cliente que necesiten usar las API publicadas deberán incluir una clave de suscripción válida en las solicitudes HTTP al realizar llamadas a esas API. Para obtener una clave de suscripción para tener acceso a las API, es necesaria una suscripción. Para obtener más información sobre las suscripciones, consulte [Suscripciones en Azure API Management](api-management-subscriptions.md)

Este artículo le guiará por los pasos necesarios para crear suscripciones en Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, necesitará lo siguiente:

+ [Creación de una instancia de APIM](get-started-create-service-instance.md)
+ Descripción de las [suscripciones de APIM](api-management-subscriptions.md)

## <a name="create-a-new-subscription"></a>Creación de una nueva suscripción

1. Haga clic en **Suscripciones** en el menú de la izquierda
2. Haga clic en **Agregar suscripción**
3. Proporcione un nombre de la suscripción y seleccione el ámbito
4. Haga clic en **Guardar**

![Suscripciones flexibles](./media/api-management-subscriptions/flexible-subscription.png)

Una vez creada la suscripción, un par de claves de API (principal y secundaria) se aprovisionan para tener acceso a las API.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre API Management:

+ Aprenda otros [conceptos](api-management-terminology.md) en API Management
+ Siga nuestros [tutoriales](import-and-publish.md) para obtener más información sobre API Management
+ Consulte nuestra [página de preguntas más frecuentes](api-management-faq.md) para ver las preguntas habituales