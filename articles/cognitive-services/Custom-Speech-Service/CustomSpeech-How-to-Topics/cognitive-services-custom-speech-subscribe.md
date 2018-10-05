---
title: 'Obtención de claves de suscripción: Custom Speech Service'
titlesuffix: Azure Cognitive Services
description: Aprenda a obtener claves de suscripción para las llamadas a Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: e4694928baf98bdb0d6aacead8dffec6bb73d6f7
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224781"
---
# <a name="obtain-subscription-keys"></a>Obtención de claves de suscripción
Para empezar a usar Azure Custom Speech Service, primero debe vincular su cuenta de usuario a una suscripción de Azure. Hay suscripciones de plan gratuito y de pago. Para obtener información sobre los planes, vea la [página de precios](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Obtener una clave suscripción
1. Puede obtener una clave de suscripción desde Azure Portal de dos maneras:

    * Vaya a [Azure Portal](https://ms.portal.azure.com) y agregue una nueva instancia de Cognitive Services API. Para ello, busque _Cognitive Services_ y, a continuación, seleccione **Cognitive Services APIs**.

      ![Búsqueda de Cognitive Services](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * O vaya directamente a [Cognitive Services APIs](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![Cognitive Services APIs](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
1. Rellene todos los campos obligatorios siguientes:

      a. **Nombre de cuenta**. Utilice un nombre que se adapte a sus necesidades. Recuerde este nombre para que pueda encontrar su suscripción a Cognitive Services en la lista de recursos.

      b. **Suscripción**. Seleccione una en las suscripciones de Azure.

      c. **Tipo de API**. Seleccione **Custom Speech Service (versión preliminar)**.

      d. **Ubicación**. Actualmente es **West US**.

      e. **Plan de tarifa**. Seleccione el plan que se adapte a sus necesidades. **F0** es el plan gratuito. Las cuotas permitidas se explican en la [página de precios](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Creación de la cuenta de Cognitive Services](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

1. Debe buscar una vista en el panel o un servicio con el nombre de cuenta proporcionado en la lista de recursos. Cuando se seleccione, podrá ver la información general del servicio. En la lista de la izquierda, en **Administración de recursos**, seleccione **Claves**. Copie **Clave 1**.

      Esta clave de suscripción es necesaria en los pasos siguientes.

      ![Clave 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > No copie la **Id. de suscripción** de la página de información general. La clave de suscripción se necesita en el paso siguiente.
      >

      ![Id. de suscripción de información general](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

1. Para especificar la clave de suscripción, en la cinta de la esquina superior derecha, seleccione su cuenta de usuario. En el menú desplegable, seleccione **Suscripciones**.

      ![Elemento de menú Suscripciones](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    Aparece una tabla de suscripciones, que está vacía la primera vez que se abre.

    ![Tabla de suscripciones](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

1. Seleccione **Agregar nuevo**. Escriba un nombre para la suscripción y la clave de la suscripción. Puede ser la **Clave 1** (clave principal) o la **Clave 2** (clave secundaria) de su suscripción.

      ![Nombre de clave de suscripción](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Para cargar datos, entrenar un modelo o realizar una implementación, debe vincular las actividades de Custom Speech Service a una suscripción de Azure. Puede ser una suscripción de plan gratuito o de pago. Consulte la [página de precios](https://www.microsoft.com/cognitive-services/en-us/pricing)para obtener más información.

## <a name="get-a-subscription-id"></a>Obtener una id. de suscripción
Para obtener una id. de suscripción, vaya a Azure Portal. Busque *Cognitive Services* y *Custom Speech Service*, y siga las instrucciones.

> [!NOTE]
> La clave de suscripción se necesitará más adelante en este proceso.
>

## <a name="next-steps"></a>Pasos siguientes
* Empezar crear el [modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md).
* Empezar a crear el [modelo de lenguaje personalizado](cognitive-services-custom-speech-create-language-model.md).
