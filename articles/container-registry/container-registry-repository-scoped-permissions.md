---
title: Permisos a repositorios en Azure Container Registry
description: Cree un token con permisos orientados a repositorios específicos de un registro para extraer o insertar imágenes.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2019
ms.author: danlep
ms.openlocfilehash: 7b9d220ac7e507513458eab6b55276b3aa434739
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73742749"
---
# <a name="repository-scoped-permissions-in-azure-container-registry"></a>Permisos orientados a repositorios en Azure Container Registry 

Azure Container Registry admite varias [opciones de autenticación](container-registry-authentication.md) que usan identidades que tienen [acceso basado en roles](container-registry-roles.md) a un registro completo. Sin embargo, para determinados escenarios, es posible que tenga que proporcionar acceso únicamente a *repositorios* específicos de un registro. 

En este artículo se muestra cómo crear y usar un token de acceso que tiene permisos para realizar acciones solo en repositorios específicos de un registro. Con un token de acceso, puede proporcionar a los usuarios o servicios un acceso de tiempo limitado y orientado a los repositorios, con el fin de extraer o insertar imágenes o de realizar otras acciones. 

Consulte [Acerca de los permisos orientados a repositorios](#about-repository-scoped-permissions) más adelante en este artículo para obtener información general sobre los conceptos y escenarios de token.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar y se [aplican algunas limitaciones](#preview-limitations). Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="preview-limitations"></a>Limitaciones de vista previa

* Esta característica solo está disponible en un registro de contenedor **Premium**. Para obtener información sobre los límites y niveles de servicio de registro, consulte [SKU de Azure Container Registry](container-registry-skus.md).
* Actualmente no se pueden asignar permisos orientados a repositorios a un objeto de Azure Active Directory como una entidad de servicio o una identidad administrada.

## <a name="prerequisites"></a>Requisitos previos

* **CLI de Azure**: para realizar este tutorial es necesaria una instalación local de la CLI de Azure (versión 2.0.76 o posterior). Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).
* **Docker**: para autenticarse con el registro, también necesita una instalación local de Docker. Docker ofrece instrucciones de instalación para sistemas [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms).
* **Registro de contenedor con repositorios**: si no tiene uno, cree un registro de contenedor en la suscripción de Azure. Por ejemplo, use [Azure Portal](container-registry-get-started-portal.md) o la [CLI de Azure](container-registry-get-started-azure-cli.md). 

  A efectos de prueba, [extraiga](container-registry-get-started-docker-cli.md) o [importe](container-registry-import-images.md) una o varias imágenes de ejemplo en el registro. En los ejemplos de este artículo se hace referencia a las siguientes imágenes de dos repositorios: `samples/hello-world:v1` y `samples/nginx:v1`. 

## <a name="create-an-access-token"></a>Creación de un token de acceso

Cree un token con el comando [az acr token create][az-acr-token-create]. Al crear un token, especifique uno o varios repositorios y acciones asociadas en cada repositorio, o bien especifique una asignación de ámbito con esos valores.

### <a name="create-access-token-and-specify-repositories"></a>Creación de un token de acceso y especificación de repositorios

En el ejemplo siguiente se crea un token de acceso con permisos para realizar acciones `content/write` y `content/read` en el repositorio `samples/hello-world`, y la acción `content/read` en el repositorio `samples/nginx`. De forma predeterminada, el comando genera dos contraseñas. 

En este ejemplo se establece el estado del token en `enabled` (el valor predeterminado), pero puede actualizarlo en cualquier momento y establecer el estado en `disabled`.

```azurecli
az acr token create --name MyToken --registry myregistry \
  --repository samples/hello-world content/write content/read \
  --repository samples/nginx content/read --status enabled
```

La salida muestra detalles sobre el token, incluidas las contraseñas generadas y la asignación de ámbito. Se recomienda guardar las contraseñas en un lugar seguro para usarlas más adelante con `docker login`. Las contraseñas no se pueden recuperar de nuevo, pero se pueden generar otras.

La salida también muestra que se crea automáticamente una asignación de ámbito, denominada `MyToken-scope-map`. Puede usar la asignación de ámbito para aplicar las mismas acciones de repositorio a otros tokens. También puede actualizar la asignación de ámbito más adelante para cambiar los permisos de token.

```console
{
  "creationDate": "2019-10-22T00:15:34.066221+00:00",
  "credentials": {
    "certificates": [],
    "passwords": [
      {
        "creationTime": "2019-10-22T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password1",
        "value": "uH54BxxxxK7KOxxxxRbr26dAs8JXxxxx"
      },
      {
        "creationTime": "2019-10-22T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password2",
        "value": "kPX6Or/xxxxLXpqowxxxxkA0idwLtmxxxx"
      }
    ],
    "username": "MyToken"
  },
  "id": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/tokens/MyToken",
  "name": "MyToken",
  "objectId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapId": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyToken-scope-map",
  "status": "enabled",
  "type": "Microsoft.ContainerRegistry/registries/tokens"
```

### <a name="create-a-scope-map-and-associated-token"></a>Creación de una asignación de ámbito y un token asociado

También puede especificar una asignación de ámbito con los repositorios y las acciones asociadas al crear un token. Para crear una asignación de ámbito, use el comando [az acr scope-map create][az-acr-scope-map-create].

El siguiente comando de ejemplo crea una asignación de ámbito con los mismos permisos utilizados en el ejemplo anterior. Permite las acciones `content/write` y `content/read` en el repositorio `samples/hello-world`, y la acción `content/read` en el repositorio `samples/nginx`:

```azurecli
az acr scope-map create --name MyScopeMap --registry myregistry \
  --repository samples/hello-world content/write content/read \
  --repository samples/nginx content/read \
  --description "Sample scope map"
```

La salida es similar a la siguiente:

```console
{
  "actions": [
    "repositories/samples/hello-world/content/write",
    "repositories/samples/nginx/content/read"
  ],
  "creationDate": "2019-10-22T05:07:35.194413+00:00",
  "description": "Sample scope map.",
  "id": "/subscriptions/fxxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyScopeMap",
  "name": "MyScopeMap",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapType": "UserDefined",
  "type": "Microsoft.ContainerRegistry/registries/scopeMaps"
```

Ejecute [az acr token create][az-acr-token-create] para crear un token asociado a la asignación de ámbito *MyScopeMap*. De forma predeterminada, el comando genera dos contraseñas. En este ejemplo se establece el estado del token en `enabled` (el valor predeterminado), pero puede actualizarlo en cualquier momento y establecer el estado en `disabled`.

```azurecli
az acr token create --name MyToken --registry myregistry --scope-map MyScopeMap --status enabled
```

La salida muestra detalles sobre el token, incluidas las contraseñas generadas y la asignación de ámbito que ha aplicado. Se recomienda guardar las contraseñas en un lugar seguro para usarlas más adelante con `docker login`. Las contraseñas no se pueden recuperar de nuevo, pero se pueden generar otras.

## <a name="generate-passwords-for-token"></a>Generación de contraseñas para el token

Si se crearon contraseñas al crear el token, consulte [Autenticación mediante el token](#authenticate-using-token).

Si no tiene una contraseña de token o desea generar contraseñas, ejecute el comando [az acr token credential generate][az-acr-token-credential-generate].

En el ejemplo siguiente se genera una contraseña para el token que ha creado, con un período de expiración de 30 días. Almacena la contraseña en la variable de entorno TOKEN_PWD. El fragmento de código tiene el formato para el shell de Bash.

```azurecli
TOKEN_PWD=$(az acr token credential generate \
  --name MyToken --registry myregistry --days 30 \
  --password1 --query 'passwords[0].value' --output tsv)
```

## <a name="authenticate-using-token"></a>Autenticación mediante el token

Ejecute `docker login` para autenticarse con el registro mediante las credenciales de token. Escriba el nombre del token como el nombre de usuario y proporcione una de sus contraseñas. El siguiente ejemplo tiene el formato para el shell de Bash y se proporcionan los valores mediante variables de entorno.

```bash
TOKEN_NAME=MyToken
TOKEN_PWD=<token password>

echo $TOKEN_PWD | docker login --username $TOKEN_NAME --password-stdin myregistry.azurecr.io
```

La salida debería mostrar la autenticación correcta:

```console
Login Succeeded
```

## <a name="verify-scoped-access"></a>Comprobación del acceso orientado

Puede comprobar que el token proporciona permisos orientados a los repositorios del registro. En este ejemplo, los siguientes comandos `docker pull` se completan correctamente para extraer las imágenes disponibles en los repositorios `samples/hello-world` y `samples/nginx`:

```console
docker pull myregistry.azurecr.io/samples/hello-world:v1
docker pull myregistry.azurecr.io/samples/nginx:v1
```

Dado que el token de ejemplo permite la acción `content/write` solo en el repositorio `samples/hello-world`, `docker push` se realiza correctamente en ese repositorio, pero falla en `samples/nginx`:

```console
# docker push succeeds
docker pull myregistry.azurecr.io/samples/hello-world:v1

# docker push fails
docker pull myregistry.azurecr.io/samples/nginx:v1
```

## <a name="update-scope-map-and-token"></a>Actualización de la asignación de ámbito y el token

Para actualizar los permisos de token, actualice los permisos en la asignación de ámbito asociada mediante [az acr scope-map update][az-acr-scope-map-update]. Por ejemplo, para actualizar *MyScopeMap* con el fin de quitar la acción `content/write` en el repositorio `samples/hello-world`:

```azurecli
az acr scope-map update --name MyScopeMap --registry myregistry \
  --remove samples/hello-world content/write
```

Si la asignación de ámbito está asociada a más de un token, el comando actualiza el permiso de todos los tokens asociados.

Si desea actualizar un token con una asignación de ámbito diferente, ejecute [az acr token update][az-acr-token-update]. Por ejemplo:

```azurecli
az acr token update --name MyToken --registry myregistry \
  --scope-map MyNewScopeMap
```

Después de actualizar un token, o una asignación de ámbito asociada a un token, los cambios de permisos surten efecto en el siguiente inicio de sesión de Docker (`docker login`) u otra autenticación mediante el token.

Después de actualizar un token, es posible que desee generar contraseñas para acceder al registro. Ejecute [az acr token credential generate][az-acr-token-credential-generate]. Por ejemplo:

```azurecli
az acr token credential generate \
  --name MyToken --registry myregistry --days 30
```

## <a name="about-repository-scoped-permissions"></a>Acerca de los permisos orientados a repositorios

### <a name="concepts"></a>Conceptos

Para configurar los permisos orientados a repositorios, cree un *token de acceso* y una *asignación de ámbito* asociada con los comandos de la CLI de Azure.

* Un **token de acceso** es una credencial usada con una contraseña para autenticarse con el registro. Se permite asociar a cada token *acciones* orientadas a uno o varios repositorios. Puede establecer una fecha de expiración para cada token. 

* Las **acciones** en cada repositorio especificado incluyen una o varias de las siguientes opciones.

  |.  |DESCRIPCIÓN  |
  |---------|---------|
  |`content/read`     |  Lee datos del repositorio. Por ejemplo, extraer un artefacto.  |
  |`metadata/read`    | Lee metadatos del repositorio. Por ejemplo, enumerar etiquetas o mostrar metadatos de manifiestos.   |
  |`content/write`     |  Escribe datos en el repositorio. Úsela con `content/read` para enviar un artefacto.    |
  |`metadata/write`     |  Escribe metadatos en el repositorio. Por ejemplo, actualizar los atributos del manifiesto.  |
  |`content/delete`    | Quita datos del repositorio. Por ejemplo, eliminar un repositorio o un manifiesto. |

* Una **asignación de ámbito** es un objeto de registro que agrupa los permisos de repositorio que se aplican a un token o que se pueden volver a aplicar a otros tokens. Si no aplica una asignación de ámbito al crear un token, se crea automáticamente una asignación de ámbito para guardar la configuración de permisos. 

  Una asignación de ámbito lo ayuda a configurar varios usuarios con un acceso idéntico a un conjunto de repositorios. Azure Container Registry también proporciona asignaciones de ámbito definidos por el sistema que se pueden aplicar al crear tokens de acceso.

En la imagen siguiente se resume la relación entre los tokens y las asignaciones de ámbito. 

![Tokens y asignaciones de ámbito de registro](media/container-registry-repository-scoped-permissions/token-scope-map-concepts.png)

### <a name="scenarios"></a>Escenarios

Estos son algunos de los escenarios para usar un token de acceso:

* Proporcionar dispositivos IoT con tokens individuales para extraer una imagen de un repositorio.
* Proporcionar a una organización externa permisos a un repositorio específico. 
* Limite el acceso del repositorio a grupos de usuarios específicos de la organización. Por ejemplo, proporcione acceso de lectura y escritura a los desarrolladores que compilan imágenes destinadas a repositorios específicos, y acceso de lectura a los equipos que realizan implementaciones desde estos repositorios.

### <a name="authentication-using-token"></a>Autenticación mediante el token

Use un nombre de token como un nombre de usuario y una de sus contraseñas asociadas para autenticarse con el registro de destino. El método de autenticación depende de las acciones configuradas.

### <a name="contentread-or-contentwrite"></a>content/read o content/write

Si el token solo permite acciones `content/read` o `content/write`, proporcione credenciales de token en cualquiera de los flujos de autenticación siguientes:

* Autenticación con Docker mediante `docker login`
* Autenticación con el registro mediante el comando [az acr login][az-acr-login] de la CLI de Azure

Después de la autenticación, el token permite las acciones configuradas en los repositorios orientados. Por ejemplo, si el token permite la acción `content/read` en un repositorio, se permiten las operaciones `docker pull` en las imágenes de ese repositorio.

#### <a name="metadataread-metadatawrite-or-contentdelete"></a>metadata/read, metadata/write o content/delete

Si el token permite acciones `metadata/read`, `metadata/write` o `content/delete` en un repositorio, las credenciales de token deben proporcionarse como parámetros con los comandos [az acr repository][az-acr-repository] relacionados de la CLI de Azure.

Por ejemplo, si se permiten acciones `metadata/read` en un repositorio, pase las credenciales de token al ejecutar el comando [az acr repository show-tags][az-acr-repository-show-tags] para enumerar las etiquetas.

## <a name="next-steps"></a>Pasos siguientes

* Para administrar asignaciones de ámbito y tokens de acceso, use los comandos adicionales del grupo de comandos [az acr scope-map][az-acr-scope-map] y [az acr token][az-acr-token].
* Consulte la [introducción a la autenticación](container-registry-authentication.md) para los escenarios de autenticación con un registro de contenedor de Azure mediante una cuenta de administrador o una identidad de Azure Active Directory.


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository]: /cli/azure/acr/repository/
[az-acr-repository-show-tags]: /cli/azure/acr/repository/#az-acr-repository-show-tags
[az-acr-scope-map]: /cli/azure/acr/scope-map/
[az-acr-scope-map-create]: /cli/azure/acr/scope-map/#az-acr-scope-map-create
[az-acr-scope-map-update]: /cli/azure/acr/scope-map/#az-acr-scope-map-update
[az-acr-scope-map-list]: /cli/azure/acr/scope-map/#az-acr-scope-map-list
[az-acr-token]: /cli/azure/acr/token/
[az-acr-token-show]: /cli/azure/acr/token/#az-acr-token-show
[az-acr-token-create]: /cli/azure/acr/token/#az-acr-token-create
[az-acr-token-update]: /cli/azure/acr/token/#az-acr-token-update
[az-acr-token-credential-generate]: /cli/azure/acr/token/credential/#az-acr-token-credential-generate
