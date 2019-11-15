---
title: Configuración del servidor de configuración de Azure Spring Cloud | Microsoft Docs
description: En este tutorial aprenderá a configurar un servidor de configuración de Spring Cloud para Azure Spring Cloud en Azure Portal
ms.service: spring-cloud
ms.topic: tutorial
ms.author: jeconnoc
author: jpconnock
ms.date: 10/18/2019
ms.openlocfilehash: 6cf7b4a52ba3a7dbda5fa3fa558c4b68d09f4eb2
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73646727"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-for-your-service"></a>Tutorial: Configuración de un servidor de configuración de Spring Cloud para el servicio

En este tutorial se muestra cómo conectar un servidor de configuración de Spring Cloud al servicio Azure Spring Cloud.

Spring Cloud Config Server ofrece soporte técnico para el servidor y el cliente para las configuraciones externalizadas de un sistema distribuido. El servidor de configuración es una ubicación central donde puede administrar las propiedades externas de las aplicaciones en todos los entornos. Para más información, visite la [referencia de Spring Cloud Config Server](https://spring.io/projects/spring-cloud-config).

## <a name="prerequisites"></a>Requisitos previos
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 
* Un servicio de Azure Spring Cloud aprovisionado y en ejecución.  Complete [este inicio rápido](spring-cloud-quickstart-launch-app-cli.md) para aprovisionar e iniciar un servicio de Azure Spring Cloud.

## <a name="restriction"></a>Restricción

Hay algunas restricciones al usar __Config Server__ con un back-end de Git. Algunas propiedades se insertarán automáticamente en el entorno de la aplicación para acceder al __servidor de configuración__ y la __detección de servicios__. Si también configura esas propiedades desde los archivos del **servidor de configuración**, puede experimentar conflictos y un comportamiento inesperado. Estas propiedades incluyen: 

```yaml
eureka.client.service-url.defaultZone
eureka.client.tls.keystore
server.port
spring.cloud.config.tls.keystore
spring.application.name
```
> [!CAUTION]
> Se recomienda encarecidamente __NO__ colocar las propiedades anteriores en los archivos de aplicación del __servidor de configuración__.

## <a name="create-your-config-server-files"></a>Creación de los archivos del servidor de configuración

Azure Spring Cloud admite Azure DevOps, GitHub, GitLab y Bitbucket para almacenar los archivos del servidor de configuración. Cuando el repositorio esté listo, cree los archivos de configuración con las instrucciones siguientes y guárdelas ahí.

Además, algunas propiedades configurables solo están disponibles para algunos tipos. En las siguientes subsecciones se enumeran las propiedades de cada tipo de repositorio.

### <a name="public-repository"></a>Repositorio público

Al usar un repositorio público, las propiedades configurables son más limitadas.

A continuación se enumeran todas las propiedades configurables que se usan para configurar el repositorio público `Git`.

> [!NOTE]
> El uso de un guión ("-") para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, puede usar `default-label`, pero no `defaultLabel`.

| Propiedad        | Obligatorio | Característica                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | El `uri` del repositorio de `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `default-label` | `no`     | La etiqueta predeterminada del repositorio de `Git` debe ser el `branch name`, el `tag name` o el `commit-id` del repositorio. |
| `search-paths`  | `no`     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de `Git`. |

------

### <a name="private-repository-with-ssh-authentication"></a>Repositorio privado con autenticación SSH

A continuación se enumeran todas las propiedades configurables que se usan para configurar un repositorio privado `Git` con `Ssh`.

> [!NOTE]
> El uso de un guión ("-") para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, puede usar `default-label`, pero no `defaultLabel`.

| Propiedad                   | Obligatorio | Característica                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | `yes`    | El `uri` del repositorio de `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `default-label`            | `no`     | La etiqueta predeterminada del repositorio de `Git` debe ser el `branch name`, el `tag name` o el `commit-id` del repositorio. |
| `search-paths`             | `no`     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de `Git`. |
| `private-key`              | `no`     | Clave privada `Ssh` para acceder al repositorio de `Git`, __obligatoria__ cuando el `uri` empieza por `git@` o `ssh://`. |
| `host-key`                 | `no`     | La clave de host del servidor de repositorio de Git no debe incluir el prefijo del algoritmo, tal y como se describe en `host-key-algorithm`. |
| `host-key-algorithm`       | `no`     | El algoritmo de clave de host debe ser `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` o `ecdsa-sha2-nistp521`. Solo es obligatorio si `host-key` existe. |
| `strict-host-key-checking` | `no`     | Indica si el servidor de configuración no se iniciará al utilizar la `host-key` privada. Debería ser `true` (valor predeterminado) o `false`. |

-----

### <a name="private-repository-with-basic-authentication"></a>Repositorio privado con autenticación básica

A continuación se enumeran todas las propiedades configurables que se usan para configurar el repositorio de Git privado con autenticación básica.

> [!NOTE]
> El uso de un guión ("-") para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, use `default-label` o `defaultLabel`.

| Propiedad        | Obligatorio | Característica                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | El `uri` del repositorio de `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `default-label` | `no`     | La etiqueta predeterminada del repositorio de `Git` debe ser el `branch name`, el `tag name` o el `commit-id` del repositorio. |
| `search-paths`  | `no`     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de `Git`. |
| `username`      | `no`     | Valor de `username` que se usa para acceder al servidor del repositorio de `Git`, es __obligatorio__ cuando el servidor del repositorio de `Git` admite `Http Basic Authentication`. |
| `password`      | `no`     | Contraseña que se usa para acceder al servidor del repositorio de `Git`, es __obligatoria__ cuando el servidor del repositorio de `Git` admite `Http Basic Authentication`. |

> [!NOTE]
> Algunos servidores del repositorio de `Git`, como GitHub, admiten un token personal o de acceso como contraseña para `HTTP Basic Authentication`. También puede usar aquí ese tipo de token como contraseña y no expirarán ni el token personal ni el token de acceso. Sin embargo, en los servidores del repositorio de Git como BitBucket y Azure DevOps, el token expirará transcurrida una hora o dos. Por ello, que esta opción no es viable para Azure Spring Cloud.

### <a name="git-repositories-with-pattern"></a>Repositorios de Git con patrón

A continuación se enumeran todas las propiedades configurables que se usan para configurar los repositorios GIT con patrones.

> [!NOTE]
> El uso de un guión ("-") para separar palabras es la única convención de nomenclatura que se admite actualmente. Por ejemplo, use `default-label` o `defaultLabel`.

| Propiedad                           | Obligatorio         | Característica                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | `no`             | Asignación que consiste en la configuración de un repositorio de `Git` con un nombre determinado. |
| `repos."uri"`                      | `yes` en `repos` | El `uri` del repositorio de `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `repos."name"`                     | `yes` en `repos` | Nombre para identificar un repositorio de `Git`; es __obligatorio__ solo si existe `repos`. Por ejemplo, antes: `team-A` y `team-B`. |
| `repos."pattern"`                  | `no`             | Matriz de cadenas que se utiliza para coincidir con un nombre de aplicación. Para cada patrón, use el formato `{application}/{profile}` con caracteres comodín. |
| `repos."default-label"`            | `no`             | La etiqueta predeterminada del repositorio de `Git` debe ser el `branch name`, el `tag name` o el `commit-id` del repositorio. |
| `repos."search-paths`"             | `no`             | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio de `Git`. |
| `repos."username"`                 | `no`             | Valor de `username` que se usa para acceder al servidor del repositorio de `Git`, es __obligatorio__ cuando el servidor del repositorio de `Git` admite `Http Basic Authentication`. |
| `repos."password"`                 | `no`             | Contraseña que se usa para acceder al servidor del repositorio de `Git`, es __obligatoria__ cuando el servidor del repositorio de `Git` admite `Http Basic Authentication`. |
| `repos."private-key"`              | `no`             | Clave privada `Ssh` para acceder al repositorio `Git`, __obligatoria__ cuando el `uri` empieza por `git@` o `ssh://`. |
| `repos."host-key"`                 | `no`             | La clave de host del servidor de repositorio de Git no debe incluir el prefijo del algoritmo, tal y como se describe en `host-key-algorithm`. |
| `repos."host-key-algorithm"`       | `no`             | El algoritmo de clave de host debe ser `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` o `ecdsa-sha2-nistp521`. Solo es __obligatorio__ si existe `host-key`. |
| `repos."strict-host-key-checking"` | `no`             | Indica si el servidor de configuración no se iniciará al utilizar la `host-key` privada. Debería ser `true` (valor predeterminado) o `false`. |

## <a name="attaching-your-config-server-repository-to-azure-spring-cloud"></a>Conexión del repositorio del servidor de configuración a Azure Spring Cloud

Ahora que tiene los archivos de configuración guardados en un repositorio, debe conectar Azure Spring Cloud a él.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Vaya a la página de **Introducción a Azure Spring Cloud**.

1. Vaya a la pestaña **Config Server** del encabezado **Configuración** en el menú del lado izquierdo.

![captura de pantalla de la ventana](media/spring-cloud-tutorial-config-server/portal-config-server.png)

### <a name="input-repository-information-directly-to-the-azure-portal"></a>Introducción directa de la información de repositorio en Azure Portal

#### <a name="default-repository"></a>Repositorio predeterminado

* Repositorio público: En la sección **Repositorio predeterminado**, pegue el identificador URI del repositorio en la sección **URI**.  Establezca la **etiqueta** en `config`. Asegúrese de que la configuración **Autenticación** es **pública** y, después, seleccione **Aplicar** para finalizar. 

* Repositorio privado: Azure Spring Cloud admite la autenticación básica basada en contraseñas o tokens y SSH.

    * Autenticación básica: En la sección **Repositorio predeterminado**, pegue el identificador URI del repositorio en la sección **URI** y después haga clic en **Autenticación**. Seleccione **Básica** como **Tipo de autenticación** y escriba el nombre de usuario y la contraseña o el token para conceder acceso a Azure Spring Cloud. Haga clic en **Aceptar** y **Aplicar** para terminar de configurar el servidor de configuración.

    ![captura de pantalla de la ventana](media/spring-cloud-tutorial-config-server/basic-auth.png)
    
    > [!CAUTION]
    > Algunos servidores de repositorio de Git como GitHub usan un `personal-token` o un `access-token` como contraseña para la **Autenticación básica**. Puede usar ese tipo de token como contraseña en Azure Spring Cloud, ya que nunca expirará. Pero para otros servidores de repositorio de Git como BitBucket y Azure DevOps, el `access-token` expirará en una o dos horas. Esto significa que la opción no es viable cuando se usan estos servidores de repositorio con Azure Spring Cloud.

    * SSH: En la sección **Repositorio predeterminado**, pegue el identificador URI del repositorio en la sección **URI** y después haga clic en **Autenticación**. Seleccione **SSH** como **Tipo de autenticación** y escriba la **Clave privada**. También puede especificar su **Clave de host** y el **Algoritmo de clave de host**. Asegúrese de incluir la clave pública en el repositorio del servidor de configuración. Haga clic en **Aceptar** y **Aplicar** para terminar de configurar el servidor de configuración.

    ![captura de pantalla de la ventana](media/spring-cloud-tutorial-config-server/ssh-auth.png)

#### <a name="pattern-repository"></a>Repositorio de patrones

Si quiere usar un **Repositorio de patrones** opcional para configurar el servicio, especifique el **URI** y la **Autenticación** de la misma manera que el **Repositorio predeterminado**. Asegúrese de incluir un **Nombre** para el patrón y ´después haga clic en **Aplicar** para adjuntarlo a la instancia. 

### <a name="enter-repository-information-into-a-yaml-file"></a>Introducción de la información del repositorio en un archivo YAML

Si ha escrito un archivo YAML con la configuración del repositorio, puede importar el archivo YAML directamente desde el equipo local a Azure Spring Cloud. Un archivo YAML simple para un repositorio privado con autenticación básica tendrá un aspecto similar al siguiente:

```yml
spring:
    cloud:
        config:
            server:
                git:
                    uri: https://github.com/azure-spring-cloud-samples/config-server-repository.git
                    username: <username>
                    password: <password/token>

```

Haga clic en el botón **Importar configuración** y seleccione el archivo `.yml` en el directorio del proyecto. Haga clic en **Importar** y después aparecerán las **notificaciones** de una operación de `async`. Después de uno o dos minutos, debería informar de que se ha realizado correctamente.

![captura de pantalla de la ventana](media/spring-cloud-tutorial-config-server/local-yml-success.png)


La información del archivo YAML debería mostrarse en Azure Portal. Haga clic en **Aplicar** para finalizar el proceso. 


## <a name="delete-your-app-configuration"></a>Eliminación de la configuración de la aplicación

Una vez guardado un archivo de configuración, aparecerá el botón **Delete app configuration** (Eliminar configuración de la aplicación) en la pestaña **Configuración**. Esto borrará completamente la configuración existente. Debe hacerlo si desea conectar el servidor de configuración a otro origen, por ejemplo, al pasar de GitHub a Azure DevOps.



## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a habilitar y configurar el servidor de configuración. Para más información sobre la administración de la aplicación, continúe con el tutorial sobre el escalado manual de la aplicación.

> [!div class="nextstepaction"]
> [Aprenda a escalar manualmente la aplicación en Azure Spring Cloud](spring-cloud-tutorial-scale-manual.md).

