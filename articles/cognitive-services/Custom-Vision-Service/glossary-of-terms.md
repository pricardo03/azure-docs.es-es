---
title: 'Glosario de términos: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Glosario de términos de Custom Vision Service
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 05/08/2017
ms.author: anroth
ms.openlocfilehash: e659367ae13026dbe48ed681d0a68058d686e3ec
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884357"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Glosario de términos de Custom Vision Service

Estos son algunos de los términos utilizados en Custom Vision Service y su significado.

## <a name="classifier"></a>Clasificador

Un clasificador es un modelo que se crea con Custom Vision Service, mediante el uso de algunas imágenes de entrenamiento. Cuando haya terminado de entrenar un nuevo clasificador, recibirá un punto de conexión de evaluación (HTTPS), que puede agregar a la aplicación. Cada clasificador que compile está en su propio proyecto, y puede ver todos los proyectos después de iniciar sesión.

## <a name="domain"></a>Dominio

Cuando se crea un proyecto, se selecciona un "dominio" para ese proyecto. El dominio optimiza un clasificador para un tipo específico del objeto en las imágenes. Por ejemplo, si el escenario es clasificar imágenes entre tarta de manzana y tarta de zanahoria, seleccione el dominio "Food" (Comidas). Si no está seguro de qué dominio elegir, seleccione el dominio "Generic" (Genérico).

- **Dominio Food (Comidas).** Optimizado para los platos que vería en el menú de un restaurante. No se ha optimizado para reconocer frutas o verduras individuales. Si desea clasificar fotografías de frutas o verduras individuales, utilice el dominio genérico para ese fin.
- **Dominio Landmark (Lugares de interés).** Optimizado para lugares de interés reconocibles, tanto naturales como artificiales. Este dominio funciona mejor si el lugar de interés es claramente visible en la fotografía, aunque aparezca ligeramente tapado por un grupo de personas situadas frente a él.
- **Dominio Retail (Tienda).** Optimizado para clasificar imágenes que se encuentran en un catálogo de compra o un sitio web de compras. Si desea una alta precisión al clasificar vestidos, pantalones, camisas, etc., use el dominio Retail.
- **Dominio Adult (Adultos).** Optimizado para definir mejor el contenido para adultos y el contenido que no es para adultos. Por ejemplo, si quiere bloquear imágenes de personas en traje de baño, este dominio le permite crear un clasificador personalizado para hacerlo.
- **Dominio General.** Idóneo para una amplia gama de tareas de clasificación de imágenes.

Los modelos generados por los **dominios compactos** se pueden exportar con la funcionalidad de exportación de iteración. Están optimizados para las restricciones de clasificación en tiempo real en dispositivos móviles. Los clasificadores creados con un dominio compacto podrían ser un poco menos precisos que los que usan un dominio estándar con la misma cantidad de datos de entrenamiento. La contrapartida es que son lo suficientemente pequeños como para ejecutarse localmente prácticamente en tiempo real. 

## <a name="training-image"></a>Imagen de entrenamiento

Para crear un clasificador de alta precisión, Custom Vision Service necesita varias imágenes de entrenamiento. Una imagen de entrenamiento es una fotografía de la imagen que desea que Custom Vision Service clasifique. Por ejemplo, para clasificar naranjas, necesitaría cargar varias imágenes de naranjas a Custom Vision Service para que servicio pueda crear un clasificador que pueda reconocer naranjas. Se recomienda al menos 30 imágenes por etiqueta.

## <a name="iteration"></a>Iteración

Cada vez que entrene o vuelva a entrenar el clasificador, se crea una nueva iteración del modelo. Mantenemos varias iteraciones anteriores para que pueda comparar el progreso a lo largo del tiempo. Puede eliminar cualquier iteración que ya no le resulte útil. Recuerde que la eliminación de una iteración es permanente y también se eliminarán las imágenes o los cambios que sean exclusivos de esa iteración. 

## <a name="workspace"></a>Área de trabajo

El área de trabajo contiene todas las imágenes de entrenamiento y refleja todos los cambios desde la última iteración, por ejemplo, las imágenes quitadas o agregadas. Cuando se entrena el clasificador, se crea una nueva iteración del mismo con las imágenes presentes en el área de trabajo.

## <a name="tags"></a>Etiquetas

Utilice etiquetas para etiquetar los objetos en las imágenes de entrenamiento. Si va a crear un clasificador para identificar perros y ponis, tendría que poner una etiqueta "perro" en las imágenes que contienen perros, una etiqueta "poni" en las imágenes que contienen ponis y etiquetas "perro" y "poni" en las imágenes que contienen perros y ponis.

## <a name="evaluation"></a>Evaluación

Después de entrenar el clasificador, puede enviar cualquier imagen para su evaluación con el punto de conexión https generado automáticamente. El clasificador devuelve un conjunto de etiquetas de predicción, en orden de confianza.

## <a name="predictions"></a>Predicciones

A medida que el clasificador acepta nuevas imágenes para clasificar, almacena las imágenes automáticamente. Puede utilizar estas imágenes para mejorar la precisión del clasificador, marcando correctamente las imágenes que se habían clasificado incorrectamente. Estas nuevas imágenes se pueden usar para volver a entrenar el clasificador.

## <a name="precision"></a>Precision

Al clasificar una imagen, ¿cuál es la probabilidad de que el clasificador la clasifique correctamente? De todas las imágenes utilizadas para entrenar el clasificador (perros y ponis), ¿qué porcentaje realizó el modelo correctamente? 99 etiquetas correctas de 100 imágenes da una precisión del 99 %.

## <a name="recall"></a>Recuperación

De todas las imágenes que se deberían haber clasificado correctamente, ¿cuántas identificó el clasificador correctamente? Un resultado del 100 % significaría que si había 38 imágenes de perros en las imágenes utilizadas para entrenar el clasificador, el clasificador encontró 38 perros.

## <a name="settings"></a>Configuración

Hay dos tipos de configuración, la configuración en el nivel de proyecto y la configuración en el nivel de usuario.

- Configuración en el nivel de proyecto: 
  
  La configuración en el nivel de proyecto se aplica a un proyecto o clasificador. Entre ellos se incluyen los siguientes:

   - Dominio del proyecto
   - Nombre de proyecto
   - Descripción del proyecto
   - Uso:
      - Número de imágenes de entrenamiento
      - Número de etiquetas creadas
      - Número de iteraciones creadas

- Configuración en el nivel de usuario: 
   - Claves de suscripción: una para entrenamiento y una para predicción y evaluación.
   - Uso:
      - Número de proyectos creados
      - Número de llamadas API de evaluación o predicción realizadas.
