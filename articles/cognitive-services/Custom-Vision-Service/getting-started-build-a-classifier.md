---
title: 'Creación de un clasificador: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Aprenda a usar el sitio web de Custom Vision para crear un modelo de clasificación de imágenes.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: f6ab2d8bcf1ae02df95b0cf36eacffa90964d43e
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243261"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Cómo crear un clasificador con Custom Vision

Para usar Custom Vision Service para la clasificación de imágenes, primero debe crear un modelo de clasificador. En esta guía, aprenderá a crear un clasificador mediante el sitio web de Custom Vision.

## <a name="prerequisites"></a>Requisitos previos

- Una [cuenta Microsoft](https://account.microsoft.com/account) válida o una cuenta de Azure Active Directory (AAD) ("cuenta profesional o educativa").

    > [!IMPORTANT] 
    > Actualmente no se admite el inicio de sesión de los usuarios de AAD desde las [nubes nacionales de Microsoft](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud).
- Un conjunto de imágenes con el que entrenar el clasificador. Consulte las siguientes sugerencias sobre cómo elegir imágenes.
- Opcionalmente: Una suscripción a Azure asociada a su cuenta Microsoft o cuenta de AAD. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. Sin una suscripción a Azure, solo podrá crear dos proyectos de __versión de evaluación de tiempo limitado__.

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

En el explorador web, vaya a la [página web de Custom Vision](https://customvision.ai) y seleccione __Sign in__ (Iniciar sesión).

![Imagen de la página de inicio de sesión](./media/browser-home.png)

Si tiene una cuenta de Azure, se le pedirá que cree recursos de entrenamiento y predicción de Custom Vision Service en [Azure Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) durante la creación del proyecto.

1. Para crear su primer proyecto, seleccione **New Project** (Nuevo proyecto). Aparece el cuadro de diálogo **Create new project** (Crear nuevo proyecto).

    ![Este cuadro de diálogo contiene campos de nombre, descripción y dominios.](./media/getting-started-build-a-classifier/new-project.png)

1. Escriba un nombre y una descripción para el proyecto. Después, seleccione un grupo de recursos. Si la cuenta con la que ha iniciado sesión está asociada a una cuenta de Azure, aparecerá el menú desplegable Resource Group (Grupo de recursos) que incluye un recurso de Custom Vision Service. En cualquier caso, también puede seleccionar la __evaluación gratuita por tiempo limitado__ en esta lista desplegable.

1. Seleccione __Classification__ (Clasificación) en __Project Types__ (Tipos de proyecto). Después, en __Classification Types__ (Tipos de clasificación), elija **Multilabel** (Multietiqueta) o **Multiclass** (Multiclase) según su caso de uso. La clasificación multietiqueta aplica un número cualquiera de etiquetas a una imagen (cero o más), mientras que la clasificación multiclase ordena las imágenes en categorías únicas (cada imagen que envíe se clasificará en la etiqueta más probable). Puede cambiar el tipo de clasificación más adelante si lo desea.

1. A continuación, seleccione uno de los dominios disponibles. Cada dominio optimiza el clasificador para determinados tipos de imágenes, como se describe en la tabla siguiente. Puede cambiar el dominio más adelante si lo desea.

    |Dominio|Propósito|
    |---|---|
    |__Genérico__| Optimizado para una amplia gama de tareas de clasificación de imágenes. Si ninguno de los otros dominios es adecuado, o no está seguro de qué dominio elegir, seleccione el dominio genérico. |
    |__Alimentos__|Optimizado para fotos de platos tal y como los vería en el menú de un restaurante. Si quiere clasificar fotos de frutas o verduras individuales, use el dominio de alimentos.|
    |__Puntos de referencia__|Optimizado para puntos de referencia reconocibles, tanto naturales como artificiales. Este dominio funciona mejor cuando el punto de referencia es claramente visible en la foto. Este dominio funciona incluso si hay gente delante del punto de referencia que obstruye parcialmente su visión.|
    |__Minoristas__|Optimizado para imágenes que se encuentran en un catálogo de compra o un sitio web de compras. Si quiere una clasificación de alta precisión entre vestidos, pantalones y camisas, use este dominio.|
    |__Adultos__|Optimizado para definir mejor el contenido para adultos y el contenido que no es para adultos. Por ejemplo, si quiere bloquear imágenes de personas en traje de baño, este dominio le permite crear un clasificador personalizado para hacerlo.|
    |__Dominios compactos__| Optimizados para las restricciones de clasificación en tiempo real en dispositivos móviles. Los modelos generados por los dominios compactos se pueden exportar para ejecutarse localmente.|
    
1. Por último, seleccione __Create project__ (Crear proyecto).

## <a name="choose-training-images"></a>Elección de las imágenes de entrenamiento

Como mínimo, se recomienda que use 30 imágenes por etiqueta en el conjunto de entrenamiento inicial. También conviene recopilar algunas imágenes adicionales para probar el modelo una vez que está entrenado.

Para entrenar el modelo de forma eficaz, use imágenes con variedad visual. Seleccione imágenes con esa variedad de:
* ángulos de cámara
* iluminación
* background
* estilo visual
* sujetos individuales o grupos
* size
* Tipo

Además, asegúrese de que todas las imágenes de entrenamiento cumplen los criterios siguientes:
* tienen el formato .jpg, .png o .bmp
* tienen menos de 6 MB de tamaño (4 MB en el caso de imágenes de predicción)
* tienen más de 256 píxeles en el borde más corto. Custom Vision Service escalará verticalmente y de forma automática todas las imágenes que sean más cortas

## <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

En esta sección cargará y etiquetará manualmente las imágenes para ayudar a entrenar el clasificador. 

1. Para agregar imágenes, haga clic en el botón __Add images__ (Agregar imágenes) y seleccione __Browse local files__ (Examinar archivos locales). Seleccione __Open__ (Abrir) para empezar a etiquetar. La selección de etiquetas se aplicará a todo el grupo de imágenes que ha seleccionado para cargar, por eso es más fácil cargar imágenes en grupos independientes según sus etiquetas deseadas. También puede cambiar las etiquetas de las imágenes individuales después de haberlas cargado.

    ![El control para agregar imágenes se muestra en la parte superior izquierda y como un botón en la parte inferior central.](./media/getting-started-build-a-classifier/add-images01.png)


1. Para crear una etiqueta, escriba texto en el campo __My Tags__ (Mis etiquetas) y pulse ENTRAR. Si la etiqueta ya existe, aparecerá en un menú desplegable. En un proyecto multietiqueta, puede agregar más de una etiqueta a las imágenes, pero en un proyecto multiclase solo puede agregar una. Para terminar la carga de las imágenes, use el botón __Upload [number] files__ (Cargar [número] archivos). 

    ![Imagen de la página de etiqueta y carga](./media/getting-started-build-a-classifier/add-images03.png)

1. Una vez cargadas las imágenes, seleccione __Done__ (Hecho).

    ![La barra de progreso muestra todas las tareas completadas.](./media/getting-started-build-a-classifier/add-images04.png)

Para cargar otro conjunto de imágenes, vuelva a la parte superior de esta sección y repita los pasos. En algún punto del proyecto, puede que tenga que agregar _ejemplos negativos_ para ayudar a que el clasificador sea más preciso. Ejemplos negativos son aquellos que no coinciden con ninguna de las otras etiquetas. Cuando cargue estas imágenes, aplíqueles la etiqueta especial **Negative** (Negativo).

## <a name="train-the-classifier"></a>Entrenamiento del clasificador

Para entrenar al clasificador, seleccione el botón **Train** (Entrenar). El clasificador usa todas las imágenes actuales para crear un modelo que identifica las calidades visuales de cada etiqueta.

![Botón Train (Entrenar) en la parte superior derecha de la barra de herramientas del encabezado de la página web](./media/getting-started-build-a-classifier/train01.png)

El proceso de entrenamiento solo debe llevar unos minutos. Durante este tiempo, se muestra información sobre el proceso de entrenamiento en la pestaña **Performance** (Rendimiento).

![Ventana del explorador con un cuadro de diálogo de entrenamiento en la sección principal](./media/getting-started-build-a-classifier/train02.png)

## <a name="evaluate-the-classifier"></a>Evaluación del clasificador

Una vez finalizado el entrenamiento, se calcula el rendimiento del modelo y se muestra. Custom Vision Service usa las imágenes que ha enviado para entrenamiento para calcular la precisión y la coincidencia, mediante un proceso denominado [validación cruzada de k iteraciones](https://en.wikipedia.org/wiki/Cross-validation_(statistics)). La precisión y la coincidencia constituyen dos medidas diferentes de la eficacia de un clasificador:

- La **precisión** indica la fracción de las clasificaciones identificadas que fueron correctas. Por ejemplo, si el modelo identificó 100 imágenes como perros y 99 de ellas eran realmente de perros, la precisión sería del 99 %.
- La **coincidencia** indica la fracción de las clasificaciones reales que se identificaron correctamente. Por ejemplo, si había realmente 100 imágenes de manzanas y el modelo identificó 80 como manzanas, la coincidencia sería del 80 %.

![Los resultados de entrenamiento muestran la precisión y recuperación globales, junto con la precisión y recuperación de cada etiqueta en el clasificador.](./media/getting-started-build-a-classifier/train03.png)

### <a name="probability-threshold"></a>Probability Threshold (Umbral de probabilidad)

Observe el control deslizante **Probability Threshold** (Umbral de probabilidad) situado en el panel izquierdo de la pestaña **Performance** (Rendimiento). Este es el umbral para que una probabilidad predicha se considere correcta al calcular la precisión y la coincidencia.

La interpretación de las llamadas de predicción con un umbral de alta probabilidad tiende a devolver resultados con alta precisión a costa del índice de coincidencia (las clasificaciones encontradas son correctas, pero muchas no se han encontrado). Por el contrario, un umbral de baja probabilidad hace lo contrario (se encontraron la mayoría de las clasificaciones reales pero había falsos positivos en el conjunto). Teniendo esto en cuenta, debe establecer el umbral de probabilidad según las necesidades específicas de su proyecto. Posteriormente, en el lado cliente, debe usar el mismo valor de umbral de probabilidad como filtro al recibir los resultados de predicción del modelo.

## <a name="manage-training-iterations"></a>Administración de iteraciones de entrenamiento

Cada vez que entrena al clasificador, se crea una nueva _iteración_ con sus propias métricas de rendimiento actualizadas. Puede ver todas las iteraciones en el panel izquierdo de la pestaña **Performance** (Rendimiento). Si selecciona una, puede convertirla en la _iteración predeterminada_. Para ello, haga clic en el botón **Make default** (Convertir en predeterminada) de la parte superior. La _iteración predeterminada_ es el modelo que se usará de forma predeterminada cuando realice consultas con Prediction API (desde una aplicación, por ejemplo). Si rechaza la actualización de la _iteración predeterminada_, podrá continuar entrenando al modelo sin que ello afecte al comportamiento actual de la aplicación. Posteriormente, una vez que esté satisfecho con el modelo mejorado, podrá actualizar la iteración predeterminada.

En el panel izquierdo encontrará también el botón **Delete** (Eliminar), que puede usar para eliminar una iteración si está obsoleta. Cuando se elimina una iteración, elimina las imágenes que están asociadas exclusivamente a ella.

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido a crear y entrenar un modelo de clasificación de imágenes mediante el sitio web de Custom Vision. A continuación, obtenga más información sobre el proceso iterativo de mejora del modelo.

[Prueba y reentrenamiento del modelo](test-your-model.md)

