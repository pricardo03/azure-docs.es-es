---
title: Creación de un proyecto de etiquetado de datos
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo crear y ejecutar proyectos para etiquetar los datos del aprendizaje automático.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: e469837c8e374e62824bd8f7a7feb110ed1be9c9
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153118"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Creación de un proyecto de etiquetado de datos y exportación de etiquetas 

Etiquetar datos voluminosos en proyectos de Machine Learning suele ser una tarea compleja. Los proyectos que tienen un componente de Computer Vision (como la clasificación de imágenes o la detección de objetos) normalmente requieren etiquetar miles de imágenes.
 
[Azure Machine Learning](https://ml.azure.com/) proporciona una ubicación central para crear, administrar y supervisar proyectos de etiquetado. Úselo para coordinar los datos, las etiquetas y los miembros del equipo para administrar de forma eficaz las tareas de etiquetado. Machine Learning permite la clasificación de imágenes (de varias etiquetas y varias clases) y la identificación de objetos mediante rectángulos de selección.

Machine Learning realiza un seguimiento del progreso y el mantenimiento de la cola de tareas de etiquetado incompletas. Los etiquetadores no necesitan una cuenta de Azure para participar. Una vez que se hayan autenticado con su cuenta Microsoft (MSA) o [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis), pueden realizar todo el etiquetado que el tiempo les permita.

En Machine Learning, puede iniciar y detener el proyecto, agregar y quitar usuarios y equipos, e incluso supervisar el progreso. Puede exportar los datos etiquetados en formato COCO o como un conjunto de datos de Azure Machine Learning.

> [!Important]
> Actualmente solo se admiten proyectos de etiquetado de clasificación de imágenes e identificación de objetos. Además, las imágenes de datos deben estar disponibles en un almacén de datos de blobs de Azure. (Si no tiene un almacén de datos existente, puede cargar las imágenes durante la creación del proyecto). 

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un proyecto
> * Especificar los datos y la estructura del proyecto
> * Administrar los equipos y usuarios que trabajan en el proyecto
> * Ejecutar y supervisar el proyecto
> * Exportar las etiquetas


## <a name="prerequisites"></a>Prerrequisitos

* Los datos que quiere etiquetar, ya sea en archivos locales o en Azure Storage.
* Conjunto de etiquetas que quiere aplicar.
* Instrucciones para el etiquetado.
* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree) antes de empezar.
* Un área de trabajo de Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Crear un proyecto de etiquetado

Los proyectos de etiquetado se administran desde Azure Machine Learning. Use la página **Proyectos de etiquetado** para administrar sus proyectos y personas. Un proyecto tiene uno o varios equipos asignados y un equipo tiene una o más personas asignadas a él.

Si los datos ya están en el almacenamiento de blobs de Azure, debe hacer que estén disponibles como un almacén de datos antes de crear el proyecto de etiquetado. Para más información, consulte [Creación y registro de almacenes de datos](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores).

Para crear un proyecto, seleccione **Agregar proyecto**. Asigne un nombre adecuado al proyecto y seleccione **Tipo de tarea de etiquetado**.

![Asistente para la creación de proyectos de etiquetado](./media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Elija un proyecto de tipo **Clasificación de imágenes con varias etiquetas** cuando quiera aplicar *una o varias* etiquetas de un conjunto de clases a una imagen. Por ejemplo, una fotografía de un perro podría etiquetarse como *perro* y *diurno*.
* Elija un proyecto de tipo **Clasificación de imágenes con varias clases** cuando quiera aplicar una *sola clase* de un conjunto de clases a una imagen.
* Elija un proyecto de tipo **Identificación del objeto (rectángulo de selección)** cuando quiera asignar una clase y un rectángulo de selección a cada objeto de una imagen.

Seleccione **Siguiente** cuando esté listo para continuar.

## <a name="specify-the-data-to-label"></a>Especificación de los datos que se van a etiquetar

Si ya ha creado un conjunto de datos que contiene los datos, selecciónelo en la lista desplegable **Seleccione un conjunto de datos existente**. O bien, seleccione **Crear un conjunto de datos** para usar un almacén de información de Azure existente o cargar archivos locales.

### <a name="create-a-dataset-from-an-azure-datastore"></a>Creación de un conjunto de datos a partir de un almacén de datos de Azure

En muchos casos, basta con cargar archivos locales. Sin embargo, [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) es una forma más rápida y eficaz de transferir una gran cantidad de datos. Se recomienda usar Explorador de Storage de forma predeterminada para migrar archivos.

Para crear un conjunto de datos a partir de los datos que ya ha almacenado en el almacenamiento de blobs de Azure:

1. Seleccione **Crear un conjunto de datos** > **De almacén de datos**.
1. En **Nombre**, asigne un nombre al conjunto de datos.
1. Elija **Archivo** como **Tipo de conjunto de datos**.  
1. Seleccione el almacén de datos.
1. Si los datos están en una subcarpeta del almacenamiento de blobs, elija **Examinar** para seleccionar la ruta de acceso.
    * Anexe "/**" a la ruta de acceso para incluir todos los archivos que haya en las subcarpetas de la ruta de acceso seleccionada.
    * Anexe "* */* .*" para incluir todos los datos que haya en el contenedor actual y sus subcarpetas.
1. Proporcione una descripción para el conjunto de datos.
1. Seleccione **Next** (Siguiente).
1. Confirme los detalles. Seleccione **Atrás** para modificar la configuración o **Crear** para crear el conjunto de datos.

### <a name="create-a-dataset-from-uploaded-data"></a>Creación de un conjunto de datos a partir de los datos cargados

Para cargar los datos directamente:

1. Seleccione **Crear un conjunto de datos** > **De archivos locales**.
1. En **Nombre**, asigne un nombre al conjunto de datos.
1. Elija "Archivo" como **Tipo de conjunto de datos**.
1. *Opcional:* Seleccione **Configuración avanzada** para personalizar el almacén de datos, el contenedor y la ruta de acceso a los datos.
1. Seleccione **Examinar** para seleccionar los archivos locales que se van a cargar.
1. Proporcione una descripción para el conjunto de datos.
1. Seleccione **Next** (Siguiente).
1. Confirme los detalles. Seleccione **Atrás** para modificar la configuración o **Crear** para crear el conjunto de datos.

Los datos se cargan en el almacén de blobs predeterminado ("workspaceblobstore") del área de trabajo de Machine Learning.

## <a name="specify-label-classes"></a>Especificación de clases de etiquetas

En la página **Clases de etiquetas**, especifique el conjunto de clases para clasificar los datos. Hágalo con cuidado, porque la precisión y la velocidad de los etiquetadores depende de su capacidad para elegir entre las clases. Por ejemplo, en lugar de deletrear el género y la especie completos para plantas o animales, use códigos de campo o abrevie el género.

Escriba una etiqueta por fila. Use el botón **+** para agregar una nueva fila. Si tiene más de 3 o 4 etiquetas pero menos de 10, quizás quiera añadir a los nombres un prefijo de número ("1:", "2:") para que los etiquetadores puedan usar las teclas numéricas para acelerar su trabajo.

## <a name="describe-the-labeling-task"></a>Descripción de la tarea de etiquetado

Es importante explicar claramente la tarea de etiquetado. En la página **Instrucciones de etiquetado**, puede agregar un vínculo a un sitio externo que contenga las instrucciones de etiquetado. Mantenga las instrucciones orientadas a tareas y adecuadas para el público. Tenga en cuenta estas preguntas:

* ¿Cuáles son las etiquetas que verán y cómo las elegirán? ¿Hay un texto de referencia que puedan consultar?
* ¿Qué debe hacer si ninguna etiqueta parece adecuada?
* ¿Qué debe hacer si varias etiquetas parecen adecuadas?
* ¿Qué umbral de confianza debe aplicarse a una etiqueta? ¿Quiere que use una conjetura si no está seguro?
* ¿Qué debe hacer con los objetos de interés ocluidos o superpuestos?
* ¿Qué debe hacer si un objeto de interés se corta con el borde de la imagen?
* ¿Qué deben hacer después de enviar una etiqueta si creen que han cometido un error?

En el caso de los rectángulos de selección, estas son algunas preguntas importantes:

* ¿Cómo se define el cuadro de límite para esta tarea? ¿Debe estar totalmente en el interior del objeto o en el exterior? ¿Debe recortarse lo más cerca posible o hay algo de margen?
* ¿Qué nivel de cuidado y coherencia espera que los etiquetadores apliquen para definir los rectángulos de selección?

>[!NOTE]
> Recuerde que los etiquetadores podrán seleccionar las 9 primeras etiquetas usando las claves numéricas de 1 a 9.

## <a name="initialize-the-labeling-project"></a>Inicialización del proyecto de etiquetado

Una vez inicializado el proyecto de etiquetado, algunos aspectos del proyecto son inmutables. No se puede cambiar el tipo de tarea ni el conjunto de datos. Se *pueden* modificar las etiquetas y la dirección URL de la descripción de la tarea. Repase atentamente la configuración antes de crear el proyecto. Después de enviar el proyecto, volverá a la página principal **Data Labelling** (Etiquetado de datos), que muestra el proyecto como **Initializing** (Inicializando). Esta página no se actualiza automáticamente. Espere unos momentos y actualice la página manualmente para ver el estado del proyecto como **Creado**.

## <a name="manage-teams-and-people"></a>Administración de equipos y personas

De forma predeterminada, para cada proyecto de etiquetado se crea un nuevo equipo con usted como miembro. Sin embargo, los equipos pueden compartirse entre los proyectos. Y los proyectos pueden tener más de un equipo. Para crear un equipo, seleccione **Agregar equipo** en la página **Equipos**.

Los usuarios se administran en la página **Personas**. Agregue y quite las personas por dirección de correo electrónico. Cada etiquetador tiene que autenticarse con su cuenta Microsoft o con Azure Active Directory, si lo usa.  

Después de agregar a una persona, puede asignarla a uno o varios equipos: Vaya a la página **Equipos**, seleccione el equipo y, a continuación, seleccione **Asignar personas** o **Quitar personas**.

Para enviar un correo electrónico al equipo, seleccione el equipo para ver la página **Detalles del equipo**. En esta página, seleccione **Correo electrónico del equipo** para abrir un borrador de correo electrónico con las direcciones de todos los miembros del equipo.

## <a name="run-and-monitor-the-project"></a>Ejecutar y supervisar el proyecto

Después de inicializar el proyecto, Azure comenzará a ejecutarlo. Seleccione el proyecto en la página **Data Labeling** (Etiquetado de datos) para ir a **Project details** (Detalles del proyecto). En la pestaña **Panel** se muestra el progreso de la tarea de etiquetado.

En la pestaña **Datos**, puede ver el conjunto de datos y revisar los datos etiquetados. Si ve datos etiquetados incorrectamente, selecciónelos y elija **Rechazar**; se quitarán las etiquetas y los datos se volverán a colocar en la cola sin etiquetar.

Use la pestaña **Equipo** para asignar o desasignar equipos del proyecto.

Para poner en pausa o reiniciar el proyecto, seleccione el botón **Pausar**/**Iniciar**. Solo se pueden etiquetar datos cuando el proyecto se está ejecutando.

Puede etiquetar los datos directamente desde la página **Detalles del proyecto** si selecciona **Label data** (Etiquetar datos).

## <a name="add-labels-to-a-project"></a>Incorporación de etiquetas a un proyecto

Durante el proceso de etiquetado es posible que se necesiten etiquetas adicionales para clasificar las imágenes.  Por ejemplo, puede que desee agregar una etiqueta "Desconocido" u "Otro" para indicar que las imágenes son confusas.

Siga estos pasos para agregar una o varias etiquetas a un proyecto:

1. Seleccione el proyecto en la página principal **Data Labeling** (Etiquetado de datos).
1. En la parte superior de la página, seleccione **Pause** (Pausa) para detener la actividad de los etiquetadores.
1. Seleccione la pestaña **Details** (Detalles).
1. En la lista de la izquierda, seleccione **Label classes** (Clases de etiquetas).
1. En la parte superior de la lista, seleccione **+ Add Labels** (+ Agregar etiquetas) ![Add a label](media/how-to-create-labeling-projects/add-label.png) (Agregar una etiqueta).
1. En el formulario, agregue la nueva etiqueta y elija cómo continuar.  Como ha cambiado las etiquetas disponibles para una imagen, elija cómo tratar los datos ya etiquetados:
    * Vuelva a empezar y quite todas las etiquetas existentes.  Elija esta opción si desea empezar a etiquetar desde el principio con el nuevo conjunto de etiquetas completo. 
    * Vuelva a empezar y conserve todas las etiquetas existentes.  Elija esta opción para marcar todos los datos como sin etiquetar, pero mantener las etiquetas existentes como predeterminadas para las imágenes etiquetadas previamente.
    * Continúe y conserve todas las etiquetas existentes. Elija esta opción para mantener todos los datos ya etiquetados como están y empezar a usar la nueva etiqueta para los datos que aún no se hayan etiquetado.
1. Modifique la página de instrucciones según sea necesario para las nuevas etiquetas.
1. Una vez que haya agregado todas las etiquetas nuevas, en la parte superior de la página, seleccione **Start** (Iniciar) para reiniciar el proyecto.  

## <a name="export-the-labels"></a>Exportar las etiquetas

En cualquier momento, puede exportar los datos de etiquetas para realizar experimentos de Machine Learning. Las etiquetas de imagen se pueden exportar en [formato COCO](http://cocodataset.org/#format-data) o como un conjunto de datos de Azure Machine Learning. Use el botón **Exportar** en la página **Detalles del proyecto** del proyecto de etiquetado.

El archivo COCO se crea en el almacén de blobs predeterminado del área de trabajo de Azure Machine Learning, en una carpeta dentro de *export/coco*. Puede acceder al conjunto de datos exportado de Azure Machine Learning en la sección **Conjuntos de datos** de Machine Learning. La página de detalles del conjunto de datos también proporciona código de ejemplo para acceder a las etiquetas desde Python.

![Conjunto de datos exportado](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Pasos siguientes

* Etiquetado de imágenes para la [clasificación de imágenes o la detección de objetos](how-to-label-images.md)
* Obtenga más información sobre [Azure Machine Learning y Machine Learning Studio (clásico)](compare-azure-ml-to-studio-classic.md)
