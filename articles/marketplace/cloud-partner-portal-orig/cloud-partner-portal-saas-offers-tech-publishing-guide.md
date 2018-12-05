---
title: Guía de publicación técnica de aplicaciones SaaS | Microsoft Docs
description: Explica la publicación de ofertas de aplicaciones SaaS en el Marketplace adecuado.
services: Azure, Marketplace, Cloud Partner Portal,
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
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 7e648d6d3bd8d8ac14fb3f9723f4667c1cc33ac6
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620190"
---
<a name="saas-application-technical-publishing-guide"></a>Guía de publicación técnica de aplicaciones SaaS
===========================================

Bienvenido a la guía de publicación técnica de aplicaciones SaaS. Esta guía está diseñada para ayudar a los candidatos y a los anunciantes existentes a mostrar sus aplicaciones y servicios en AppSource o Azure Marketplace mediante la oferta de aplicaciones SaaS.

Para obtener información general de todas las demás ofertas de Marketplace, consulte la [Guía del anunciante de Marketplace](https://aka.ms/sellerguide).


<a name="what-are-pre-requisites-for-publishing-a-saas-app"></a>¿Cuáles son los requisitos previos para publicar una aplicación SaaS?
-------------------------------------------------

[Cloud Partner Portal](https://cloudpartner.azure.com) proporciona un acceso basado en rol al portal lo cual permite que varias personas colaboren en la publicación de una oferta. Para más información, consulte [Administración de usuarios](./cloud-partner-portal-manage-users.md). 

Antes de publicar una oferta en nombre de un publicador de cuenta, una persona con el rol *Propietario* tiene que aceptar los [Términos de uso](https://azure.microsoft.com/support/legal/website-terms-of-use/), la [Declaración de privacidad de Microsoft](https://www.microsoft.com/privacystatement/default.aspx) y el [Contrato del programa Microsoft Azure Certified](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).


<a name="creating-an-offer"></a>Creación de una oferta
-----------------

En esta sección se describe el proceso de creación de una nueva oferta de aplicación SaaS.

![Información general de la oferta de aplicación SaaS](media/cpp-creating-saas-offers/saas-offer-overview.png)

La oferta de aplicación SaaS consta de cinco secciones que se describen en la tabla siguiente:

| **Sección de oferta**  | **Descripción**                                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Configuración de oferta     | Ayuda a definir un nombre y un identificador únicos para la aplicación SaaS                                                                         |
| Información técnica     | Ayuda a configurar el tipo de solución SaaS y proporciona los detalles de conexión de la aplicación                            |
| Versión de prueba         | Sección opcional que le permite definir un servicio que permitirá a los clientes probar la oferta antes de adquirirla. |
| Detalles del escaparate | Contiene las secciones de marketing, legal, administración de clientes potenciales y de listas:   <br/> - La sección de marketing le permite escribir la descripción y los logotipos necesarios para que la oferta aparezca correctamente en el portal de usuarios de Marketplace.  <br/> -La sección de administración de clientes potenciales le permite definir un lugar al que redirigir los clientes potenciales del usuario final generados en el portal de usuarios finales de Azure Marketplace.  <br/> - La sección legal le permite especificar la documentación legal sobre la directiva de privacidad y los términos de uso.  |
| Contacto            | Le permite especificar la información de contacto de soporte técnico de la oferta.                                                                  |
|  |  |


### <a name="creating-a-new-offer"></a>Creación de una nueva oferta

Después de iniciar sesión en [Cloud Partner Portal](https://cloudpartner.azure.com), seleccione el elemento **Nueva oferta** en la barra de menús de la izquierda que muestra un menú con las ofertas disponibles. En la imagen siguiente se muestra un ejemplo de estas ofertas:

![Nueva oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer.png)

Seleccione la oferta que se le ha autorizado de la lista y se abrirá un nuevo formulario de oferta.

![Nuevo formulario de oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-2.png)

En la siguiente tabla se describen los campos de la oferta:

| **Campos de la oferta** | **Descripción**                                                                                            |
|------------------|----------------------------------------------------------------------------------------------------------- |
| Id. de oferta         | Identificador único de la oferta en un perfil del anunciante. Este identificador se mostrará en las direcciones URL de producto y en los informes de facturación. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. Tenga en cuenta que este campo queda bloqueado en cuanto se lanza una oferta. Por ejemplo, si un anunciante llamado Contoso publica una oferta con el identificador de oferta sample-vm, este aparecerá en Azure Marketplace como: [https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview](https://azuremarketplace.microsoft.com/) |
| Id. de publicador     | El identificador del anunciante es su identificador único en Marketplace. Se deben asociar todas las ofertas al identificador del anunciante. Este identificador no se puede modificar una vez que se haya guardado la oferta.                                                                                       |
| NOMBRE             | Se trata del nombre para mostrar de la oferta. Este es el nombre que se mostrará en Azure Marketplace y en Azure Portal. Puede tener un máximo de 50 caracteres. Aquí se recomienda incluir un nombre de marca que identifique el producto. No incluya aquí el nombre de su empresa a menos sea así como se comercializa. Si comercializa esta oferta en su propio sitio web, asegúrese de que el nombre sea exactamente el mismo que aparece en el sitio web.             |
|  |  |

Haga clic en **Guardar** para guardar lo que ha hecho hasta ahora. En la sección siguiente se describe la incorporación de planes a la oferta.


### <a name="technical-information"></a>Información técnica

La sección de información técnica le permite especificar la siguiente información:

![Información técnica de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-technical-info.png)

La decisión más importante es si va a disponer de una lista de SaaS o a tener el comercio habilitado. Si va a tener una lista de SaaS, debe seleccionar entre:

-   Gratis: proporcione una dirección URL de la aplicación SaaS en la que los clientes puedan obtener la aplicación.
-   Evaluación gratuita: proporcione una dirección URL de la aplicación SaaS en la que los clientes puedan ejecutar la prueba antes de adquirir la oferta.
-   Ponerse en contacto conmigo: solo es aplicable si tiene conectado un sistema de administración de clientes potenciales. Esta opción permite a los clientes pedir que se pongan en contacto con ellos y se comparte un cliente potencial con usted.

Si dispone de una aplicación SaaS que está en Azure Marketplace y desea habilitar el comercio mediante transacciones de Microsoft, seleccione **Sell through Azure** (Vender en Azure).  
Para más información sobre cómo conectar la aplicación SaaS, consulte [SaaS - Sell through Azure](./cloud-partner-portal-saas-offer-subscriptions.md) (SaaS: Vender en Azure).


### <a name="test-drive"></a>Versión de prueba

La creación de una experiencia de evaluación para los clientes es un procedimiento recomendado para garantizar que puedan comprar con confianza. De las opciones de prueba disponibles, Versión de prueba es la más eficaz a la hora de generar clientes potenciales de alta calidad y lograr un aumento en la conversión de estos.

Proporciona a los clientes una versión de prueba práctica y autodidacta con las principales características y ventajas del producto, y una demostración en un escenario de implementación del mundo real.

![Versión de prueba de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-test-drive.png)


#### <a name="how-a-test-drive-works"></a>Funcionamiento de una versión de prueba

Un cliente potencial busca y encuentra la aplicación en Marketplace. El cliente inicia sesión y acepta los términos de uso. En ese momento, el cliente recibe su entorno preconfigurado de prueba durante un número determinado de horas, mientras que usted recibe una notificación de cliente potencial altamente cualificado que le permite realizar un seguimiento del mismo.

![Versión de prueba 2 de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-test-drive-2.png)

No importa lo compleja que sea la aplicación, la versión de prueba de Microsoft le ayuda a realizar una demostración real del producto para el cliente. Hay tres tipos diferentes de versiones de prueba disponibles, cada uno basado en el tipo de producto, el escenario y el Marketplace en el que se encuentra.

-   **Azure Resource Manager**: una versión de prueba de Azure Resource Manager es una plantilla de implementación que contiene todos los recursos de Azure que componen una solución que está compilando el anunciante. Los productos que pertenecen a este tipo de versión de prueba son aquellos que solo usan recursos de Azure.
-   **Logic Apps**: una versión de prueba de Logic Apps es una plantilla de implementación que está diseñada para incluir todas las arquitecturas de soluciones complejas. Todas las aplicaciones o productos personalizados de Dynamics deberían usar este tipo de versión de prueba.
-   **Power BI**: una versión de prueba de Power BI consta de un vínculo insertado a un panel con una compilación personalizada. Aquellos productos que deseen realizar una demostración de un objeto visual interactivo de Power BI deben usar este tipo de versión de prueba. Todo lo que necesita para la carga es la dirección URL insertada de Power BI.

Los pasos de publicación principales para agregar una versión de prueba son:

1.  Definición del escenario de la versión de prueba
2.  Compilación o modificación de la plantilla de Azure Resource Manager
3.  Creación del manual detallado de la versión de prueba
4.  Nueva publicación de la oferta

Para más información, consulte [Versión de prueba](./what-is-test-drive.md).


### <a name="storefront-details"></a>Detalles del escaparate

Las aplicaciones SaaS necesitan que se proporcionen las dos primeras secciones de un resumen y una descripción sobre la aplicación.

![Detalles del escaparate de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-storefront-details.png)

En la siguiente tabla se describen los detalles del escaparate de la oferta:

| **Campos de la oferta**        | **Descripción**                                                                                          |
|-------------------------| ---------------------------------------------------------------------------------------------------------|
| Resumen de la oferta           | Resumen de la propuesta de valor de la oferta. Aparecerá en la página de búsqueda de la oferta. Debe tener un máximo de 100 caracteres.   |
| Descripción de la oferta       | Descripción que aparecerá en la página de detalles de la aplicación. Debe tener un máximo de 1300 caracteres *Nota* este campo acepta contenido HTML con etiquetas como &ltp\>, &lth1\>, &lth2\>, &ltli\>, etc., que le permiten hacer que el contenido sea mucho más presentable. El equipo del portal de publicación está trabajando para agregar una característica que permita obtener una versión preliminar de los detalles del escaparate para hacer iterativamente que el contenido sea más presentable. Mientras tanto, puede usar cualquier herramienta HTML como http://htmledit.squarefree.com para ver el aspecto de la descripción. |
| Industrias              | Seleccione los sectores más afines a la oferta. Si la aplicación está relacionada con varios sectores, puede seleccionar un máximo de dos. |
| Suggested Categories (Categorías sugeridas)    | Seleccione las categorías más afines a la oferta. Puede seleccionar un máximo de tres categorías.     |
| Versión de la aplicación     | Escriba el número de versión de la aplicación.                                                                |
| Palabras clave de búsqueda (3 como máximo) | Escriba un máximo de tres palabras clave de búsqueda que los clientes pueden usar para buscar la aplicación en el sitio web del escaparate de Marketplace. |
|  |  |


### <a name="marketing-artifacts"></a>Artefactos de marketing

La sección de artefactos de marketing le permite definir los recursos de marketing de Azure Marketplace como logotipos, vídeos, capturas de pantalla y documentos:

![Artefactos de marketing de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-marketing-artifacts.png)

En la siguiente tabla se describen los campos de marketing:

| **Campos de la oferta** | **Descripción**                                                                                                          |
|------------------| ------------------------------------------------------------------------------------------------------------------------ |
| Logotipos            | Si dispone de una aplicación SaaS que desea **vender en Azure**, debe proporcionar las imágenes del logotipo. Si es solo un anuncio, se necesitan únicamente 2 logotipos. Siga estas directrices para todos los logotipos cargados en [Cloud Partner Portal](https://cloudpartner.azure.com):     <br/> - Utilice pocos colores primarios y secundarios en el logotipo. El diseño de Azure tiene una paleta de colores simple.     <br/> - Evite emplear el blanco o el negro como color de fondo del logotipo. Los colores del tema de Azure Portal son el blanco y el negro. En su lugar, utilice algún color que haga destacar a los logotipos en Azure Portal. Nosotros recomendamos usar colores primarios simples. Si va a utilizar un fondo transparente, asegúrese de que el texto y el logotipo no sean de color azul, blanco o negro.     <br/> - No utilice un fondo degradado en el logotipo.     <br/> - Evite colocar texto, incluso el nombre de la empresa o de la marca, en el logotipo. El aspecto del logotipo debe ser "plano" y debe evitar degradados.    <br/> - El logotipo de la imagen no se debe extender.                   |
| Vídeos           | Le permite agregar vínculos de vídeos de la oferta. Puede usar vínculos a vídeos de YouTube o Vimeo para que acompañen a la oferta para los clientes. También deberá especificar una imagen en miniatura del vídeo, con una imagen png de 1280 x 720 píxeles. Puede incluir un máximo de cuatro vídeos por oferta. |
| Documentos        | Le permite agregar documentos de marketing a su oferta. Todos los documentos deben estar en formato PDF y puede tener un máximo de tres documentos por oferta.                                                                                                                                                      |
| Capturas de pantalla      | Le permite agregar capturas de pantalla de la oferta. Hay un máximo de cinco capturas de pantalla que se pueden agregar por oferta. El tamaño máximo de la imagen es 1280 x 720 píxeles.                                                                                                                                             |
| Vínculos útiles     | Le permite agregar direcciones URL externas a su oferta para ayudar a señalar diagramas de arquitectura u otros sitios web que puede que el cliente quiera ver.                                                                                                                                                              |
|  |  |

La siguiente imagen muestra cómo aparece la información en un resultado de búsqueda de Marketplace:

![Información de marketing del anunciante de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info.png)

En la siguiente imagen se muestra cómo aparece la oferta en Marketplace después de que un cliente haga clic en un pequeño icono de oferta:

![Información 2 de marketing del anunciante de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info-2.png)


### <a name="lead-management-and-legal-information"></a>Administración de clientes potenciales e información legal

La sección legal le permite establecer la documentación legal de la oferta.
Hay dos documentos legales que son obligatorios para cada oferta de aplicación SaaS: la directiva de privacidad y los términos de uso. Para obtener más información, consulte  
[Configuración de clientes potenciales](./cloud-partner-portal-get-customer-leads.md).

![Información legal de marketing de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-legal-info.png)

En la tabla siguiente se describen las propiedades de la sección legal:

| **Campos de la oferta**   | **Descripción**                                                                                 |
|--------------------| ----------------------------------------------------------------------------------------------- |
| Dirección URL de la directiva de privacidad | La dirección URL que lleva a la directiva de privacidad de su empresa.                                                       |
| Términos de uso       | Los términos de uso de la oferta. Escriba o copie y pegue los términos de uso aquí. Se permite el uso básico de HTML como, por ejemplo, que este campo admita contenido HTML con etiquetas como &ltp\>, &lth1\>, &lth2\>, &ltli\>, etc. *Importante*: es muy recomendable que revise y obtenga una versión preliminar del contenido HTML que ha creado en un explorador antes de enviar la oferta. |
|  |  |


### <a name="contact-information"></a>Información de contacto

En esta sección, escribirá los contactos de soporte técnico de su empresa que serán responsables del soporte técnico a los clientes que utilizan esta oferta.
Hay tres áreas principales: Contacto de ingeniería, Contacto de soporte técnico y Direcciones URL de soporte técnico:

![Información de contacto de marketing de la oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-contact-info.png)


| **Contacto**         | **Descripción**                                                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Contacto de ingeniería | Proporcione el nombre, correo electrónico y número de teléfono de un contacto de ingeniería con el que Microsoft se pueda poner en contacto para cualquier problema relacionado con el soporte técnico o el negocio. |
| Contacto de soporte técnico     | Proporcione el nombre, correo electrónico, número de teléfono y dirección URL de soporte técnico para que los clientes se puedan poner en contacto cuando tienen solicitudes de soporte técnico.                  |
|  |  |


Una vez que su oferta esté lista y presione Publicar, la oferta pasará al proceso de certificación. Se probará la aplicación SaaS mediante un proceso de comprobación manual que incluye la comprobación de la dirección URL si tiene un anuncio, o de los puntos de conexión si tiene la opción Sell through Azure (Vender en Azure) seleccionada. Durante esta aprobación manual también se decide el escaparate adecuado en el que debe aparecer la aplicación (Azure Marketplace, AppSource o ambos).

<a name="updating-the-offer"></a>Actualización de la oferta
------------------

Hay varios tipos de actualizaciones que puede que quiera realizar en su oferta una vez que esta se ha publicado y está activa. Cualquier cambio que realice en una nueva versión de la oferta se debe guardar y volver a publicar para que quede reflejado en Marketplace.

<a name="deleting-an-existing-offer"></a>Eliminación de una oferta existente
--------------------------

Puede decidir eliminar su oferta de Marketplace. La eliminación de ofertas garantiza que los clientes nuevos ya no puedan adquirir ni implementar la oferta pero, sin embargo, esto no afecta a los clientes ya existentes. La finalización de la oferta es el proceso de terminación del servicio o contrato de licencia entre usted y los clientes existentes.

Las guías y las directivas relacionadas con la eliminación y la terminación de ofertas se rigen por el contrato del anunciante de Microsoft Marketplace (consulte la sección 7) y las directivas de participación (consulte la sección 6.2). En esta sección se proporciona información relacionada con los diferentes escenarios de eliminación admitidos y los pasos que puede seguir en ellos.

### <a name="delete-the-live-offer"></a>Eliminación de una oferta activa

En el caso de una solicitud para quitar una oferta activa, son varios los aspectos que se deben tener en cuenta. Siga estos pasos si quiere recibir ayuda del equipo de soporte técnico para eliminar una oferta activa de Azure Marketplace:

1.  Genere una incidencia de soporte técnico mediante este vínculo
2.  En la lista Tipo de problema, seleccione **Managing offers** (Administrar ofertas), y en la lista Categoría, seleccione **Modificar una oferta o SKU ya en producción**.
3.  Envíe la solicitud.

El equipo de soporte técnico le guiará en el proceso de eliminación de la oferta.

> [!NOTE] 
> La eliminación de una oferta no afecta a las compras actuales de dicha oferta. Esas compras de clientes seguirán funcionando como antes.
Sin embargo, la oferta no estará disponible para nuevas compras una vez completada la eliminación.
