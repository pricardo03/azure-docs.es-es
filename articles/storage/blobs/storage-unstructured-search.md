---
title: 'Tutorial: Búsqueda de datos no estructurados en Azure Blob Storage'
description: 'Tutorial: Búsqueda de datos no estructurados en Blob Storage mediante Azure Search'
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 12/13/2018
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: 8c11d2e6d9a902707d3fd98f09d3474a5d2a5f64
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150790"
---
# <a name="tutorial-search-unstructured-data-in-cloud-storage"></a>Tutorial: Búsqueda de datos no estructurados en almacenamiento en la nube

En este tutorial, aprenderá a buscar datos no estructurados con [Azure Search](../../search/search-what-is-azure-search.md) mediante los datos almacenados en Azure Blob Storage. Los datos no estructurados son datos que no están organizados de manera predefinida o que no tienen un modelo de datos. Un ejemplo es un archivo .txt.

En este tutorial también se requiere tener una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un grupo de recursos
> * Crear una cuenta de almacenamiento
> * Crear un contenedor
> * Cargar datos en el contenedor
> * Crear un servicio de búsqueda a través del portal
> * Conectar un servicio de búsqueda a una cuenta de almacenamiento
> * Creación de un origen de datos
> * Configuración del índice
> * Creación de un indexador
> * Usar el servicio de búsqueda para buscar en el contenedor

## <a name="prerequisites"></a>Requisitos previos

Cada cuenta de almacenamiento debe pertenecer a un grupo de recursos de Azure. Un grupo de recursos es un contenedor lógico para agrupar servicios de Azure. Al crear una cuenta de almacenamiento, puede elegir entre crear un grupo de recursos o usar uno existente. En este tutorial se crea un grupo de recursos nuevo.

Inicie sesión en el [Azure Portal](https://portal.azure.com).

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

Se ha preparado un conjunto de datos de ejemplo para que pueda usarlo en este tutorial. Descargue [clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip) y descomprímalo en su propia carpeta.

El ejemplo consta de los archivos de texto obtenidos de [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Este tutorial los usa como archivos de texto de ejemplo que se buscan con los servicios de Azure Search.

## <a name="create-a-container"></a>Crear un contenedor

Los contenedores son similares a carpetas y se usan para almacenar blobs.

En este tutorial, usará un único contenedor para almacenar los archivos de texto obtenidos de clinicaltrials.gov.

1. Vaya a la cuenta de almacenamiento en Azure Portal.

2. Seleccione **Examinar blobs** en **Blob Service**.

3. Agregue un nuevo contenedor.

4. Asigne al contenedor el nombre **data** y seleccione **Contenedor** para el nivel de acceso público.

5. Seleccione **Aceptar** para crear el contenedor.

   ![Búsqueda no estructurada](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>Carga de los datos de ejemplo

Ahora que tiene un contenedor, puede cargar los datos de ejemplo en él.

1. Seleccione el contenedor y seleccione **Cargar**.

2. Seleccione el icono de la carpeta azul representado junto al campo **Archivos** y vaya a la carpeta local en la que extrajo los datos de ejemplo.

3. Seleccione todos los archivos extraídos y, después, **Abrir**.

4. Seleccione **Cargar** para comenzar el proceso de carga.

   ![Búsqueda no estructurada](media/storage-unstructured-search/upload.png)

El proceso de carga puede tardar unos instantes.

Cuando finalice, vuelva al contenedor de datos para confirmar la carga de los archivos de texto.

  ![Búsqueda no estructurada](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Creación de una instancia de Search Service

Azure Search es una solución de búsqueda como servicio en la nube que ofrece a los desarrolladores las API y las herramientas necesarias para permitir la experiencia de búsqueda en los datos.

En este tutorial, usará un servicio de búsqueda para buscar en los archivos de texto obtenidos de clinicaltrials.gov.

1. Vaya a la cuenta de almacenamiento en Azure Portal.

2. Desplácese hacia abajo y seleccione **Agregar Azure Search** en **BLOB SERVICE**.

3. En **Importar datos**, seleccione **Elegir el servicio**.

4. Seleccione **Haga clic aquí para crear un nuevo servicio de búsqueda**.

5. Dentro de **Nuevo servicio de búsqueda**, escriba un nombre único para el servicio de búsqueda en el campo **URL**.

6. En **Grupo de recursos**, seleccione **Usar existente** y elija el grupo de recursos que creó anteriormente.

7. En **Plan de tarifa**, seleccione el nivel **Gratis** y haga clic en **Seleccionar**.

8. Seleccione **Crear** para crear el servicio de búsqueda.

   ![Búsqueda no estructurada](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Conexión del servicio de búsqueda al contenedor

Ahora que tiene un servicio de búsqueda, puede asociarlo a su instancia de Blob Storage. Esta sección le guía en el proceso de elegir un origen de datos, crear un índice y crear un indexador.

1. Vaya a la cuenta de almacenamiento.

2. Seleccione **Agregar Azure Search** en **BLOB SERVICE**.

3. Seleccione **Search Service** dentro de **Importar datos** y haga clic en el servicio de búsqueda que creó en la sección anterior. Así se abre el campo **Nuevo origen de datos**.

### <a name="create-a-data-source"></a>Creación de un origen de datos

  Un origen de datos especifica qué datos indexar y cómo acceder a los datos. Un origen de datos se puede usar varias veces por parte del mismo servicio de búsqueda.

1. Escriba el nombre del origen de datos. En **Datos que se extraerán**, seleccione **Contenido y metadatos**. El origen de datos especifica qué partes del blob se indexan.

2. Puesto que los blobs que usa son archivos de texto, establezca el **modo de análisis** en **Texto**.

   ![Búsqueda no estructurada](media/storage-unstructured-search/datasources.png)

3. Seleccione **Contenedor de almacenamiento** para enumerar las cuentas de almacenamiento disponibles.

4. Seleccione la cuenta de almacenamiento y luego el contenedor que creó anteriormente.

   ![Búsqueda no estructurada](media/storage-unstructured-search/datacontainer.png)

5. Haga clic en **Seleccionar** para volver a **Nuevo origen de datos** y seleccione **Aceptar** para continuar.

### <a name="configure-the-index"></a>Configuración del índice

  Un índice es una colección de campos del origen de datos que se pueden buscar. Establezca y configure los parámetros en estos campos para que el servicio de búsqueda sepa de qué manera se deben buscarse los datos.

1. En **Importar datos**, seleccione **Personalizar índice de destino**.

2. Escriba el nombre del índice en el campo **Nombre de índice**.

3. Active la casilla del atributo **Recuperable** en **metadata_storage_name**.

   ![Búsqueda no estructurada](media/storage-unstructured-search/valuestoselect.png)

4. Seleccione **Aceptar**. Se abrirá **Crear indexador**.

Los parámetros de su índice y los atributos que asigne a esos parámetros son importantes. Los parámetros especifican *qué* datos se almacenarán y los atributos *cómo* se almacenarán esos datos.

La columna **Nombre de campo** contiene los parámetros. En la tabla siguiente se proporciona una lista de los atributos disponibles y sus descripciones.

#### <a name="field-attributes"></a>Atributos de campo

| Atributo | DESCRIPCIÓN |
| --- | --- |
| *Clave* |Una cadena que proporciona el identificador único de cada documento, que se usa para buscar los documentos. Todos los índices deben tener una clave. Solo un campo puede ser la clave y se debe establecer su tipo en Edm.String. |
| *Retrievable* |Establece si el campo se puede devolver en un resultado de búsqueda. |
| *Filterable* |Permite que el campo se use en consultas de filtro. |
| *Sortable* |Permite que una consulta ordene los resultados de búsqueda mediante este campo. |
| *Facetable* |Permite que un campo se use en una estructura de navegación con facetas para el filtrado autodirigido. Normalmente, los campos que contienen valores repetitivos que se pueden usar para agrupar documentos (por ejemplo, varios documentos que forman parte de una única categoría de servicio o un único producto) funcionan mejor como facetas. |
| *Searchable* |Marca el campo como campo de búsqueda de texto completo. |

### <a name="create-an-indexer"></a>Creación de un indexador

  Un indexador conecta un origen de datos con un índice de búsqueda y proporciona una programación para volver a indexar los datos.

1. Escriba un nombre en el campo **Nombre** y seleccione **Aceptar**.

   ![Búsqueda no estructurada](media/storage-unstructured-search/exindexer.png)

2. Vuelve a **Importar datos**. Seleccione **Aceptar** para completar el proceso de conexión.

Ahora ha conectado correctamente el blob al servicio de búsqueda. El portal tarda unos minutos en mostrar que el índice se ha rellenado. Sin embargo, el servicio de búsqueda comienza inmediatamente la indexación para que pueda empezar a buscar enseguida.

## <a name="search-your-text-files"></a>Búsqueda en los archivos de texto

Para buscar en los archivos, abra el Explorador de búsqueda dentro del índice de su servicio de búsqueda recién creado.

Los pasos siguientes muestran dónde encontrar el Explorador de búsqueda y proporcionan algunas consultas de ejemplo:

1. Vaya a todos los recursos y busque el servicio de búsqueda recién creado.

   ![Búsqueda no estructurada](media/storage-unstructured-search/exampleurl.png)

2. Seleccione el índice para abrirlo.

   ![Búsqueda no estructurada](media/storage-unstructured-search/overview.png)

3. Seleccione **Explorador de búsqueda** para abrir el Explorador de búsqueda, donde puede realizar consultas en vivo en los datos.

   ![Búsqueda no estructurada](media/storage-unstructured-search/indexespane.png)

4. Seleccione **Buscar** mientras el campo de cadena de consulta está vacío. Una consulta vacía devuelve *todos* los datos de sus blobs.

   ![Búsqueda no estructurada](media/storage-unstructured-search/emptySearch.png)

### <a name="perform-a-full-text-search"></a>Realiza una búsqueda de texto completo

Escriba **Miopía** en el campo **Cadena de consulta** y seleccione **Buscar**. Así se inicia una búsqueda del contenido de los archivos y se devuelve un subconjunto que contiene la palabra "Miopía".

  ![Búsqueda no estructurada](media/storage-unstructured-search/secondMyopia.png)

### <a name="perform-a-system-properties-search"></a>Realiza una búsqueda de propiedades del sistema

Además de una búsqueda de texto completo, puede crear consultas que buscan por propiedades del sistema mediante el parámetro `$select`.

Escriba `$select=metadata_storage_name` en la cadena de consulta y presione **Entrar**. Así, solo se devuelve ese campo en particular.

La cadena de consulta está modificando directamente la dirección URL, por lo que no se permiten espacios. Para buscar en varios campos, use una coma, por ejemplo: `$select=metadata_storage_name,metadata_storage_path`.

El parámetro `$select` solo puede usarse con campos que estén marcados como recuperables al definir el índice.

  ![Búsqueda no estructurada](media/storage-unstructured-search/metadatasearchunstructured.png)

Ahora ha completado este tutorial y tiene un conjunto de datos no estructurados que permiten búsqueda.

## <a name="clean-up-resources"></a>Limpieza de recursos

La forma más fácil de quitar los recursos que ha creado es eliminar el grupo de recursos. Al quitar el grupo de recursos también se eliminan todos los recursos contenidos en él. En el ejemplo siguiente, al quitar el grupo de recursos, se quita la cuenta de almacenamiento y el propio grupo de recursos.

1. En Azure Portal, vaya a la lista de los grupos de recursos en su suscripción.
2. Seleccione el grupo de recursos que desea eliminar.
3. Seleccione el botón **Eliminar grupo de recursos** y escriba el nombre del grupo de recursos en el campo de eliminación.
4. Seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Siga este vínculo para aprender más sobre la indexación de documentos con Azure Search:

> [!div class="nextstepaction"]
> [Indexación de documentos en Azure Blob Storage con Azure Search](../../search/search-howto-indexing-azure-blob-storage.md)
