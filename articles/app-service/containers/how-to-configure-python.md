---
title: 'Configuración de aplicaciones de Python en Linux: Azure App Service'
description: En este tutorial se describen opciones para crear y configurar aplicaciones de Python para Azure App Service en Linux.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/09/2018
ms.author: astay;cephalin;kraigb
ms.custom: seodec18
ms.openlocfilehash: f7e63fa75f473d5da911fbf845f0662d8eec5c70
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53717405"
---
# <a name="configure-your-python-app-for-the-azure-app-service-on-linux"></a>Configuración de una aplicación de Python para Azure App Service en Linux

En este artículo se describe cómo [Azure App Service en Linux](app-service-linux-intro.md) ejecuta aplicaciones de Python, y cómo puede personalizar el comportamiento de App Service cuando es necesario.

## <a name="set-python-version"></a>Establecimiento de la versión de Python

Dos imágenes base están disponibles: Python 3.6 y Python 3.7. Puede crear una aplicación con la imagen deseada basada en Python. Por ejemplo, para crear una aplicación con Python 3.7, ejecute el siguiente comando en Cloud Shell:

```azurecli-interactive
az webapp create --resource-group <group_name> --plan <plan_name> --name <app_name> --runtime "PYTHON|3.7"
```

Para cambiar la versión de Python (imagen base) a Python 3.6, por ejemplo, ejecute el siguiente comando en Cloud Shell:

```azurecli-interactive
az webapp config set --resource-group <group_name> --name <app_name> --linux-fx-version "PYTHON|3.6"
```

Si necesita una versión diferente de Python, debe compilar e implementar su propia imagen de contenedor en su lugar. Para más información, consulte [Uso de una imagen personalizada de Docker para Web App for Containers](tutorial-custom-docker-image.md).

## <a name="container-characteristics"></a>Características del contenedor

Las aplicaciones de Python que se implementan en App Service en Linux se ejecutan dentro de un contenedor de Docker que se define en el repositorio de GitHub, [Python 3.6](https://github.com/Azure-App-Service/python/tree/master/3.6.6) o [Python 3.7](https://github.com/Azure-App-Service/python/tree/master/3.7.0).

Este contenedor tiene las siguientes características:

- Las aplicaciones se ejecutan mediante el [servidor HTTP de WSGI de Gunicorn](https://gunicorn.org/), mediante los argumentos adicionales `--bind=0.0.0.0 --timeout 600`.

- De forma predeterminada, la imagen base incluye la plataforma web Flask, pero el contenedor admite otras plataformas que cumplen con WSGI y son compatibles con Python 3.7, como Django.

- Para instalar paquetes adicionales, como Django, cree un archivo [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files) en la raíz del proyecto mediante `pip freeze > requirements.txt`. A continuación, publique el proyecto en App Service mediante la implementación de Git, que ejecutará automáticamente `pip install -r requirements.txt` en el contenedor para instalar las dependencias de la aplicación.

## <a name="container-startup-process-and-customizations"></a>Personalizaciones y proceso de inicio del contenedor

Durante el inicio, la instancia de App Service en el contenedor de Linux ejecuta los siguientes pasos:

1. Busca y aplica un comando de inicio personalizado, si se proporciona.
2. Comprueba la existencia de un archivo *wsgi.py* de aplicación de Django y, si existe, inicia Gunicorn mediante ese archivo.
3. Busca un archivo denominado *application.py* y, si lo encuentra, inicia Gunicorn mediante `application:app` suponiendo que haya una aplicación de Flask.
4. Si no se encuentra ninguna otra aplicación, inicia una aplicación predeterminada que está integrada en el contenedor.

En las secciones siguientes se proporcionan detalles adicionales para cada opción.

### <a name="django-app"></a>Aplicación de Django

Para las aplicaciones de Django, App Service busca un archivo denominado `wsgi.py` en el código de la aplicación y, a continuación, ejecuta Gunicorn mediante el comando siguiente:

```bash
# <module> is the path to the folder containing wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Si desea un control más específico sobre el comando de inicio, utilice un [comando de inicio personalizado](#custom-startup-command) y reemplace `<module>` por el nombre del módulo que contiene el archivo *wsgi.py*.

### <a name="flask-app"></a>Aplicación de Flask

Para Flask, App Service busca un archivo denominado *application.py* e inicia Gunicorn como sigue:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 application:app
```

Si el módulo de la aplicación principal está contenido en un archivo diferente, use un nombre diferente para el objeto de la aplicación, o si desea proporcionar argumentos adicionales para Gunicorn, use un [comando de inicio personalizado](#custom-startup-command). Esa sección proporciona un ejemplo para Flask mediante el código de entrada que se encuentra en *hello.py* y un objeto de aplicación de Flask denominado `myapp`.

### <a name="custom-startup-command"></a>Comando de inicio personalizado

Puede controlar el comportamiento de inicio del contenedor proporcionando un comando de inicio de Gunicorn personalizado. Por ejemplo, si tiene una aplicación de Flask cuyo módulo principal es *hello.py* y el objeto de aplicación de Flask en ese archivo se denomina `myapp`, el comando es como sigue:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
```

Si el módulo principal está en una subcarpeta como, por ejemplo, `website` especifique esa carpeta con el argumento `--chdir`:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
```

También puede agregar argumentos adicionales para Gunicorn al comando, como `--workers=4`. Para más información, consulte [Running Gunicorn](https://docs.gunicorn.org/en/stable/run.html) (Ejecución de Gunicorn) (docs.gunicorn.org).

Para proporcionar un comando personalizado, realice los pasos siguientes:

1. Vaya a la página de [configuración de la aplicación](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) en Azure Portal.

1. En los valores de **Entorno de ejecución**, establezca la opción **Pila** en **Python 3.7** y escriba el comando directamente en el campo **Archivo de inicio**.

    Como alternativa, puede guardar el comando en un archivo de texto en la raíz del proyecto con un nombre como *startup.txt* (o cualquier otro nombre que desee). A continuación, implemente ese archivo en App Service y especifique el nombre del archivo en el campo **Archivo de inicio** en su lugar. Esta opción le permite administrar el comando dentro de su repositorio de código fuente en lugar de mediante Azure Portal.

1. Seleccione **Guardar**. La instancia de App Service se reinicia automáticamente y después de algunos segundos debería ver el comando de inicio personalizado aplicado.

> [!Note]
> App Service ignora los errores que se producen al procesar un archivo de comandos personalizados y continúa su proceso de inicio buscando aplicaciones de Django y Flask. Si no observa el comportamiento previsto, compruebe que el archivo de inicio se ha implementado en App Service y que no contiene errores.

### <a name="default-behavior"></a>Comportamiento predeterminado

Si el servicio de aplicación no encuentra un comando personalizado, una aplicación de Django o una aplicación de Flask, se ejecutará una aplicación predeterminada de solo lectura, ubicada en la carpeta _opt/defaultsite_. La aplicación predeterminada aparece como sigue:

![Instancia predeterminada de App Service en la página web de Linux](media/how-to-configure-python/default-python-app.png)

## <a name="troubleshooting"></a>solución de problemas

- **Ve la aplicación predeterminada después de implementar su propio código de aplicación.**  La aplicación predeterminada aparece porque no ha implementado realmente el código de la aplicación en App Service o porque este no pudo encontrar el código de la aplicación y ejecutó en su lugar la aplicación predeterminada.
  - Reinicie App Service, espere 15-20 segundos y vuelva a comprobar la aplicación.
  - Asegúrese de que está utilizando App Service para Linux en lugar de una instancia basada en Windows. Desde la CLI de Azure, ejecute el comando `az webapp show --resource-group <resource_group_name> --name <app_service_name> --query kind`, reemplazando `<resource_group_name>` y `<app_service_name>` en consecuencia. Debería ver `app,linux` como salida; en caso contrario, vuelva a crear la instancia de App Service y elija Linux.
    - Use SSH o la consola de Kudu para conectarse directamente con App Service y compruebe que los archivos existen en *site/wwwroot*. Si no existen los archivos, revise el proceso de implementación y vuelva a implementar la aplicación.
  - Si existen los archivos, App Service no fue capaz de identificar el archivo de inicio específico. Compruebe que la aplicación está estructurada como espera App Service para [Django](#django-app) o [Flask](#flask-app), o use un [comando de inicio personalizado](#custom-startup-command).
  
- **Ve el mensaje "Servicio no disponible" en el explorador.** El explorador ha superado el tiempo de espera mientras esperaba una respuesta de App Service, lo cual indica que App Service ha iniciado el servidor Gunicorn, pero los argumentos que especifica el código de la aplicación son incorrectos.
  - Actualice el explorador, especialmente si usa los planes de tarifa más bajos en su plan de App Service. Por ejemplo, la aplicación puede tardar más en iniciarse si usa niveles gratis, por ejemplo, y comienza a tener capacidad de respuesta después de actualizar el explorador.
  - Compruebe que la aplicación está estructurada como espera App Service para [Django](#django-app) o [Flask](#flask-app), o use un [comando de inicio personalizado](#custom-startup-command).
  - Use SSH o la consola de Kudu para conectarse a App Service y, después, examine los registros de diagnóstico almacenados en la carpeta *LogFiles*. Para más información sobre los registros, consulte [Habilitación del registro de diagnóstico para aplicaciones web en Azure App Service](../troubleshoot-diagnostic-logs.md).
