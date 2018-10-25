---
title: Introducción a Elementos acústicos de un proyecto
titlesuffix: Azure Cognitive Services
description: Esta guía de inicio rápido le mostrará cómo integrar el complemento en el proyecto de Unity, elaborar la escena y aplicar la acústica a los orígenes de sonido.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.component: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 62b6b40e6289ccca92c0a92d8bb7d0489dd44246
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902950"
---
# <a name="getting-started-with-project-acoustics"></a>Introducción a Elementos acústicos de un proyecto
Esta guía de inicio rápido le mostrará cómo integrar el complemento en el proyecto de Unity, elaborar la escena y aplicar la acústica a los orígenes de sonido. Para este inicio rápido, primero deberá crear una [cuenta de Azure Batch](create-azure-account.md). Esta guía supone que está algo familiarizado con Unity.

## <a name="download-the-plugin"></a>Descarga del complemento
Regístrese [aquí](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) para unirse a la versión preliminar del diseñador.

## <a name="supported-platforms-for-quickstart"></a>Plataformas compatibles para el inicio rápido
* [Unity 2018.2+](http://www.unity3d.com)
  * Requiere establecer el proyecto en la versión en tiempo de ejecución de scripting **equivalente a .NET 4.x** 
  * Requiere el editor de Unity basado en Windows

## <a name="import-the-plugin"></a>Importación del complemento
Importe el paquete UnityPackage de acústica al proyecto. 
* En Unity, vaya a **Assets > Import Package > Custom Package...** (Recursos > Importar paquete > Paquete personalizado)

![Importación de paquete](media/ImportPackage.png)  

* Elija **MicrosoftAcoustics.unitypackage**

Si va a importar el complemento a un proyecto existente, es posible que el proyecto ya tenga un archivo **mcs.rsp** en la raíz del proyecto, que especifica las opciones del compilador de C#. Deberá combinar el contenido de ese archivo con el archivo mcs.rsp que se incluye con el complemento de Elementos acústicos de un proyecto.

## <a name="enable-the-plugin"></a>Habilitación del complemento
La parte de elaboración del kit de herramientas de Elementos acústicos requiere la versión en tiempo de ejecución de scripting .NET 4.x. La importación del paquete actualizará la configuración del reproductor de Unity. Reinicie Unity para que esta configuración surta efecto.

![Configuración del Reproductor](media/PlayerSettings.png)

![.NET 4.5](media/Net45.png)

## <a name="create-a-navigation-mesh"></a>Creación de una malla de navegación
Usar el [flujo de trabajo de Unity](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) estándar para crear una malla de navegación para el proyecto. Para información sobre cómo usar sus propias mallas de navegación, consulte el [tutorial de la interfaz de elaboración](bake-ui-walkthrough.md).

## <a name="mark-static-meshes-for-acoustics"></a>Marca de las mallas estáticas para la acústica
Use **Window > Acoustics** (Ventana > Acústica) de Unity para mostrar la ventana de acústica. Esta ventana se puede acoplar junto al Inspector.

![Apertura de la ventana Acoustics (Acústica)](media/WindowAcoustics.png)

En la ventana de jerarquía de Unity, anule la selección de los elementos seleccionados. En la pestaña **Object** (Objeto) de la acústica, active la casilla "Acoustics Geometry" (Geometría de la acústica) para marcar todas las mallas y los terrenos de la escena como geometría de la acústica.

En la pestaña **Materials** (Materiales), asigne los materiales acústicos a los materiales que se usan en la escena. El material **predeterminado** tiene una absorción equivalente al concreto. Para más información sobre cómo especificar sus propias propiedades de materiales, consulte la [página del proceso de diseño](design-process.md).

![Pestaña Materials (Materiales)](media/MaterialsTab.png)

## <a name="preview-the-probes"></a>Vista previa de los sondeos
En la pestaña **Probes** (Sondeos), haga clic en **Calculate** (Calcular). Este cálculo puede tardar unos minutos, en función del tamaño de la escena. Cuando se complete el cálculo, podrá ver esferas flotante en la vista de la escena, lo que marca las ubicaciones para la simulación acústica, que se denominan "puntos de sondeo". Si se acerca lo suficiente a un objeto en la ventana de la escena, también podrá ver la voxelización de la escena. Los vóxeles verdes se deben alinear con los objetos que marcó como geometría. Las visualizaciones de vóxeles y puntos de sondeo se pueden alternar en el menú Gizmos que está en la parte superior derecha de la vista de la escena.

![GizmosPreview](media/BakePreviewWithGizmos.png)

## <a name="bake-the-scene"></a>Elaboración de la escena
En la pestaña **Bake** (Elaborar), escriba sus credenciales de Azure y haga clic en **Bake** (Elaborar). Si no tiene una cuenta de Azure Batch, consulte [este tutorial para ver la configuración de cuenta recomendada](create-azure-account.md).
Cuando finalice la elaboración, el archivo de datos se descargará automáticamente en el directorio **Assets/AcousticsData** del proyecto.

## <a name="set-up-audio-runtime-dsp"></a>Configuración de DSP de entorno de ejecución
Insertamos DSP de entorno de ejecución de audio para la acústica en el marco del espacializador de Unity y lo integramos con la espacialización basada en HRTF. Para habilitar el procesamiento acústico, cámbiese al espacializador **Microsoft Acoustics** en **Edit > Project Settings > Audio** (Editar > Configuración del proyecto > Audio) y seleccione **Microsoft Acoustics** como el **complemento de espacializador** del proyecto. Además, asegúrese de que el **tamaño de búfer de DSP** está establecido en el mejor rendimiento.

![Configuración del proyecto](media/ProjectSettings.png)  

![Espacializador de Elementos acústicos de un proyecto](media/ChooseSpatializer.png)

Abra el mezclador de audio (**Window > Audio Mixer** [Ventana > Mezclador de audio]). Asegúrese de tener al menos un mezclador, con un grupo. Si no es así, haga clic en el botón "+" que está a la derecha de **Mixers** (Mezcladores). Haga clic con el botón secundario en la parte inferior de la fila de canales en la sección de efectos y agregue el efecto **Microsoft Acoustics Mixer** (Mezclador de Microsoft Acoustics). Tenga en cuenta que solo se admite un mezclador de Elementos acústicos de un proyecto a la vez.

![Mezclador de audio](media/AudioMixer.png)

## <a name="set-up-the-acoustics-lookup-table"></a>Configuración de la tabla de búsqueda acústica
Arrastre y coloque el objeto prefabricado **Microsoft Acoustics** desde el panel del proyecto en la escena:

![Objeto prefabricado de acústica](media/AcousticsPrefab.png)

Haga clic en el objeto de juego **ProjectAcoustics** y vaya al panel del inspector. Especifique la ubicación del resultado de la elaboración (el archivo .ACE, en **Assets/AcousticsData**) mediante la función de arrastrarlo y colocarlo en el script del administrador de acústica o con un clic en el botón de círculo situado junto al cuadro de texto.

![Administrador de acústica](media/AcousticsManager.png)  

## <a name="apply-acoustics-to-sound-sources"></a>Aplicación de la acústica a los orígenes de sonidos
Cree un origen de sonido. Active la casilla de verificación en la parte inferior del panel de inspector de AudioSource que dice **Spatialize** (Espacializar). Asegúrese de que **Spatial Blend** (Combinación espacial) se establece en 3D completo.  

![Origen de audio](media/AudioSource.png)

## <a name="apply-post-bake-design"></a>Aplicación del diseño posterior a la elaboración
Puede adjuntar el script **AcousticsAdjust** a un origen de sonido de la escena para habilitar parámetros de diseño de origen adicionales si hace clic en **Add Component** (Agregar componente) y selecciona **Scripts > Acoustics Adjust** (Scripts > Ajuste de acústica):

![AcousticsAdjust](media/AcousticsAdjust.png)

También hay parámetros en **Microsoft Acoustics Mixer** (Mezclador de Microsoft Acoustics). Para más información sobre el diseño posterior a la elaboración, consulte los [parámetros de diseño](design-process.md).

## <a name="next-steps"></a>Pasos siguientes
* Pruebe la [escena de ejemplo](sample-walkthrough.md)
* Obtenga información sobre todo el conjunto de [características de elaboración](bake-ui-walkthrough.md)
* Explore [parámetros de diseño](design-process.md) más detallados

