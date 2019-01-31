---
title: Creación de un punto de conexión personalizado de voz con Speech Service en Azure | Microsoft Docs
description: Aprenda a crear un punto de conexión personalizado de voz a texto con Speech Service en Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 03/12/2018
ms.author: panosper
ms.openlocfilehash: 63d2c15c14340b0d7407b0144a8b8c75a2634e6a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220253"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Creación de un punto de conexión personalizado de voz a texto

Una vez creados los modelos acústicos o modelos de lenguaje personalizados, puede implementarlos en un punto de conexión personalizado de voz a texto. 

## <a name="create-an-endpoint"></a>Creación de un extremo
Para crear un nuevo punto de conexión personalizado, seleccione **Puntos de conexión** en el menú **Custom Speech** que encontrará en la parte superior de la página. Esta acción le llevará a la página **Puntos de conexión**, que contiene una tabla con los puntos de conexión personalizados actuales. Si aún no ha creado ningún punto de conexión, la tabla estará vacía. La configuración regional actual se refleja en el título de la tabla. 

Para crear una implementación en un idioma diferente, seleccione **Change Locale** (Cambiar configuración regional). Para más información, consulte los idiomas compatibles.

Para crear un nuevo punto de conexión, seleccione **Crear nuevo**. En el panel **Crear punto de conexión**, especifique los valores que correspondan en los cuadros **Nombre** y **Descripción** de la implementación personalizada.

En el cuadro combinado **Suscripción**, seleccione la suscripción que quiera usar. Si es una suscripción S0 (de pago), puede seleccionar solicitudes simultáneas.

También puede seleccionar si el registro del contenido debe estar activado o desactivado. Es decir, puede seleccionar si desea almacenar el tráfico del punto de conexión. Si no se selecciona, se suprimirá el almacenamiento de tráfico. Puede buscar vínculos de descarga en todo el contenido registrado mediante la vista Punto de conexión -> Detalles.

En la lista **Modelo acústico**, seleccione el modelo acústico que quiera y en la lista **Modelo de lenguaje**, seleccione el modelo de lenguaje que quiera. En las elecciones para los modelos acústico y de lenguaje siempre se incluyen los modelos base de Microsoft. La selección del modelo base limita las combinaciones. No se pueden combinar modelos base conversacionales con modelos base de dictado.

> [!NOTE]
> Asegúrese de aceptar los términos de uso y la información sobre los precios, seleccionando la casilla.
>

Después de seleccionar los modelos acústico y de lenguaje, seleccione **Crear**. Esta acción le devolverá a la página **Implementaciones**. Ahora, la tabla incluye una entrada que se corresponde con el nuevo punto de conexión. El estado del punto de conexión refleja su estado actual mientras se está creando. Puede tardar hasta 30 minutos en crear una instancia de un nuevo punto de conexión con los modelos personalizados. Cuando el estado de la implementación es *Completado*, el punto de conexión está listo para su uso.

Cuando la implementación está lista, el nombre del punto de conexión se convierte en un vínculo. Al seleccionar el vínculo, aparece la página **Endpoint Information** (Información del punto de conexión), que contiene las direcciones URL del punto de conexión personalizado que se puede usar con una solicitud HTTP o con la biblioteca cliente del servicio Voz de Microsoft Cognitive Services, que usa sockets web.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más tutoriales, vea:
- [Obtener la suscripción de evaluación gratuita del servicio Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Creación de un modelo acústico personalizado](how-to-customize-acoustic-models.md)
- [Crear un modelo de lenguaje personalizado](how-to-customize-language-model.md)
