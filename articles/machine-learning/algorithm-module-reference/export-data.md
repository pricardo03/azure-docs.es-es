---
title: 'Exportación de datos: referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Exportación de datos en Azure Machine Learning Service para guardar los resultados, los datos intermedios y los datos de trabajo de las canalizaciones en los destinos de almacenamiento en la nube fuera de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: b7b4b9de1e91279243e35f1b71f1ef6d2244e9e0
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693725"
---
# <a name="export-data-module"></a>Módulo Exportación de datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para guardar los resultados, los datos intermedios y los datos de trabajo de las canalizaciones en los destinos de almacenamiento en la nube fuera de Azure Machine Learning.

Este módulo admite exportar o guardar los datos en los siguientes servicios de datos en la nube:


- **Exportación a Azure Blob Storage**: Guarda los datos en Blob service en Azure. Los datos de Blob service se pueden compartir públicamente o guardar en almacenes de datos de la aplicación protegida.

  
## <a name="how-to-configure-export-data"></a>Procedimiento para configurar la exportación de datos

1. Agregue el módulo **Exportación de datos** a la canalización en la interfaz. Puede encontrar este módulo en la categoría **Entrada y salida**.

2. Conecte **Exportación de datos** al módulo que contiene los datos que desea exportar.

3. Haga doble clic en **Exportación de datos** para abrir el panel **Propiedades**.

4. Para **Data destination** (Destino de los datos), seleccione el tipo de almacenamiento en la nube donde guardará los datos. Si realiza cambios en esta opción, se restablecen todas las demás propiedades. Por tanto, asegúrese de elegir esta opción en primer lugar.

5. Proporcione un método de autenticación y nombre de cuenta necesarios para acceder a la cuenta de almacenamiento especificada.

    **Exportación a Azure Blob Storage** es la única opción en la versión preliminar privada. A continuación, se muestra cómo establecer el módulo.
    1. Azure Blob service sirve para almacenar grandes cantidades de datos, incluidos los datos binarios. Hay dos tipos de almacenamiento de blobs: blobs públicos y blobs que requieren credenciales de inicio de sesión.

    2. Para **Tipo de autenticación**, elija **Public (SAS)** [Pública (SAS)] si sabe que el almacenamiento admite el acceso mediante una dirección URL de SAS.

          Una dirección URL SAS es un tipo especial de dirección URL que se puede generar con una utilidad de Azure Storage y está disponible solo durante un tiempo limitado.  Contiene toda la información necesaria para la autenticación y la descarga.

        Para **URI**, escriba o pegue el URI completo que define la cuenta y el blob público.

        Formato de archivo, se admiten CSV y TSV.

    3. Para las cuentas privadas, elija **Cuenta**y proporcione el nombre de cuenta y la clave de cuenta, para que la canalización pueda escribir en la cuenta de almacenamiento.

         - **Nombre de cuenta**: Escriba o pegue el nombre de la cuenta en la desea guardar los datos. Por ejemplo, si la dirección URL completa de la cuenta de almacenamiento es `http://myshared.blob.core.windows.net`, escribiría `myshared`.

        - **Clave de cuenta**: Pegue la clave de acceso de almacenamiento asociada con la cuenta.

        -  **Ruta de acceso al contenedor, directorio o blob**: Escriba el nombre del blob donde se almacenarán los datos exportados. Por ejemplo, para guardar los resultados de la canalización en un nuevo blob denominado **results01.csv** del contenedor **predictions** en una cuenta denominada **mymldata**, la dirección URL completa para el blob sería `http://mymldata.blob.core.windows.net/predictions/results01.csv`.

            Por lo tanto, en el campo **Ruta de acceso al contenedor, directorio o blob**, debería especificar el contenedor y el nombre de blob como sigue: `predictions/results01.csv`

        - Si especifica el nombre de un blob que aún no existe, Azure creará el blob.

       -  Al escribir en un blob existente, puede especificar que se sobrescriba el contenido actual del blob estableciendo la propiedad, **Azure blob storage write mode** (Modo de escritura de Azure Blob Storage). De manera predeterminada, esta propiedad se establece en **Error**, lo que significa que se produce un error cada vez que se encuentra un archivo de blob existente con el mismo nombre.


    4. Para **File format for blob file** (Formato de archivo de blob), seleccione el formato en el que se deben almacenar los datos.

        - **CSV**: Valores separados por comas (CSV) son el formato de almacenamiento predeterminado. Para exportar los encabezados de columna junto con los datos, seleccione la opción **Write blob header row** (Escribir fila de encabezado de blob).  Para obtener más información acerca del formato delimitado por comas usado en Azure Machine Learning, consulte [Convert to CSV](./convert-to-csv.md) (Convertir a CSV).

        - **TSV**: El formato de valores separados por tabulaciones (TSV) es compatible con muchas herramientas de Machine Learning. Para exportar los encabezados de columna junto con los datos, seleccione la opción **Write blob header row** (Escribir fila de encabezado de blob).  

 
    5. **Utilizar resultados almacenados en caché**: Seleccione esta opción si no quiere tener que escribir los resultados en el archivo de blob cada vez que ejecute la canalización. Si no hay ningún otro cambio en los parámetros del módulo, la canalización solo escribe los resultados la primera vez que se ejecuta el módulo o cuando hay cambios en los datos.

    6. Ejecución de la canalización

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 