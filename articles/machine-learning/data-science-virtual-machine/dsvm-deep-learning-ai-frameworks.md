---
title: Marcos de aprendizaje profundo y AI
titleSuffix: Azure Data Science Virtual Machine
description: Disponibilidad de los marcos de aprendizaje profundo y las herramientas en Azure Data Science Virtual Machine.
keywords: herramientas de ciencia de datos, máquina virtual de ciencia de datos, herramientas para la ciencia de datos, ciencia de datos de linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/1/2019
ms.openlocfilehash: fd38bf1f7741c4d610ef43a12d90533d4ac7b703
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802407"
---
# <a name="deep-learning-and-ai-frameworks-for-the-azure-data-science-vm"></a>Marcos de aprendizaje profundo e inteligencia artificial para Azure Data Science VM
La lista a continuación enumera los marcos de aprendizaje profundo en DSVM.

## <a name="caffehttpsgithubcombvlccaffe"></a>[Caffe](https://github.com/BVLC/caffe)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | |
| Ediciones de DSVM admitidas      | Linux (Ubuntu)     |
| ¿Cómo se configura/instala en DSVM?  | Caffe se instala en `/opt/caffe`.   Se pueden encontrar ejemplos en `/opt/caffe/examples`.|
| Cómo ejecutarlo      | use X2Go para iniciar sesión en la máquina virtual y, a continuación, inicie un nuevo terminal y escriba lo siguiente:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Se abre una nueva ventana del explorador con blocs de notas de ejemplo. Los binarios se instalan en /opt/caffe/build/install/bin.<br/><br/>La versión instalada de Caffe requiere Python 2.7 y no funcionará con Python 3.5, que está activado de forma predeterminada. Para cambiar a Python 2.7, ejecute `source activate root` para cambiar al entorno de Anaconda.|    

## <a name="caffe2httpsgithubcomcaffe2caffe2"></a>[Caffe2](https://github.com/caffe2/caffe2)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | |
| Ediciones de DSVM admitidas      | Linux (Ubuntu)     |
| ¿Cómo se configura/instala en DSVM?  | Caffe2 se instala en el entorno de Conda de Python 2.7 (root). |
| Cómo ejecutarlo      | Terminal: Inicie Python e importe Caffe2. <br/> * JupyterHub: [conéctese a JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) y desplácese al directorio Caffe2 para buscar blocs de notas de ejemplo. Algunos blocs de notas requieren que se establezca la raíz de Caffe2 en el código de Python. Escriba /opt/caffe2. |

## <a name="chainerhttpschainerorg"></a>[Chainer](https://chainer.org/)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 5.2 |
| Ediciones de DSVM admitidas      | Linux (Ubuntu)     |
| ¿Cómo se configura/instala en DSVM?  | Chainer se instala en Python 3.5. |
| Cómo ejecutarlo      | Terminal: active el entorno de Python 3.5, ejecute `python` y luego `import chainer`. <br/> * JupyterHub: [conéctese a JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) y desplácese al directorio Chainer para buscar cuadernos de ejemplo.| 

## <a name="cuda-cudnn-nvidia-driverhttpsdevelopernvidiacomcuda-toolkit"></a>[CUDA, cuDNN, NVIDIA Driver](https://developer.nvidia.com/cuda-toolkit)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 10.0.130|
| Ediciones de DSVM admitidas      | Windows y Linux   |
| ¿Cómo se configura/instala en DSVM?  |_nvidia-smi_ está disponible en la ruta de acceso del sistema.  |
| Cómo ejecutarlo      | Abra un símbolo del sistema (en Windows) o un terminal (en Linux) y ejecute _nvidia-smi_. |


## <a name="horovodhttpsgithubcomuberhorovod"></a>[Horovod](https://github.com/uber/horovod)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 0.16.1|
| Ediciones de DSVM admitidas      | Linux (Ubuntu)   |
| ¿Cómo se configura/instala en DSVM?  | Horovod se instala en Python 3.5 |
| Cómo ejecutarlo      | Active el entorno correcto en el terminal y, luego, ejecute Python. |

## <a name="kerashttpskerasio"></a>[Keras](https://keras.io/)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 2.2.4 |
| Ediciones de DSVM admitidas      | Windows y Linux   |
| ¿Cómo se configura/instala en DSVM?  | Keras se instala en Python 3.6 en Windows, y en Python 3.5 en Linux |
| Cómo ejecutarlo      | Active el entorno correcto en el terminal y, luego, ejecute Python. |

## <a name="microsoft-cognitive-toolkit-cntkhttpsdocsmicrosoftcomcognitive-toolkit"></a>[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 2.5.1 |
| Ediciones de DSVM admitidas      | Windows y Linux   |
| ¿Cómo se configura/instala en DSVM?  | CNTK se instala en Python 3.6 en [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition), y en Python 3.5 en [Linux](./dsvm-languages.md#python-linux-edition) |
| Cómo ejecutarlo      | Terminal: active el entorno correcto y ejecute Python. <br/>Jupyter: conéctese a [Jupyter](provision-vm.md) o [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) y abra el directorio CNTK para acceder a los ejemplos. |

## <a name="pytorchhttpspytorchorg"></a>[PyTorch](https://pytorch.org/)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 1.2.0 |
| Ediciones de DSVM admitidas      | Linux |
| ¿Cómo se configura/instala en DSVM?  | Se instala en [Python 3.5](dsvm-languages.md#python-linux-edition). Se incluyen cuadernos de Jupyter de ejemplo, así como ejemplos en /dsvm/samples/pytorch. |
| Cómo ejecutarlo      | Terminal: active el entorno correcto y ejecute Python.<br/>* [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): conéctese y abra el directorio de PyTorch para obtener ejemplos.  |

## <a name="tensorflowhttpswwwtensorfloworg"></a>[TensorFlow](https://www.tensorflow.org/)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 1.13 |
| Ediciones de DSVM admitidas      | Windows, Linux |
| ¿Cómo se configura/instala en DSVM?  | Se instala en Python 3.5 en [Linux](dsvm-languages.md#python-linux-edition) y en Python 3.6 en [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) |
| Cómo ejecutarlo      | Terminal: active el entorno correcto y ejecute Python. <br/> *Jupyter: conéctese a [Jupyter](provision-vm.md) o [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) y abra el directorio TensorFlow para acceder a los ejemplos.   |

## <a name="tensorflow-servinghttpswwwtensorfloworgserving"></a>[TensorFlow Serving](https://www.tensorflow.org/serving/)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 1.12 |
| Ediciones de DSVM admitidas      | Linux |
| ¿Cómo se configura/instala en DSVM?  | tensorflow_model_server está disponible en el terminal. |
| Cómo ejecutarlo      |  Otros ejemplos están disponibles [en línea](https://www.tensorflow.org/serving/).   |


## <a name="theanohttpsgithubcomtheanotheano"></a>[Theano](https://github.com/Theano/Theano)

|    |           |
| ------------- | ------------- |
| Versiones compatibles | 1.0.3 |
| Ediciones de DSVM admitidas      | Linux |
| ¿Cómo se configura/instala en DSVM?  |Theano se instala en el entorno de Python 2.7 (_root_), así como en el entorno de Python 3.5 (_py35_). |
| Cómo ejecutarlo      |  Terminal: active la versión de Python que quiera (root o py35), ejecute Python e importe Theano.<br/>*Jupyter: seleccione el kernel de Python 2.7 o 3.5 e importe Theano.  <br/>Para solucionar un error reciente de Math Kernel Library (MKL), primero debe establecer la capa de subprocesos de MKL de la siguiente manera:<br/><br/>`export MKL_THREADING_LAYER=GNU`  |