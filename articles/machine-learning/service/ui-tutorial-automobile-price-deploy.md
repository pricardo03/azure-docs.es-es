---
title: 'Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual'
titleSuffix: Azure Machine Learning
description: Aprenda a crear una solución de análisis predictivo en la interfaz visual de Azure Machine Learning. Entrene, puntúe e implemente un modelo de Machine Learning mediante módulos de arrastrar y colocar.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 10/22/2019
ms.openlocfilehash: 6f8717f70a2cb03a7fd683cfe61f1198461f4305
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792671"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual

Para que otros usuarios puedan usar el modelo predictivo desarrollado en la [parte uno de este tutorial](ui-tutorial-automobile-price-train-score.md), se puede implementar como un punto de conexión en tiempo real. En la parte 1, ha entrenado el modelo. Ahora, es momento de generar predicciones basadas en los datos que escriba el usuario. En esta parte del tutorial, va a:

> [!div class="checklist"]
> * Implementar un punto de conexión en tiempo real
> * Crear un clúster de inferencia
> * Probar un punto de conexión en tiempo real

## <a name="prerequisites"></a>Requisitos previos

Complete la [parte uno del tutorial](ui-tutorial-automobile-price-train-score.md) para aprender a entrenar y puntuar un modelo de Machine Learning en la interfaz visual.

## <a name="deploy-a-real-time-endpoint"></a>Implementar un punto de conexión en tiempo real

Para implementar la canalización, debe:

1. Convertir la canalización de entrenamiento en una canalización de inferencia en tiempo real, que quita los módulos de entrenamiento y agrega entradas y salidas para la inferencia de solicitudes.
1. Implementar la canalización de inferencia

### <a name="create-a-real-time-inference-pipeline"></a>Crear una canalización de inferencia en tiempo real

1. En la parte superior del lienzo de la canalización, seleccione **Create inference pipeline** > **Real-time inference pipeline** (Crear canalización de inferencia > Canalización de inferencia en tiempo real).

    Al seleccionar **Create inference pipeline** (Crear canalización de inferencia), suceden varias cosas:
    
    * El modelo entrenado se almacena como un módulo **Conjunto de datos** en la paleta de módulos. Puede encontrarlo en **My Datasets** (Mis conjuntos de datos).
    * Los módulos, como **Entrenar modelo** y **Dividir datos**, que se usaron para el entrenamiento se eliminan.
    * El modelo entrenado guardado se vuelve a agregar a la canalización.
    * Se agregan los módulos **Web Service Input** (Entrada de servicio web) y **Web Service Output** (Salida de servicio web). Estos módulos identifican dónde entrarán los datos del usuario en el modelo y dónde se devuelven los datos.

    > [!Note]
    > La **canalización de entrenamiento** se guarda en la nueva pestaña en la parte superior del lienzo de la canalización. También se puede encontrar como una canalización publicada en la interfaz visual.
    >

    La canalización debería tener este aspecto:  

   ![Captura de pantalla que muestra la configuración esperada de la canalización después de prepararla para la implementación](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

1. Seleccione **Ejecutar** y use el mismo destino de proceso y experimento que usó en la parte 1.

1. Seleccione el módulo **Score Model** (Puntuar modelo).

1. En el panel de propiedades, seleccione **Salidas** > **Visualizar** para comprobar que el modelo sigue funcionando. Puede ver que se muestran los datos originales, junto con el precio previsto ("etiquetas puntuadas").

1. Seleccione **Implementar**.

### <a name="create-an-inferencing-cluster"></a>Creación de un clúster de inferencia

En el cuadro de diálogo que aparece, puede seleccionar entre los clústeres de Azure Kubernetes Service (AKS) existentes en el área de trabajo para implementar el modelo. Si no tiene un clúster de AKS, use los pasos siguientes para crear uno.

1. Seleccione **Proceso** en el cuadro de diálogo para ir a la página **Proceso**.

1. En la cinta de opciones de navegación, seleccione **Inference Clusters** (Clústeres de inferencia)  >  **+ Nuevo**.

    ![Captura de pantalla que muestra cómo ir al panel de nuevo clúster de inferencia](./media/ui-tutorial-automobile-price-deploy/new-inference-cluster.png)

1. En el panel del clúster de inferencia, configure un nuevo servicio de Kubernetes.

1. Escriba "aks-compute" en **Compute name** (Nombre de proceso).
    
1. En **Región**, seleccione una región disponible cercana.

1. Seleccione **Crear**.

    > [!Note]
    > La creación de un servicio AKS tarda unos 15 minutos. Puede comprobar el estado de aprovisionamiento en la página **Inference Clusters** (Clústeres de inferencia).
    >

### <a name="deploy-the-real-time-endpoint"></a>Implementación del punto de conexión en tiempo real

Después de que el servicio AKS haya terminado de aprovisionarse, vuelva a la canalización de inferencia en tiempo real para finalizar la implementación.

1. Seleccione **Implementar** encima del lienzo.

1. Seleccione **Deploy new real-time endpoint** (Implementar nuevo punto de conexión en tiempo real). 

1. Seleccione el clúster de AKS que ha creado.

1. Seleccione **Implementar**.

    ![Captura de pantalla que muestra cómo configurar un nuevo punto de conexión en tiempo real](./media/ui-tutorial-automobile-price-deploy/setup-endpoint.png)

    Cuando finalice la implementación (puede tardar unos minutos), aparecerá una notificación de operación correcta encima del lienzo.

## <a name="test-the-real-time-endpoint"></a>Prueba del punto de conexión en tiempo real

Para probar el punto de conexión en tiempo real, vaya a la página **Puntos de conexión** en el panel de navegación del área de trabajo de la izquierda.

1. En la página **Puntos de conexión**, seleccione el punto de conexión que implementó.

    ![Captura de pantalla que muestra la pestaña de puntos de conexión en tiempo real con el punto de conexión creado recientemente resaltado](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Seleccione **Probar**.

1. Escriba los datos de prueba o use los datos de ejemplo que se rellenan automáticamente y seleccione **Test** (Probar).

    La solicitud de prueba se envía al punto de conexión y los resultados se muestran en la página. Aunque se genera un valor de precio para los datos de entrada, este no se usa para generar el valor de predicción.

    ![Captura de pantalla que muestra cómo probar el punto de conexión en tiempo real con la etiqueta puntuada del precio resaltada](./media/ui-tutorial-automobile-price-deploy/test-endpoint.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido los pasos clave para crear, implementar y consumir un modelo de Machine Learning en la interfaz visual. Para más información sobre cómo usar la interfaz visual para resolver otros tipos de problemas, consulte las otras canalizaciones de ejemplo.

> [!div class="nextstepaction"]
> [Ejemplo de clasificación de riesgo crediticio](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
