---
title: Presentación de un cuaderno de Jupyter Notebook como una presentación en Azure
description: Cómo configurar las celdas para el modo de presentación en un cuaderno de Jupyter Notebook y, después, realizar la presentación con la extensión RISE.
services: app-service
documentationcenter: ''
author: kraigb
manager: barbkess
ms.assetid: c372175b-beb5-4b45-b2f8-34cb06990117
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 04612dc8892a544397251580b3494badc84e2ad5
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973111"
---
# <a name="run-a-notebook-slideshow"></a>Ejecutar una presentación de un cuaderno

El servicio Azure Notebooks está preconfigurado con la extensión de presentación Jupyter/IPython (RISE) que permite presentar un cuaderno directamente como una presentación. En una presentación, las celdas se muestran normalmente de una en una con un tamaño de fuente adecuado para presentar en pantallas grandes y aún puede ejecutar el código en lugar de cambiar a un equipo independiente de demostración.

En la siguiente imagen se muestra la vista estándar del cuaderno, en el que puede ver las celdas de Markdown y código juntas:

![Cuaderno en la vista estándar](media/slideshow/slideshow-notebook-view.png)

Al iniciar una presentación, la primera celda se amplía para rellenar el explorador, donde el símbolo **X** en la parte superior izquierda permite cerrar la presentación; el símbolo **?** en la parte inferior izquierda muestra los métodos abreviados de teclado; y las flechas en la esquina inferior derecha permiten desplazarse entre las diapositivas:

![Cuaderno en el modo de presentación](media/slideshow/slideshow-slide-view.png)

La preparación de un cuaderno para una presentación implica dos actividades principales:

1. Debido a que las celdas Markdown se representan con fuentes grandes, es posible que haya contenido que no sea visible en la presentación. Por lo tanto, la cantidad de texto en cualquier celda dada debe limitarse; un encabezado entre cuatro y seis líneas suele funcionar mejor. Si tiene más texto, divida esa información en varias celdas.

2. Configure el comportamiento de cada celda en la presentación con la barra de herramientas para celdas de la presentación. Los tipos de celda determinan el comportamiento de los botones de navegación.

## <a name="the-anatomy-of-a-slideshow"></a>Anatomía de una presentación

Si coge un cuaderno aleatorio y lo usa para una presentación, verá que todas las celdas están mezcladas y gran parte del contenido se oculta en la parte inferior de la ventana del explorador. Para realizar una presentación eficaz, deberá asignar un tipo de presentación a cada celda con la barra de herramientas para celdas de la presentación:

1. En el menú **Vista**, seleccione **Cell Toolbar (Barra de herramientas de celda)**  > **Presentación**:

    ![Activación de la barra de herramientas de presentación de celdas](media/slideshow/slideshow-view-cell-toolbar.png)

1. Se muestra un menú desplegable **Tipo de diapositiva** en la parte superior derecha de cada celda del cuaderno:

    ![Barra de herramientas de presentación de celdas](media/slideshow/slideshow-cell-toolbar.png)

1. Para cada celda, seleccione uno de estos cinco tipos:

    ![Tipos de presentación de celdas](media/slideshow/slideshow-cell-slide-types.png)

    | Tipo de diapositiva | Comportamiento |
    | --- | --- |
    | - (sin establecer) | La celda se muestra con la celda anterior, lo que no suele ser un efecto deseado en una presentación. |
    | Diapositiva | La celda es una diapositiva principal, que se navega con las flechas izquierda y derecha del control de navegación. |
    | Diapositiva secundaria | La celda está "debajo" de una diapositiva principal, que se navega con la flecha hacia abajo del control de navegación. La flecha hacia arriba hace volver a la diapositiva principal. Las diapositivas secundarias se usan para el material secundario que podría omitir en la ruta principal de una presentación, pero están disponibles si es necesario. |
    | Fragmento | El contenido de la celda aparece en el contexto de la diapositiva anterior o de la diapositiva secundaria cuando se usa la flecha hacia abajo de navegación (se elimina un fragmento al usar la flecha hacia arriba). Puede usar un fragmento con una celda de código para hacer que el código aparezca dentro de una diapositiva o puede usar varios fragmentos para que las viñetas del texto aparezcan una a una (vea el ejemplo de la sección siguiente). Debido a que los fragmentos se compilan en la diapositiva actual, los fragmentos excedentes no se verán en la parte inferior de la ventana del explorador. |
    | Skip | La celda no se muestra en la presentación. |
    | Notas | Las celdas contienen notas del orador, que no se muestran en la presentación. |

1. Inicialmente, resulta útil elegir **Diapositiva** para cada una de las celdas. A continuación, puede ejecutar la presentación y realizar los ajustes adecuados.

### <a name="example-fragment-cells-for-bullet-items"></a>Ejemplo: fragmentar celdas para los elementos de viñeta

Para hacer que las viñetas aparezcan una a una en una diapositiva, coloque el encabezado de la diapositiva en una celda Markdown con el tipo **Diapositiva** y, después, coloque cada viñeta en una celda Markdown independiente con el tipo **Fragmento**:

![Ejemplo de la creación de diferentes celdas Markdown para elementos de viñeta](media/slideshow/slideshow-fragments.png)

Dado que la presentación representa fragmentos con más espaciado vertical que cuando todas las viñetas están en la misma celda, es posible que no pueda usar tantos elementos de viñeta.

## <a name="run-the-slideshow"></a>Ejecutar como presentación

1. Si ha editado las celdas de Markdown, asegúrese de ejecutarlas para representar su HTML; en caso contrario, aparecen *como* Markdown en la presentación.

1. Una vez que configurado el **tipo de diapositiva** para cada celda, seleccione la celda con la que se va a iniciar la presentación y, después, seleccione el botón **Entrar en/salir de la presentación RISE** en la barra de herramientas principal:

    ![Botón para entrar o salir de la presentación RISE en la barra de herramientas principal](media/slideshow/slideshow-start.png)

1. Para navegar entre diapositivas y entre fragmentos, utilice las flechas izquierda y derecha en el control de navegación. El texto del control muestra un número que representa el valor de *slide.sub-slide*.

    ![Control de navegación de la presentación](media/slideshow/slideshow-navigation-control.png)

1. Para desplazarse entre diapositivas y diapositivas secundarias, así como entre fragmentos, use las flechas arriba y abajo si están habilitadas:

    ![Controles de navegación de la presentación de diapositivas secundarias](media/slideshow/slideshow-navigation-control-subslide.png)

1. En una celda de código, use el botón de reproducción para ejecutar el código; la salida aparece en la diapositiva:

    ![Botón de reproducción para ejecutar una celda de código](media/slideshow/slideshow-run-code-cell.png)

    ![La salida de la celda de código aparece en la presentación](media/slideshow/slideshow-run-code-cell-output.png)

    > [!Tip]
    > La salida de la celda se considera parte de la celda en una presentación. Si ejecuta una celda en el cuaderno o en la vista de presentación, la salida aparece también en la otra vista. Para borrar la salida, use el comando **Celda** > **Salidas actuales** > **Borrar** (para la celda actual) o **Celda**  >  **Todas las salidas** > **Borrar** (para todas las celdas).

1. Cuando haya terminado con la presentación, utilice el botón **X** para volver a la vista de cuaderno.

## <a name="next-steps"></a>Pasos siguientes

- [How to: Configure and manage projects](configure-manage-azure-notebooks-projects.md) (Configuración y administración de proyectos)
- [Procedimientos: Instalación de paquetes en un cuaderno](install-packages-jupyter-notebook.md)
- [Instrucciones: con archivos de datos en proyectos de Azure Notebooks](work-with-project-data-files.md)
- [How to: Acceso a recursos de datos](access-data-resources-jupyter-notebooks.md)
