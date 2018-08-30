---
title: Creación de un clúster de Azure Kubernetes Service (AKS)
description: Creación de un clúster de AKS con la CLI o Azure Portal.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6bfe6f9b76693ded79aa9b9d21ddcac4e1a0733e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43110311"
---
# <a name="create-an-azure-kubernetes-service-aks-cluster"></a>Creación de un clúster de Azure Kubernetes Service (AKS)

Un clúster de Azure Kubernetes Service (AKS) se puede crear con la CLI de Azure o Azure Portal.

## <a name="azure-cli"></a>Azure CLI

Use el comando [az aks create][az-aks-create] para crear el clúster de AKS.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Las siguientes opciones están disponibles con el comando `az aks create`. Para más información sobre cada uno de estos argumentos, consulte la [referencia de la CLI de Azure][az-aks-create] para AKS.

| Argumento | DESCRIPCIÓN | Obligatorio |
|---|---|:---:|
| `--name``-n` | Nombre de recurso para el clúster administrado. | Sí |
| `--resource-group``-g` | Nombre del grupo de recursos de Azure Kubernetes Service. | Sí |
| `--admin-username``-u` | Nombre de usuario para Linux Virtual Machines.  Valor predeterminado: azureuser. | no |
| `--aad-client-app-id` | (VERSIÓN PRELIMINAR) Identificador de una aplicación cliente de Azure Active Directory de tipo "nativo". | no |
| `--aad-server-app-id` | (VERSIÓN PRELIMINAR) Identificador de una aplicación cliente de Azure Active Directory de tipo "aplicación web/API". | no |
| `--aad-server-app-secret` | (VERSIÓN PRELIMINAR) Secreto de una aplicación de servidor de Azure Active Directory. | no |
| `--aad-tenant-id` | (VERSIÓN PRELIMINAR) Identificador de un inquilino de Azure Active Directory. | no |
| `--admin-username``-u` | Cuenta de usuario que se va a crear en las máquinas virtuales de un nodo para el acceso de SSH.  Valor predeterminado: azureuser. | no |
| ` --client-secret` | Secreto asociado a la entidad de servicio. | no |
| `--dns-name-prefix``-p` | Prefijo DNS para la dirección ip pública de clústeres. | no |
| `--dns-service-ip` | Dirección IP asignada al servicio DNS de Kubernetes. | no |
| `--docker-bridge-address` | Dirección IP y máscara de red asignadas al puente de Docker. | no |
| `--enable-addons``-a` | Habilita los complementos de Kubernetes de una lista separada por comas. | no |
| `--enable-rbac``-r` | Habilita el control de acceso basado en rol de Kubernetes. | no |
| `--generate-ssh-keys` | Generar archivos de clave pública y privada SSH si faltan. | no |
| `--kubernetes-version``-k` | Versión de Kubernetes que se va a usar para crear el clúster; por ejemplo, "1.7.9" o "1.9.6". | no |
| `--location``-l` | Ubicación del grupo de recursos creado automáticamente. | no |
| `--max-pods``-m` | Número máximo de pods que se pueden implementar en un nodo. | no |
| `--network-plugin` | Complemento de red de Kubernetes que se va a utilizar. | no |
| `--no-ssh-key``-x` | No se utilizan ni crean claves de SSH locales. | no |
| `--no-wait` | No espere hasta que finalice la operación de ejecución prolongada. | no |
| `--node-count``-c` | Número predeterminado de nodos para los grupos de nodos.  Valor predeterminado: 3. | no |
| `--node-osdisk-size` | Tamaño de osDisk en GB de la máquina virtual del grupo de nodos. | no |
| `--node-vm-size``-s` | Tamaño de la máquina virtual.  Valor predeterminado: Standard_D1_v2. | no |
| `--pod-cidr` | Intervalo IP en notación CIDR desde el que se van a asignar las direcciones IP de los pods cuando se utilice kubenet. | no |
| `--service-cidr` | Intervalo IP en notación CIDR desde el que se van a asignar las direcciones IP del clúster de servicio. | no |
| `--service-principal` | Entidad de servicio utilizada para la autenticación de clúster. | no |
| `--ssh-key-value` | Valor de archivo de clave o ruta de acceso al archivo de clave SSH.  Valor predeterminado: ~/.ssh/id_rsa.pub. | no |
| `--tags` | Etiquetas separadas por espacios con el formato “clave [= valor]”. Use “” para borrar las etiquetas existentes. | no |
| `--vnet-subnet-id` | Identificador de la subred de una red virtual existente en la que se va a implementar el clúster. | no |
| `--workspace-resource-id` | Identificador de recurso de un área de trabajo de Log Analytics para almacenar los datos relacionados con la supervisión. | no |

## <a name="azure-portal"></a>Azure Portal

Para instrucciones sobre cómo implementar un clúster de AKS con Azure Portal, consulte el [inicio rápido de Azure Portal][aks-portal-quickstart] de Azure Kubernetes Service (AKS).

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
