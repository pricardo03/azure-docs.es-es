---
title: Transformación de datos mediante Databricks en Azure Data Factory | Microsoft Docs
description: Aprenda a usar una plantilla de solución para transformar datos mediante un cuaderno de Databricks en Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: 562ce675acc43002ce468d60f8a8c412410be86c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60395400"
---
# <a name="transform-data-by-using-databricks-in-azure-data-factory"></a>Transformación de datos mediante Databricks en Azure Data Factory

En este tutorial, creará una canalización de un extremo a otro que contenga las actividades **Lookup** (Buscar), **Copy** (Copiar) y **Databricks notebook** (Cuaderno de Databricks) en Data Factory.

-   La actividad **Lookup** (Buscar) o GetMetadata se usa para garantizar que el conjunto de datos de origen está listo para el consumo de bajada, antes de desencadenar el trabajo de copia y análisis.

-   La actividad **Copy** (Copiar) copia el archivo o conjunto de datos de origen en el almacenamiento receptor. El almacenamiento receptor se monta como DBFS en el cuaderno de Databricks para que Spark pueda consumir directamente el conjunto de datos.

-   La actividad **Databricks notebook** (Cuaderno de Databricks) desencadena el cuaderno de Databricks que transforma el conjunto de datos y lo agrega a una carpeta o un almacén de datos SQL procesados.

Para simplificar esta plantilla, la plantilla no crea un desencadenador programado. Sin embargo, puede agregarlo en caso necesario.

![1](media/solution-template-Databricks-notebook/Databricks-tutorial-image01.png)

## <a name="prerequisites"></a>Requisitos previos

1.  Cree una **cuenta de almacenamiento de blobs** y un contenedor llamado `sinkdata` para que se use como **receptor**. Anote el **nombre de la cuenta de almacenamiento**, el **nombre del contenedor** y la **clave de acceso**, ya que se hará referencia a ellos más adelante en la plantilla.

2.  Asegúrese de que tiene un **área de trabajo de Azure Databricks** o cree una.

1.  **Importe el cuaderno para ETL**. Importe el siguiente cuaderno de transformación al área de trabajo de Databricks. (No tiene que estar en la misma ubicación que aparece a continuación, pero recuerde la ruta de acceso que elija para más adelante). Importe el cuaderno desde la siguiente dirección URL, para lo cual debe escribirla en el campo de dirección URL: `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`. Seleccione **Import** (Importar).

    ![2](media/solution-template-Databricks-notebook/Databricks-tutorial-image02.png)

    ![3](media/solution-template-Databricks-notebook/Databricks-tutorial-image03.png)  

1.  Ahora se actualizará el cuaderno **Transformation** (Transformación) con la **información de la conexión de almacenamiento** (nombre y clave de acceso). Vaya a **command 5** (comando 5) en el cuaderno importado anteriormente y reemplácelo por el siguiente fragmento de código tras reemplazar los valores resaltados. Asegúrese de que esta cuenta sea la misma cuenta de almacenamiento creada anteriormente y que incluya el contenedor `sinkdata`.

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

1.  Genere un **token de acceso de Databricks** para Data Factory para acceder a Databricks. **Guarde el token de acceso** para su usarlo más adelante en la creación de un servicio vinculado de Databricks, que es algo parecido a "dapi32db32cbb4w6eee18b7d87e45exxxxxx".

    ![4](media/solution-template-Databricks-notebook/Databricks-tutorial-image04.png)

    ![5](media/solution-template-Databricks-notebook/Databricks-tutorial-image05.png)

## <a name="create-linked-services-and-datasets"></a>Crear servicios vinculados y conjuntos de datos

1.  Cree **servicios vinculados** en la interfaz de usuario de Data Factory; para ello, vaya a *Connections Linked services + new* (Servicios vinculados de conexiones + nuevo)

    1.  **Origen**: para el acceso a los datos de origen. Puede usar el almacenamiento de blobs público, que contiene los archivos de origen para este ejemplo.

        Seleccione **Blob Storage** y use el siguiente **URI de SAS** para conectarse al almacenamiento de origen (acceso de solo lectura).

        `https://storagewithdata.blob.core.windows.net/?sv=2017-11-09&ss=b&srt=sco&sp=rl&se=2019-12-31T21:40:53Z&st=2018-10-24T13:40:53Z&spr=https&sig=K8nRio7c4xMLnUV0wWVAmqr5H4P3JDwBaG9HCevI7kU%3D`

        ![6](media/solution-template-Databricks-notebook/Databricks-tutorial-image06.png)

    1.  **Receptor**: donde se copian los datos.

        Seleccione un almacenamiento que creó en el requisito previo 1, en el servicio vinculado del receptor.

        ![7](media/solution-template-Databricks-notebook/Databricks-tutorial-image07.png)

    1.  **Databricks**: para conectarse al clúster de Databricks.

        Crear un servicio vinculado de Databricks con la clave de acceso generada en el requisito previo 2.c. Si tiene un *clúster interactivo*, puede seleccionar ese. (En este ejemplo se usa la opción *New job cluster* [Nuevo clúster de trabajos]).

        ![8](media/solution-template-Databricks-notebook/Databricks-tutorial-image08.png)

2.  Crear **conjuntos de datos**

    1.  Cree **"sourceAvailability_Dataset"** para comprobar si los datos de origen están disponibles.

    ![9](media/solution-template-Databricks-notebook/Databricks-tutorial-image09.png)

    1.  **Conjunto de datos de origen:** para copiar los datos de origen (mediante copia binaria)

    ![10](media/solution-template-Databricks-notebook/Databricks-tutorial-image10.png)

    1.  **Conjunto de datos del receptor**: para copiar la ubicación de recepción o destino

        1.  Linked service (Servicio vinculado): seleccione "sinkBlob_LS" que creó en el paso 1.b

        2.  File path (Ruta de acceso de archivo): "sinkdata/staged_sink"

        ![11](media/solution-template-Databricks-notebook/Databricks-tutorial-image11.png)

## <a name="create-activities"></a>Crear actividades

1.  Cree una actividad de búsqueda "**Availability flag**" (Indicador de disponibilidad) para realizar una comprobación de disponibilidad de origen (se puede usar Lookup [Buscar] o GetMetadata). Seleccione "sourceAvailability_Dataset", que creó en el paso 2.a.

    ![12](media/solution-template-Databricks-notebook/Databricks-tutorial-image12.png)

1.  Cree una actividad de copia "**file-to-blob**" (archivar en blob) para copiar el conjunto de datos del origen al receptor. En este caso, los datos son el archivo binario. Consulte las siguientes capturas de pantalla para las configuraciones de origen y receptor en la actividad de copia.

    ![13](media/solution-template-Databricks-notebook/Databricks-tutorial-image13.png)

    ![14](media/solution-template-Databricks-notebook/Databricks-tutorial-image14.png)

1.  Definir los **parámetros de canalización**

    ![15](media/solution-template-Databricks-notebook/Databricks-tutorial-image15.png)

1.  Crear una **actividad de Databricks**

    Seleccione el servicio vinculado creado en un paso anterior.

    ![16](media/solution-template-Databricks-notebook/Databricks-tutorial-image16.png)

    Configure los valores de **Settings** (Configuración). Realice la configuración de **Base Parameters** (Parámetros base) como se muestra en la captura de pantalla y cree los parámetros que se van a pasar al cuaderno de Databricks desde Data Factory. Examine y **seleccione** la **ruta de acceso de cuaderno correcta** cargada en el **requisito previo 2**.

    ![17](media/solution-template-Databricks-notebook/Databricks-tutorial-image17.png)

1.  **Ejecutar la canalización**. Puede encontrar el vínculo a los registros de Databricks si quiere tener registros de Spark más detallados.

    ![18](media/solution-template-Databricks-notebook/Databricks-tutorial-image18.png)

    También puede comprobar el archivo de datos mediante el Explorador de Storage. (Para establecer una correlación con ejecuciones de canalización de Data Factory, en este ejemplo se agrega el identificador de ejecución de la canalización de la factoría de datos a la carpeta de salida. De esta forma, puede realizar un seguimiento de los archivos generados con cada ejecución).

![19](media/solution-template-Databricks-notebook/Databricks-tutorial-image19.png)

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)
