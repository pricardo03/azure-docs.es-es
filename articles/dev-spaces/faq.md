---
title: Preguntas más frecuentes sobre Azure Dev Spaces
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: conceptual
description: Encuentre respuestas a algunas de las preguntas comunes sobre Azure Dev Spaces.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: d5ab56edfe4799d51fb7f08642aad9e2ee01db05
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044972"
---
# <a name="frequently-asked-questions-about-azure-dev-spaces"></a>Preguntas más frecuentes sobre Azure Dev Spaces

Aborda las preguntas más frecuentes sobre Azure Dev Spaces.

## <a name="which-azure-regions-currently-provide-azure-dev-spaces"></a>¿Qué regiones de Azure proporcionan actualmente Azure Dev Spaces?

Consulte las [regiones admitidas][supported-regions] para obtener una lista completa de las regiones disponibles.

## <a name="can-i-use-azure-dev-spaces-without-a-public-ip-address"></a>¿Puedo usar Azure Dev Spaces sin una dirección IP pública?

No, no se puede aprovisionar Azure Dev Spaces en un clúster de AKS sin una dirección IP pública. Una dirección IP pública es [necesaria para el enrutamiento en Azure Dev Spaces][dev-spaces-routing].

## <a name="can-i-use-my-own-ingress-with-azure-dev-spaces"></a>¿Puedo usar mi propia entrada con Azure Dev Spaces?

Sí, puede configurar su propia entrada junto con la que Azure Dev Spaces crea. Por ejemplo, puede usar [traefik][ingress-traefik].

## <a name="can-i-use-https-with-azure-dev-spaces"></a>¿Puedo usar HTTPS con Azure Dev Spaces?

Sí, puede configurar su propia entrada con HTTPS mediante [traefik][ingress-https-traefik].

## <a name="can-i-use-azure-dev-spaces-on-a-cluster-that-uses-cni-rather-than-kubenet"></a>¿Puedo usar Azure Dev Spaces en un clúster que use CNI en lugar de kubenet? 

Sí, puede usar Azure Dev Spaces en un clúster de AKS que use CNI para redes. Por ejemplo, puede usar Azure Dev Spaces en un clúster de AKS con [contenedores de Windows existentes][windows-containers] que use CNI para redes. Puede encontrar más información sobre el uso de CNI para redes con Azure Dev Spaces [aquí](configure-networking.md#using-azure-cni).

## <a name="can-i-use-azure-dev-spaces-with-windows-containers"></a>¿Puedo usar Azure Dev Spaces con contenedores de Windows?

Actualmente, Azure Dev Spaces está diseñado para ejecutarse solo en nodos y pods de Linux, pero puede ejecutar Azure Dev Spaces en un clúster de AKS con [contenedores de Windows existentes][windows-containers].

## <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-api-server-authorized-ip-address-ranges-enabled"></a>¿Puedo usar Azure Dev Spaces en clústeres de AKS con intervalos de direcciones IP autorizados del servidor de API habilitado?

Sí, puede usar Azure Dev Spaces en clústeres de AKS con [intervalos de direcciones IP autorizados del servidor de API][aks-auth-range] habilitado. [Aquí](configure-networking.md#using-api-server-authorized-ip-ranges) puede encontrar más información sobre el uso de un clúster de AKS con intervalos de direcciones IP autorizadas del servidor de API habilitados con Azure Dev Spaces.

### <a name="can-i-use-azure-dev-spaces-on-aks-clusters-with-restricted-egress-traffic-for-cluster-nodes"></a>¿Puedo usar Azure Dev Spaces en clústeres de AKS con tráfico de salida restringido para los nodos de clúster?

Sí, puede usar Azure Dev Spaces en clústeres de AKS con el [tráfico de salida restringido para los nodos de clúster][aks-restrict-egress-traffic] habilitado una vez que se han permitido los FQDN correctos. [Aquí](configure-networking.md#ingress-and-egress-network-traffic-requirements) puede encontrar más información sobre el uso de un clúster de AKS con el tráfico de salida restringido para nodos de clúster habilitado con Azure Dev Spaces.

[aks-auth-range]: ../aks/api-server-authorized-ip-ranges.md
[aks-auth-range-create]: ../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled
[aks-auth-range-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[aks-auth-range-update]: ../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges
[aks-restrict-egress-traffic]: ../aks/limit-egress-traffic.md
[dev-spaces-routing]: how-dev-spaces-works.md#how-routing-works
[ingress-traefik]: how-to/ingress-https-traefik.md#configure-a-custom-traefik-ingress-controller
[ingress-https-traefik]: how-to/ingress-https-traefik.md#configure-the-traefik-ingress-controller-to-use-https
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[windows-containers]: how-to/run-dev-spaces-windows-containers.md