---
title: Formatos de contenido de Azure Container Registry
description: Obtenga más información sobre los formatos de contenido admitidos en Azure Container Registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 04/18/2019
ms.author: danlep
ms.openlocfilehash: fe129847e685c7151a9b7ad7ea65abbd38530733
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60827466"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Formatos de contenido admitidos en Azure Container Registry

Use un repositorio privado en Azure Container Registry para administrar uno de los siguientes formatos de contenido. 

## <a name="docker-compatible-container-images"></a>Imágenes de contenedor compatibles con Docker

Se admiten los siguientes formatos de imagen de contenedor de Docker:

* [Manifiesto de imagen de Docker V2, esquema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Manifiesto de imagen de Docker V2, esquema 2](https://docs.docker.com/registry/spec/manifest-v2-2/): incluye listas de manifiesto que permiten que los registros almacenen imágenes multiplataforma en una referencia "image: tag" única.

## <a name="oci-images"></a>Imágenes OCI

Azure Container Registry admite también las imágenes que cumplen el [especificación de formato de imagen de capa de sockets seguros (OCI, Open Container Initiative)](https://github.com/opencontainers/image-spec/blob/master/spec.md). Los formatos de empaquetado incluyen [singularidad en la imagen de formato (SIF)](https://www.sylabs.io/2018/03/sif-containing-your-containers/).

## <a name="helm-charts"></a>Gráficos de Helm

Azure Container Registry puede hospedar los repositorios para [gráficos de Helm](https://helm.sh/), un formato de empaquetado que se usa para administrar e implementar rápidamente aplicaciones de Kubernetes. Se admite la versión 2.11.0 o posterior del [cliente de Helm](https://docs.helm.sh/using_helm/#installing-helm).

## <a name="next-steps"></a>Pasos siguientes

* Vea cómo [insertar y extraer](container-registry-get-started-docker-cli.md) imágenes con Azure Container Registry.

* Use [ACR Tasks](container-registry-tasks-overview.md) para compilar y probar imágenes de contenedor. 

* Use [Moby BuildKit](https://github.com/moby/buildkit) para compilar y empaquetar contenedores en formato OCI.

* Configure un [repositorio de Helm](container-registry-helm-repos.md) hospedado en Azure Container Registry. 


