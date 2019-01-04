---
title: Creación de un proyecto de Azure Notebooks con un entorno personalizado
description: Cree un proyecto en Azure Notebooks que esté configurado con un conjunto específico de scripts de inicio y paquetes instalados.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: e049f591-27a7-440f-a1a3-c5bef25e8a28
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 9e5f4d6f2086048e428f313bc49cc6f599ee1bec
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085338"
---
# <a name="quickstart-create-a-project-with-a-custom-environment"></a>Inicio rápido: Creación de un proyecto con un entorno personalizado

Un proyecto de Azure Notebooks es una colección de archivos, como cuadernos, archivos de datos, documentación, imágenes, etc. junto con un entorno que se puede configurar con comandos de instalación específicos. Al definir el entorno con el proyecto, cualquier usuario que clone el proyecto en su propia cuenta de Azure Notebooks tiene toda la información necesaria para volver a crear el entorno necesario.

## <a name="create-a-project"></a>Crear un proyecto

1. Vaya a [Azure Notebooks](https://notebooks.azure.com) e inicie sesión. (Para más información, consulte [Quickstart - Sign in to Azure Notebooks](quickstart-sign-in-azure-notebooks.md) [Inicio rápido: Inicio de sesión en Azure Notebooks]).

1. En la página del perfil público, seleccione **Mis proyectos** en la parte superior de la página:

    ![Vínculo Mis proyectos en la parte superior de la ventana del explorador](media/quickstarts/my-projects-link.png)

1. En la página **Mis proyectos**, seleccione **+ Nuevo proyecto** (método abreviado de teclado: n); el botón puede aparecer solo como **+** si la ventana del explorador es estrecha:

    ![Comando Nuevo proyecto en la página Mis proyectos](media/quickstarts/new-project-command.png)

1. En la ventana emergente **Crear nuevo proyecto** que aparece, escriba o establezca estos detalles y, luego, seleccione **Crear**:

    - **Nombre del proyecto**: Proyecto con un entorno personalizado
    - **Identificador del proyecto**: project-custom-environment
    - **Proyecto público**: (desactivado)
    - **Crear un archivo README.md**: (desactivado)

1. Después de unos instantes, Azure Notebooks lo lleva al proyecto nuevo. Para agregar un cuaderno al proyecto, seleccione el menú desplegable **+ Nuevo** (que puede aparecer simplemente como **+**) y seleccione **Cuaderno**.

1. Asigne al cuaderno un nombre similar a *Entorno personalizado.ipynb*, seleccione **Python 3.6** como el lenguaje y, luego, seleccione **Nuevo**.

## <a name="add-a-custom-setup-step"></a>Incorporación de un paso de configuración personalizado

1. En la página del proyecto, seleccione **Configuración del proyecto**.

    ![Comando de configuración del proyecto](media/quickstarts/project-settings-command.png)

1. En la ventana emergente **Configuración del proyecto**, seleccione la pestaña **Entorno** y luego, en **Environment Setup Steps** (Pasos para la configuración del entorno), seleccione **+ Agregar**:

    ![Comando para agregar un nuevo paso para la configuración del entorno](media/quickstarts/environment-add-command.png)

1. El comando **+ Agregar** crea un paso definido por una operación y un archivo de destino que se selecciona de los archivos del proyecto. Se admiten las operaciones siguientes:

    | Operación | DESCRIPCIÓN |
    | --- | --- |
    | Requirements.txt | Los proyectos de Python definen sus dependencias en un archivo llamado requirements.txt. Con esta opción, seleccione el archivo adecuado en la lista de archivos del proyecto y seleccione también la versión de Python en la otra lista desplegable que aparece. Si es necesario, seleccione **Cancelar** para volver al proyecto, cargue o cree el archivo y, luego, vuelva a la pestaña **Configuración del proyecto** > **Entorno** para crear un paso. Una vez que se crea este paso, al ejecutar un cuaderno en el proyecto, automáticamente se ejecuta `pip install -r <file>` |
    | Script de shell | Úselo para indicar un script de shell de bash (por lo general, un archivo con extensión *.sh*) que contiene cualquier comando que quiere ejecutar para inicializar el entorno. |
    | Environment.yml | Un proyecto de Python que usa conda para administrar un entorno usa un archivo *environments.yml* para describir las dependencias. Con esta opción, seleccione el archivo correspondiente en la lista de archivos del proyecto. |

1. Para quitar cualquier paso de configuración, seleccione la **X** que se encuentra a la derecha del paso.

1. Una vez que estén todos los pasos de configuración, seleccione **Guardar**. (Seleccione **Cancelar** para descartar los cambios).

1. Para probar el entorno, cree y ejecute un cuaderno nuevo y, luego, cree una celda de código con instrucciones que dependan de un paquete del entorno, como usar una instrucción `import` de Python. Si la instrucción se realiza correctamente, el paquete necesario se instaló correctamente en el entorno.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Manage and configure projects in Azure Notebooks](configure-manage-azure-notebooks-projects.md) (Administración y configuración de proyectos en Azure Notebooks)

> [!div class="nextstepaction"]
> [Tutorial: create an run a Jupyter notebook to do linear regression](tutorial-create-run-jupyter-notebook.md) (Tutorial: Creación y ejecución de un cuaderno de Jupyter Notebook para hacer regresión lineal)
