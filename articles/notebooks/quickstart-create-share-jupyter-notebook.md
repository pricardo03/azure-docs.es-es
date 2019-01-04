---
title: Creación y uso compartido de un cuaderno de Jupyter Notebook en Azure
description: Obtenga información sobre cómo crear y ejecutar un cuaderno de Jupyter Notebook en Azure Notebooks y, luego, compartir ese cuaderno con otros usuarios.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 5add60ad-0b4b-4fd5-adf5-eb50ce072d00
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 96eb6a823299521a071edc87b5bce95409405ffb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096280"
---
# <a name="quickstart-create-and-share-a-notebook"></a>Inicio rápido: Creación y uso compartido de un cuaderno

1. Vaya a [Azure Notebooks](https://notebooks.azure.com) e inicie sesión. (Para más información, consulte [Quickstart - Sign in to Azure Notebooks](quickstart-sign-in-azure-notebooks.md) [Inicio rápido: Inicio de sesión en Azure Notebooks]).

1. En la página del perfil público, seleccione **Mis proyectos** en la parte superior de la página:

    ![Vínculo Mis proyectos en la parte superior de la ventana del explorador](media/quickstarts/my-projects-link.png)

1. En la página **Mis proyectos**, seleccione **+ Nuevo proyecto** (método abreviado de teclado: n); el botón puede aparecer solo como **+** si la ventana del explorador es estrecha:

    ![Comando Nuevo proyecto en la página Mis proyectos](media/quickstarts/new-project-command.png)

1. En la ventana emergente **Crear nuevo proyecto** que aparece, escriba o establezca estos detalles y, luego, seleccione **Crear**:

    - **Nombre del proyecto**: Hola mundo en Python
    - **Identificador del proyecto**: hello-world-python
    - **Proyecto público**: (desactivado)
    - **Crear un archivo README.md**: (desactivado)

    ![Ventana emergente Nuevo proyecto con los detalles completados](media/quickstarts/new-project-popup.png)

1. Después de unos instantes, Azure Notebooks lo lleva al proyecto nuevo. Para agregar un cuaderno al proyecto, seleccione el menú desplegable **+ Nuevo** (que puede aparecer simplemente como **+**) y seleccione **Cuaderno**:

    [![](media/quickstarts/empty-project-new-notebook-button.png "Un proyecto nuevo vacío y el comando para agregar cuaderno")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. En la ventaja emergente **Create New Notebook** (Crear nuevo cuaderno) que aparece, escriba un nombre de archivo para el cuaderno, como *HelloWorldInPython.ipynb* (*.ipynb* se refiere a IronPython (Jupyter) Notebook) y seleccione **Python 3.6** en el lenguaje (que también se denomina como el *kernel*):

    ![La ventana emergente para crear un cuaderno](media/quickstarts/new-notebook-popup.png)

1. Seleccione **Nuevo** para terminar de crear el cuaderno, el que aparecerá en la lista de archivos del proyecto en cuestión:

    ![El cuaderno nuevo aparece en la lista de archivos del proyecto](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Ejecución del cuaderno

1. Seleccione el cuaderno nuevo para ejecutarlo en el editor; el kernel que seleccionó (en este ejemplo, Python 3.6) se activa automáticamente para este cuaderno:

    ![Vista de un cuaderno nuevo en Azure Notebooks](media/quickstarts/create-notebook-first-open.png)

1. De manera predeterminada, el cuaderno tiene una celda de código vacía. Para cambiar el tipo de celda a **Markdown**, use el menú desplegable de tipos de celda para seleccionar **Markdown**:

    ![Cambio del tipo de celda de un cuaderno nuevo](media/quickstarts/create-notebook-cell-type.png)

1. Escriba o pegue el texto de encabezado siguiente en la celda:

    ```markdown
    # Hello World in Python
    ```

1. Como se edita Markdown, el texto aparece como encabezado con el carácter "#". Para representar el archivo Markdown en HTML, seleccione el botón **Ejecutar**. Luego, Azure Notebooks crea automáticamente una celda de código nueva:

    ![El botón de ejecución de una celda y la representación del archivo Markdown](media/quickstarts/run-cell-markdown-render.png)

1. En la celda de código, escriba el código de Python siguiente:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Seleccione **Ejecutar** (método abreviado de teclado: Mayús + ENTRAR) para ejecutar el código. Debajo de la celda debería aparecer una salida correcta similar al texto siguiente:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Seleccione el icono Guardar para guardar el trabajo:

    ![Icono Guardar en la barra de herramientas de Jupyter Notebook](media/quickstarts/hello-results-save-icon.png)

1. Seleccione el comando del menú **Archivo** > **Halt and close** (Detener y cerrar) para detener el servidor y cerrar la ventana del explorador.

## <a name="share-the-notebook"></a>Uso compartido del cuaderno

Para compartir el cuaderno, vuelva a la página del proyecto si es necesario, haga clic con el botón derecho en el archivo del cuaderno, seleccione **Copiar vínculo** (método abreviado de teclado: y) y pegue el vínculo en un mensaje adecuado (correo electrónico, mensaje instantáneo, etc.).

En la página del proyecto, también puede usar el menú **Compartir** para obtener un vínculo, crear un mensaje de correo electrónico con el vínculo u obtener el código HTML y Markdown para insertar:

![Comando para compartir el proyecto](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: create an run a Jupyter notebook to do linear regression](tutorial-create-run-jupyter-notebook.md) (Tutorial: Creación y ejecución de un cuaderno de Jupyter Notebook para hacer regresión lineal)
