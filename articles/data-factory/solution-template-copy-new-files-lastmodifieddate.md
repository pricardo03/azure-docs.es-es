---
title: Copia de archivos nuevos y cambiados por LastModifiedDate con Azure Data Factory
description: Aprenda a usar una plantilla de solución para copiar archivos nuevos y cambiados por LastModifiedDate con Azure Data Factory.
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
ms.openlocfilehash: aaa7114113d5f0330d2dc7d656b0d91963931512
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684234"
---
# <a name="copy-new-and-changed-files-by-lastmodifieddate-with-azure-data-factory"></a>Copia de archivos nuevos y cambiados por LastModifiedDate con Azure Data Factory

En este artículo se describe una plantilla de solución que puede usar para copiar archivos nuevos y cambiados solo por LastModifiedDate desde un almacén basado en archivos en un almacén de destino. 

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla selecciona primero los archivos nuevos y cambiados solo por sus atributos **LastModifiedDate** y, a continuación, copia esos archivos seleccionados desde el almacén de origen de datos en el almacén de destino de datos.

La plantilla contiene una actividad:
- **Copia** para copiar archivos nuevos y cambiados solo por LastModifiedDate desde un almacén de archivos en un almacén de destino.

La plantilla define cuatro parámetros:
-  *FolderPath_Source* es la ruta de acceso a la carpeta donde puede leer los archivos del almacén de origen. Debe reemplazar el valor predeterminado por su propia ruta de acceso a la carpeta.
-  *FolderPath_Destination* es la ruta de acceso a la carpeta donde desea copiar archivos en el almacén de destino. Debe reemplazar el valor predeterminado por su propia ruta de acceso a la carpeta.
-  *LastModified_From* se usa para seleccionar los archivos cuyo atributo LastModifiedDate es posterior o igual a este valor datetime.  Para seleccionar solo los archivos nuevos, los cuales no se han copiado la última vez, este valor datetime puede ser la hora a la que se desencadenó la canalización la última vez. Puede reemplazar el valor predeterminado '2019-02-01T00:00:00Z' por su LastModifiedDate esperado en la zona horaria UTC. 
-  *LastModified_To* se usa para seleccionar los archivos cuyo atributo LastModifiedDate es anterior a este valor datetime. Para seleccionar solo los archivos nuevos, los cuales no se han copiado la última vez, este valor datetime puede ser la hora actual.  Puede reemplazar el valor predeterminado '2019-02-01T00:00:00Z' por su LastModifiedDate esperado en la zona horaria UTC. 

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **Copia de archivos nuevos por LastModifiedDate**. Cree una **nueva** conexión con el almacén de almacenamiento de origen. El almacén de almacenamiento de origen es desde donde se van a copiar los archivos.

    ![Creación de una nueva conexión con el origen](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate1.png)
    
2. En primer lugar, seleccione el **tipo** de almacenamiento. A continuación, escriba el **nombre de la cuenta** de almacenamiento y la **clave de cuenta**. Por último, seleccione **Finalizar**.

    ![Incorporación de una cadena de conexión](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate2.png)
    
3. Cree una **nueva** conexión con el almacén de destino. El almacén de destino es donde se van a copiar los archivos. También debe especificar la información de conexión del almacén de destino de datos de forma similar a como lo hizo en el paso 2.

    ![Creación de una nueva conexión con el destino](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate3.png)

4. Seleccione **Usar esta plantilla**.

    ![Uso de esta plantilla](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate4.png)
    
5. Verá la canalización disponible en el panel, como se muestra en el ejemplo siguiente:

    ![Visualización de la canalización](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate5.png)

6. Seleccione **Depurar**, escriba el valor de los **parámetros** y seleccione **Finalizar**.  En la imagen que se muestra a continuación, establecemos los parámetros de la siguiente manera.
   - **FolderPath_Source** =  **/source/**
   - **FolderPath_Destination** =  **/destination/**
   - **LastModified_From** =  **2019-02-01T00:00:00Z**
   - **LastModified_To** = **2019-03-01T00:00:00Z**
    
     En el ejemplo se indican los archivos que se han modificado por última vez en el intervalo de tiempo entre *2019-02-01T00:00:00Z* y *2019-03-01T00:00:00Z*. Estos se copiarán desde una carpeta */source/* en una carpeta */destination/* .  Puede reemplazarlos por sus propios parámetros.
    
     ![Ejecución de la canalización](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate6.png)

7. Revise el resultado. Verá solo los archivos modificados por última vez en el intervalo configurado. Este se ha copiado en el almacén de destino.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate7.png)
    
8. Ahora puede agregar un desencadenador de ventanas de saltos de tamaño constante para automatizar esta canalización para que siempre pueda copiar archivos nuevos y cambiados solo por LastModifiedDate de forma periódica.  Seleccione **Agregar desencadenador** y luego **Nuevo/Editar**.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate8.png)
    
9. En la ventana **Agregar desencadenadores**, seleccione **+ Nuevo**.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate9.png)

10. Seleccione **Ventana de saltos de tamaño constante** para el tipo de desencadenador, establezca **Cada 15 minutos** como periodicidad (puede cambiarla por cualquier intervalo de tiempo) y, a continuación, seleccione **Siguiente**.

    ![Creación de un desencadenador](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate10.png)    
    
11. Escriba el valor de los **parámetros de ejecución de desencadenador** de la manera siguiente y seleccione **Finalizar**.
    - **FolderPath_Source** =  **/source/** .  Se puede reemplazar por su carpeta en el almacén de datos de origen.
    - **FolderPath_Destination** =  **/destination/** .  Se puede reemplazar por su carpeta en el almacén de datos de destino.
    - **LastModified_From** =   **\@trigger().outputs.windowStartTime**.  Es una variable del sistema del desencadenador que determina la hora a la que se desencadenó la canalización por última vez.
    - **LastModified_To** =  **\@trigger().outputs.windowEndTime**.  Es una variable del sistema del desencadenador que determina la hora a la que se desencadena la canalización esta vez.
    
    ![Parámetros de entrada](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate11.png)
    
12. Seleccione **Publish All** (Publicar todo).
    
    ![Publicar todo](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate12.png)

13. Cree nuevos archivos en su carpeta de origen del almacén de origen de datos.  Ahora espera a que la canalización se desencadene automáticamente y solo se copiarán los nuevos archivos en el almacén de destino.

14. Seleccione la pestaña **Supervisión** en el panel de navegación izquierdo y espere unos 15 minutos si la periodicidad del desencadenador se ha establecido en cada 15 minutos. 

    ![Selección de Supervisión](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate14.png)

15. Revise el resultado. Verá que su canalización se desencadenará automáticamente cada 15 minutos y solo se copiarán los archivos nuevos o cambiados del almacén de origen en el almacén de destino en cada ejecución de canalización.

    ![Revisión del resultado](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate15.png)
    
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)
