---
title: Compilación e implementación de un modelo de detección de objetos con Azure Machine Learning Package for Computer Vision.
description: Aprenda a compilar, entrenar, probar e implementar un modelo de detección de objetos de Computer Vision con Azure Machine Learning Package for Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 06/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 1451cdc5efcb62cff5c5d3725d5b15eb44be1755
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613671"
---
# <a name="build-and-deploy-object-detection-models-with-azure-machine-learning"></a>Compilación e implementación de modelos de detección de objetos con Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


En este artículo, aprenderá a usar **Azure Machine Learning Package for Computer Vision** para entrenar, probar e implementar un modelo de detección de objetos de [Faster R-CNN](https://arxiv.org/abs/1506.01497). 

Es posible solucionar gran cantidad de problemas en el dominio Computer Vision mediante la detección de objetos. Estos problemas incluyen la creación de modelos que encuentran un número variable de objetos en una imagen. 

Al compilar e implementar este modelo con este paquete, realizará los pasos siguientes:
1.  Creación del conjunto de datos
2.  Definición del modelo de red neuronal profunda (DNN)
3.  Entrenamiento del modelo
4.  Evaluación y visualización
5.  Implementación del servicio web
6.  Prueba de carga del servicio web

En este ejemplo, se usa TensorFlow como el marco de aprendizaje profundo, el entrenamiento se realiza de manera local en una máquina con tecnología GPU, como la máquina virtual de ciencia de datos de aprendizaje profundo ([Deep learning Data Science VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)) y la implementación usa la CLI de operacionalización de Azure ML.

Consulte la [documentación de referencia del paquete](https://aka.ms/aml-packages/vision) para ver la referencia detallada de cada módulo y clase.

## <a name="prerequisites"></a>Requisitos previos

1. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

1. Deben estar configuradas e instaladas las siguientes cuentas y la siguiente aplicación:
   - Una cuenta de Experimentación de Azure Machine Learning. 
   - Una cuenta de Administración de modelos de Azure Machine Learning.
   - Azure Machine Learning Workbench instalado.

   Si todavía no se crearon o instalaron las tres, siga las instrucciones que aparecen en el artículo sobre la [instalación de Workbench y el inicio rápido de Azure Machine Learning](../desktop-workbench/quickstart-installation.md). 

1. Azure Machine Learning Package for Computer Vision debe estar instalado. Aprenda a [instalar este paquete aquí](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Notebook y datos de ejemplo

### <a name="get-the-jupyter-notebook"></a>Obtención de Jupyter Notebook

Descargue el Notebook para ejecutar el ejemplo que se describe aquí.

> [!div class="nextstepaction"]
> [Obtención de Jupyter Notebook](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Carga de los datos de ejemplo

Para esta demostración, se proporciona un conjunto de datos de artículos de supermercado dentro de frigoríficos, que consta de 30 imágenes y 8 clases (huevos, yogur, kétchup, champiñón, mostaza, naranja, calabacín y agua). Para cada imagen jpg, hay un archivo xml de anotaciones con el mismo nombre. 

En la siguiente ilustración se muestra la estructura de carpetas recomendada. 

![estructura de carpetas](media/how-to-build-deploy-object-detection-models/data_directory.JPG)

## <a name="image-annotation"></a>Anotación de imagen

Las ubicaciones de objetos anotados son necesarias para entrenar y evaluar un detector de objetos. [LabelImg](https://tzutalin.github.io/labelImg) es una herramienta de anotación de código abierto que puede usarse para anotar las imágenes. LabelImg escribe un archivo xml por imagen en formato Pascal-VOC, que este paquete puede leer. 


```python
import warnings
warnings.filterwarnings("ignore")
import os, time
from cvtk.core import Context, ObjectDetectionDataset, TFFasterRCNN
from cvtk.evaluation import DetectionEvaluation
from cvtk.evaluation.evaluation_utils import graph_error_counts
from cvtk.utils import detection_utils

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

from matplotlib import pyplot as plt
# Display the images
%matplotlib inline
```

## <a name="create-a-dataset"></a>Crear un conjunto de datos

Cree un conjunto de datos CVTK que conste de un conjunto de imágenes, con sus respectivas anotaciones en el rectángulo de selección. En este ejemplo, se usan las imágenes del frigorífico que se proporcionan en la carpeta "../sample_data/foods/training". Solo se admiten imágenes JPEG.


```python
image_folder = "detection/sample_data/foods/train"
data_train = ObjectDetectionDataset.create_from_dir(dataset_name='training_dataset', data_dir=image_folder,
                                                    annotations_dir="Annotations", image_subdirectory='JPEGImages')

# Show some statistics of the training image, and also give one example of the ground truth rectangle annotations
data_train.print_info()
_ = data_train.images[2].visualize_bounding_boxes(image_size = (10,10))
```

    F1 2018-05-25 23:12:21,727 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-05-25 23:12:21,733 INFO azureml.vision:dataset creating dataset for scenario=detection 
    Dataset name: training_dataset
    Total classes: 8, total images: 25
    Label-wise object counts:
        Label eggBox: 20 objects
        Label joghurt: 20 objects
        Label ketchup: 20 objects
        Label mushroom: 20 objects
        Label mustard: 20 objects
        Label orange: 20 objects
        Label squash: 40 objects
        Label water: 20 objects
    Bounding box width and height distribution:
        Bounding box widths  0/5/25/50/75/95/100-percentile: 54/61/79/117/133/165/311 pixels
        Bounding box heights 0/5/25/50/75/95/100-percentile: 48/58/75/124/142/170/212 pixels
    


![png](media/how-to-build-deploy-object-detection-models/output_6_1.JPG)


## <a name="define-a-model"></a>Definición de un modelo

En este ejemplo, se utiliza el modelo de Faster R-CNN. Al definir este modelo, se pueden proporcionar varios parámetros. El significado de estos parámetros, así como el de los parámetros utilizados para el entrenamiento (consulte la sección siguiente) se puede encontrar en la documentación de API de cualquier CVTK o en el [sitio web de detección de objetos de Tensorflow](https://github.com/tensorflow/models/tree/master/research/object_detection). Puede encontrar más información sobre el modelo de Faster R-CNN en [este vínculo](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Faster-R-CNN#technical-details). Este modelo está basado en Faster R-CNN y puede encontrar más información sobre él [aquí](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Fast-R-CNN#algorithm-details).


```python
score_threshold = 0.0       # Threshold on the detection score, use to discard lower-confidence detections.
max_total_detections = 300  # Maximum number of detections. A high value slows down training but might increase accuracy.
my_detector = TFFasterRCNN(labels=data_train.labels, 
                           score_threshold=score_threshold, 
                           max_total_detections=max_total_detections)
```

## <a name="train-the-model"></a>Entrenamiento del modelo

El modelo Faster R-CNN entrenado con COCO con ResNet50 se usa como punto inicial del entrenamiento. 

Para entrenar el detector, el número de pasos de entrenamiento en el código se establece en 350 para que el entrenamiento se ejecute más rápidamente (~5 minutos con GPU). En la práctica, establézcalo en al menos 10 veces el número de imágenes del conjunto de entrenamiento.

En este ejemplo, se establece el número de pasos de entrenamiento del detector en 350 para lograr un entrenamiento rápido. Sin embargo, en la práctica, una regla de oro consiste en establecer los pasos en 10 o más veces el número de imágenes del conjunto de entrenamiento.

Dos parámetros clave para el entrenamiento son:
- El número de pasos para entrenar el modelo, representado por el argumento num_seps. Cada paso entrena el modelo con un minilote de tamaño uno.
- La velocidad del aprendizaje, que se puede establecer mediante initial_learning_rate

```python
print("tensorboard --logdir={}".format(my_detector.train_dir))

# to get good results, use a larger value for num_steps, e.g., 5000.
num_steps = 350
learning_rate = 0.001 # learning rate

start_train = time.time()
my_detector.train(dataset=data_train, num_steps=num_steps, 
                  initial_learning_rate=learning_rate)
end_train = time.time()
print(end_train-start_train)
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\train
    F1 2018-05-25 23:12:22,764 INFO azureml.vision:Fit starting in experiment  1125722225 
    F1 2018-05-25 23:12:22,767 INFO azureml.vision:model starting trainging for scenario=detection 
    Using existing checkpoint file that's saved at 'C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\models\detection\faster_rcnn_resnet50_coco_2018_01_28\model.ckpt.index'.
    TFRecords creation started.
    F1 2018-05-25 23:12:22,773 INFO On image 0 of 25
    TFRecords creation completed.
    Training started.
    Training progressing: step 0 ...
    Training progressing: step 100 ...
    Training progressing: step 200 ...
    Training progressing: step 300 ...
    F1 2018-05-25 23:18:02,730 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:18:10,722 INFO 2953 ops in the final graph.
    F1 2018-05-25 23:18:24,244 INFO azureml.vision:Fit finished in experiment  1125722225 
    Training completed.
    361.604615688324
    

Es posible usar TensorBoard para visualizar el progreso del entrenamiento. Los eventos de TensorBoard se encuentran en la carpeta especificada por el atributo train_dir del objeto de modelo. Para ver TensorBoard, siga estos pasos:
1. Copie la tarea de impresión que empieza por 'tensorboard--logdir' en una línea de comandos y ejecútela. 
2. Copie la dirección URL que se devuelve desde la línea de comandos en un explorador web para ver TensorBoard. 

TensorBoard debe presentar un aspecto similar a la siguiente captura de pantalla. Se tarda solo unos instantes en rellenar la carpeta de entrenamiento. Por tanto, si TensorBoard no aparece correctamente la primera vez, intente repetir los pasos 1 y 2.  

![tensorboard](media/how-to-build-deploy-object-detection-models/tensorboard.JPG)

## <a name="evaluate-the-model"></a>Evaluación del modelo

Se usa el método "evaluate" para evaluar el modelo. Esta función requiere un objeto ObjectDetectionDataset como entrada. El conjunto de datos de evaluación se puede crear mediante la misma función que se usa para el conjunto de datos de entrenamiento. La métrica compatible es Precisión media tal como se define en [PASCAL VOC Challenge](http://host.robots.ox.ac.uk/pascal/VOC/pubs/everingham10.pdf) (Desafío PASCAL VOC).  


```python
image_folder = "detection/sample_data/foods/test"
data_val = ObjectDetectionDataset.create_from_dir(dataset_name='val_dataset', data_dir=image_folder)
eval_result = my_detector.evaluate(dataset=data_val)
```

    F1 2018-05-25 23:18:24,253 INFO azureml.vision:dataset creating dataset for scenario=detection 
    F1 2018-05-25 23:18:24,286 INFO On image 0 of 5
    F1 2018-05-25 23:18:29,300 INFO Starting evaluation at 2018-05-26-03:18:29
    F1 2018-05-25 23:18:32,403 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:33,158 INFO Detection visualizations written to summary with tag image-0.
    F1 2018-05-25 23:18:33,518 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:34,342 INFO Detection visualizations written to summary with tag image-1.
    F1 2018-05-25 23:18:34,714 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:35,470 INFO Detection visualizations written to summary with tag image-2.
    F1 2018-05-25 23:18:35,835 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:36,654 INFO Detection visualizations written to summary with tag image-3.
    F1 2018-05-25 23:18:37,010 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:37,798 INFO Detection visualizations written to summary with tag image-4.
    F1 2018-05-25 23:18:37,804 INFO Running eval batches done.
    F1 2018-05-25 23:18:37,805 INFO # success: 5
    F1 2018-05-25 23:18:37,806 INFO # skipped: 0
    F1 2018-05-25 23:18:38,119 INFO Writing metrics to tf summary.
    F1 2018-05-25 23:18:38,121 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/eggBox: 1.000000
    F1 2018-05-25 23:18:38,205 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/joghurt: 0.942857
    F1 2018-05-25 23:18:38,206 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/ketchup: 1.000000
    F1 2018-05-25 23:18:38,207 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mushroom: 1.000000
    F1 2018-05-25 23:18:38,208 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mustard: 1.000000
    F1 2018-05-25 23:18:38,209 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/orange: 1.000000
    F1 2018-05-25 23:18:38,210 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/squash: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/water: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/Precision/mAP@0.5IOU: 0.992857
    F1 2018-05-25 23:18:38,253 INFO Metrics written to tf summary.
    F1 2018-05-25 23:18:38,254 INFO Finished evaluation!
    

Los resultados de la evaluación se pueden imprimir en un formato correcto.


```python
# print out the performance metric values
for label_obj in data_train.labels:
    label = label_obj.name
    key = 'PASCAL/PerformanceByCategory/AP@0.5IOU/' + label
    print('{0: <15}: {1: <3}'.format(label, round(eval_result[key], 2)))
print('{0: <15}: {1: <3}'.format("overall:", round(eval_result['PASCAL/Precision/mAP@0.5IOU'], 2))) 
```

    joghurt        : 0.94
    squash         : 1.0
    mushroom       : 1.0
    eggBox         : 1.0
    ketchup        : 1.0
    mustard        : 1.0
    water          : 1.0
    orange         : 1.0
    overall:       : 0.99
    

De forma similar, puede calcular la precisión del modelo en el conjunto de entrenamiento. Esto le permitirá asegurarse de que el entrenamiento se convertirá en una buena solución. La precisión del conjunto de entrenamiento después de un entrenamiento correcto es, a menudo, cercana al 100%.

Los resultados de la evaluación también se pueden ver en TensorBoard, incluidos los valores y las imágenes mAP con rectángulos de selección predichos. Copie la tarea de impresión del siguiente código en una ventana de la línea de comandos para iniciar el cliente de TensorBoard. Aquí se emplea un valor de puerto 8008 para evitar el conflicto con el valor predeterminado de 6006, que se estaba utilizando para ver el estado del entrenamiento.


```python
print("tensorboard --logdir={} --port=8008".format(my_detector.eval_dir))
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\eval --port=8008
    

## <a name="score-an-image"></a>Puntuación de una imagen

Cuando esté satisfecho con el rendimiento del modelo entrenado, se puede usar la función "score" del objeto de modelo para puntuar nuevas imágenes. Las puntuaciones devueltas se pueden visualizar con la función "visualize". 


```python
image_path = data_val.images[1].storage_path
detections_dict = my_detector.score(image_path)
path_save = "./scored_images/scored_image_preloaded.jpg"
ax = detection_utils.visualize(image_path, detections_dict, image_size=(8, 12))
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

![png](media/how-to-build-deploy-object-detection-models/output_20_0.JPG)

##  <a name="save-the-model"></a>Guardar el modelo

El modelo entrenado se puede guardar en un disco y cargar de nuevo en memoria, como se indica en los siguientes ejemplos de código.


```python
save_model_path = "./frozen_model/faster_rcnn.model"
my_detector.save(save_model_path)
```

    F1 2018-05-25 23:18:55,166 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:19:03,867 INFO 2953 ops in the final graph.
    

## <a name="load-the-saved-model-for-scoring"></a>Carga del modelo guardado para puntuación

Para usar el modelo guardado, cárguelo en memoria con la función "load". Solo es necesario cargarlo una vez. 

```python
my_detector_loaded = TFFasterRCNN.load(save_model_path)
```

Una vez cargado el modelo, se puede usar para puntuar una imagen o una lista de imágenes. Para una sola imagen, se devuelve un diccionario con claves, como "detection_boxes", "detection_scores" y "num_detections". Si la entrada es una lista de imágenes, se devuelve una lista de diccionarios, con un diccionario para cada imagen. 


```python
detections_dict = my_detector_loaded.score(image_path)
```

Los objetos detectados con puntuaciones por encima de 0,5, incluidas las etiquetas, puntuaciones y coordenadas se pueden imprimir.


```python
look_up = dict((v,k) for k,v in my_detector.class_map.items())
n_obj = 0
for i in range(detections_dict['num_detections']):
    if detections_dict['detection_scores'][i] > 0.5:
        n_obj += 1
        print("Object {}: label={:11}, score={:.2f}, location=(top: {:.2f}, left: {:.2f}, bottom: {:.2f}, right: {:.2f})".format(
            i, look_up[detections_dict['detection_classes'][i]], 
            detections_dict['detection_scores'][i], 
            detections_dict['detection_boxes'][i][0],
            detections_dict['detection_boxes'][i][1], 
            detections_dict['detection_boxes'][i][2],
            detections_dict['detection_boxes'][i][3]))    
        
print("\nFound {} objects in image {}.".format(n_obj, image_path))           
```

    Object 0: label=squash     , score=0.99, location=(top: 0.74, left: 0.30, bottom: 0.84, right: 0.42)
    Object 1: label=squash     , score=0.98, location=(top: 0.27, left: 0.21, bottom: 0.37, right: 0.33)
    Object 2: label=orange     , score=0.98, location=(top: 0.31, left: 0.39, bottom: 0.37, right: 0.48)
    Object 3: label=joghurt    , score=0.98, location=(top: 0.57, left: 0.29, bottom: 0.67, right: 0.39)
    Object 4: label=eggBox     , score=0.97, location=(top: 0.41, left: 0.53, bottom: 0.49, right: 0.69)
    Object 5: label=water      , score=0.95, location=(top: 0.23, left: 0.51, bottom: 0.37, right: 0.57)
    Object 6: label=mustard    , score=0.88, location=(top: 0.61, left: 0.47, bottom: 0.66, right: 0.57)
    Object 7: label=ketchup    , score=0.80, location=(top: 0.62, left: 0.62, bottom: 0.68, right: 0.72)
    
    Found 8 objects in image ../sample_data/foods/test\JPEGImages\10.jpg.
    

Visualice las puntuaciones como antes.


```python
path_save = "./scored_images/scored_image_frozen_graph.jpg"
ax = detection_utils.visualize(image_path, detections_dict, path_save=path_save, image_size=(8, 12))
# ax.get_figure() # use this code extract the returned image
```

![png](media/how-to-build-deploy-object-detection-models/output_30_0.JPG)

## <a name="operationalization-deploy-and-consume"></a>Operacionalización: implementación y consumo

La operacionalización es el proceso de publicar modelos y código como servicios web y el consumo de estos servicios para generar resultados empresariales. 

Una vez que el modelo está entrenado, puede implementarlo como servicio web para su consumo mediante la [CLI de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Los modelos se pueden implementar en una máquina local o en un clúster de Azure Container Service (ACS). Con ACS, puede escalar manualmente el servicio web o usar la funcionalidad de escalado automático.

**Inicio de sesión con la CLI de Azure**

Con una cuenta [Azure](https://azure.microsoft.com/) con una suscripción válida, inicie sesión con el comando de CLI siguiente:
<br>`az login`

+ Para cambiar a otra suscripción de Azure, use el comando:
<br>`az account set --subscription [your subscription name]`

+ Para ver la cuenta de administración del modelo actual, use el comando:
  <br>`az ml account modelmanagement show`

**Creación y establecimiento del entorno de implementación de clústeres**

Solo tiene que establecer el entorno de implementación una vez. Si todavía no tiene uno, configure ahora mismo el entorno de implementación con [estas instrucciones](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Para ver el entorno de implementación activo, use el comando de CLI siguiente:
<br>`az ml env show`
   
Comando de la CLI de Azure de ejemplo para crear y establecer el entorno de implementación

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Administración de servicios web e implementaciones

Es posible usar las API siguientes para implementar modelos como servicios web, administrar esos servicios web y administrar las implementaciones.

|Task|API|
|----|----|
|Crear objeto de implementación|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Implementar el servicio web|`deploy_obj.deploy()`|
|Puntuar la imagen|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Eliminar el servicio web|`deploy_obj.delete()`|
|Compilar una imagen de Docker sin servicio web|`deploy_obj.build_docker_image()`|
|Mostrar la implementación existente|`AMLDeployment.list_deployment()`|
|Eliminar si existe el servicio con el nombre de la implementación|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**Documentación de la API:** consulte la [documentación de referencia del paquete](https://aka.ms/aml-packages/vision) para ver la referencia detallada de cada módulo y clase.

**Referencia de la CLI:** para opciones más avanzadas relativas a la implementación, consulte la [referencia de la CLI de administración de modelos](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Administración de implementación en Azure Portal**: puede hacer seguimiento de las implementaciones y administrarlas en [Azure Portal](https://ms.portal.azure.com/). En Azure Portal, busque la página de su cuenta de Administración de modelos de Machine Learning mediante su nombre. Luego vaya a la página de la cuenta de Administración de modelos > Administración de modelos > Servicios.

```python
# ##### OPTIONAL - Interactive CLI setup helper ###### 
# # Interactive CLI setup helper, including model management account and deployment environment.
# # If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
# # UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT

# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Create deployment object
# It will use the current deployment environment (you can check it with CLI command "az ml env show").
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=my_detector, replicas=1)

# Alternatively, you can provide azure machine learning deployment cluster name (environment name) and resource group name
# to deploy your model. It will use the provided cluster to deploy. To do that, please uncomment the following lines to create 
# the deployment object.

# azureml_rscgroup = "<resource group>"
# cluster_name = "<cluster name>"
# deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=my_detector,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# create the webservice
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Consumo del servicio web

Una vez creado el servicio web, puede puntuar imágenes con el servicio web implementado. Tiene varias opciones:

   - Puede puntuar directamente el servicio web con el objeto de implementación con: deploy_obj.score_image(image_path_or_url) 
   - O bien, puede usar la dirección URL del punto de conexión de servicio y la clave de este (ninguna para la implementación local) con: AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
   - Formar las solicitudes HTTP directamente para puntuar el punto de conexión del servicio web (solo para usuarios avanzados).

### <a name="score-with-existing-deployment-object"></a>Puntuación con objeto de implementación existente
```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = data_train.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])

```


```python
# Time image scoring
import timeit

num_images = 3
for img_index, img_obj in enumerate(data_train.images[:num_images]):
    print("Calling API for image {} of {}: {}...".format(img_index, num_images, img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Puntuación con dirección URL del punto de conexión de servicio y clave de servicio
```
    AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
```


```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "http://xxx" # please replace with your own service url
service_key = "xxx" # please replace with your own service key

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])
```

### <a name="score-endpoint-with-http-request-directly"></a>Puntuación de punto de conexión con solicitud HTTP directamente
El código de ejemplo siguiente forma la solicitud HTTP directamente en Python. Puede hacerlo en otros lenguajes de programación.


```python
def score_image_with_http(image, service_endpoint_url, service_key=None, parameters={}):
    """Score local image with http request

    Args:
        image (str): Image file path
        service_endpoint_url(str): web service endpoint url
        service_key(str): Service key. None for local deployment.
        parameters (dict): Additional request parameters in dictionary. Default is {}.


    Returns:
        str: serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    import json

    if service_key is None:
        headers = {'Content-Type': 'application/json'}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key)}
    payload = []
    encoded = None
    
    # Read image
    with open(image,'rb') as f:
        image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        
    # Convert your image to base64 string
    # image_in_base64 : "b'{base64}'"
    encoded = base64.b64encode(image_buffer.getvalue())
    image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
    payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
        json.loads(result[0])
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result[0]

```

### <a name="parse-serialized-result-from-webservice"></a>Análisis del resultado serializado del servicio web
El resultado del servicio web está en la cadena json que se puede analizar.


```python
image_path_or_url = image_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])
```


```python
# Parse result from json string
import numpy as np
parsed_result = TFFasterRCNN.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
```


```python
ax = detection_utils.visualize(image_path, parsed_result)
path_save = "./scored_images/scored_image_web.jpg"
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Azure Machine Learning Package for Computer Vision en estos artículos:

+ Lea la [información general del paquete](https://aka.ms/aml-packages/vision).

+ Explore la [documentación de referencia](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) de este paquete.

+ Obtenga información sobre [otros paquetes Python para Azure Machine Learning](reference-python-package-overview.md).
