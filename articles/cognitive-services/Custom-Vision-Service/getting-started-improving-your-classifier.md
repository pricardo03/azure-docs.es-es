---
title: 'Mejora del clasificador: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de cómo mejorar la calidad del clasificador.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: pafarley
ms.openlocfilehash: 2bee7f0af98bf03a13e376dea9dbf083b3f61815
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340297"
---
# <a name="how-to-improve-your-classifier"></a>Mejora del clasificador

Obtenga información acerca de cómo mejorar la calidad del clasificador de Custom Vision Service. La calidad de su clasificador depende de la cantidad, calidad y variedad de los datos con etiqueta que proporcione, así como del grado de equilibrio del conjunto de datos. Un buen clasificador normalmente tiene un conjunto de datos de aprendizaje equilibrado que es representativo de lo que se enviará al clasificador. El proceso de creación de un clasificador de este tipo suele ser iterativo. Suele ser necesarias algunas rondas de entrenamiento para obtener los resultados esperados.

Estos son los pasos comunes adoptados para mejorar un clasificador. Estos pasos no son reglas rápidas ni estrictas, sino la heurística que le ayudará a crear un clasificador mejor.

1. Primera ronda de entrenamiento
1. Agregar más imágenes y equilibrar datos
1. Reentrenar el modelo
1. Agregar imágenes con fondo, luz, tamaño de objeto, ángulo de cámara y estilo diversos
1. Reentrenar e introducir imágenes para predicciones
1. Examinar resultados de predicciones
1. Modificar datos de entrenamiento existentes

## <a name="data-quantity-and-data-balance"></a>Cantidad de datos y equilibrio de datos

Lo más importante consiste en cargar suficientes imágenes para entrenar al clasificador. Se recomiendan al menos 50 imágenes por etiqueta para el conjunto de entrenamiento como un punto de partida. Con menos imágenes, existe el riesgo elevado de que se produzca un sobreajuste. Si bien las cifras sobre rendimiento pueden sugerir una buena calidad, puede que tenga que enfrentarse a datos del mundo real. Entrenar al clasificador con más imágenes, por lo general, aumentará la precisión de los resultados de las predicciones.

Otra cosa que hay que considerar es que hay que asegurarse de que los datos están equilibrados. Por ejemplo, tener 500 imágenes para una etiqueta y 50 imágenes para otra generará un conjunto de datos de entrenamiento desequilibrado, por lo que el modelo será más preciso en la predicción de una etiqueta que de otra. Probablemente obtendrá mejores resultados si mantiene al menos una relación 1:2 entre la etiqueta con menos imágenes y la etiqueta que tiene la mayoría de las imágenes. Por ejemplo, si la etiqueta con más imágenes tiene 500 imágenes, la etiqueta con menos imágenes necesitará tener al menos 250 imágenes para el entrenamiento.

## <a name="train-more-diverse-images"></a>Entrenamiento de imágenes más diversas

Proporcione imágenes que sean representativas de lo que se va a enviar al clasificador durante el uso normal. Por ejemplo, si entrena a un clasificador de "manzanas", este no será tan preciso si solo entrena fotos de manzanas en platos pero realiza predicciones de fotos de manzanas en árboles. Incluir diversos tipos de imágenes garantizará que no haya sesgos en el clasificador y que este pueda generalizar bien. A continuación se indican algunas formas de diversificar más el conjunto de entrenamiento:

__Fondo:__ proporcione imágenes del objeto delante de fondos distintos (es decir, fruta en platos frente a fruta en la bolsa de la compra). Las fotos en contexto son mejores que las fotos delante de fondos neutros, ya que ofrecen más información para el clasificador.

![Ejemplos de imágenes de fondos](./media/getting-started-improving-your-classifier/background.png)

__Luz:__ proporcione imágenes con una luz distinta (es decir, fotos realizadas con flash, con alta exposición, etc.), sobre todo si las imágenes utilizadas para predicciones presentan una iluminación distinta. También resulta útil incluir imágenes con niveles diferentes de saturación, matiz y brillo.

![Ejemplos de imágenes con luz](./media/getting-started-improving-your-classifier/lighting.png)

__Tamaño de objeto:__ proporcione imágenes en las que los objetos presenten distintos tamaños, con la captura de partes distintas del objeto. Por ejemplo, una foto de racimos de plátanos y un primer plano de un solo plátano. Los diferentes tamaños ayudan al clasificador a generalizar mejor.

![Ejemplos de tamaños en imágenes](./media/getting-started-improving-your-classifier/size.png)

__Ángulo de cámara:__ proporcione imágenes realizadas con ángulos de cámara diferentes. Si todas las fotos se realizan con un conjunto de cámaras fijas (como cámaras de vigilancia), asegúrese de asignar una etiqueta diferente a cada cámara incluso aunque capturen los mismos objetos para evitar sobreajustes o modelados de objetos no relacionados (como las farolas) como la característica principal.

![Ejemplo de imágenes con ángulo](./media/getting-started-improving-your-classifier/angle.png)

__Estilo:__ proporcione imágenes con estilos diferentes de la misma clase (es decir, distintos tipos de cítricos). Sin embargo, si tiene imágenes de objetos de estilos radicalmente distintos (es decir, Mickey Mouse frente a una rata real), se recomienda etiquetarlas como clases independientes para representar mejor sus características distintivas.

![Ejemplos de imágenes de estilos](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-images-submitted-for-prediction"></a>Uso de imágenes enviadas para la predicción

El servicio Custom Vision Service almacena las imágenes enviadas al punto de conexión de la predicción. Para utilizar estas imágenes para mejorar el clasificador, siga estos pasos:

1. Para ver las imágenes enviadas al clasificador, abra la [página web de Custom Vision](https://customvision.ai), vaya al proyecto y seleccione la pestaña __Predictions__ (Predicciones). La vista predeterminada muestra imágenes de la iteración actual. Puede usar el campo de lista desplegable __Iteración__ para ver las imágenes enviadas durante las iteraciones anteriores.

    ![Imagen de la pestaña de predicciones](./media/getting-started-improving-your-classifier/predictions.png)

2. Mantenga el mouse sobre una imagen para ver las etiquetas previstas por el clasificador. Las imágenes se clasifican, y aquellas que pueden aportar más ganancias al clasificador se colocan en primer lugar. Para seleccionar una clasificación diferente, use la sección __Ordenar__. Para agregar una imagen a los datos de entrenamiento existentes, seleccione la imagen, seleccione la etiqueta correcta y después seleccione __Guardar y cerrar__. La imagen se quitará de __Predictions__ (Predicciones) y se agrega a las imágenes de entrenamiento. Para verla, seleccione la pestaña __Training Images__ (Imágenes de entrenamiento).

    ![Imagen de la página de etiquetado](./media/getting-started-improving-your-classifier/tag.png)

3. Use el botón __Entrenar__ para reentrenar el clasificador.

## <a name="visually-inspect-predictions"></a>Inspección visual de predicciones

Para inspeccionar las predicciones de la imagen, seleccione la pestaña __Training Images__ (Imágenes de entrenamiento) y, a continuación, seleccione __Iteration History__ (Historial de iteración). Las imágenes rodeadas de una línea roja se predijeron incorrectamente.

![Imagen del historial de iteración](./media/getting-started-improving-your-classifier/iteration.png)

En ocasiones, la inspección visual puede identificar patrones que puede corregir agregando datos de entrenamiento adicionales o modificando los datos de entrenamiento existentes. Por ejemplo, un clasificador que distingue manzanas de limas puede etiquetar incorrectamente las manzanas verdes como limas. Puede corregir este problema agregando y proporcionando datos de entrenamiento que contengan imágenes etiquetadas de manzanas verdes.

## <a name="unexpected-classification"></a>Clasificación inesperada

A veces, el clasificador aprende incorrectamente las características que las imágenes tienen en común. Por ejemplo, si va a crear un clasificador que distingue las manzanas de los cítricos y se proporcionan imágenes de manzanas en las manos y de cítricos en platos blancos, puede que el clasificador se entrene para las manos y los platos blancos y no para diferenciar las manzanas de los cítricos.

![Imagen de una clasificación inesperada](./media/getting-started-improving-your-classifier/unexpected.png)

Para corregir este problema, utilice las instrucciones de entrenamiento anteriores con imágenes más variadas: proporcione imágenes con distintos ángulos, fondos, tamaños de objeto, grupos y otras variantes.

## <a name="negative-image-handling"></a>Control negativo de la imagen

El servicio Custom Vision Service admite cierto control negativo automático de la imagen. Si va a crear un clasificador que distingue uvas de plátanos y envía una imagen de un zapato para la predicción, el clasificador puntuaría esa imagen con un valor cercano al 0 % para uva y plátano.

Por otro lado, en los casos en que las imágenes negativas son solo una variación de las imágenes utilizadas en el entrenamiento, es probable que el modelo clasifique las imágenes negativas como una clase etiquetada debido a las grandes similitudes que existen. Por ejemplo, si tiene un clasificador que distingue naranjas de pomelos y envía una imagen de una clementina, puede que clasifique la clementina como una naranja. Esto puede ocurrir porque muchas características de las clementina (color, forma, textura, hábitat natural, etc.) se parecen a las de las naranjas.  Si las imágenes negativas son de esta naturaleza, se recomienda crear una o varias etiquetas independientes ("Otros") y etiquetar las imágenes negativas con esta etiqueta durante el entrenamiento, para permitir que el modelo diferencie mejor entre estas clases.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo puede probar imágenes mediante programación enviándolas a Prediction API.

> [!div class="nextstepaction"]
[Uso de la API de predicción](use-prediction-api.md)
