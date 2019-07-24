---
title: Uso de una identidad administrada con Azure Container Registry Tasks
description: Asigne una identidad administrada para recursos de Azure con el fin de facilitar el acceso de una tarea de Azure Container Registry a los recursos de Azure como, por ejemplo, otros registros de contenedor privado.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5b60727472a06aaac8ccd3dce8609461e8972311
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148037"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Uso de una identidad administrada de Azure en ACR Tasks 

Use una [identidad administrada para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) para autenticarse en ACR Tasks u otros recursos de Azure, sin necesidad de proporcionar o administrar credenciales en el código. Por ejemplo, use una identidad administrada para extraer o insertar imágenes en otro registro como paso de una tarea.

En este artículo, conocerá mejor las identidades administradas y aprenderá a realizar las siguientes tareas:

> [!div class="checklist"]
> * Habilitación de una identidad asignada por el sistema o asignada por el usuario en una tarea de ACR
> * Concesión de acceso de identidad a recursos de Azure, como otros registros de contenedor de Azure
> * Uso de la identidad administrada para acceder a los recursos de una tarea 

Para crear los recursos de Azure, en este artículo es necesario ejecutar la CLI de Azure versión 2.0.66 o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli].

## <a name="why-use-a-managed-identity"></a>¿Por qué usar una identidad administrada?

Una identidad administrada de los recursos de Azure proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede configurar [algunos recursos de Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), por ejemplo, ACR Tasks, con una identidad administrada. A continuación, use la identidad para tener acceso a otros recursos de Azure sin pasar credenciales mediante código o scripts.

Las identidades administradas son de dos tipos:

* Las *identidades asignadas por el usuario*, que puede asignar a varios recursos y conservar durante tanto tiempo como necesite. Las identidades asignadas por el usuario están actualmente en versión preliminar.

* Una *identidad administrada por el sistema*, que es única para un recurso especifico como una tarea de ACR y tiene una duración igual a la vigencia del recurso.

Una vez que haya configurado un recurso de Azure con una identidad administrada, conceda acceso a la identidad para que esta acceda a otro recurso, tal como haría con cualquier entidad de seguridad. Por ejemplo, asigne a una identidad administrada un rol con permisos de extracción, de inserción y extracción o de otro tipo en un registro de contenedor privado de Azure. (Para obtener una lista completa de los roles de registros, consulte [Roles y permisos de Azure Container Registry](container-registry-roles.md)). Puede permitir que una identidad acceda a uno o más recursos.

## <a name="create-container-registries"></a>Creación de registros de contenedor

En este tutorial, necesita tres registros de contenedor:

* Utilice el primer registro para crear y ejecutar ACR Tasks. En este artículo, este registro de origen se denomina *myregistry*. 
* El segundo y tercer registro son registros de destino para que la primera tarea de ejemplo inserte una imagen que compila. En este artículo, los registros de destino se denominan *customregistry1* y *customregistry2*.

Reemplácelos por sus propios nombres de registro en pasos posteriores.

Si aún no tiene los registros de contenedor de Azure necesarios, consulte [Inicio rápido: Creación de un registro de contenedor privado con la CLI de Azure](container-registry-get-started-azure-cli.md). No tiene que insertar imágenes en el registro todavía.

## <a name="example-task-with-a-system-assigned-identity"></a>Ejemplo: Tarea con una identidad asignada por el sistema

En este ejemplo se muestra cómo crear una [tarea de varios pasos](container-registry-tasks-multi-step.md) con una identidad asignada por el sistema. La tarea compila una imagen y luego usa la identidad para autenticarse con dos registros de destino para insertar la imagen.

Los pasos de esta tarea de ejemplo se definen en un [archivo YAML](container-registry-tasks-reference-yaml.md) denominado `testtask.yaml`. El archivo se encuentra en el directorio multipleRegistries del repositorio de ejemplos [acr-tasks](https://github.com/Azure-Samples/acr-tasks). El archivo se reproduce aquí:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Creación de una tarea con una identidad asignada por el sistema

Cree la tarea *multiple-reg* mediante la ejecución del comando [az acr task create][az-acr-task-create] siguiente. El contexto de la tarea es la carpeta multipleRegistries del repositorio de ejemplos, y el comando hace referencia al archivo `testtask.yaml` en el repositorio. El parámetro `--assign-identity` sin valor adicional crea una identidad asignada por el sistema para la tarea. Esta tarea se configura para que haya que desencadenarla manualmente, pero puede configurarse para que se ejecute al insertar confirmaciones en el repositorio o al realizar una solicitud de incorporación de cambios. 

```azurecli
az acr task create \
  --registry myregistry \
  --name multiple-reg \
  --context https://github.com/Azure-Samples/acr-tasks.git#:multipleRegistries \
  --file testtask.yaml \
  --commit-trigger-enabled false \
  --pull-request-trigger-enabled false \
  --assign-identity
```

En la salida del comando, la sección `identity` muestra que en la tarea se establece una identidad de tipo `SystemAssigned`. `principalId` es el identificador de entidad de servicio de la identidad:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 

Utilice el comando [az acr task show][az-acr-task-show] para almacenar `principalId` en una variable, para su uso en comandos posteriores:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>Concesión de permisos de envío de cambios de identidad a dos registros de contenedor de destino

En esta sección se concede a la identidad asignada por el sistema permisos de envío de cambios a los dos registros de destino llamados *customregistry1* y *customregistry2*.

Utilice primero el comando [az acr show][az-acr-show] para obtener el identificador de recurso de cada registro de contenedor y almacenar los identificadores en variables:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

Utilice el comando [az role assignment create][az-role-assignment-create] para asignar a la identidad el rol `acrpush` en cada registro. Este rol tiene permisos de extracción e inserción de imágenes en un registro de contenedor.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Incorporación de las credenciales del registro de destino a la tarea

Utilice ahora el comando [az acr task credential add][az-acr-task-credential-add] para agregar las credenciales de la identidad a la tarea para que se pueda autenticar con ambos registros de destino.

```azurecli
az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry1.azurecr.io \
  --use-identity [system]

az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry2.azurecr.io \
  --use-identity [system]
```

### <a name="manually-run-the-task"></a>Ejecución manual de la tarea

Use el comando [az acr task run][az-acr-task-run] para desencadenar manualmente la tarea. El parámetro `--set` se usa para pasar los nombres de servidor de inicio de sesión de los dos registros de destino como valores para las variables de tarea `REGISTRY1` y `REGISTRY2`.

```azurecli
az acr task run \
  --name multiple-reg \
  --registry myregistry \
  --set REGISTRY1=customregistry1.azurecr.io \
  --set REGISTRY2=customregistry2.azurecr.io
```

La salida es parecida a esta:

```console
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:02 Successfully executed container: acb_step_0
2019/05/31 22:16:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:02 Pushing image: customregistry2.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry2.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:08 Successfully pushed image: customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:08 Executing step ID: acb_step_2. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:08 Scanning for dependencies...
2019/05/31 22:16:08 Successfully scanned dependencies
2019/05/31 22:16:08 Launching container with name: acb_step_2
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:09 Successfully executed container: acb_step_2
2019/05/31 22:16:09 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:09 Pushing image: customregistry1.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry1.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:21 Successfully pushed image: customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:21 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.985291)
2019/05/31 22:16:21 Populating digests for step ID: acb_step_0...
2019/05/31 22:16:22 Successfully populated digests for step ID: acb_step_0
2019/05/31 22:16:22 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 5.743225)
2019/05/31 22:16:22 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 1.925959)
2019/05/31 22:16:22 Populating digests for step ID: acb_step_2...
2019/05/31 22:16:24 Successfully populated digests for step ID: acb_step_2
2019/05/31 22:16:24 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.061057)
2019/05/31 22:16:24 The following dependencies were found:
2019/05/31 22:16:24
- image:
    registry: customregistry2.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b
- image:
    registry: customregistry1.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b

Run ID: cf31 was successful after 35s
```

## <a name="example-task-with-a-user-assigned-identity"></a>Ejemplo: Tarea con una identidad asignada por el usuario

En este ejemplo se crea una identidad asignada por el usuario con permisos de lectura de secretos en una instancia de Azure Key Vault. Esta identidad se asigna a una tarea de varios pasos que lee el secreto, compila una imagen e inicia sesión en la CLI de Azure para leer la etiqueta de imagen.

### <a name="create-a-key-vault-and-store-a-secret"></a>Creación de un almacén de claves y almacenamiento de un secreto

En primer lugar, si es necesario, cree un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus* con el comando [az group create][az-group-create] siguiente:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Utilice el comando [az keyvault create][az-keyvault-create] para crear un almacén de claves. Asegúrese de especificar un nombre de almacén de claves único. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Almacene un secreto de ejemplo en el almacén de claves con el comando [az keyvault secret set][az-keyvault-secret-set]:

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Por ejemplo, es posible que quiera almacenar las credenciales para autenticarse con un registro privado de Docker a fin de extraer una imagen privada.

### <a name="create-an-identity"></a>Creación de una identidad

Cree una identidad denominada *myACRTasksId* en la suscripción con el comando [az identity create][az-identity-create]. Puede usar el mismo grupo de recursos que usó anteriormente para crear el registro de contenedor o el almacén de claves, o usar uno diferente.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Para configurar la identidad en los pasos siguientes, use el comando [az identity show][az-identity-show] para almacenar en variables los identificadores de la entidad de servicio y del recurso de la identidad.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Concesión de acceso de identidad a KeyVault para leer un secreto

Ejecute el comando [az keyvault set-policy][az-keyvault-set-policy] siguiente para establecer una directiva de acceso en el almacén de claves. En el ejemplo siguiente se permite que la identidad asignada por el usuario obtenga secretos del almacén de claves. Este acceso es necesario posteriormente para ejecutar correctamente una tarea de varios pasos.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Concesión de acceso de lectura de identidad en el grupo de recursos del registro

Ejecute el comando [az role assignment create][az-role-assignment-create] siguiente para asignar a la identidad un rol de lector, en este caso, en el grupo de recursos que contiene el registro de origen. Este rol es necesario posteriormente para ejecutar correctamente una tarea de varios pasos.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Creación de una tarea con una identidad asignada por el sistema

Cree ahora una [tarea de varios pasos](container-registry-tasks-multi-step.md) y asígnele la identidad asignada por el usuario. En esta tarea de ejemplo se crea un [archivo YAML](container-registry-tasks-reference-yaml.md) denominado `managed-identities.yaml` en un directorio de trabajo local y se pega el siguiente contenido. Asegúrese de reemplazar el nombre de almacén de claves del archivo por el nombre de su almacén de claves.

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: name
    keyvault: https://mykeyvault.vault.azure.net/secrets/SampleSecret
steps:
  # Verify that the task can access the secret in the key vault
  - cmd: bash -c 'if [ -z "$MY_SECRET" ]; then echo "Secret not resolved"; else echo "Secret resolved"; fi'
    env: 
      - MY_SECRET='{{.Secrets.name}}' 

  # Build/push the website image to source registry, using dockerfile in the Azure-Samples repo
  - cmd: docker build -t {{.Run.Registry}}/my-website:{{.Run.ID}} https://github.com/Azure-Samples/aci-helloworld.git
  - push: 
    - "{{.Run.Registry}}/my-website:{{.Run.ID}}"
  
  # Login to Azure CLI with identity and list the tags to verify that the image is in the registry
  - cmd: microsoft/azure-cli az login --identity
  - cmd: microsoft/azure-cli az acr repository show-tags -n {{.Values.registryName}} --repository my-website
```

Esta tarea realiza lo siguiente:

* Comprueba que puede acceder al secreto del almacén de claves. Este paso se realiza a efectos de demostración. En un escenario real, es posible que necesite un paso de tarea para obtener las credenciales con el fin de acceder a un repositorio privado de Docker Hub.
* Compila e inserta la imagen `mywebsite` en el registro de origen.
* Inicia sesión en la CLI de Azure para ver la lista de etiquetas de la imagen `my-website` en el registro de origen.

Cree una tarea denominada *msitask* y pásele el identificador de recurso de la identidad asignada por el usuario que creó anteriormente. Esta tarea de ejemplo se crea a partir del archivo `managed-identities.yaml` que guardó en el directorio de trabajo local, por lo que debe desencadenarlo manualmente.

```azurecli
az acr task create \
  --name msitask \
  --registry myregistry \
  --context /dev/null \
  --file managed-identities.yaml \
  --pull-request-trigger-enabled false \
  --commit-trigger-enabled false \
  --assign-identity $resourceID
```

En la salida del comando, la sección `identity` muestra que en la tarea se establece una identidad de tipo `UserAssigned`. `principalId` es el identificador de entidad de servicio de la identidad:

```console
[...]
"identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRTasksId": {
        "clientId": "xxxxxxxx-f17e-4768-bb4e-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-1335-433d-bb6c-xxxxxxxxxxxx"
      }
[...]
```

### <a name="manually-run-the-task"></a>Ejecución manual de la tarea

Use el comando [az acr task run][az-acr-task-run] para desencadenar manualmente la tarea. El parámetro `--set` se usa para pasar el nombre del registro de origen a la tarea:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

La salida muestra que el secreto se ha resuelto, la imagen se ha compilado e insertado, y la tarea inicia sesión en la CLI de Azure con la identidad para leer la etiqueta de imagen en el registro de origen:

```console
Queued a run with ID: cf32
Waiting for an agent...
2019/06/10 23:25:37 Downloading source code...
2019/06/10 23:25:38 Finished downloading source code
2019/06/10 23:25:39 Using acb_vol_4e4a0a7c-b6ef-4ec5-b40f-3436fc5eb0f5 as the home volume
2019/06/10 23:25:41 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/10 23:25:41 Successfully set up Docker network: acb_default_network
2019/06/10 23:25:41 Setting up Docker configuration...
2019/06/10 23:25:42 Successfully set up Docker configuration
2019/06/10 23:25:42 Logging in to registry: myregistry.azurecr.io
2019/06/10 23:25:43 Successfully logged into myregistry.azurecr.io
2019/06/10 23:25:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:43 Launching container with name: acb_step_0
Secret resolved
2019/06/10 23:25:44 Successfully executed container: acb_step_0
2019/06/10 23:25:44 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:44 Launching container with name: acb_step_1
Sending build context to Docker daemon  74.75kB

[...]

cf32: digest: sha256:cbb4aa83b33f6959d83e84bfd43ca901084966a9f91c42f111766473dc977f36 size: 1577
2019/06/10 23:26:05 Successfully pushed image: myregistry.azurecr.io/my-website:cf32
2019/06/10 23:26:05 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:05 Launching container with name: acb_step_3
[
  {
    "environmentName": "AzureCloud",
    "id": "xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx",
    "isDefault": true,
    "name": "DanLep Internal Consumption",
    "state": "Enabled",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
      "user": {
      "assignedIdentityInfo": "MSI",
      "name": "systemAssignedIdentity",
      "type": "servicePrincipal"
    }
  }
]
2019/06/10 23:26:09 Successfully executed container: acb_step_3
2019/06/10 23:26:09 Executing step ID: acb_step_4. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:09 Launching container with name: acb_step_4
[
  "cf32"
]
2019/06/10 23:26:14 Successfully executed container: acb_step_4
2019/06/10 23:26:14 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.025312)
2019/06/10 23:26:14 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 13.703823)
2019/06/10 23:26:14 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 6.791506)
2019/06/10 23:26:14 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 3.852972)
2019/06/10 23:26:14 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 5.079079)
```


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a usar identidades administradas con Azure Container Registry Tasks y a realizar las siguientes tareas:

> [!div class="checklist"]
> * Habilitación de una identidad asignada por el sistema o asignada por el usuario en una tarea de ACR
> * Concesión de acceso de identidad a recursos de Azure, como otros registros de contenedor de Azure
> * Uso de la identidad administrada para acceder a los recursos de una tarea  

* Obtenga más información sobre las [identidades administradas para recursos de Azure](/azure/active-directory/managed-identities-azure-resources/).

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
[az-keyvault-create]: /cli/azure/keyvault?#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
