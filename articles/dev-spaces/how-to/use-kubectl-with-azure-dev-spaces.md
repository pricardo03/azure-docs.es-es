---
title: Uso de kubectl con Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: article
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: b0ceccc4f8b16c21e8a66a5f1b8cf0e7b6f47a4f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469318"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Uso de kubectl con una instancia de Azure Dev Spaces

Puede tener acceso al clúster de Kubernetes dentro de un espacio de Azure Dev Space y utilizar herramientas de Kubernetes existentes, como `kubectl`.

La ejecución de `az aks use-dev-spaces` agregará automáticamente un contexto de configuración de `kubectl`, por lo que kubectl ya debe estar conectado a su clúster de Kubernetes en Azure Dev Spaces. Ejemplos:
- Confirme el contexto actual: `kubectl config current-context`
- Enumere todos los contextos disponibles: `kubectl config get-contexts`. 
- Cambie el contexto: `kubectl config use-context <context-name>`
- Vea el panel de Kubernetes: ejecute `kubectl proxy` y luego abra el explorador en la dirección que emite este comando (adjunte `/ui` a la dirección URL para navegar al panel de Kubernetes).
- Enumere los servicios en ejecución en el espacio predeterminado de Azure Dev Spaces denominado *default*: `kubectl get services --namespace=default`

