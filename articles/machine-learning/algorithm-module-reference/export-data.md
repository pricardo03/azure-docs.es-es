---
title: 'Exportación de datos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo exportar datos en el servicio de Azure Machine Learning para guardar los resultados, los datos intermedios y datos de trabajo de los experimentos en los destinos de almacenamiento en la nube fuera de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: c3744803f172edf9fbf2556a12677e8faef370c2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028326"
---
# <a name="export-data-module"></a>Módulo de exportación de datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para guardar los resultados, los datos intermedios y datos de trabajo de los experimentos en los destinos de almacenamiento en la nube fuera de Azure Machine Learning.

Este módulo admite exportar o guardar los datos en los siguientes servicios de datos en la nube:


- **Exportar a Azure Blob Storage**: Guarda los datos en Blob service en Azure. Los datos de Blob service pueden compartir públicamente o guardar en almacenes de datos de la aplicación protegida.

  
## <a name="how-to-configure-export-data"></a>Cómo configurar la exportación de datos

1. Agregar el **exportar datos** módulo al experimento en la interfaz. Puede encontrar este módulo en el **de entrada y salida** categoría.

2. Conectar **exportar datos** al módulo que contiene los datos que desea exportar.

3. Haga doble clic en **exportar datos** para abrir el **propiedades** panel.

4. Para **destino datos**, seleccione el tipo de almacenamiento en la nube donde deberá guardar los datos. Si realiza cambios en esta opción, se restablecen todas las demás propiedades. Por tanto, asegúrese de elegir esta opción en primer lugar.

5. Proporcionan un método de autenticación y nombre de cuenta necesario para acceder a la cuenta de almacenamiento especificada.

    **Exportar a Azure Blob Storage** es la única opción en versión preliminar privada. A continuación muestra cómo establecer el módulo.
    1. Es el servicio blob de Azure para almacenar grandes cantidades de datos, incluidos los datos binarios. Hay dos tipos de almacenamiento de blobs: blobs públicos y los blobs que requieran credenciales de inicio de sesión.

    2. Para **tipo de autenticación**, elija **público (SAS)** si sabe que el almacenamiento admite el acceso mediante una dirección URL de SAS.

          Una dirección URL de SAS es un tipo especial de dirección URL que se puede generar mediante una utilidad de almacenamiento de Azure y está disponible para solo un tiempo limitado.  Contiene toda la información necesaria para la autenticación y la descarga.

        Para **URI**, escriba o pegue el URI completo que define la cuenta y el blob público.

        Formato de archivo CSV y TSV se admiten.

    3. Para las cuentas de privada, elija **cuenta**y proporcione el nombre de cuenta y la clave de cuenta, por lo que puede escribir el experimento en la cuenta de almacenamiento.

         - **Nombre de cuenta**: Escriba o pegue el nombre de la cuenta en la desea guardar los datos. Por ejemplo, si la dirección URL completa de la cuenta de almacenamiento es `http://myshared.blob.core.windows.net`, escribiría `myshared`.

        - **Clave de cuenta**: Pegue la clave de acceso de almacenamiento que está asociada con la cuenta.

        -  **Ruta de acceso al contenedor, directorio o blob**: Escriba el nombre del blob donde se almacenarán los datos exportados. Por ejemplo, para guardar los resultados del experimento en un nuevo blob denominado **results01.csv** en el contenedor **predicciones** en una cuenta denominada **mymldata**, la dirección URL completa para el BLOB sería `http://mymldata.blob.core.windows.net/predictions/results01.csv`.

            Por lo tanto, en el campo **ruta de acceso al contenedor, directorio o blob**, debería especificar el contenedor y nombre de blob como sigue: `predictions/results01.csv`

        - Si especifica el nombre de un blob que aún no existe, Azure crea el blob.

       -  Al escribir en un blob existente, puede especificar que se sobrescribe el contenido actual del blob estableciendo la propiedad, **almacenamiento de blobs de Azure en modo de escritura**. De forma predeterminada, esta propiedad se establece en **Error**, lo que significa que se produce un error cada vez que se encuentra un archivo de blob existente del mismo nombre.


    4. Para **formato de archivo de blob**, seleccione el formato en el que se deben almacenar los datos.

        - **CSV**: Valores separados por comas (CSV) son el formato de almacenamiento predeterminada. Para exportar los encabezados de columna junto con los datos, seleccione la opción **fila de encabezado de blob de escritura**.  Para obtener más información acerca de la coma: formato delimitado usado en Azure Machine Learning, consulte [convertir a CSV](./convert-to-csv.md).

        - **TSV**: Formato de valores separados por tabulaciones (TSV) es compatible con muchas herramientas de aprendizaje automático. Para exportar los encabezados de columna junto con los datos, seleccione la opción **fila de encabezado de blob de escritura**.  

 
    5. **Utilizar resultados almacenados en caché**: Seleccione esta opción si desea evitar tener que escribir los resultados en el archivo blob cada vez que ejecute el experimento. Si no hay ningún otro cambio en los parámetros del módulo, el experimento escribe los resultados solo la primera vez que se ejecuta el módulo, o cuando hay cambios en los datos.

    6. Ejecute el experimento.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 