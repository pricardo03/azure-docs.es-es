---
title: 'Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual'
titleSuffix: Azure Machine Learning service
description: Aprenda a cómo crear una solución de análisis predictivo en la interfaz visual de Azure Machine Learning Service. Entrene, puntúe e implemente un modelo de Machine Learning mediante módulos de arrastrar y colocar. Este tutorial es la segunda de una serie de dos partes sobre la predicción de precios de automóviles mediante la regresión lineal.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 07/11/2019
ms.openlocfilehash: dd28fb51a4fc3fbf3dfc893f2f5f159ccafdb4b3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839300"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual

Para que otros usuarios puedan usar el modelo predictivo desarrollado en la [parte uno de este tutorial](ui-tutorial-automobile-price-train-score.md), se puede implementar como servicio web de Azure. Hasta ahora ha estado experimentando con el entrenamiento del modelo. Ahora, es momento de generar predicciones basadas en los datos que escriba el usuario. En esta parte del tutorial, va a:

> [!div class="checklist"]
> * Preparar un modelo para la implementación
> * Implementación de un servicio web
> * Probara un servicio web
> * Administrar un servicio web
> * Consumo del servicio web

## <a name="prerequisites"></a>Requisitos previos

Complete la [parte uno del tutorial](ui-tutorial-automobile-price-train-score.md) para aprender a entrenar y puntuar un modelo de Machine Learning en la interfaz visual.

## <a name="prepare-for-deployment"></a>Preparación de la implementación

Antes de implementar el experimento como un servicio web, primero debe convertir el *experimento de entrenamiento* en un *experimento predictivo*.

1. Seleccione **Create Predictive Experiment** (Crear experimento predictivo)* en la parte inferior del lienzo.

    ![Gif animado que muestra la conversión automática de un experimento de entrenamiento en un experimento de predicción](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

    Cuando selecciona **Create Predictive Experiment** (Crear experimento predictivo), suceden varias cosas:
    
    * El modelo entrenado se almacena como un módulo del **modelo entrenado** en la paleta de módulos. Puede encontrarlo en **Trained Models** (Modelos entrenados).
    * Se quitan los módulos que se utilizaron para el entrenamiento, en particular:
      * Train Model (Entrenar modelo)
      * Split Data (Dividir datos)
      * Evaluate Model (Evaluar modelo)
    * El modelo entrenado guardado se vuelve a agregar al experimento.
    * Se agregan los módulos de **entrada de servicio web** y de **salida de servicio web**. Estos módulos identifican el lugar en que los datos del usuario entrarán en el modelo y el lugar en que se devuelven los datos.

    El **experimento de entrenamiento** se guarda todavía en las nuevas pestañas de la parte superior del lienzo de experimentos.

1. **Ejecute** el experimento.

1. Para comprobar que el modelo aún funciona, seleccione la salida del módulo **Score Model** (Puntuar modelo) y, después, **View Results** (Ver resultados). Puede ver que se muestran los datos originales, junto con el precio previsto ("Etiquetas puntuadas").

El experimento debería ser similar al siguiente:  

![Captura de pantalla que muestra la configuración esperada del experimento después de prepararlo para su implementación](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

## <a name="deploy-the-web-service"></a>Implementación del servicio web

1. Seleccione **Deploy Web Service** (Implementar servicio web) debajo del lienzo.

1. Seleccione el **destino de proceso** que desea que ejecute el servicio web.

    Actualmente, la interfaz visual solo admite la implementación en los destinos de proceso de Azure Kubernetes Service (AKS). Puede elegir entre los destinos de proceso AKS disponibles en el área de trabajo de Machine Learning Service o configure un entorno de AKS nuevo mediante los pasos del cuadro de diálogo.

    ![Captura de pantalla que muestra una posible configuración para un nuevo destino de proceso](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Seleccione **Deploy Web Service** (Implementar servicio web). Cuando finalice la implementación verá la siguiente notificación. La implementación puede tardar unos minutos.

    ![Captura de pantalla que muestra el mensaje de confirmación tras una implementación correcta.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>Prueba del servicio web

Puede probar y administrar los servicios web de la interfaz visual. Para ello, vaya a la pestaña **Servicios web**.

1. Vaya a la sección del servicio web. Verá el servicio web que ha implementado con el nombre **Tutorial - Predict Automobile Price[Predictive Exp]** (Tutorial: predecir el precio de automóviles [exp. de predicción]).

     ![Captura de pantalla que muestra la pestaña del servicio web con el servicio web recién creado resaltado](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Seleccione el nombre del servicio web para ver más detalles.

     ![Captura de pantalla que muestra los detalles adicionales que hay disponibles en la vista del servicio web](./media/ui-tutorial-automobile-price-deploy/web-service-details.png)

1. Seleccione **Probar**.

    ![Captura de pantalla que muestra la página de prueba del servicio web](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)

1. Escriba los datos de prueba o use los datos de ejemplo que se rellenan automáticamente y seleccione **Test** (Probar).

    La solicitud de prueba se envía al servicio web y los resultados se muestran en la página. Aunque se genera un valor de precio para los datos de entrada, este no se usa para generar el valor de predicción.

## <a name="consume-the-web-service"></a>Consumo del servicio web

Los usuarios ya pueden enviar solicitudes de API al servicio web de Azure y recibir los resultados para predecir el precio de sus nuevos automóviles.

**Solicitud/respuesta**: el usuario envía una o varias filas de datos de automóviles al servicio mediante un protocolo HTTP. El servicio responde con uno o varios conjuntos de resultados.

Puede encontrar en las llamadas de REST de ejemplo en la pestaña **Consume** (Consumir) de la página de detalles del servicio web.

   ![Captura de pantalla que muestra una llamada de REST de ejemplo que los usuarios pueden encontrar en la pestaña Consume (Consumir)](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Vaya a la pestaña **API Doc** (Documentación de la API) para buscar más detalles acerca de la API.

  ![Captura de pantalla que muestra detalles adicionales de la API que los usuarios pueden encontrar en la pestaña API Doc (Documentación de la API)](./media/ui-tutorial-automobile-price-deploy/web-service-api.png)

## <a name="manage-models-and-deployments"></a>Administración de modelos e implementaciones

Los modelos y las implementaciones de servicios web que cree en la interfaz visual se pueden administrar también desde el área de trabajo de Azure Machine Learning Service.

1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com/).  

1. En el área de trabajo, seleccione **Models** (Modelos). Luego, seleccione el experimento que ha creado.

    ![Captura de pantalla que muestra cómo ir a los experimentos en Azure Portal](./media/ui-tutorial-automobile-price-deploy/portal-models.png)

    En esta página, verá más detalles acerca del modelo.

    ![Captura de pantalla que muestra información general acerca de las estadísticas del experimento en Azure portal](./media/ui-tutorial-automobile-price-deploy/model-details.png)

1. Seleccione **Deployments** (implementaciones) y obtendrá una lista de todos los servicios web que usan el modelo. Seleccione el nombre del servicio web, irá a la página de detalles del servicio web. En dicha página, puede obtener información más detallada del servicio web.

    ![Captura de pantalla de informe de ejecución detallado](./media/ui-tutorial-automobile-price-deploy/deployment-details.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido los pasos clave para crear, implementar y consumir un modelo de Machine Learning en la interfaz visual. Para más información acerca de cómo usar la interfaz visual para resolver otros tipos de problemas, consulte los experimentos de ejemplo.

> [!div class="nextstepaction"]
> [Ejemplo de clasificación de riesgo crediticio](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
