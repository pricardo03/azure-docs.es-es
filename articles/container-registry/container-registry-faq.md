---
title: 'Azure Container Registry: preguntas más frecuentes'
description: Respuestas a las preguntas más frecuentes relacionadas con el servicio de Azure Container Registry
services: container-registry
author: sajayantony
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 5/13/2019
ms.author: sajaya
ms.openlocfilehash: 86efb6b655405500f994a5a5ec7acbd18c645004
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957856"
---
# <a name="frequently-asked-questions-about-azure-container-registry"></a>Preguntas más frecuentes sobre Azure Container Registry

Este artículo tratan las preguntas más frecuentes y problemas conocidos de Azure Container Registry.

## <a name="resource-management"></a>Administración de recursos

- [¿Puedo crear un Azure container registry mediante una plantilla de Resource Manager?](#can-i-create-an-azure-container-registry-using-a-resource-manager-template)
- [¿Hay una vulnerabilidad de seguridad buscando imágenes en ACR?](#is-there-security-vulnerability-scanning-for-images-in-acr)
- [¿Cómo configuro Kubernetes con Azure Container Registry?](#how-do-i-configure-kubernetes-with-azure-container-registry)
- [¿Cómo se puede obtener las credenciales de administrador para un registro de contenedor?](#how-do-i-get-admin-credentials-for-a-container-registry)
- [¿Cómo se puede obtener las credenciales de administrador en una plantilla de Resource Manager?](#how-do-i-get-admin-credentials-in-a-resource-manager-template)
- [Eliminación de la replicación se produce un error con el estado prohibido aunque se elimina la replicación mediante la CLI de Azure o Azure PowerShell](#delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell)

### <a name="can-i-create-an-azure-container-registry-using-a-resource-manager-template"></a>¿Puedo crear un Azure Container Registry mediante una plantilla de Resource Manager?

Sí. Aquí es [una plantilla](https://github.com/Azure/azure-cli/blob/master/src/command_modules/azure-cli-acr/azure/cli/command_modules/acr/template.json) que puede usar para crear un registro.

### <a name="is-there-security-vulnerability-scanning-for-images-in-acr"></a>¿Hay una vulnerabilidad de seguridad buscando imágenes en ACR?

Sí. Consulte la documentación del [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry/) y [Aqua](http://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry).

### <a name="how-do-i-configure-kubernetes-with-azure-container-registry"></a>¿Cómo configuro Kubernetes con Azure Container Registry?

Consulte la documentación de [Kubernetes](http://kubernetes.io/docs/user-guide/images/#using-azure-container-registry-acr) y pasos para [Azure Kubernetes Service](container-registry-auth-aks.md).

### <a name="how-do-i-get-admin-credentials-for-a-container-registry"></a>¿Cómo se puede obtener las credenciales de administrador para un registro de contenedor?

> [!IMPORTANT]
> La cuenta de usuario de administración está diseñada para que un solo usuario acceder al registro, principalmente para fines de prueba. No se recomienda compartir las credenciales de cuenta de administrador con varios usuarios. Se recomienda la identidad individual para usuarios y entidades de servicio para escenarios desatendidos. Consulte [Introducción a la autenticación](container-registry-authentication.md).

Antes de obtener las credenciales de administrador, asegúrese de que el usuario de administrador del registro está habilitado.

Para obtener las credenciales mediante la CLI de Azure:

```azurecli
az acr credential show -n myRegistry
```

Uso de Azure Powershell:

```powershell
Invoke-AzureRmResourceAction -Action listCredentials -ResourceType Microsoft.ContainerRegistry/registries -ResourceGroupName myResourceGroup -ResourceName myRegistry
```

### <a name="how-do-i-get-admin-credentials-in-a-resource-manager-template"></a>¿Cómo se puede obtener las credenciales de administrador en una plantilla de Resource Manager?

> [!IMPORTANT]
> La cuenta de usuario de administración está diseñada para que un solo usuario acceder al registro, principalmente para fines de prueba. No se recomienda compartir las credenciales de cuenta de administrador con varios usuarios. Se recomienda la identidad individual para usuarios y entidades de servicio para escenarios desatendidos. Consulte [Introducción a la autenticación](container-registry-authentication.md).

Antes de obtener las credenciales de administrador, asegúrese de que el usuario de administrador del registro está habilitado.

Para obtener la primera contraseña:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[0].value]"
}
```

Para obtener la segunda contraseña:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[1].value]"
}
```

### <a name="delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell"></a>Eliminación de la replicación se produce un error con el estado prohibido aunque se elimina la replicación mediante la CLI de Azure o Azure PowerShell

El error aparece cuando el usuario tiene permisos en un registro pero no tiene permisos de lectura en la suscripción. Para resolver este problema, asignar permisos de lectura sobre la suscripción al usuario:


```azurecli  
az role assignment create --role "Reader" --assignee user@contoso.com --scope /subscriptions/<subscription_id> 
```

## <a name="registry-operations"></a>Operaciones de registro

- [¿Cómo accedo a Docker Registry V2 de API HTTP?](#how-do-i-access-docker-registry-http-api-v2)
- [¿Cómo se puede eliminar todos los manifiestos que no se hace referencia por cualquier etiqueta en un repositorio?](#how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository)
- [¿Por qué no reducir el uso de cuota del registro después de eliminar imágenes?](#why-does-the-registry-quota-usage-not-reduce-after-deleting-images)
- [¿Cómo se puede validar los cambios de cuota de almacenamiento?](#how-do-i-validate-storage-quota-changes)
- [¿Cómo autenticar con mi registro cuando se ejecuta la CLI en un contenedor?](#how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container)
- [¿Azure Container Registry ofrece la única configuración de TLS v1.2 y cómo habilitar TLS v1.2?](#does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12)
- [¿Azure Container Registry admite contenido de confianza?](#does-azure-container-registry-support-content-trust)
- [¿Cómo se puede conceder acceso a las imágenes sin permiso para administrar el recurso del registro de inserción o extracción?](#how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource)
- [¿Cómo habilito cuarentena automática de imágenes para un registro](#how-do-i-enable-automatic-image-quarantine-for-a-registry)

### <a name="how-do-i-access-docker-registry-http-api-v2"></a>¿Cómo accedo a Docker Registry V2 de API HTTP?

ACR es compatible con Docker Registry V2 de API de HTTP. Pueden tener acceso a las API en `https://<your registry login server>/v2/`. Ejemplo: `https://mycontainerregistry.azurecr.io/v2/`

### <a name="how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository"></a>¿Cómo se puede eliminar todos los manifiestos que no se hace referencia por cualquier etiqueta en un repositorio?

Si se encuentra en bash:

```bash
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv  | xargs -I% az acr repository delete -n myRegistry -t myRepository@%
```

Para Powershell:

```powershell
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv | %{ az acr repository delete -n myRegistry -t myRepository@$_ }
```

Nota: Puede agregar `-y` en el comando de eliminación para omitir la confirmación.

Para obtener más información, consulte [eliminar imágenes de contenedor en Azure Container Registry](container-registry-delete.md).

### <a name="why-does-the-registry-quota-usage-not-reduce-after-deleting-images"></a>¿Por qué no reducir el uso de cuota del registro después de eliminar imágenes?

Esta situación puede ocurrir si las capas subyacentes son todavía se hace referencia a otras imágenes de contenedor. Si elimina una imagen con ninguna referencia, el uso del registro se actualiza en unos minutos.

### <a name="how-do-i-validate-storage-quota-changes"></a>¿Cómo se puede validar los cambios de cuota de almacenamiento?

Crear una imagen con una capa de 1GB mediante el siguiente archivo de docker. Esto garantiza que la imagen tiene una capa que no se comparte por cualquier otra imagen en el registro.

```dockerfile
FROM alpine
RUN dd if=/dev/urandom of=1GB.bin  bs=32M  count=32
RUN ls -lh 1GB.bin
```

Compile e inserte la imagen en el registro mediante la CLI de docker.

```bash
docker build -t myregistry.azurecr.io/1gb:latest .
docker push myregistry.azurecr.io/1gb:latest
```

Debe ser capaz de ver que ha aumentado el uso de almacenamiento en Azure portal, o bien puede consultar mediante la CLI de uso.

```bash
az acr show-usage -n myregistry
```

Elimine la imagen mediante la CLI de Azure o el portal y compruebe el uso actualizado en unos minutos.

```bash
az acr repository delete -n myregistry --image 1gb
```

### <a name="how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container"></a>¿Cómo autenticar con mi registro cuando se ejecuta la CLI en un contenedor?

Deberá ejecutar el contenedor de la CLI de Azure montando el socket de Docker:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock azuresdk/azure-cli-python:dev
```

En el contenedor, instalar `docker`:

```bash
apk --update add docker
```

A continuación, autenticarse con el registro:

```azurecli
az acr login -n MyRegistry
```

### <a name="does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12"></a>¿Azure Container Registry ofrece la única configuración de TLS v1.2 y cómo habilitar TLS v1.2?

Sí. Habilitar TLS mediante cualquier cliente reciente de docker (versión 18.03.0 y versiones posteriores). 

### <a name="does-azure-container-registry-support-content-trust"></a>¿Azure Container Registry admite contenido de confianza?

Sí, puede usar las imágenes de confianza en Azure Container Registry, ya que el [Docker notario](https://docs.docker.com/notary/getting_started/) se ha integrado y se puede habilitar. Para obtener más información, consulte [confiar en contenido en Azure Container Registry](container-registry-content-trust.md).


####  <a name="where-is-the-file-for-the-thumbprint-located"></a>¿Dónde está el archivo de la huella digital que se encuentra?

En `~/.docker/trust/tuf/myregistry.azurecr.io/myrepository/metadata`:

* Las claves públicas y los certificados de todas las funciones (excepto las funciones de delegación) se almacenan en el `root.json`.
* Las claves públicas y los certificados de la función de delegación se almacenan en el archivo JSON de su rol principal (por ejemplo `targets.json` para el `targets/releases` rol).

Se recomienda para comprobar los certificados y claves públicas después de la comprobación de TUF global que realiza el cliente de Docker y notario.

### <a name="how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource"></a>¿Cómo se puede conceder acceso a las imágenes sin permiso para administrar el recurso del registro de inserción o extracción?

Es compatible con ACR [roles personalizados](container-registry-roles.md) que proporcionan diferentes niveles de permisos. En concreto, `AcrPull` y `AcrPush` roles permiten a los usuarios a las imágenes de extracción o inserción sin el permiso para administrar el recurso del registro en Azure.

* Azure Portal: El registro -> Control de acceso (IAM) -> Agregar (seleccione `AcrPull` o `AcrPush` para el rol).
* CLI de Azure: Busque el identificador de recurso del registro, ejecute el comando siguiente:

  ```azurecli
  az acr show -n myRegistry
  ```
  
  A continuación, puede asignar el `AcrPull` o `AcrPush` rol a un usuario (en el ejemplo siguiente se usa `AcrPull`):

  ```azurecli
    az role assignment create --scope resource_id --role AcrPull --assignee user@example.com
    ```

  O bien, asigne el rol a una entidad de servicio identificada por su Id. de aplicación:

  ```
  az role assignment create --scope resource_id --role AcrPull --assignee 00000000-0000-0000-0000-000000000000
  ```

El encargado, a continuación, es capaz de autenticar y obtener acceso a las imágenes en el registro.

* Para autenticarse en un registro:
    
  ```azurecli
  az acr login -n myRegistry 
  ```

* Para enumerar los repositorios:

  ```azurecli
  az acr repository list -n myRegistry
  ```

 Para extraer una imagen:
    
  ```azurecli
  docker pull myregistry.azurecr.io/hello-world
  ```

Con el uso de solo el `AcrPull` o `AcrPush` rol, el usuario asignado no tiene permiso para administrar el recurso del registro en Azure. Por ejemplo, `az acr list` o `az acr show -n myRegistry` no mostrar el registro.

### <a name="how-do-i-enable-automatic-image-quarantine-for-a-registry"></a>¿Cómo habilitar la cuarentena automática de imágenes para un registro?

Cuarentena de imagen actualmente es una característica de vista previa de ACR. Puede habilitar el modo de cuarentena de un registro para que únicamente aquellas imágenes que se han completado correctamente el examen de seguridad son visibles para los usuarios normales. Para obtener más información, consulte el [repositorio de GitHub de ACR](https://github.com/Azure/acr/tree/master/docs/preview/quarantine).

## <a name="diagnostics"></a>Diagnóstico

- [se produce un error en la extracción de docker con el error: net/http: cancela la solicitud mientras se espera de conexión (Client.Timeout superado mientras se espera los encabezados)](#docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers)
- [inserción de docker se ejecuta correctamente pero se produce un error en la extracción de docker con el error: no autorizado: se requiere autenticación](#docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required)
- [Habilitar y obtener los registros de depuración del demonio de docker](#enable-and-get-the-debug-logs-of-the-docker-daemon) 
- [Nuevos permisos de usuario no pueden ser efectivos inmediatamente después de actualizar](#new-user-permissions-may-not-be-effective-immediately-after-updating)
- [No se proporciona información de autenticación en el formato correcto en las llamadas de API de REST directas](#authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls)
- [¿Por qué no mostrar el portal de Azure todos los repositorios o Mis etiquetas?](#why-does-the-azure-portal-not-list-all-my-repositories-or-tags)

### <a name="docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers"></a>se produce un error en la extracción de docker con el error: net/http: cancela la solicitud mientras se espera de conexión (Client.Timeout superado mientras se espera los encabezados)

 - Si este error es un problema transitorio, reintento se realizará correctamente. 
 - Si `docker pull` produce un error continuamente, podría haber un problema con el demonio de docker. Por lo general se puede mitigar el problema, reinicie el demonio de docker. 
 - Si continúa apareciendo este problema después de reiniciar el demonio de docker, el problema podría ser algunos problemas de conectividad de red con la máquina. Para comprobar si está en buen estado general de la red en el equipo, probar un comando como `ping www.bing.com`.
 - Siempre debe tener un mecanismo de reintento en todas las operaciones de cliente de docker.

### <a name="docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required"></a>inserción de docker se ejecuta correctamente pero se produce un error en la extracción de docker con el error: no autorizado: se requiere autenticación

Este error puede producirse con la versión de Red Hat de demonio de docker, donde `--signature-verification` está habilitada de forma predeterminada. Puede comprobar las opciones de demonio de docker para Red Hat Enterprise Linux (RHEL) o Fedora, ejecute el comando siguiente:

```bash
grep OPTIONS /etc/sysconfig/docker
```

Por ejemplo, Fedora 28 servidor tiene las siguientes opciones de demonio de docker:

```
OPTIONS='--selinux-enabled --log-driver=journald --live-restore'
```

Con `--signature-verification=false` falta, `docker pull` produce un error similar al:

```bash
Trying to pull repository myregistry.azurecr.io/myimage ...
unauthorized: authentication required
```

Para resolver el error:
1. Agregue la opción `--signature-verification=false` al archivo de configuración del demonio de docker `/etc/sysconfig/docker`. Por ejemplo:

  ```
  OPTIONS='--selinux-enabled --log-driver=journald --live-restore --signature-verification=false'
  ```
2. Reinicie el servicio de demonio de docker, ejecute el comando siguiente:

  ```bash
  sudo systemctl restart docker.service
  ```

Detalles de `--signature-verification` puede encontrarse ejecutando `man dockerd`.

### <a name="enable-and-get-the-debug-logs-of-the-docker-daemon"></a>Habilitar y obtener los registros de depuración del demonio de docker  

Iniciar `dockerd` con el `debug` opción. En primer lugar, cree el archivo de configuración del demonio de docker (`/etc/docker/daemon.json`) si no existe y agregar el `debug` opción:

```json
{   
    "debug": true   
}
```

A continuación, reinicie el demonio. Por ejemplo, con Ubuntu 14.04:

```bash
sudo service docker restart
```

Encontrará detalles en el [documentación de Docker](https://docs.docker.com/engine/admin/#enable-debugging). 

 * Los registros pueden generarse en ubicaciones diferentes, dependiendo de su sistema. Por ejemplo, para Ubuntu 14.04, tiene `/var/log/upstart/docker.log`.   
Consulte [documentación de Docker](https://docs.docker.com/engine/admin/#read-the-logs) para obtener más información.    

 * Docker para Windows, los registros se generan bajo % LOCALAPPDATA%/docker/. Sin embargo no puede contener todavía toda la información de depuración.   

   Con el fin de obtener acceso al registro de demonio completa, puede que necesite algunos pasos adicionales:

    ```console
    docker run --privileged -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker alpine sh

    docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
    chroot /host
    ```
    Ahora que tiene acceso a todos los archivos de la máquina virtual en ejecución `dockerd`. El registro está en `/var/log/docker.log`.

### <a name="new-user-permissions-may-not-be-effective-immediately-after-updating"></a>Nuevos permisos de usuario no pueden ser efectivos inmediatamente después de actualizar

Al conceder nuevos permisos (nuevos roles) a una entidad de servicio, el cambio podrían no surtirán efecto inmediatamente. Hay dos razones posibles:

* Retraso de asignación de rol de Azure Active Directory. Normalmente es rápida, pero puede tardar minutos debido a un retraso de propagación.
* Retraso de permiso en el servidor de tokens de ACR. Esto puede tardar hasta 10 minutos. Para mitigar, también puede `docker logout` y, a continuación, autenticarse de nuevo con el mismo usuario después de un minuto:

  ```bash
  docker logout myregistry.azurecr.io
  docker login myregistry.azurecr.io
  ```

Actualmente ACR no admite la eliminación de la replicación principal por los usuarios. La solución consiste en incluir la replicación principal crea en la plantilla, pero omitir su creación mediante la adición de `"condition": false` tal como se muestra a continuación:

```json
{
    "name": "[concat(parameters('acrName'), '/', parameters('location'))]",
    "condition": false,
    "type": "Microsoft.ContainerRegistry/registries/replications",
    "apiVersion": "2017-10-01",
    "location": "[parameters('location')]",
    "properties": {},
    "dependsOn": [
        "[concat('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
     ]
},
```

### <a name="authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls"></a>No se proporciona información de autenticación en el formato correcto en las llamadas de API de REST directas

Puede encontrar un `InvalidAuthenticationInfo` error, utilizando especialmente el `curl` herramienta con la opción `-L`, `--location` (seguir redirecciones).
Por ejemplo, capturar el blob mediante `curl` con `-L` opción y la autenticación básica:

```bash
curl -L -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest
```

es posible que en la siguiente respuesta:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Error><Code>InvalidAuthenticationInfo</Code><Message>Authentication information is not given in the correct format. Check the value of Authorization header.
RequestId:00000000-0000-0000-0000-000000000000
Time:2019-01-01T00:00:00.0000000Z</Message></Error>
```

La causa raíz es que algunos `curl` implementaciones seguir redirecciones con encabezados de la solicitud original.

Para resolver el problema, deberá seguir redirecciones manualmente sin los encabezados. Imprimir los encabezados de respuesta con el `-D -` opción de `curl` y, a continuación, extraiga: el `Location` encabezado:

```bash
redirect_url=$(curl -s -D - -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest | grep "^Location: " | cut -d " " -f2 | tr -d '\r')
curl $redirect_url
```

### <a name="why-does-the-azure-portal-not-list-all-my-repositories-or-tags"></a>¿Por qué no mostrar el portal de Azure todos los repositorios o Mis etiquetas? 

Si usa el explorador Microsoft Edge, puede ver como máximo 100 repositorios o las etiquetas que aparecen. Si el registro tiene más de 100 repositorios o etiquetas, se recomienda que use el explorador Firefox o Chrome para enumerar todos ellos.

## <a name="tasks"></a>Tareas

- [¿Cómo procesar por lotes Cancelar ejecuciones?](#how-do-i-batch-cancel-runs)
- [¿Cómo se puede incluir la carpeta .git en el comando de az acr build?](#how-do-i-include-the-git-folder-in-az-acr-build-command)

### <a name="how-do-i-batch-cancel-runs"></a>¿Cómo procesar por lotes Cancelar ejecuciones?

Los siguientes comandos cancelar todas las tareas en ejecución en el registro especificado.

```azurecli
az acr task list-runs -r $myregistry --run-status Running --query '[].runId' -o tsv \
| xargs -I% az acr task cancel-run -r $myregistry --run-id %
```

### <a name="how-do-i-include-the-git-folder-in-az-acr-build-command"></a>¿Cómo se puede incluir la carpeta .git en el comando de az acr build?

Si se pasa una carpeta de origen local para la `az acr build` comando, el `.git` carpeta se excluye del paquete cargado de forma predeterminada. Puede crear un `.dockerignore` archivo con la siguiente configuración. Indica el comando para restaurar todos los archivos de `.git` en el paquete cargado. 

```
!.git/**
```

Esta configuración también se aplica a la `az acr run` comando.

## <a name="cicd-integration"></a>Integración de CI/CD

- [CircleCI](https://github.com/Azure/acr/blob/master/docs/integration/CircleCI.md)
- [Acciones de GitHub](https://github.com/Azure/acr/blob/master/docs/integration/github-actions/github-actions.md)


## <a name="next-steps"></a>Pasos siguientes

* [Obtenga más información](container-registry-intro.md) sobre Azure Container Registry.
