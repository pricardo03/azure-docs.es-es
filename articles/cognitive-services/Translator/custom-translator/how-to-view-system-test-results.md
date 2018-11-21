---
title: 'Ver resultados de pruebas del sistema e implementación: Custom Translator'
titleSuffix: Azure Cognitive Services
description: Cuando el entrenamiento se realice correctamente, revise las pruebas del sistema para analizar los resultados del entrenamiento. Si está satisfecho con los resultados del entrenamiento, envíe una solicitud de implementación para el modelo entrenado.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 61d3869559d88e14c0b9a3c3e23cd8a1f9c2b6c4
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627189"
---
# <a name="view-system-test-results"></a>Ver resultados de pruebas del sistema

Cuando el entrenamiento se realice correctamente, revise las pruebas del sistema para analizar los resultados del entrenamiento. Si está satisfecho con los resultados del entrenamiento, envíe una solicitud de implementación para el modelo entrenado. 

## <a name="system-test-results-page"></a>Página de resultados de pruebas del sistema

Seleccione un proyecto y, a continuación, seleccione la pestaña de modelos de ese proyecto, busque el modelo que quiere usar y, por último, seleccione la pestaña de pruebas.

En la pestaña de pruebas se muestra lo siguiente:

1.  Resultados de pruebas del sistema: el resultado del proceso de pruebas del entrenamiento. El proceso de pruebas genera la puntuación BLEU.

    **Recuento de oraciones:** cuántas oraciones paralelas se usaron en el conjunto de pruebas.

     **Puntuación BLEU:** puntuación BLEU generada para un modelo tras la finalización del entrenamiento.

    **Estado:** indica si se ha completado el proceso de prueba o si está en curso.

    ![Resultados de pruebas del sistema](media/how-to/how-to-system-test-results.png)

2.  Haga clic en los resultados de pruebas del sistema y se mostrará la página de detalles del resultado de las pruebas. En esta página se muestra la traducción automática de las oraciones que formaban parte del conjunto de datos de prueba.

3.  La tabla en la página de detalles del resultado de pruebas tiene dos columnas: una para cada idioma en el par. La columna del idioma de origen muestra la oración que se va a traducir. La columna para el idioma de destino contiene dos oraciones por cada fila.

    **Ref:** esta oración es la traducción de referencia para la oración de origen tal como se especificó en el conjunto de datos de prueba.

    **MT:** esta oración es la traducción automática de la oración de origen realizada por el modelo creado después de llevar a cabo el entrenamiento.

    ![Comparación de los resultados de pruebas del sistema](media/how-to/how-to-system-test-results-2.png)

## <a name="download-test"></a>Descarga de la prueba

Haga clic en el vínculo Download Translations (Descargar traducciones) para descargar un archivo zip. El archivo zip contiene las traducciones automáticas de las oraciones de origen en el conjunto de datos de pruebas.

![Descargar la prueba](media/how-to/how-to-system-test-download.png)

Este archivo zip descargado contiene tres archivos.

1.  custom.mt.txt: contiene la traducción automática de las oraciones del idioma de origen al idioma de destino que realizó el modelo entrenado con los datos del usuario.

2.  ref.txt: contiene las traducciones de las oraciones del idioma de origen proporcionadas por el usuario en el idioma de destino.

3.  source.txt: contiene las oraciones en el idioma de origen.

    ![Resultados de pruebas del sistema descargados](media/how-to/how-to-download-system-test.png)

## <a name="deploy-a-model"></a>Implementar un modelo

Para solicitar una implementación, haga lo siguiente:

1.  Seleccione un proyecto y vaya a la pestaña de modelos.

2. Para cada modelo entrenado correctamente, se muestra un botón "Implementar" si aún no se implementa.

    ![Implementación del modelo](media/how-to/how-to-deploy-model.png)

3.  Haga clic en Implementar.
4.  Seleccione las regiones donde quiere que se implemente el modelo y haga clic en Guardar. Puede seleccionar varias regiones.

    ![Implementación del modelo](media/how-to/how-to-deploy-model-regions.png)

5.  Puede ver el estado del modelo en la columna "Estado".

>[!Note]
>Si ya se ha implementado un modelo, verá un botón "Anular la implementación" para ese modelo. Para anular la implementación de un modelo, haga clic en "Anular la implementación".

## <a name="next-steps"></a>Pasos siguientes

- Comience a utilizar el modelo de traducción personalizado implementado mediante [Microsoft Translator Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
- Obtenga información sobre [cómo administrar la configuración](how-to-manage-settings.md) para compartir el área de trabajo y administrar la clave de suscripción.
- Obtenga información sobre [cómo migrar áreas de trabajo y proyectos](how-to-migrate.md) desde [Microsoft Translator Hub](https://hub.microsofttranslator.com)