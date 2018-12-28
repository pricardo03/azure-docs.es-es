---
title: Migración de un cuaderno de Jupyter Notebook local a Azure Notebooks
description: Transfiera rápidamente un cuaderno de Jupyter Notebook a Azure Notebooks desde el equipo local o una dirección URL web y, posteriormente, a compartirlo con fines de colaboración.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 2e935425-3923-4a33-89b2-0f2100b0c0c4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 3bd7f895e3cc30c4c5a0496977d65d68c220931f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084509"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook"></a>Inicio rápido: Migración de un cuaderno de Jupyter Notebook local

Solo usted puede acceder a los cuadernos de Jupyter Notebook que crea localmente en su propio equipo. Puede compartir los archivos de diferentes maneras, pero los destinatarios tendrán su propia copia local del cuaderno y le será difícil incorporar los cambios que estos realicen. También puede almacenar cuadernos en un repositorio compartido en línea como GitHub, pero para ello es necesario que cada colaborador tenga su propia instalación local de Jupyter con la misma configuración que la suya.

Al migrar los cuadernos basados en repositorio o locales a Azure Notebooks, estos se almacenarán en la nube y podrá compartirlos al instante con sus colaboradores. Los colaboradores solo necesitan un explorador para ver y ejecutar el cuaderno y, si [inician sesión](quickstart-sign-in-azure-notebooks.md) en Azure Notebooks, también podrán realizar cambios.

En este inicio rápido se muestra el proceso de migración de un bloc de notas desde su equipo local u otra dirección URL de archivo accesible. Para migrar los cuadernos desde un repositorio de GitHub, consulte [Inicio rápido: Clonación de un cuaderno](quickstart-clone-jupyter-notebook.md).

## <a name="create-a-project-on-azure-notebooks"></a>Creación de un proyecto en Azure Notebooks

1. Vaya a [Azure Notebooks](https://notebooks.azure.com) e inicie sesión. (Para obtener más información, consulte [Quickstart - Sign in to Azure Notebooks](quickstart-sign-in-azure-notebooks.md) (Inicio rápido: Inicio de sesión en Azure Notebooks)).

1. En la página del perfil público, seleccione **Mis proyectos** en la parte superior de la página:

    ![Vínculo Mis proyectos en la parte superior de la ventana del explorador](media/quickstarts/my-projects-link.png)

1. En la página **Mis proyectos**, seleccione **+ Nuevo proyecto** (método abreviado de teclado: n); el botón puede aparecer solo como **+** si la ventana del explorador es estrecha:

    ![Comando Nuevo proyecto en la página Mis proyectos](media/quickstarts/new-project-command.png)

1. En la ventana emergente **Crear nuevo proyecto**, introduzca los valores adecuados para el cuaderno que va a migrar en los campos **Nombre del proyecto** e **Id. de proyecto**, desactive las opciones **Proyecto público** y **Crear un archivo README.md** y, después, seleccione **Crear**.

## <a name="upload-the-local-notebook"></a>Carga del cuaderno local

1. En la página del proyecto, seleccione **Cargar** (que puede aparecer como solo una flecha hacia arriba si la ventana del explorador es pequeña) y, después, seleccione 1. En la ventana emergente que aparece, seleccione **From computer** (Desde el equipo) si el cuaderno se encuentra en el sistema de archivos local, o **From URL** (Desde una URL) si su cuaderno está en línea:

    ![Comando para cargar un cuaderno desde una dirección URL o el equipo local](media/quickstarts/upload-from-computer-url-command.png)

   (De nuevo, si el cuaderno se encuentra en un repositorio de GitHub, siga los pasos de [Inicio rápido: Clonación de un cuaderno](quickstart-clone-jupyter-notebook.md).)

    - Si usa **From Computer** (Desde el equipo), arrastre y coloque los archivos *.ipynb* en la ventana emergente o seleccione **Elegir archivos** y, después, vaya a los archivos que quiere importar. A continuación, seleccione **Cargar**. Los archivos cargados tienen el mismo nombre que los archivos locales. (No es necesario cargar el contenido de ninguna carpeta *.ipynb_checkpoints*.)

    ![Ventana emergente para cargar desde equipo](media/quickstarts/upload-from-computer-popup.png)

    - Si usa **From URL** (Desde la URL), escriba la dirección de origen en el campo **Dirección URL del archivo** y el nombre de archivo que quiere asignar al cuaderno en el proyecto en el campo **Nombre de archivo**. A continuación, seleccione **Cargar**. Si tiene varios archivos con direcciones URL diferentes, use el comando **+ Agregar archivo** para comprobar la primera dirección URL que escribió, después de que la ventana emergente proporcione nuevos campos para otro archivo.

    ![Ventana emergente para cargar desde URL](media/quickstarts/upload-from-url-popup.png)

1. Abra y ejecute el cuaderno recién cargado para comprobar su contenido y la operación. Cuando haya terminado, seleccione **Archivo** > **Detener y cerrar** para cerrar el cuaderno.

1. Para compartir un vínculo en el cuaderno cargado, haga clic en el archivo del proyecto y seleccione **Copiar vínculo** (método abreviado de teclado: y) y, después, péguelo en el mensaje adecuado. Como alternativa, puede compartir el proyecto como un todo con el control **Compartir** en la página del proyecto.

1. Para editar los archivos que no sean cuadernos, haga clic en el archivo del proyecto y seleccione **Editar archivo** (método abreviado de teclado: i). La acción predeterminada, **Ejecutar** (método abreviado de teclado: r), solo se muestra el contenido del archivo y no permite la edición.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: create an run a Jupyter notebook to do linear regression](tutorial-create-run-jupyter-notebook.md) (Tutorial: Creación y ejecución de un cuaderno de Jupyter Notebook para hacer regresión lineal)
