---
author: PatrickFarley
ms.service: cognitive-services
ms.topic: include
ms.date: 02/07/2019
ms.author: pafarley
ms.openlocfilehash: 56b6ba3dcd88dd45bbda3ccb9ba7ddeb5b373b10
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "73518916"
---
## <a name="get-the-training-and-prediction-keys"></a>Obtención de las claves de entrenamiento y predicción

El proyecto necesita un conjunto válido de claves de suscripción para interactuar con el servicio. Puede encontrar los elementos en el [sitio web de Custom Vision](https://customvision.ai). Inicie sesión con la cuenta asociada a la cuenta de Azure usada para crear los recursos de Custom Vision. Seleccione el __icono de engranaje__ en la esquina superior derecha. En la sección __Cuentas__, tiene disponibles listas desplegables para Azure Resources, lo que le permitirá ver los valores de los campos __Training Key__ (Clave de entrenamiento), __Prediction Key__ (Clave de predicción) y __Prediction Resource Id__ (Id. de recurso de predicción).

![Imagen de la interfaz de usuario de claves](../media/csharp-tutorial/training-prediction-keys.png)

 Como alternativa, estas claves y este identificador se pueden obtener en [Azure Portal](https://www.portal.azure.com) viendo los recursos de entrenamiento y predicción de Custom Vision y revisando la pestaña __Claves__. Allí puede encontrar la __clave de entrenamiento__ o la __clave de predicción__. Vaya a la pestaña __Propiedades__ del recurso de predicción para obtener el __identificador de recurso de predicción__.

