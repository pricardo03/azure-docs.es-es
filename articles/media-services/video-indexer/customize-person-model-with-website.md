---
title: 'Uso del sitio web de Video Indexer para personalizar un modelo de persona: Azure'
titlesuffix: Azure Media Services
description: En este artículo se muestra cómo personalizar un modelo de persona con el sitio web de Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 37e556ca458a5b0a171664e089d39cfb448d5f96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65800112"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Personalización de un modelo de persona con el sitio web de Video Indexer

Video Indexer es compatible con el reconocimiento de celebridades en el contenido de vídeo. La característica de reconocimiento de celebridades abarca aproximadamente un millón de caras basadas en orígenes de datos comúnmente solicitados, como IMDB, Wikipedia y personas con más influencia de LinkedIn. Para información general detallada, consulte [Customize a Person model in Video Indexer](customize-person-model-overview.md) (Personalización de un modelo de persona en Video Indexer).

Puede usar el sitio web de Video Indexer para editar las caras que se detectaron en un vídeo, como se describe en este tema. También puede usar la API, como se describe en el artículo de [Personalización de un modelo de persona mediante las API](customize-person-model-with-api.md).

## <a name="central-management-of-person-models-in-your-account"></a>Administración central de modelos de persona en la cuenta

1. Para ver, editar y eliminar los modelos de persona de su cuenta, vaya al sitio web de Video Indexer e inicie sesión.
2. Haga clic en el botón de personalización del modelo de contenido en la esquina superior derecha de la página.

    ![Personalización del modelo de contenido](./media/customize-face-model/content-model-customization.png)
3. Seleccione la pestaña Personas.

    Verá el modelo de persona predeterminado en su cuenta. El modelo de persona predeterminado contiene las caras que haya editado o modificado en la información de los vídeos para las que no haya especificado un modelo de persona personalizado durante la indexación. 

    Si ha creado otros modelos de persona, se mostrarán en esta página también. 

    ![Personalización del modelo de contenido](./media/customize-face-model/content-model-customization-people-tab.png)

## <a name="create-a-new-person-model"></a>Creación de un modelo de persona

1. Haga clic en el botón **+ Add model** (+ Agregar modelo).

    ![Agregar nueva persona](./media/customize-face-model/add-new-person.png)
2. Escriba el nombre del modelo y haga clic en el botón de comprobación junto al nombre.

    ![Agregar nueva persona](./media/customize-face-model/add-new-person2.png)

    Ha creado un modelo de persona. Ahora puede agregar caras al nuevo modelo de persona.
3. Haga clic en el botón de menú de lista y elija **+ Add person** (+ Agregar persona).

    ![Agregar nueva persona](./media/customize-face-model/add-new-person3.png)
    
## <a name="add-a-new-person-to-a-person-model"></a>Incorporación de una persona nueva a un modelo de persona

> [!NOTE]
> Video Indexer permite agregar varias personas con el mismo nombre a un modelo de persona. Sin embargo, se recomienda asignar un nombre único a cada persona del modelo para su uso sea más sencillo y claro.

1. Para agregar una cara nueva a un modelo de persona, haga clic en el botón de menú de la lista junto al modelo de persona al que desea agregar la cara.
1. Haga clic en **+ Add person** (+ Agregar persona) en el menú.

    ![Incorporación de una cara nueva a la persona](./media/customize-face-model/add-new-face.png)
 
    En un elemento emergente se le pedirá que rellene los detalles de la persona. Escriba el nombre de la persona y haga clic en el botón de comprobación.

    ![Incorporación de una cara nueva a la persona](./media/customize-face-model/add-new-face2.png)
 
A continuación, puede usar el Explorador de archivos o arrastrar y colocar las imágenes de la cara. Video Indexer admite todos los tipos de archivo de imagen estándares (p. ej.: JPG, PNG, etc.).

Video Indexer debería poder detectar la presencia de esta persona en los vídeos futuros que indexe y los actuales que ya haya indexado gracias al modelo que persona al que agregó la nueva cara. El reconocimiento de la persona en los vídeos actuales puede tardar algún tiempo en aplicarse, ya que se trata de un proceso por lotes.


## <a name="rename-a-person-model"></a>Cambio de nombre de un modelo de persona

Puede cambiar el nombre de cualquier modelo de persona en su cuenta, incluido el modelo predeterminado. Aunque cambie el nombre del modelo de persona predeterminado, este seguirá sirviendo como predeterminado en su cuenta.

1. Haga clic en el botón de menú de la lista junto al modelo de persona al que desee cambiarle el nombre.
2. Haga clic **Rename** (Cambiar nombre) en el menú.

    ![Cambio de nombre de una persona](./media/customize-face-model/rename-person.png)
3. Haga clic en el nombre del modelo actual y escriba el nuevo nombre.

    ![Cambio de nombre de una persona](./media/customize-face-model/rename-person2.png)
4. Haga clic en el botón de comprobación del modelo cuyo nombre se va a cambiar.

## <a name="delete-a-person-model"></a>Eliminación de un modelo de persona

Puede eliminar cualquier modelo de persona que haya creado en su cuenta. Sin embargo, el modelo de persona predeterminado no se puede eliminar.

1. Haga clic en **Delete** (Eliminar) en el menú.

    ![Eliminación de una persona](./media/customize-face-model/delete-person.png)
    
    Se mostrará un elemento emergente y se le notificará que esta acción eliminará el modelo de persona y todas las personas y los archivos que contenga. Esta operación no se puede deshacer. 

    ![Eliminación de una persona](./media/customize-face-model/delete-person2.png)
1. Si está seguro, haga clic en Delete (Eliminar) de nuevo.

> [!NOTE]
> Los vídeos existentes que se hayan indexado mediante este modelo de persona (eliminado) no serán compatible con la capacidad de actualizar los nombres de las caras que aparezcan en el vídeo. Solo podrá editar los nombres de las caras de estos vídeos después de volver a indexarlos mediante otro modelo de persona. Si vuelve indexar sin especificar un modelo de persona, se usará el predeterminado. 

## <a name="manage-existing-people-in-a-person-model"></a>Administración de personas existentes en un modelo de persona

Para ver el contenido de cualquier modelo de persona, haga clic en la flecha situada junto al nombre de este.
La lista desplegable muestra todas las personas de ese modelo de persona concreto. Si hace clic en el botón de menú de la lista situada junto a cada persona, podrá ver, administrar y eliminar opciones o cambiarles el nombre.  

![Incorporación de una cara nueva a la persona](./media/customize-face-model/manage-people.png)

### <a name="rename-a-person"></a>Cambio de nombre de una persona

1. Para cambiar el nombre de una persona del modelo, haga clic en el botón de menú de la lista y elija **Rename** (Cambiar el nombre).
1. Haga clic en el nombre actual de la persona y escriba el nuevo.
1. Haga clic en el botón de comprobación y el nombre de la persona se cambiará.

### <a name="delete-a-person"></a>Eliminación de una persona

1. Para eliminar el nombre de una persona del modelo, haga clic en el botón de menú de la lista y elija **Delete** (Eliminar).
1. En un elemento emergente se indicará que la acción elimina la persona y no se puede deshacer.
1. Haga clic en Delete (Eliminar) de nuevo para eliminar la persona del modelo de persona.

### <a name="manage-a-person"></a>Administración de una persona

Si hace clic en **Manage** (Administrar), verá todas las caras para las que se está entrenando este modelo de persona. Estas caras proceden de apariciones de esa persona en los vídeos que utilizan este modelo de persona o de imágenes que haya cargado manualmente. 

Puede agregar más caras a la persona haciendo clic en Add images (Agregar imágenes).

Puede usar el panel de administración para cambiar el nombre de la persona y eliminar la persona del modelo.

## <a name="use-a-person-model-to-index-a-video"></a>Uso de un modelo de persona para indexar un vídeo

Puede usar un modelo de persona para indexar el vídeo nuevo mediante la asignación del modelo durante la carga del vídeo.

Para usar el modelo de persona en un nuevo vídeo, haga lo siguiente:

1. Haga clic en el botón **Upload** (Cargar) en la parte superior de la página.

    ![Cargar](./media/customize-face-model/upload.png)
1. Coloque el archivo de vídeo en el círculo o búsquelo.
1. Haga clic en la flecha Advanced options (Opciones avanzadas).

    ![Cargar](./media/customize-face-model/upload2.png)
1. Haga clic en la lista desplegable y seleccione el modelo de persona que ha creado.

    ![Cargar](./media/customize-face-model/upload3.png)
1. Haga clic en la opción Upload (Cargar) en la parte inferior de la página y el nuevo vídeo se indexará por ese modelo de persona.

Si no especifica un modelo de persona durante la carga, Video Indexer indexará el vídeo con el modelo de persona predeterminado en su cuenta.

## <a name="use-a-person-model-to-reindex-a-video"></a>Uso de un modelo de persona para volver a indexar un vídeo 

Para usar el modelo de persona para volver a indexar un vídeo de la colección, vaya a Account videos (Vídeos de cuenta) en la página de inicio de Video Indexer y mantenga el puntero sobre el nombre del vídeo que quiera volver a indexar.

Verá opciones para editar, eliminar y volver a indexar el vídeo. 

1. Haga clic en la opción de volver a indexar el vídeo.

    ![Volver a indexar](./media/customize-face-model/reindex.png)

    Ahora puede seleccionar el modelo de persona con el que volver a indexar el vídeo.
1. Haga clic en la lista desplegable y seleccione el modelo de persona que desee usar. 

    ![Volver a indexar](./media/customize-face-model/reindex2.png)

1. Haga clic en el botón **Reindex** (Volver a indexar) y el vídeo se volverá a indexar con su modelo de persona.

Las nuevas modificaciones que realice en las caras detectadas y que se reconozcan en el vídeo que acaba de reindexar se guardarán en el modelo de persona que usó para el reindexado.

## <a name="managing-people-in-your-videos"></a>Administración de las personas en sus vídeos

Puede administrar las caras que se detectan y las personas que se reconozcan en los vídeos que indexe mediante la edición y la eliminación de caras.

Al eliminar una cara se elimina esa cara concreta de la información del vídeo.

Al editar una cara, el nombre de la cara que se detecta cambia, así como quizá también el reconocimiento en el vídeo. Al editar una cara en el vídeo, el nombre se guarda como una entrada de persona en el modelo de persona asignado al vídeo durante la carga y el indexado.

Si no asigna un modelo de persona al vídeo durante la carga, la edición se guarda en el modelo de persona predeterminado de su cuenta.

### <a name="edit-a-face"></a>Edición de una cara


> [!NOTE]
> Si un modelo de persona tiene dos o más personas diferentes con el mismo nombre, no podrá etiquetar ese nombre en los vídeos que usen ese modelo de persona. Solo podrá realizar cambios en las personas que comparten el nombre en la pestaña People (Personas) de la página de personalización del modelo de contenido de Video Indexer. Por este motivo se recomienda proporcionar nombres únicos a las personas del modelo.

1. Vaya al sitio web de Video Indexer e inicie sesión.
1. Busque un vídeo que quiera ver y edítelo en su cuenta.
1. Para editar una cara del vídeo, vaya a la pestaña Insights (Información) y haga clic en el icono de lápiz en la esquina superior derecha de la ventana.

    ![Edición de una cara en el vídeo](./media/customize-face-model/edit-face.png)
1. Haga clic en cualquiera de las caras detectadas y cambie sus nombres "#X desconocido", o los que asignara previamente a la cara, por otros. 
1. Después de escribir el nuevo nombre, haga clic en el icono de verificación junto a él. Esta acción guardará el nuevo nombre, y todas las apariciones de esta cara en otros vídeos actuales y en los vídeos futuros que cargue se reconocerán y nombrarán. El reconocimiento de la cara en los otros vídeos actuales puede tardar algún tiempo en aplicarse, ya que se trata de un proceso por lotes.

Si a una cara le da el nombre de una persona existente en el modelo que usa el vídeo, la imagen de la cara detectada en el vídeo se combinará con la de la persona con la que ya existe en el modelo. Si se le da un nombre completamente nuevo, se creará una entrada de persona en el modelo que use el vídeo. 

    ![Edit a face in your video](./media/customize-face-model/edit-face2.png)

### <a name="delete-a-face"></a>Eliminación de un archivo

Para eliminar una cara detectada en el vídeo, vaya al panel Insights (Información) y haga clic en el icono de lápiz en la esquina superior derecha del panel. Haga clic en la opción Delete (Eliminar) debajo del nombre de la cara. Esta acción quitará esa cara detectada del vídeo. La cara de la persona se seguirá detectando en los otros vídeos en los que aparezca, pero también puede eliminarla de esos vídeos después de que se hayan indexado. Si se le había dado un nombre, la persona también seguirá existiendo en el modelo que se usara para indexar el vídeo donde eliminó la cara (a menos que elimine específicamente la persona del modelo).

![Eliminación de una cara del vídeo](./media/customize-face-model/delete-face.png)


## <a name="next-steps"></a>Pasos siguientes

[Personalización del modelo de persona mediante las API](customize-person-model-with-api.md)
