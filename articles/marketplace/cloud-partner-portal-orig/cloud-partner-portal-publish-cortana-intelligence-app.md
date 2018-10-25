---
title: Cortana Intelligence | Microsoft Docs
description: Publicar una oferta de Cortana Intelligence.
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: f12a15a0d739ae6e98be3871fa8bb4104f49565c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809049"
---
# <a name="publish-a-cortana-intelligence-offer-using-the-cloud-partner-portal"></a>Publicar una oferta de Cortana Intelligence mediante Cloud Partner Portal

En este artículo se describe cómo publicar una oferta de Cortana Intelligence mediante Cloud Partner Portal.

## <a name="prerequisites"></a>Requisitos previos

Cloud Partner Portal admite acceso basado en rol en el portal, lo que permite a los colaboradores contribuir en la publicación de una oferta. Para obtener más información, vea [Managing users on cloud partner portal](./cloud-partner-portal-manage-users.md) (Administrar usuarios en Cloud Partner Portal).

Antes de publicar una oferta en nombre de un publicador de cuenta, una persona con el rol \"Propietario\" tiene que aceptar los [Términos de uso](https://azure.microsoft.com/support/legal/website-terms-of-use/), la [Declaración de privacidad de Microsoft](http://www.microsoft.com/privacystatement/default.aspx) y el [Contrato del programa Microsoft Azure Certified](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="to-start-creating-a-cortana-inteligence-offer"></a>Para empezar a crear una oferta de Cortana Intelligence

Cuando cumpla todos los requisitos previos, ya puede empezar a crear la oferta de Cortana Intelligence.

1.  Inicie sesión en [Cloud Partner Portal](http://cloudpartner.azure.com/).
2.  En la barra de navegación de la izquierda, seleccione **+ Nueva oferta**.
3. Seleccione **Cortana Intelligence**.
4. Seleccione la pestaña **Editor** en la vista **Nueva oferta** para configurar las opciones siguientes:
    -   Configuración de oferta
    -   Información técnica
    -   Detalles del escaparate
    -   Contactos

    Cada una de las opciones anteriores muestran un formulario que deberá rellenar.  Los campos obligatorios de cada formulario están marcados con un asterisco rojo (\*).

## <a name="to-configure-offer-settings"></a>Para configurar las opciones de la oferta

La Configuración de la oferta proporciona información básica acerca de la oferta, por ejemplo, un Id. de oferta, un Id. de publicador y el nombre de la oferta.

### <a name="offer-id"></a>Id. de oferta

Se trata de un identificador único de la oferta en un perfil del publicador.
Este identificador es visible en las direcciones URL del producto. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. 
>[!Note]
>Este campo queda bloqueado en cuanto se lanza una oferta.

**Ejemplo:**

Si el publicador **contoso** crea una oferta con el identificador de oferta *sample-Cortana Intelligence*, se mostrará en AppSource como “https://appsource.microsoft.com/marketplace/apps/**contoso**.*sample-Cortana Intelligence*?tab=Overview”.

### <a name="publisher-id"></a>Id. de publicador

Una lista desplegable de perfiles de publicador. Úsela para elegir el perfil del publicador con el que desea publicar esta oferta.

>[!Note]
>Este campo queda bloqueado en cuanto se lanza una oferta.

### <a name="name"></a>NOMBRE

Nombre para mostrar de la oferta. Este nombre se muestra en [AppSource](https://appsource.microsoft.com). Puede tener un máximo de 50 caracteres.

Cuando termine de proporcionar la información necesaria en la configuración de la oferta, seleccione **Guardar** para pasar a la parte de la información técnica de la opción. de la oferta.

## <a name="to-configure-technical-info"></a>Para configurar la información técnica

Use esta vista para ofrecer información técnica que se mostrará en la página de la oferta. Los siguientes campos se aplican a esta vista.

### <a name="partner-mpn-id"></a>Id. de MPN del asociado

Si es un asociado de Microsoft registrado, vaya al [sitio web de asociados](https://partners.microsoft.com/) e inicie sesión para obtener el identificador de asociado de la organización. Escriba ese identificador para el **Id. de MPN de asociado**.

### <a name="analytics-components-used"></a>Componentes de análisis usados

Seleccione todos los componentes que usa su oferta. Seleccione al menos 1 componente. Seleccione **Microsoft R** solo si usa cualquiera de las licencias de MRS de los servicios SQL 2016 R, HDInsight Premium o MRS que se ejecuta en máquinas virtuales.

### <a name="additional-components-used"></a>Componentes adicionales que se usan

Seleccione todos los demás componentes que usa la solución.

### <a name="customer-name-using-solution"></a>Nombre del cliente que usa la solución

Proporcione el nombre del cliente que está usando esta solución en producción. 

>[!Note]
>Esta información no se publica en AppSource. Solo se utiliza para la evaluación de la solución.

### <a name="azure-consumption-requirement-met"></a>¿Se cumple el requisito de consumo de Azure?

Indicar si la solución genera al menos 1000 € al mes por cada cliente de los componentes del conjunto de aplicaciones de Azure o si la solución utiliza Microsoft R.

>[!Note]
>Esta información no se publica en AppSource. Solo se utiliza para la evaluación de la solución.

### <a name="demo-video-url"></a>Dirección URL del vídeo de demostración

Proporcione una dirección URL para ver un vídeo de demostración para la solución o aplicación que su equipo de ventas previas puede mostrar a los clientes. 

>[!Note]
>Esta información no se publica en AppSource. Solo se utiliza para la evaluación de la solución.

#### <a name="demo-video-guidelines"></a>Directrices de vídeo de demostración

- La duración del vídeo debe ser inferior a 15 minutos.
- El vídeo debe ser una grabación de la funcionalidad de la solución con edición mínima. 
- El vídeo resalta los aspectos clave de la funcionalidad de cara al usuario y se centra en la integración de análisis avanzado. 
- Los vídeos de demostración **no** estarán disponibles públicamente para los clientes, pero se espera que sean representativos de cómo se *presentaría* la solución a un cliente potencial. Por tanto, deben estar cifrados y deben ser repetibles.
- Use cualquier herramienta de grabación de pantalla que exporte un formato de vídeo estándar (por ejemplo, MPEG) para crear el vídeo. 

Las siguientes instrucciones muestran cómo un vídeo usando Skype Empresarial. 

1. [Comenzar una reunión](https://support.office.com/article/Start-a-Skype-for-Business-conference-call-8dc8ac52-91ac-4db9-8672-11551fdaf997)
2. [Compartir el escritorio](https://support.office.com/article/Share-your-screen-in-Skype-for-Business-2d436dc9-d092-4ef1-83f1-dd9f7a7cd3fc)
3. [Empezar a grabar](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232)
4. Después de detener la grabación, [use el Administrador de grabaciones para publicarla](https://support.office.com/article/Recording-Manager-save-and-publish-59a3beb7-c700-40cf-ab21-bc82a2b06351).

Cargue el vídeo grabado en un servicio que permita generar una dirección URL compartida. Por ejemplo, un [vínculo de invitado de OneDrive o SharePoint](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232).

### <a name="supported-regions"></a>Regiones admitidas

Seleccione todas las regiones que admite la solución. Seleccione al menos 1 región.

### <a name="power-bi-desktop-file-pbix"></a>Archivo de escritorio de Power BI (.pbix)

Si procede, cargue un archivo .pbix. Asegúrese de que se importan datos en el archivo a los que no se hace referencia externamente. Crearemos el informe insertado por usted.

### <a name="solution-architecture"></a>Arquitectura de la solución

Cargue un documento que detalle la arquitectura de la solución. Si desea obtener instrucciones para cargar el diagrama de la arquitectura de la solución, vea [Advanced Analytics Solution Workflow Template](https://partnersprofilesint.blob.core.windows.net/partner-assets/documents/AppSource%20Solution%20Publishing%20Guide%20Docs/Advanced%20Analytics%20Solution%20Workflow%20Template%20V.2017.3.23.pptx) (Plantilla de flujo de trabajo de soluciones de análisis avanzados).

>[!Note]
>Esta información no se publica en AppSource. Solo se utiliza para la evaluación de la solución.

### <a name="select-segments"></a>Seleccionar segmentos

Seleccione todos los segmentos del sector relacionados. Si la aplicación es para cualquier segmento que no aparece en la lista, escriba el nombre del segmento en el campo **Otros**. Pude usar tres palabras para el segmento.

### <a name="select-business-processes"></a>Seleccionar procesos empresariales

Seleccione los procesos empresariales que mejor describen la solución. Si la aplicación es para cualquier proceso que no aparece en la lista, escriba el nombre del proceso en el campo **Otros**. Pude usar tres palabras para el proceso.

### <a name="trial-info"></a>Información de versión de prueba

-   **Dirección URL de evaluación de SaaS:** escriba la dirección URL de la experiencia de evaluación de la aplicación.
-   **Dirección URL de versión limitada de prueba de SaaS:** escriba la dirección URL de la experiencia de versión limitada de prueba de la aplicación.

Para obtener más información acerca de las pruebas, vea **Tipo de aplicación** en la siguiente sección de este artículo.

Cuando termine de proporcionar la información necesaria en Información técnica, seleccione **Guardar** para pasar a la parte de la información del escaparate de la oferta. 

## <a name="to-configure-storefront-details"></a>Para configurar los detalles del escaparate

### <a name="offer-summary"></a>Resumen de la oferta

Este es un resumen de la propuesta de valor de la oferta. Aparecerá en la página de búsqueda de la oferta. La longitud máxima del resumen es de 100 caracteres.

### <a name="offer-description"></a>Descripción de la oferta

Esta es la descripción que aparecerá en la página de detalles de la aplicación.
La longitud máxima de la descripción es de 1300 caracteres.

Tenga en cuenta que este campo admite contenido html con etiquetas como \<p\>, \<h1\>, \<h2\>, \<li\>, etc., que le permite hacer que el contenido sea mucho más presentable. El equipo del portal de publicación está trabajando para agregar una característica que permita obtener una vista previa de los detalles del escaparate para hacer iterativamente que el contenido sea más presentable. Mientras tanto, puede usar cualquier herramienta html como [http://htmledit.squarefree.com](http://htmledit.squarefree.com/) para ver el aspecto de la descripción.

El formato sugerido para la descripción es dividir el texto en subsecciones en función de las propuestas de valor, cada una de ellas resaltada con un subtítulo. Los visitantes suelen echar un vistazo al campo del "resumen de la oferta" y a los subtítulos para hacerse una idea de lo que aborda la aplicación y por qué deben considerar la aplicación con tan solo una vista rápida. Por tanto, es importante que la atención del usuario sea un motivo para continuar leyendo a fin de conocer los detalles.

#### <a name="partner-examples"></a>Ejemplos de asociado

- [Neal Analytics Inventory Optimization](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Personalización comercial de Plexure](https://appsource.microsoft.com/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint Citizen Services](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

### <a name="industries"></a>Industrias

Seleccione el sector más afín a la aplicación. Si la aplicación está relacionada con varios sectores, puede dejar este campo en blanco.

### <a name="categories"></a>Categorías

Seleccione las categorías que son relevantes para la aplicación. Seleccione un máximo de dos categorías.

### <a name="app-type"></a>Tipo de aplicación

Seleccione el tipo de prueba que admitirá la aplicación en AppSource. Elija una de las siguientes pruebas:
- **Gratuita** significa que la aplicación es gratuita.
- **Versión de prueba** significa que los clientes pueden probarla durante un período especificado.
- **Solicitud de versión de prueba** significa que los clientes pueden solicitar una versión de prueba de la aplicación.

Los asociados pueden ofrecer dos tipos de experiencias de prueba en AppSource.

- El opción **evaluación**, también denominada **versiones de evaluación dirigida al cliente (CLT)** puede ser de uno de los siguientes tipos: 
    - Versión de evaluación de SaaS
        - El cliente puede navegar a una dirección URL que proporcione usted o su asociado. El cliente pasa por un inicio de sesión único de AAD federada para tener un tiempo de experiencia de evaluación limitada.
        - Se trata de una aplicación SaaS con multiinquilino que aísla la configuración y datos de un usuario del resto de usuarios. O bien, esta experiencia solo proporciona un subconjunto que utiliza las operaciones de solo lectura.

        **Ejemplos:**

        - [Ejecución de AFS POP Retail](https://appsource.microsoft.com/product/web-apps/afs.fa9fc926-3bc3-43dd-becd-3ef41b52c10b?tab=Overview)
        - [AvePoint Citizen Services](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview). (Esta aplicación proporciona una experiencia exclusiva con trayectorias claras para un conjunto seleccionado de roles de usuario.)

     - Versión de prueba
        - Su solución (o la de su asociado), o un subconjunto de ella, se puede empaquetar mediante plantillas de Azure Resource Manager de las que AppSource puede crear una instancia y AppSource puede administrar la aplicación en una suscripción de asociado con conversión boxing del tiempo y mantenimiento de grupos activos o inactivos de instancias, etc.
        - Se pueden proporcionar plantillas y código de ejemplo para ayudarle si opta por esta opción.

        **Ejemplos:**

        - [Neal Analytics Inventory Optimization](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome)

- La opción **Solicitud de versión de prueba** (**Versiones de prueba gestionadas por el asociado / PLT**) requiere que los clientes rellenen un formulario de información de contacto para el seguimiento del asociado. Después el asociado ofrece una demostración o una versión de prueba de la aplicación. Para obtener más información, vea el vídeo sobre el [AppSource trial experience walk through](http://aka.ms/trialexperienceforwebapps) (Tutorial de la experiencia de prueba en AppSource) para obtener información general de alto nivel.

>[!Note]
>Los datos muestran que las **pruebas gestionadas por el cliente** tienen una mayor generación de clientes potenciales que las **pruebas gestionadas por los asociados**.


### <a name="help-link-for-your-app"></a>Vínculo de ayuda de la aplicación

Proporcione una dirección URL a una página que tiene información de ayuda para la aplicación.

### <a name="supported-countriesregions"></a>Países y regiones admitidos

En este campo se determinan las regiones en las que estará disponible la versión de prueba.

![Países y regiones admitidos](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot3.png)

### <a name="supported-languages"></a>Idiomas admitidos

Seleccione los idiomas que admite la aplicación. Si la aplicación es compatible con idiomas que no están en esta lista, publique su oferta y envíenos un correo electrónico a <appsource@microsoft.com> para hacérnoslo saber.

### <a name="app-version"></a>Versión de la aplicación

Escriba el número de versión de la aplicación.

### <a name="products-your-app-works-with"></a>Productos con los que funciona su aplicación

Indique los productos específicos con los que funciona la aplicación. Puede enumerar tres productos como máximo. Para indicar un producto, seleccione el **signo más** (junto a Nuevo) y se creará un campo de texto abierto. Escriba el nombre de un producto con el que funciona su aplicación.

### <a name="hide-key"></a>Ocultar la clave

Esta es una clave combinada con la URL de vista previa de la oferta para ocultar la oferta de la vista pública. No es una contraseña. Puede especificar cualquier cadena alfanumérica.

### <a name="offer-logo-small"></a>Logotipo de la oferta (pequeño)

Este logotipo aparecerá en la página de búsqueda de la aplicación. El formato de imagen png es el único formato que se permite. El tamaño de la imagen es de 48 x 48 píxeles.

### <a name="offer-logo-large"></a>Logotipo de la oferta (grande)

Este logotipo aparecerá en la página de información de la aplicación. El formato de imagen png es el único formato que se permite. El tamaño de la imagen es de 48 x 48 píxeles.

### <a name="video"></a>Vídeo

Puede cargar un máximo de cuatro vídeos. Para cada vídeo que desee cargar:
- Proporcione el nombre del vídeo
- Proporcione una dirección URL (solo YouTube o Vimeo)
- Proporcione una miniatura para asociar con el vídeo. La miniatura debe usar el formato de imagen png y su tamaño debe ser de 1280 X 720 píxeles. 

Para agregar nuevos videos, seleccione **+ Nuevo**, como se muestra en la siguiente captura de pantalla.

![Agregar nuevo vídeo](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot4.png)

### <a name="document"></a>Documento

Puede cargar un máximo de tres documentos. Cada documento debe usar el formato de archivo PDF. Para agregar un nuevo documento:

- Seleccione **+ Nuevo**
- Escriba un nombre de documento
- Seleccione **Cargar** para cargar el documento.

![Agregar un nuevo documento](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot5.png)

### <a name="privacy-policy"></a>Directiva de privacidad

Escriba la dirección URL de la directiva de privacidad de la aplicación

### <a name="terms-of-use"></a>Términos de uso

Escriba los términos de uso de la aplicación. Los clientes de AppSource deben aceptar estos términos antes de poder probar la aplicación.

>[!Note]
>Este campo acepta contenido con etiquetas HTML como el contenido de html < p\>, < h1\> y < li\>. Puede usar estas etiquetas para dar formato al contenido. 

### <a name="lead-destination"></a>Destino de clientes potenciales

Seleccione un sistema donde se almacenarán los clientes potenciales. 

Seleccione **Tabla de Azure** si usa uno de los siguientes sistemas de CRM: Salesforce, Marketo o Microsoft Dynamics CRM. 

Para obtener más información sobre el sistema CRM que desea usar, seleccione uno de los siguientes vínculos para los sistemas compatibles.

-   [Tabla de Azure](./cloud-partner-portal-lead-management-instructions-azure-table.md)
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
-   [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
-   [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)
