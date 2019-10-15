---
title: Inicio de una aplicación Java Spring mediante la CLI de Azure
description: En este inicio rápido se implementa una aplicación de ejemplo en Azure Spring Cloud en la CLI de Azure.
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/04/2019
ms.author: v-vasuke
ms.openlocfilehash: 6d399f04015140477af17f718c3e2205b8c3855f
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170551"
---
# <a name="quickstart-launch-a-java-spring-application-using-the-azure-cli"></a>Inicio rápido: Inicio de una aplicación Java Spring mediante la CLI de Azure

Azure Spring Cloud permite ejecutar fácilmente en Azure una aplicación de microservicios basada en Spring Boot.

En este inicio rápido se muestra cómo implementar una aplicación de Java Spring Cloud existente en Azure. Cuando haya terminado, puede seguir administrando la aplicación mediante la CLI de Azure o cambiar a usar Azure Portal.

En este inicio rápido aprenderá a:

> [!div class="checklist"]
> * Aprovisionar una instancia de servicio
> * Establecer un servidor de configuración para una instancia
> * Compilar una aplicación de microservicios localmente
> * Implementar cada microservicio
> * Asignar un punto de conexión público para la aplicación

## <a name="prerequisites"></a>Requisitos previos

>[!Note]
> Antes de comenzar este inicio rápido, asegúrese de que la suscripción de Azure tiene acceso a Azure Spring Cloud.  Al ser un servicio en versión preliminar, le pedimos que se ponga en contacto con nosotros para que podamos agregar su suscripción a la lista de permitidos.  Si desea explorar las funcionalidades de Azure Spring Cloud, [rellene este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u
).

>[!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo.  Incluye herramientas comunes de Azure preinstaladas, entre las que se incluyen las versiones más recientes de Git, JDK, Maven y la CLI de Azure. Si ha iniciado sesión en su suscripción de Azure, inicie [Azure Cloud Shell](https://shell.azure.com) desde shell.azure.com.  Para más información acerca de Azure Cloud Shell, [lea la documentación](../cloud-shell/overview.md).

Para completar esta guía de inicio rápido:

1. [Instalación de Git](https://git-scm.com/)
2. [Instalación de JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Instalación de Maven 3.0, o cualquier versión superior](https://maven.apache.org/download.cgi)
4. [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Registro para obtener una suscripción a Azure](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Instalación de la extensión de la CLI de Azure

Instale la extensión de Azure Spring Cloud para la CLI de Azure, para lo cual debe usar el siguiente comando

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-cli"></a>Aprovisionamiento de una instancia de servicio en la CLI de Azure

1. Inicie sesión en la CLI de Azure y elija una suscripción activa. Asegúrese de elegir la suscripción activa que se encuentre en la lista de permitidos para Azure Spring Cloud

    ```azurecli
        az login
        az account list -o table
        az account set --subscription
    ```

2. Prepare un nombre para el servicio Azure Spring Cloud.  El nombre debe tener entre 4 y 32 caracteres, y solo puede contener números, letras minúsculas y guiones.  El primer carácter del nombre del servicio debe ser una letra y el último debe ser una letra o un número.

3. Cree un grupo de recursos que contenga el servicio Azure Spring Cloud.

    ```azurecli
        az group create --location eastus --name <resource group name>
    ```
    Más información sobre los [grupos de recursos de Azure](../azure-resource-manager/resource-group-overview.md).

4. Abra una ventana de la CLI de Azure y ejecute los siguientes comandos para aprovisionar una instancia de Azure Spring Cloud.

    ```azurecli
        az spring-cloud create -n <service name> -g <resource group name>
    ```

    La instancia de servicio tardará aproximadamente cinco minutos en implementarse.

5. Establezca el nombre del grupo de recursos y el nombre del clúster predeterminados con los siguientes comandos:

    ```azurecli
        az configure --defaults group=<service group name>
        az configure --defaults spring-cloud=<service instance name>
    ```

## <a name="setup-your-configuration-server"></a>Configuración de un servidor de configuración

Actualice config-server con la ubicación del repositorio de Git de nuestro proyecto:

```git
az spring-cloud config-server git set -n <your-service-name> --uri https://github.com/Azure-Samples/piggymetrics --label config
```

## <a name="build-the-microservices-applications-locally"></a>Compilación local de aplicaciones de microservicios

1. Cree una carpeta y clone el repositorio de la aplicación de ejemplo en su cuenta de la nube de Azure.  

    ```azurecli
        mkdir source-code
        git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Cambie de directorio y compile el proyecto.

    ```azurecli
        cd PiggyMetrics
        mvn clean package -D skipTests
    ```

La compilación del proyecto tarda aproximadamente 5 minutos.  Una vez completado, debe tener archivos JAR individuales para cada servicio en sus respectivas carpetas.

## <a name="create-the-microservices"></a>Creación de los microservicios

Cree microservicios de Spring Cloud mediante los archivos JAR compilados en el paso anterior. Creará tres microservicios: **gateway**, **auth-service** y **account-service**.

```azurecli
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

>[!NOTE]
> Para que el servidor de configuración proporcionado funcione correctamente, los nombres de aplicación deben coincidir exactamente con los nombres de los archivos JAR.

## <a name="deploy-applications-and-set-environment-variables"></a>Implementación de aplicaciones y establecimiento de variables de entorno

Necesitamos implementar nuestras aplicaciones en Azure. Use los siguientes comandos para implementar las tres aplicaciones:

```azurecli
az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
```

## <a name="assign-public-endpoint-to-gateway"></a>Asignación de un punto de conexión público a una puerta de enlace

Necesitamos una forma de acceder a la aplicación mediante un explorador web. Nuestra aplicación de puerta de enlace necesita un punto de conexión público, que se puede asignar mediante el siguiente comando:

```azurecli
az spring-cloud app update -n gateway --is-public true
```

Finalmente, consulte la aplicación de **puerta de enlace** para ver su dirección IP pública para que pueda comprobar que la aplicación se está ejecutando:

```azurecli
az spring-cloud app show --name gateway | grep url
```

Vaya hasta la dirección URL proporcionada por el comando anterior para ver la ejecución de la aplicación PiggyMetrics.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha implementado una aplicación de Spring Cloud desde la CLI de Azure.  Para más información sobre Azure Spring Cloud, consulte el tutorial sobre la preparación de la aplicación para la implementación.

> [!div class="nextstepaction"]
> [Preparación de la aplicación de Azure Spring Cloud para la implementación](spring-cloud-tutorial-prepare-app-deployment.md)
