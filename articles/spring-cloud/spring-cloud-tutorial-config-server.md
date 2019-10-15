---
title: Configuración del servidor de configuración de Azure Spring Cloud | Microsoft Docs
description: En este tutorial aprenderá a configurar un servidor de configuración de Spring Cloud para Azure Spring Cloud en Azure Portal
services: spring-cloud
ms.service: spring-cloud
ms.topic: tutorial
ms.reviewer: jeconnoc
ms.author: v-vasuke
author: v-vasuke
ms.date: 08/08/2019
ms.openlocfilehash: f959d216859ded54de52de474ee3150f91bcfff1
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038657"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-for-your-service"></a>Tutorial: Configuración de un servidor de configuración de Spring Cloud para el servicio

En este tutorial se muestra cómo conectar un servidor de configuración de Spring Cloud al servicio Azure Spring Cloud.

Spring Cloud Config Server ofrece soporte técnico para el servidor y el cliente para las configuraciones externalizadas de un sistema distribuido. El servidor de configuración es una ubicación central donde puede administrar las propiedades externas de las aplicaciones en todos los entornos. Para más información, visite la [referencia de Spring Cloud Config Server](https://spring.io/projects/spring-cloud-config).

## <a name="prerequisites"></a>Requisitos previos
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 
* Un servicio de Azure Spring Cloud aprovisionado y en ejecución.  Complete [este inicio rápido](spring-cloud-quickstart-launch-app-cli.md) para aprovisionar e iniciar un servicio de Azure Spring Cloud.


## <a name="restriction"></a>Restricción

Hay algunas restricciones al usar __Config Server__ con un back-end de Git. Algunas propiedades se insertarán automáticamente en el entorno de la aplicación para acceder al __servidor de configuración__ y la __detección de servicios__. Si también configura esas propiedades desde los archivos del **servidor de configuración**, puede experimentar conflictos y un comportamiento inesperado.  Estas propiedades incluyen: 

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

Azure Spring Cloud admite Azure DevOps, GitHub, GitLab y Bitbucket para almacenar los archivos del servidor de configuración.  Cuando el repositorio esté listo, cree los archivos de configuración con las instrucciones siguientes y guárdelas ahí.

Además, algunas propiedades configurables solo están disponibles para algunos tipos. En las siguientes subsecciones se enumeran las propiedades de cada tipo de repositorio.


### <a name="public-repository"></a>Repositorio público

Al usar un repositorio público, las propiedades configurables son más limitadas.

A continuación se enumeran todas las propiedades configurables que se usan para configurar el repositorio público `Git`.

> [!NOTE]
> Por ahora solo se admite la convención de nomenclatura de palabras separadas por guiones ("-"). Por ejemplo, se usa `default-label`, pero no `defaultLabel`.

| Propiedad        | Obligatorio | Característica                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | El `uri` del repositorio `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `default-label` | `no`     | La etiqueta predeterminada del repositorio `Git` debe ser `branch name`, `tag name` o `commit-id` del repositorio. |
| `search-paths`  | `no`     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio `Git`. |

------

### <a name="private-repository-with-ssh-authentication"></a>Repositorio privado con autenticación SSH

A continuación se enumeran todas las propiedades configurables que se usan para configurar un repositorio privado `Git` con `Ssh`.

> [!NOTE]
> Por ahora solo se admite la convención de nomenclatura de palabras separadas por guiones ("-"). Por ejemplo, se usa `default-label`, pero no `defaultLabel`.

| Propiedad                   | Obligatorio | Característica                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | `yes`    | El `uri` del repositorio `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `default-label`            | `no`     | La etiqueta predeterminada del repositorio `Git` debe ser `branch name`, `tag name` o `commit-id` del repositorio. |
| `search-paths`             | `no`     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio `Git`. |
| `private-key`              | `no`     | Clave privada `Ssh` para acceder al repositorio `Git`, __obligatoria__ cuando el `uri` comienza por `git@` o `ssh://`. |
| `host-key`                 | `no`     | La clave de host del servidor de repositorio de Git no debe incluir el prefijo del algoritmo, tal y como se describe en `host-key-algorithm`. |
| `host-key-algorithm`       | `no`     | El algoritmo de clave de host debe ser uno de `ssh-dss`. `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` y `ecdsa-sha2-nistp521`. Solo es obligatorio si `host-key` existe. |
| `strict-host-key-checking` | `no`     | Indica si el servidor de configuración no se iniciará al aprovechar el valor de `host-key` proporcionado; debe ser `true` (valor predeterminado) o `false`. |

-----

### <a name="private-repository-with-basic-authentication"></a>Repositorio privado con autenticación básica

A continuación se enumeran todas las propiedades configurables que se usan para configurar el repositorio de Git privado con autenticación básica.

> [!NOTE]
> Por ahora solo se admite la convención de nomenclatura de palabras separadas por guiones ("-"). Por ejemplo, puede usar `default-label`, pero no `defaultLabel`.

| Propiedad        | Obligatorio | Característica                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | El `uri` del repositorio `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `default-label` | `no`     | La etiqueta predeterminada del repositorio `Git` debe ser `branch name`, `tag name` o `commit-id` del repositorio. |
| `search-paths`  | `no`     | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio `Git`. |
| `username`      | `no`     | Valor de `username` que se usa para acceder al servidor del repositorio de `Git`, el servidor del repositorio de `Git` __obligatorio__ admite `Http Basic Authentication`. |
| `password`      | `no`     | Contraseña que se usa para acceder al servidor del repositorio de `Git`, el servidor del repositorio de `Git` __obligatorio__ admite `Http Basic Authentication`. |

> [!NOTE]
> Algunos servidores del repositorio de `Git`, como GitHub, admiten un token personal o de acceso como contraseña para `HTTP Basic Authentication`. También puede usar aquí ese tipo de token como contraseña y no expirará. No obstante, en los servidores del repositorio de Git como BitBucket y Azure DevOps, el token expirará transcurrida una hora o dos, por lo que esta opción es inviable para Azure Spring Cloud.

### <a name="git-repositories-with-pattern"></a>Repositorios de Git con patrón

A continuación se enumeran todas las propiedades configurables que se usan para configurar los repositorios GIT con patrones.

> [!NOTE]
> Por ahora solo se admite la convención de nomenclatura de palabras separadas por guiones ("-"). Por ejemplo, puede usar `default-label`, pero no `defaultLabel`.

| Propiedad                           | Obligatorio         | Característica                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | `no`             | Una asignación está formada por configuraciones de los repositorios de `Git` con un nombre determinado. |
| `repos."uri"`                      | `yes` en `repos` | El `uri` del repositorio `Git` que sirve como back-end del servidor de configuración debe empezar por `http://`, `https://`, `git@` o `ssh://`. |
| `repos."name"`                     | `yes` en `repos` | Nombre para identificar un repositorio de `Git`; __obligatorio__ solo si existe `repos`. Por ejemplo, antes: `team-A` y `team-B`. |
| `repos."pattern"`                  | `no`             | Una matriz de cadena que se usa para encontrar el nombre de la aplicación; con cada patrón, usa el formato `{application}/{profile}` con caracteres comodín. |
| `repos."default-label"`            | `no`             | La etiqueta predeterminada del repositorio `Git` debe ser `branch name`, `tag name` o `commit-id` del repositorio. |
| `repos."search-paths`"             | `no`             | Matriz de cadenas que se usa para buscar en subdirectorios del repositorio `Git`. |
| `repos."username"`                 | `no`             | Valor de `username` que se usa para acceder al servidor del repositorio de `Git`, el servidor del repositorio de `Git` __obligatorio__ admite `Http Basic Authentication`. |
| `repos."password"`                 | `no`             | Contraseña que se usa para acceder al servidor del repositorio de `Git`, el servidor del repositorio de `Git` __obligatorio__ admite `Http Basic Authentication`. |
| `repos."private-key"`              | `no`             | Clave privada `Ssh` para acceder al repositorio `Git`, __obligatoria__ cuando el `uri` comienza por `git@` o `ssh://`. |
| `repos."host-key"`                 | `no`             | La clave de host del servidor de repositorio de Git no debe incluir el prefijo del algoritmo, tal y como se describe en `host-key-algorithm`. |
| `repos."host-key-algorithm"`       | `no`             | El algoritmo de clave de host debe ser uno de `ssh-dss`. `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` y `ecdsa-sha2-nistp521`. Solo es __obligatorio__ si existe `host-key`. |
| `repos."strict-host-key-checking"` | `no`             | Indica si el servidor de configuración no se iniciará al aprovechar el valor de `host-key` proporcionado; debe ser `true` (valor predeterminado) o `false`. |

### <a name="import-applicationyml-file-from-spring-cloud-config"></a>Importación del archivo `application.yml` desde Spring Cloud Config

Puede importar algunos valores de configuración del servidor de configuración predeterminados directamente desde el sitio web del [servidor de configuración de Spring Cloud](https://spring.io/projects/spring-cloud-config). Puede hacerlo directamente desde Azure Portal, por lo que no es necesario realizar ningún paso ahora para preparar los archivos o el repositorio del servidor de configuración.

## <a name="attaching-your-config-server-repository-to-azure-spring-cloud"></a>Conexión del repositorio del servidor de configuración a Azure Spring Cloud

Ahora que tiene los archivos de configuración guardados en un repositorio, debe conectar Azure Spring Cloud a él.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Vaya a la página de **Introducción a Azure Spring Cloud**.

1. Vaya a la pestaña **Config Server** del encabezado **Configuración** en el menú del lado izquierdo.

### <a name="public-repository"></a>Repositorio público

Si el repositorio es público, solo tiene que hacer clic en el botón **Público** y pegar la dirección URL.

### <a name="private-repository"></a>Repositorio privado

Azure Spring Cloud admite la autenticación SSH. Siga las instrucciones de Azure Portal para agregar la clave pública al repositorio. A continuación, asegúrese de incluir la clave privada en el archivo de configuración.

Haga clic en **Aplicar** para terminar de configurar el servidor de configuración.


## <a name="delete-your-app-configuration"></a>Eliminación de la configuración de la aplicación

Una vez guardado un archivo de configuración, aparecerá el botón **Delete app configuration** (Eliminar configuración de la aplicación) en la pestaña **Configuración**. Esto borrará completamente la configuración existente. Debe hacerlo si desea conectar el servidor de configuración a otro origen, por ejemplo, al pasar de GitHub a Azure DevOps.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a habilitar y configurar el servidor de configuración. Para más información sobre la administración de la aplicación, continúe con el tutorial sobre el escalado manual de la aplicación.

> [!div class="nextstepaction"]
> [Aprenda a escalar manualmente la aplicación en Azure Spring Cloud](spring-cloud-tutorial-scale-manual.md).

