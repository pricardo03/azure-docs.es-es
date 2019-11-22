---
title: Introducción a Azure Dev Spaces
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 05/07/2019
ms.topic: overview
description: Introducción a Azure Dev Spaces
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, kubectl, k8s
manager: gwallace
ms.openlocfilehash: c87aee26f5f9a4c3f919150a22a8b2f5a7b57352
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73889747"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces

Azure Dev Spaces proporciona una experiencia de desarrollo de Kubernetes iterativa y rápida para los equipos de los clústeres de Azure Kubernetes Service (AKS). Azure Dev Spaces también permite depurar y probar todos los componentes de la aplicación en AKS con la configuración mínima del equipo de desarrollo, sin replicar ni simular las dependencias.

![](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Cómo simplifica Azure Dev Spaces el desarrollo de Kubernetes

Azure Dev Spaces ayuda a los equipos a centrarse en el desarrollo y la iteración rápida de su aplicación de microservicio, ya que les permite trabajar directamente con su arquitectura de microservicios completa o con la aplicación entera que se ejecuta en AKS. Azure Dev Spaces también proporciona una manera de actualizar de forma independiente partes de la arquitectura de microservicios de forma aislada sin afectar al resto del clúster de AKS o a otros desarrolladores. Azure Dev Spaces se usa para desarrollo y pruebas en entornos de desarrollo y pruebas de bajo nivel y no va dirigido a la ejecución en clústeres de AKS de producción.

Puesto que los equipos pueden trabajar con la aplicación entera y colaborar directamente en AKS, Azure Dev Spaces permite:

* Reducir la instalación en la máquina local
* Disminuir el tiempo de instalación para los nuevos desarrolladores del equipo
* Aumentar la velocidad de un equipo gracias a una iteración más rápida
* Reducir el número de entornos de desarrollo e integración redundantes puesto que los miembros del equipo pueden compartir un clúster
* Eliminar la necesidad de replicar o simular las dependencias
* Mejorar la colaboración entre los equipos de desarrollo y los equipos con los que trabajan, como pueden ser los equipos de DevOps

Azure Dev Spaces proporciona herramientas para generar recursos de Docker y Kubernetes para sus proyectos. Estas herramientas le permiten agregar fácilmente las aplicaciones nuevas y existentes a un espacio de desarrollo y a otros clústeres de AKS.

Para más información sobre cómo funciona Azure Dev Spaces, consulte [How Azure Dev Spaces works and is configured][how-dev-spaces-works] (Funcionamiento y configuración de Azure Dev Spaces).

## <a name="supported-regions-and-configurations"></a>Configuraciones y regiones admitidas

Azure Dev Spaces solo es compatible con los clústeres de AKS en las regiones del **Este de EE. UU.** , **Este de EE. UU. 2**, **Centro de EE. UU.** , **Centro-sur de EE. UU.** , **Oeste de EE. UU. 2**, **Norte de Europa**, **Oeste de Europa**, **Sur de Reino Unido**, **Asia Pacífico**, **Sudeste Asiático**, **Este de Australia**, **Centro de Canadá** y **Este de Canadá**. Azure Dev Spaces admite el uso de la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) o [Visual Studio Code](https://code.visualstudio.com/download) con la [extensión de Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) instalada en Linux, MacOS o Windows 8 (o alguna versión posterior) para compilar y ejecutar las aplicaciones en AKS. También admite el uso de [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) en Windows 8 o versiones posteriores. En Visual Studio 2019, necesitará la carga de trabajo Desarrollo de Azure. En Visual Studio 2017, necesitará la carga de trabajo de desarrollo web y [Visual Studio Tools para Kubernetes](https://aka.ms/get-vsk8stools).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el desarrollo rápido e iterativo en equipos con Azure Dev Spaces, consulte el inicio rápido de desarrollo de equipos.

> [!div class="nextstepaction"]
> [Inicio rápido de desarrollo de equipos](quickstart-team-development.md)


[how-dev-spaces-works]: how-dev-spaces-works.md
[team-development-quickstart]: quickstart-team-development.md
