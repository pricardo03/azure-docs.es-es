---
title: 'Tutorial: Uso de Python y TensorFlow en Azure Functions para hacer inferencias de aprendizaje automático | Microsoft Docs'
description: En este tutorial se muestra cómo aplicar los modelos de aprendizaje automático de TensorFlow en Azure Functions
services: functions
author: anthonychu
manager: gwallace
ms.service: azure-functions
ms.devlang: python
ms.topic: tutorial
ms.date: 07/29/2019
ms.author: antchu
ms.custom: mvc
ms.openlocfilehash: e243fd2f5c4a90e45f424ce39a97913df2332b2b
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677883"
---
# <a name="tutorial-apply-machine-learning-models-in-azure-functions-with-python-and-tensorflow"></a>Tutorial: Aplicación de modelos de aprendizaje automático en Azure Functions con Python y TensorFlow

En este artículo se muestra cómo Azure Functions permite usar Python y TensorFlow con un modelo de aprendizaje automático para clasificar una imagen en función de su contenido.

En este tutorial, aprenderá a: 

> [!div class="checklist"]
> * Inicializar un entorno local para el desarrollo de Azure Functions en Python
> * Importar un modelo de aprendizaje automático de TensorFlow personalizado en una aplicación de funciones
> * Crear una API HTTP sin servidor para predecir si una foto contiene un perro o un gato
> * Uso de la API desde una aplicación web

![Captura de pantalla del proyecto terminado](media/functions-machine-learning-tensorflow/functions-machine-learning-tensorflow-screenshot.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos 

Para crear instancias de Azure Functions en Python, debe instalar algunas herramientas.

- [Python 3.6](https://www.python.org/downloads/release/python-360/)
- [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools)
- Un editor de código como [Visual Studio Code](https://code.visualstudio.com/)

## <a name="clone-the-tutorial-repository"></a>Clonación del repositorio de tutoriales

Para empezar, abra un terminal y clone el siguiente repositorio mediante Git:

```console
git clone https://github.com/Azure-Samples/functions-python-tensorflow-tutorial.git
cd functions-python-tensorflow-tutorial
```

El repositorio contiene algunas carpetas.

- *start*:  esta es la carpeta de trabajo del tutorial
- *end*: este es el resultado final y la implementación completa como referencia
- *resources*: contiene el modelo de aprendizaje automático y las bibliotecas auxiliares
- *frontend*: sitio web que llama a la aplicación de funciones

## <a name="create-and-activate-a-python-virtual-environment"></a>Creación y activación de un entorno virtual de Python

Azure Functions requiere Python 3.6.x. Creará un entorno virtual para asegurarse de que está usando la versión de Python necesaria.

Cambie el directorio de trabajo actual a la carpeta *start*. Después, cree y active un entorno virtual denominado *.venv*. En función de la instalación de Python, los comandos para crear un entorno virtual de Python 3.6 pueden diferir de las siguientes instrucciones.

#### <a name="linux-and-macos"></a>Linux y macOS:

```bash
cd start
python3.6 -m venv .venv
source .venv/bin/activate
```

#### <a name="windows"></a>Windows:

```powershell
cd start
py -3.6 -m venv .venv
.venv\scripts\activate
```

El símbolo del sistema del terminal ahora tiene el prefijo `(.venv)` que indica que se ha activado correctamente el entorno virtual. Confirme que `python` en el entorno virtual sea realmente Python 3.6.x.

```console
python --version
```

> [!NOTE]
> En el resto del tutorial se ejecutan comandos en el entorno virtual. Si necesita reactivar el entorno virtual en un terminal, ejecute el comando de activación correspondiente al sistema operativo.

## <a name="create-an-azure-functions-project"></a>Creación de un proyecto de Azure Functions

En la carpeta *start*, use Azure Functions Core Tools para inicializar una aplicación de funciones de Python.

```console
func init --worker-runtime python
```

Una aplicación de funciones puede contener una o más instancias de Azure Functions. Abra la carpeta *start* en un editor y examine el contenido.

- [*local.settings.json*](functions-run-local.md#local-settings-file): contiene la configuración de la aplicación que se usa para el desarrollo local
- [*host.json*](functions-host-json.md): contiene la configuración para el host y las extensiones de Azure Functions
- [*requirements.txt*](functions-reference-python.md#python-version-and-package-management): contiene los paquetes de Python que requiere esta aplicación

## <a name="create-an-http-function"></a>Creación de una función HTTP

La aplicación requiere un único punto de conexión de una API HTTP que toma una dirección URL de imagen como entrada y devuelve una predicción de si la imagen contiene un perro o un gato.

En el terminal, use Azure Functions Core Tools para aplicar scaffolding a una nueva función HTTP denominada *classify*.

```console
func new --language python --template HttpTrigger --name classify
```

Se crea una carpeta denominada *classify* que contiene dos archivos.

- *\_\_init\_\_.py*: archivo para la función principal
- *function.json*:  archivo que describe el desencadenador de la función y sus enlaces de entrada y salida

### <a name="run-the-function"></a>Ejecución de la función

En el terminal con el entorno virtual de Python activado, inicie la aplicación de funciones.

```console
func start
```

Abra un explorador y vaya a la dirección URL siguiente. La función debe ejecutarse y devolver *Hello Azure!*

```
http://localhost:7071/api/classify?name=Azure
```

Use `Ctrl-C` para detener la aplicación de funciones.

## <a name="import-the-tensorflow-model"></a>Importación del modelo de TensorFlow

Usará un modelo de TensorFlow pregenerado que se haya entrenado con el servicio Azure Custom Vision y que se haya exportado de allí.

> [!NOTE]
> Si desea crear su propio modelo con el nivel gratuito de Custom Vision, puede seguir las [instrucciones del repositorio del proyecto de ejemplo](https://github.com/Azure-Samples/functions-python-tensorflow-tutorial/blob/master/train-custom-vision-model.md).

El modelo consta de dos archivos en la carpeta *<REPOSITORY_ROOT>/resources/model*: *model.pb* y *labels.txt*. Cópielos en la carpeta *classify* de la función.

#### <a name="linux-and-macos"></a>Linux y macOS:

```bash
cp ../resources/model/* classify
```

#### <a name="windows"></a>Windows:

```powershell
copy ..\resources\model\* classify
```

Asegúrese de incluir \* en el comando anterior. Confirme que *classify* ahora contiene los archivos denominados *model.pb* y *labels.txt*.

## <a name="add-the-helper-functions-and-dependencies"></a>Incorporación de las funciones auxiliares y las dependencias

Algunas funciones auxiliares para preparar la imagen de entrada y efectuar una predicción mediante TensorFlow se encuentran en un archivo denominado *predict.py* en la carpeta *resources*. Copie este archivo en la carpeta *classify* de la función.

#### <a name="linux-and-macos"></a>Linux y macOS:

```bash
cp ../resources/predict.py classify
```

#### <a name="windows"></a>Windows:

```powershell
copy ..\resources\predict.py classify
```

Confirme que *classify* ahora contiene un archivo denominado *predict.py*.

### <a name="install-dependencies"></a>Instalación de dependencias

La biblioteca auxiliar tiene algunas dependencias que deben instalarse. Abra *start/requirements.txt* en el editor y agregue las siguientes dependencias al archivo.

```txt
tensorflow==1.15
Pillow
requests
```

Guarde el archivo.

En el terminal con el entorno virtual activado ejecute el siguiente comando en la carpeta *start* para instalar las dependencias. Algunos pasos de la instalación pueden tardar algunos minutos en completarse.

```console
pip install --no-cache-dir -r requirements.txt
```

### <a name="caching-the-model-in-global-variables"></a>Almacenamiento en caché del modelo en variables globales

En el editor, abra *predict.py* y mire la función `_initialize` situada cerca de la parte superior del archivo. Observe que el modelo TensorFlow se carga desde el disco la primera vez que se ejecuta la función y se guarda en variables globales. La carga desde el disco se omite en las ejecuciones posteriores de la función `_initialize`. Almacenar en la memoria caché el modelo con esta técnica acelera las predicciones posteriores.

Para más información sobre las variables globales, consulte la [Guía de Azure Functions para desarrolladores de Python](functions-reference-python.md#global-variables).

## <a name="update-function-to-run-predictions"></a>Actualización de la función para ejecutar predicciones

Abra *classify/\_\_init\_\_.py* en el editor. Importe la biblioteca *predict* que ha agregado a la misma carpeta anteriormente. Agregue las siguientes instrucciones `import` debajo de las otras importaciones que ya están en el archivo.

```python
import json
from .predict import predict_image_from_url
```

Reemplace el código de plantilla de la función por lo siguiente.

```python
def main(req: func.HttpRequest) -> func.HttpResponse:
    image_url = req.params.get('img')
    results = predict_image_from_url(image_url)

    headers = {
        "Content-type": "application/json",
        "Access-Control-Allow-Origin": "*"
    }
    return func.HttpResponse(json.dumps(results), headers = headers)
```

Asegúrese de guardar los cambios.

Esta función recibe una dirección URL de imagen en un parámetro de cadena de consulta denominado `img`. Llama a `predict_image_from_url` desde la biblioteca auxiliar que descarga la imagen y devuelve una predicción mediante el modelo de TensorFlow. A continuación, la función devuelve una respuesta HTTP con los resultados.

Dado que una página web hospedada en otro dominio llama al punto de conexión HTTP, la respuesta HTTP incluye un encabezado `Access-Control-Allow-Origin` para satisfacer los requisitos de uso compartido de recursos entre orígenes (CORS) del explorador.

> [!NOTE]
> En una aplicación de producción, cambie `*` al origen específico de la página web para mayor seguridad.

### <a name="run-the-function-app"></a>Ejecución de la aplicación de funciones

Asegúrese de que el entorno virtual de Python todavía esté activado e inicie la aplicación de funciones con el siguiente comando.

```console
func start
```

En un explorador, abra esta dirección URL que llama a la función con la dirección URL de una foto de gato. Confirme que se devuelve un resultado de predicción válido.

```
http://localhost:7071/api/classify?img=https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png
```

Mantenga la aplicación de funciones en ejecución.

### <a name="run-the-web-app"></a>Ejecución de la aplicación web

Hay una aplicación web simple en la carpeta *frontend* que usa la API HTTP en la aplicación de funciones.

Abra un terminal *independiente* y cambie a la carpeta *frontend*. Inicie un servidor HTTP con Python 3.6.

#### <a name="linux-and-macos"></a>Linux y macOS:

```bash
cd <FRONT_END_FOLDER>
python3.6 -m http.server
```

#### <a name="windows"></a>Windows:

```powershell
cd <FRONT_END_FOLDER>
py -3.6  -m http.server
```

En un explorador, vaya a la dirección URL del servidor HTTP que se muestra en el terminal. Debería aparecer una aplicación web. Escriba una de las siguientes direcciones URL de foto en el cuadro de texto. También puede usar una dirección URL de una foto de gato o perro públicamente accesible.

- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png`
- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat2.png`
- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog1.png`
- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog2.png`

Al hacer clic en Enviar, se llama a la aplicación de funciones y se muestra un resultado en la página.

## <a name="clean-up-resources"></a>Limpieza de recursos
La totalidad de este tutorial se ejecuta localmente en la máquina, por lo que no hay que limpiar ningún recurso o servicio de Azure.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió a crear y personalizar una API HTTP con Azure Functions para realizar predicciones mediante un modelo de TensorFlow. También ha aprendido a llamar a la API desde una aplicación web.

Puede utilizar estas técnicas del tutorial para crear API de cualquier complejidad, todo ello mientras ejecuta el modelo de proceso sin servidor proporcionado por Azure Functions.

Para implementar la aplicación de funciones en Azure, use [Azure Functions Core Tools](./functions-run-local.md#publish) o [Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-azure-functions).

> [!div class="nextstepaction"]
> [Guía de Azure Functions para desarrolladores de Python](./functions-reference-python.md)
