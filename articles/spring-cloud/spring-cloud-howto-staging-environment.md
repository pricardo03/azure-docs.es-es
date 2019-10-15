---
title: Configuración de un entorno de ensayo en Azure Spring Cloud | Microsoft Docs
description: Aprenda a usar la implementación blue-green con Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: 454eeaa2568891ec35fe698cdb20c5448e10887e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038304"
---
# <a name="how-to-set-up-a-staging-environment"></a>Configuración de un entorno de ensayo

En este artículo se muestra cómo aprovechar una implementación de ensayo con el patrón de implementación blue-green en Azure Spring Cloud. También le mostrará cómo comenzar la producción de la implementación de ensayo sin cambiar la implementación de producción.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por hecho que ya ha implementado la aplicación PiggyMetrics, para lo que ha usado el [tutorial en el que se explica cómo iniciar una aplicación](spring-cloud-quickstart-launch-app-portal.md). PiggyMetrics consta de tres aplicaciones: "gateway", "account-service" y "auth-service".  

Si desea usar otra aplicación en este ejemplo, deberá realizar un sencillo cambio en una parte pública de la aplicación.  Con este cambio se diferencia la implementación de ensayo de la producción.

>[!NOTE]
> Antes de comenzar este inicio rápido, asegúrese de que la suscripción de Azure tiene acceso a Azure Spring Cloud.  Al ser un servicio en versión preliminar, le pedimos que se ponga en contacto con nosotros para que podamos agregar su suscripción a la lista de permitidos.  Si desea explorar las funcionalidades de Azure Spring Cloud, póngase en contacto con nosotros por correo electrónico: azure-spring-cloud@service.microsoft.com.

>[!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo.  Incluye herramientas comunes de Azure preinstaladas, entre las que se incluyen las versiones más recientes de Git, JDK, Maven y la CLI de Azure. Si ha iniciado sesión en su suscripción de Azure, inicie [Azure Cloud Shell](https://shell.azure.com) desde shell.azure.com.  Para más información acerca de Azure Cloud Shell, [lea la documentación](../cloud-shell/overview.md).

Para completar este artículo:

1. [Instalación de Git](https://git-scm.com/)
1. [Instalación de JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
1. [Instalación de Maven 3.0, o cualquier versión superior](https://maven.apache.org/download.cgi)
1. [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
1. [Registro para obtener una suscripción a Azure](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Instalación de la extensión de la CLI de Azure

Instale la extensión de Azure Spring Cloud para la CLI de Azure, para lo cual debe usar el siguiente comando

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```
    
## <a name="view-all-deployments"></a>Visualización de todas las implementaciones

Vaya a la instancia de servicio en Azure Portal y seleccione **Administración de implementación** para ver todas las implementaciones. Para ver más detalles, puede seleccionar cada una de las implementaciones.

## <a name="create-a-staging-deployment"></a>Creación de una implementación de ensayo

1. En el entorno de desarrollo local, realice una pequeña modificación en la aplicación de puerta de enlace de Piggy Metric. Por ejemplo, cambie el color en `gateway/src/main/resources/static/css/launch.css`. Esto le permitirá diferenciar fácilmente las dos implementaciones. Ejecute el siguiente comando para compilar el paquete jar: 

    ```azurecli
    mvn clean package
    ```

1. Cree una implementación con la CLI de Azure y asígnele el nombre de la implementación de ensayo "green".

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app gateway -n green --jar-path gateway/target/gateway.jar
    ```

1. Una vez que la implementación se complete correctamente, acceda a la página de la puerta de enlace desde el **panel de la aplicación** y vea todas las instancias en la pestaña **App Instances** (Instancias de la aplicación) de la izquierda.
  
> [!NOTE]
> El estado de la detección es "OUT_OF_SERVICE" (FUERA DE SERVICIO), por lo que el tráfico no se enrutará a esta implementación antes de que se complete la comprobación.

## <a name="verify-the-staging-deployment"></a>Comprobación de la implementación de ensayo

1. Vuelva a la página **Administración de implementación** y seleccione la nueva implementación. El estado de la implementación que debería aparecer es **En ejecución**. El botón "Assign/Unassign domain" (Asignar o cancelar asignación de dominio) se deshabilitará, ya que es un entorno de ensayo.

1. En la página **Información general**, debería ver un **punto de conexión de prueba**. Cópielo y péguelo en una nueva página del explorador y debería ver la nueva página de Piggy Metrics.

>[!TIP]
> * Confirme que el punto de conexión de prueba termina en "/", con el fin de asegurarse de que las CSS se cargan correctamente.  
> * Si el explorador requiere que escriba las credenciales de inicio de sesión para ver la página, use [Descodificar como dirección URL ](https://www.urldecoder.org/) para descodificar el punto de conexión de prueba. Esta operación devuelve una dirección URL con el formato "https://\<nombreDeUsuario>:\<contraseña>@\<nombreDeClúster >.test.azureapps.io/gateway/green".  Use esta dirección para acceder al punto de conexión.

>[!NOTE]    
> Los valores de Config Server se aplican tanto al entorno de ensayo como al de producción. Por ejemplo, si establece la ruta de acceso del contexto (`server.servlet.context-path`) para la puerta de enlace de aplicaciones en Config Server como *unaRuta*, la ruta de acceso a implementación green cambia: "https://\<nombreDeUsuario>:\<contraseña>@\<nombreDeClúster>.test.azureapps.io/gateway/green/unaRuta/..."
 
 Si va a la puerta de enlace de aplicaciones pública en este momento, debería ver la página anterior sin el nuevo cambio.
    
## <a name="set-the-green-as-production-deployment"></a>Establecimiento de green como implementación de producción

1. Una vez que haya comprobado el cambio en el entorno de ensayo, puede enviarlo a producción. Vuelva a **Administración de implementación** y active la casilla antes de la aplicación de la "puerta de enlace".
2. Seleccione "Set deployment" (Establecer implementación).
3. Seleccione "Green" en el menú "IMPLEMENTACIÓN DE PRODUCCIÓN" y seleccione **Aplicar**
4. Vaya a la página **Información general** de la aplicación de la puerta de enlace. Si ya ha asignado un dominio a la aplicación de puerta de enlace, la dirección URL aparecerá en la página **Información general**. Visite la dirección URL y verá la página de Piggy Metrics modificada.

>[!NOTE]
> Una vez que la implementación green está establecida en entorno de producción, la implementación anterior se convierte en la implementación de ensayo.

## <a name="modify-the-staging-deployment"></a>Modificación de la implementación de ensayo

Si no está satisfecho con el cambio, puede modificar el código de la aplicación, compilar un nuevo paquete jar y cargarlo en la implementación green mediante la CLI de Azure.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-a-staging-deployment"></a>Eliminación de una implementación de ensayo

Para eliminar una implementación de ensayo desde Azure Portal, vaya a la página de la implementación de ensayo y seleccione el botón **Eliminar**.

También puede eliminarla desde la CLI de Azure con el siguiente comando:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```
