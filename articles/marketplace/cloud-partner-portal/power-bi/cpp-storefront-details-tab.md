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
ms.openlocfilehash: 2c99b20e554d92dbe63594d63525dd8e1d765423
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60726236"
---
# <a name="power-bi-app-storefront-details-tab"></a>Pestaña de detalles del escaparate electrónico de aplicación de Power BI

En el **nueva oferta** , utilice el **detalles del escaparate electrónico** ficha para proporcionar información de marketing, ventas y legal para sus clientes potenciales. En esta pestaña también puede configurar la administración de clientes potenciales que genera Azure Marketplace. Este largo formulario se divide en seis secciones: **Detalles de la oferta**, **Detalles del anuncio**, **Artefactos de marketing**, **Legal**, **Asistencia al cliente** y **Administración de clientes potenciales**.  Un asterisco (*) al final de una etiqueta de campo significa que el campo es obligatorio.


## <a name="offer-details-section"></a>Sección Detalles de la oferta

En el **detalles de la oferta** sección, especifique información general acerca de la oferta en AppSource.

![Sección Detalles de la oferta en la pestaña Detalles del escaparate electrónico](./media/offer-details-section.png)

Para completar los campos de esta sección, utilice la siguiente tabla:

|   Campo               |   DESCRIPCIÓN                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------|
| **Resumen de la oferta**     | Un breve propósito de la aplicación, con hasta 100 caracteres.                             |
| **Descripción de la oferta** | Descripción de la aplicación, con hasta 3000 caracteres. Este campo es compatible con el formato HTML simple. |
|                       |                                                                                         |


## <a name="listing-details-section"></a>Sección Detalles del anuncio

El **lista detalles** sección recopila información sobre el contexto de la aplicación: sectores donde normalmente se usa, la categoría de la aplicación, productos compatibles y los términos de búsqueda asociada.

![Sección Detalles del anuncio en la pestaña Detalles del escaparate electrónico](./media/listing-details-section.png)

Para completar los campos de esta sección, utilice la siguiente tabla:
 
|   Campo                                  |   DESCRIPCIÓN                                                        |
| --------------                           | ---------------------                                                |
| **Industrias**                           | Seleccione el sector dónde encaja mejor la aplicación. Si la aplicación se relaciona con varios sector industrial correspondiente, deje este campo en blanco.      |
| **Categorías**                           | Seleccione hasta tres categorías que se relacionan con la aplicación.     |
| **Vínculo de ayuda de la aplicación**               | Proporcione una dirección URL a una página que se ofrece ayuda en línea para la aplicación.           |
| **Productos con los que funciona su aplicación (máximo de 3)** | Seleccione el **New** signo más para crear un campo de texto. En el campo, escriba el nombre de un producto que su aplicación funciona con. Lista de productos hasta tres.       |
| **Palabras clave de búsqueda (máximo de 3)**              | Escriba las palabras clave hasta tres usuarios probablemente usará para buscar de la aplicación en AppSource. Por ejemplo, si la aplicación se denomina "Mi aplicación enviando por correo", las palabras clave podrían ser **correos electrónicos**, **correo**, y **aplicación de correo electrónico**. |
|  |  |


## <a name="marketing-artifacts-section"></a>Sección de artefactos de marketing

En el **artefactos de Marketing** sección, la personalización de marca de carga y materiales que se mostrará en AppSource de marketing.  En esta sección se divide en cuatro subsecciones: **Logotipos**, **vídeos**, **documentos** y **capturas de pantalla**. Los logotipos y capturas de pantalla son la única requiere artefactos de marketing. Pero para el recurso de cliente recomendado, se recomienda agregar vídeos y documentos también.

![Sección Artefactos de marketing en la pestaña Detalles del escaparate electrónico](./media/marketing-artifacts-section.png)

Para completar los campos de esta sección, utilice la siguiente tabla:
 
|    Campo                             |    DESCRIPCIÓN                                                    |
|   -----------                        |    -------------                                                  |
| *Logotipos*                              |                                                                   |
| **Logotipo de la oferta (formato .png, 48 x 48)**   | Cargue un logotipo que desea mostrar en la información general de la aplicación o en resultados de búsqueda de la aplicación. AppSource admite solo el formato PNG, con una resolución de 48 x 48 px.  |
| **Logotipo de la oferta (formato .png, 216 x 216)** | Cargue un logotipo que desea mostrar en la página de detalles de la aplicación.  AppSource admite solo el formato PNG, con una resolución de 216 x 216 px.  |
| *Vídeos*                             |                                                                   |
| **Nombre**                             | Escriba el nombre o título de la aplicación.                                          |
| **URL**                              | Escriba la dirección URL de un vídeo hospedado en YouTube o Vimeo.                              |
| **Miniatura**                        | Agregar una imagen en miniatura de la aplicación.  AppSource admite sólo formato PNG, con una resolución de 1280 x 720 píxeles.   |
| *Documentos*                          | Agregar hasta tres documentos que se mostrará en AppSource, en el **más** encabezado.  |
| **Nombre**                             | Escriba el nombre o título de un documento de soporte.                              |
| **Archivo**                             | Cargar un archivo PDF.                             |
| *Capturas de pantalla*                        | Agregue hasta cinco capturas de pantalla.                        |
| **Nombre**                             | Escriba un nombre o título de una captura de pantalla.                                       |
| **Imagen**                            | Cargar una captura de pantalla PNG. Su resolución debe ser 1280 x 720 píxeles.  | 
|   |   |

Los logotipos que se carga en [Cloud Partner Portal](https://cloudpartner.azure.com) debe *no*:

- Utilizar un degradado. La apariencia del logotipo debe ser sin formato.
- Incluir nombre de su compañía, nombre de la marca u otro texto. 
- Parecer estirada.

## <a name="legal-section"></a>Sección legal

En el **Legal** sección, proporcione los dos documentos legales necesarios para cada oferta: la directiva de privacidad y los términos de uso.

![Sección Legal en la pestaña Detalles del escaparate electrónico](./media/legal-section.png)

Para completar los campos de esta sección, utilice la siguiente tabla:

|   Campo                |   DESCRIPCIÓN                           |
|------------------------|--------------------------------------   |
| **URL de la política de privacidad** | Dirección URL de la directiva de privacidad registrado       |
| **Condiciones de uso**       | La directiva de uso, con formato de texto sin formato o HTML simple     |
|  |  |


## <a name="customer-support-section"></a>Sección Asistencia al cliente

En el **soporte al cliente** sección, proporcione el **dirección URL de soporte técnico** para la página de soporte técnico al cliente en línea.  Esta página debe proporcionar varias opciones de contacto, como teléfono, correo electrónico y chat en directo. 


## <a name="lead-management-section"></a>Sección de administración de clientes potenciales

En el **administración de clientes potenciales** sección, configurar el sistema para recopilar los clientes potenciales que generan las ofertas de AppSource. Para elegir las opciones de almacenamiento para los clientes potenciales, utilice la siguiente tabla:

|    Campo               |   Destino de clientes potenciales                               |
|------------------------|--------------------------------------            |
|  **None**              | No recopilar ninguna clientes potenciales. Esta es la opción predeterminada.  |
| **Azure Blob (en desuso)** | Especificar [almacenamiento de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) mediante un nombre de contenedor y una cadena de conexión.  Esta opción está en desuso. Use **Azure Table** en su lugar.  |
| **Tabla de Azure**        | Especificar [de Azure Table storage](https://docs.microsoft.com/azure/cosmos-db/table-storage-overview) mediante el uso de una cadena de conexión.  |
| **Dynamics CRM Online** | Especificar [Dynamics 365](https://dynamics.microsoft.com/) mediante el uso de una dirección URL y credenciales de autenticación. |
| **Punto de conexión HTTPS**     | Especifique el extremo HTTPS mediante el uso de una carga JSON.   |
| **Marketo**            | Especifique un [Marketo](https://www.marketo.com/) instancia mediante un servidor ID, Id. de munchkin e Id. de formulario.   |
| **Salesforce**         | Especificar [Salesforce](https://www.salesforce.com/) mediante el uso de un identificador de objeto. |
|  |  |

Una vez publicada la oferta, se valida la conexión de clientes potenciales y se envía de forma automática un cliente potencial de prueba al destino especificado. Administrar continuamente información de clientes potenciales y actualizar rápidamente la configuración para reflejar la arquitectura de administración de cliente actual.


## <a name="next-steps"></a>Pasos siguientes

En el [contactos](./cpp-contacts-tab.md) pestaña, proporcione técnicas y usuario recursos de soporte técnico de su oferta.
