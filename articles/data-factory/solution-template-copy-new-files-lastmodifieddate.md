---
title: Copiar archivos nuevos y modificados por LastModifiedDate & gt; con Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo usar una plantilla de solución para copiar archivos nuevos y modificados por LastModifiedDate & gt; con Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 3/8/2019
ms.openlocfilehash: cae75f4d64c8b3f74cc40e94a675c0f10a6bd9ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60312855"
---
# <a name="copy-new-and-changed-files-by-lastmodifieddate-with-azure-data-factory"></a>Copiar archivos nuevos y modificados por LastModifiedDate & gt; con Azure Data Factory

En este artículo se describe una plantilla de solución que puede usar para copiar archivos nuevos y modificados únicamente por LastModifiedDate & gt; desde un almacén basado en archivos a un almacén de destino. 

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla selecciona primero los archivos nuevos y modificados sólo por sus atributos **LastModifiedDate & gt;** y, a continuación, copia los archivos seleccionados desde el almacén de origen de datos en el almacén de datos de destino.

La plantilla contiene una actividad:
- **Copia** para copiar archivos nuevos y modificados únicamente por LastModifiedDate & gt; desde un almacén de archivos en un almacén de destino.

La plantilla define cuatro parámetros:
-  *FolderPath_Source* es la ruta de acceso donde puede leer los archivos del almacén de origen. Deberá reemplazar el valor predeterminado con su propia ruta de acceso de carpeta.
-  *FolderPath_Destination* es la ruta de acceso de la carpeta donde desea copiar archivos en el almacén de destino. Deberá reemplazar el valor predeterminado con su propia ruta de acceso de carpeta.
-  *LastModified_From* se usa para seleccionar los archivos cuyo atributo LastModifiedDate & gt; es después o igual al valor de fecha y hora.  Para seleccionar los nuevos archivos únicamente, que no se ha copiado última vez, este valor de fecha y hora puede ser la hora cuando la canalización se desencadenó la última vez. Puede reemplazar el valor predeterminado ' 2019-02-01T00:00:00Z' para su LastModifiedDate & gt; se esperaba en la zona horaria UTC. 
-  *LastModified_To* se utiliza para seleccionar los archivos cuyo atributo LastModifiedDate & gt; es antes de este valor de fecha y hora. Para seleccionar los nuevos archivos únicamente, que no se ha copiado última vez, este valor de fecha y hora puede ser la hora actual.  Puede reemplazar el valor predeterminado ' 2019-02-01T00:00:00Z' para su LastModifiedDate & gt; se esperaba en la zona horaria UTC. 

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **copiar archivos nuevos sólo por LastModifiedDate & lt;**. Crear un **New** conexión con el almacén de almacenamiento de origen. El almacén de almacenamiento de origen es donde desea copiar los archivos.

    ![Creación de una nueva conexión con el origen](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate1.png)
    
2. Seleccione primero el almacenamiento **tipo**. Una vez que el almacenamiento de entrada **nombreCuenta** y **clave de cuenta**. Por último, seleccione **finalizar**.

    ![Escriba una cadena de conexión](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate2.png)
    
3. Crear un **New** conexión a su almacén de destino. El almacén de destino es donde van a copiar archivos. También debe escribir la información de conexión de almacén de datos de destino similar como hizo en el paso 2.

    ![Creación de una nueva conexión con el destino](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate3.png)

4. Seleccione **usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate4.png)
    
5. Verá la canalización disponible en el panel, como se muestra en el ejemplo siguiente:

    ![Visualización de la canalización](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate5.png)

6. Seleccione **depurar**, escribir el valor de la **parámetros** y seleccione **finalizar**.  En la siguiente imagen, establecemos los parámetros como sigue.
   - **FolderPath_Source** = **/source/**
   - **FolderPath_Destination** = **/destination/**
   - **LastModified_From** =  **2019-02-01T00:00:00Z**
   - **LastModified_To** = **2019-03-01T00:00:00Z**
    
     El ejemplo es que indica los archivos que se modificaron por última vez dentro del intervalo de tiempo entre *2019-02-01T00:00:00Z* y *2019-03-01T00:00:00Z* se copiará desde una carpeta */source/*  en una carpeta */destination/*.  Puede reemplazar estos elementos con sus propios parámetros.
    
     ![Ejecución de la canalización](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate6.png)

7. Revise el resultado. Verá solo los archivos que se modificó por última vez en el intervalo de tiempo se ha copiado en el almacén de destino.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate7.png)
    
8. Ahora puede agregar un desencadenador de windows de saltos de tamaño constante para automatizar esta canalización, para que la canalización siempre pueda copiar archivos nuevos y modificados únicamente por LastModifiedDate & gt; periódicamente.  Seleccione **Agregar desencadenador**y seleccione **nueva/Editar**.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate8.png)
    
9. En la ventana **Agregar desencadenadores**, seleccione **+ Nuevo**.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate9.png)

10. Seleccione **Tumblingwindow** para el tipo de desencadenador, establezca **cada 15 minutos** como la periodicidad (puede cambiar a cualquier intervalo de tiempo) y, a continuación, seleccione **siguiente**.

    ![Crear desencadenador](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate10.png)    
    
11. Escribir el valor de la **parámetros de ejecución de desencadenador** como siguiente y seleccione **finalizar**.
    - **FolderPath_Source** = **/source/**.  Puede reemplazar con la carpeta en el almacén de datos de origen.
    - **FolderPath_Destination** = **/destination/**.  Puede reemplazar con la carpeta en el almacén de datos de destino.
    - **LastModified_From** =  **@trigger().outputs.windowStartTime**.  Es una variable del sistema desde el desencadenador determinar la hora cuando la canalización se desencadenó la última vez.
    - **LastModified_To** = **@trigger().outputs.windowEndTime**.  Es una variable del sistema desde el desencadenador determinar el momento cuando la canalización se desencadena este momento.
    
    ![Parámetros de entrada](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate11.png)
    
12. Seleccione **Publish All** (Publicar todo).
    
    ![Publicar todo](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate12.png)

13. Crear nuevos archivos en la carpeta de origen del almacén de datos de origen.  Ahora se está esperando la canalización se activen automáticamente y solo los nuevos archivos se copiarán en el almacén de destino.

14. Seleccione **supervisión** pestaña en el panel de navegación izquierdo y espere unos 15 minutos si se ha establecido la periodicidad del desencadenador a cada 15 minutos. 

    ![Seleccione la supervisión](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate14.png)

15. Revise el resultado. Verá la canalización se activará automáticamente cada 15 minutos, y solo los archivos nuevos o modificados desde el almacén de origen se copiarán en el almacén de destino en cada ejecución de canalización.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate15.png)
    
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)