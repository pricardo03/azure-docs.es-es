---
title: Creación y uso de destinos de proceso para el entrenamiento del modelo
titleSuffix: Azure Machine Learning service
description: Configurar los entornos de entrenamiento (destinos de proceso) del entrenamiento del modelo de Machine Learning. Es fácil cambiar entre entornos de entrenamiento. Inicie el entrenamiento de forma local. Si necesita escalar horizontalmente, cambie a un destino de proceso basado en la nube.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/07/2019
ms.custom: seodec18
ms.openlocfilehash: 3edc1c2bd328cd6e7b7991ff2b5438b8899a0ce7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66160481"
---
# <a name="set-up-compute-targets-for-model-training"></a>Configuración de destinos de proceso del entrenamiento del modelo 

Con el servicio Azure Machine Learning, puede entrenar el modelo en una variedad de recursos o entornos, denominados colectivamente [__destinos de proceso__](concept-azure-machine-learning-architecture.md#compute-target). Un destino de proceso puede ser una máquina local o un recurso de nube, como una instancia de Azure Machine Learning Compute, Azure HDInsight o una máquina virtual remota.  También puede crear destinos de proceso para la implementación de modelos tal como se describe en ["Cómo y dónde implementar los modelos"](how-to-deploy-and-where.md).

Los destinos de proceso se pueden crear y administrar mediante el SDK de Azure Machine Learning, Azure Portal o la CLI de Azure. Si tiene destinos de proceso creados mediante cualquier otro servicio (por ejemplo, un clúster de HDInsight), para usarlos debe adjuntarlos al área de trabajo del servicio Azure Machine Learning.
 
En este artículo, aprenderá a usar diversos destinos de proceso para el entrenamiento de modelos.  Los pasos para todos los destinos de proceso siguen el mismo flujo de trabajo:
1. __Crear__ un destino de proceso si aún no tiene uno.
2. __Asociar__ el destino de proceso al área de trabajo.
3. __Configurar__ el destino de proceso para que contenga las dependencias de paquete y entorno de Python necesarias por el script.


>[!NOTE]
> El código de este artículo se ha probado con el SDK de Azure Machine Learning, versión 1.0.6.

## <a name="compute-targets-for-training"></a>Destinos de proceso para entrenamiento

Azure Machine Learning Service tiene distintas modalidades de soporte técnico en los diferentes destinos de proceso. Un ciclo de vida de desarrollo de modelos típico comienza con el desarrollo y la experimentación en una pequeña cantidad de datos. En esta fase, se recomienda usar un entorno local. Por ejemplo, un equipo local o una máquina virtual basada en la nube. Si escala verticalmente su entrenamiento a conjuntos de datos grandes o realiza el entrenamiento distribuido, se recomienda usar Proceso de Azure Machine Learning para crear un clúster con uno o varios nodos que se escala automáticamente cada vez que se envía una ejecución. También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar como se detalla a continuación:


|Destinos de proceso para entrenamiento| Aceleración de GPU | Automatizado<br/> ajuste de hiperparámetros | Automatizado<br/> aprendizaje automático | Canalizaciones de Azure Machine Learning |
|----|:----:|:----:|:----:|:----:|
|[Equipo local](#local)| Es posible | &nbsp; | ✓ | &nbsp; |
|[Proceso de Azure Machine Learning](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Máquina virtual remota](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](how-to-create-your-first-pipeline.md#databricks)| &nbsp; | &nbsp; | ✓ | ✓ |
|[Análisis con Azure Data Lake](how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | &nbsp; | ✓ |
|[HDInsight de Azure](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |
|[Azure Batch](#azbatch)| &nbsp; | &nbsp; | &nbsp; | ✓ |

**Todos los destinos de proceso se pueden reutilizar para varios trabajos de entrenamiento**. Por ejemplo, una vez que se adjunta una VM remota al área de trabajo, puede reutilizarla para varios trabajos.

> [!NOTE]
> Proceso de Azure Machine Learning puede crearse como recurso persistente o crearse dinámicamente al solicitar una ejecución. La creación basada en ejecución quita el destino de proceso después de que se completa la ejecución de entrenamiento, por lo que no puede reutilizar los destinos de proceso creados de esta manera.

## <a name="whats-a-run-configuration"></a>¿En qué consiste una configuración de ejecución?

Cuando se entrena, es normal comenzar en el equipo local y después ejecutar ese script de entrenamiento en un destino de proceso diferente. Con el servicio Azure Machine Learning, puede ejecutar el script en varios destinos de proceso sin tener que cambiar el script. 

Todo lo que necesita hacer es definir el entorno para cada destino de proceso con una **configuración de ejecución**.  Después, cuando desee ejecutar el experimento de entrenamiento en un destino de proceso diferente, especifique la configuración de ejecución para ese proceso. 

Obtenga más información sobre cómo [enviar experimentos](#submit) al final de este artículo.

### <a name="manage-environment-and-dependencies"></a>Administración del entorno y de las dependencias

Al crear una configuración de ejecución, debe decidir cómo administrar el entorno y las dependencias del destino de proceso. 

#### <a name="system-managed-environment"></a>Entorno administrado por el sistema

Utilice un entorno administrado por el sistema cuando desee que [Conda](https://conda.io/docs/) administre el entorno de Python y las dependencias de script. Un entorno administrado por el sistema se asume de manera predeterminada y es la opción más común. Es útil en destinos de proceso remotos, especialmente cuando no se puede configurar ese objetivo. 

Todo lo que necesita hacer es especificar cada dependencia de paquetes mediante la clase [CondaDependency class](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py). Después, Conda crea un archivo llamado **conda_dependencies.yml** en el directorio **aml_config** de su área de trabajo con la lista de dependencias de paquetes y configura el entorno de Python cuando envía el experimento de entrenamiento. 

La configuración inicial de un nuevo entorno puede tardar varios minutos, según el tamaño de las dependencias necesarias. Mientras la lista de paquetes permanezca sin cambios, el tiempo de configuración solo se realiza una vez.
  
El siguiente código muestra un ejemplo de un entorno administrado por el sistema que requiere scikit-learn:
    
[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_system_managed)]

#### <a name="user-managed-environment"></a>Entorno administrado por el usuario

Para un entorno administrado por usuarios, es responsable de configurar el entorno e instalar todos los paquetes que necesita el script de entrenamiento en el destino de proceso. Si el entorno de entrenamiento ya está configurado (como en la máquina local), puede omitirse el paso de configuración mediante el establecimiento de `user_managed_dependencies` en True. Conda no comprobará el entorno ni instalará nada por usted.

El código siguiente muestra un ejemplo de configuración de ejecuciones de entrenamiento para un entorno administrado por el usuario:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_user_managed)]
  
## <a name="set-up-compute-targets-with-python"></a>Configuración de los destinos de proceso con Python

Use las secciones siguientes para configurar estos destinos de proceso:

* [Equipo local](#local)
* [Proceso de Azure Machine Learning](#amlcompute)
* [Máquinas virtuales remotas](#vm)
* [HDInsight de Azure](#hdinsight)


### <a id="local"></a>Equipo local

1. **Crear y adjuntar**: no es necesario crear o asociar un destino de proceso para utilizar el equipo local como entorno de entrenamiento.  

1. **Configurar**:  cuando se utiliza el equipo local como destino de proceso, el código de entrenamiento se ejecuta en el [entorno de desarrollo](how-to-configure-environment.md).  Si ese entorno ya tiene los paquetes Python que necesita, utilice el entorno administrado por usuarios.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Ahora que ha asociado el proceso y ha configurado la ejecución, el siguiente paso es [enviar la ejecución de entrenamiento](#submit).

### <a id="amlcompute"></a>Proceso de Azure Machine Learning

Proceso de Azure Machine Learning es una infraestructura de proceso administrado que permite al usuario crear fácilmente un proceso de uno o varios nodos. El proceso se crea dentro de la región de su área de trabajo y es un recurso que se puede compartir con otros usuarios del área de trabajo. El proceso se escala verticalmente de forma automática cuando se envía un trabajo y se puede colocar en una instancia de Azure Virtual Network. El proceso se ejecuta en un entorno con contenedores y empaqueta las dependencias del modelo en un [contenedor de Docker](https://www.docker.com/why-docker).

Proceso Azure Machine Learning Compute para distribuir el proceso de entrenamiento en un clúster de nodos de proceso de CPU o GPU de la nube. Para más información sobre los tamaños de máquina virtual que incluyen GPU, consulte [Tamaños de máquinas virtuales optimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

Proceso de Azure Machine Learning tiene límites predeterminados, como el número de núcleos que se pueden asignar. Para más información, consulte [Administración y solicitud de cuotas para recursos de Azure](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).


Puede crear un entorno de Proceso de Azure Machine Learning a petición al programar una ejecución, o bien como un recurso persistente.

#### <a name="run-based-creation"></a>Creación basada en ejecución

Puede crear una instancia de Proceso de Azure Machine Learning como un destino de proceso en tiempo de ejecución. El proceso se crea automáticamente para la ejecución. El proceso se elimina automáticamente una vez completada la ejecución. 

> [!NOTE]
> Para especificar el número máximo de nodos que usará, establecería normalmente `node_count` al número de nodos. Actualmente no hay (04/04/2019) un error que impide que esto funcione. Como alternativa, use el `amlcompute._cluster_max_node_count` propiedad de la configuración de ejecución. Por ejemplo, `run_config.amlcompute._cluster_max_node_count = 5`.

> [!IMPORTANT]
> La creación basada en ejecución de una instancia de Proceso de Azure Machine Learning está actualmente en versión preliminar. No use la creación basada en la ejecución si usa el ajuste de hiperparámetros o el aprendizaje automático automatizado. Para utilizar la optimización de hiperparámetros o el aprendizaje automático automatizado, cree en su lugar un [destino de proceso persistente](#persistent).

1.  **Crear, adjuntar y configurar**: la creación basada en la ejecución realiza todos los pasos necesarios para crear, adjuntar y configurar el destino de proceso con la configuración de ejecución.  

  [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute.py?name=run_temp_compute)]


Ahora que ha asociado el proceso y ha configurado la ejecución, el siguiente paso es [enviar la ejecución de entrenamiento](#submit).

#### <a id="persistent"></a>Proceso persistente

Se puede reutilizar una instancia de Proceso de Azure Machine Learning persistente en varios trabajos. El proceso se puede compartir con otros usuarios del área de trabajo y se conserva entre un trabajo y otro.

1. **Crear y adjuntar**: para crear un recurso de Proceso de Azure Machine Learning persistente en Python, especifique las propiedades **vm_size** y **max_nodes**. Azure Machine Learning usará valores predeterminados inteligentes para las demás propiedades. El proceso se reduce verticalmente a cero nodos cuando no se utiliza.   Se crean máquinas virtuales dedicadas para ejecutar los trabajos según sea necesario.
    
    * **vm_size**: la familia máquina virtual de los nodos creados por el Proceso de Azure Machine Learning.
    * **max_nodes**: el número máximo de nodos hasta los que se aumenta automáticamente cuando ejecuta un trabajo en Proceso de Azure Machine Learning.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   Cuando cree una instancia de Proceso de Azure Machine Learning, puede configurar también varias propiedades avanzadas. Estas propiedades permiten crear un clúster persistente de tamaño fijo o dentro de una instancia existente de Azure Virtual Network de su suscripción.  Consulte la [clase AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ) para más información.
    
   O bien puede crear y adjuntar un recurso persistente de Proceso de Azure Machine Learning [en Azure Portal](#portal-create).

1. **Configurar**: Cree una configuración de ejecución para el destino de proceso persistente.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

Ahora que ha asociado el proceso y ha configurado la ejecución, el siguiente paso es [enviar la ejecución de entrenamiento](#submit).


### <a id="vm"></a>Máquinas virtuales remotas

Azure Machine Learning también admite la posibilidad de que traiga su propio recurso de proceso y lo adjunte a su área de trabajo. Uno de estos tipos de recursos es una máquina virtual remota arbitraria, siempre que se pueda acceder desde Azure Machine Learning Service. El recurso puede ser una máquina virtual de Azure, un servidor remoto de la organización o local. En concreto, dada la dirección IP y las credenciales (nombre de usuario y contraseña o clave SSH), puede usar cualquier máquina virtual a la que se pueda acceder para las ejecuciones remotas.

Puede usar un entorno de Conda integrado en el sistema, un entorno de Python existente o un contenedor de Docker. Para realizar la ejecución en un contenedor de Docker, el motor de Docker debe estar en ejecución en la máquina virtual. Esta funcionalidad es especialmente útil cuando se desea un entorno de desarrollo o experimentación basado en la nube más flexible que un equipo local.

Utilice Azure Data Science Virtual Machine (DSVM) como máquina virtual de Azure preferida para este escenario. Esta máquina virtual es un entorno de desarrollo de IA y de ciencia de datos preconfigurados de Azure. La máquina virtual ofrece una selección de herramientas y plataformas mantenidas para el desarrollo del aprendizaje automático a lo largo de todo el ciclo de vida. Para obtener más información sobre cómo usar la DSVM con Azure Machine Learning, consulte [Configuración del entorno de desarrollo](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

1. **Crear**: Cree una instancia de DSVM antes de usarla para entrenar el modelo. Para crear este recurso, consulte [Aprovisionamiento de Data Science Virtual Machine para Linux (Ubuntu)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Azure Machine Learning solo admite máquinas virtuales que ejecuten Ubuntu. Al crear una máquina virtual o elegir una máquina virtual existente, debe seleccionar una máquina virtual que use Ubuntu.

1. **Adjuntar**: Para asociar una máquina virtual existente como destino de proceso, debe proporcionar el nombre de dominio completo (FQDN), el nombre de usuario y la contraseña de la máquina virtual. En el ejemplo, reemplace \<fqdn> por el FQDN de la máquina virtual o por la dirección IP pública. Reemplace \<username> y \<password> por el nombre de usuario y contraseña de SSH para la máquina virtual.

   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   attach_config = RemoteCompute.attach_configuration(address = "<fqdn>",
                                                    ssh_port=22,
                                                    username='<username>',
                                                    password="<password>")

   # If you authenticate with SSH keys instead, use this code:
   #                                                  ssh_port=22,
   #                                                  username='<username>',
   #                                                  password=None,
   #                                                  private_key_file="<path-to-file>",
   #                                                  private_key_passphrase="<passphrase>")

   # Attach the compute
   compute = ComputeTarget.attach(ws, compute_target_name, attach_config)

   compute.wait_for_completion(show_output=True)
   ```

   O bien puede asociar la instancia de DSVM al área de trabajo [mediante Azure Portal](#portal-reuse).

1. **Configurar**: Cree una configuración de ejecución para el destino de proceso de Data Science Virtual Machine. Docker y Conda se utilizan para crear y configurar el entorno de entrenamiento en la DSVM.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Ahora que ha asociado el proceso y ha configurado la ejecución, el siguiente paso es [enviar la ejecución de entrenamiento](#submit).

### <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight es una plataforma popular para el análisis de macrodatos. La plataforma proporciona Apache Spark, que se puede usar para entrenar el modelo.

1. **Crear**:  Cree el clúster de HDInsight antes de usarlo para entrenar el modelo. Para crear un clúster de Spark en HDInsight, consulte [Creación de un clúster de Spark en HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql). 

    Al crear el clúster, debe especificar un nombre de usuario SSH y una contraseña. Anote estos valores, ya que los necesitará al usar HDInsight como destino de proceso.
    
    Cuando se ha creado el clúster, conéctelo al nombre de host \<clustername>-ssh.azurehdinsight.net, donde \<clustername> es el nombre que ha proporcionado para el clúster. 

1. **Adjuntar**: Para asociar un clúster de HDInsight como destino de proceso, debe proporcionar el nombre de host, el nombre de usuario y la contraseña para el clúster de HDInsight. El ejemplo siguiente usa el SDK para asociar un clúster al área de trabajo. En el ejemplo, reemplace \<clustername> por el nombre del clúster. Reemplace \<username> y \<password> por el nombre de usuario y contraseña de SSH para el clúster.

   ```python
   from azureml.core.compute import ComputeTarget, HDInsightCompute
   from azureml.exceptions import ComputeTargetException

   try:
    # if you want to connect using SSH key instead of username/password you can provide parameters private_key_file and private_key_passphrase
    attach_config = HDInsightCompute.attach_configuration(address='<clustername>-ssh.azureinsight.net', 
                                                          ssh_port=22, 
                                                          username='<ssh-username>', 
                                                          password='<ssh-pwd>')
    hdi_compute = ComputeTarget.attach(workspace=ws, 
                                       name='myhdi', 
                                       attach_configuration=attach_config)

   except ComputeTargetException as e:
    print("Caught = {}".format(e.message))

   hdi_compute.wait_for_completion(show_output=True)
   ```

   O bien puede asociar el clúster de HDInsight al área de trabajo [mediante Azure Portal](#portal-reuse).

1. **Configurar**: Cree una configuración de ejecución para el destino de proceso de HDI. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Ahora que ha asociado el proceso y ha configurado la ejecución, el siguiente paso es [enviar la ejecución de entrenamiento](#submit).


### <a id="azbatch"></a>Azure Batch 

Azure Batch se utiliza para ejecutar aplicaciones a gran escala paralelas y de alto rendimiento informática (HPC) de manera eficaz en la nube. AzureBatchStep puede utilizarse en una canalización de aprendizaje automático de Azure para enviar trabajos a un grupo de Azure Batch de máquinas.

Para asociar el lote de Azure como un destino de proceso, debe usar el SDK de Azure Machine Learning y proporcione la siguiente información:

-   **Nombre de la ejecución de Azure Batch**: Un nombre descriptivo que se usará para el proceso en el área de trabajo
-   **Nombre de cuenta de Azure Batch**: El nombre de la cuenta de Azure Batch
-   **Grupo de recursos**: El grupo de recursos que contiene la cuenta de Azure Batch.

El código siguiente muestra cómo conectar Azure Batch como un destino de proceso:

```python
from azureml.core.compute import ComputeTarget, BatchCompute
from azureml.exceptions import ComputeTargetException

batch_compute_name = 'mybatchcompute' # Name to associate with new compute in workspace

# Batch account details needed to attach as compute to workspace
batch_account_name = "<batch_account_name>" # Name of the Batch account
batch_resource_group = "<batch_resource_group>" # Name of the resource group which contains this account

try:
    # check if the compute is already attached
    batch_compute = BatchCompute(ws, batch_compute_name)
except ComputeTargetException:
    print('Attaching Batch compute...')
    provisioning_config = BatchCompute.attach_configuration(resource_group=batch_resource_group, account_name=batch_account_name)
    batch_compute = ComputeTarget.attach(ws, batch_compute_name, provisioning_config)
    batch_compute.wait_for_completion()
    print("Provisioning state:{}".format(batch_compute.provisioning_state))
    print("Provisioning errors:{}".format(batch_compute.provisioning_errors))

print("Using Batch compute:{}".format(batch_compute.cluster_resource_id))
```

## <a name="set-up-compute-in-the-azure-portal"></a>Configuración de proceso en Azure Portal

Desde Azure Portal, puede acceder a destinos de proceso que están asociados con el área de trabajo.  Puede usar el portal para:

* [Ver los destinos de proceso](#portal-view) asociados a un área de trabajo
* [Crear un destino de proceso](#portal-create) en el área de trabajo
* [Adjuntar un destino de proceso](#portal-reuse) que se creó fuera del área de trabajo

Después de crear un destino y de adjuntarlo al área de trabajo, lo utilizará en la configuración de ejecución con un objeto `ComputeTarget`: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a id="portal-view"></a>Visualización de destinos de proceso


Para ver los destinos de proceso del área de trabajo, use los pasos siguientes:

1. Navegue hasta [Azure Portal](https://portal.azure.com) y abra el área de trabajo. 
1. En __Aplicaciones__, seleccione __Proceso__.

    ![Pestaña en la que se ve el proceso](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a id="portal-create"></a>Creación de un destino de proceso

Siga los pasos anteriores para ver la lista de destinos de proceso. A continuación, siga estos pasos para crear un destino de proceso: 

1. Haga clic en el signo más (+) para agregar un destino de proceso.

    ![Agregar un destino de proceso](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. Escriba un nombre para el destino de proceso. 

1. Seleccione **Proceso de Machine Learning** como tipo de proceso que se usará en __Entrenamiento__. 

    >[!NOTE]
    >Proceso de Azure Machine Learning es el único recurso de proceso administrado que se puede crear en Azure Portal.  Todos los demás recursos de proceso se pueden asociar después de su creación.

1. Rellene el formulario. Proporcione valores para las propiedades necesarias, especialmente para **Familia de máquinas virtuales** y **Nodos máximos** que se van a utilizar para acelerar el proceso.  

    ![Rellenado del formulario](./media/how-to-set-up-training-targets/add-compute-form.png) 

1. Seleccione __Crear__.


1. Vea el estado de la operación de creación seleccionando el destino de proceso en la lista:

    ![Seleccionar un destino de proceso para ver el estado de la operación de creación](./media/how-to-set-up-training-targets/View_list.png)

1. Verá los detalles del destino de proceso: 

    ![Ver los detalles del destino de proceso](./media/how-to-set-up-training-targets/compute-target-details.png) 



### <a id="portal-reuse"></a>Adjuntar destinos de proceso

Para usar destinos de proceso creados fuera del área de trabajo de Azure Machine Learning Service, debe adjuntarlos. Adjuntar un destino de proceso hace que esté disponible para el área de trabajo.

Siga los pasos descritos previamente para ver la lista de destinos de proceso. Luego, siga los pasos a continuación para adjuntar un destino de proceso: 

1. Haga clic en el signo más (+) para agregar un destino de proceso. 
1. Escriba un nombre para el destino de proceso. 
1. Seleccione el tipo de proceso que va a adjuntar para __Entrenamiento__:

    > [!IMPORTANT]
    > No todos los tipos de proceso se pueden adjuntar desde Azure Portal. Los tipos de proceso que se pueden adjuntar actualmente para entrenamiento incluyen:
    >
    > * Una máquina virtual remota
    > * Azure Databricks (para su uso en las canalizaciones de aprendizaje automático)
    > * Azure Data Lake Analytics (para su uso en las canalizaciones de aprendizaje automático)
    > * HDInsight de Azure

1. Rellene el formulario y proporcione valores para las propiedades necesarias.

    > [!NOTE]
    > Microsoft recomienda que use claves SSH, que son más seguras que las contraseñas. Las contraseñas son vulnerables a ataques por fuerza bruta. Las claves SSH se basan en las firmas criptográficas. Para información sobre cómo crear claves SSH para usarlas con Azure Virtual Machines, consulte los siguientes documentos:
    >
    > * [Creación y uso de claves SSH en Linux o macOS](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Creación y uso de claves SSH en Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Seleccione __Adjuntar__. 
1. Consulte el estado de la operación de adjuntar seleccionando el destino de proceso en la lista.

## <a name="set-up-compute-with-the-cli"></a>Configuración de proceso con la CLI

Puede acceder a los destinos de proceso asociados con el área de trabajo mediante la [extensión de la CLI](reference-azure-machine-learning-cli.md) para el servicio Azure Machine Learning.  Puede usar la CLI para:

* Crear un destino de proceso administrado
* Actualizar un destino de proceso administrado
* Asociar un destino de proceso no administrado

Para más información, consulte el artículo sobre la [administración de recursos](reference-azure-machine-learning-cli.md#resource-management).

## <a id="submit"></a>Envío de una ejecución de entrenamiento

Después de crear una configuración de ejecución, se utiliza para ejecutar el experimento.  El patrón de código para enviar una ejecución de entrenamiento es el mismo para todos los tipos de destinos de proceso:

1. Creación de un experimento para su ejecución
1. Envíe la ejecución.
1. Espere a que la ejecución se complete.

> [!IMPORTANT]
> Al enviar la ejecución de formación, una instantánea del directorio que contiene los scripts de entrenamiento se crean y envían al destino de proceso. También se almacena como parte del experimento en el área de trabajo. Si cambia los archivos y enviar la ejecución de nuevo, se cargará solo los archivos cambiados.
>
> Para evitar que los archivos que se incluye en la instantánea, cree un [.gitignore](https://git-scm.com/docs/gitignore) o `.amlignore` en el directorio y agréguele los archivos. El `.amlignore` archivo usa la misma sintaxis y patrones como la [.gitignore](https://git-scm.com/docs/gitignore) archivo. Si ambos archivos se encuentran el `.amlignore` archivo tiene prioridad.
> 
> Para más información, consulte [Instantánea](concept-azure-machine-learning-architecture.md#snapshot).

### <a name="create-an-experiment"></a>Creación de un experimento

En primer lugar, cree un experimento en el área de trabajo.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Envío del experimento

Envíe el experimento con un objeto `ScriptRunConfig`.  Este objeto incluye:

* **source_directory**: el directorio de origen que contiene el script de entrenamiento.
* **script**: Identificación del script de entrenamiento
* **run_config**: la configuración de ejecución, que a su vez define donde tendrá lugar el entrenamiento.

Por ejemplo, para usar la configuración del [destino local](#local):

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

Cambie el mismo experimento para que se ejecute en un destino de proceso diferente mediante una configuración de ejecución diferente, como [amlcompute target](#amlcompute):

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

También puede:

* Enviar el experimento con un objeto `Estimator`, tal como se muestra en [Entrenamiento de modelos de aprendizaje automático con estimadores](how-to-train-ml-models.md). 
* Envíe un experimento [mediante la extensión de la CLI](reference-azure-machine-learning-cli.md#experiments).

## <a name="github-tracking-and-integration"></a>Integración y seguimiento de GitHub

Cuando se inicia un entrenamiento que se ejecute, donde el directorio de origen es un repositorio Git local, se almacena información sobre el repositorio en el historial de ejecución. Por ejemplo, el identificador de confirmación actual para el repositorio se registra como parte del historial.

## <a name="notebook-examples"></a>Ejemplos de cuadernos

Consulte estos cuadernos para ver ejemplos de entrenamiento con varios destinos de proceso:
* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md) utiliza un destino de proceso administrado para entrenar un modelo.
* Obtenga información sobre cómo [eficazmente ajustar los hiperparámetros](how-to-tune-hyperparameters.md) para crear modelos mejores.
* Cuando tenga un modelo entrenado, aprenda [cómo y dónde implementar los modelos](how-to-deploy-and-where.md).
* Consulte la referencia del SDK de la [clase RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py).
* [Uso de Azure Machine Learning Service con Azure Virtual Network](how-to-enable-virtual-network.md)
