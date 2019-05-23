---
title: 'Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual'
titleSuffix: Azure Machine Learning service
description: Aprenda a cómo crear una solución de análisis predictivo en la interfaz visual de Azure Machine Learning Service. Entrene, puntúe e implemente un modelo de Machine Learning mediante módulos de arrastrar y colocar. Este tutorial es la segunda de una serie de dos partes sobre la predicción de precios de automóviles mediante la regresión lineal.
author: peterclu
ms.author: peterclu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: fe26417c6e3a9f159e884c19d7bd7c9dc7569229
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787829"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Tutorial: Implementación de un modelo de Machine Learning con la interfaz visual

En este tutorial, se amplía el examen de cómo desarrollar una solución predictiva en la interfaz visual de Azure Machine Learning Service. Este tutorial es la **segunda parte de dos**. En [la primera parte del tutorial](ui-tutorial-automobile-price-train-score.md), ha entrenado, puntuado y evaluado un modelo para predecir los precios de los automóviles. En esta parte del tutorial, va a:

> [!div class="checklist"]
> * Preparar un modelo para la implementación
> * Implementación de un servicio web
> * Probara un servicio web
> * Administrar un servicio web
> * Consumo del servicio web

## <a name="prerequisites"></a>Requisitos previos

Completar la [parte uno del tutorial](ui-tutorial-automobile-price-train-score.md).

## <a name="prepare-for-deployment"></a>Preparación de la implementación

Para que otros usuarios puedan usar el modelo predictivo desarrollado en este tutorial, se puede implementar como servicio web de Azure.

Hasta ahora ha estado experimentando con el entrenamiento del modelo. Ahora, es momento de generar predicciones basadas en los datos que escriba el usuario.

La preparación para la implementación es un proceso que consta de dos pasos:  

1. Convertir el *experimento de entrenamiento* que ha creado en un *experimento predictivo*
1. Implementar el experimento predictivo como servicio web

Puede que desee hacer una copia del experimento antes; para ello, seleccione **Save As** (Guardar como) en la parte inferior del lienzo de experimento.

### <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Convertir un experimento de entrenamiento en experimento predictivo

Para que este modelo esté listo para la implementación, convierta este experimento de entrenamiento en un experimento de predicción. Normalmente, este es un proceso de tres pasos:

1. Guardar el modelo que ha entrenado y sustituir los módulos de entrenamiento.
1. Recortar el experimento para quitar los módulos que fueron necesarios solo para el entrenamiento.
1. Definir el lugar en que el servicio web aceptará los datos de entrada y el lugar en que generará la salida.

Estos pasos se pueden realizar manualmente, o bien puede seleccionar **Set Up Web Service** (Configurar servicio web) en la parte inferior del lienzo del experimento para que se realicen automáticamente.

![Gif animado que muestra la conversión automática de un experimento de entrenamiento en un experimento de predicción](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

Al seleccionar **Set Up Web Service** (Configurar servicio web), se producen varias cosas:

* El modelo entrenado se convierte en un módulo **Trained Model** (Modelo entrenado). Se almacena en la paleta de módulos de la izquierda del lienzo de experimentos. Puede encontrarlo en **Trained Models** (Modelos entrenados).
* Se quitan los módulos que se utilizaron para el entrenamiento, en particular:
  * Train Model (Entrenar modelo)
  * Split Data (Dividir datos)
  * Evaluate Model (Evaluar modelo)
* El modelo entrenado guardado se vuelve a agregar al experimento.
* Se agregan los módulos de **entrada de servicio web** y de **salida de servicio web**. Estos módulos identifican el lugar en que los datos del usuario entrarán en el modelo y el lugar en que se devuelven los datos.

Puede ver que el experimento se guarda en dos partes en las pestañas nuevas que están en la parte superior del lienzo del experimento. El experimento de entrenamiento original está en la pestaña **Experimento de entrenamiento**, y el experimento de predicción recién creado está en **Experimento predictivo**. El experimento predictivo es el que se va a implementar como servicio web.

El experimento debería ser similar al siguiente:  

![Captura de pantalla que muestra la configuración esperada del experimento después de prepararlo para su implementación](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

Ejecute el experimento por última vez (seleccione **Ejecutar**). Elija el destino de proceso en que desea que se ejecute el experimento en el cuadro de diálogo emergente. Para comprobar que el modelo aún funciona, seleccione la salida del módulo Score Model (Puntuar modelo) y, después, **View Results** (Ver resultados). Puede ver que se muestran los datos originales, junto con el precio previsto ("Etiquetas puntuadas").

## <a name="deploy-the-web-service"></a>Implementación del servicio web

Para implementar un servicio web nuevo derivado del experimento:

1. Seleccione **Deploy Web Service** (Implementar servicio web) debajo del lienzo.
1. Seleccione el **destino de proceso** que desea que ejecute el servicio web.

    Actualmente, la interfaz visual solo admite la implementación en los destinos de proceso de Azure Kubernetes Service (AKS). Puede elegir entre los destinos de proceso AKS disponibles en el área de trabajo de Machine Learning Service o configure un entorno de AKS nuevo mediante los pasos del cuadro de diálogo.

    ![Captura de pantalla que muestra una posible configuración para un nuevo destino de proceso](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Seleccione **Deploy Web Service** (Implementar servicio web). Cuando finalice la implementación verá la siguiente notificación. La implementación puede tardar unos minutos.

    ![Captura de pantalla que muestra el mensaje de confirmación tras una implementación correcta.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>Prueba del servicio web

Los datos de entrada del usuario entran en el modelo implementado a través del módulo **Web service input** (Entrada del servicio web). Después, la entrada se puntúa en el módulo **Score Model** (Puntuar modelo). De la forma que ha configurado el experimento de predicción, el modelo espera los datos en el mismo formato que el conjunto de datos de precios de automóviles. Por último, los resultados se devuelven al usuario a través del módulo **Web service output** (Salida del servicio web).

Puede probar un servicio web en la pestaña del servicio web en la interfaz visual.

1. Vaya a la sección del servicio web. Verá el servicio web que ha implementado con el nombre **Tutorial - Predict Automobile Price[Predictive Exp]** (Tutorial: predecir el precio de automóviles [exp. de predicción]).

     ![Captura de pantalla que muestra la pestaña del servicio web con el servicio web recién creado resaltado](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Seleccione el nombre del servicio web para ver más detalles.

     ![Captura de pantalla que muestra los detalles adicionales que hay disponibles en la vista del servicio web](./media/ui-tutorial-automobile-price-deploy/web-service-details.png)

1. Seleccione **Probar**.

    ![Captura de pantalla que muestra la página de prueba del servicio web](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)

1. Escriba los datos de prueba o use los datos de ejemplo que se rellenan automáticamente y seleccione **Test** (Probar) en la parte inferior. La solicitud de prueba se envía al servicio web y los resultados se muestran en la página.

## <a name="manage-the-web-service"></a>Administración del servicio web

Una vez que haya implementado el servicio web, puede administrarlo desde la pestaña **Servicios web** de la interfaz visual.

Para eliminar un servicio web, seleccione **Delete** (Eliminar) en la página de detalles del servicio web.

   ![Captura de pantalla que muestra la ubicación del botón Delete web service (Eliminar servicio web) en la parte inferior de la ventana](./media/ui-tutorial-automobile-price-deploy/web-service-delete.png)

## <a name="consume-the-web-service"></a>Consumo del servicio web

En los pasos anteriores de este tutorial, ha implementado un modelo de predicción de automóviles como un servicio web de Azure. Ahora los usuarios pueden enviarle y recibir datos a través de API REST.

**Solicitud/respuesta**: el usuario envía una o varias filas de datos de automóviles al servicio mediante un protocolo HTTP. El servicio responde con uno o varios conjuntos de resultados.

Puede encontrar en las llamadas de REST de ejemplo en la pestaña **Consume** (Consumir) de la página de detalles del servicio web.

   ![Captura de pantalla que muestra una llamada de REST de ejemplo que los usuarios pueden encontrar en la pestaña Consume (Consumir)](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Vaya a la pestaña **API Doc** (Documentación de la API) para buscar más detalles acerca de la API.

  ![Captura de pantalla que muestra detalles adicionales de la API que los usuarios pueden encontrar en la pestaña API Doc (Documentación de la API)](./media/ui-tutorial-automobile-price-deploy/web-service-api.png)

## <a name="manage-models-and-deployments-in-azure-machine-learning-service-workspace"></a>Administración de modelos e implementaciones en el área de trabajo de Azure Machine Learning Service

Los modelos y las implementaciones de servicios web que cree en la interfaz visual se pueden administrar desde el área de trabajo de Azure Machine Learning Service.

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
