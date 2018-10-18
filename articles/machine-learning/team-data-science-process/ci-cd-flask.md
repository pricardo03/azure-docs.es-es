---
title: 'DevOps para aplicaciones de inteligencia artificial (IA): creación de canalización de integración continua en Azure mediante una aplicación de Docker, Kubernetes y Python Flask'
description: 'DevOps para aplicaciones de inteligencia artificial (IA): creación de canalización de integración continua en Azure mediante Docker y Kubernetes'
services: machine-learning, team-data-science-process
documentationcenter: ''
author: jainr
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jainr
ms.openlocfilehash: b0368e742c990feed626a1c4982bfedc35785b49
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304295"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>DevOps para aplicaciones de inteligencia artificial (IA): creación de canalización de integración continua en Azure mediante Docker y Kubernetes
Para una aplicación de IA, suele haber dos flujos de trabajo: los científicos de datos, que crean modelos de aprendizaje automático, y los desarrolladores de aplicaciones, que crean la aplicación y la exponen a los usuarios finales para que la consuman. En este artículo, se muestra cómo implementar una canalización de integración continua (CI) y entrega continua (CD) para una aplicación de IA. Una aplicación de IA es una combinación de código de aplicación insertado con un modelo de aprendizaje automático (ML) previamente entrenado. Para este artículo, obtenemos un modelo previamente entrenado de una cuenta de almacenamiento de blobs de Azure privada; también podría ser una cuenta de AWS S3. Usaremos una aplicación web sencilla de Python Flask para el artículo.

> [!NOTE]
> Esta es una de las diferentes maneras de llevar a cabo CI/CD. Hay alternativas a las herramientas y otros requisitos previos que se mencionan más adelante. A medida que desarrollamos contenido adicional, los iremos publicando.
>
>

## <a name="github-repository-with-document-and-code"></a>Repositorio de GitHub con el documento y el código
Puede descargar el código fuente de [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). También se encuentra disponible un [tutorial detallado](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md).

## <a name="pre-requisites"></a>Requisitos previos
Estos son los requisitos previos para seguir la canalización de CI/CD que se describe a continuación:
* [Organización de Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
* [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Clúster de Azure Container Service (AKS) donde se ejecuta Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Cuenta de Azure Container Registy (ACR)](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)
* [Instale Kubectl para ejecutar comandos en clústeres de Kubernetes.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) Necesitaremos esta opción para obtener la configuración del clúster de ACS. 
* Bifurque el repositorio de GitHub a su cuenta de GitHub.

## <a name="description-of-the-cicd-pipeline"></a>Descripción de la canalización de CI/CD
La canalización se inicia para cada nueva confirmación; se ejecuta el conjunto de pruebas; si la prueba se supera, toma la compilación más reciente y la empaqueta en un contenedor de Docker. Después, el contenedor se implementa mediante Azure Container Service (ACS) y las imágenes se almacenan de forma segura en Azure Container Registry (ACR). ACS ejecuta Kubernetes para la administración de clúster de contenedor, pero puede elegir Docker Swarm o Mesos.

La aplicación extrae el modelo más reciente de forma segura desde una cuenta de Azure Storage y lo empaqueta como parte de la aplicación. La aplicación implementada tiene el código de aplicación y el modelo ML empaquetados como un solo contenedor. Esto desacopla los desarrolladores de aplicación y los científicos de datos, para asegurarse de que su aplicación de producción siempre ejecuta el código más reciente con el modelo ML más reciente.

La arquitectura de canalización se indica a continuación. 

![Arquitectura](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>Pasos de la canalización de CI/CD
1. El programador trabaja en el IDE que prefiere en el código de aplicación.
2. Confirman el código en el control de código fuente que eligen (Azure DevOps tiene buena compatibilidad con diversos controles de código fuente).
3. Por separado, los científicos de datos trabajan en el desarrollo de su modelo.
4. Una vez satisfechos, publican el modelo en un repositorio de modelos; en este caso, usamos una cuenta de almacenamiento de blobs. Esto se puede reemplazar fácilmente con el servicio de administración de modelos de Azure ML Workbench a través de sus API REST.
5. Se inicia una compilación en Azure DevOps según la confirmación en GitHub.
6. La canalización de compilación de Azure DevOps extrae el último modelo del contenedor de blobs y crea un contenedor.
7. Azure DevOps inserta la imagen en el repositorio de imágenes privado en Azure Container Registry.
8. En una programación establecida (por la noche), se inicia la canalización de versión.
9. La imagen más reciente de ACR se extrae y se implementa en el clúster de Kubernetes en ACS.
10. La solicitud de los usuarios para la aplicación va a través del servidor DNS.
11. El servidor DNS pasa la solicitud al equilibrador de carga y devuelve la respuesta al usuario.

## <a name="next-steps"></a>Pasos siguientes
* Consulte el [tutorial](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) para seguir los detalles e implemente su propia canalización de CI/CD para su aplicación.

## <a name="references"></a>Referencias
* [Proceso de ciencia de datos en equipo (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/azure/machine-learning/service/)
* [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/vso/)
* [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)
