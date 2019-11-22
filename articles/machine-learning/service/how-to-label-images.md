---
title: Cómo usar la herramienta de etiquetado de datos de Azure Machine Learning
title.suffix: Azure Machine Learning
description: En este artículo se enseña cómo usar las herramientas de etiquetado de datos en un proyecto de etiquetado de Azure Machine Learning.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 9cbebb905ba39dabaf905be2fa741702bd1a0088
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73838942"
---
# <a name="how-to-tag-images-in-a-labeling-project"></a>Cómo etiquetar imágenes en un proyecto de etiquetado

Una vez que el administrador del proyecto haya creado un proyecto de etiquetado en Azure Machine Learning, puede usar la herramienta de etiquetado para preparar rápidamente los datos para un proyecto de aprendizaje automático. 

> [!div class="checklist"]
> * Cómo acceder a proyectos de etiquetado
> * Herramientas de etiquetado
> * Cómo usar las herramientas para tareas específicas de etiquetado

## <a name="prerequisites"></a>Requisitos previos

* La dirección URL del portal de etiquetado para un proyecto de etiquetado de datos en ejecución
* Una [cuenta de Microsoft](https://account.microsoft.com/account) o
* Una cuenta de Azure Active Directory para la organización y el proyecto

> [!Note]
> El administrador del proyecto puede encontrar la dirección URL del portal de etiquetado en la pestaña **Detalles** de la página **Detalles del proyecto**. 

## <a name="logging-in-to-the-projects-labeling-portal"></a>Inicio de sesión en el portal de etiquetado del proyecto

Vaya a la dirección URL del portal de etiquetado que le proporcionó el administrador del proyecto. 

Inicie sesión con la cuenta de correo electrónico que el administrador usó para agregarle al equipo. Para la mayoría de los usuarios, el inicio de sesión se realizará con la cuenta de Microsoft. Si el proyecto de etiquetado usa Azure Active Directory, esa será la forma de iniciar sesión. 

## <a name="understanding-the-labeling-task"></a>Descripción de la tarea de etiquetado

Una vez que haya iniciado sesión, se le dirigirá a la página de información general del proyecto. 

Lo primero que hay que hacer es **Ver las instrucciones detalladas**. Estas instrucciones son específicas de su proyecto y explicarán el tipo de datos a los que se enfrenta, cómo debe tomar sus decisiones y otra información relevante. Cuando haya terminado, vuelva a la página del proyecto y elija **Start labeling** (Iniciar etiquetado).

## <a name="common-features-of-the-labeling-task"></a>Características comunes de la tarea de etiquetado

Todas las tareas de etiquetado de imágenes implican la elección de una etiqueta o etiquetas adecuadas de un conjunto que especifica el administrador del proyecto. Puede seleccionar entre las nueve primeras etiquetas mediante las teclas numéricas del teclado.  

Las tareas de clasificación de imágenes le permiten mostrar varias imágenes simultáneamente. Los distintos diseños se pueden elegir utilizando los iconos situados encima del área de imagen. Puede seleccionar todas las imágenes mostradas al mismo tiempo presionando el botón **Seleccionar todo**. Seleccione fotografías individuales con el botón de selección circular en la esquina superior derecha del área de la imagen. Debe seleccionar al menos una imagen para aplicar una etiqueta. Si ha seleccionado varias imágenes, al seleccionar una etiqueta, se aplicará a cada una de las fotos seleccionadas.

Aquí, hemos elegido un diseño de 2 x 2 y vamos a aplicar la etiqueta "Mammal" a las imágenes del oso y la orca. La imagen con el tiburón ya se ha etiquetado como "Cartilaginous fish" y la iguana todavía no se ha etiquetado.

![Múltiples diseños de imagen y selección](media/how-to-label-images/layouts.png)

> [!Important] 
> Cambie solo de diseño cuando tenga una página nueva de datos sin etiquetar. El cambio de diseño borra el trabajo de etiquetado en curso de la página. 

Azure habilita el botón **Enviar** una vez etiquetadas todas las imágenes de la página. Presione **Enviar** para guardar el trabajo. 

Una vez que haya enviado etiquetas para los datos con los que está trabajando, Azure actualizará la página con un nuevo conjunto de imágenes de la cola de trabajo.  

## <a name="tagging-images-for-multi-class-classification"></a>Etiquetado de imágenes para la clasificación multiclase

Si su proyecto es del tipo "multiclase de clasificación de imágenes", asignará una sola etiqueta a toda la imagen. En cualquier momento, elija la página **Instrucciones** y navegue a **Ver instrucciones detalladas** para ver una guía específica del proyecto. 

Como se explicó anteriormente, puede seleccionar entre varios diseños para presentar imágenes. Si, después de seleccionar una imagen y asignarle una etiqueta, se da cuenta de que ha cometido un error, puede corregirla. Si, en la etiqueta que aparece debajo de la imagen, hace clic en el destino `X`, borrará la etiqueta. O bien, si selecciona la imagen y elige otra clase, la etiqueta cambiará al valor recién seleccionado.

## <a name="tagging-images-for-multi-label-classification"></a>Etiquetado de imágenes para la clasificación multietiqueta

Si está trabajando en un proyecto del tipo de clasificación de imágenes multietiqueta, aplicará una _o más_ etiquetas a una imagen. En cualquier momento, elija la página **Instrucciones** y desplácese hasta **Ver instrucciones detalladas** para ver una guía específica del proyecto. 

Seleccione la imagen que quiera etiquetar y, a continuación, haga clic en la etiqueta. Al elegir la etiqueta, se aplica a todas las imágenes seleccionadas y se deseleccionan. Para aplicar más etiquetas, debe volver a seleccionar las imágenes. Esta animación muestra una página de etiquetado de varias etiquetas:

* **Seleccionar todo** se usa para aplicar la etiqueta "Ocean".
* Se selecciona una sola imagen y se etiqueta como "Closeup".
* Se seleccionan tres imágenes y se etiquetan como "Wide angle"

![Animación que muestra el flujo multietiqueta](media/how-to-label-images/multilabel.gif)

Para corregir un error, puede hacer clic en `X` para borrar etiquetas individuales o seleccionar las imágenes y elegir la etiqueta, lo que borrará la etiqueta de todas las imágenes seleccionadas. Aquí se muestra este escenario, en el que, al hacer clic en "Land", se borra la etiqueta de las dos imágenes seleccionadas.

![Captura de pantalla que muestra varias deselecciones](media/how-to-label-images/multiple-deselection.png)

Azure solo habilitará el botón **Enviar** cuando haya aplicado al menos una etiqueta a cada imagen. Presione **Enviar** para guardar el trabajo.

## <a name="tagging-images-and-bounding-boxes-for-object-detection"></a>Etiquetado de imágenes y cuadros de límite para la detección de objetos

Si el proyecto es del tipo de identificación de objetos (cuadros de límite), deberá especificar uno o varios cuadros de límite en la imagen y aplicar una etiqueta a ese cuadro. Cada imagen puede tener varios cuadros de límite, cada uno con una sola etiqueta. Use **Ver instrucciones detalladas** para determinar si la adición de varios cuadros de límite es adecuada para el proyecto.

1. Seleccionar una etiqueta para el cuadro de límite que quiere crear
1. Seleccione la herramienta **Cuadro rectangular** ![Herramienta Cuadro rectangular](media/how-to-label-images/rectangular-box-tool.png) o presione "R". 
1. Hacer clic y arrastrar diagonalmente en el destino para crear un cuadro de límite simple
    * Para ajustar el cuadro de límite, haga clic y arrastre los bordes o las esquinas del cuadro.

![Captura de pantalla de creación del cuadro de límite básico](media/how-to-label-images/bounding-box-sequence.png)

Si quiere eliminar el cuadro de límite, haga clic en el destino en forma de X que aparece junto al cuadro de límite después de la creación.

No se puede reasignar la etiqueta de un cuadro de límite existente. Si comete un error con la asignación de etiquetas, debe eliminar el cuadro de límite y crear uno nuevo con la etiqueta correcta.

De forma predeterminada, los cuadros de límite existentes se pueden editar. La herramienta **Bloquear/desbloquear regiones** ![Bloquear/desbloquear regiones](media/how-to-label-images/lock-bounding-boxes-tool.png) o "L" alterna ese comportamiento. Si las regiones están bloqueadas, solo puede cambiar la forma o la ubicación de un nuevo cuadro de límite.

Use la herramienta **Manipulación de regiones** ![Herramienta Manipulación de regiones](media/how-to-label-images/regions-tool.png) o "M" para ajustar un cuadro de límite existente. Puede hacer clic y arrastrar en los bordes o esquinas para ajustar la forma. Si hace clic en el interior, puede arrastrar todo el cuadro de límite. Si no puede editar una región, es probable que haya activado la herramienta **Bloquear/desbloquear regiones**. 

Use la herramienta **Cuadro basado en plantilla** ![Herramienta Cuadro de plantilla](media/how-to-label-images/template-box-tool.png) o "T" para crear varios cuadros de límite del mismo tamaño. Si la imagen no tiene cuadros de límite y activa cuadros basados en plantilla, la herramienta generará cuadros de 50 x 50 píxeles. Si ha creado un cuadro de límite y, a continuación, activa los cuadros basados en plantillas, los nuevos cuadros de límite serán del tamaño del último que hizo. Se puede cambiar el tamaño de los cuadros basados en plantillas después de la selección de ubicación. Al cambiar el tamaño de un cuadro basado en una plantilla, solo se cambia el tamaño del cuadro en particular. 

Si quiere desea eliminar _todos_ los cuadros de límite de la imagen actual, puede elegir la herramienta **Eliminar todas las regiones** ![Herramienta Eliminar regiones](media/how-to-label-images/delete-regions-tool.png). 

Una vez que haya creado los cuadros de límite de la imagen, presione **Enviar** para guardar el trabajo. El trabajo en curso no se guardará a menos que presione el botón **Enviar**. 

## <a name="finishing-up"></a>Para acabar 

Cuando envía una página de datos etiquetados, Azure le asigna nuevos datos sin etiquetar de una cola de trabajo. Si no hay más datos sin etiquetar, verá un mensaje a tal efecto, con un vínculo de vuelta a la página principal del portal. 

Si sabe que no va a hacer más etiquetas, elija su nombre en la esquina superior derecha del portal de etiquetado y, a continuación, **Cerrar sesión**. Si no lo hace, Azure acabará finalizando su "tiempo de espera" y asignará sus datos a otro etiquetador. 

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [entrenar modelos de clasificación de imágenes en Azure](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml).
* Obtenga información sobre la [detección de objetos con Azure y la técnica R-CNN más rápida](https://www.microsoft.com/developerblog/2017/10/24/bird-detection-with-azure-ml-workbench/)