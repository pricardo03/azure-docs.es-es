---
title: Crear un punto de conexión personalizado de voz con servicios de voz en Azure | Microsoft Docs
description: Obtenga información sobre cómo crear un extremo personalizado de voz a texto mediante los servicios de voz de Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 03/12/2018
ms.author: panosper
ms.openlocfilehash: 1f7a84d187ba6279caad4926d54bfc56254152af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60539033"
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
- [Obtenga su suscripción de evaluación gratuita de Speech Service](https://azure.microsoft.com/try/cognitive-services/)
- [Creación de un modelo acústico personalizado](how-to-customize-acoustic-models.md)
- [Crear un modelo de lenguaje personalizado](how-to-customize-language-model.md)
