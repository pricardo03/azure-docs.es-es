---
title: 'Registros de contenedores privados de Docker en Azure: información general'
description: Introducción al servicio Azure Container Registry, que proporciona registros de Docker basados en la nube, administrados y privados.
services: container-registry
author: stevelas
ms.service: container-registry
ms.topic: overview
ms.date: 04/03/2019
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: ce870bfb8d29f7a808962e4d273388ab31186f10
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997417"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Introducción a los registros de contenedores privados de Docker en Azure

Azure Container Registry es un servicio administrado de [Docker Registry](https://docs.docker.com/registry/) basado en Docker Registry 2.0 de código abierto. Cree y mantenga los registros de contenedor de Azure para almacenar y administrar las imágenes privadas del [contenedor Docker](https://www.docker.com/what-docker).

Use registros de contenedor en Azure con el desarrollo de contenedor y las canalizaciones de implementación existentes o use [ACR Tasks](#azure-container-registry-tasks) para compilar imágenes de contenedor en Azure. Compile bajo demanda o automatice completamente sus compilaciones gracias al código fuente y los desencadenadores de la compilación para la actualización de la imagen de base.

Para más información sobre Docker y los contenedores, consulte la [información general de Docker](https://docs.docker.com/engine/docker-overview/).

## <a name="use-cases"></a>Casos de uso

Extraiga imágenes desde un registro de contenedor de Azure a varios destinos de implementación:

* **Sistemas escalables de orquestación** que administran aplicaciones en contenedores a través de clústeres de hosts, incluidos [Kubernetes](https://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/), y [Docker Swarm](https://docs.docker.com/swarm/).
* Los **Servicios de Azure** que admiten la compilación y ejecución de aplicaciones a escala, entre los que se incluyen [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) y otros más.

Los desarrolladores también pueden insertar en un registro de contenedor como parte de un flujo de trabajo de desarrollo de contenedor. Por ejemplo, puede dirigir un registro de contenedor desde una herramienta de implementación e integración continua como [Azure DevOps Services](https://docs.microsoft.com/azure/devops/) o [Jenkins](https://jenkins.io/).

Configure ACR Tasks para que recompile automáticamente imágenes de aplicaciones cuando se actualicen sus imágenes base o automatice las compilaciones de imágenes cuando el equipo guarde el código en un repositorio de Git. Cree tareas de varios pasos para automatizar la compilación, prueba y aplicación de revisiones de varias imágenes de contenedor en paralelo en la nube.

Azure proporciona varias herramientas, entre las que se incluyen la Interfaz de la línea de comandos de Azure, Azure Portal y soporte de API para administrar los registros de contenedor de Azure. Opcionalmente, instale la [extensión de Docker para Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) y la extensión de la [cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) para trabajar con los registros de contenedor de Azure. Extraiga e inserte imágenes en un registro de contenedor de Azure o ejecute ACR Tasks y, todo ello, en Visual Studio Code.

## <a name="key-concepts"></a>Conceptos clave

* **Registro**: cree un registro de contenedor o varios en la suscripción de Azure. Los registros están disponibles en tres SKU: [Basic, Standard y Premium](container-registry-skus.md), cada una de las cuales admite la integración de webhook, la autenticación del registro con Azure Active Directory y la funcionalidad de eliminación. Aproveche las ventajas del almacenamiento local, cercano a la red, de las imágenes de contenedor mediante la creación de un registro en la misma ubicación de Azure que las implementaciones. Use la característica de [replicación geográfica](container-registry-geo-replication.md) de los registros Premium en escenarios avanzados de replicación y de distribución de imágenes de contenedor. Un nombre completo de registro tiene el formato `myregistry.azurecr.io`.

  Puede [controlar el acceso](container-registry-authentication.md) a un registro de contenedor mediante una identidad de Azure, una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) respaldada por Azure Active Directory o una cuenta de administrador proporcionada. Inicie sesión en el registro mediante la CLI de Azure o el comando `docker login` estándar.

* **Repositorio**: un registro contiene uno o varios repositorios, que son grupos virtuales de imágenes de contenedor con el mismo nombre pero diferentes etiquetas o códigos hash. Azure Container Registry es compatible con los espacios de nombres del repositorio de varios niveles. Con espacios de nombres de varios niveles puede agrupar colecciones de imágenes relacionadas con una aplicación específica o una colección de aplicaciones con equipos operativos o de desarrollo específicos. Por ejemplo: 

  * `myregistry.azurecr.io/aspnetcore:1.0.1` representa una imagen de toda la organización
  * `myregistry.azurecr.io/warrantydept/dotnet-build` representa una imagen que se utiliza para compilar aplicaciones .NET y compartirlas en el departamento de garantías
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` representa una imagen web, agrupada en la aplicación de envíos al cliente, propiedad del departamento de garantías.

* **Imagen**: se almacena en un repositorio, cada imagen es una instantánea de solo lectura de un contenedor compatible con Docker. Los registros de contenedor de Azure pueden incluir imágenes de Windows y de Linux. Controle los nombres de imagen de todas las implementaciones de contenedor. Use [comandos de Docker](https://docs.docker.com/engine/reference/commandline/) estándar para insertar imágenes en un repositorio o extraer una imagen de un repositorio. Además de las imágenes de contenedor de Docker, Azure Container Registry almacena los [formatos de contenido relacionados](container-registry-image-formats.md), como los [gráficos de Helm](container-registry-helm-repos.md) y las imágenes creadas para la [especificación de formato de imagen de Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md).

* **Contenedor**: un contenedor define una aplicación de software y las dependencias ajustadas en un sistema de archivos completo que incluye el código, el tiempo de ejecución, las herramientas del sistema y las bibliotecas. Ejecute contenedores Docker basados en imágenes de Windows o Linux que extrae de un registro de contenedor. Los contenedores que se ejecutan en una sola máquina comparten el kernel del sistema operativo. Los contenedores de Docker son totalmente portátiles a las principales distribuciones de Linux, macOS y Windows.

## <a name="azure-container-registry-tasks"></a>Tareas de Azure Container Registry

[Azure Container Registry Tasks](container-registry-tasks-overview.md) (ACR Tasks) es un conjunto de características de Azure Container Registry que proporcionan compilaciones de imágenes de contenedor de Docker optimizadas y eficientes en Azure. Utilice ACR Tasks para extender el bucle interno de desarrollo a la nube descargando operaciones de `docker build` en Azure. Configure las tareas de compilación para automatizar el sistema operativo del contenedor y la canalización de aplicaciones de revisión de marcos, y compile imágenes de forma automática cuando el equipo guarde el código en el control de origen.

Las [tareas con varios pasos](container-registry-tasks-overview.md#multi-step-tasks) proporcionan definición y ejecución de tareas basadas en pasos para compilar y probar imágenes de contenedor en la nube, y aplicarles revisiones. Los pasos de las tareas definen las operaciones de creación e inserción de imágenes de contenedor individuales. También pueden definir la ejecución de uno o más contenedores, donde cada paso utiliza el contenedor como su entorno de ejecución.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un registro de contenedor mediante Azure Portal](container-registry-get-started-portal.md)
* [Creación de un registro de contenedor con la CLI de Azure](container-registry-get-started-azure-cli.md)
* [Automatización de la aplicación de revisiones de sistema operativo y marco con ACR Tasks](container-registry-tasks-overview.md)
