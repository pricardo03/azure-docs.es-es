---
title: Autenticación con Azure Container Registry desde Azure Kubernetes Service
description: Aprenda a proporcionar acceso a las imágenes del registro de contenedor privado desde Azure Kubernetes Service mediante una entidad de servicio de Azure Active Directory.
services: container-service
author: dlepow
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.openlocfilehash: 1d7e130d619f580aeb82939e19ea5abf680ff039
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326483"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Autenticación con Azure Container Registry desde Azure Kubernetes Service

Cuando se usa Azure Container Registry (ACR) con Azure Kubernetes Service (AKS), es preciso establecer un mecanismo de autenticación. En este documento se detallan las configuraciones recomendadas para la autenticación entre estos dos servicios de Azure.

En este artículo se da por supuesto que ya ha creado un clúster de AKS y es posible tener acceso al clúster con el cliente de línea de comandos `kubectl`. 

## <a name="grant-aks-access-to-acr"></a>Concesión de acceso de AKS a ACR

Cuando se crea un clúster de AKS, Azure también crea una entidad de servicio para posibilitar la operatividad de los clústeres con otros recursos de Azure. Puede utilizar esta entidad de servicio generada automáticamente para la autenticación con un registro de control de acceso. Para ello, deberá crear una instancia de [asignación de roles](../role-based-access-control/overview.md#role-assignments) de Azure AD que conceda a la entidad de servicio del clúster acceso al registro de contenedor.

Use el siguiente script para conceder a la entidad de servicio generada con AKS acceso para extraer del registro de contenedor de Azure. Modifique las variables `AKS_*` y `ACR_*` para su entorno antes de ejecutar el script.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Acceso con un secreto de Kubernetes

En algunos casos, es posible que no pueda asignar el rol necesario a la entidad de servicio de AKS generada automáticamente concediéndole acceso al registro de contenedor de acceso. Por ejemplo, debido al modelo de seguridad de su organización, podría no tener permisos suficientes en el inquilino de Azure Active Directory para asignar un rol a la entidad de servicio generada con AKS. La asignación de un rol a una entidad de servicio requiere que la cuenta de Azure AD tenga permiso de escritura para el inquilino de Azure AD. Si no tiene permiso, puede crear una nueva entidad de servicio y luego concederle acceso al registro de contenedor con un secreto de extracción de imagen de Kubernetes.

Use el siguiente script para crear una nueva entidad de servicio (usará sus credenciales para el secreto de extracción de imágenes de Kubernetes). Modifique la variable `ACR_NAME` para su entorno antes de ejecutar el script.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create acrpull role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name http://$SERVICE_PRINCIPAL_NAME --role acrpull --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Ahora puede almacenar las credenciales de la entidad de servicio en un [secreto de extracción de imágenes][image-pull-secret] de Kubernetes, al que el clúster de AKS hará referencia al ejecutar los contenedores.

Use el siguiente comando **kubectl** para crear el secreto de Kubernetes. Reemplace `<acr-login-server>` con el nombre completo del registro de contenedor de Azure (está en formato "nombrearc.azurecr.io"). Reemplace `<service-principal-ID>` y `<service-principal-password>` con los valores obtenidos al ejecutar el script anterior. Reemplace `<email-address>` por cualquier dirección de correo electrónico con el formato correcto.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Ahora puede usar el secreto de Kubernetes en las implementaciones de pod especificando su nombre (en este caso, "acr-auth") en el parámetro `imagePullSecrets`:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
