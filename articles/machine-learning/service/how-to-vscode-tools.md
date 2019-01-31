---
title: Uso de Visual Studio Code con
titleSuffix: Azure Machine Learning service
description: Aprenda a instalar Azure Machine Learning para Visual Studio Code y crear un sencillo experimento en Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: d6f49eb0359db6184b5ecd146d7328a64611a9f9
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245574"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Introducción a Azure Machine Learning para Visual Studio Code

En este artículo aprenderá a instalar la extensión **Azure Machine Learning para Visual Studio Code** extensión y a crear su primer experimento con el servicio Azure Machine Learning en Visual Studio Code (VS Code).

Use la extensión Azure Machine Learning en Visual Studio Code para utilizar el servicio Azure Machine Learning para preparar los datos, entrenar y probar modelos de aprendizaje automático en destinos de proceso locales y remotos, implementar esos modelos y realizar un seguimiento de los experimentos y las métricas personalizados.

## <a name="prerequisite"></a>Requisito previo


+ Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

+ Visual Studio Code debe estar instalado. VS Code es un editor de código fuente ligero pero eficaz que se ejecuta en el escritorio. Incluye compatibilidad integrada para Python y mucho más.  [Obtenga información sobre cómo instalar VS Code](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Instale Python 3.5 o superior](https://www.anaconda.com/download/).


## <a name="install-the-azure-machine-learning-for-vs-code-extension"></a>Instalación de la extensión Azure Machine Learning para VS Code

Cuando instale la extensión **Azure Machine Learning**, se instalan automáticamente dos extensiones más (si tiene acceso a Internet). Son la extensión [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) y la extensión [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Para trabajar con Azure Machine Learning, necesitamos convertir VS Code en un IDE de Python. Trabajar con [Python en Visual Studio Code](https://code.visualstudio.com/docs/languages/python), requiere la extensión Microsoft Python, que se instala con la extensión Azure Machine Learning automáticamente. La extensión hace que VS Code sea un excelente IDE y funciona en cualquier sistema operativo con diversos intérpretes de Python. Aprovecha toda la versatilidad de VS Code para proporcionar la función autocompletar e IntelliSense, búsqueda de errores, depuración y prueba unitaria, junto con la capacidad de cambiar fácilmente entre entornos Python, incluidos los entornos virtuales y de Conda. Consulte [tutorial de la aplicación Hola mundo en Python](https://code.visualstudio.com/docs/python/python-tutorial), que es un tutorial paso a paso sobre edición, ejecución y depuración de código.

**Para instalar la extensión Azure Machine Learning:**

1. Inicie VS Code.

1. En un explorador, visite: Extensión [Azure Machine Learning para Visual Studio Code (versión preliminar)](https://aka.ms/vscodetoolsforai)

1. En esa página web, haga clic en **Install** (Instalar). 

1. En la pestaña de la extensión, haga clic en **Install** (Instalar).

1. Se abre una pestaña bienvenida en VS Code para la extensión y el símbolo de Azure se agrega a la barra de actividad.

   ![Icono de Azure en la barra de actividad de Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. En el cuadro de diálogo, haga clic en **Iniciar sesión** y siga las indicaciones de la pantalla para autenticarse con Azure. 
   
   La extensión Azure Account, que se instaló junto con la extensión Azure Machine Learning para VS Code, le ayuda a autenticarse con su cuenta de Azure. Consulte la lista de comandos en la página de la [extensión Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Consulte la [extensión IntelliCode para VS Code (versión preliminar)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode proporciona un conjunto de funcionalidades asistido por inteligencia artificial para IntelliSense en Python, como inferir las finalizaciones automáticas más relevantes según el contexto de código actual.

## <a name="install-the-sdk"></a>Instalación del SDK

1. Asegúrese de que Python 3.5, o una versión posterior, está instalado y que VS lo reconoce. Si lo instala ahora, reinicie VS Code y seleccione un intérprete de Python siguiendo las instrucciones de https://code.visualstudio.com/docs/python/python-tutorial.

1. En VS Code, abra la paleta de comandos con **Ctrl+Mayús+P**.

1. Escriba "Instalar el SDK de Azure ML" para buscar el comando de instalación pip para el SDK. Se crea un entorno de Python local privado que tiene los requisitos previos de Visual Studio Code para trabajar con Azure Machine Learning.

   ![Instalación del SDK de Azure Machine Learning para Python](./media/vscode-tools-for-ai/install-sdk.png)

1. En la ventana del terminal integrado, especifique el intérprete de Python que desea usar o presione **Intro** para usar el intérprete de Python predeterminado.

   ![Elección del intérprete](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Introducción a Azure Machine Learning

Antes de iniciar el entrenamiento y la implementación de los modelos de aprendizaje automático mediante VS Code, debe crear un [área de trabajo de Azure Machine Learning Services](concept-azure-machine-learning-architecture.md#workspace) en la nube para que contenga los modelos y los recursos. Aprenda a crear una y cree su primer experimento en esa área de trabajo.

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

   [![Instalación](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Haga clic con el botón derecho en su suscripción de Azure y seleccione **Crear área de trabajo**. Aparece una lista. En la imagen animada, el nombre de la suscripción es "Evaluación gratuita" y el área de trabajo es "TeamWorkspace". 

1. Seleccione un grupo de recursos existente de la lista o cree uno mediante el asistente de la paleta de comandos.

1. En el campo, escriba un nombre único y claro para la nueva área de trabajo. En las capturas de pantalla, el área de trabajo se denomina "TeamWorkspace".

1. Presione Intro y se creará la nueva área de trabajo. Aparece en el árbol por debajo del nombre de la suscripción.

1. Haga clic con el botón derecho en el nodo Experimento y elija **Create Experiment** (Crear experimento) en el menú contextual.  Los experimentos realizan un seguimiento de las ejecuciones mediante Azure Machine Learning.

1. En el campo, escriba un nombre para el experimento. En las capturas de pantalla, el experimento se denomina "MNIST".
 
1. Presione Intro y se creará el nuevo experimento. Aparece en el árbol por debajo del nombre de la suscripción.

1. Puede hacer clic con el botón derecho en un experimento en un área de trabajo y seleccionar "Establecer como experimento activo". El experimento **'Activo'** es el que se usa actualmente y la carpeta abierta en VS Code se vinculará a él en la nube. Esta carpeta debe contener los scripts de Python locales.

   Ahora, cada experimento se ejecuta con su experimento, de forma que todas las métricas clave se almacenarán en el historial del experimento y los modelos que entrene se cargarán automáticamente en Azure Machine Learning y se almacenarán con los registros y las métricas del experimento.

   [![Asociación de una carpeta en VS Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Uso de métodos abreviados de teclado

Como la mayoría de VS Code, se puede acceder a las características de Azure Machine Learning en VS Code desde el teclado. La combinación de teclas más importante que debe conocer es Ctrl+Mayús+P, que abre la paleta de comandos. Desde aquí, tendrá acceso a toda la funcionalidad de VS Code, incluidos los métodos abreviados de teclado para las operaciones más habituales.

[![Métodos abreviados de teclado para Azure Machine Learning para VS Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Ahora puede usar Visual Studio Code para trabajar con Azure Machine Learning.

Obtenga información sobre cómo [crear destinos de proceso, entrenar e implementar modelos en Visual Studio Code](how-to-vscode-train-deploy.md).
