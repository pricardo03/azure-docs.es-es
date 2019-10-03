---
title: Preguntas más frecuentes sobre Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/25/2019
ms.topic: conceptual
description: Encuentre respuestas a algunas de las preguntas comunes sobre Azure Dev Spaces.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: de44bf323b300a4467dc8b30555798f557898494
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71305915"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Preguntas más frecuentes sobre Azure Dev Spaces

Aborda las preguntas más frecuentes sobre Azure Dev Spaces.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>¿Qué regiones de Azure proporcionan actualmente Azure Dev Spaces?

Para obtener una lista completa de las regiones disponibles, consulte [Configuraciones y regiones admitidas][supported-regions].

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>¿Puedo usar Azure Dev Spaces sin una dirección IP pública?

No, no se puede aprovisionar Azure Dev Spaces en un clúster de AKS sin una dirección IP pública. Una dirección IP pública es [necesaria para el enrutamiento en Azure Dev Spaces][dev-spaces-routing].

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>¿Puedo usar mi propia entrada con Azure Dev Spaces?

Sí, puede configurar su propia entrada junto con la que Azure Dev Spaces crea. Por ejemplo, puede usar [traefik][ingress-traefik].

## <a name="can-i-use-https-with-azure-dev-spaces"></a>¿Puedo usar HTTPS con Azure Dev Spaces?

Sí, puede configurar su propia entrada con HTTPS mediante [traefik][ingress-https-traefik].

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>¿Puedo usar Azure Dev Spaces en un clúster que use CNI en lugar de kubenet? 

Sí, puede usar Azure Dev Spaces en un clúster de AKS que use CNI para redes. Por ejemplo, puede usar Azure Dev Spaces en un clúster de AKS con [contenedores de Windows existentes][windows-containers] que use CNI para redes.

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>¿Puedo usar Azure Dev Spaces con contenedores de Windows?

Actualmente, Azure Dev Spaces está diseñado para ejecutarse solo en nodos y pods de Linux, pero puede ejecutar Azure Dev Spaces en un clúster de AKS con [contenedores de Windows existentes][windows-containers].


[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: about.md#supported-regions-and-configurations
[windows-containers]: how-to/run-dev-spaces-windows-containers.md