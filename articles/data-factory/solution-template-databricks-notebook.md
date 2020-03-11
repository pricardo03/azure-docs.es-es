---
title: Transformación con Azure Databricks
description: Aprenda a usar una plantilla de solución para transformar datos mediante un cuaderno de Databricks en Azure Data Factory.
services: data-factory
ms.author: abnarain
author: nabhishek
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/10/2018
ms.openlocfilehash: 322f5306949b266958ded908e981ed530e8245c8
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2020
ms.locfileid: "78227501"
---
# <a name="transformation-with-azure-databricks"></a>Transformación con Azure Databricks

En este tutorial, creará una canalización de un extremo a otro que contenga las actividades **Validación**, **Copia** y **Cuaderno** en Data Factory.

-   La actividad **Validación** se usa para garantizar que el conjunto de datos de origen está listo para el consumo de bajada, antes de desencadenar el trabajo de copia y análisis.

-   La actividad **Copia** copia el archivo o conjunto de datos de origen en el almacenamiento receptor. El almacenamiento receptor se monta como DBFS en el cuaderno de Databricks para que Spark pueda consumir directamente el conjunto de datos.

-   La actividad **Cuaderno de Databricks** desencadena el cuaderno de Databricks que transforma el conjunto de datos y lo agrega a una carpeta o un almacén de datos SQL procesados.

Para simplificar esta plantilla, la plantilla no crea un desencadenador programado. Sin embargo, puede agregarlo en caso necesario.

![1](media/solution-template-Databricks-notebook/Databricks-tutorial-image01.png)

## <a name="prerequisites"></a>Prerrequisitos

1. Cree una **cuenta de almacenamiento de blobs** y un contenedor llamado `sinkdata` para que se use como **receptor**. Anote el **nombre de la cuenta de almacenamiento**, el **nombre del contenedor** y la **clave de acceso**, ya que se hará referencia a ellos más adelante en la plantilla.

2. Asegúrese de que tiene un **área de trabajo de Azure Databricks** o cree una.

3. **Importe el cuaderno para Transformation (Transformación)** . 
    1. En Azure Databricks, haga referencia a las capturas de pantallas siguientes para importar un cuaderno **Transformation** (Transformación) en el área de trabajo de Databricks. No tiene que estar en la misma ubicación que aparece a continuación, pero recuerde la ruta de acceso que elija para más adelante.
   
       ![2](media/solution-template-Databricks-notebook/Databricks-tutorial-image02.png)    
    
    1. Seleccione "Import from: **URL**" (Importar desde: Dirección URL), y escriba la siguiente dirección URL en el cuadro de texto:
    
       * `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`
        
       ![3](media/solution-template-Databricks-notebook/Databricks-tutorial-image03.png)    

4. Ahora se va a actualizar el cuaderno **Transformation** (Transformación) con la información de la conexión de almacenamiento. Vaya a **command 5** (comando 5) (como se muestra debajo del fragmento de código) en el cuaderno importado anterior y reemplace `<storage name>` y `<access key>` por su propia información de la conexión de almacenamiento. Asegúrese de que esta cuenta sea la misma cuenta de almacenamiento creada anteriormente y que incluya el contenedor `sinkdata`.

    ```python
    # Supply storageName and accessKey values  
    storageName = "<storage name>"  
    accessKey = "<access key>"  

    try:  
      dbutils.fs.mount(  
        source = "wasbs://sinkdata\@"+storageName+".blob.core.windows.net/",  
        mount_point = "/mnt/Data Factorydata",  
        extra_configs = {"fs.azure.account.key."+storageName+".blob.core.windows.net": accessKey})  

    except Exception as e:  
      # The error message has a long stack track. This code tries to print just the relevant line indicating what failed.

    import re
    result = re.findall(r"\^\s\*Caused by:\s*\S+:\s\*(.*)\$", e.message, flags=re.MULTILINE)
    if result:
      print result[-1] \# Print only the relevant error message
    else:  
      print e \# Otherwise print the whole stack trace.  
    ```

5.  Genere un **token de acceso de Databricks** para Data Factory para acceder a Databricks. **Guarde el token de acceso** para su usarlo más adelante en la creación de un servicio vinculado de Databricks, que es algo parecido a "dapi32db32cbb4w6eee18b7d87e45exxxxxx".

    ![4](media/solution-template-Databricks-notebook/Databricks-tutorial-image04.png)

    ![5](media/solution-template-Databricks-notebook/Databricks-tutorial-image05.png)

## <a name="how-to-use-this-template"></a>Uso de esta plantilla

1.  Vaya a la plantilla **Transformation with Azure Databricks** (Transformación con Azure Databricks). Cree nuevos servicios vinculados para las siguientes conexiones. 
    
    ![Configuración de conexiones](media/solution-template-Databricks-notebook/connections-preview.png)

    1.  **Conexión al blob de origen**: para el acceso a los datos de origen. 
        
        Puede usar el almacenamiento de blobs público, que contiene los archivos de origen para este ejemplo. Tome como referencia la siguiente captura de pantalla para la configuración. Use a continuación la **dirección URL de SAS** para conectarse al almacenamiento de origen (acceso de solo lectura): 
        * `https://storagewithdata.blob.core.windows.net/data?sv=2018-03-28&si=read%20and%20list&sr=c&sig=PuyyS6%2FKdB2JxcZN0kPlmHSBlD8uIKyzhBWmWzznkBw%3D`

        ![6](media/solution-template-Databricks-notebook/Databricks-tutorial-image06.png)

    1.  **Conexión al blob de destino**: para copiar datos dentro. 
        
        En el servicio vinculado del receptor, seleccione un almacenamiento que creó en el **requisito previo** 1.

        ![7](media/solution-template-Databricks-notebook/Databricks-tutorial-image07.png)

    1.  **Azure Databricks**: para la conexión al clúster de Databricks.

        Cree un servicio vinculado de Databricks con la clave de acceso generada en el **requisito previo** 2.c. Si tiene un *clúster interactivo*, puede seleccionar ese. (En este ejemplo se usa la opción *New job cluster* [Nuevo clúster de trabajos]).

        ![8](media/solution-template-Databricks-notebook/Databricks-tutorial-image08.png)

1. Seleccione **Use this template** (Usar esta plantilla) y verá una canalización creada como se muestra a continuación:
    
    ![Crear una canalización](media/solution-template-Databricks-notebook/new-pipeline.png)   

## <a name="pipeline-introduction-and-configuration"></a>Introducción y configuración de canalizaciones

En la nueva canalización creada, la mayoría de las opciones se han configurado automáticamente con los valores predeterminados. Revise las configuraciones y actualícelas cuando sea necesario para adaptarlas a sus propios valores. Para más información, puede consultar las siguientes instrucciones y capturas de pantallas como referencia.

1.  Se crea una **marca de disponibilidad** de actividad de validación para realizar una comprobación de la disponibilidad del origen. *SourceAvailabilityDataset* creado en el paso anterior se selecciona como conjunto de datos.

    ![12](media/solution-template-Databricks-notebook/Databricks-tutorial-image12.png)

1.  Se crea una actividad de copia "**file-to-blob**" (de archivo a blob) para copiar el conjunto de datos del origen al receptor. Consulte las siguientes capturas de pantalla para las configuraciones de origen y receptor en la actividad de copia.

    ![13](media/solution-template-Databricks-notebook/Databricks-tutorial-image13.png)

    ![14](media/solution-template-Databricks-notebook/Databricks-tutorial-image14.png)

1.  Se crea una actividad de **Transformation** (Transformación) de Notebook y se selecciona el servicio vinculado creado en el paso anterior.
    ![16](media/solution-template-Databricks-notebook/Databricks-tutorial-image16.png)

     1. Seleccione la pestaña **Settings** (Configuración). En *Notebook path* (Ruta de acceso del cuaderno), la plantilla define una ruta de acceso de forma predeterminada. Puede que deba examinar y seleccionar la ruta de acceso del cuaderno correcta cargada en el **requisito previo** 2. 

         ![17](media/solution-template-Databricks-notebook/databricks-tutorial-image17.png)
    
     1. Consulte los *parámetros base* creados en la captura de pantalla. Se van a pasar al cuaderno de Databricks desde Data Factory. 

         ![Parámetros base](media/solution-template-Databricks-notebook/base-parameters.png)

1.  A continuación se definen los **parámetros de la canalización**.

    ![15](media/solution-template-Databricks-notebook/Databricks-tutorial-image15.png)

1. Configuración de conjuntos de datos.
    1.  Se crea **SourceAvailabilityDataset** para comprobar si están disponibles los datos de origen.

        ![9](media/solution-template-Databricks-notebook/Databricks-tutorial-image09.png)

    1.  **SourceFilesDataset**: para copiar los datos de origen.

        ![10](media/solution-template-Databricks-notebook/Databricks-tutorial-image10.png)

    1.  **DestinationFilesDataset**: para copiar en la ubicación receptora (destino).

        1.  El servicio vinculado, *sinkBlob_LS*, creado en el paso anterior.

        2.  Ruta de acceso de archivo: *sinkdata/staged_sink*.

            ![11](media/solution-template-Databricks-notebook/Databricks-tutorial-image11.png)


1.  Seleccione **Debug** (Depurar) para ejecutar la canalización. Puede encontrar el vínculo a los registros de Databricks si quiere tener registros de Spark más detallados.

    ![18](media/solution-template-Databricks-notebook/Databricks-tutorial-image18.png)

    También puede comprobar el archivo de datos mediante el Explorador de Storage. (Para establecer una correlación con ejecuciones de canalización de Data Factory, en este ejemplo se agrega el identificador de ejecución de la canalización de la factoría de datos a la carpeta de salida. De esta forma, puede realizar un seguimiento de los archivos generados con cada ejecución).

    ![19](media/solution-template-Databricks-notebook/Databricks-tutorial-image19.png)

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)
