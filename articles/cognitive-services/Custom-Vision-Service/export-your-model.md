---
title: 'Exportación del modelo para su uso con dispositivos móviles: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Vea cómo exportar el modelo para su uso en la creación de aplicaciones móviles.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 50417e9e1722e69c24185c05ea5e286e6b13e8c7
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364948"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Exportación del modelo para su uso con dispositivos móviles

Custom Vision Service permite exportar los clasificadores para ejecutarlos sin conexión. Puede insertar el clasificador exportado en una aplicación y ejecutarla localmente en un dispositivo para realizar una clasificación en tiempo real. 

Custom Vision Service admite las exportaciones siguientes:

* __Tensorflow__ para __Android__.
* __CoreML__ para __iOS11__.
* __ONNX__ para __Windows ML__.
* Un __contenedor__ Windows o Linux. El contenedor incluye un modelo de Tensorflow y código de servicio para usar la API de Custom Vision Service. 

> [!IMPORTANT]
> Custom Vision Service solo exporta dominios __compactos__. Los modelos generados por los dominios compactos están optimizados para las restricciones de la clasificación en tiempo real en dispositivos móviles. Los clasificadores creados con un dominio compacto podrían ser un poco menos precisos que los que usan un dominio estándar con la misma cantidad de datos de entrenamiento.
>
> Para más información sobre cómo mejorar los clasificadores, consulte el documento [Improving your classifier](getting-started-improving-your-classifier.md) (Mejora del clasificador).

## <a name="convert-to-a-compact-domain"></a>Conversión a un dominio compacto

> [!NOTE]
> Los pasos descritos en esta sección solo se aplican si tiene un clasificador que no se ha establecido para un dominio compacto.
 
Para convertir el dominio de un clasificador existente, siga estos pasos:

1. En la [página de Custom Vision](https://customvision.ai), seleccione el icono __Inicio__ para ver una lista de los proyectos. También puede usar [ https://customvision.ai/projects ](https://customvision.ai/projects) para ver los proyectos.

    ![Imagen del icono Inicio y la lista de proyectos](./media/export-your-model/projects-list.png)

2. Seleccione un proyecto y, a continuación, seleccione el icono de __engranaje__ en la esquina superior derecha de la página.

    ![Imagen del icono de engranaje](./media/export-your-model/gear-icon.png)

3. En la sección __Domains__ (Dominios), seleccione un dominio __compacto__. Seleccione __Save Changes__ (Guardar cambios) para guardar los cambios.

    ![Imagen de la selección de dominio](./media/export-your-model/domains.png)

4. En la parte superior de la página, seleccione __Train__ (Entrenar) para volver a entrenar el modelo con el nuevo dominio.

## <a name="export-your-model"></a>Exportar el modelo

Para exportar el modelo después de volver a entrenarlo, siga estos pasos:

1. Vaya a la pestaña **Performance** (Rendimiento) y seleccione __Export__ (Exportar). 

    ![Imagen del icono de exportación](./media/export-your-model/export.png)

    > [!TIP]
    > Si la entrada __Export__ (Exportar) no está disponible, la iteración seleccionada no usa un dominio compacto. Use la sección __Iterations__ de esta página para seleccionar una iteración que usa un dominio compacto y, a continuación, seleccione __Exportar__.

2. Seleccione el formato de exportación y, a continuación, seleccione __Export__ (Exportar) para descargar el modelo.

## <a name="next-steps"></a>Pasos siguientes

Integre el modelo exportado en una aplicación. Hay varias aplicaciones de ejemplo:

* Ejemplo de cómo [usar el modelo de CoreML exportado en una aplicación de iOS](https://go.microsoft.com/fwlink/?linkid=857726) para la clasificación de imágenes en tiempo real con Swift
* Aplicación de iOS de ejemplo para [usar el modelo de CoreML exportado con Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) para la clasificación de imágenes en tiempo real 
* Ejemplo de cómo [usar el modelo de CoreML exportado en una aplicación de Android](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) para la clasificación de imágenes en tiempo real 
* [Uso del modelo de Tensorflow con Windows](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-model-python)
* Ejemplo para [usar el modelo ONNX exportado con Azure Machine Learning](https://azure.microsoft.com/resources/samples/cognitive-services-onnx-customvision-sample/)
