---
title: Visualización de los registros de controlador de Azure Kubernetes Service (AKS)
description: Aprenda a habilitar y ver los registros del nodo maestro de Kubernetes en Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 07/26/2018
ms.author: iainfou
ms.openlocfilehash: 04afd71183bcb8001d017b0027f29338b8d67ddb
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42442374"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Habilitación y revisión de los registros del nodo maestro de Kubernetes en Azure Kubernetes Service (AKS)

Con Azure Kubernetes Service (AKS), los componentes principales, como *kube-apiserver* y *kube-controller-manager* se proporcionan como un servicio administrado. Cree y administre los nodos que ejecutan *kubelet* y el entorno de tiempo de ejecución del contenedor, e implemente las aplicaciones mediante el servidor de API de Kubernetes administrado. Para ayudar a solucionar los problemas de sus aplicaciones y servicios, quizás deba ver los registros generados por estos componentes principales. En este artículo se muestra cómo usar Azure Log Analytics para habilitar y consultar los registros de los componentes principales de Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se requiere un clúster de AKS existente que se ejecute en su cuenta de Azure. Si no dispone de un clúster de AKS, cree uno mediante la [CLI de Azure][cli-quickstart] o [Azure Portal][portal-quickstart]. Log Analytics funciona tanto con clústeres de AKS habilitados para RBAC como no habilitados para RBAC.

## <a name="enable-diagnostics-logs"></a>Habilitación del registro de diagnósticos

Para ayudar a recopilar y revisar los datos de varios orígenes, Log Analytics proporciona un motor de lenguaje y análisis de consultas que ofrece información detallada sobre su entorno. Un área de trabajo se usa para intercalar y analizar los datos, y se puede integrar con otros servicios de Azure como Application Insights y Security Center. Para usar una plataforma diferente para analizar los registros, puede decidir en su lugar enviar los registros de diagnóstico a una cuenta de almacenamiento o un centro de eventos de Azure. Para más información, consulte [¿Qué es Azure Log Analytics?][log-analytics-overview].

Log Analytics se habilita y administra en Azure Portal. Para habilitar la recopilación de registros para los componentes principales de Kubernetes en el clúster de AKS, abra Azure Portal en un explorador web y realice los pasos siguientes:

1. Seleccione el grupo de recursos del clúster de AKS, como *myResourceGroup*. No seleccione el grupo de recursos que contiene los recursos del clúster de AKS individuales, como *MC_myResourceGroup_myAKSCluster_eastus*.
1. En la parte izquierda, elija **Configuración de diagnóstico**.
1. Seleccione el clúster de AKS, como *myAKSCluster* y luego elija **Turn on diagnostics** (Activar diagnósticos).
1. Escriba un nombre, como *myAKSLogs* y luego seleccione la opción **Send to Log Analytics** (Enviar a Log Analytics).
    * Elija *Configure Log Analytics* (Configurar Log Analytics) y seleccione un área de trabajo existente o **Crear área de trabajo nueva**.
    * Si necesita crear un área de trabajo, proporcione un nombre, un grupo de recursos y una ubicación.
1. En la lista de registros disponibles, seleccione los registros que quiere habilitar, como *kube-apiserver*, *kube-controller-manager* y *kube-scheduler*. Puede volver y cambiar los registros recopilados una vez que Log Analytics está habilitado.
1. Cuando esté listo, seleccione **Guardar** para habilitar la recopilación de los registros seleccionados.

La siguiente captura de pantalla de ejemplo del portal muestra la ventana *Configuración de diagnóstico* y luego la opción para crear un área de trabajo de OMS:

![Habilitación del área de trabajo de OMS para el clúster de AKS de Log Analytics](media/view-master-logs/enable-oms-log-analytics.png)

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Programación de un pod de prueba en el clúster de AKS

Para generar algunos registros, cree un nuevo pod en el clúster de AKS. El siguiente ejemplo de manifiesto de YAML puede usarse para crear una instancia básica de NGINX. Cree un archivo denominado `nginx.yaml` en el editor que prefiera y pegue el contenido siguiente:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: myfrontend
    image: nginx
    ports:
    - containerPort: 80
```

Cree el pod con el comando [kubectl create][kubectl-create] y especifique el archivo YAML, como se muestra en el ejemplo siguiente:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Visualización de los datos recopilados

Los registros de diagnóstico pueden tardar unos minutos en habilitarse y aparecer en el área de trabajo de OMS. En Azure Portal, seleccione el grupo de recursos para el área de trabajo de Log Analytics, como *myResourceGroup* y luego elija el recurso de Log Analytics, como *myAKSLogs*.

![Selección del área de trabajo de Log Analytics para el clúster de AKS](media/view-master-logs/select-log-analytics-workspace.png)

En el lado izquierdo, elija **Búsqueda de registros**. Para ver *kube-apiserver*, escriba la siguiente consulta en el cuadro de texto:

```
search *
| where Type == "AzureDiagnostics"
| where Category == "kube-apiserver"
| project log_s
```

Es probable que se devuelvan muchos registros para el servidor de API. Para reducir el ámbito de la consulta y ver los registros sobre el pod de NGINX creado en el paso anterior, agregue una instrucción *where* adicional para buscar *pods/nginx*, como se muestra en la consulta de ejemplo siguiente:

```
search *
| where Type == "AzureDiagnostics"
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

Se muestran los registros específicos de su pod NGINX, como se ilustra en la siguiente captura de pantalla de ejemplo:

![Resultados de consulta de Log Analytics del pod NGINX de ejemplo](media/view-master-logs/log-analytics-query-results.png)

Para ver registros adicionales, puede actualizar la consulta del nombre *Categoría* a *kube-controller-manager* o *kube-programador*, dependiendo de qué registros adicionales habilite. Luego, se pueden usar instrucciones adicionales *where* para refinar los eventos que busca.

Para más información sobre cómo consultar y filtrar sus datos de registro, consulte [Visualización o análisis de los datos recopilados con la búsqueda de registros de Log Analytics][analyze-log-analytics].

## <a name="log-event-schema"></a>Esquema de evento de registro

Para ayudar a analizar los datos de registro, en la tabla siguiente se describe el esquema usado para cada evento:

| Nombre del campo               | DESCRIPCIÓN |
|--------------------------|-------------|
| *resourceId*             | Recursos de Azure que generó el registro |
| *time*                   | Marca de tiempo de cuando se cargó el registro |
| *category*               | Nombre del contenedor o componente que genera el registro |
| *operationName*          | Siempre *Microsoft.ContainerService/managedClusters/diagnositicLogs/Read* |
| *properties.log*         | Texto completo del registro desde el componente |
| *properties.stream*      | *stderr* o *stdout* |
| *properties.pod*         | Nombre del pod del que procede el registro |
| *properties.containerID* | Identificador del contenedor de Docker del que procede este registro |

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a habilitar y revisar los registros de los componentes principales de Kubernetes en el clúster de AKS. Para seguir supervisando y solucionando problemas, también puede [ver los registros de Kubelet][kubelet-logs] y [habilitar el acceso al nodo SSH][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../log-analytics/log-analytics-tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md