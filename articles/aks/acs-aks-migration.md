---
title: Migración de Azure Container Service (ACS) a Azure Kubernetes Service (AKS)
description: Migración de Azure Container Service (ACS) a Azure Kubernetes Service (AKS)
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: 910c96988ec0a8b8aa7b6ac8ce287c4fdc59e177
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649975"
---
# <a name="migrating-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migración de Azure Container Service (ACS) a Azure Kubernetes Service (AKS)

El objetivo de este documento es ayudarle a planear y ejecutar una migración correcta entre Azure Container Service con Kubernetes (ACS) y Azure Kubernetes Service (AKS). Esta guía detalla las diferencias entre ACS y AKS, proporciona información general sobre el proceso de migración y le ayudará a tomar decisiones claves.

## <a name="differences-between-acs-and-aks"></a>Diferencias entre ACS y AKS

ACS y AKS difieren en algunas áreas claves que afectan la migración. Debe revisar y planificar cómo abordar las siguientes diferencias antes de cualquier migración.

* Los nodos de AKS utilizan [Managed Disks](../virtual-machines/windows/managed-disks-overview.md)
    * Los discos no administrados han de convertirse para poder asociarlos a nodos de AKS
    * Los objetos `StorageClass` personalizados para discos de Azure tienen que cambiarse de `unmanaged` a `managed`
    * Cualquier objeto `PersistentVolumes` tendrá que utilizar `kind: Managed`
* AKS solo admite en este momento un único grupo de agentes
* Los nodos basados en Windows Server se encuentran actualmente en [versión preliminar privada](https://azure.microsoft.com/blog/kubernetes-on-azure/)
* Comprobar la lista de [regiones admitidas](https://docs.microsoft.com/azure/aks/container-service-quotas) de AKS
* AKS es un servicio administrado con un plano de control hospedado de Kubernetes. Es posible que tenga que modificar las aplicaciones si previamente ha modificado la configuración de los maestros de ACS

### <a name="differences-between-kubernetes-versions"></a>Diferencias entre versiones de Kubernetes

Si está migrando a una versión más reciente de Kubernetes (p. ej.: 1.7 a 1.9. x), hay algunos cambios a la API k8s que requieren su atención.

* [Migrate a ThirdPartyResource to CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/) (Migración de un ThirdPartyResource a CustomResourceDefinition)
* [Workloads API changes in versions 1.8 and 1.9](https://kubernetes.io/docs/reference/workloads-18-19/) (Workloads API cambia en las versiones 1.8 y 1.9).

## <a name="migration-considerations"></a>Consideraciones sobre la migración

### <a name="agent-pools"></a>Grupos de agentes

Aunque AKS administra el plano de control de Kubernetes, usted define el tamaño y el número de nodos que desea incluir en su nuevo clúster. Se supone que quiere una asignación 1:1 de ACS a AKS, por lo que querrá capturar la información de nodo de ACS existente. Utilizará estos datos al crear el clúster de AKS.

Ejemplo:

| NOMBRE | Número | Tamaño de VM | Sistema operativo |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 |  Windows |

Dado que las máquinas virtuales adicionales se implementarán en su suscripción durante la migración, debe comprobar que los límites y cuotas son suficientes para estos recursos. Para aprender más consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-subscription-service-limits). Para comprobar las cuotas actuales, vaya a la [hoja suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en Azure Portal, seleccione su suscripción y elija `Usage + quotas`.

### <a name="networking"></a>Redes

En aplicaciones complejas se suele migrar a lo largo del tiempo y no de una vez. Es decir, puede que los entornos nuevos y antiguos tengan que comunicarse a través de la red. Es posible que aplicaciones que anteriormente podían usar servicios `ClusterIP` para comunicarse deban exponerse como tipo `LoadBalancer` y estar protegidas adecuadamente.

Para completar la migración, querrá dirigir a los clientes a los nuevos servicios que se ejecutan en AKS. La mejor manera de redirigir el tráfico consiste en actualizar DNS para que apunte a la instancia de Load Balancer que se encuentra frente al clúster de AKS.

### <a name="stateless-applications"></a>Aplicaciones sin estado

La migración de aplicaciones sin estado es el caso más sencillo. Aplique las definiciones de YAML al nuevo clúster, valide que todo funciona según lo previsto y redirija el tráfico para activar el nuevo clúster.

### <a name="stateful-applications"></a>Aplicaciones con estado

La migración de aplicaciones con estado requiere un planeamiento cuidadoso para evitar la pérdida de datos o tiempos de inactividad inesperados.

#### <a name="highly-available-applications"></a>Aplicaciones de alta disponibilidad

Algunas aplicaciones con estado se pueden implementar en una configuración de alta disponibilidad y pueden copiar datos en todas las réplicas. Si esto describe su implementación actual, es posible crear otro miembro en el nuevo clúster de AKS y migrar sin apenas afectar a los autores de llamadas de bajada. Los pasos de migración para este escenario suelen ser:

1. Creación de una réplica secundaria en AKS
2. Espera hasta que los datos se repliquen
3. Realización de una conmutación por error que convierta la réplica secundaria en la nueva principal
4. Desvío del tráfico al clúster de AKS

#### <a name="migrating-persistent-volumes"></a>Migración de volúmenes persistentes

Hay varios factores que debe tener en cuenta si migra volúmenes persistentes existentes a AKS. Los pasos necesarios suelen ser:

1. (Opcional) Poner en modo inactivo las operaciones de escritura en la aplicación (requiere tiempo de inactividad)
2. Realización de instantáneas de los discos
3. Creación de Managed Disks a partir de las instantáneas
4. Creación de volúmenes persistentes en AKS
5. Actualización de las especificaciones de Pod para [usar volúmenes existentes](https://docs.microsoft.com/azure/aks/azure-disk-volume) en lugar de PersistentVolumeClaims (aprovisionamiento estático)
6. Implementación de aplicaciones en AKS
7. Validación
8. Desvío del tráfico al clúster de AKS

> **Importante**: Si decide no escrituras en reposo, deberá replicar datos a la nueva implementación, como faltarán los datos que se ha escrito desde la instantánea de disco

Existen herramientas de código abierto que pueden ayudarle a crear Managed Disks y a migrar volúmenes entre clústeres de Kubernetes.

* [noelbundick/azure-cli-disk-extension](https://github.com/noelbundick/azure-cli-disk-copy-extension): copiar y convertir discos en grupos de recursos y regiones de Azure
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli): mostrar volúmenes de ACS Kubernetes y migrarlos a un clúster de AKS

#### <a name="azure-files"></a>Archivos de Azure

A diferencia de los discos, Azure Files puede montarse simultáneamente en varios hosts. Ni Azure ni Kubernetes impide que pueda crear un Pod en el clúster de AKS que sigue usando el clúster de ACS. Para evitar la pérdida de datos y un comportamiento inesperado, debe asegurarse de que ambos clústeres no escriben en los mismos archivos al mismo tiempo.

Si la aplicación puede hospedar varias réplicas que apunten al mismo recurso compartido de archivos, puede seguir los pasos de la migración sin estado e implementar las definiciones de YAML en el nuevo clúster.

Si no es así, un posible enfoque de migración implica los pasos siguientes:

1. Implementar la aplicación en AKS con un número de réplicas de 0
2. Escalar la aplicación en ACS a 0 (requiere tiempo de inactividad)
3. Escalar la aplicación en AKS hasta 1
4. Validación
5. Desvío del tráfico al clúster de AKS

En los casos en los que quiera comenzar con un recurso compartido vacío y luego realizar una copia de los datos de origen, puede utilizar los comandos [`az storage file copy`](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) para migrar los datos.

### <a name="deployment-strategy"></a>Estrategia de implementación

El método recomendado consiste en usar la canalización de CI/CD existente para implementar una configuración válida conocida en AKS. Clonará las tareas de implementación existentes y se asegurará de que su `kubeconfig` apunta al nuevo clúster de AKS.

En los casos en que esto no sea posible, deberá exportar la definición de recursos de ACS y aplicarla a AKS. Puede usar `kubectl` para exportar objetos.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

También hay varias herramientas de código abierto que, según sus necesidades, pueden resultar útiles:

* [heptio/ark](https://github.com/heptio/ark): requiere k8s 1.7
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli)
* [mhausenblas/reshifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Pasos de migración

### <a name="1-create-an-aks-cluster"></a>1. Creación de un clúster de AKS

Puede seguir los documentos para [crear un clúster de AKS](https://docs.microsoft.com/azure/aks/create-cluster) mediante Azure Portal, la CLI de Azure o una plantilla de Resource Manager.

> Puede encontrar plantillas de Azure Resource Manager de ejemplo para AKS en repositorio [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) de GitHub

### <a name="2-modify-applications"></a>2. Modificación de aplicaciones

Realice las modificaciones necesarias a las definiciones de YAML. Por ejemplo: reemplazar `apps/v1beta1` por `apps/v1` para `Deployments`

### <a name="3-optional-migrate-volumes"></a>3. (Opcional) Migración de volúmenes

Migre los volúmenes del clúster de ACS la clúster de AKS. Puede encontrar más detalles en la sección [Migración de volúmenes persistentes](#migrating-persistent-volumes).

### <a name="4-deploy-applications"></a>4. Implementación de aplicaciones

Utilice el sistema de CI/CD para implementar aplicaciones en AKS o use kubectl para aplicar las definiciones de YAML.

### <a name="5-validate"></a>5. Validación

Valide que las aplicaciones funcionan según lo previsto y que se han copiado los datos migrados.

### <a name="6-redirect-traffic"></a>6. Redirección del tráfico

Actualice el DNS para dirigir a los clientes a la implementación de AKS.

### <a name="7-post-migration-tasks"></a>7. Pasos posteriores a la migración

Si migró volúmenes y decidió no poner en modo inactivo las operaciones de escritura, tendrá que copiar los datos en el nuevo clúster.
