---
title: Mejora de su clasificador con Custom Vision Service en Azure Cognitive Services | Microsoft Docs
description: Obtenga información acerca de cómo mejorar la calidad del clasificador de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 65b1424f259066c7d5bd6b2b508d2a4052ff0527
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381006"
---
# <a name="how-to-improve-your-classifier"></a>Mejora del clasificador

Obtenga información acerca de cómo mejorar la calidad del clasificador de Custom Vision Service. La calidad de su clasificador depende de la calidad de los datos con etiqueta que proporcione. 

## <a name="train-more-varied-images"></a>Entrenamiento de imágenes más variadas

El hecho de proporcionar imágenes etiquetadas con distintos ángulos, fondos, tamaños de objeto, grupos de fotografías y otras variantes mejora el clasificador. Las fotos en contexto son mejores que las fotos delante de fondos neutros. Incluya imágenes que sean representativas de lo que se va a enviar al clasificador durante el uso normal.

Para obtener más información sobre cómo agregar imágenes, consulte el documento [Build a classifier](getting-started-build-a-classifier.md) (Compilación de un clasificador).

> [!IMPORTANT]
> No olvide entrenar el clasificador después de haber agregado imágenes.

## <a name="use-images-submitted-for-prediction"></a>Uso de imágenes enviadas para la predicción

El servicio Custom Vision Service almacena las imágenes enviadas al punto de conexión de la predicción. Para utilizar estas imágenes para mejorar el clasificador, siga estos pasos:

1. Para ver las imágenes enviadas al clasificador, abra la [página web de Custom Vision](https://customvision.ai) y seleccione la pestaña __Predictions__ (Predicciones).

    ![Imagen de la pestaña de predicciones](./media/getting-started-improving-your-classifier/predictions-tab.png)

    > [!TIP]
    > La vista predeterminada muestra imágenes de la iteración actual. Puede usar el campo de lista desplegable __Iteración__ para ver las imágenes enviadas durante las iteraciones anteriores.

2. Mantenga el mouse sobre una imagen para ver las etiquetas previstas por el clasificador.

    > [!TIP]
    > Las imágenes se clasifican, y aquellas que pueden aportar más ganancias al clasificador se colocan en primer lugar. Para seleccionar una clasificación diferente, use la sección __Ordenar__.

    Para agregar una imagen a los datos de entrenamiento, seleccione la imagen, seleccione la etiqueta y, a continuación, seleccione __Guardar y cerrar__. La imagen se quita de __Predictions__ (Predicciones) y se agrega a las imágenes de entrenamiento. Para verla, seleccione la pestaña __Training Images__ (Imágenes de entrenamiento).

    ![Imagen de la página de etiquetado](./media/getting-started-improving-your-classifier/tag-image.png)

3. Use el botón __Entrenar__ para reentrenar el clasificador.

## <a name="visually-inspect-predictions"></a>Inspección visual de predicciones

Para inspeccionar las predicciones de la imagen, seleccione la pestaña __Training Images__ (Imágenes de entrenamiento) y, a continuación, seleccione __Iteration History__ (Historial de iteración). Las imágenes rodeadas de una línea roja se predijeron incorrectamente.

![Imagen del historial de iteración](./media/getting-started-improving-your-classifier/iteration-history.png)

En ocasiones, la inspección visual puede identificar patrones que puede corregir agregando datos de entrenamiento adicionales. Por ejemplo, un clasificador que distingue entre rosas y margaritas podría etiquetar incorrectamente todas las rosas blancas como margaritas. Puede corregir este problema agregando y proporcionando datos de entrenamiento que contengan imágenes etiquetadas de rosas blancas.

## <a name="unexpected-classification"></a>Clasificación inesperada

A veces, el clasificador aprende las características que las imágenes tienen en común. Por ejemplo, desea crear un clasificador de rosas y tulipanes. Proporciona imágenes de tulipanes en el campo y de rosas en un jarrón rojo frente a una pared azul. Con estos datos, el clasificador podría aprender la diferencia entre campo y pared+jarrón en lugar de entre rosas y tulipanes.

Para corregir este problema, utilice las instrucciones de entrenamiento con imágenes más variadas: proporcione imágenes con distintos ángulos, fondos, tamaños del objeto, grupos y otras variantes.

## <a name="negative-image-handling"></a>Control negativo de la imagen

El servicio Custom Vision Service admite cierto control negativo automático de la imagen. Si está creando un clasificador que distingue entre gatos y perros y envía una imagen de un zapato para la predicción, el clasificador puntuaría esa imagen con un valor cercano al 0 % para perro y gato. 

> [!WARNING]
> El enfoque automático funciona con imágenes claramente negativas. Es posible que no funcione bien en casos donde las imágenes negativas son simplemente una variación de las imágenes que se usan para el entrenamiento. 
>
> Por ejemplo, si tiene un clasificador que distingue entre las razas de perro husky y corgi y proporciona la imagen de un pomerano, es posible que el pomerano se puntúe como husky. Si las imágenes negativas son de este tipo, cree una nueva etiqueta (por ejemplo, "otros") y aplíquela a las imágenes de aprendizaje negativo.

## <a name="next-steps"></a>Pasos siguientes

[Uso de la API de predicción](use-prediction-api.md)