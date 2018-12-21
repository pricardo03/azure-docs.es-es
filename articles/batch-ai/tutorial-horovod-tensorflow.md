---
title: 'Tutorial: entrenamiento distribuido con Azure Batch AI y Horovod | Microsoft Docs'
description: 'Tutorial: entrenamiento de un modelo distribuido con Horovod mediante el servicio Azure Batch AI y la CLI de Azure.'
services: batch-ai
author: johnwu10
manager: jeconnoc
ms.service: batch-ai
ms.topic: tutorial
ms.date: 09/03/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 45255845d8645391ee33471830ac2ef27870a40d
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408636"
---
# <a name="tutorial-train-a-distributed-model-with-horovod"></a>Tutorial: Entrenamiento de un modelo distribuido con Horovod

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

En este tutorial se entrena un modelo de aprendizaje profundo distribuido mediante su ejecución en paralelo en varios nodos de un clúster de Batch AI. Batch AI es un servicio administrado para el entrenamiento de modelos de aprendizaje automático y de inteligencia artificial a escala en clústeres con unidades de GPU de Azure. 

Este tutorial presenta un flujo de trabajo común de Batch AI y muestra cómo se interactúa con los recursos de Batch AI mediante la CLI de Azure. Temas cubiertos:

> [!div class="checklist"]
> * Configuración de un área de trabajo de Batch AI, el experimento y el clúster
> * Configuración de un recurso compartido de archivos de Azure para la entrada y la salida
> * Establecimiento de paralelismos en un modelo de aprendizaje profundo con Horovod
> * Envío de un trabajo de entrenamiento
> * Supervisión del trabajo
> * Recuperación de los resultados del entrenamiento

Para este tutorial se modifica un modelo de detección de objetos para ejecutarlo en paralelo con [Horovod](https://github.com/uber/horovod). El modelo se entrena en el [conjunto de datos CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html) de imágenes. El trabajo de entrenamiento se ejecuta en un clúster que contiene 24 CPU virtuales y 4 GPU y tarda unos 60 minutos en completarse.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tutorial es preciso que ejecute la CLI de Azure de la versión 2.0.38 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

## <a name="why-use-horovod"></a>¿Por qué usar Horovod?

Horovod es una plataforma de entrenamiento distribuido para Tensorflow, Keras y PyTorch, y es la que se utiliza para este tutorial. Con Horovod puede convertir un script de entrenamiento diseñado para ejecutarse en una sola unidad GPU en uno que se ejecuta eficazmente en un sistema distribuido con pocas líneas de código.

Además de Horovod, Batch AI admite el entrenamiento distribuido con otras plataformas de código abierto populares distintas. No olvide revisar los términos de licencia de cualquier plataforma que utilice para entrenar modelos en producción.

## <a name="prepare-the-batch-ai-environment"></a>Preparación del entorno de Batch AI

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Use el comando `az group create` para crear un grupo de recursos denominado `batchai.horovod` en la región `eastus`. Use el grupo de recursos para implementar los recursos de Batch AI.

```azurecli-interactive
az group create --name batchai.horovod --location eastus
```

### <a name="create-a-workspace"></a>Crear un área de trabajo

Use el comando `az batchai workspace create` para crear el área de trabajo de Batch AI. Un área de trabajo es una colección de nivel máximo de otros recursos de Batch AI. El siguiente comando crea un área de trabajo denominado `batchaidev` en el grupo de recursos.

```azurecli-interactive
az batchai workspace create --resource-group batchai.horovod --workspace batchaidev 
```

### <a name="create-an-experiment"></a>Creación de un experimento

Un experimento de Batch AI agrupa uno o varios trabajos que se consultan y administran conjuntamente. El siguiente comando `az batchai experiment create` crea un experimento denominado `cifar` en el área de trabajo y el grupo de recursos.

```azurecli-interactive
az batchai experiment create --resource-group batchai.horovod --workspace batchaidev --name cifar 
```

## <a name="set-up-a-gpu-cluster"></a>Configuración de un clúster de GPU

A continuación, configure un clúster de GPU para ejecutar el experimento. Batch AI proporciona un rango flexible de opciones para personalizar los clústeres para necesidades específicas.

El siguiente comando `az batchai cluster create` crea un clúster de 4 nodos denominado `nc6cluster` en el área de trabajo y el grupo de recursos. De forma predeterminada, las máquinas virtuales del clúster ejecutan una imagen de Ubuntu Server diseñada para hospedar aplicaciones en contenedores. Los nodos del clúster en este ejemplo usan el tamaño `Standard_NC6`, que contiene una unidad GPU NVIDIA Tesla K80.

```azurecli-interactive
az batchai cluster create --resource-group batchai.horovod --workspace batchaidev --name nc6cluster --vm-priority dedicated  --vm-size Standard_NC6 --target 4 --generate-ssh-keys
```

Ejecute el comando `az batchai cluster show` para ver el estado del clúster. El aprovisionamiento total del clúster suele tardar unos minutos.

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --output table
```

Al principio de la creación del clúster, el clúster se encuentra en estado `resizing`. Siga estos pasos mientras el estado del clúster cambia. El clúster está preparado para ejecutar el trabajo de entrenamiento cuando el estado es `steady` y los nodos, `idle`. Por ejemplo: 

```
Name        Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
----------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
nc6cluster  batchai.horovod  batchaidev   STANDARD_NC6  steady        4          0            0          0           0
```

## <a name="set-up-storage"></a>Configuración del almacenamiento

Use el comando `az storage account create` para crear una cuenta de almacenamiento, y almacenar el script y la salida del entrenamiento.

```azurecli-interactive
az storage account create --resource-group batchai.horovod --name mystorageaccount --location eastus --sku Standard_LRS
```

Cree un recurso compartido de archivos de Azure denominado `myshare` en la cuenta mediante el comando `az storage share create`:

```azurecli-interactive
az storage share create --name myshare --account-name mystorageaccount
```

En la práctica, este mismo almacenamiento puede usarse para varios trabajos y experimentos. Para mantener las cosas organizadas, cree un directorio en el recurso compartido de archivos para almacenar los archivos relacionados con este experimento en concreto. Con el siguiente comando `az storage directory create` se crea un directorio denominado `cifar`.

```azurecli-interactive
az storage directory create --name cifar --share-name myshare --account-name mystorageaccount
```

El paso siguiente consiste en preparar el script de entrenamiento real y cargarlo en el directorio recién creado.

## <a name="create-the-training-script"></a>Creación del script de entrenamiento

En este experimento se ejecuta un script de Python que se actualiza con algunos cambios para poder ejecutar un modelo de detección de objetos en paralelo mediante Horovod. El [modelo original](https://raw.githubusercontent.com/keras-team/keras/master/examples/cifar10_cnn.py) usa Keras con un back-end de TensorFlow. 

En un directorio de trabajo del shell, use el editor de texto que prefiera para crear un archivo denominado `cifar_cnn_distributed.py` con el siguiente contenido. Los cambios realizados en el código fuente original se han comentado con un prefijo `HOROVOD`.

```python
from __future__ import print_function
import keras
from keras.datasets import cifar10
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation, Flatten
from keras.layers import Conv2D, MaxPooling2D
import tensorflow as tf
import horovod.keras as hvd
import os
from keras import backend as K
import math
import argparse 

# HOROVOD: initialize Horovod.
hvd.init()

# HOROVOD: pin GPU to be used to process local rank (one GPU per process)
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
config.gpu_options.visible_device_list = str(hvd.local_rank())
K.set_session(tf.Session(config=config))

batch_size = 32
num_classes = 10
# HOROVOD: adjust number of epochs based on number of GPUs.
epochs = int(math.ceil(100.0 / hvd.size()))

data_augmentation = True
num_predictions = 20
# BATCH AI: change save directory to mounted storage path
parser = argparse.ArgumentParser()
parser.add_argument("-d", "--dir", help="directory to save model to")
args = parser.parse_args()
save_dir = os.path.join(args.dir, 'saved_models')
model_name = 'keras_cifar10_trained_model.h5'

# The data, split between train and test sets:
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
print('x_train shape:', x_train.shape)
print(x_train.shape[0], 'train samples')
print(x_test.shape[0], 'test samples')

# Convert class vectors to binary class matrices.
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

model = Sequential()
model.add(Conv2D(32, (3, 3), padding='same',
                 input_shape=x_train.shape[1:]))
model.add(Activation('relu'))
model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(64, (3, 3), padding='same'))
model.add(Activation('relu'))
model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Flatten())
model.add(Dense(512))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(num_classes))
model.add(Activation('softmax'))

# HOROVOD: adjust learning rate based on number of GPUs.
opt = keras.optimizers.rmsprop(lr=0.0001 * hvd.size(), decay=1e-6)

# HOROVOD: add Horovod Distributed Optimizer.
opt = hvd.DistributedOptimizer(opt)

# Let's train the model using RMSprop
model.compile(loss='categorical_crossentropy',
              optimizer=opt,
              metrics=['accuracy'])

callbacks = [
    # HOROVOD: broadcast initial variable states from rank 0 to all other processes.
    # This is necessary to ensure consistent initialization of all workers when
    # training is started with random weights or restored from a checkpoint.
    hvd.callbacks.BroadcastGlobalVariablesCallback(0),
]

# HOROVOD: save checkpoints only on worker 0 to prevent other workers from corrupting them.
if hvd.rank() == 0:
    callbacks.append(keras.callbacks.ModelCheckpoint('./checkpoint-{epoch}.h5'))

x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
x_train /= 255
x_test /= 255

if not data_augmentation:
    print('Not using data augmentation.')
    model.fit(x_train, y_train,
              batch_size=batch_size,
              epochs=epochs,
              validation_data=(x_test, y_test),
              shuffle=True)
else:
    print('Using real-time data augmentation.')
    # This will do preprocessing and realtime data augmentation:
    datagen = ImageDataGenerator(
        featurewise_center=False,  # set input mean to 0 over the dataset
        samplewise_center=False,  # set each sample mean to 0
        featurewise_std_normalization=False,  # divide inputs by std of the dataset
        samplewise_std_normalization=False,  # divide each input by its std
        zca_whitening=False,  # apply ZCA whitening
        zca_epsilon=1e-06,  # epsilon for ZCA whitening
        rotation_range=0,  # randomly rotate images in the range (degrees, 0 to 180)
        width_shift_range=0.1,  # randomly shift images horizontally (fraction of total width)
        height_shift_range=0.1,  # randomly shift images vertically (fraction of total height)
        shear_range=0.,  # set range for random shear
        zoom_range=0.,  # set range for random zoom
        channel_shift_range=0.,  # set range for random channel shifts
        fill_mode='nearest',  # set mode for filling points outside the input boundaries
        cval=0.,  # value used for fill_mode = "constant"
        horizontal_flip=True,  # randomly flip images
        vertical_flip=False,  # randomly flip images
        rescale=None,  # set rescaling factor (applied before any other transformation)
        preprocessing_function=None,  # set function that will be applied on each input
        data_format=None,  # image data format, either "channels_first" or "channels_last"
        validation_split=0.0)  # fraction of images reserved for validation (strictly between 0 and 1)

    # Compute quantities required for feature-wise normalization
    # (std, mean, and principal components if ZCA whitening is applied).
    datagen.fit(x_train)

    # Fit the model on the batches generated by datagen.flow().
    model.fit_generator(datagen.flow(x_train, y_train,
                                     batch_size=batch_size),
                        epochs=epochs,
                        validation_data=(x_test, y_test),
                        workers=4)

# Save model and weights
if not os.path.isdir(save_dir):
    os.makedirs(save_dir)
model_path = os.path.join(save_dir, model_name)
model.save(model_path)
print('Saved trained model at %s ' % model_path)

# Score trained model.
scores = model.evaluate(x_test, y_test, verbose=1)
print('Test loss:', scores[0])
print('Test accuracy:', scores[1])
```

Tal como se muestra en este ejemplo, para habilitar el entrenamiento distribuid mediante la plataforma Horovod solo se necesitan algunas actualizaciones en el modelo. 

Tenga en cuenta que este script usa un modelo relativamente pequeño y un conjunto de datos con fines de demostración, por lo que el modelo distribuido no mostrará necesariamente una mejora considerable en el rendimiento. Para ver realmente la eficacia del entrenamiento distribuido, utilice un modelo y un conjunto mucho mayores.

## <a name="upload-the-training-script"></a>Carga del script de entrenamiento

Con el script listo, el paso siguiente es cargarlo en el directorio del recurso compartido de archivos que creó anteriormente. El siguiente comando `az storage file upload` lo carga desde el directorio de trabajo local en la ubicación adecuada.

```azurecli-interactive
az storage file upload --path cifar --share-name myshare --source cifar_cnn_distributed.py --account-name mystorageaccount
```

## <a name="submit-the-training-job"></a>Envío del trabajo de entrenamiento

Después de completar los pasos anteriores, cree un trabajo de entrenamiento. En Batch AI se usa un archivo `job.json` para definir los parámetros de ejecución del trabajo. Con su editor de texto favorito, cree un archivo de configuración para el trabajo que se llame `job.json` con el siguiente contenido.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 4,
        "horovodSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar/cifar_cnn_distributed.py",
            "commandLineArgs": "--dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "jobPreparation": {
            "commandLine": "apt update; apt install mpi-default-dev mpi-default-bin -y; pip install keras horovod"
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Explicación de las propiedades:

| Propiedad | DESCRIPCIÓN |
| --------- | --------- |
| `nodeCount` | Número de nodos que se dedicarán al trabajo. En este caso el trabajo se ejecutará en paralelo en `4` nodos. |
| `horovodSettings` | El campo `pythonScriptFilePath` define la ruta de acceso al script de Horovod, que se encuentra en el directorio `cifar` creado anteriormente. El campo `commandLineArgs` son los argumentos de la línea de comandos para ejecutar el script. Para este experimento, el directorio donde se guarda el modelo de es el único argumento necesario. `$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare` es la ruta de acceso donde se ha montado el recurso compartido de archivos. | 
| `stdOutErrPathPrefix` | Ruta de acceso para almacenar las salidas y los registros del trabajo, que en este ejemplo es el mismo directorio `cifar`. |
| `jobPreparation` | Instrucciones especiales para preparar el entorno para ejecutar el trabajo. Este script requiere la instalación de los paquetes de MPI y Horovod indicados. |
| `containerSettings` | Configuración para el contenedor donde se ejecuta el trabajo. Este trabajo utiliza un contenedor de Docker compilado con `tensorflow`.

Con la configuración, cree el trabajo mediante el comando `az batchai job create`. El siguiente comando pone en cola un trabajo denominado `cifar_distributed` con todos los recursos que se han establecido hasta este punto. 

```azurecli-interactive
az batchai job create --cluster nc6cluster --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --experiment cifar --config-file job.json --storage-account-name mystorageaccount
```

Si los nodos están ocupados actualmente, el trabajo puede tardar unos instantes en iniciar la ejecución. Use el comando `az batchai job show` para ver el estado de ejecución del trabajo.

```azurecli-interactive
az batchai job show --experiment cifar --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --query "executionState"
```

### <a name="visualize-the-distributed-training"></a>Visualización del entrenamiento distribuido

Cuando el trabajo haya empezado a ejecutarse, utilice el comando `az batchai cluster show` de nuevo para consultar el estado de los nodos del clúster. 

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --query "nodeStateCounts"
```

La salida debe ser similar a la siguiente, que muestra los cuatro nodos en un estado de ejecución. Este resultado muestra que los cuatro nodos se están utilizando actualmente en el entrenamiento distribuido.

```
{
  "idleNodeCount": 0,
  "leavingNodeCount": 0,
  "preparingNodeCount": 0,
  "runningNodeCount": 4,
  "unusableNodeCount": 0
}
```

## <a name="monitor-the-job"></a>Supervisión del trabajo

### <a name="list-output-files"></a>Enumeración de los archivos de salida

Con el trabajo en ejecución, use el comando `az batchai job file list` para enumerar los archivos de salida que este genera.

```azurecli-interactive
az batchai job file list --experiment cifar --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev --output table
```

Para este experimento en concreto, la salida debe ser similar a la siguiente. La salida global del trabajo se registra en `stdout.txt` mientras `stderr.txt` indica como salida los errores que se produjeran durante la ejecución principal. Los demás archivos son los registros de salida, errores y preparación del trabajo correspondientes a cada nodo.

```
Name                                                    Type       Size  Modified
------------------------------------------------------  ------  -------  -------------------------
execution-tvm-676767296_1-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_2-20180718t174802z-p.log        file      15094  2018-07-18T22:41:55+00:00
execution-tvm-676767296_3-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_4-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
stderr-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr.txt                                              file       7653  2018-07-18T22:46:32+00:00
stdout-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout.txt                                              file    2316480  2018-07-18T22:46:32+00:00
```

### <a name="stream-an-output-file"></a>Flujo de un archivo de salida

Use el comando `az batchai job file stream` para transmitir el contenido de un archivo. En el siguiente ejemplo se transmite el registro de salida principal.

```azurecli-interactive
az batchai job file stream --experiment cifar --file-name stdout.txt --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev
```

Con el trabajo en ejecución, el comando transmite la salida estándar del trabajo de entrenamiento y muestra una salida similar a la siguiente.

```
...
50000 train samples
10000 test samples
Using real-time data augmentation.
Epoch 1/25


   1/1563 [..............................] - ETA: 2:42:25 - loss: 2.3334 - acc: 0.0312   1/1563 [..............................] - ETA: 2:30:42 - loss: 2.2973 - acc: 0.0938
   1/1563 [..............................] - ETA: 30:36 - loss: 2.3175 - acc: 0.1250
   1/1563 [..............................] - ETA: 2:32:58 - loss: 2.3489 - acc: 0.0625
   2/1563 [..............................] - ETA: 1:21:59 - loss: 2.3230 - acc: 0.0625

   2/1563 [..............................]   2/1563 [..............................] - ETA: 1:16:09 - loss: 2.2913 - acc: 0.0938 - ETA: 1:17:15 - loss: 2.3147 - acc: 0.0781
   2/1563 [..............................] - ETA: 16:07 - loss: 2.3678 - acc: 0.0938
   3/1563 [..............................] - ETA: 55:05 - loss: 2.3232 - acc: 0.0938  
   3/1563 [..............................] - ETA: 51:57 - loss: 2.3185 - acc: 0.1146  
   3/1563 [..............................] - ETA: 51:12 - loss: 2.3179 - acc: 0.1042  
   3/1563 [..............................] - ETA: 11:13 - loss: 2.3504 - acc: 0.0833
   4/1563 [..............................] - ETA: 39:43 - loss: 2.3224 - acc: 0.1094
   4/1563 [..............................] - ETA: 42:09 - loss: 2.3049 - acc: 0.1250
   4/1563 [..............................] - ETA: 39:15 - loss: 2.3089 - acc: 0.1094
   4/1563 [..............................] - ETA: 9:16 - loss: 2.3316 - acc: 0.1016 
   5/1563 [..............................] - ETA: 39:51 - loss: 2.3153 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:58 - loss: 2.3197 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:35 - loss: 2.3148 - acc: 0.1062
   5/1563 [..............................] - ETA: 13:38 - loss: 2.3263 - acc: 0.1062
   6/1563 [..............................] - ETA: 35:48 - loss: 2.3168 - acc: 0.1198

   6/1563 [..............................]   6/1563 [..............................] - ETA: 34:13 - loss: 2.3142 - acc: 0.1198 - ETA: 33:51 - loss: 2.3162 - acc: 0.1042
   6/1563 [..............................] - ETA: 13:54 - loss: 2.3225 - acc: 0.1094
   7/1563 [..............................] - ETA: 30:53 - loss: 2.3181 - acc: 0.1071

   7/1563 [..............................]   7/1563 [..............................] - ETA: 29:32 - loss: 2.3149 - acc: 0.1161 - ETA: 29:13 - loss: 2.3140 - acc: 0.0938
   7/1563 [..............................] - ETA: 12:09 - loss: 2.3174 - acc: 0.1205
   8/1563 [..............................] - ETA: 26:04 - loss: 2.3113 - acc: 0.1133
   8/1563 [..............................] - ETA: 27:15 - loss: 2.3169 - acc: 0.1133
   8/1563 [..............................] - ETA: 10:51 - loss: 2.3152 - acc: 0.1172
...
```

El script entrena en más de 25 épocas o pasa por el conjunto de datos de entrenamiento. Este proceso tarda unos 60 minutos. 

## <a name="retrieve-the-results"></a>Recuperación de los resultados

Cuando finalice el script, si todo ha ido bien, la precisión de validación debe ser entre el 70-75 % y el modelo entrenado se guarda en el recurso compartido de archivos de `cifar/saved_models/keras_cifar10_trained_model.h5`. 

El entrenamiento del modelo suele formar parte de un flujo de trabajo mayor. Por ejemplo, podría exponer el modelo entrenado en otra aplicación. Para descargar el modelo entrenado a un entorno local, use el comando `az storage file download`. 

```azurecli-interactive
az storage file download --path cifar/saved_models/keras_cifar10_trained_model.h5 --share-name myshare --account-name mystorageaccount
```
## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado la ejecución de los trabajos, un procedimiento recomendado para ahorrar costos de proceso es reducir todos los clústeres a `0 nodes` para que no se cobre por tiempo de inactividad. Use el siguiente comando `az batchai cluster resize`. 

```azurecli-interactive
az batchai cluster resize --name nc6cluster --resource-group batchai.horovod --target 0 --workspace batchaidev
```

Más adelante, vuelva a cambiarlo de tamaño a 1 o más nodos para ejecutar los trabajos. 

Si no va a usar la cuenta de almacenamiento ni el área de trabajo en el futuro, elimine el grupo de recursos mediante el comando `az group delete`. Al eliminar un grupo de recursos se eliminan todos los recursos que forman parte de él.

```azurecli-interactive
az group delete --name batchai.horovod
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Configuración de un área de trabajo de Batch AI, el experimento y el clúster
> * Configuración de un recurso compartido de archivos de Azure para la entrada y la salida
> * Establecimiento de paralelismos en un modelo mediante Horovod
> * Envío de un trabajo de entrenamiento
> * Supervisión del trabajo
> * Recuperación de los resultados del entrenamiento

Para ejemplos de uso de Batch AI con diferentes plataformas, consulte las instrucciones en GitHub.

> [!div class="nextstepaction"]
> [Instrucciones para Batch AI](https://github.com/Azure/BatchAI/tree/master/recipes)
