---
title: Introducción al servicio Azure Machine Learning con la extensión para la CLI | Microsoft Docs
description: En este tutorial de inicio rápido, aprenderá empezar a trabajar con el servicio Azure Machine Learning mediante la extensión de la CLI de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
author: rastala
ms.author: roastala
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 856f9629e97f8cf7cf811e7d591cbcad6067f47a
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237167"
---
# <a name="quickstart-get-started-with-azure-machine-learning-using-the-cli-extension"></a>Guía de inicio rápido: Introducción a Azure Machine Learning con la extensión de la CLI

En este tutorial, usará una extensión de la CLI de Machine Learning para empezar a trabajar con el [servicio Azure Machine Learning](overview-what-is-azure-ml.md) (versión preliminar).

Mediante la CLI, obtendrá información sobre cómo:

1. Crear un área de trabajo en la suscripción de Azure. Uno o varios usuarios usan el área de trabajo para almacenar recursos de proceso, modelos e implementaciones, y ejecutar historiales en la nube.
1. Adjuntar un proyecto al área de trabajo.   Un proyecto es una carpeta local que contiene los scripts y archivos de configuración necesarios para solucionar el problema de aprendizaje automático.  
1. Ejecutar un script de Python en el proyecto, que registre algunos valores a través de varias iteraciones.
1. Ver los valores registrados en el historial de ejecución del área de trabajo.

> [!NOTE]
> Para su comodidad, los siguientes recursos de Azure se agregarán automáticamente al área de trabajo cuando estén disponibles de forma regional: [Container Registry](https://azure.microsoft.com/services/container-registry/), [Storage](https://azure.microsoft.com/services/storage/), [Application Insights](https://azure.microsoft.com/services/application-insights/) y [Key Vault](https://azure.microsoft.com/services/key-vault/).

Los recursos creados pueden usarse como requisitos previos para otros tutoriales y artículos de procedimientos de Azure Machine Learning.

Esta CLI se creó sobre el <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> basado en Python para el servicio Azure Machine Learning.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que cumple los siguientes requisitos previos antes de empezar con los pasos de la guía de inicio rápido:

+ Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
+ [Python 3.5 o superior](https://www.python.org/) instalado
+ [La CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalada

## <a name="install-the-cli-extension"></a>Instalación de la extensión de la CLI

En el equipo, abra un editor de la línea de comandos e instale [la extensión de aprendizaje automático en la CLI de Azure](reference-azure-machine-learning-cli.md).  La instalación puede tardar varios minutos en completarse.

```azurecli
az extension add azureml-sdk
```

## <a name="install-the-sdk"></a>Instalación del SDK

[!INCLUDE [aml-install-sdk](../../../includes/aml-install-sdk.md)]

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. Con la CLI de Azure, inicie sesión en Azure, especifique la suscripción y cree un grupo de recursos.

En una ventana de la línea de comandos, inicie sesión con el comando de la CLI de Azure, `az login`. Siga las indicaciones para el inicio de sesión interactivo:
    
   ```azurecli
   az login
   ```

Enumere las suscripciones de Azure disponibles y especifique lo que quiera usar:
   ```azurecli
   az account list --output table
   az account set --subscription <your-subscription-id>
   az account show
   ```
   donde \<your-subscription-id\> es el valor del identificador para la suscripción que quiere usar. No incluya los corchetes.

Cree un grupo de recursos que contenga el área de trabajo.
En esta guía de inicio rápido:
   + El nombre del grupo de recursos es `docs-aml`.
   + La región es `eastus2`. 

   ```azurecli
   az group create -n docs-aml -l eastus2
   ```

## <a name="create-a-workspace-and-a-project-folder"></a>Creación de un área de trabajo y una carpeta de proyecto

En la ventana de línea de comandos, cree un área de trabajo del servicio Azure Machine Learning en el grupo de recursos.


   En esta guía de inicio rápido:
   + El nombre del área de trabajo es `docs-ws`.
   + El nombre del grupo de recursos es `docs-aml`.

   ```azurecli
   az ml workspace create -n docs-ws -g docs-aml
   ```

En la ventana de línea de comandos, cree una carpeta en el equipo local para el proyecto de Azure Machine Learning.

   ```
   mkdir docs-prj
   cd docs-prj
   ```

## <a name="create-a-python-script"></a>Creación de un script de Python

[!INCLUDE [aml-create-script-pi](../../../includes/aml-create-script-pi.md)]

## <a name="run-the-script"></a>Ejecute el script

Adjunte la carpeta como proyecto al área de trabajo. El argumento `--history` especifica un nombre para el archivo del historial de ejecución que captura las métricas para cada ejecución.

   ```azurecli
   az ml project attach --history my_history -w docs-ws -g docs-aml
   ```

Ejecute el script en el equipo local.

   ```azurecli
   az ml run submit -c local pi.py
   ```

   Este comando ejecuta el código y genera un vínculo web a la consola. Copie el vínculo y péguelo en el explorador web.

En un explorador web, visite la dirección URL. Aparece un portal web con los resultados de la ejecución. Puede inspeccionar los resultados de esa ejecución o de ejecuciones anteriores, si existen.

El panel del portal solo es compatible con los exploradores de Edge, Chrome y Firefox.

   ![Visualización del historial](./media/quickstart-get-started/web-results.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes
Ya ha creado los recursos necesarios para comenzar a experimentar e implementar modelos. También ha creado un proyecto, ha ejecutado un script y ha explorado el historial de ejecuciones del script.

Para ver de forma detallada la experiencia de flujo de trabajo, siga los tutoriales de Azure Machine Learning para crear, entrenar e implementar un modelo.

> [!div class="nextstepaction"]
> [Tutorial: Crear, entrenar e implementar](tutorial-train-models-with-aml.md)