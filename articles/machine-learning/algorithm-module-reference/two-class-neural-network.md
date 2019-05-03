---
title: 'Red neuronal de dos clases: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de red neuronal de dos clases en el servicio Azure Machine Learning para crear un modelo de red neuronal que se puede usar para predecir un destino que tenga solo dos valores.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7ea852fcd312c6f7b1b716278ed538b7accde5bd
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029226"
---
# <a name="two-class-neural-network-module"></a>Módulo de red neuronal de dos clases

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de red neuronal que se puede usar para predecir un destino que tenga solo dos valores.

Clasificación con redes neuronales es un método de aprendizaje supervisado y, por tanto, requiere una *etiquetados dataset*, que incluye una columna de etiqueta. Por ejemplo, puede usar este modelo de red neuronal para predecir resultados binarios, como si un paciente tiene una determinada enfermedad, o si una máquina es propenso a errores dentro de un intervalo de tiempo especificado.  

Después de definir el modelo, entrenar, ya que proporciona un conjunto de datos etiquetado y el modelo como entrada para [entrenar modelo](./train-model.md). El modelo entrenado, a continuación, puede utilizarse para predecir valores para nuevas entradas.

### <a name="more-about-neural-networks"></a>Más información acerca de las redes neuronales

Una red neuronal es un conjunto de niveles interconectados. Las entradas son la primera capa y están conectadas a un nivel de salida mediante un gráfico acíclico dirigido constan de nodos y las aristas ponderadas.

Entre las capas de entrada y salidas puede insertar varias capas ocultas. Más tareas de predicción pueden realizarse fácilmente con solo una o varias capas ocultas. Sin embargo, las investigaciones recientes han demostrado que redes neurales profundas (DNN) con muchos niveles pueden ser eficaces en tareas complejas, como reconocimiento de imágenes o voz. Los niveles sucesivos se usan para modelar los crecientes niveles de profundidad semántica.

La relación entre entradas y salidas se aprendido de entrenamiento de la red neuronal en los datos de entrada. La dirección del grafo procede de las entradas a través de la capa oculta y la capa de salida. Todos los nodos de una capa se conectan mediante las aristas ponderadas a los nodos del nivel siguiente.

Para calcular el resultado de la red para una entrada determinada, se calcula un valor en cada nodo en los niveles ocultos y del nivel de salida. El valor se establece calculando la suma ponderada de los valores de los nodos de la capa anterior. Una función de activación, a continuación, se aplica a esa suma ponderada.
  
## <a name="how-to-configure"></a>Configuración

1.  Agregar el **Red neuronal de dos clases** módulo al experimento. Puede encontrar este módulo en **Machine Learning**, **inicializar**, en el **clasificación** categoría.  
  
2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Elija esta opción si ya sabe cómo desea configurar el modelo.  

3.  Para **oculta la especificación de niveles**, seleccione el tipo de arquitectura de red para crear.  
  
    -   **Totalmente conectado caso**: Usa la arquitectura de red neuronal predeterminada, que se define para las redes neuronales de dos clases como sigue:
  
        -   Tiene una capa oculta.
  
        -   La capa de salida está completamente conectada a la capa oculta, y está conectada por completo la capa oculta a la capa de entrada.
  
        -   El número de nodos en el nivel de entrada es igual al número de características de los datos de entrenamiento.
  
        -   El usuario establece el número de nodos de la capa oculta. El valor predeterminado es 100.
  
        -   El número de nodos es igual al número de clases. Para una red neuronal de dos clases, esto significa que todas las entradas deben asignarse a uno de dos nodos en el nivel de salida.

5.  Para **velocidad de aprendizaje**, defina el tamaño del paso llevadas a cabo en cada iteración, antes de la corrección. Un valor mayor para la velocidad de aprendizaje puede hacer que el modelo converja con mayor rapidez, pero puede pasar por alto los mínimos locales.

6.  Para **número de iteraciones de aprendizaje**, especifique el número máximo de veces que el algoritmo debe procesar los casos de entrenamiento.

7.  Para **diámetro del aprendizaje inicial de los pesos**, especificar los pesos de nodo al principio del proceso de aprendizaje.

8.  Para **el impulso**, especificar un peso para aplicar durante el aprendizaje a los nodos de iteraciones anteriores  

10. Seleccione el **ordenar ejemplos aleatoriamente** opción para ordenar de forma aleatoria los casos entre iteraciones. Si desactiva esta opción, se procesan casos en exactamente el mismo orden cada vez que ejecute el experimento.
  
11. Para **valor de inicialización aleatorio número**, escriba un valor que se usará como el valor de inicialización.
  
     Especificar un valor de inicialización valor es útil cuando desea asegurar la repetibilidad entre ejecuciones del mismo experimento.  En caso contrario, un valor de reloj del sistema se utiliza como valor de inicialización, lo que puede producir resultados ligeramente diferentes cada vez que ejecute el experimento.
  
13. Agregue un conjunto de datos etiquetado para el experimento y conecte uno de los [módulos de formación](module-reference.md).  
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, utilice el [Train Model](train-model.md) módulo.  
  
14. Ejecute el experimento.

## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver un resumen de los parámetros del modelo, junto con la característica ponderaciones aprendidas de aprendizaje y otros parámetros de la red neuronal, haga clic en la salida de [Train Model](./train-model.md)y seleccione **visualizar**.  

+ Para guardar una instantánea del modelo entrenado, haga clic en el **calificado modelo** de salida y seleccione **Guardar como modelo entrenado**. Este modelo no se actualiza en las ejecuciones sucesivas del mismo experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 
