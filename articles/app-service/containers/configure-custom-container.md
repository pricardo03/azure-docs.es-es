---
title: Configurar los contenedores de cliente - Azure App Service | Microsoft Docs
description: Obtenga información sobre cómo configurar aplicaciones de Node.js para que funcione en Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: feeb9ae4472fb3439ecc5d6505860cc407f9e4d3
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919730"
---
# <a name="configure-a-custom-linux-container-for-azure-app-service"></a>Configurar un contenedor de Linux personalizado para Azure App Service

Este artículo muestra cómo configurar un contenedor de Linux personalizado para ejecutarse en Azure App Service.

Esta guía proporciona los conceptos clave e instrucciones para la inclusión en contenedores de aplicaciones de Linux en App Service. Si nunca ha usado Azure App Service, siga el [inicio rápido de contenedor personalizado](quickstart-docker-go.md) y [tutorial](tutorial-custom-docker-image.md) primero. También hay un [inicio rápido de aplicaciones de varios contenedores](quickstart-multi-container.md) y [tutorial](tutorial-multi-container-app.md).

## <a name="configure-port-number"></a>Configurar el número de puerto

El servidor web en la imagen personalizada puede utilizar un puerto distinto de 80. Para indicar a Azure el puerto que usa el personalizado mediante la `WEBSITES_PORT` configuración de la aplicación. La página de GitHub que contiene el [ejemplo de Python de este tutorial](https://github.com/Azure-Samples/docker-django-webapp-linux) muestra que es preciso establecer `WEBSITES_PORT` en _8000_. Puede establecer mediante la ejecución de [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) comando en Cloud Shell. Por ejemplo: 

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

## <a name="configure-environment-variables"></a>Configuración de las variables de entorno

El contenedor personalizado puede usar variables de entorno que deben proporcionarse externamente. Puede pasar en ejecutando [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) comando en Cloud Shell. Por ejemplo: 

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WORDPRESS_DB_HOST="myownserver.mysql.database.azure.com"
```

Este método funciona tanto para aplicaciones de contenedor único o varios contenedores, donde se especifican las variables de entorno en el *docker-compose.yml* archivo.

## <a name="use-persistent-shared-storage"></a>Usar almacenamiento compartido persistente

Puede usar el */home* directorio en el sistema de archivos de la aplicación para conservar archivos entre reinicios y compartirlos entre instancias. El `/home` en la aplicación se proporciona para habilitar la aplicación de contenedor para acceder al almacenamiento persistente.

Cuando se deshabilita el almacenamiento persistente después se escribe en el `/home` directorio no se conservan entre reinicios de aplicación o entre varias instancias. La única excepción es el `/home/LogFiles` directorio, que se usa para almacenar los registros de Docker y el contenedor. Cuando se habilita el almacenamiento persistente, todas las escrituras a la `/home` directory se conservan y pueden tener acceso a todas las instancias de una aplicación de escala horizontal.

De forma predeterminada, el almacenamiento persistente es *deshabilitado*. Para habilitarlo o deshabilitarlo, establezca el `WEBSITES_ENABLE_APP_SERVICE_STORAGE` configuración de la aplicación mediante la ejecución de [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) comando en Cloud Shell. Por ejemplo: 

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
```

> [!NOTE]
> También puede [configurar su propio almacenamiento persistente](how-to-serve-content-from-azure-storage.md).

## <a name="enable-ssh"></a>Habilite SSH

SSH habilita la comunicación segura entre un contenedor y un cliente. En el orden de un contenedor personalizado admitir SSH, debe agregarlo en el Dockerfile propio.

> [!TIP]
> Todos los contenedores de Linux integrados agregó las instrucciones de SSH en sus repositorios de imágenes. Puede seguir las instrucciones siguientes con el [Node.js 10.14 repositorio](https://github.com/Azure-App-Service/node/blob/master/10.14) para ver cómo se habilita allí.

- Use la [ejecutar](https://docs.docker.com/engine/reference/builder/#run) instrucciones para instalar el servidor SSH y establecer la contraseña para la cuenta raíz en `"Docker!"`. Por ejemplo, para una imagen basada en [Alpine Linux](https://hub.docker.com/_/alpine), necesita los siguientes comandos:

    ```Dockerfile
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 
    ```

    Esta configuración no permite realizar conexiones externas al contenedor. SSH solo está disponible en `https://<app-name>.scm.azurewebsites.net` y autentica con las credenciales de publicación.

- Agregar [este archivo sshd_config](https://github.com/Azure-App-Service/node/blob/master/10.14/sshd_config) en su repositorio de imágenes y usar el [copia](https://docs.docker.com/engine/reference/builder/#copy) instrucciones para copiar el archivo a la */etcetera/ssh/* directory. Para obtener más información acerca de *sshd_config* archivos, consulte [OpenBSD documentación](https://man.openbsd.org/sshd_config).

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > El archivo *sshd_config* debe incluir los elementos siguientes:
    > - `Ciphers` debe incluir al menos un elemento de esta lista: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs` debe incluir al menos un elemento de esta lista: `hmac-sha1,hmac-sha1-96`.

- Use la [exponer](https://docs.docker.com/engine/reference/builder/#expose) instrucciones para abrir el puerto 2222 en el contenedor. Aunque se conoce la contraseña raíz, el puerto 2222 no es accesible desde internet. Es accesible solo por los contenedores dentro de la red de puente de una red privada virtual.

    ```Dockerfile
    EXPOSE 80 2222
    ```

- En el script de inicio para el contenedor, inicie el servidor SSH.

    ```bash
    /usr/sbin/sshd
    ```

    Para obtener un ejemplo, vea cómo el valor predeterminado [Node.js 10.14 contenedor](https://github.com/Azure-App-Service/node/blob/master/10.14/startup/init_container.sh) inicia el servidor SSH.

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="configure-multi-container-apps"></a>Configurar aplicaciones de varios contenedores

- [Utilice el almacenamiento persistente en Docker Compose](#use-persistent-storage-in-docker-compose)
- [Limitaciones de vista previa](#preview-limitations)
- [Opciones de docker Compose](#docker-compose-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Utilice el almacenamiento persistente en Docker Compose

Aplicaciones de varios contenedores, como WordPress necesitan almacenamiento persistente para funcionar correctamente. Para habilitarla, la configuración de Docker Compose debe apuntar a una ubicación de almacenamiento *fuera* su contenedor. Ubicaciones de almacenamiento dentro del contenedor no conservan los cambios más allá de reinicio de la aplicación.

Habilitar el almacenamiento persistente, establezca el `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app, mediante el [az appservice Web config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) comando en Cloud Shell.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

En su *docker-compose.yml* de archivos, asigne el `volumes` opción `${WEBAPP_STORAGE_HOME}`. 

`WEBAPP_STORAGE_HOME` es una variable de entorno en App Service que se asigna al almacenamiento persistente para la aplicación. Por ejemplo: 

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="preview-limitations"></a>Limitaciones de vista previa

Varios contenedores está actualmente en versión preliminar. No se admiten las siguientes características de la plataforma de App Service:

- Autenticación/autorización
- Identidades administradas

### <a name="docker-compose-options"></a>Opciones de docker Compose

Las listas siguientes muestran las opciones de configuración admitidas y Docker Compose:

#### <a name="supported-options"></a>Opciones admitidas

- command
- entrypoint
- Environment
- image
- ports
- restart
- services
- volumes

#### <a name="unsupported-options"></a>Opciones no admitidas

- build (no permitido)
- depends_on (omitido)
- networks (omitido)
- secrets (omitido)
- puertos que no sean el 80 y 8080 (omitido)

> [!NOTE]
> Se omiten otras opciones no expresamente en versión preliminar pública.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Implementar desde el repositorio de contenedor privado](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [Tutorial: Aplicación de WordPress con varios contenedores](tutorial-multi-container-app.md)
