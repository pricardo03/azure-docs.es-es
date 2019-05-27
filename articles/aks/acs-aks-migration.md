---
title: Migrar desde Azure Container Service (ACS) a Azure Kubernetes Service (AKS)
description: Migrar desde Azure Container Service (ACS) a Azure Kubernetes Service (AKS).
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: dcee8da943603fb0978caf9992be76347ca197d6
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65977715"
---
# <a name="migrate-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migrar desde Azure Container Service (ACS) a Azure Kubernetes Service (AKS)

En este artículo le ayuda a planear y ejecutar una migración correcta entre Azure Container Service (ACS) con Kubernetes y Azure Kubernetes Service (AKS). Para ayudarle a tomar decisiones claves, esta guía detalla las diferencias entre ACS y AKS y proporciona información general sobre el proceso de migración.

## <a name="differences-between-acs-and-aks"></a>Diferencias entre ACS y AKS

ACS y AKS difieren en algunas áreas claves que afectan a la migración. Antes de cualquier migración, debe revisar y planear abordar las siguientes diferencias:

* Utilizan los nodos de AKS [discos administrados](../virtual-machines/windows/managed-disks-overview.md).
    * Discos no administrados se deben convertir antes de poder conectar a los nodos AKS.
    * Custom `StorageClass` objetos para los discos de Azure deben cambiarse de `unmanaged` a `managed`.
    * Cualquier `PersistentVolumes` debe usar `kind: Managed`.
* Es compatible con AKS [varios grupos de nodos](https://docs.microsoft.com/azure/aks/use-multiple-node-pools) (actualmente en versión preliminar).
* Nodos basados en Windows Server se encuentran actualmente en [vista previa en AKS](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* AKS admite un conjunto limitado de [regiones](https://docs.microsoft.com/azure/aks/quotas-skus-regions).
* AKS es un servicio administrado con un plano de control hospedado de Kubernetes. Debe modificar las aplicaciones si se ha modificado la configuración de los patrones de ACS.

## <a name="differences-between-kubernetes-versions"></a>Diferencias entre versiones de Kubernetes

Si está migrando a una versión más reciente de Kubernetes (por ejemplo, desde 1.7 a 1.9. x), revise los siguientes recursos para comprender algunos cambios a la API de Kubernetes:

* [Migrar un ThirdPartyResource a CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [Cambios de la API de las cargas de trabajo en las versiones 1.8 y 1.9](https://kubernetes.io/docs/reference/workloads-18-19/)

## <a name="migration-considerations"></a>Consideraciones sobre la migración

### <a name="agent-pools"></a>Grupos de agentes

Aunque AKS administra el plano de control de Kubernetes, todavía se definen el tamaño y el número de nodos que se va a incluir en el nuevo clúster. Se supone que quiere una asignación 1:1 de ACS a AKS, por lo que querrá capturar la información de nodo de ACS existente. Use estos datos al crear el nuevo clúster de AKS.

Ejemplo:

| NOMBRE | Count | Tamaño de VM | Sistema operativo |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 |  Windows |

Dado que las máquinas virtuales adicionales se implementarán en su suscripción durante la migración, debe comprobar que los límites y cuotas son suficientes para estos recursos. 

Para obtener más información, consulte [suscripción de Azure y límites de servicio](https://docs.microsoft.com/azure/azure-subscription-service-limits). Para comprobar las cuotas actuales en el portal de Azure, vaya a la [hoja suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), seleccione su suscripción y, a continuación, seleccione **uso y cuotas**.

### <a name="networking"></a>Redes

En aplicaciones complejas se suele migrar a lo largo del tiempo y no de una vez. Esto significa que los antiguos y nuevos entornos que necesite comunicarse a través de la red. Las aplicaciones que usaban `ClusterIP` servicios comunicarse deba exponerse como tipo `LoadBalancer` y protegerse correctamente.

Para completar la migración, desea apuntar a los clientes a los nuevos servicios que se ejecutan en AKS. Se recomienda redirigir el tráfico mediante la actualización de DNS para que apunte al equilibrador de carga que se coloca delante de su clúster de AKS.

### <a name="stateless-applications"></a>Aplicaciones sin estado

La migración de aplicaciones sin estado es el caso más sencillo. Deberá aplicar las definiciones de YAML al nuevo clúster, asegúrese de que todo funciona según lo previsto y redirigir el tráfico para activar el nuevo clúster.

### <a name="stateful-applications"></a>Aplicaciones con estado

Planear cuidadosamente la migración de aplicaciones con estado para evitar la pérdida de datos o tiempos de inactividad inesperados.

#### <a name="highly-available-applications"></a>Aplicaciones de alta disponibilidad

Puede implementar algunas aplicaciones con estado en una configuración de alta disponibilidad. Estas aplicaciones pueden copiar datos entre las réplicas. Si actualmente usa a este tipo de implementación, puede crear a un nuevo miembro en el nuevo clúster AKS y, a continuación, migrar con un efecto mínimo en los llamadores indirectos. Por lo general, los pasos de migración para este escenario son:

1. Crear una nueva réplica secundaria en AKS.
2. Espere a que replicación de datos.
3. Un error realizar una réplica secundaria de la nueva réplica principal.
4. Punto de tráfico en el clúster de AKS.

#### <a name="migrating-persistent-volumes"></a>Migrar los volúmenes persistentes

Si va a migrar los volúmenes persistentes existentes en AKS, generalmente le siga estos pasos:

1. En reposo se escribe en la aplicación. (Este paso es opcional y requiere tiempo de inactividad).
2. Tomar instantáneas de los discos.
3. Cree nuevos discos administrados de las instantáneas.
4. Cree volúmenes persistentes en AKS.
5. Actualizar las especificaciones de pod para [usar volúmenes existentes](https://docs.microsoft.com/azure/aks/azure-disk-volume) en lugar de PersistentVolumeClaims (aprovisionamiento estática).
6. Implementar la aplicación en AKS.
7. Validar.
8. Punto de tráfico en el clúster de AKS.

> [!IMPORTANT]
> Si decide no escrituras en reposo, deberá replicar datos a la nueva implementación. En caso contrario, perderá los datos que se ha escrito una vez llevada a cabo las instantáneas de disco.

Algunas herramientas de código abierto pueden ayudarle a crear discos administrados y migrar los volúmenes entre clústeres de Kubernetes:

* [Extensión de copia de discos de la CLI de Azure](https://github.com/noelbundick/azure-cli-disk-copy-extension) copia y convierte los discos en grupos de recursos y regiones de Azure.
* [Extensión de Kube CLI de Azure](https://github.com/yaron2/azure-kube-cli) enumera los volúmenes de Kubernetes de ACS y migrarlos a un clúster de AKS.

#### <a name="azure-files"></a>Archivos de Azure

A diferencia de los discos, Azure Files puede montarse simultáneamente en varios hosts. En el clúster de AKS, Azure y Kubernetes no impiden que se crea un pod que sigue usando el clúster de ACS. Para evitar la pérdida de datos y un comportamiento inesperado, asegúrese de que los clústeres no escriben en los mismos archivos al mismo tiempo.

Si la aplicación puede hospedar varias réplicas que apuntan al mismo recurso compartido de archivos, siga los pasos de migración de estado e implementar las definiciones de YAML al nuevo clúster. Si no es así, un posible enfoque de migración implica los pasos siguientes:

1. Implementar la aplicación en AKS con un número de réplicas de 0.
2. Puede escalar la aplicación en ACS en 0. (Este paso requiere tiempo de inactividad).
3. Hasta 1 puede escalar la aplicación en AKS.
4. Validar.
5. Punto de tráfico en el clúster de AKS.

Si desea comenzar con un recurso compartido de vacío y hacer una copia del origen de datos, puede usar el [ `az storage file copy` ](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) comandos para migrar los datos.

### <a name="deployment-strategy"></a>Estrategia de implementación

Se recomienda usar la canalización de CI/CD existente para implementar una configuración válida conocida en AKS. Clonar las tareas de implementación existente y asegúrese de que `kubeconfig` apunta al nuevo clúster de AKS.

Si no es posible, exportar las definiciones de recursos de ACS y, a continuación, aplicarlos a AKS. Puede usar `kubectl` para exportar objetos.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Pueden ayudar varias herramientas de código abierto según sus necesidades de implementación:

* [Velero](https://github.com/heptio/ark) (esta herramienta requiere 1.7 Kubernetes).
* [Extensión de Kube CLI de Azure](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Pasos de migración

1. [Crear un clúster de AKS](https://docs.microsoft.com/azure/aks/create-cluster) mediante Azure portal, CLI de Azure o plantilla de Azure Resource Manager.

   > [!NOTE]
   > Buscar plantillas de Azure Resource Manager de ejemplo para AKS en la [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) repositorio de GitHub.

2. Realice los cambios necesarios para las definiciones de YAML. Por ejemplo, reemplace `apps/v1beta1` con `apps/v1` para `Deployments`.

3. [Migrar volúmenes](#migrating-persistent-volumes) (opcional) en el clúster de ACS a su clúster AKS.

4. Usar el sistema de CI/CD para implementar aplicaciones en AKS. O bien, use kubectl para aplicar las definiciones de YAML.

5. Validar. Asegúrese de que sus aplicaciones funcionan según lo previsto y que se han copiado los datos migrados.

6. Redirigir el tráfico. Actualice el DNS para dirigir a los clientes a la implementación de AKS.

7. Finalizar las tareas posteriores a la migración. Si migra los volúmenes y eligió no poner en modo inactivo escrituras, copiar los datos al nuevo clúster.
