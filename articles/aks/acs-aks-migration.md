---
title: Migrar Azure Container Service (ACS) a Azure Kubernetes Service (AKS)
description: Migre Azure Container Service (ACS) a Azure Kubernetes Service (AKS).
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: dcee8da943603fb0978caf9992be76347ca197d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65977715"
---
# <a name="migrate-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migrar Azure Container Service (ACS) a Azure Kubernetes Service (AKS)

El artículo le ayuda a planear y ejecutar una migración correcta entre Azure Container Service (ACS) con Kubernetes y Azure Kubernetes Service (AKS). Para ayudarle a tomar las decisiones clave, esta guía detalla las diferencias entre ACS y AKS, así como proporciona información general sobre el proceso de migración.

## <a name="differences-between-acs-and-aks"></a>Diferencias entre ACS y AKS

ACS y AKS difieren en algunas áreas clave que afectan a la migración. Antes de cualquier migración, debe revisar y planificar cómo abordar las siguientes diferencias:

* Los nodos de AKS utilizan [discos administrados](../virtual-machines/windows/managed-disks-overview.md).
    * Los discos no administrados se deben convertir para poder conectarlos a los nodos de AKS.
    * Los objetos `StorageClass` personalizados para discos de Azure deben que cambiarse de `unmanaged` a `managed`.
    * Cualquier elemento `PersistentVolumes` debe usar `kind: Managed`.
* AKS admite [varios grupos de nodos](https://docs.microsoft.com/azure/aks/use-multiple-node-pools) (actualmente en versión preliminar).
* Los nodos basados en Windows Server están actualmente [en versión preliminar en AKS](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* AKS admite un conjunto limitado de [regiones](https://docs.microsoft.com/azure/aks/quotas-skus-regions).
* AKS es un servicio administrado con un plano de control hospedado de Kubernetes. Es posible que tenga que modificar las aplicaciones si previamente ha modificado la configuración de los maestros de ACS.

## <a name="differences-between-kubernetes-versions"></a>Diferencias entre versiones de Kubernetes

Si está migrando a una versión más reciente de Kubernetes (por ejemplo, de 1.7 a 1.9.x), revise los siguientes recursos para comprender algunos cambios en la API de Kubernetes:

* [Migrate a ThirdPartyResource to CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/) (Migrar ThirdPartyResource a CustomResourceDefinition)
* [Workloads API changes in versions 1.8 and 1.9](https://kubernetes.io/docs/reference/workloads-18-19/) (Cambios de la API de cargas de trabajo en las versiones 1.8 y 1.9)

## <a name="migration-considerations"></a>Consideraciones sobre la migración

### <a name="agent-pools"></a>Grupos de agentes

Aunque AKS administra el plano de control de Kubernetes, usted define el tamaño y el número de nodos que quiere incluir en su nuevo clúster. Se supone que quiere una asignación 1:1 de ACS a AKS, por lo que querrá capturar la información de nodo de ACS existente. Use estos datos al crear el clúster de AKS.

Ejemplo:

| NOMBRE | Recuento | Tamaño de VM | Sistema operativo |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Dado que las máquinas virtuales adicionales se implementarán en su suscripción durante la migración, debe comprobar que los límites y cuotas son suficientes para estos recursos. 

Para más información, consulte [Azure subscription and service limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) (Límites de suscripción y servicio de Azure). Para comprobar las cuotas actuales, en Azure Portal, vaya a la [hoja de suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), seleccione la suscripción y seleccione **Uso y cuotas**.

### <a name="networking"></a>Redes

En aplicaciones complejas se suele migrar a lo largo del tiempo y no de una vez. Es decir, puede que los entornos nuevos y antiguos tengan que comunicarse a través de la red. Es posible que aplicaciones que anteriormente usaban servicios `ClusterIP` para comunicarse deban exponerse como tipo `LoadBalancer` y estar protegidas adecuadamente.

Para completar la migración, querrá dirigir los clientes a los nuevos servicios que se ejecutan en AKS. Recomendamos redirigir el tráfico mediante la actualización de DNS para que apunte a la instancia de Load Balancer que se encuentra frente al clúster de AKS.

### <a name="stateless-applications"></a>Aplicaciones sin estado

La migración de aplicaciones sin estado es el caso más sencillo. Aplicará las definiciones de YAML al nuevo clúster, se asegurará de que todo funciona según lo previsto y redirigirá el tráfico para activar el nuevo clúster.

### <a name="stateful-applications"></a>Aplicaciones con estado

Planee meticulosamente la migración de aplicaciones con estado para evitar la pérdida de datos o tiempos de inactividad inesperados.

#### <a name="highly-available-applications"></a>Aplicaciones de alta disponibilidad

Puede implementar algunas aplicaciones con estado en una configuración de alta disponibilidad. Estas aplicaciones pueden copiar datos entre las réplicas. Si actualmente usa este tipo de implementación, puede crear a un nuevo miembro en el nuevo clúster de AKS y, a continuación, migrar con un efecto mínimo en los llamadores indirectos. Los pasos de migración para este escenario suelen ser:

1. Creación de una réplica secundaria en AKS
2. Espera hasta que los datos se repliquen
3. Realización de una conmutación por error que convierta la réplica secundaria en la nueva principal
4. Desvío del tráfico al clúster de AKS

#### <a name="migrating-persistent-volumes"></a>Migración de volúmenes persistentes

Si migra volúmenes persistentes existentes a AKS, generalmente seguirá estos pasos:

1. Desactivar las escrituras en la aplicación. (Este paso es opcional y requiere tiempo de inactividad).
2. Tomar instantáneas de los discos.
3. Crear discos administrados nuevos a partir de las instantáneas.
4. Crear volúmenes persistentes en AKS.
5. Actualizar las especificaciones de pod para [usar volúmenes existentes](https://docs.microsoft.com/azure/aks/azure-disk-volume) en lugar de PersistentVolumeClaims (aprovisionamiento estático).
6. Implementar la aplicación en Azure.
7. Validar.
8. Desviar el tráfico al clúster de AKS.

> [!IMPORTANT]
> Si decide no desactivar las escrituras, deberá replicar los datos en la nueva implementación. En caso contrario, perderá los datos que se escriban después de tomar las instantáneas de disco.

Algunas herramientas de código abierto pueden ayudarle a crear discos administrados y a migrar volúmenes entre clústeres de Kubernetes:

* La [extensión de copia de disco de la CLI de Azure](https://github.com/noelbundick/azure-cli-disk-copy-extension) copia y convierte los discos entre grupos de recursos y regiones de Azure.
* La [extensión de la CLI de Azure Kube](https://github.com/yaron2/azure-kube-cli) enumera los volúmenes de ACS Kubernetes y los migra a un clúster de AKS.

#### <a name="azure-files"></a>Archivos de Azure

A diferencia de los discos, Azure Files puede montarse simultáneamente en varios hosts. En el clúster de AKS, Azure y Kubernetes no le impiden que cree un pod que el clúster de ACS siga usando. Para evitar la pérdida de datos y un comportamiento inesperado, asegúrese de que ambos clústeres no escriben en los mismos archivos al mismo tiempo.

Si la aplicación puede hospedar varias réplicas que apunten al mismo recurso compartido de archivos, siga los pasos de la migración sin estado e implemente las definiciones de YAML en el nuevo clúster. Si no es así, un posible enfoque de migración implica los pasos siguientes:

1. Implementar la aplicación en AKS con un número de réplicas de 0.
2. Escalar la aplicación en ACS hasta 0. (En este paso es necesario un tiempo de inactividad).
3. Escalar la aplicación en AKS hasta 1.
4. Validar.
5. Desviar el tráfico al clúster de AKS.

Si quiere comenzar con un recurso compartido vacío y luego realizar una copia de los datos de origen, puede usar los comandos [`az storage file copy`](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) para migrar los datos.

### <a name="deployment-strategy"></a>Estrategia de implementación

Recomendamos usar la canalización de CI/CD existente para implementar una configuración válida conocida en AKS. Clone las tareas de implementación existentes y asegúrese de que `kubeconfig` apunta al nuevo clúster de AKS.

Si esto no es posible, exporte las definiciones de recursos de ACS y luego aplíquelas a AKS. Puede usar `kubectl` para exportar objetos.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Varias herramientas de código abierto pueden ayudarle, según sus necesidades de implementación:

* [Velero](https://github.com/heptio/ark) (esta herramienta requiere Kubernetes 1.7).
* [Extensión de la CLI de Azure Kube](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Pasos de migración

1. [Cree un clúster de AKS](https://docs.microsoft.com/azure/aks/create-cluster) mediante Azure Portal, la CLI de Azure o una plantilla de Resource Manager.

   > [!NOTE]
   > Puede encontrar plantillas de Azure Resource Manager de ejemplo para AKS en el repositorio [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) de GitHub.

2. Realice los cambios necesarios en las definiciones de YAML. Por ejemplo, reemplace `apps/v1beta1` por `apps/v1` para `Deployments`.

3. [Migre volúmenes](#migrating-persistent-volumes) (opcional) del clúster de ACS al clúster de AKS.

4. Use el sistema de CI/CD para implementar aplicaciones en AKS. O bien, use kubectl para aplicar las definiciones de YAML.

5. Validar. Asegúrese de que las aplicaciones funcionan según lo previsto y que se han copiado los datos migrados.

6. Redireccione el tráfico. Actualice el DNS para dirigir a los clientes a la implementación de AKS.

7. Finalice los pasos posteriores a la migración. Si migró volúmenes y decidió no poner en modo inactivo las operaciones de escritura, copie los datos en el nuevo clúster.
