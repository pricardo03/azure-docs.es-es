---
title: 'Detalles del escaparate electrónico de una oferta de la aplicación Power BI: Azure Marketplace | Microsoft Docs'
description: Configure los campos de detalles del escaparate electrónico para una oferta de la aplicación Power BI para Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: pbutlerm
ms.openlocfilehash: ffc0d8e9e41cf4d202b4040ce3f09e7180101c8b
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665846"
---
# <a name="power-bi-apps-storefront-details-tab"></a>Pestaña de detalles del escaparate electrónico de aplicaciones Power BI

Use la pestaña **Detalles del escaparate electrónico** de la página **Nueva oferta** para proporcionar información de ventas, marketing y legal a sus clientes potenciales. Esta pestaña también especifica cómo administrar clientes potenciales generados desde Marketplace. Este largo formulario se divide en seis secciones: **Detalles de la oferta**, **Detalles del anuncio**, **Artefactos de marketing**, **Legal**, **Asistencia al cliente** y **Administración de clientes potenciales**.  Un asterisco (*) anexo a la etiqueta del campo indica que es obligatorio.


## <a name="offer-details-section"></a>Sección Detalles de la oferta

En esta sección, se escribe la información general sobre la oferta de AppSource.

![Sección Detalles de la oferta en la pestaña Detalles del escaparate electrónico](./media/offer-details-section.png)

En la tabla siguiente se describen el nombre y la finalidad de estos campos.

|   Campo               |   DESCRIPCIÓN                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------|
| **Offer summary** (Resumen de la oferta)     | Propósito resumido de la aplicación. Tiene una longitud máxima de 100 caracteres.                             |
| **Offer description** (Descripción de la oferta) | Descripción de la aplicación. La longitud máxima es de 3000 caracteres y admite el formato HTML simple. |
|                       |                                                                                         |


## <a name="listing-details-section"></a>Sección Detalles del anuncio

Esta segunda sección proporciona contexto adicional para su aplicación: en qué sectores suele utilizarse, qué categoría es más conveniente para ella, productos compatibles y términos de búsqueda asociados.

![Sección Detalles del anuncio en la pestaña Detalles del escaparate electrónico](./media/listing-details-section.png)

 En la tabla siguiente se describen el nombre y la finalidad de estos campos.
 
|   Campo                                  |   DESCRIPCIÓN                                                        |
| --------------                           | ---------------------                                                |
| **Industrias**                           | Seleccione el sector más afín a la aplicación. Si la aplicación está relacionada con varios sectores, puede dejar esta opción en blanco.      |
| **Categorías**                           | Seleccione las categorías que sean pertinentes para su aplicación. Seleccione un máximo de 3.     |
| **Vínculo de ayuda de la aplicación**               | Dirección URL a una página que contiene ayuda en pantalla para la aplicación           |
| **Productos con los que funciona su aplicación (máximo de 3)** | Indique los productos específicos con los que funciona la aplicación. Puede enumerar tres productos como máximo. Para agregar un producto a la lista, haga clic en el signo más (junto a la opción Nuevo) y se creará un nuevo campo de texto abierto en el que podrá escribir el nombre de un producto con el que funcione la aplicación.      |
| **Palabras clave de búsqueda (máximo de 3)**              | AppSource permite que el cliente haga una búsqueda basada en palabras clave. Puede escribir el conjunto de palabras para las cuales la aplicación se mostrará a los clientes. Por ejemplo, si la aplicación es "Mi aplicación de correo electrónico", algunas palabras clave pueden ser "correos electrónicos", "correo" o "aplicación de correo". Elija las palabras que es probable que los usuarios usen para buscar su aplicación en el cuadro de búsqueda de AppSource. |
|  |  |


## <a name="marketing-artifacts-section"></a>Sección de artefactos de marketing

Esta tercera sección permite cargar el material de personalización de marca y marketing.  Se divide en cuatro subsecciones: **Logotipos**, **vídeos**, **documentos** y **capturas de pantalla**. Los logotipos y las capturas de pantalla son los únicos artefactos de marketing obligatorios, aunque todos son muy recomendables para atraer al consumidor.

![Sección Artefactos de marketing en la pestaña Detalles del escaparate electrónico](./media/marketing-artifacts-section.png)

 
|    Campo                             |    DESCRIPCIÓN                                                    |
|   -----------                        |    -------------                                                  |
| *Logotipos*                              |                                                                   |
| **Logotipo de la oferta (formato .png, 48 x 48)**   | Aparece en AppSource en la información general o los resultados de la aplicación, cuando se completa una búsqueda. Solo es compatible un formato png con una resolución de 48px\*48px.  |
| **Logotipo de la oferta (formato .png, 216 x 216)** | Aparece en AppSource en la página de detalles de la aplicación.  Solo es compatible el formato png con una resolución de 216px\*216px.  |
| *Vídeos*                             |                                                                   |
| **Nombre**                             | Nombre o título de la aplicación.                                          |
| **URL**                              | Dirección URL del vídeo hospedado en YouTube o Vimeo.                              |
| **Miniatura**                        | Imagen en miniatura de la aplicación.  Solo es compatible el formato png con una resolución de 1280px\*720px.   |
| *Documentos*                          | Es opcional, pero se admiten tres documentos como máximo. Los documentos que cargue aquí aparecerán en AppSource en "Más información".  |
| **Nombre**                             | Nombre o título del documento complementario.                              |
| **Archivo**                             | El documento cargado debe tener un formato PDF.                             |
| *Capturas de pantalla*                        | Es opcional, pero se admite un máximo de cinco capturas de pantalla.                        |
| **Nombre**                             | Nombre o título de la captura de pantalla.                                       |
| **Imagen**                            | La imagen de captura de pantalla cargada debe tener un formato png con una resolución de 1280px\*720px.  | 
|   |   |


### <a name="logo-guidelines"></a>Directrices para logotipos

Todos los logotipos cargados en [Cloud Partner Portal](https://cloudpartner.azure.com) deben seguir estas instrucciones:

- No use un fondo degradado en el logotipo.
- Evite colocar texto, incluso el nombre de la empresa o la marca, en el logotipo. El aspecto del logotipo debe ser "plano" y evitar los degradados.
- No extienda el logotipo.


## <a name="legal-section"></a>Sección legal

Esta última sección permite proporcionar los dos documentos legales necesarios para cada oferta: Directiva de privacidad y Términos de uso.

![Sección Legal en la pestaña Detalles del escaparate electrónico](./media/legal-section.png)

|   Campo                |   DESCRIPCIÓN                           |
|------------------------|--------------------------------------   |
| **Dirección URL de la directiva de privacidad** | Dirección URL de la directiva de privacidad publicada       |
| **Condiciones de uso**       | Directiva como texto sin formato o HTML simple.     |
|  |  |


## <a name="customer-support-section"></a>Sección Asistencia al cliente

Proporcione la **dirección URL de soporte técnico** para la página de asistencia al cliente en línea.  Es mejor si esta página de soporte técnico en línea proporciona a los clientes varias opciones de contacto, como teléfono, correo electrónico y chat en directo. 


## <a name="lead-management-section"></a>Sección de administración de clientes potenciales

La última sección permite recopilar los clientes potenciales generados a partir de las ofertas de AppSource. Ofrece las opciones de almacenamiento siguientes (en una lista desplegable) para esta información de clientes potenciales.

|    Campo               |   Destino de clientes potenciales                               |
|------------------------|--------------------------------------            |
|  **None**              | Los clientes potenciales no se recopilan (opción predeterminada).  |
| **Azure Blob (en desuso)** | Una instancia de [Azure Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview), especificada por un nombre de contenedor y una cadena de conexión.  Esta opción está en desuso; use **Azure Table** en su lugar.  |
| **Tabla de Azure**        | Una instancia de [Azure Table](https://docs.microsoft.com/azure/cosmos-db/table-storage-overview) especificada por una cadena de conexión.  |
| **Dynamics CRM Online** | Una instancia de [Microsoft Dynamics 365 Online](https://dynamics.microsoft.com/) especificada por una dirección URL y credenciales de autenticación. |
| **Punto de conexión HTTPS**     | El punto de conexión HTTPS especificado como una carga JSON.   |
| **Marketo**            | Una instancia de [Marketo](https://www.marketo.com/) especificada por el id. de servidor, el id. de Munchkin y el id. de formulario.   |
| **Salesforce**         | Una base de datos de [Salesforce](https://www.salesforce.com/) especificada por un identificador de objeto. |
|  |  |

Una vez publicada la oferta, se valida la conexión de clientes potenciales y se envía de forma automática un cliente potencial de prueba al destino especificado. La información sobre los clientes potenciales se debe administrar de forma continua, y esta configuración debe actualizarse con prontitud para reflejar la arquitectura de administración de clientes actual.


## <a name="next-steps"></a>Pasos siguientes

En la pestaña [Contactos](./cpp-contacts-tab.md) siguiente, proporcionará los recursos de soporte técnico y de usuario para la oferta.
