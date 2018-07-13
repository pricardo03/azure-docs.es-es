---
title: Creación de un clasificador con Custom Vision Service en Azure Cognitive Services | Microsoft Docs
description: Aprenda a usar Custom Vision Service para crear un clasificador que pueda distinguir objetos en fotos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: 6dc271c13f53a445c7d1101f5264d890208bd03c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381030"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Cómo crear un clasificador con Custom Vision

Para usar Custom Vision Service, primero debe crear un clasificador. En este documento, aprenderá a crear un clasificador mediante su explorador web.

## <a name="prerequisites"></a>requisitos previos

Para crear un clasificador, necesita:

- Una [cuenta Microsoft](https://account.microsoft.com/account) válida o una cuenta profesional o educativa (OrgID) de Azure Active Directory, para poder iniciar sesión y empezar a trabajar en customvision.ai.

    > [!IMPORTANT] 
    > El inicio de sesión con OrgID para usuarios de Azure Active Directory (Azure AD) desde [nubes nacionales](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) no se admite actualmente.

- Una serie de imágenes para entrenar al clasificador (con 30 imágenes como mínimo por etiqueta).

- Algunas imágenes para probar el clasificador una vez entrenado.

- Una suscripción a Azure asociada a su cuenta Microsoft o OrgID (opcional). Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

    > [!IMPORTANT]
    > Sin una suscripción a Azure, solo podrá crear proyectos de __versión de evaluación de tiempo limitado__. Si tiene una suscripción a Azure, se le pedirá que cree recursos de entrenamiento y predicción de Custom Vision Service en [Azure Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) durante la creación del proyecto.   

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

Para crear un proyecto, use los pasos siguientes:

1. En el explorador web, vaya a la [página web de Custom Vision](https://customvision.ai). Seleccione __Sign in__ (Iniciar sesión) para comenzar a usar el servicio.

    ![Imagen de la página de inicio de sesión](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Después de iniciar sesión en Custom Vision Service, se le presenta una lista de proyectos. Aparte de los dos proyectos de "evaluación gratuita de tiempo limitado" para pruebas, los proyectos están asociados con un recurso de Azure. Si es un usuario de Azure, verá todos los proyectos asociados a [recursos de Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) a los que tenga acceso. 

2. Para crear su primer proyecto, seleccione **New Project** (Nuevo proyecto). En su primer proyecto, se le pide que acepte las condiciones del servicio. Active la casilla y luego seleccione el botón **I agree** (Acepto). Aparece el cuadro de diálogo **New project** (Nuevo proyecto).

    ![Este cuadro de diálogo contiene campos de nombre, descripción y dominios.](./media/getting-started-build-a-classifier/new-project.png)

3. Escriba un nombre y una descripción para el proyecto. A continuación, seleccione uno de los dominios disponibles. Cada dominio optimiza el clasificador para determinados tipos de imágenes, como se describe en la tabla siguiente:

    |Dominio|Propósito|
    |---|---|
    |__Genérico__| Optimizado para una amplia gama de tareas de clasificación de imágenes. Si ninguno de los otros dominios es adecuado, o no está seguro de qué dominio elegir, seleccione el dominio genérico. |
    |__Alimentos__|Optimizado para fotos de platos tal y como los vería en el menú de un restaurante. Si quiere clasificar fotos de frutas o verduras individuales, use el dominio de alimentos.|
    |__Puntos de referencia__|Optimizado para puntos de referencia reconocibles, tanto naturales como artificiales. Este dominio funciona mejor cuando el punto de referencia es claramente visible en la foto. Este dominio funciona incluso si hay gente delante del punto de referencia que obstruye parcialmente su visión.|
    |__Minoristas__|Optimizado para imágenes que se encuentran en un catálogo de compra o un sitio web de compras. Si quiere una clasificación de alta precisión entre vestidos, pantalones y camisas, use este dominio.|
    |__Adultos__|Optimizado para definir mejor el contenido para adultos y el contenido que no es para adultos. Por ejemplo, si quiere bloquear imágenes de personas en traje de baño, este dominio le permite crear un clasificador personalizado para hacerlo.|
    |__Dominios compactos__| Optimizados para las restricciones de clasificación en tiempo real en dispositivos móviles. Los modelos generados por los dominios compactos se pueden exportar para ejecutarse localmente.|

    Puede cambiar el dominio más adelante si lo desea.

4. Seleccione un grupo de recursos. La lista desplegable de grupo de recursos muestra todos los grupos de recursos de Azure que incluyen un recurso de Custom Vision Service. También puede seleccionar la __evaluación gratuita de tiempo limitado__. La entrada de evaluación gratuita de tiempo limitado es el único grupo de recursos del que podrá elegir un usuario que no sea de Azure.

    Para crear el proyecto, seleccione __Create project__ (Crear proyecto).

## <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

1. Para agregar imágenes al clasificador, use el botón __Add images__ (Agregar imágenes) y seleccione __Browse local files__ (Examinar archivos locales). Seleccione __Open__ (Abrir) para empezar a etiquetar.

    > [!TIP]
    > Después de seleccionar las imágenes, debe etiquetarlas. La etiqueta se aplica al grupo de imágenes que ha seleccionado para cargar, por lo que puede ser más fácil cargar las imágenes por las etiquetas que tiene pensado usar. También puede cambiar la etiqueta de las imágenes seleccionadas después de haberlas etiquetado y cargado.

    > [!TIP]
    > Cargue imágenes con diferentes ángulos de cámara, iluminación, fondo, tipos, estilos, grupos, tamaños, etc. Use diversos tipos de foto para garantizar que no hay sesgos en el clasificador y puede generalizar bien.

    Custom Vision Service acepta imágenes de entrenamiento en los formatos .jpg, .png y .bmp, con un tamaño de hasta 6 MB cada una. (Las imágenes de predicción pueden tener un tamaño de hasta 4 MB cada una). Se recomienda que las imágenes sean de 256 píxeles en el borde más corto. En caso contrario, Custom Vision Service las escala verticalmente.

    ![El control para agregar imágenes se muestra en la parte superior izquierda y como un botón en la parte inferior central.](./media/getting-started-build-a-classifier/add-images01.png)

    ![El botón para examinar archivos locales se muestra cerca de la parte inferior central.](./media/getting-started-build-a-classifier/add-images02.png)

    >[!NOTE] 
    > La API REST se puede usar para cargar imágenes de entrenamiento desde direcciones URL.

2. Para establecer la etiqueta, escriba texto en el campo __My Tags__ (Mis etiquetas) y luego use el botón __+__. Para cargar las imágenes y etiquetarlas, use el botón __Upload [number] files__ (Cargar [número] archivos). Puede agregar más de una etiqueta a las imágenes. 

    > [!NOTE]
    > El tiempo de carga varía según el número y tamaño de las imágenes que ha seleccionado.

    ![Imagen de la página de etiqueta y carga](./media/getting-started-build-a-classifier/add-images03.png)

3. Una vez cargadas las imágenes, seleccione __Done__ (Hecho).

    ![La barra de progreso muestra todas las tareas completadas.](./media/getting-started-build-a-classifier/add-images04.png)

4. Para cargar otro conjunto de imágenes, vuelva al paso 1. Por ejemplo, si quiere distinguir entre perros y ponis, cargue y etiquete imágenes de ponis.

## <a name="train-and-evaluate-the-classifier"></a>Entrenamiento y evaluación del clasificador

Para entrenar al clasificador, seleccione el botón **Train** (Entrenar).

![El botón de entrenamiento se encuentra cerca de la parte superior derecha de la ventana del explorador.](./media/getting-started-build-a-classifier/train01.png)

Solo se tarda unos minutos en entrenar al clasificador. Durante este tiempo, se muestra información sobre el proceso de entrenamiento.

![El botón de entrenamiento se encuentra cerca de la parte superior derecha de la ventana del explorador.](./media/getting-started-build-a-classifier/train02.png)

Después del entrenamiento, se muestra el __rendimiento__. Los indicadores de precisión y recuperación le indican la calidad del clasificador, según pruebas automáticas. Custom Vision Service usa las imágenes que ha enviado para entrenamiento para calcular estas cifras, mediante un proceso denominado [validación cruzada de k iteraciones](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![Los resultados de entrenamiento muestran la precisión y recuperación globales, junto con la precisión y recuperación de cada etiqueta en el clasificador.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Cada vez que selecciona el botón **Train** (Entrenar), se crea una nueva iteración de su clasificador. Puede ver todas las iteraciones antiguas en la pestaña **Performance** (Rendimiento) y puede eliminar cualquiera que podría estar obsoleta. Cuando se elimina una iteración, acaba eliminando las imágenes que están asociadas exclusivamente a ella.

El clasificador usa todas las imágenes para crear un modelo que identifica cada etiqueta. Para probar la calidad del modelo, el clasificador prueba cada imagen en el modelo para ver lo que encuentra.

Se muestran las calidades de los resultados del clasificador.

|Término|Definición|
|---|---|
|__Precisión__|Al clasificar una imagen, ¿qué probabilidad existe de que el clasificador clasifique correctamente la imagen? Aparte de todas las imágenes que se usan para entrenar al clasificador (perros y ponis), ¿qué porcentaje correcto obtuvo el modelo? 99 etiquetas correctas de 100 imágenes ofrecen una precisión del 99 %.|
|__Recuperación__|Aparte de todas las imágenes que se deberían haber clasificado correctamente, ¿cuántas identificó el clasificador correctamente? Una recuperación del 100 % significa que si hay 38 imágenes de perros en las imágenes que se usaron para entrenar al clasificador, el clasificador encontró los 38 perros.|

## <a name="next-steps"></a>Pasos siguientes

[Probar y volver a entrenar el modelo](test-your-model.md)

