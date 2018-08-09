---
title: Registro de datos de Data Lake Store en Azure Data Catalog | Microsoft Docs
description: Registro de datos de Data Lake Store en Azure Data Catalog
services: data-lake-store,data-catalog
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 8da9f0f8aeb36d9ff2f87511c902dd719bc755b9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441611"
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registro de datos de Data Lake Store en Azure Data Catalog
En este artículo, obtendrá información sobre cómo integrar Azure Data Lake Store con Azure Data Catalog para hacer que los datos sean reconocibles dentro de una organización mediante la integración con Data Catalog. Para más información sobre la catalogación de datos, consulte [¿Qué es Azure Data Catalog?](../data-catalog/data-catalog-what-is-data-catalog.md). Para saber en qué escenarios se puede utilizar Data Catalog, consulte [Escenarios comunes de Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar este tutorial, debe contar con lo siguiente:

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
* **Habilite su suscripción de Azure** para la versión preliminar pública de Azure Data Lake Store. Consulte las [instrucciones](data-lake-store-get-started-portal.md).
* **Cuenta del Almacén de Azure Data Lake**. Siga las instrucciones que se describen en [Introducción a Azure Data Lake Store mediante Azure Portal](data-lake-store-get-started-portal.md). Para este tutorial, cree una cuenta de Data Lake Store denominada **datacatalogstore**.

    Una vez creada la cuenta, cargue un conjunto de datos de ejemplo en ella. En este tutorial, vamos a cargar todos los archivos .csv en la carpeta **AmbulanceData** del [repositorio Git de Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Puede utilizar varios clientes, como el [explorador de Azure Storage](http://storageexplorer.com/), para cargar datos en un contenedor de blobs.
* **Azure Data Catalog**. Su organización ya debe tener un Azure Data Catalog creado. Se permite solo un catálogo por cada organización.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Registro del Almacén de Data Lake como origen para el Catálogo de datos

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Vaya a `https://azure.microsoft.com/services/data-catalog`y haga clic en **Iniciado**.
1. Inicie sesión en el portal de Azure Data Catalog y haga clic en **Publicar datos**.

    ![Registrar un origen de datos](./media/data-lake-store-with-data-catalog/register-data-source.png "Registrar un origen de datos")
1. En la siguiente página, haga clic en **Iniciar aplicación**. Esto descargará el archivo de manifiesto de la aplicación en el equipo. Haga doble clic en este archivo para iniciar la aplicación.
1. En la página principal, haga clic en **Iniciar sesión**y escriba sus credenciales.

    ![Pantalla de bienvenida](./media/data-lake-store-with-data-catalog/welcome.screen.png "Pantalla de bienvenida")
1. En la página Seleccionar un origen de datos, seleccione **Azure Data Lake**, y, a continuación, haga clic en **Siguiente**.

    ![Seleccionar origen de datos](./media/data-lake-store-with-data-catalog/select-source.png "Seleccionar origen de datos")
1. En la siguiente página, proporcione el nombre de la cuenta del Almacén de Data Lake que desea registrar en el Catálogo de datos. Deje las demás opciones con el valor predeterminado y, a continuación, haga clic en **Conectar**.

    ![Conectar al origen de datos](./media/data-lake-store-with-data-catalog/connect-to-source.png "Conectar al origen de datos")
1. La página resultante se puede dividir en los siguientes segmentos.

    a. El cuadro **Jerarquía de servidor** representa la estructura de carpetas de la cuenta del Almacén de Data Lake. **$Root** representa la raíz de la cuenta de Data Lake Store y **AmbulanceData** representa la carpeta creada en la raíz de dicha cuenta.

    b. El cuadro **Objetos disponibles** muestra los archivos y carpetas de la carpeta **AmbulanceData**.

    c. En el cuadro **Objetos que se registrarán** se enumeran los archivos y las carpetas que quiere registrar en Azure Data Catalog.

    ![Ver estructura de datos](./media/data-lake-store-with-data-catalog/view-data-structure.png "Ver estructura de datos")
1. Para este tutorial, deberá registrar todos los archivos del directorio. Para ello, haga clic en el botón (![Mover objetos](./media/data-lake-store-with-data-catalog/move-objects.png "Mover objetos")) para mover todos los archivos al cuadro **Objetos que se registrarán**.

    Dado que los datos se registrarán en un catálogo de datos de toda la organización, es recomendable agregar algunos metadatos que podrá usar posteriormente para localizar rápidamente los datos. Por ejemplo, puede agregar una dirección de correo electrónico del propietario de los datos (por ejemplo, la persona que está cargando los datos) o agregar una etiqueta para identificar los datos. En la captura de pantalla siguiente se muestra una etiqueta que ha agregado a los datos.

    ![Ver estructura de datos](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Ver estructura de datos")

    Haga clic en **Registrar**.
1. La captura de pantalla siguiente indica que los datos se han registrado correctamente en el Catálogo de datos.

    ![Registro completo](./media/data-lake-store-with-data-catalog/registration-complete.png "Ver estructura de datos")
1. Haga clic en **Ver portal** para volver atrás al portal del Catálogo de datos y comprobar que ya puede acceder a los datos registrados desde el portal. Para realizar búsquedas en los datos, puede utilizar la etiqueta que usó al registrar los datos.

     ![Buscar datos en el catálogo](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Buscar datos en el catálogo")
1. Ahora ya puede realizar operaciones como agregar anotaciones y documentación a los datos. Para más información, consulte los vínculos siguientes:

    * [Anotación de orígenes de datos](../data-catalog/data-catalog-how-to-annotate.md)
    * [Orígenes de datos de documentos](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Otras referencias
* [Anotación de orígenes de datos](../data-catalog/data-catalog-how-to-annotate.md)
* [Orígenes de datos de documentos](../data-catalog/data-catalog-how-to-documentation.md)
* [Integración del Almacén de Data Lake con otros servicios de Azure](data-lake-store-integrate-with-other-services.md)
