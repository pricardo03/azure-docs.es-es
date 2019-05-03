---
title: 'Red neuronal multiclase: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de red neuronal Multiclase en el servicio Azure Machine Learning para crear un modelo de red neuronal que puede utilizarse para predecir un destino que tenga varios valores.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a7bef225c001ebd9bbb9a45c8bcc301cfb49edb6
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029391"
---
# <a name="multiclass-neural-network-module"></a>Módulo de red neuronal multiclase

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de red neuronal que puede utilizarse para predecir un destino que tenga varios valores. 

Por ejemplo, las redes neurales de este tipo podrían usarse en tareas de visión informática complejas, como el reconocimiento de dígitos o letras, la clasificación de documentos y reconocimiento de patrones.

Clasificación con redes neuronales es un método de aprendizaje supervisado y, por tanto, requiere una *etiquetados dataset* que incluye una columna de etiqueta.

Puede entrenar el modelo, ya que proporciona el modelo y el conjunto de datos etiquetado como entrada para [entrenar modelo](./train-model.md). El modelo entrenado, a continuación, puede utilizarse para predecir valores para los nuevos ejemplos de entrada.  

## <a name="about-neural-networks"></a>Acerca de las redes neuronales

Una red neuronal es un conjunto de niveles interconectados. Las entradas son la primera capa y están conectadas a un nivel de salida mediante un gráfico acíclico dirigido constan de nodos y las aristas ponderadas.

Entre las capas de entrada y salidas puede insertar varias capas ocultas. Más tareas de predicción pueden realizarse fácilmente con solo una o varias capas ocultas. Sin embargo, las investigaciones recientes han demostrado que redes neurales profundas (DNN) con muchos niveles pueden ser eficaces en tareas complejas, como reconocimiento de imágenes o voz. Los niveles sucesivos se usan para modelar los crecientes niveles de profundidad semántica.

La relación entre entradas y salidas se aprendido de entrenamiento de la red neuronal en los datos de entrada. La dirección del grafo procede de las entradas a través de la capa oculta y la capa de salida. Todos los nodos de una capa se conectan mediante las aristas ponderadas a los nodos del nivel siguiente.

Para calcular el resultado de la red para una entrada determinada, se calcula un valor en cada nodo en los niveles ocultos y del nivel de salida. El valor se establece calculando la suma ponderada de los valores de los nodos de la capa anterior. Una función de activación, a continuación, se aplica a esa suma ponderada.

## <a name="configure-multiclass-neural-network"></a>Configurar la red neuronal Multiclase

1. Agregar el **Red neuronal Multiclase** módulo al experimento en la interfaz. Puede encontrar este módulo en **Machine Learning**, **inicializar**, en el **clasificación** categoría.

2. **Crear modo instructor**: Use esta opción para especificar cómo desea que el modelo se entrene:

    - **Único parámetro**: Elija esta opción si ya sabe cómo desea configurar el modelo.

    

3. **Oculta la especificación de niveles**: Seleccione el tipo de arquitectura de red para crear.

    - **Totalmente conectado caso**: Seleccione esta opción para crear un modelo con la arquitectura de red neuronal de forma predeterminada. Para los modelos de red neuronal multiclase, los valores predeterminados son los siguientes:

        - Un nivel oculto
        - La capa de salida está conectada por completo a la capa oculta.
        - La capa oculta por completo está conectada a la capa de entrada.
        - El número de nodos en el nivel de entrada viene determinada por el número de características de los datos de entrenamiento.
        - Por el usuario se puede establecer el número de nodos de la capa oculta. El valor predeterminado es 100.
        - El número de nodos del nivel de salida depende del número de clases.
  
   

5. **Número de nodos ocultos**: Esta opción le permite personalizar el número de nodos ocultos en la arquitectura predeterminada. Escriba el número de nodos ocultos. El valor predeterminado es una capa oculta con 100 nodos.

6. **La velocidad de aprendizaje**: Definir el tamaño del paso llevadas a cabo en cada iteración, antes de la corrección. Un valor mayor para la velocidad de aprendizaje puede hacer que el modelo converja con mayor rapidez, pero puede pasar por alto los mínimos locales.

7. **Número de iteraciones de aprendizaje**: Especifique el número máximo de veces que el algoritmo debe procesar los casos de entrenamiento.

8. **Diámetro del aprendizaje inicial de los pesos**: Especifique los pesos de nodo al principio del proceso de aprendizaje.

9. **El impulso**: Especificar un peso para aplicar durante el aprendizaje a los nodos de iteraciones anteriores.
  
11. **Ordenar ejemplos aleatoriamente**: Seleccione esta opción para ordenar de forma aleatoria los casos entre iteraciones.

    Si desactiva esta opción, se procesan casos en exactamente el mismo orden cada vez que ejecute el experimento.

12. **Valor de inicialización de números aleatorios**: Escriba un valor que se usará como el valor de inicialización, si desea asegurar la repetibilidad entre ejecuciones del mismo experimento.

14. Conectar un conjunto de datos de entrenamiento y uno de los [módulos de formación](module-reference.md): 

    - Si establece **crear modo de entrenador** a **único parámetro**, utilice [Train Model](train-model.md).  
  

## <a name="results"></a>Results

Una vez completada la formación:

- Para ver un resumen de los parámetros del modelo, junto con la característica ponderaciones aprendidas de aprendizaje y otros parámetros de la red neuronal, haga clic en la salida de [Train Model](./train-model.md) y seleccione **visualizar**.  

- Para guardar una instantánea del modelo entrenado, haga clic en el **calificado modelo** de salida y seleccione **Guardar como modelo entrenado**. Este modelo no se actualiza en las ejecuciones sucesivas del mismo experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 