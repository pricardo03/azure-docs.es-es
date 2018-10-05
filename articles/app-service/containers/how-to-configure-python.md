---
title: Configuración de una imagen integrada de Python en Azure App Service
description: En este tutorial se describen opciones para crear y configurar una aplicación de Python en Azure App Service con la imagen integrada de Python.
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
ms.date: 09/25/2018
ms.author: astay;cephalin
ms.custom: mvc
ms.openlocfilehash: 9316805993b81e4d2511e833e0cc8f240807a1f9
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228559"
---
# <a name="configure-built-in-python-image-in-azure-app-service-preview"></a>Configuración de una imagen integrada de Python en Azure App Service (versión preliminar)

En este artículo se muestra cómo configurar la imagen integrada de Python en [App Service en Linux](app-service-linux-intro.md) para ejecutar las aplicaciones de Python.

## <a name="python-version"></a>Versión de Python

El entorno de ejecución de Python en App Service en Linux usa la versión `python-3.7.0`.

## <a name="supported-frameworks"></a>Marcos admitidos

Se admiten todas las versiones de marcos web compatibles con Web Server Gateway Interface (WSGI) que son compatibles con el entorno de ejecución `python-3.7`.

## <a name="package-management"></a>Administración de paquetes

Durante la publicación de Git, el motor Kudu busca el archivo [requirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files) en la raíz del repositorio e instala automáticamente los paquetes en Azure mediante `pip`.

Para generar este archivo antes de publicarlo, vaya a la raíz del repositorio y ejecute el comando siguiente en el entorno Python:

```bash
pip freeze > requirements.txt
```

## <a name="configure-your-python-app"></a>Configuración de la aplicación de Python

La imagen integrada de Python en App Service usa el servidor [Gunicorn](http://gunicorn.org/) para ejecutar la aplicación de Python. Gunicorn es un servidor HTTP de WSGI de Python para UNIX. App Service automatiza y configura Gunicorn de manera automática para proyectos de Django y Flask.

### <a name="django-app"></a>Aplicación de Django

Si publica un proyecto de Django que contiene un módulo `wsgi.py`, Azure llama automáticamente a Gunicorn con este comando:

```bash
gunicorn <path_to_wsgi>
```

### <a name="flask-app"></a>Aplicación de Flask

Si va a publicar una aplicación de Flask y el punto de entrada está en un módulo `application.py` o `app.py`, Azure llama automáticamente a Gunicorn con uno de estos comandos, respectivamente:

```bash
gunicorn application:app
```

o 

```bash
gunicorn app:app
```

### <a name="customize-start-up"></a>Personalización del inicio

Para definir un punto de entrada personalizado para la aplicación, primero debe crear un archivo _.txt_ con un comando personalizado de Gunicorn y colocarlo en la raíz del proyecto. Por ejemplo, para iniciar el servidor con el módulo _helloworld.py_ y la variable `app`, cree un archivo _startup.txt_ con este contenido:

```bash
gunicorn helloworld:app
```

En la página [Configuración de la aplicación](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), elija **Python|3.7** como la **Pila en tiempo de ejecución** y escriba el nombre del **archivo de inicio** del paso anterior. Por ejemplo, _startup.txt_.
