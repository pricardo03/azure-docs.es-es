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
ms.openlocfilehash: 01140e94e8d0cc47570824970801bdd0043324d7
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166513"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-using-the-maven-plug-in"></a>Inicio rápido: Inicio de una aplicación de Azure Spring Cloud mediante el complemento Maven

El complemento Maven de Azure Spring Cloud permite crear y actualizar fácilmente las aplicaciones de servicio de Azure Spring Cloud. Al predefinir una configuración, puede implementar aplicaciones en un servicio Azure Spring Cloud existente. En este artículo, se usa una aplicación de ejemplo denominada PiggyMetrics para mostrar esta característica.

>[!Note]
> Antes de comenzar este inicio rápido, asegúrese de que la suscripción de Azure tiene acceso a Azure Spring Cloud.  Al ser un servicio en versión preliminar, pedimos a los clientes que se pongan en contacto con nosotros para que podamos agregar su suscripción a la lista de permitidos.  Si desea explorar las funcionalidades de Azure Spring Cloud, [rellene este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u
).

>[!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo.  Incluye herramientas comunes de Azure preinstaladas, entre las que se incluyen las versiones más recientes de Git, JDK, Maven y la CLI de Azure. Si ha iniciado sesión en su suscripción de Azure, inicie [Azure Cloud Shell](https://shell.azure.com) desde shell.azure.com.  Para más información acerca de Azure Cloud Shell, [lea la documentación](../cloud-shell/overview.md).

Para completar esta guía de inicio rápido:

1. [Instalación de Git](https://git-scm.com/)
2. [Instalación de JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Instalación de Maven 3.0, o cualquier versión superior](https://maven.apache.org/download.cgi)
4. [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Registro para obtener una suscripción a Azure](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Instalación de la extensión de la CLI de Azure

Instale la extensión de Azure Spring Cloud para la CLI de Azure, para lo cual debe usar el siguiente comando

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Aprovisionamiento de una instancia de servicio en Azure Portal

1. En un explorador web, abra [Azure Portal](https://portal.azure.com) e inicie sesión en su cuenta.

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

## <a name="clone-and-build-the-sample-application-repository"></a>Clonación y compilación del repositorio de aplicaciones de ejemplo

1. Ejecute el siguiente comando para clonar el repositorio de Git.

    ```azurecli
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Ejecute el siguiente comando para cambiar el directorio y compilar el proyecto.

    ```azurecli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-and-deploy-the-azure-spring-cloud-configuration"></a>Generación e implementación de la configuración de Azure Spring Cloud

1. Agregue lo siguiente a los archivos `pom.xml` o `settings.xml` para que Maven funcione con Azure Spring Cloud.

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

1. Ejecute el siguiente comando para generar una configuración.

    ```azurecli
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:config
    ```

    1. Seleccione los módulos `gateway`, `auth-service` y `account-service`.

    1. Seleccione la suscripción y el clúster de servicio de Azure Spring Cloud.

    1. En la lista de proyectos que se proporciona, escriba el número que se corresponde con `gateway` para concederle acceso público.
    
    1. Confirmación de la configuración.

1. Implemente las aplicaciones mediante el siguiente comando:

   ```azurecli
   mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:deploy
   ```

1. Puede acceder a PiggyMetrics mediante la dirección URL proporcionada en la salida del comando anterior.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha implementado una aplicación de Spring Cloud desde un repositorio de Maven.  Para más información sobre Azure Spring Cloud, consulte el tutorial sobre la preparación de la aplicación para la implementación.

> [!div class="nextstepaction"]
> [Preparación de la aplicación de Azure Spring Cloud para la implementación](spring-cloud-tutorial-prepare-app-deployment.md)
