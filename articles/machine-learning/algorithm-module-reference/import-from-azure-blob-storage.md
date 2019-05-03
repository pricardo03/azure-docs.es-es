---
title: 'Importación de Azure Blob Storage: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre que este tema describe cómo usar la importación del módulo de Azure Blob Storage en el servicio de Azure Machine Learning para leer datos desde Azure blob storage, por lo que puede usar los datos en un experimento de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 4ac98516c1a326e1ede09bbb9660113ffd0642a0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029691"
---
# <a name="import-from-azure-blob-storage-module"></a>Importación del módulo de Azure Blob Storage

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para leer datos desde Azure blob storage, por lo que puede usar los datos en un experimento de aprendizaje automático.  

Es el servicio de Blob de Azure para almacenar grandes cantidades de datos, incluidos los datos binarios. Los blobs de Azure pueden obtenerse desde cualquier lugar, mediante HTTP o HTTPS. La autenticación puede ser necesaria según el tipo de almacenamiento de blobs. 

- Blobs públicos pueden tener acceso cualquier usuario o los usuarios que tienen una dirección URL de SAS.
- Blobs privados requieren un inicio de sesión y las credenciales.

Importación desde blob storage requiere que los datos se almacenan en los blobs que usan el **blob en bloques** formato. Deben usar los archivos almacenados en el blob separados por comas (CSV) o separados por tabulaciones (TSV) formatos. Al leer el archivo, los registros y los encabezados de atributo aplicables se cargan como filas en la memoria como un conjunto de datos.


Se recomienda encarecidamente que se generan perfiles de los datos antes de importar, para asegurarse de que el esquema es como se esperaba. El proceso de importación examina un número de filas principales para determinar el esquema, pero las filas posteriores pueden contener columnas adicionales o datos que provocan errores.



## <a name="manually-set-properties-in-the-import-data-module"></a>Establecer manualmente las propiedades en el módulo importar datos

Los pasos siguientes describen cómo configurar manualmente el origen de importación.

1. Agregar el **importar datos** módulo al experimento. Puede encontrar este módulo en la interfaz, en el **datos de entrada y salida**

2. Para **origen de datos**, seleccione **Azure Blob Storage**.

3. Para **tipo de autenticación**, elija **público (dirección URL de SAS)** si sabe que se ha proporcionado la información como un origen de datos públicos. Una dirección URL de SAS es una dirección URL de límite de tiempo para el acceso público que pueden generar mediante una utilidad de almacenamiento de Azure.

    En caso contrario, elija **cuenta**.

4. Si los datos están en un **pública** blob que se puede acceder mediante una dirección URL de SAS, no necesita credenciales adicionales porque la cadena de dirección URL contiene toda la información necesaria para la autenticación y la descarga.

    En el **URI** campo, escriba o pegue el URI completo que define la cuenta y el blob público.



5. Si los datos están en un **privada** cuenta, debe proporcionar credenciales, incluidos el nombre de cuenta y la clave.

    - Para **nombreCuenta**, escriba o pegue el nombre de la cuenta que contiene el blob que desea tener acceso.

        Por ejemplo, si la dirección URL completa de la cuenta de almacenamiento es `http://myshared.blob.core.windows.net`, escribiría `myshared`.

    - Para **clave de cuenta**, pegue la clave de acceso de almacenamiento que está asociada con la cuenta.

        Si no conoce la clave de acceso, consulte la sección, "Administrar sus cuentas de almacenamiento de Azure" en este artículo: [Acerca de las cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

6. Para **ruta de acceso al contenedor, directorio o blob**, escriba el nombre del blob específico que desea recuperar.

    Por ejemplo, si se ha cargado un archivo denominado **data01.csv** al contenedor **trainingdata** en una cuenta denominada **mymldata**, sería la dirección URL completa del archivo: `http://mymldata.blob.core.windows.net/trainingdata/data01.txt` .

    Por lo tanto, en el campo **ruta de acceso al contenedor, directorio o blob**, escribiría: `trainingdata/data01.csv`

    Para importar varios archivos, puede usar los caracteres comodín `*` (asterisco) o `?` (signo de interrogación).

    Por ejemplo, suponiendo que el contenedor `trainingdata` contiene varios archivos de un formato compatible, puede usar la siguiente especificación para leer todos los archivos a partir de `data`y concatenarlas en un único conjunto de datos:

    `trainingdata/data*.csv`

    No se puede usar caracteres comodín en nombres de contenedor. Si necesita importar archivos de varios contenedores, use una instancia independiente de la **importar datos** módulo para cada contenedor y, a continuación, combinar los conjuntos de datos usando el [agregar filas](./add-rows.md) módulo.

    > [!NOTE]
    > Si ha seleccionado la opción **usar en caché de resultados**, los cambios realizados en los archivos en el contenedor no desencadenan una actualización de los datos en el experimento.

7. Para **formato de archivo de Blob**, seleccione una opción que indica el formato de los datos que se almacenan en el blob, por lo que Azure Machine Learning puede procesar los datos de forma adecuada. Se admiten los siguientes formatos:

    - **CSV**: Valores separados por comas (CSV) son el formato de almacenamiento predeterminada para exportar e importar archivos en Azure Machine Learning. Si los datos ya contienen una fila de encabezado, no olvide seleccionar la opción, **archivo tiene fila de encabezado**, o el encabezado se tratará como una fila de datos.

       

    - **TSV**: Valores separados por tabulaciones (TSV) son un formato utilizado por muchas herramientas de aprendizaje de automático. Si los datos ya contienen una fila de encabezado, no olvide seleccionar la opción, **archivo tiene fila de encabezado**, o el encabezado se tratará como una fila de datos.

       

    - **ARFF**: Este formato admite la importación de archivos en el formato usado por el conjunto de herramientas Weka. 

   

8. Ejecute el experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 