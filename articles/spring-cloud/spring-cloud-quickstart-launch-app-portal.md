---
title: Inicio de una aplicación de Azure Spring Cloud desde Azure Portal
description: Implemente una aplicación de ejemplo en Azure Spring Cloud desde Azure Portal.
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/04/2019
ms.author: v-vasuke
ms.openlocfilehash: 74a47bc5fc6dbcadef5e1a0da88eb93056334703
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244876"
---
# <a name="quickstart-launch-an-azure-spring-cloud-application-using-the-azure-portal"></a>Inicio rápido: Inicio de una aplicación de Azure Spring Cloud desde Azure Portal

Azure Spring Cloud permite ejecutar fácilmente en Azure aplicaciones de microservicios basadas en Spring Cloud.

En este inicio rápido se muestra cómo implementar una aplicación de Spring Cloud existente en Azure. [Este es un vínculo](https://github.com/Azure-Samples/PiggyMetrics) al código de la aplicación de ejemplo que se usa en este tutorial. Cuando haya terminado, se podrá acceder en línea a la aplicación de ejemplo proporcionada y está estará lista para ser administrada mediante Azure Portal.

En este inicio rápido aprenderá a:

> [!div class="checklist"]
> * Aprovisionar una instancia de servicio
> * Establecer un servidor de configuración para una instancia
> * Compilar una aplicación de microservicios localmente
> * Implementar cada microservicio
> * Asignar un punto de conexión público para la aplicación

## <a name="prerequisites"></a>Requisitos previos

>[!Note]
> Antes de comenzar este inicio rápido, asegúrese de que la suscripción de Azure tiene acceso a Azure Spring Cloud.  Al ser un servicio en versión preliminar, pedimos a los clientes que se pongan en contacto con nosotros para que podamos agregar su suscripción a la lista de permitidos.  Si desea explorar las funcionalidades de Azure Spring Cloud, póngase en contacto con nosotros por correo electrónico: azure-spring-cloud@service.microsoft.com.

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

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Aprovisionamiento de una instancia de servicio en Azure Portal

1. En un explorador web, abra [Azure Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=AppPlatformExtension) e inicie sesión en su cuenta.

1. Busque **Azure Spring Cloud** y selecciónelo para ir a la página de información general. Seleccione el botón **Crear** para empezar a usarlo.

1. Rellene el formulario teniendo en cuenta las siguientes directrices:
    - Nombre del servicio: especifique el nombre de la instancia de servicio.  El nombre debe tener entre 4 y 32 caracteres, y solo puede contener números, letras minúsculas y guiones.  El primer carácter del nombre del servicio debe ser una letra y el último debe ser una letra o un número.
    - Suscripción: seleccione la suscripción a la que desea que se facture este recurso.  Asegúrese de que esta suscripción se ha agregado a nuestra lista de permitidos para Azure Spring Cloud.
    - Grupo de recursos: se recomienda crear grupos de recursos para los nuevos recursos.
    - Ubicación: seleccione la ubicación de la instancia de servicio. Las ubicaciones admitidas actualmente son Este de EE. UU., Oeste de EE. UU. 2, Oeste de Europa y Sudeste Asiático.
    
El servicio tarda aproximadamente 5 minutos en implementarse.  Una vez implementado, aparecerá la página **Información general** de la instancia de servicio.

## <a name="set-up-your-configuration-server"></a>Configuración del servidor de configuración

1. Vaya a la página **Información general** del servicio y seleccione **Config Server**.

1. En la sección **Default repository** (Repositorio predeterminado ), establezca **URI** en "https://github.com/Azure-Samples/piggymetrics", establezca **LABEL** (ETIQUETA) en "config" y seleccione **Aplicar** para guardar los cambios.

## <a name="build-and-deploy-microservice-applications"></a>Compilación e implementación de aplicaciones de microservicios

1. Abra una ventana Comandos y ejecute el comando siguiente para clonar el repositorio de la aplicación de ejemplo en la máquina local.

    ```cli
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

1. Ejecute el comando siguiente para compilar el proyecto.

    ```cli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

1. Inicie sesión en la CLI de Azure y establezca la suscripción activa.

    ```cli
    # Login to Azure CLI
    az login

    # List all subscriptions
    az account list -o table

    # Set active subscription
    az account set --subscription <target subscription ID>
    ```

1. Asigne nombres a su grupo de recursos y a su servicio. Asegúrese de sustituir los marcadores de posición siguientes por el nombre del grupo de recursos y el nombre del servicio que aprovisionó anteriormente en este tutorial.

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

1. Cree la aplicación `gateway` e implemente el archivo JAR.

    ```azurecli
    az spring-cloud app create -n gateway
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    ```

1. Siguiendo el mismo patrón, cree las aplicaciones `account-service` y `auth-service`, e implemente sus archivos JAR.

    ```cli
    az spring-cloud app create -n account-service
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app create -n auth-service
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

1. La implementación de las aplicaciones tarda unos minutos en completarse. Para confirmar que se han implementado, vaya a la hoja **Aplicaciones** de Azure Portal. Debería ver una línea de cada una de las tres aplicaciones.

## <a name="assign-a-public-endpoint-to-gateway"></a>Asignación de un punto de conexión público a una puerta de enlace

1. Abra la página **Panel de la aplicación**.
2. Seleccione la aplicación `gateway` y se mostrará la página **Detalles de la aplicación**.
3. Seleccione **Asignar dominio** para asignar un punto de conexión público a la puerta de enlace. Esta operación puede tardar unos minutos. 
4. Escriba la dirección IP pública asignada en el explorador para ver la aplicación en ejecución.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a hacer lo siguiente:

> [!div class="checklist"]
> * Aprovisionar una instancia de servicio
> * Establecer un servidor de configuración para una instancia
> * Compilar una aplicación de microservicios localmente
> * Implementar cada microservicio
> * Asignar un punto de conexión público a la puerta de enlace de aplicaciones

> [!div class="nextstepaction"]
> [Preparación de la aplicación de Azure Spring Cloud para la implementación](spring-cloud-tutorial-prepare-app-deployment.md)
