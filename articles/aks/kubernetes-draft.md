---
title: Uso de Draft con AKS y Azure Container Registry
description: Uso de Draft con AKS y Azure Container Registry
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: zarhoads
ms.openlocfilehash: 462cfd6ec0a6b25f85dda0245dd4f5feed7cb712
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755659"
---
# <a name="use-draft-with-azure-kubernetes-service-aks"></a>Uso de Draft con Azure Kubernetes Service (AKS)

Draft es una herramienta de código abierto que ayuda a empaquetar e implementar contenedores de aplicaciones en un clúster de Kubernetes, lo que le permite concentrarse en el ciclo de desarrollo: el "bucle interno" del desarrollo concentrado. Draft trabaja durante el desarrollo del código y antes de la confirmación en el control de versiones. Con Draft, puede reimplementar rápidamente una aplicación en Kubernetes cuando se producen cambios en el código. Para obtener más información sobre Draft, consulte la [documentación de Draft en GitHub][draft-documentation].

En este artículo se muestra cómo usar Draft con un clúster de Kubernetes en AKS.

## <a name="prerequisites"></a>Requisitos previos

En los pasos que se detallan en este artículo se da por hecho que ha creado un clúster de AKS y que ha establecido una conexión `kubectl` con dicho clúster. Si necesita estos elementos, vea la [Guía de inicio rápido de AKS][aks-quickstart].

Necesita un registro de Docker privado en Azure Container Registry (ACR). Para conocer los pasos a seguir para la creación de una instancia de ACR, consulte la [Guía de inicio rápido de Azure Container Registry][acr-quickstart].

Helm también debe instalarse en el clúster AKS. Para más información acerca de cómo instalar y configurar Helm, consulte [Uso de Helm con Azure Kubernetes Service (AKS)][aks-helm].

Por último, debe instalar [Docker](https://www.docker.com).

## <a name="install-draft"></a>Instalación de Draft

La CLI de Draft es un cliente que se ejecuta en su sistema de desarrollo, y que permite implementar código en un clúster de Kubernetes. Para instalar la CLI de Draft en un equipo Mac, use `brew`. Para conocer otras opciones de instalación, consulte la [Guía de instalación de Draft][draft-documentation].

> [!NOTE]
> Si tiene instalada una versión de Draft anterior a la 0.12, primero elimine Draft del clúster mediante `helm delete --purge draft` y, a continuación, quitar la configuración local ejecutando `rm -rf ~/.draft`. Si se encuentra en Mac OS, ejecute `brew upgrade draft`.

```console
brew tap azure/draft
brew install draft
```

Ahora, inicialice Draft con el comando `draft init`:

```console
draft init
```

## <a name="configure-draft"></a>Configuración de Draft

Draft crea las imágenes de contenedor de forma local para, posteriormente, implementarlas desde el registro local (como en el caso de Minikube), o utilizar un registro de la imagen que el usuario haya especificado. Este artículo usa Azure Container Registry (ACR), por lo que tiene que establecer una relación de confianza entre el clúster de AKS y el registro de ACR y configurar Draft para que inserte las imágenes de contenedor en ACR.

### <a name="create-trust-between-aks-cluster-and-acr"></a>Creación de relación de confianza entre el clúster de AKS y ACR

Para establecer la confianza entre un clúster de AKS y un registro de ACR, conceda permisos para la entidad de servicio de Azure Active Directory que utiliza el clúster AKS para acceder al registro ACR. En los siguientes comandos, proporcione su propio `<resourceGroupName>`, reemplace `<aksName>` con el nombre de su clúster de AKS y `<acrName>` con el nombre del registro de ACR:

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group <resourceGroupName> --name <aksName> --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group <resourceGroupName> --name <acrName> --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

Para más información acerca de estos pasos para acceder a ACR, consulte [autenticación con ACR](../container-registry/container-registry-auth-aks.md).

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Configuración de Draft para que inserte en ACR e implemente desde este

Ahora que hay una relación de confianza entre AKS y ACR, habilite el uso de ACR desde el clúster de AKS.

1. Establezca el valor de *registro* de la configuración de Draft. En los siguientes comandos, reemplace `<acrName>` con el nombre del registro de ACR:

    ```console
    draft config set registry <acrName>.azurecr.io
    ```

1. Inicie sesión en el registro de ACR con [az acr login][az-acr-login]:

    ```azurecli
    az acr login --name <acrName>
    ```

Como se ha creado una relación de confianza entre AKS y ACR, no es necesario utilizar ni contraseñas ni secretos para insertar en el registro de ACR o extraer desde el mismo. La autenticación se produce en el nivel de Azure Resource Manager mediante Azure Active Directory.

## <a name="run-an-application"></a>Ejecución de una aplicación

Para ver Draft en acción, vamos a implementar una aplicación de ejemplo desde el [repositorio de Draft][draft-repo]. Primero, clone el repositorio:

```console
git clone https://github.com/Azure/draft
```

Cambie al directorio de ejemplos de Java:

```console
cd draft/examples/example-java/
```

Use el comando `draft create` para iniciar el proceso. Este comando crea los artefactos que se usan para ejecutar la aplicación en un clúster de Kubernetes. Estos elementos incluyen un Dockerfile, un gráfico de Helm y un archivo *draft.toml*, que es el archivo de configuración de Draft.

```
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

Para ejecutar la aplicación de ejemplo en el clúster de AKS, use el comando `draft up`. Este comando compila el Dockerfile para crear una imagen de contenedor, inserta la imagen en ACR y, finalmente, instala el gráfico de Helm para iniciar la aplicación en AKS.

La primera vez que se ejecuta este comando, insertar y extraer la imagen de contenedor pueden tardar algún tiempo. Una vez que los niveles de base se almacenan en caché, el tiempo necesario para implementar la aplicación se reduce drásticamente.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

Si encuentra problemas al insertar la imagen de Docker, asegúrese de que haya iniciado sesión correctamente en el registro de ACR con [az acr login][az-acr-login], y luego vuelva a intentarlo con el comando `draft up`.

## <a name="test-the-application-locally"></a>Prueba de la aplicación de forma local

Para probar la aplicación, use el comando `draft connect`. Este comando redirige una conexión segura mediante proxy al pod de Kubernetes. Al terminar, se puede acceder a la aplicación en la dirección URL proporcionada.

> [!NOTE]
> Es posible que se tarde unos minutos en descargar la imagen del contenedor e iniciar la aplicación. Si recibe un error al acceder a la aplicación, vuelva a intentar la conexión.

```
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Para obtener acceso a la aplicación, abra un explorador web a la dirección y el puerto especificado en el `draft connect` salida, como `http://localhost:49804`. 

![Aplicación de Java de ejemplo que se ejecuta con Draft](media/kubernetes-draft/sample-app.png)

Use `Control+C` para detener la conexión del proxy.

> [!NOTE]
> También puede usar el comando `draft up --auto-connect` para compilar e implementar la aplicación y conectarse inmediatamente al primer contenedor en ejecución.

## <a name="access-the-application-on-the-internet"></a>Acceso a la aplicación en Internet

El paso anterior creó una conexión de proxy al pod de la aplicación en el clúster AKS. A medida que desarrolle y pruebe la aplicación, es posible que quiera que esté disponible en Internet. Para exponer una aplicación en Internet, cree un servicio de Kubernetes con un tipo de [LoadBalancer][kubernetes-service-loadbalancer], o cree un [controlador de entrada][kubernetes-ingress]. Vamos a crear un servicio *LoadBalancer*.

En primer lugar, actualice el paquete de Draft *Values.yaml* para especificar que debe crearse un servicio con un tipo *LoadBalancer*:

```console
vi charts/java/values.yaml
```

Busque la propiedad *service.type* y actualice el valor de *ClusterIP* a *LoadBalancer*, tal y como se muestra en el siguiente ejemplo reducido:

```yaml
[...]
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
[...]
```

Guarde y cierre el archivo y luego use `draft up` para volver a ejecutar la aplicación:

```console
draft up
```

El servicio puede tardar unos minutos en devolver una dirección IP pública. Para supervisar el progreso, utilice el comando `kubectl get service` con el parámetro *watch*:

```console
kubectl get service --watch
```

En un primer momento, el parámetro *EXTERNAL-IP* del servicio aparece como *pendiente*:

```
NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
```

Una vez que la dirección EXTERNAL-IP haya cambiado de *pendiente* a una dirección IP, use `Control+C` para detener el proceso de inspección de `kubectl`:

```
NAME                TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Para ver la aplicación, navegue hasta la dirección IP externa del equilibrador de carga con `curl`:

```
$ curl 52.175.224.118

Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Iteración en la aplicación

Ahora que Draft está configurado y que la aplicación se ejecuta en Kubernetes, está preparado para la iteración de código. Cada vez que quiera probar el código actualizado, ejecute el comando `draft up` para actualizar la aplicación en ejecución.

En este ejemplo, actualice la aplicación de ejemplo de Java para cambiar el texto para mostrar. Abra el archivo *Hello.java*:

```console
vi src/main/java/helloworld/Hello.java
```

Actualice el texto de salida para mostrar, *Hola mundo, soy Java en AKS!*:

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
}
```

Ejecute el comando `draft up` para volver a implementar la aplicación:

```console
$ draft up

Draft Up Started: 'example-java': 01CMZC9RF0TZT7XPWGFCJE15X4
example-java: Building Docker Image: SUCCESS ⚓  (25.0202s)
example-java: Pushing Docker Image: SUCCESS ⚓  (7.1457s)
example-java: Releasing Application: SUCCESS ⚓  (3.5773s)
Inspect the logs with `draft logs 01CMZC9RF0TZT7XPWGFCJE15X4`
```

Para ver la aplicación actualizada, vuelva a enrollar la dirección IP del equilibrador de carga:

```
$ curl 52.175.224.118

Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el uso de Draft, consulte la documentación de Draft en GitHub.

> [!div class="nextstepaction"]
> [Documentación de Draft][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[draft-repo]: https://github.com/Azure/draft

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[kubernetes-ingress]: ./ingress-basic.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[az-acr-login]: /cli/azure/acr#az-acr-login
