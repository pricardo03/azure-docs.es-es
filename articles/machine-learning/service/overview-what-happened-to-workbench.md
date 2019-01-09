---
title: ¿Qué ha ocurrido con Machine Learning Workbench?
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre lo que ocurrió con la aplicación Machine Learning Workbench, qué cambió en Azure Machine Learning Service y qué es la escala de tiempo del soporte técnico.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ff6b61874363bbc869bd509174e58640a2487f56
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811314"
---
# <a name="whats-happening-to-machine-learning-workbench-in-azure-machine-learning-service"></a>¿Qué ocurre con Machine Learning Workbench en Azure Machine Learning Service?

La aplicación Azure Machine Learning Workbench y algunas otras características tempranas han quedado en desuso y se han reemplazado en la versión de septiembre de 2018 para dar paso a una [arquitectura](concept-azure-machine-learning-architecture.md) mejorada. Para mejorar la experiencia, la versión contiene muchas actualizaciones importantes solicitadas por los comentarios de los clientes. La funcionalidad básica de ejecuciones de experimento hasta la implementación de modelos no ha cambiado, pero ahora puede usar el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> sólido y la [CLI de Azure](reference-azure-machine-learning-cli.md) para realizar sus tareas y canalizaciones de aprendizaje automático.  

En este artículo obtendrá información sobre qué ha cambiado y cómo ello afecta al trabajo previamente existente con Azure Machine Learning Workbench y sus API.

## <a name="what-changed"></a>¿Qué ha cambiado?

La versión más reciente de Azure Machine Learning Service incluye las características siguientes:
+ Un [modelo simplificado de los recursos de Azure](concept-azure-machine-learning-architecture.md).
+ Una [nueva interfaz de usuario del portal](how-to-track-experiments.md) para administrar sus experimentos y destinos de proceso.
+ Un <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> nuevo y más completo de Python.
+ La nueva [extensión de la CLI de Azure](reference-azure-machine-learning-cli.md) expandida para el aprendizaje automático.

La [arquitectura](concept-azure-machine-learning-architecture.md) se rediseñó para facilitar el uso. En lugar de varias cuentas y recursos de Azure, solo necesita un [área de trabajo del servicio Azure Machine Learning](concept-azure-machine-learning-architecture.md#workspace). Puede crear áreas de trabajo rápidamente en [Azure Portal](quickstart-get-started.md). Al usar un área de trabajo, varios usuarios pueden almacenar destinos de proceso de entrenamiento e implementación, experimentos de modelos, imágenes de Docker, modelos implementados, etc.

Aunque hay nuevos clientes mejorados de CLI y SDK en la versión actual, la propia aplicación de escritorio Workbench está en desuso. Ahora, puede supervisar sus experimentos en el [panel del área de trabajo en Azure Portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Use el panel para obtener el historial de experimentos, administrar los destinos de proceso asociados al área de trabajo, administrar los modelos e imágenes de Docker e incluso implementar servicios web.

## <a name="how-do-i-migrate"></a>¿Cómo realizo una migración?

La mayoría de los artefactos creados en la versión anterior de Azure Machine Learning Service se almacenan en su propio almacenamiento local o en la nube. Estos artefactos nunca desaparecerán. Para migrar, debe volver a registrar los artefactos con el servicio de Azure Machine Learning actualizado. Obtenga información sobre lo que puede migrar y cómo hacerlo en este [artículo de migración](how-to-migrate.md).

<a name="timeline"></a>

## <a name="support-timeline"></a>Escala de tiempo del soporte técnico

Puede seguir usando sus cuentas de Experimentación y Administración de modelos de Machine Learning y la aplicación Machine Learning Workbench después de septiembre de 2018. El soporte técnico para los siguientes recursos se retirará progresivamente en los tres o cuatro meses posteriores a ese lanzamiento. De todas formas, la documentación de las características antiguas sigue disponible en la [sección Recursos](../desktop-workbench/tutorial-classifying-iris-part-1.md) al final de la tabla de contenido.

|Fase de retirada|Detalles del soporte técnico para características anteriores|
|:---:|----------------|
|4 de diciembre de 2018|Termina la capacidad para crear cuentas de Experimentación y Administración de modelos de Azure Machine Learning en Azure Portal y desde la CLI. También termina la capacidad para crear entornos de proceso de Machine Learning desde la CLI. Si ya tiene una cuenta, la CLI y la aplicación de escritorio Machine Learning Workbench continúan funcionando en esta fase.|
|9 de enero de 2019|El soporte para todo lo demás finaliza en esta fecha. Algunos ejemplos son las API restantes y la aplicación de escritorio Machine Learning Workbench.|

[Inicia la migración](how-to-migrate.md) hoy mismo. Todas las funcionalidades más recientes están disponibles si se usa el nuevo <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, la [CLI](reference-azure-machine-learning-cli.md) y el [portal](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>¿Qué sucede con los historiales de ejecución?

Los historiales de ejecución serán accesibles durante un tiempo. Cuando esté listo para pasar a la versión actualizada de Azure Machine Learning Service, puede exportar estos historiales de ejecución si quiere conservar una copia.

Los historiales de ejecución se denominan **experimentos** en la versión actual. Puede recopilar los experimentos de su modelo y explorarlos mediante el SDK, la CLI o Azure Portal.

El panel del área de trabajo del portal solo es compatible con los exploradores Microsoft Edge, Chrome y Firefox:

[ ![Portal en línea](./media/overview-what-happened-to-workbench/image001.png)] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>¿Aún se puede preparar datos?

Los archivos de preparación de datos ya existentes no se pueden portar a la versión más reciente, puesto que ya no tenemos Machine Learning Workbench. Pero todavía puede preparar los datos para el modelado.  

Con conjuntos de datos cualquier tamaño puede usar el [SDK de preparación de datos de Azure Machine Learning](http://aka.ms/data-prep-sdk) para preparar rápidamente los datos antes del modelado mediante la escritura de código Python. 

Puede seguir [este tutorial](tutorial-data-prep.md) para obtener más información sobre cómo usar el SDK de preparación de datos de Azure Machine Learning.

## <a name="will-projects-persist"></a>¿Se conservarán los proyectos?

No perderá ningún código ni trabajo. En la versión anterior, los proyectos son entidades en la nube con un directorio local. En la versión más reciente, los directorios locales se asocian al área de trabajo de Azure Machine Learning Service mediante un archivo de configuración local. Consulte un [diagrama de la arquitectura más reciente](concept-azure-machine-learning-architecture.md).

Gran parte del contenido del proyecto ya estaba en la máquina local, por lo tanto, solo necesita crear un archivo de configuración en ese directorio y hacer referencia a él en el código para conectarse al área de trabajo. Obtenga información sobre cómo [migrar los proyectos existentes](how-to-migrate.md#projects).

Obtenga información sobre cómo empezar a trabajar [en Python con el SDK principal](quickstart-create-workspace-with-python.md) o con [Azure Portal](quickstart-get-started.md).

## <a name="what-about-my-registered-models-and-images"></a>¿Qué sucede con mis imágenes y modelos registrados?
 
Debe migrar los modelos que ha registrado en el registro del modelo antiguo a la nueva área de trabajo si quiere seguir usándolos. Para migrar los modelos, [descárguelos y vuelva a registrarlos](how-to-migrate.md) en el área de trabajo nueva. 

Las imágenes que creó en el registro de la imagen anterior se deben volver a crear en el área de trabajo nueva para seguir usándolas. Para volver a crear estas imágenes, siga las instrucciones que aparecen en las secciones sobre la [configuración y la creación de imágenes](how-to-deploy-and-where.md#configureimage). 

## <a name="what-about-deployed-web-services"></a>¿Qué sucede con los servicios web implementados?

Los modelos que implementó como servicios web con su cuenta de Administración de modelos de Machine Learning funcionarán siempre que Azure Container Service sea compatible. Esos servicios web funcionarán incluso después de que finalice el soporte técnico para las cuentas de Administración de modelos de Machine Learning. Pero cuando finalice el soporte técnico para las CLI anteriores, también lo hará la capacidad de administrar dichos servicios web.

En la versión más reciente, los modelos se implementan como servicios web para clústeres de Azure Container Instances o Azure Kubernetes Service (AKS). También puede implementar en FPGA y en Azure IoT Edge. Para más información, consulte el artículo sobre [cómo y dónde llevar a cabo la implementación](how-to-deploy-and-where.md). Sin tener que cambiar ninguno de los archivos, dependencias y esquemas de puntuación, puede volver a implementar los modelos mediante el nuevo SDK o la CLI. 

## <a name="what-about-the-old-sdk-and-cli"></a>¿Qué sucede con el SDK y la CLI antiguos?

Seguirán funcionando hasta enero. Consulte la [escala de tiempo](#timeline) anterior. Es recomendable que comience a crear nuevos experimentos y modelos con el SDK o la CLI más reciente.

Con el nuevo SDK de Python de la versión más reciente, puede interactuar con Azure Machine Learning Service en cualquier entorno de Python. Más información sobre cómo instalar el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> más reciente. También puede usar la [extensión de la CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md) actualizada con el amplio conjunto de comandos `az ml` para interactuar con el servicio en cualquier entorno de línea de comandos, incluido Azure Cloud Shell.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>Más información sobre Azure Machine Learning para Visual Studio Code

En esta versión más reciente, se ha ampliado y mejorado Azure Machine Learning para Visual Studio Code para trabajar con las nuevas características mencionadas.

[ ![Azure Machine Learning para Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>¿Qué sucede con los paquetes de dominio?

Los paquetes de dominio para [Computer Vision, Text Analytics y Forecasting](../desktop-workbench/reference-python-package-overview.md) no se pueden usar con la versión más reciente de Azure Machine Learning. Sin embargo, todavía puede crear y entrenar modelos de Computer Vision, Text y Forecasting con el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> más reciente de Python de Azure Machine Learning. Para más información sobre cómo migrar los modelos ya existentes creados con los paquetes Computer Vision, Text Analytics y Forecasting, póngase en contacto con [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la [arquitectura más reciente para Azure Machine Learning Service](concept-azure-machine-learning-architecture.md). Pruebe una de las guías de inicio rápido o tutoriales:

* [¿Qué es Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Inicio rápido: Creación de un área de trabajo con Python](quickstart-get-started.md)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
