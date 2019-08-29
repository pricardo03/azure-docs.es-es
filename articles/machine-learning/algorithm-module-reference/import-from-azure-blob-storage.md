---
title: 'Import from Azure Blob Storage: referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: En este tema se describe cómo usar el módulo Import from Azure Blob Storage (Importación dese Azure Blob Storage) en Azure Machine Learning Service para leer datos de Azure blob Storage, para que puede usar los datos en un experimento de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: fea64070c496379351bb75f2a38aba9b4db70dcd
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128715"
---
# <a name="import-from-azure-blob-storage-module"></a>Módulo Import from Azure Blob Storage

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Use este módulo para leer datos de Azure Blob Storage, para poder usar los datos en un experimento de aprendizaje automático.  

Azure Blob Service sirve para almacenar grandes cantidades de datos, incluidos datos binarios. Puede acceder a los blobs de Azure desde cualquier lugar, mediante HTTP o HTTPS. La autenticación puede ser necesaria según el tipo de almacenamiento de blobs. 

- Cualquier persona puede acceder a los blobs públicos, así como usuarios que tengan una dirección URL de SAS.
- Los blobs privados requieren un inicio de sesión y credenciales.

Importing from Blob Storage requiere que los datos se almacenan en blobs que usen el formato **blob en bloques**. Los archivos almacenados en el blob deben tener el formato de separación por comas (CSV) o separación por tabulaciones (TSV). Al leer el archivo, los registros y los encabezados de atributo aplicables se cargan como filas en la memoria como un conjunto de datos.


Se recomienda encarecidamente generar el perfil de los datos antes de importar, para asegurarse de que el esquema sea el esperado. El proceso de importación examina algunas filas de encabezado para determinar el esquema, pero las filas posteriores pueden contener columnas adicionales o datos que provoquen errores.



## <a name="manually-set-properties-in-the-import-data-module"></a>Establecer manualmente las propiedades en el módulo Importar datos

En los pasos siguientes se describe cómo configurar manualmente el origen de la importación.

1. Agregue el módulo **Importación de datos** al experimento. Puede encontrar este módulo en la interfaz, en **Entrada y salida de datos**.

2. En **Origen de datos**, seleccione **Azure Blob Storage**.

3. Para **Tipo de autenticación**, elija **Public (SAS URL)** (Público [dirección URL de SAS]) si sabe que la información se ha proporcionado como origen de datos público. Una dirección URL de SAS es una dirección URL por tiempo limitado para el acceso público, que puede generar mediante una utilidad de almacenamiento de Azure.

    De lo contrario, elija **Cuenta**.

4. Si los datos están en un blob **público** al que se puede acceder mediante una dirección URL de SAS, no necesita credenciales adicionales, ya que la cadena de dirección URL contiene toda la información necesaria para la descarga y autenticación.

    En el campo **URI**, escriba o pegue el URI completo que define la cuenta y el blob público.



5. Si los datos están en una cuenta **privada**, debe proporcionar credenciales, incluido el nombre de cuenta y la clave.

    - Para **Nombre de cuenta**, escriba o pegue el nombre de la cuenta que contiene el blob al que quiere acceder.

        Por ejemplo, si la dirección URL completa de la cuenta de almacenamiento es `http://myshared.blob.core.windows.net`, escribiría `myshared`.

    - Para **Clave de cuenta**, pegue la clave de acceso de almacenamiento asociada con la cuenta.

        Si no conoce la clave de acceso, consulte la sección "Administración de una cuenta de almacenamiento" en este artículo: [Acerca de las cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

6. Para **Path to container, directory, or blob** (Ruta de acceso a contenedor, directorio lo blob), escriba el nombre del blob específico que quiere recuperar.

    Por ejemplo, si se ha cargado un archivo denominado **data01.csv** al contenedor **trainingdata** en una cuenta denominada **mymldata**, la dirección URL completa del archivo sería: `http://mymldata.blob.core.windows.net/trainingdata/data01.txt`.

    Por lo tanto, en el campo **Path to container, directory, or blob**, escribiría: `trainingdata/data01.csv`.

    Para importar varios archivos, puede usar los caracteres comodín `*` (asterisco) o `?` (signo de interrogación).

    Por ejemplo, suponiendo que el contenedor `trainingdata` incluya varios archivos de un formato compatible, puede usar la siguiente especificación para leer todos los archivos que empiecen con `data`y concatenarlos en un único conjunto de datos:

    `trainingdata/data*.csv`

    No puede usar caracteres comodín en nombres de contenedor. Si tiene que importar archivos de varios contenedores, use una instancia independiente del módulo **Importación de datos** para cada contenedor y, luego, combine los conjuntos de datos con el módulo [Add Rows](./add-rows.md) (Agregar filas).

    > [!NOTE]
    > Si ha seleccionado la opción **Use cached results** (Usar resultados en caché), los cambios que haga en los archivos en el contenedor no desencadenan una actualización de los datos en el experimento.

7. Para **Formato de archivo de blob**, seleccione una opción que indique el formato de los datos que se almacenan en el blob, de modo que Azure Machine Learning pueda procesar los datos de forma adecuada. Se admiten los siguientes formatos:

    - **CSV**: Los valores separados por comas (CSV) son el formato de almacenamiento predeterminado para exportar e importar archivos en Azure Machine Learning. Si los datos ya contienen una fila de encabezado, no olvide seleccionar la opción, **El archivo tiene una fila de encabezado** o, de lo contrario, el encabezado se tratará como una fila de datos.

       

    - **TSV**: Muchas herramientas de aprendizaje automático usan el formato de valores separados por tabulaciones (TSV). Si los datos ya contienen una fila de encabezado, no olvide seleccionar la opción, **El archivo tiene una fila de encabezado** o, de lo contrario, el encabezado se tratará como una fila de datos.

       

    - **ARFF**: Este formato admite la importación de archivos en el formato usado por el conjunto de herramientas Weka. 

   

8. Ejecute el experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 