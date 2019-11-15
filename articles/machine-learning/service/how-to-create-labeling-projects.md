---
title: Obtención de etiquetas para los datos
titleSuffix: Azure Machine Learning
description: En este artículo se muestra cómo crear y ejecutar proyectos para etiquetar los datos del aprendizaje automático.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 76f995901814c90ff9fd78585c98d56b3478e8b4
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612771"
---
# <a name="get-labels-for-data"></a>Obtención de etiquetas para los datos

El etiquetado de grandes cantidades de datos a menudo ha resultado un dolor de cabeza en los proyectos de aprendizaje automático. Los proyectos de Machine Learning con un componente de Computer Vision (como la clasificación de imágenes o la detección de objetos) normalmente requieren miles de imágenes y sus etiquetas correspondientes. 
 
Azure Machine Learning Studio le proporciona una ubicación central para crear, administrar y supervisar proyectos de etiquetado. Los proyectos de etiquetado ayudan a coordinar los datos, las etiquetas y los miembros del equipo, lo que le permite administrar de forma más eficaz las tareas de etiquetado. Actualmente, las tareas admitidas son la clasificación de imágenes (de varias etiquetas y de varias clases) y la identificación de objetos mediante cuadros de límite.

Azure realiza un seguimiento del progreso y mantiene la cola de tareas de etiquetado incompletas. Los etiquetadores no requieren una cuenta de Azure para participar. Una vez que se haya autenticado con su cuenta de Microsoft (MSA) o [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis), pueden realizar todas las tareas de etiquetado para las que tengan tiempo. Pueden asignar y cambiar etiquetas mediante métodos abreviados de teclado. 

Puede iniciar y detener el proyecto, agregar y quitar usuarios y equipos, e incluso supervisar el progreso. Puede exportar los datos etiquetados en formato COCO o como un conjunto de datos de Azure Machine Learning. 

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un proyecto
> * Especificar los datos y la estructura del proyecto
> * Administrar los equipos y los usuarios que trabajan en el proyecto
> * Ejecutar y supervisar el proyecto
> * Exportar las etiquetas 

## <a name="prerequisites"></a>Requisitos previos

* Los datos que quiera etiquetar, ya sea en archivos locales o en el almacenamiento de Azure.
* El conjunto de etiquetas que quiere aplicar.
* Instrucciones para etiquetar
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* Un área de trabajo de Azure Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Crear un proyecto de etiquetado

Los proyectos de etiquetado se administran desde [Azure Machine Learning Studio](https://ml.azure.com/). La página **Proyectos de etiquetado** permite administrar proyectos, equipos y personas. Un proyecto tiene uno o varios equipos asignados y un equipo tiene una o más personas asignadas a él. 

Si los datos ya están almacenados en el almacenamiento de blobs de Azure, debe hacer que estén disponibles como un almacén de datos antes de crear el proyecto de etiquetado. Para obtener más información, consulte [Creación y registro de almacenes de datos](https://docs.microsoft.com/azure/machine-learning/service/how-to-access-data#create-and-register-datastores). 

Para crear un proyecto, seleccione **Agregar proyecto**. Asígnele un nombre adecuado y seleccione **Labeling task type** (Tipo de tarea de etiquetado). 

![Asistente para la creación de proyectos de etiquetado](media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Seleccione **Image Classification Multi-label** (clasificación de imágenes con varias etiquetas) para los proyectos en los que se aplicarán **una _o más_** etiquetas de un conjunto de clases a una imagen. Por ejemplo, una fotografía de un perro podría etiquetarse con *perro* y *diurno*
* Seleccione **Image Classification Multi-class** (clasificación de imágenes con varias clases) para los que solo se aplicará **una clase única** de un conjunto de clases a una imagen.
* Elija **Object Identification (Bounding Box)** (identificación de objetos [cuadro de límite]) para los proyectos en los que la tarea consiste en asignar una clase a un objeto de una imagen y especificar un cuadro de límite alrededor del objeto.

Elija **Siguiente** cuando esté listo para continuar.

## <a name="specify-data-to-be-labeled"></a>Especificación de los datos que se van a etiquetar

Si ya ha creado un conjunto de datos que contiene sus datos, puede seleccionarlo en la lista desplegable **Seleccionar un conjunto de datos existente**. O bien puede elegir **Crear un conjunto de datos** para seleccionar un almacén de datos de Azure existente o cargar archivos locales. 

### <a name="create-a-dataset-from-an-azure-datastore"></a>Creación de un conjunto de datos a partir de un almacén de datos de Azure

Aunque elegir la carga directa de archivos locales es suficiente para muchos casos de uso, el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) es más sólido y rápido para transferir grandes cantidades de datos. Se recomienda el Explorador de Azure Storage como la manera predeterminada de migrar archivos.

Para crear un conjunto de datos a partir de los datos que ya ha almacenado en el almacenamiento de blobs de Azure:

1. Elija **Crear un conjunto de datos** y **Desde el almacén datos**.
1. Asigne un **nombre** al conjunto de datos.
1. Debe elegir "Archivo" como **Tipo de conjunto de datos**.  
1. Seleccione el almacén de datos. 
1. Si los datos están en una subcarpeta dentro del almacenamiento de blobs, elija **Examinar** para seleccionar la ruta de acceso. 
    * Además, puede anexar `/**` después de la ruta de acceso para incluir todos los archivos en las subcarpetas de la ruta de acceso seleccionada.
    * Use `**/*.*` para incluir todos los datos en el contenedor y las subcarpetas actuales.
1. Proporcione una descripción para el conjunto de datos.
1. Elija **Siguiente**. 
1. Confirme los detalles. Puede elegir **Atrás** para modificar la configuración o **Crear** para crear el conjunto de datos.

### <a name="create-a-dataset-by-uploading-data"></a>Creación de un conjunto de datos mediante la carga de datos

Si quiere cargar los datos directamente:

1. Elija **Crear un conjunto de datos** y **Desde archivos locales**.
1. Asigne un **nombre** al conjunto de datos.
1. Debe elegir "Archivo" como **Tipo de conjunto de datos**.
1. Si elige **Configuración avanzada**, puede personalizar el almacén de datos, el contenedor y la ruta de acceso a los datos.
1. Elija **Examinar** para seleccionar archivos locales y cargarlos.
1. Proporcione una descripción para el conjunto de datos.
1. Elija **Siguiente**. 
1. Confirme los detalles. Puede elegir **Atrás** para modificar la configuración o **Crear** para crear el conjunto de datos.

Los datos se cargan en el almacén de blobs predeterminado (`workspaceblobstore`) del área de trabajo de Azure Machine Learning.

## <a name="specify-label-classes"></a>Especificación de clases de etiquetas

En la página **Clases de etiquetas**, especifique el conjunto de clases que se utilizará para clasificar los datos. Piense detenidamente estas clases, ya que la velocidad y la precisión de su etiquetador se verán afectadas por la facilidad con la que puedan elegir entre ellas correctamente. Por ejemplo, en lugar de deletrear el género y la especie completos para plantas o animales, podría ser mejor usar códigos de campo o abreviar el género. 

Escriba una etiqueta por fila, con el botón **+** para agregar una nueva fila. Si tiene más de tres o cuatro etiquetas, pero menos de diez, considere la posibilidad de prefijarlas con "1:", "2:", y así sucesivamente para guiar a los etiquetadores que usan las teclas numéricas a acelerar su trabajo. 

## <a name="describe-the-labeling-task"></a>Descripción de la tarea de etiquetado

Es importante explicar claramente la tarea de etiquetado. La página de **Instrucciones de etiquetado** le permite vincularse a un sitio externo para obtener las instrucciones que se mostrarán a los etiquetadores. Mantenga las instrucciones orientadas a tareas y adecuadas para el público. 

* ¿Cuáles son las etiquetas que verán y cómo podrán elegir entre ellas? ¿Hay un texto de referencia que deben consultar?
* ¿Qué debe hacer si ninguna etiqueta parece adecuada? 
* ¿Qué debe hacer si varias etiquetas parecen adecuadas?
* ¿Qué umbral de confianza debe aplicarse a una etiqueta? ¿Quiere que use una conjetura si no está seguro?
* ¿Qué debe hacer con los objetos de interés ocluidos o superpuestos?
* ¿Qué debe hacer si un objeto de interés se corta con el borde de la imagen?
* ¿Qué debe hacer si cree que ha cometido un error con una imagen después de enviarla? 

Con los cuadros de límite, otras preguntas importantes incluyen:

* ¿Cómo se define el cuadro de límite para esta tarea? ¿Debe estar completamente dentro del objeto, se debe recortar lo más cerca posible de su extensión o es aceptable cierta cantidad de espacio? 
* ¿Qué nivel de atención y coherencia se espera que el etiquetador aplique al definir los cuadros de límite?

>[!Note]
> Asegúrese de indicar que el etiquetador podrá elegir entre las nueve primeras etiquetas con las teclas numéricas del 1 al 9. 

## <a name="initialize-the-labeling-project"></a>Inicialización del proyecto de etiquetado

Una vez inicializado, algunos aspectos del proyecto de etiquetado no pueden cambiarse. Por ejemplo, el tipo de tarea o el conjunto de elementos. Puede modificar las etiquetas y cambiar la dirección URL de la descripción de la tarea. Revise atentamente la configuración antes de crear el proyecto. Una vez que haya enviado el proyecto, volverá a la página principal de **Etiquetado**, que mostrará el proyecto en el estado **Inicializando**. Esta página no se actualiza automáticamente, por lo que, después de un período de tiempo, actualizarla de forma manual mostrará el proyecto como **Creado**. 

## <a name="manage-teams-and-people"></a>Administración de equipos y personas

Un proyecto de etiquetado obtiene un equipo predeterminado y lo agrega como miembro predeterminado. Cada proyecto de etiquetado obtiene un nuevo equipo predeterminado, pero los equipos se pueden compartir entre proyectos. Los proyectos pueden tener más de un equipo. Para crear un equipo, elija **Agregar equipo** en la página **Equipos**. 

Los usuarios se administran en la página **Contactos**. Puede agregar y quitar personas identificadas por su dirección de correo electrónico. Cada etiquetador tendrá que autenticarse con su cuenta de Microsoft o Azure Active Directory si tiene una.  

Una vez que haya agregado una persona, puede asignarla a uno o varios equipos. Vaya a la página **Equipos**, seleccione el equipo específico en el que esté interesado y después use **Asignar personas** o **Quitar personas** según corresponda.

Si alguna vez quiere enviar un correo electrónico a todos los usuarios del equipo, puede hacerlo. Para ello, elija el equipo para abrir la página **Detalles del equipo**. En esta página, el botón **Enviar correo electrónico al equipo** abrirá el editor de correo electrónico con las direcciones de todos los miembros del equipo.

## <a name="run-and-monitor-the-project"></a>Ejecutar y supervisar el proyecto

Una vez inicializado el proyecto, Azure comenzará a ejecutarlo. Si en la página de **Etiquetado** principal hace clic en el proyecto, se le dirigirá a los **Detalles del proyecto**. En la pestaña **Panel** se mostrará el progreso de la tarea de etiquetado. 

En la pestaña **Datos**, puede consultar el conjunto de datos y revisar los datos etiquetados. Si ve datos etiquetados incorrectamente, puede **Seleccionar** dichos datos y elegir **Rechazar**, lo que quitará las etiquetas y devolverá los datos a la cola sin etiquetar. 

En la pestaña **Equipo**, puede asignar o cancelar la asignación de equipos a este proyecto. 

Si quiere poner el proyecto en modo sin conexión o en línea, elija el botón **Pausar**/**Iniciar**, que alterna el estado de ejecución del proyecto.

Puede etiquetar los datos directamente desde la página **Detalles del proyecto** si selecciona **Label data** (Etiquetar datos). Solo puede etiquetar los datos cuando el proyecto se está ejecutando. 

## <a name="export-the-labels"></a>Exportar las etiquetas

En cualquier momento, puede exportar los datos de etiquetas para realizar experimentos de aprendizaje automático. Las etiquetas de imagen se pueden exportar en [formato COCO](http://cocodataset.org/#format-data) o como un conjunto de datos de Azure Machine Learning. Encontrará el botón **Exportar** en la página **Detalles del proyecto** del proyecto de etiquetado.

El archivo COCO se crea en el almacén de blobs predeterminado del área de trabajo de Azure Machine Learning en una carpeta dentro de **export/coco**. Puede acceder al conjunto de datos exportado de Azure Machine Learning en la sección **Conjuntos de datos** de Studio. La página de detalles del conjunto de datos también proporciona código de ejemplo para acceder a las etiquetas desde Python.

![Conjunto de datos exportado](media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Pasos siguientes

* Etiquetado de imágenes para la [clasificación de imágenes o la detección de objetos](how-to-label-images.md)
* Más información sobre [Azure Machine Learning y Azure Machine Learning Studio](../compare-azure-ml-to-studio-classic.md)