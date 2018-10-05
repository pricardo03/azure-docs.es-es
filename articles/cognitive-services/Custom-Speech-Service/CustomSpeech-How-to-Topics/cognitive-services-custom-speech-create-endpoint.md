---
title: 'Creación de un punto de conexión de voz personalizado: Custom Speech Service'
titlesuffix: Azure Cognitive Services
description: Aprenda a crear un punto de conexión personalizado de voz a texto con Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 07/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 958e97f708e7c6920bcb0d65d91656fcf00cd71a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219030"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Creación de un punto de conexión personalizado de voz a texto
Una vez creados los modelos acústicos o modelos de lenguaje personalizados, puede implementarlos en un punto de conexión personalizado de voz a texto. 

## <a name="create-an-endpoint"></a>Creación de un extremo
Para crear un nuevo punto de conexión personalizado, seleccione **Implementaciones** en el menú **Custom Speech** en la parte superior de la página. Esta acción lleva a la página **Implementaciones**, que contiene una tabla de puntos de conexión personalizados actuales. Si aún no ha creado ningún punto de conexión, la tabla estará vacía. La configuración regional actual se refleja en el título de la tabla. 

Para crear una implementación en un idioma diferente, seleccione **Change Locale** (Cambiar configuración regional). Para obtener más información acerca de los idiomas compatibles, consulte [Supported locales in Custom Speech Service](cognitive-services-custom-speech-change-locale.md) (Configuraciones regionales compatibles en Custom Speech Service).

Para crear un nuevo punto de conexión, seleccione **Crear nuevo**. En el panel **Crear implementación**, especifique la información en los cuadros **Nombre** y **Descripción** de la implementación personalizada.

En el cuadro combinado **Suscripción**, seleccione la suscripción que quiera usar. Si se trata de una suscripción S2, puede seleccionar las unidades de escalado y el registro de contenido. Para obtener más información sobre las unidades de escalado y el registro, vea [Custom Speech Service meters and quotas](../cognitive-services-custom-speech-meters.md) (Métricas y cuotas de Custom Speech Service).

En la tabla siguiente se muestra cómo se asignan las unidades de escalado a las solicitudes simultáneas disponibles:

| Unidad de escalado | Número de solicitudes simultáneas |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| n | 5 * n |

También puede seleccionar si el registro del contenido debe estar activado o desactivado. Es decir, puede seleccionar si el tráfico del punto de conexión se almacena para uso interno de Microsoft. Si no se selecciona, se suprimirá el almacenamiento de tráfico. La supresión del registro del contenido no da lugar a ningún costo adicional. Consulte la [página de información sobre precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) para ver los detalles.

> [!NOTE]
> El registro de contenido se denomina "Sin seguimiento" en la página de precios.
>


Además, Microsoft proporciona una estimación de costos aproximada para que conozca el impacto en los costos de las unidades de escalado y el registro del contenido. Este cálculo es una estimación aproximada y puede diferir de los costos reales.

> [!NOTE]
> Estas opciones no están disponibles con las suscripciones F0 (nivel gratuito).
>

En la lista **Modelo acústico**, seleccione el modelo acústico que quiera y en la lista **Modelo de lenguaje**, seleccione el modelo de lenguaje que quiera. En las elecciones para los modelos acústico y de lenguaje siempre se incluyen los modelos base de Microsoft. La selección del modelo base limita las combinaciones. No se pueden combinar modelos base conversacionales con modelos base de dictado.

![Página de creación de implementación](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Asegúrese de aceptar los términos de uso y la información sobre los precios, seleccionando la casilla.
>

Después de seleccionar los modelos acústico y de lenguaje, seleccione **Crear**. Esta acción le devolverá a la página **Implementaciones**. Ahora, la tabla incluye una entrada que se corresponde con el nuevo punto de conexión. El estado del punto de conexión refleja su estado actual mientras se está creando. Puede tardar hasta 30 minutos en crear una instancia de un nuevo punto de conexión con los modelos personalizados. Cuando el estado de la implementación es *Completado*, el punto de conexión está listo para su uso.

![Página de implementaciones](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Cuando la implementación está lista, el nombre de la implementación se convierte en un vínculo. Al seleccionar el vínculo aparece la página **Información de implementación**, que muestra las direcciones URL de su punto de conexión personalizado que se puede usar con una solicitud HTTP o con la biblioteca cliente de voz de Microsoft Cognitive Services, que usa Web Sockets.

![Página de información de implementación](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más tutoriales, vea:
* [Uso de un punto de conexión personalizado de voz a texto](cognitive-services-custom-speech-use-endpoint.md)
* [Crear un modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* [Crear un modelo de lenguaje personalizado](cognitive-services-custom-speech-create-language-model.md)
