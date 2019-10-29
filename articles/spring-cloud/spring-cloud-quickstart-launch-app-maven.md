---
title: 'Inicio rápido: Inicio de una aplicación mediante Maven (Azure Spring Cloud)'
description: Inicio de una aplicación de ejemplo mediante Maven
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: v-vasuke
ms.openlocfilehash: e773b997cca3fa9a1f11fec2ac449e1fc11c5364
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554559"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-by-using-the-maven-plug-in"></a>Inicio rápido: Inicio de una aplicación de Azure Spring Cloud mediante el complemento Maven

Con el complemento Maven de Azure Spring Cloud puede crear y actualizar fácilmente las aplicaciones de servicio de Azure Spring Cloud. Al predefinir una configuración, puede implementar aplicaciones en un servicio Azure Spring Cloud existente. En este artículo, se usa una aplicación de ejemplo denominada PiggyMetrics para mostrar esta característica.

>[!Note]
> Antes de empezar este inicio rápido asegúrese de que la suscripción de Azure tiene acceso a Azure Spring Cloud. Al ser un servicio en versión preliminar, le invitamos a que se ponga en contacto con nosotros para que podamos agregar su suscripción a la lista de permitidos. Si desea explorar las funcionalidades de Azure Spring Cloud, rellene y envíe el [formulario de interés de Azure Spring Cloud (versión preliminar privada)](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u).

>[!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los comandos de este artículo. Tiene herramientas comunes de Azure preinstaladas, entre las que se incluyen las versiones más recientes de Git, Java Development Kit (JDK), Maven y la CLI de Azure. Si ha iniciado sesión en su suscripción de Azure, inicie [Azure Cloud Shell](https://shell.azure.com). Para más información, vea [Introducción a Azure Cloud Shell](../cloud-shell/overview.md).

Para completar esta guía de inicio rápido:

1. [Instale Git](https://git-scm.com/).
2. [Instale JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable).
3. [Instale Maven 3.0, o cualquier versión posterior](https://maven.apache.org/download.cgi).
4. [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
5. [Regístrese para obtener una suscripción gratuita de Azure](https://azure.microsoft.com/free/).

## <a name="install-the-azure-cli-extension"></a>Instalación de la extensión de la CLI de Azure

Para instalar la extensión de Azure Spring Cloud para la CLI de Azure, use el siguiente comando:

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Aprovisionamiento de una instancia de servicio en Azure Portal

1. En un explorador web, abra [Azure Portal](https://portal.azure.com) e inicie sesión en su cuenta.

1. Busque y seleccione **Azure Spring Cloud**. 
1. En la página de información general, seleccione **Crear**y, después, haga lo siguiente:  

    a. En el cuadro **Nombre de servicio**, especifique el nombre de la instancia de servicio. Dicho nombre debe tener entre 4 y 32 caracteres, y solo puede contener números, letras minúsculas y guiones. El primer carácter del nombre de servicio debe ser una letra y el último una letra o un número.  

    b. En la lista desplegable **Suscripción**, seleccione la suscripción en la que desea que se le facture este recurso. Asegúrese de que la suscripción se ha agregado a nuestra lista de permitidos para Azure Spring Cloud.  

    c. En el cuadro **Grupo de recursos**, cree un grupo de recursos. La creación de grupos de recursos es un procedimiento recomendado.  

    d. En la lista desplegable **Ubicación**, seleccione la ubicación de la instancia de servicio. Las ubicaciones admitidas actualmente son Este de EE. UU., Oeste de EE. UU. 2, Oeste de Europa y Sudeste Asiático.
    
El servicio tarda aproximadamente 5 minutos en implementarse. Una vez implementado, aparece la página **Información general** de la instancia de servicio.

## <a name="set-up-your-configuration-server"></a>Configuración del servidor de configuración

1. En la página **Información general** del servicio y seleccione **Config Server**.
1. En la sección **Default repository** (Repositorio predeterminado), establezca **URI** en **https://github.com/Azure-Samples/piggymetrics** , establezca **Label** (Etiqueta) en **config** y seleccione **Aplicar** para guardar los cambios.

## <a name="clone-and-build-the-sample-application-repository"></a>Clonación y compilación del repositorio de aplicaciones de ejemplo

1. Para clonar el repositorio de Git, ejecute el siguiente comando:

    ```azurecli
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Ejecute el siguiente comando para cambiar el directorio y compilar el proyecto:

    ```azurecli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-and-deploy-the-azure-spring-cloud-configuration"></a>Generación e implementación de la configuración de Azure Spring Cloud

1. Para habilitar Maven para que funcione con Azure Spring Cloud, agregue el siguiente código a los archivos *pom.xml* o *settings.xml*.

    ```xml
    <pluginRepositories>
      <pluginRepository>
        <id>maven.snapshots</id>
        <name>Maven Central Snapshot Repository</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <releases>
          <enabled>false</enabled>
        </releases>
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </pluginRepository>
    </pluginRepositories>
    ```

1. Ejecute el siguiente comando para generar una configuración:

    ```azurecli
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:config
    ```

    a. Seleccione los módulos `gateway`, `auth-service` y `account-service`.

    b. Seleccione la suscripción y el clúster de servicio de Azure Spring Cloud.

    c. En la lista de proyectos que se proporcionan, escriba el número correspondiente a `gateway` para concederle acceso público.
    
    d. Confirmación de la configuración.

1. Implemente las aplicaciones mediante el siguiente comando:

   ```azurecli
   mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:deploy
   ```

1. Para acceder a PiggyMetrics, use la dirección URL que se proporciona en la salida del comando anterior.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha implementado una aplicación de Spring Cloud desde un repositorio de Maven. Para más información sobre Azure Spring Cloud, consulte el tutorial sobre la preparación de una aplicación para la implementación.

> [!div class="nextstepaction"]
> [Preparación de la aplicación Azure Spring Cloud para la implementación](spring-cloud-tutorial-prepare-app-deployment.md)
