---
title: 'Uso del sitio web de Video Indexer para personalizar un modelo de lenguaje: Azure'
titlesuffix: Azure Media Services
description: En este artículo se muestra cómo personalizar un modelo de lenguaje con el sitio web de Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 9bf48a994c3efeb433bcb99342f7a477d87858eb
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53283591"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Personalización de un modelo de lenguaje con el sitio web de Video Indexer

Video Indexer permite crear modelos de lenguaje personalizados para personalizar el reconocimiento de voz mediante la carga de texto de adaptación, es decir, texto del dominio a cuyo vocabulario desea que se adapte el motor. Una vez que se entrena el modelo, se reconocerán las nuevas palabras que aparecen en el texto de adaptación. 

Para información general detallada y conocer los procedimientos recomendados para modelos de lenguaje personalizados, consulte [Customize a Language model with Video Indexer](customize-language-model-overview.md) (Personalización de un modelo de lenguaje con Video Indexer).

Puede usar el sitio web de Video Indexer para crear y editar modelos de lenguaje personalizados en su cuenta, como se describe en este tema. También puede usar la API, como se describe en [Personalización del modelo de lenguaje mediante las API](customize-language-model-with-api.md).

## <a name="create-a-language-model"></a>Creación de un modelo de lenguaje

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
2. Para personalizar un modelo en su cuenta, haga clic en el botón **Content model customization** (Personalización del modelo de contenido) en la esquina superior derecha de la página.

   ![Personalización del modelo de contenido](./media/content-model-customization/content-model-customization.png)

3. Seleccione la pestaña **Language** (Lenguaje).

    Verá una lista de idiomas admitidos. 

    ![Personalización del modelo de lenguaje](./media/customize-language-model/customize-language-model.png)

4. En el idioma que desee, haga clic en **Add model** (Agregar modelo).
5. Escriba el nombre para el modelo de lenguaje y presione Entrar.

    Se crea el modelo y se ofrece la opción de cargar en él archivos de texto.
6. Para agregar un archivo de texto, haga clic en **Add file** (Agregar archivo). Se abre el explorador de archivos.

7. Vaya al archivo de texto y selecciónelo. Puede agregar varios archivos de texto a un modelo de lenguaje.

    También, para agregar un archivo de texto, haga clic en el botón **...**  situado en el lado derecho del modelo de lenguaje y seleccione **Add file** (Agregar archivo).
8. Cuando termine de cargar los archivos de texto, haga clic en la opción verde **Train** (Entrenar).

    ![Entrenamiento del modelo de lenguaje](./media/customize-language-model/train-model.png)

Este proceso puede tardar varios minutos. Finalizado el entrenamiento, verá **Trained** (Entrenado) junto al modelo. Puede obtener una vista previa del archivo del modelo, descargarlo y eliminarlo.

![Modelo de lenguaje entrenado](./media/customize-language-model/preview-model.png)

### <a name="using-a-language-model-on-a-new-video"></a>Uso de un modelo de lenguaje en un nuevo vídeo

Para usar el modelo de lenguaje en un nuevo vídeo, realice una de las siguientes acciones:

* Haga clic en el botón **Upload** (Cargar) en la parte superior de la página. 

    ![Cargar](./media/customize-language-model/upload.png)
* Coloque el archivo de audio o vídeo en el círculo o busque el archivo.

    ![Cargar](./media/customize-language-model/upload2.png)

Esto le dará la opción de seleccionar el **idioma de origen de vídeo**. Haga clic en la lista desplegable y seleccione un modelo de lenguaje que haya creado de la lista. Debería aparecer el lenguaje del modelo de lenguaje y el nombre que le dio entre paréntesis.

Haga clic en la opción **Upload** (Cargar) en la parte inferior de la página y el nuevo vídeo se indexará mediante el modelo de lenguaje.

### <a name="using-a-language-model-to-reindex"></a>Uso de un modelo de lenguaje para volver a indexar

Para usar el modelo de lenguaje para volver a indexar un vídeo de la colección, vaya a **Account videos** (Vídeos de cuenta) en la página de inicio de [Video Indexer](https://www.videoindexer.ai/) y mantenga el puntero sobre el nombre del vídeo que quiera volver a indexar.

Verá opciones para editar, eliminar y volver a indexar el vídeo. Haga clic en la opción para volver a indexar el vídeo.

![Volver a indexar](./media/customize-language-model/reindex1.png)

Tiene la opción de seleccionar el **idioma de origen del vídeo** para volver a indexar el vídeo. Haga clic en la lista desplegable y seleccione un modelo de lenguaje que haya creado de la lista. Debería aparecer el lenguaje del modelo de lenguaje y el nombre que le dio entre paréntesis.

![Volver a indexar](./media/customize-language-model/reindex.png)

Haga clic en el botón **Re-index** (Volver a indexar) y el vídeo se volverá a indexar con su modelo de lenguaje.

## <a name="edit-a-language-model"></a>Edición de un modelo de lenguaje

Para editar un modelo de lenguaje, puede cambiar su nombre, agregarle archivos y eliminar archivos de él.

Si agrega o elimina archivos en el modelo de lenguaje, deberá volver a entrenar el modelo; para ello, haga clic en la opción verde **Train** (Entrenar).

### <a name="rename-the-language-model"></a>Cambio de nombre del modelo de lenguaje

Para cambiar el nombre del modelo de lenguaje, haga clic en **...**  en el lado derecho del modelo de lenguaje y seleccione **Rename** (Cambiar nombre). 

Escriba el nuevo nombre y presione Entrar.

### <a name="add-files"></a>Incorporación de archivos

Para agregar un archivo de texto, haga clic en **Add file** (Agregar archivo). Se abre el explorador de archivos.

Vaya al archivo de texto y selecciónelo. Puede agregar varios archivos de texto a un modelo de lenguaje.

También, para agregar un archivo de texto, haga clic en el botón **...**  situado en el lado derecho del modelo de lenguaje y seleccione **Add file** (Agregar archivo).

### <a name="delete-files"></a>Eliminar archivos

Para eliminar un archivo del modelo de lenguaje, haga clic en el botón **...**  situado en el lado derecho del archivo de texto y seleccione **Delete** (Eliminar). Se abrirá una nueva ventana que le indica que la eliminación no se puede deshacer. Haga clic en la opción **Delete** (Eliminar) en la nueva ventana.

Esta acción quita completamente el archivo del modelo de lenguaje.

## <a name="delete-a-language-model"></a>Eliminación de un modelo de lenguaje

Para eliminar un modelo de lenguaje de su cuenta, haga clic en el botón **...** situado en el lado derecho del modelo de lenguaje y seleccione **Delete** (Eliminar).

Se abrirá una nueva ventana que le indica que la eliminación no se puede deshacer. Haga clic en la opción **Delete** (Eliminar) en la nueva ventana.

Esta acción quita completamente el modelo de lenguaje de su cuenta. Cualquier vídeo que estuviera usando el modelo de lenguaje eliminado mantendrá el mismo índice hasta que vuelva a indexar el vídeo. Si vuelve a indexar el vídeo, puede asignarle un nuevo modelo de lenguaje. En caso contrario, Video Indexer usará su modelo predeterminado para volver a indexar el vídeo. 

## <a name="next-steps"></a>Pasos siguientes

[Personalización del modelo de lenguaje mediante las API](customize-language-model-with-api.md)
