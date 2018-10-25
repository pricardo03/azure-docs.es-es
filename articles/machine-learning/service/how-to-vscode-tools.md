---
title: Uso de la extensión Visual Studio Code Tools for AI con Azure Machine Learning
description: Obtenga información sobre Visual Studio Code Tools for AI y sobre cómo comenzar a entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con el servicio Azure Machine Learning en VS Code.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: a2f37cffb37ce7008c3e372763784240e0d4250b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945559"
---
# <a name="vs-code-tools-for-ai-get-started-with-azure-machine-learning-from-visual-studio-code"></a>VS Code Tools for AI: Introducción a Azure Machine Learning desde Visual Studio Code

En este artículo, obtendrá información sobre la extensión de Visual Studio Code (VS Code), **Tools for AI** y sobre cómo comenzar a entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con el servicio Azure Machine Learning en VS Code.

Use la extensión Tools para AI de Visual Studio Code para utilizar el servicio Azure Machine Learning para preparar los datos, entrenar y probar modelos ML en destinos de proceso locales y remotos, implementar esos modelos y realizar un seguimiento de los experimentos y las métricas personalizados.

## <a name="prerequisite"></a>Requisito previo

+ Visual Studio Code debe estar instalado. VS Code es un editor de código fuente ligero pero eficaz que se ejecuta en el escritorio. Incluye compatibilidad integrada para Python y mucho más.  [Obtenga información sobre cómo instalar VS Code](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Instale Python 3.5 o superior](https://www.anaconda.com/download/).

+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="install-vs-code-tools-for-ai-extension"></a>Instalación de la extensión VS Code Tools for AI

Cuando instale la extensión **Tools para AI**, se instalan automáticamente dos extensiones más (si tiene acceso a Internet). Son la extensión [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) y la extensión [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Para trabajar con Azure Machine Learning, necesitamos convertir VS Code en un IDE de Python. Trabajar con [Python en Visual Studio Code](https://code.visualstudio.com/docs/languages/python), requiere la extensión Microsoft Python, que se instala con Tools for AI automáticamente. La extensión hace que VS Code sea un excelente IDE y funciona en cualquier sistema operativo con diversos intérpretes de Python. Aprovecha toda la versatilidad de VS Code para proporcionar la función autocompletar e IntelliSense, búsqueda de errores, depuración y prueba unitaria, junto con la capacidad de cambiar fácilmente entre entornos Python, incluidos los entornos virtuales y de Conda. Consulte [tutorial de la aplicación Hola mundo en Python](https://code.visualstudio.com/docs/python/python-tutorial), que es un tutorial paso a paso sobre edición, ejecución y depuración de código.

**Para instalar la extensión Tools for AI:**

1. Inicie VS Code.

1. En un explorador, visite: http://aka.ms/vscodetoolsforai. 

1. En esa página web, haga clic en **Install** (Instalar). 

1. En la pestaña de la extensión, haga clic en **Install** (Instalar).

1. Se abre una pestaña bienvenida en VS Code para la extensión y el símbolo de Azure se agrega a la barra de actividad.

   ![Icono de Azure en la barra de actividad de Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. En el cuadro de diálogo, haga clic en **Iniciar sesión** y siga las indicaciones de la pantalla para autenticarse con Azure. 
   
   La extensión Azure Account, que se instaló junto con VS Code Tools para AI, le ayuda a autenticarse con su cuenta de Azure. Consulte la lista de comandos en la página de la [extensión Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Consulte la [extensión IntelliCode para VS Code (versión preliminar)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode proporciona un conjunto de funcionalidades asistido por inteligencia artificial para IntelliSense en Python, como inferir las finalizaciones automáticas más relevantes según el contexto de código actual.

## <a name="install-the-sdk"></a>Instalación del SDK

1. Asegúrese de que Python 3.5, o una versión posterior, está instalado y que VS lo reconoce. Si lo instala ahora, reinicie VS Code y seleccione un intérprete de Python siguiendo las instrucciones de https://code.visualstudio.com/docs/python/python-tutorial.

1. En VS Code, abra la paleta de comandos presionando **Ctrl+Mayús+P**.

1. Escriba "Instalar el SDK de Azure ML" para buscar el comando de instalación pip para el SDK. Se crea un entorno de Python local privado que tiene los requisitos previos de Visual Studio Code para trabajar con Azure Machine Learning.
   ![install](./media/vscode-tools-for-ai/install-sdk.png)

1. En la ventana del terminal integrado, especifique el intérprete de Python que desea usar o puede presionar **Intro** para usar el intérprete de Python predeterminado.

   ![instalar](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Introducción a Azure Machine Learning

Antes de iniciar el entrenamiento y la implementación de los modelos de aprendizaje automático mediante VS Code, debe crear un [área de trabajo de Azure Machine Learning Services ](concept-azure-machine-learning-architecture.md#workspace) en la nube para que contenga los modelos y recursos. Aprenda a crear una y cree su primer experimento en esa área de trabajo.

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

   ![instalar](./media/vscode-tools-for-ai/createworkspace.gif)

1. Haga clic con el botón derecho en su suscripción de Azure y seleccione **Crear área de trabajo**. Aparece una lista. En la imagen animada, el nombre de la suscripción es "OpenMind Studio" y el área de trabajo es "MyWorkspace". 

1. Seleccione un grupo de recursos existente de la lista o cree uno nuevo mediante el asistente de la paleta de comandos.

1. En el campo, escriba un nombre único y claro para la nueva área de trabajo. En las capturas de pantalla, el área de trabajo se denomina "MyWorkspace".

1. Presione Intro y se creará la nueva área de trabajo. Aparece en el árbol por debajo del nombre de la suscripción.

1. Haga clic con el botón derecho en el nombre del área de trabajo y elija **Create Experiment** (Crear experimento) en el menú contextual.  Los experimentos realizan un seguimiento de las ejecuciones mediante Azure Machine Learning.

1. En el campo, escriba un nombre para el experimento. En las capturas de pantalla, el experimento se denomina "MNIST".
 
1. Presione Intro y se creará el nuevo experimento. Aparece en el árbol por debajo del nombre de la suscripción.

1. Haga clic con el botón derecho en el nombre del experimento y elija **Attach a local folder** (Asociar una carpeta local). Esta carpeta debe contener los scripts de Python locales. La carpeta se vinculará al experimento en la nube. 

   Ahora cada experimento se ejecuta con el experimento de forma que todas las métricas clave se almacenarán en el historial del experimento y los modelos que entrene se cargarán automáticamente en Azure Machine Learning y se almacenarán con los registros y las métricas del experimento.

   [![Asociación de una carpeta en VS Code](./media/vscode-tools-for-ai/attachfolder.gif)](./media/vscode-tools-for-ai/attachfolder.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Uso de métodos abreviados de teclado

Como la mayoría de VS Code, se puede acceder a las características de Azure Machine Learning en VS Code desde el teclado. La combinación de teclas más importante que debe conocer es Ctrl+Mayús+P, que abre la paleta de comandos. Desde aquí, tendrá acceso a toda la funcionalidad de VS Code, incluidos los métodos abreviados de teclado para las operaciones más habituales.

[![Métodos abreviados de teclado para VS Code Tools for AI](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Ahora puede usar Visual Studio Code para trabajar con Azure Machine Learning.

Obtenga información sobre cómo [crear destinos de proceso, entrenar e implementar modelos en Visual Studio Code](how-to-vscode-train-deploy.md).