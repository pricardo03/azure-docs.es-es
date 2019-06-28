---
title: Restringir el tráfico de salida en Azure Kubernetes Service (AKS)
description: Obtenga información sobre qué puertos y direcciones son necesarios para controlar el tráfico de salida en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 06/06/2019
ms.author: iainfou
ms.openlocfilehash: 43ba7593336372bbbd7a3a4bb9821665a42bbf29
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752183"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Limitar el tráfico de salida en los nodos de clúster y controlar el acceso a los puertos y servicios necesarios en Azure Kubernetes Service (AKS), versión preliminar

De forma predeterminada, los clústeres de AKS tienen acceso de salida a Internet ilimitado. Este nivel de acceso a la red permite que los nodos y servicios que ejecuta accedan a recursos externos según sea necesario. Si desea restringir el tráfico de salida, es necesario el acceso a un número limitado de puertos y direcciones para mantener las tareas de mantenimiento del clúster en buen estado. El clúster se configura para que solo use las imágenes de contenedor del sistema base de Microsoft Container Registry (MCR) o Azure Container Registry (ACR), pero no los repositorios públicos externos. Debe configurar las reglas de seguridad y de firewall que prefiera para permitir estos puertos y direcciones necesarios.

En este artículo se detallan los puertos de red y los nombres de dominio completo (FQDN) obligatorios y opcionales al restringir el tráfico de salida de un clúster de AKS.  Esta funcionalidad actualmente está en su versión preliminar.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Se proporcionan para recopilar comentarios y errores de nuestra comunidad. En la versión preliminar, estas características no están diseñadas para su uso en producción. Las características en versión preliminar pública se incluyen en el soporte técnico de “mejor esfuerzo”. Los equipos de soporte técnico de AKS ofrecen asistencia solo durante el horario laboral en la zona horaria del Pacífico (PST). Para más información, vea los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para Azure Kubernetes Service][aks-support-policies]
> * [Preguntas más frecuentes sobre Azure Kubernetes Service][aks-faq]

## <a name="before-you-begin"></a>Antes de empezar

Es necesario tener instalada y configurada la versión 2.0.66 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

Para crear un clúster de AKS que pueda limitar el tráfico de salida, habilite primero una marca de características en su suscripción. Este registro de características configura cualquier clúster de AKS que cree para usar imágenes de contenedor del sistema base de MCR o ACR. Para registrar la marca de característica *AKSLockingDownEgressPreview*, use el comando [az feature register][az-feature-register] tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az feature register --name AKSLockingDownEgressPreview --namespace Microsoft.ContainerService
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSLockingDownEgressPreview')].{Name:name,State:properties.state}"
```

Cuando haya terminado, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="egress-traffic-overview"></a>Descripción del tráfico de salida

Para fines operativos y de administración, los nodos de un clúster de AKS deben tener acceso a determinados puertos y nombres de dominio completo (FQDN). Estas acciones podrían consistir en la comunicación con el servidor de la API o la descarga e instalación de actualizaciones de seguridad de nodos y componentes principales del clúster de Kubernetes. De forma predeterminada, el tráfico de Internet (saliente) de salida no está restringido a los nodos de un clúster de AKS. El clúster puede extraer imágenes de contenedor del sistema base de repositorios externos.

Para aumentar la seguridad de su clúster de AKS, le recomendamos que restrinja el tráfico de salida. El clúster está configurado para extraer imágenes de contenedor del sistema base de MCR o ACR. Si bloquea el tráfico de salida de esta forma, deberá definir puertos específicos y FQDN para permitir que los nodos de AKS se comuniquen correctamente con los servicios externos necesarios. Sin estos FQDN y puertos autorizados, los nodos de AKS no podrán comunicarse con el servidor de API ni instalar los componentes principales.

Puede usar [Azure Firewall][azure-firewall] o un dispositivo de firewall de terceros para proteger el tráfico de salida y definir estos puertos y direcciones obligatorios. AKS no crea automáticamente estas reglas para usted. Los siguientes puertos y direcciones sirven como referencia para crear las reglas correspondientes en el firewall de red.

En AKS, hay dos conjuntos de puertos y direcciones:

* Los [puertos y direcciones obligatorios para los clústeres de AKS](#required-ports-and-addresses-for-aks-clusters), que son los requisitos mínimos para el tráfico de salida autorizado.
* Los [puertos y direcciones recomendados opcionales para los clústeres de AKS](#optional-recommended-addresses-and-ports-for-aks-clusters), que no son obligatorios en todos los escenarios, pero implican que la integración con otros servicios como Azure Monitor no funcione correctamente. Consulte esta lista de FQDN y puertos opcionales y autorice cualquiera de los servicios y componentes que se usan en el clúster de AKS.

> [!NOTE]
> La limitación del tráfico de salida solo funciona en nuevos clústeres de AKS creados después de habilitar el registro de la marca de características. En los clústeres existentes, [realice una operación de actualización de clústeres][aks-upgrade] utilizando el comando `az aks upgrade` antes de limitar el tráfico de salida.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Puertos y direcciones obligatorios para los clústeres de AKS

Un clúster de AKS requiere los siguientes puertos de salida / reglas de red:

* Puerto TCP *443*
* Puerto TCP *9000* y puerto TCP *22* para el pod de la parte delantera del túnel para comunicarse con el extremo de túnel en el servidor de la API.
    * Para obtener información más concreta, vea las direcciones * *.hcp.\<ubicación\>.azmk8s.io* y * *.tun.\<ubicación\>.azmk8s.io* en la tabla siguiente.

Se requieren las siguientes reglas de aplicación / FQDN:

| FQDN                       | Port      | Uso      |
|----------------------------|-----------|----------|
| *.hcp.\<ubicación\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000 | Esta dirección es el punto de conexión del servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| *.tun.\<ubicación\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000 | Esta dirección es el punto de conexión del servidor de la API. Reemplace *\<ubicación\>* con la región en la que se implemente su clúster de AKS. |
| aksrepos.azurecr.io        | HTTPS:443 | Esta dirección es necesaria para acceder a las imágenes de Azure Container Registry (ACR). |
| \* .blob.core.windows.net    | HTTPS:443 | Esta dirección es el almacén de back-end para las imágenes almacenadas en ACR. |
| mcr.microsoft.com          | HTTPS:443 | Esta dirección es necesaria para acceder a las imágenes de Microsoft Container Registry (MCR). |
| *.cdn.mscr.io              | HTTPS:443 | Esta dirección es necesaria para el almacenamiento de MCR respaldado por la red de entrega de contenido (CDN) de Azure. |
| management.azure.com       | HTTPS:443 | Esta dirección es necesaria para las operaciones de Kubernetes GET o PUT. |
| login.microsoftonline.com  | HTTPS:443 | Esta dirección es necesaria para la autenticación de Azure Active Directory. |
| api.snapcraft.io           | HTTPS:443, HTTP:80 | Esta dirección es necesaria para instalar paquetes de ajuste en nodos de Linux. |
| ntp.ubuntu.com             | UDP:123   | Esta dirección es necesaria para la sincronización de la hora NTP en nodos de Linux. |
| *.docker.io                | HTTPS:443 | Esta dirección es necesaria para extraer imágenes de contenedor necesarios para la parte delantera del túnel. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Direcciones y puertos recomendados opcionales para clústeres de AKS

* Puerto UDP *53* para DNS

Se recomiendan las siguientes reglas de aplicación / FQDN para que los clústeres de AKS funcionen correctamente:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| *.ubuntu.com                            | HTTP:80   | Esta dirección permite a los nodos del clúster de Linux descargar las revisiones de seguridad y actualizaciones necesarias. |
| packages.microsoft.com                  | HTTPS:443 | Esta dirección es el repositorio de paquetes de Microsoft que se usa para las operaciones *apt-get* almacenadas en caché. |
| dc.services.visualstudio.com            | HTTPS:443 | Se recomienda para la supervisión y las métricas correctas mediante Azure Monitor. |
| *.opinsights.azure.com                  | HTTPS:443 | Se recomienda para la supervisión y las métricas correctas mediante Azure Monitor. |
| *.monitoring.azure.com                  | HTTPS:443 | Se recomienda para la supervisión y las métricas correctas mediante Azure Monitor. |
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 | Esta dirección se utiliza para el correcto funcionamiento de Azure Policy (actualmente en versión preliminar en AKS). |
| apt.dockerproject.org                   | HTTPS:443 | Esta dirección se utiliza para la instalación y el funcionamiento correctos del controlador en nodos basados en GPU. |
| nvidia.github.io                        | HTTPS:443 | Esta dirección se utiliza para la instalación y el funcionamiento correctos del controlador en nodos basados en GPU. |

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido qué puertos y direcciones hay que permitir si restringe el tráfico de salida para el clúster. También puede definir cómo se pueden comunicar los pods y qué restricciones tienen en el clúster. Para más información, vea [Protección del tráfico entre pods mediante directivas de red en Azure Kubernetes Service (AKS)][network-policy].

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
