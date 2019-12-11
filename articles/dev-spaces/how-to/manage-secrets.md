---
title: Administración de secretos cuando se trabaja con un espacio de Azure Dev Spaces
services: azure-dev-spaces
ms.date: 12/03/2019
ms.topic: conceptual
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contenedores
ms.openlocfilehash: b184f72dfbbfe093443ab8a9b79bafbece3a3d51
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74790173"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Administración de secretos cuando se trabaja con un espacio de Azure Dev Spaces

Los servicios pueden requerir algunas contraseñas, cadenas de conexión y otros secretos, como para las bases de datos u otros servicios de Azure seguros. Al establecer los valores de estos secretos en archivos de configuración, puede que estén disponibles en el código como variables de entorno.  Estos se deben administrar con cuidado para evitar poner en peligro la seguridad de los secretos.

Azure Dev Spaces proporciona dos opciones optimizadas recomendadas para almacenar secretos en gráficos de Helm generados por las herramientas de cliente de Azure Dev Spaces: en el archivo `values.dev.yaml` y en línea directamente en `azds.yaml`. No se recomienda almacenar secretos en `values.yaml`. Aparte de los dos enfoques para gráficos de Helm generados por las herramientas de cliente definidos en este artículo, si crea su propio gráfico de Helm, podrá usarlo directamente para administrar y almacenar secretos.

## <a name="method-1-valuesdevyaml"></a>Método 1: values.dev.yaml
1. Abra VS Code con el proyecto que está habilitado para Azure Dev Spaces.
2. Agregue un archivo denominado _values.dev.yaml_ en la misma carpeta que el archivo _azds.yaml_ existente y defina la clave secreta y los valores, como en el ejemplo siguiente:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. _azds.yaml_ ya hace referencia al archivo _values.dev.yaml_ si existe. Si prefiere un nombre de archivo diferente, actualice la sección install.values:

    ```yaml
    install:
      values:
      - values.dev.yaml?
      - secrets.dev.yaml?
    ```
 
4. Modifique el código de servicio para hacer referencia a estos secretos como variables de entorno, como en el ejemplo siguiente:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Actualice los servicios que se ejecutan en el clúster con estos cambios. En la línea de comandos ejecute el comando:

    ```
    azds up
    ```
 
6. (Opcional) En la línea de comandos, compruebe que se han creado estos secretos:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Asegúrese de que agrega _values.dev.yaml_ al archivo _.gitignore_ para evitar la confirmación de los secretos en el control de código fuente.
 
 
## <a name="method-2-azdsyaml"></a>Método 2: azds.yaml
1.  En _azds.yaml_, establezca los secretos en la sección de yaml configurations/develop/install. Aunque puede especificar valores de secreto directamente, no se recomienda porque _azds.yaml_ está protegido en el control de código fuente. En su lugar, agregue los marcadores de posición utilizando la sintaxis de "$PLACEHOLDER".

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT"
                host: "$REDIS_HOST"
                key: "$REDIS_KEY"
    ```
     
2.  Cree un archivo _.env_ en la misma carpeta que _azds.yaml_. Escriba secretos con clave estándar= notación de valor. No confirme el archivo _.env_ en el control de código fuente. (Para omitir el control de código fuente en sistemas de control de versiones basados en git, agréguelo al archivo _.gitignore_). En el ejemplo siguiente se muestra un archivo _.env_:

    ```
    REDIS_PORT=3333
    REDIS_HOST=myredishost
    REDIS_KEY=myrediskey
    ```
2.  Modifique el código fuente del servicio para hacer referencia a estos secretos en el código, como en el ejemplo siguiente:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Actualice los servicios que se ejecutan en el clúster con estos cambios. En la línea de comandos, ejecute el comando:

    ```
    azds up
    ```

4.  (opcional) Ver secretos de kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="passing-secrets-as-build-arguments"></a>Pasar secretos como argumentos de compilación

En las secciones anteriores se ha mostrado cómo pasar secretos para usarlos en el tiempo de ejecución del contenedor. También puede pasar un secreto en el tiempo de compilación del contenedor, como una contraseña para un NuGet privado, mediante `azds.yaml`.

En `azds.yaml`, establezca los secretos de tiempo de compilación en *configurations.develop.build.args* con la sintaxis `<variable name>: ${secret.<secret name>.<secret key>}`. Por ejemplo:

```yaml
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
        MYTOKEN: ${secret.mynugetsecret.pattoken}
```

En el ejemplo anterior, *mynugetsecret* es un secreto existente y *pattoken* es una clave existente.

>[!NOTE]
> Los nombres de secretos y las claves pueden contener el carácter `.`. Use `\` para escapar `.` al pasar secretos como argumentos de compilación. Por ejemplo, para pasar un secreto denominado *foo.bar* con la clave de *token*: `MYTOKEN: ${secret.foo\.bar.token}`. Además, los secretos se pueden evaluar con texto de prefijo y sufijo. Por ejemplo, `MYURL: eus-${secret.foo\.bar.token}-version1`. Asimismo, los secretos disponibles en espacios primarios y principales se pueden pasar como argumentos de compilación.

En el archivo Dockerfile, use la directiva *ARG* para consumir el secreto y, después, use esa misma variable más adelante en el archivo Dockerfile. Por ejemplo:

```dockerfile
...
ARG MYTOKEN
...
ARG NUGET_EXTERNAL_FEED_ENDPOINTS="{'endpointCredentials': [{'endpoint':'PRIVATE_NUGET_ENDPOINT', 'password':'${MYTOKEN}'}]}"
...
```

Actualice los servicios que se ejecutan en el clúster con estos cambios. En la línea de comandos, ejecute el comando:

```
azds up
```

## <a name="next-steps"></a>Pasos siguientes

Con estos métodos, ahora puede conectarse de forma segura a una base de datos, a una instancia de Azure Cache for Redis o acceder a los servicios de Azure seguros.
 
