---
title: Servicios de consultoría | Microsoft Docs
description: Guía para definir y publicar una oferta de servicio de consultoría.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: b3f6b9166afce9ca0cdeaa1c9dd6e10a5a976453
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807963"
---
<a name="defining-and-publishing-your-consulting-services-offer"></a>Definición y publicación de una oferta de servicio de consultoría
======================================================

Esta guía está diseñada para ayudarle a definir y publicar ofertas de servicios de consultoría en Cloud Partner Portal.

<a name="define-your-consulting-services-offer"></a>Definición de ofertas de servicios de consultoría
-------------------------------------

Definición del paquete de ofertas de servicios de consultoría Céntrese en ofertas de ámbito fijo, duración fija, precio estimado fijo (o gratis) y principalmente orientadas a la preventa para un único cliente. Seleccione paquetes de compromisos reiterativos que han demostrado ser populares y eficaces a la hora de impulsar el negocio.

<a name="publish-a-consulting-service-offer"></a>Publicación de una oferta de servicios de consultoría
----------------------------------

En las secciones siguientes se describe el proceso de publicación de una oferta de servicios de consultoría

1.  Creación de una nueva oferta
2.  Definición de la configuración de oferta
3.  Especificación de los detalles del escaparate y publicación en Azure Marketplace o en AppSource
4.  Publicación de la oferta

### <a name="create-a-new-offer"></a>Creación de una nueva oferta

Para crear una nueva oferta, complete los siguientes pasos:

1.  En el menú principal de Cloud Partner Portal, seleccione **Nueva oferta**.
2.  En este menú, seleccione **Servicios de consultoría**.

    ![Creación de una nueva oferta de servicios de consultoría](media/consulting-services-publishing-offer/cppselectnewconsultingoffer.png)

### <a name="define-offer-settings"></a>Definición de la configuración de oferta

En la pantalla Nueva oferta, el primer paso es crear la identidad de la oferta.
La identidad de la oferta consta de tres partes:

1.  Id. de oferta
2.  Id. de publicador
3.  NOMBRE

Cada una de estas partes se explica en las secciones siguientes.

#### <a name="offer-id"></a>Id. de oferta

Se trata de un nombre único que se crea cuando envía por primera vez una oferta. Debe constar únicamente de caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. El identificador será visible en la dirección URL y afecta a los resultados del motor de búsqueda. Por ejemplo, *nombredesuempresa\_serviciodeejemplo*

Como se puede ver en el ejemplo, el identificador de la oferta se anexa al identificador del anunciante para crear un identificador único. Esto se expone como un vínculo permanente que los motores de búsqueda pueden reservar e indexar.

**Una vez que una oferta está activa, no se puede actualizar**

#### <a name="publisher-id"></a>Id. de publicador

Este está relacionado con su cuenta. Cuando inicia sesión con su cuenta de la organización, el identificador de publicador aparece en el menú desplegable.

#### <a name="name"></a>NOMBRE

Este es el nombre que se mostrará como nombre de la oferta en AppSource o Azure Marketplace.

**Importante:** escriba solo el nombre del servicio real aquí. No incluya la duración y el tipo de servicio.

El ejemplo siguiente de Edgewater Fullscope muestra cómo se compone el nombre de la oferta. El nombre de la oferta aparece de esta forma:

![Creación de una nueva oferta de servicios de consultoría](media/consulting-services-publishing-offer/cppsampleconsultingoffer.png)

El nombre de la oferta consta de cuatro partes:

-   **Duración:** puede definirla en la pestaña Detalles del escaparate del editor. La duración se puede expresar en horas, días o semanas.
-   **Tipo de servicio:** puede definirlo en la pestaña Detalles del escaparate del editor. Los tipos de servicios son: evaluación, sesión informativa, implementación, prueba de concepto y taller.
-   **Preposición:** la inserta el revisor
-   **Nombre:** esto es lo que se define en la página Configuración de oferta.

En la lista siguiente se proporcionan varios nombres de oferta adecuados:

-   Fundamentos de los servicios profesionales: Sesión informativa de 1 hora
-   Plataforma de migración a nube: Sesión informativa de 1 hora
-   PowerApps y Microsoft Flow: Taller de 1 día
-   Azure Machine Learning Services: Prueba de concepto de 3 semanas
-   Solución de venta para tiendas físicas y virtuales: Sesión informativa de 1 hora
-   Aporte sus propios datos: Taller de 1 semana
-   Análisis en la nube: Taller de 3 días
-   Aprendizaje de Power BI: Taller de 3 días
-   Solución de administración de ventas: Implementación de 1 semana
-   Guía de inicio rápido de CRM: Taller de 1 día
-   Dynamics 365 for Sales: Evaluación de 2 días

Después de haber completado la información de la pestaña **Configuración de oferta**, puede guardar su envío. El nombre de la oferta aparece ahora encima del editor y puede encontrarlo de nuevo en Todas las ofertas.

### <a name="enter-storefront-details"></a>Especificación de los detalles del escaparate

A continuación, tiene que especificar los detalles del escaparate. Los detalles del escaparate constan de las siguientes secciones:

-   Detalles de la oferta
-   Información del anunciante
-   Detalles del anuncio
-   Artefactos de marketing

#### <a name="offer-details"></a>Detalles de la oferta

La sección de detalles de la oferta contiene los siguientes campos:

-   Resumen de la oferta
-   Descripción de la oferta

##### <a name="offer-summary"></a>Resumen de la oferta

El resumen de la oferta es una breve descripción de la oferta que aparece justo debajo del nombre de esta. Use texto sin formato al escribir el resumen de la oferta y no debe tener saltos de línea. Los siguientes son ejemplos de resúmenes correctos de ofertas junto con sus correspondientes nombres:

*Ejemplo 1*

-   **Nombre de la oferta:** Análisis en la nube: Taller de 3 días
-   **Resumen de la oferta:** Información general de Microsoft Azure y Power BI, evaluación del entorno actual y pequeña prueba de concepto.

*Ejemplo 2*

-   **Nombre de la oferta:** Azure IoT industrial: Prueba de concepto de 30 días
-   **Resumen de la oferta:** Creación de un producto piloto conectado industrial para la conexión segura de equipos de campo a una solución de Azure IoT Hub con paneles, informes y notificaciones.

*Ejemplo 3*

-   **Nombre de la oferta:** Servicios profesionales: Sesión informativa de 1 hora
-   **Resumen de la oferta:** Introducción y demostración de una solución preconfigurada y ampliada de Dynamics 365 for Operations que proporciona una administración mejorada de proyectos, facturación y recursos para servicios profesionales.

*Ejemplo 4*

-   **Nombre de la oferta:** Power BI en acción: Taller de 4 horas
-   **Resumen de la oferta:** Póngase manos a la obra y ejecute su primer panel, y conozca los procedimientos recomendados. Para un máximo de 12 alumnos, se realiza in situ.

*Ejemplo 5*

-   **Nombre de la oferta:** Dynamics y los proyectos: Evaluación de 3 días
-   **Resumen de la oferta:** Recopilación y evaluación de requisitos para una solución de ERP diseñada para empresas de servicios profesionales y empresas controladas por proyectos.

##### <a name="offer-description"></a>Descripción de la oferta

Esta es una descripción de la oferta de servicios de consultoría. Una buena descripción de oferta abarca los detalles exactos del grado de involucración y qué será lo que se entrega al final al cliente. Tiene que ayudar al cliente claramente a comprender lo que obtendrán.

No incluya vínculos de correo electrónico ni números de teléfono de contacto en la descripción de la oferta. Habrá un botón "Ponerse en contacto conmigo" con la oferta que cargará los clientes potenciales en el destino de administración de clientes potenciales que identifique en la oferta.

Debe proporcionar la descripción de la oferta en formato de Markdown. Si no está familiarizado con Markdown o con los formatos de HTML, puede revisar los recursos del [sitio de documentación de Microsoft](https://docs.microsoft.com/contribute/how-to-write-use-markdown).

El uso de estos formatos garantiza que su oferta tendrá la máxima legibilidad para los clientes.

Haga que la descripción de la oferta sea breve y adáptese al límite de caracteres, ya que a los usuarios no les gusta leer mucho texto. Tendrá la opción de cargar folletos de marketing, hojas informativas y otros documentos que describen la oferta con más detalle.

En el ejemplo siguiente se muestra una descripción de la oferta correctamente compuesta y su nombre y resumen correspondientes:

**Nombre de la oferta:** Análisis en la nube: Taller de 3 días

**Resumen de la oferta:** Información general de Microsoft Azure y Power BI, evaluación del entorno actual y pequeña prueba de concepto.

**Descripción de la oferta:** este taller de 3 días está destinado a directivos técnicos y empresariales, y se realiza in situ en las oficinas del cliente.

***Agenda***

Día 1

-   Se centra en la protección, escalado y organización de los datos en la nube de Microsoft con Azure Data Lake, HDInsight o Azure SQL Data Warehouse.

Día 2

-   Describe la configuración e implementación de soluciones avanzadas de análisis con Microsoft R y Azure Machine Learning.

Día 3

-   Explica la extracción de información procesable y la operacionalización del análisis con Power BI, e incluye una sesión colaborativa para crear de forma conjunta un panel de Power BI.

Al final del taller, el cliente podrá definir un plan de alto nivel y un plan de desarrollo de soluciones de datos y de análisis en la nube de Microsoft.

*Archivo de ejemplo de Markdown de una oferta que sigue este formato:*

    This 3-day workshop is for technical and business leaders and is held on-site at the client’s facility.

      ### Agenda

      **Day 1**

      * Focuses on how to secure, scale, and organize data within the Microsoft cloud using Azure Data Lake, HDInsight, or Azure SQL Data Warehouse

      **Day 2**

      * Covers how to configure and deploy advanced analytics solutions with Microsoft R and Azure Machine Learning

      **Day 3**

      * Covers how to draw actionable insights and operationalize analytics with Power BI, including a collaborative session to co-build a Power BI dashboard.


      ### Deliverables
      By the end of the workshop, the client will be able to define a high-level plan and an implementation roadmap for data and analytics solutions in the Microsoft cloud.


#### <a name="publisher-information"></a>**Información del anunciante**

**Identificador de MPN**

Identificador de 9 dígitos de Microsoft Partner Network (MPN). Si no dispone de un identificador de MPN, puede ir al Centro de partners de Microsoft para adquirirlo.

**Identificador del centro de partners**

Nuevo identificador del centro de partners en caso de que ya disponga de uno.

**Identificador de MPN**

Escriba una clave secreta para obtener una versión preliminar de la oferta en AppSource antes de publicarla.
Esta clave no es una contraseña.

#### <a name="listing-details"></a>Detalles del anuncio

**Tipo de servicio de consultoría**

Microsoft se centra exclusivamente en ofertas de servicios de consultoría de ámbito fijo, duración fija, precio estimado o fijo (o gratis), principalmente orientadas a la preventa para un único cliente, y en ofertas de evaluación, implementación, prueba de concepto, sesiones informativas o talleres que se realizan in situ o de manera virtual (las sesiones informativas se deben realizar in situ).

Se incluyen los siguientes cinco tipos de ofertas:

-   **Evaluación**: una evaluación del entorno del cliente para determinar la aplicabilidad de una solución y proporcionar una estimación del costo y el tiempo.
-   **Sesión informativa**: introducción a una solución o un servicio de consultoría para determinar el interés del cliente mediante plataformas, demostraciones y ejemplos de clientes. Las sesiones informativas se deben realizar in situ.
-   **Implementación:** una instalación completa que da como resultado una solución que funciona completamente. Para este programa piloto, Microsoft recomienda limitarse a las soluciones que se pueden implementar en una semana o menos.
-   **Prueba de concepto:** una implementación de ámbito limitado para determinar si una solución cumplirá los requisitos del cliente.
-   **Taller:** una participación interactiva realizada en las instalaciones de un cliente que puede incluir entrenamiento, sesiones informativas, evaluaciones o demostraciones basadas en el entorno o los datos del cliente.

**Disponibilidad de país/región**

Seleccione el país y región en la que estará disponible esta oferta de servicios de consultoría. No se puede publicar una oferta única en varios países o regiones. Se debe crear una oferta nueva para cada país o región.

**Industrias**

Seleccione los sectores en los que el servicio de consultoría es más aplicable.

**Duration**

Seleccione un número (por ejemplo, 3, 4, etc.) en Duración y, a continuación, seleccione Horas, Días o Semanas.

**Productos principales**

Para publicar en Azure Marketplace, seleccione **Azure** como el producto principal y, a continuación, seleccione las áreas de solución correspondientes.

Para publicar en AppSource, seleccione **Dynamics 365**, **Power BI** o **PowerApps** como el producto principal. También puede seleccionar otros productos aplicables que sean pertinentes, y la oferta del servicio de consultoría se mostrará en los anuncios que están asociados con cada uno de estos productos en AppSource.

**Competencias pertinentes**

Seleccione las competencias que sean pertinentes para esta oferta para hacer que aparezcan junto con los detalles de la oferta.

#### <a name="marketing-artifacts"></a>Artefactos de marketing

**Logotipo de empresa (formato .png, 48 x 48 píxeles)**

Cargue una imagen que va a aparecer en el icono de la oferta en la página de visualización de la galería de ofertas. La imagen debe ser una imagen .png con una resolución de 48 x 48 píxeles.

**Logotipo de empresa (formato .png, 216 x 216 píxeles)**

Cargue una imagen y aparecerá en la página de detalles de la oferta. La imagen debe ser una imagen .png con una resolución de 216 x 216 píxeles.

**Vídeos (4 como máximo)**

Cargue un máximo de cuatro casos prácticos de clientes o vídeos de referencia para clientes. Si no tiene ninguno, cargue un vídeo que explique la experiencia de su compañía en relación con la oferta. Si tiene una presentación de la solución en Power BI o PowerApps, cargue el vídeo de presentación aquí. Los vínculos de los vídeos deben ser para YouTube o Vimeo.

**Documentos (3 como máximo)**

Cargue el folleto de marketing que describe detalladamente la oferta de servicios de consultoría. También puede cargar información general de la empresa, hojas informativas o casos prácticos.

**Capturas de pantalla (5 como máximo)**

Cargue un máximo de cinco imágenes que proporcionen más información sobre la oferta, la entrega o la empresa. Por ejemplo, un fragmento de un folleto de marketing, una diapositiva adecuada de una presentación o una imagen que muestre los conocimientos de la empresa o su experiencia.

### <a name="publish-your-offer"></a>Publicación de la oferta

Después de completar la configuración de la oferta, los detalles del escaparate y los contactos, seleccione **Publicar** y proporcione una dirección de correo electrónico. Cuando Microsoft esté listo para publicar su oferta, recibirá un correo electrónico para obtener una versión preliminar antes de su publicación. Puede volver al portal para comprobar el estado de la oferta en cualquier momento.

Las ofertas pueden aparecer con el estado "Publicación cancelada" o "No se pudo publicar" durante el proceso de publicación. Esta es una parte normal del proceso y permite a Microsoft realizar modificaciones en la oferta. Si ve que el estado de la oferta es "Publicación cancelada", déjela en ese estado.
