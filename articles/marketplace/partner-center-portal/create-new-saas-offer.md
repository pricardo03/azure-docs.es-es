---
title: Creación de una oferta de SaaS en Marketplace comercial
description: Cómo crear una oferta de software como servicio (SaaS) para mostrarla o venderla en Azure Marketplace, AppSource, o bien a través del programa Proveedor de soluciones en la nube (CSP) mediante el portal de Marketplace comercial de Microsoft Partner Center.
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: dc086bc1252c084b717807213b5ba4c7f9d7bb97
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514056"
---
# <a name="create-a-new-saas-offer"></a>Creación de una oferta de SaaS

Para empezar a crear ofertas de software como servicio (SaaS), asegúrese de que primero [crea una cuenta del Centro de partners](./create-account.md) y después abre el [panel de Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), con la pestaña **Información general** seleccionada.

![Panel de Marketplace comercial en el Centro de partners](./media/new-offer-overview.png)

Seleccione el botón + **Crear...** y luego seleccione el elemento de menú **Software como servicio**. 

Si selecciona uno de los otros tipos de oferta, se le redirigirá a la versión anterior de [Cloud Partner Portal](https://cloudpartner.azure.com/).  En este momento, en el portal de Marketplace comercial del Centro de partners solo hay disponibles ofertas de SaaS. 

![Crear ventana de oferta en el Centro de partners](./media/new-offer-click.png)

Se muestra el cuadro de diálogo **Nueva oferta**. 

![Cuadro de diálogo Nueva oferta](./media/new-offer-popup.png)


## <a name="offer-id-and-name"></a>Nombre e identificador de oferta

- **Id. de oferta**: Cree un identificador único para cada oferta de su cuenta. Los clientes lo verán en la dirección URL de la oferta de Marketplace y en las plantillas de Azure Resource Manager (si procede). El identificador de la oferta debe ser alfanumérico y contener solo minúsculas (puede incluir guiones y caracteres de subrayado, pero no espacios en blanco). Tiene un límite de 50 caracteres y no se podrá actualizar después de seleccionar Crear.  
Ejemplo: test-offer-1
<br>Lo que resulta en la siguiente dirección URL: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Nombre de la oferta**: El nombre oficial de la oferta de la aplicación SaaS. Se usa el mismo en todas las publicaciones, anuncios y sitios web.  Este nombre puede ser una marca comercial.  El nombre de la oferta no debe contener espacios en blanco, emojis (a menos que sean una marca comercial o el símbolo de copyright) y no puede tener más de 50 caracteres.
<br>Ejemplo: Test Offer 1&#8482;

Seleccione **Crear**.  Se crea la página **Información general de la oferta** para esta oferta.  

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

## <a name="offer-overview"></a>Información general de la oferta

La página **Información general de la oferta** incluye: 

- **Estado de publicación** muestra una representación visual de los pasos necesarios para publicar esta oferta y el tiempo se tardará en completarse cada uno de ellos. Los iconos de los pasos de publicación que no se hayan completados aparecerán atenuados. 

- El menú **Información general de la oferta** contiene una lista de vínculos para realizar operaciones en esta oferta. Esta lista de operaciones cambiará en función de la selección que realice para la oferta.  
    - Si la oferta es un borrador: Eliminar borrador 
    - Si la oferta está activa: Detener venta de oferta 
    - Si la oferta está en vista previa: Transmitir 
    - Si no ha completado el cierre de sesión del anunciante: Cancelar publicación

## <a name="offer-setup"></a>Configuración de la oferta

En la pestaña **Configuración de la oferta** se solicita la siguiente información. Después de completar estos campos seleccione **Guardar**.

- **¿Desea vender a través de Microsoft?** (Sí/No)
    - **Sí**, desea vender su oferta a través de Microsoft y que Microsoft hospede las transacciones de Marketplace en su nombre; o bien 
    - **No**, solo desea que su oferta aparezca en Marketplace, pero que el procesamiento de las transacciones monetarias se realice de forma independiente de Microsoft.    

### <a name="sell-through-microsoft"></a>Venta mediante Microsoft

La venta a través de Microsoft mejora los procesos de detección y captación de clientes, permite que Microsoft hospede transacciones de Marketplace a su nombre y aprovecha las funcionalidades de comercio disponibles globalmente de Microsoft.

#### <a name="saas-offer-requirements"></a>Requisitos de la oferta de SaaS

Para enumerar las ofertas de software como servicio (SaaS) con Marketplace comercial en el Centro de partners, se deben cumplir los siguientes criterios:

- La oferta debe usar [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) para la administración y autenticación de identidades.
- La oferta debe usar las [API de suministro de SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) para integrarse en Azure Marketplace.
- Para más información acerca de los requisitos, consulte la [Guía de publicación de ofertas de SaaS](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

#### <a name="saas-on-azure-billing-infrastructure-costs"></a>SaaS en los costos de infraestructura de facturación de Azure
Si la oferta de SaaS se hospeda en Azure, el anunciante debe considerar los precios de uso de la infraestructura de Azure y los precios de las licencias de software como un elemento de costo individual. Este costo se representa como un precio mensual plano para el cliente. El uso de la infraestructura de Azure se administra y se factura directamente al asociado. El cliente no ve los honorarios de uso de infraestructura reales. En general, los publicadores optan por agrupar los honorarios de uso de la infraestructura de Azure en los precios de las licencias de software. 

Los precios de las licencias de software se presentan como una tarifa plana mensual de suscripción basada en sitio recurrente, no se realizan mediciones ni se basan en el consumo.

|**Costo de su licencia**|**100 USD al mes**|
|:---|:---|
|Costo de uso de Azure (D1/1 núcleo)|Facturado directamente al publicador, no al cliente|
|Microsoft factura al cliente|100,00 USD al mes (el anunciante debe contar con los costos de paso a través o en que se incurran en el precio de la licencia)|

- En este escenario, Microsoft factura 100,00 USD por su licencia de software y paga 80,00 USD al publicador.
- Los asociados que se sean aptos para el **precio reducido del servicio Marketplace** verán un precio de transacción reducido en las ofertas de SaaS desde mayo de 2019 hasta junio de 2020. En este escenario, Microsoft factura 100,00 USD por su licencia de software y paga 90,00 USD al anunciante.

> [!NOTE]
> **Precio reducido del servicio Marketplace:** Para determinadas ofertas de SaaS que haya publicado en nuestro Marketplace comercial, Microsoft reducirá su precio del servicio Marketplace del 20 % (como se describe en el Acuerdo para anunciantes de Microsoft) al 10 %. Para que su oferta sea apta, Microsoft debe haber designado que al menos una de sus ofertas esté lista para realizar una venta conjunta por IP o esté clasificada orden de prioridad para realizar una venta conjunta por IP.  Para recibir este precio reducido del servicio Marketplace durante el mes, se deben cumplir los criterios de idoneidad al menos cinco (5) días hábiles antes del final de cada mes natural.  El precio reducido del servicio Marketplace no se aplica a las máquinas virtuales, las aplicaciones administradas ni cualquier otro producto que esté disponible a través de nuestro Marketplace comercial.  El precio reducido del servicio Marketplace solo estará disponible para las ofertas aptas para los cargos de licencia recaudados por Microsoft entre el 1 de mayo de 2019 y el 30 de junio de 2020.  Transcurrido este tiempo, la tarifa del servicio Marketplace volverá a su importe normal. 

|**Microsoft factura**|**100 USD al mes**|
|:---|:---|
|Microsoft le paga el 80 % del costo de licencia <br>**En el caso de las aplicaciones SaaS aptas, Microsoft paga el 90 % del costo de la licencia*|80,00 USD al mes <br>*$* 90,00 al mes*|


#### <a name="csp-program-opt-in"></a>Participación en el programa CSP
El programa [Proveedor de soluciones en la nube (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) permite que las ofertas de software lleguen a millones de clientes de Microsoft aptos con una inversión en marketing y comercialización mínima.

- **Canales: hacer que mi oferta esté disponible en el programa CSP.**  (casilla)

Si elige esta opción, los proveedores de soluciones en la nube podrán vender su producto como parte de una solución agrupada a sus clientes. 

### <a name="list-through-microsoft"></a>Listas a través de Microsoft

La creación de una lista en Marketplace promocionará su negocio. Si selecciona la opción de mostrar solo su oferta, no realizar transacciones a través de Microsoft significa que Microsoft no participa directamente en las transacciones de las licencias de software. No hay precio de transacción asociado y el anunciante conserva el 100 % del precio de las licencias de software abonado por el cliente. Sin embargo, el anunciante es el responsables de dar soporte a todos los aspectos de la transacción de la licencia de software, entre los que se incluyen, entre otros: la cumplimentación del pedido, la medición, la facturación, el pago y el cobro. 

- **¿Cómo desea que los clientes potenciales interactúen con esta oferta en la lista?**

##### <a name="get-it-now-free"></a>Obténgalo ahora (de forma gratuita)
Presenta su oferta a los clientes de forma gratuita proporcionándoles una dirección URL válida (que comienza por http o https) desde donde pueden acceder a su aplicación.  Por ejemplo: `https://contoso.com/saas-app`

##### <a name="free-trial"></a>Evaluación gratuita
Presente su oferta a los clientes en forma de evaluación gratuita proporcionándoles una dirección URL válida (que comienza por http o https) desde donde pueden acceder a su aplicación.  Por ejemplo: `https://contoso.com/trial/saas-app`

##### <a name="contact-me"></a>Ponerse en contacto conmigo
Recopile información de contacto de los clientes mediante la conexión de su sistema de Administración de relaciones con los clientes (CRM). El cliente le pedirá permiso para compartir su información. Estos datos del cliente, junto con el nombre e identificador de la oferta, y el origen de Marketplace donde se encontró la oferta, se enviarán al sistema CRM que haya configurado. Para más información acerca de cómo configurar CRM, consulte [Conexión de administración de clientes potenciales](#connect-lead-management). 

## <a name="example-marketplace-offer-listing"></a>Descripción de la oferta de Marketplace de ejemplo

![Descripción de la oferta de Marketplace de ejemplo con notas](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Habilitación de una versión de prueba

Una versión de prueba es una excelente manera de presentar la oferta a clientes potenciales, ya que se les brinda la oportunidad de "probarla antes de comprarla", lo que aumenta la conversión y la generación de clientes potenciales altamente cualificados. [Más información acerca de las versiones de prueba.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

- **Habilitación de una versión de prueba** (casilla) 

Al habilitar una versión de prueba, se le pedirá que configure un entorno de demostración para que los clientes prueben su oferta durante un período fijo. 

### <a name="type-of-test-drive"></a>Tipo de versión de prueba

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Una plantilla de implementación que contiene todos los recursos de Azure que componen la solución. Los productos que se ajustan a este escenario usan solo recursos de Azure.
- **[Dynamics 365 for Business Central](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft hospeda y mantiene el servicio de la unidad de prueba (lo que incluye el aprovisionamiento y la implementación) de un sistema de planeamiento de recursos empresariales Business Central (finanzas, operaciones, cadena de suministros, CRM, etc.)  
- **[Dynamics 365 for Customer Engagement](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft hospeda y mantiene el servicio de la unidad de prueba (incluido el aprovisionamiento y la implementación) para un sistema de Customer Engagement (ventas, servicio, servicio de proyecto, el servicio de campo, etc.)  
- **[Dynamics 365 for Operations](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft hospeda y mantiene el servicio de la unidad de prueba (lo que incluye el aprovisionamiento y la implementación) de un sistema de planeamiento de recursos empresariales Finance and Operations (finanzas, operaciones, fabricación, cadena de suministros, etc.) 
- **[Aplicación lógica](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : una plantilla de implementación que abarca todas las arquitecturas de soluciones complejas. Todos los productos personalizados deben usar este tipo de versión de prueba.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : un vínculo incrustado a un panel personalizado. Los productos que deseen realizar una demostración de un objeto visual interactivo de Power BI deben usar este tipo de versión de prueba. Todo lo que necesita para la carga aquí es la dirección URL insertada de Power BI.

#### <a name="additional-test-drive-resources"></a>Recursos adicionales de una unidad de prueba
- [Procedimientos recomendados técnicos de la versión de prueba](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Procedimientos recomendados de marketing de la versión de prueba](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Póster Introducción a las versiones de prueba](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Conexión de administración de clientes potenciales

Conecte directamente con los clientes mediante el anuncio de la oferta en los catálogos de soluciones y el enlace de su sistema de Administración de relaciones con los clientes (CRM) para que pueda recibir información de contacto de cualquier cliente en cuanto este exprese interés por su producto o lo implemente.

- **Elija un destino de clientes potenciales** (menú desplegable): Proporcione detalles de la conexión al sistema CRM al que desea que le enviemos clientes potenciales. 

El Centro de partners admite los siguientes sistemas CRM para la administración de clientes potenciales. Seleccione el vínculo de las instrucciones de configuración.

- Azure Blob: especifique el correo electrónico del contacto, el nombre del contenedor y la cadena de conexión de la cuenta de almacenamiento. 
- [Azure Table](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table): especifique el correo electrónico del contacto y la cadena de conexión de la cuenta de almacenamiento. 
- [Dynamics CRM Online](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics): especifique el correo electrónico de contacto, la dirección URL y el modo de autenticación (Office 365 o Azure Active Directory).
- [Punto de conexión HTTPS](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https): especifique el correo electrónico de contacto y la dirección URL del punto de conexión HTTPS. 
- [Marketo](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo): especifique el correo electrónico de contacto, el identificador del formulario, el identificador de la cuenta de Munchkin y el identificador del servidor.
- [Salesforce](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce): especifique el correo electrónico de contacto y el identificador de la organización. 

#### <a name="additional-lead-management-resources"></a>Recursos adicionales de la administración de clientes potenciales
- [Preguntas frecuentes de la administración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Errores comunes de la configuración de clientes potenciales](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Póster Introducción a la administración de clientes potenciales](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

No olvide **guardar** antes de pasar a la siguiente sección.

## <a name="properties"></a>properties (Propiedades)
La pestaña **Propiedades** le pide que defina las categorías y los sectores que se usan para agrupar su oferta en los catálogos de soluciones, los contratos legales que dan soporte a su oferta y su versión de la aplicación. 

Después de completar estos campos seleccione **Guardar**. 

### <a name="category"></a>Categoría
Seleccione un mínimo de una (1) y un máximo de tres (3) categorías utilizadas para agrupar la oferta en las áreas de búsqueda de Marketplace adecuadas. Indique la forma en que la oferta da soporte a estas categorías en la descripción de la oferta. 

### <a name="industry"></a>Sector
Seleccione uno máximo de dos (2) sectores que se usan para agrupar la oferta en las áreas de búsqueda apropiadas de Marketplace. Si la oferta no es específica para un sector, no seleccione ninguno. Indique la forma en que la oferta da soporte a los sectores seleccionados en la descripción de la oferta. 

### <a name="app-version"></a>Versión de la aplicación
Se trata de un campo opcional que se utiliza en el Marketplace de AppSource para identificar el número de versión de la oferta. 

### <a name="standard-contract"></a>Contrato estándar

- **¿Desea usar el contrato estándar?**

Para simplificar el proceso de adquisición para los clientes y reducir la complejidad legal para los proveedores de software, Microsoft ofrece una plantilla de Contrato estándar con el fin de ayudar a facilitar una transacción en Marketplace. 

En lugar de crear términos y condiciones personalizados, los editores de Azure Marketplace pueden ofrecer su software en virtud del Contrato estándar, que los clientes solo tienen que revisar y aceptar una vez. 

El contrato estándar se puede encontrar aquí: https://go.microsoft.com/fwlink/?linkid=2041178.

#### <a name="terms-of-use"></a>Términos de uso

Si las condiciones de la licencia no coinciden con las del contrato estándar, puede optar por escribir sus propias condiciones de uso legales aquí. En este campo también puede escribir hasta 10 000 caracteres de texto. Si las condiciones de uso requieren una descripción más larga, escriba en este campo un vínculo de una dirección URL donde se pueden encontrar las condiciones de la licencia adicionales. A los clientes les aparecerá como un vínculo activo.

Los clientes deben aceptar estos términos antes de poder probar la aplicación. 

No olvide **guardar** antes de pasar a la siguiente sección.

## <a name="offer-listing"></a>Descripción de la oferta

La pestaña Descripción de la oferta muestra los idiomas (y los mercados) en los que está disponible la oferta, actualmente la única ubicación disponible es el inglés (Estados Unidos) . Además, esta página muestra el estado de la descripción específica del idioma y la fecha y hora en que se agregó. Tendrá que definir los detalles de Marketplace (nombre de la oferta, descripción, términos de búsqueda, etc.) para cada idioma y mercado.

### <a name="offer-listings"></a>Descripción de la oferta

Especifique los detalles que se muestran en Marketplace, lo que incluye descripciones de la oferta y los recursos de marketing.

- **Nombre** (obligatorio): El nombre que se define aquí aparecerá como título de la descripción de la oferta en los Marketplace que ha elegido. El nombre se rellena previamente en función de lo que se haya escrito en **Nueva oferta**.  Dicho nombre puede ser una marca comercial.  No debe contener espacios en blanco, emojis (a menos que sean una marca comercial y símbolos de copyright) y no puede tener más de 50 caracteres.
- **Resumen** (se requiere): especifique una descripción breve de la oferta que se va a usar en los resultados de búsqueda en las descripciones de Marketplace. En este campo se puede escribir hasta 100 caracteres de texto.
- **Descripción** (se requiere): especifique una descripción de la oferta que se va a mostrar en la información general de las descripciones de Marketplace. Considere la posibilidad de incluir una propuesta de valor, los beneficios clave, todas las asociaciones de la categoría o sector, las oportunidades de compra en la aplicación, todas las divulgaciones necesarias y un vínculo para obtener más información.
En este campo se puede escribir hasta 3000 caracteres de texto. En el artículo [Escribir una excelente descripción de la aplicación](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description) encontrará más sugerencias.
- **Palabras clave de búsqueda**: escriba un máximo de tres palabras clave de búsqueda que los clientes puedan usar para buscar su oferta en Marketplace.
- **Instrucciones para las Tareas iniciales**  (se requiere): se explica cómo configurar y empezar a usar la aplicación para los clientes potenciales.  Este tutorial de inicio rápido puede contener vínculos a documentación en línea más detallada. En este campo se puede escribir hasta 3000 caracteres de texto. 

#### <a name="links"></a>Vínculos

- **Directiva de privacidad** (se requiere): vínculo a la directiva de privacidad de su organización. Usted es el responsable no solo de garantizar que la aplicación cumple la normativa y la legislación en lo tocante a la privacidad, sino también de proporcionar una directiva de privacidad válida
- **Materiales de marketing del programa CSP** (opcional): Si decide ampliar su oferta al programa [Proveedor de soluciones en la nube (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers), debe incluir un vínculo a los materiales de marketing. CSP amplía su oferta a una rango mayor clientes aptos, ya que permite que los asociados de CSP incluyan su oferta en paquetes, la comercialicen y la distribuyan. Los revendedores necesitarán acceder a estos materiales para comercializar su oferta. Para más información, consulte [Servicios de comercialización](https://partner.microsoft.com/reach-customers/gtm).
- **Vínculos útiles** (opcional): Documentos en línea complementarios opcionales acerca de la aplicación o de servicios relacionados que se muestran tras escribir un **título** y una **dirección URL**. Para agregar más vínculos útiles, haga clic en **+ Agregar una dirección URL** .

#### <a name="contact-information"></a>Información de contacto

- **Contactos**: en cada contacto de cliente incluya el **nombre**, **número de teléfono** y dirección de **correo electrónico** de un empleado.  (estos datos *no* se mostrarán públicamente). También se necesita una **URL de soporte técnico** para el grupo **Contacto con soporte técnico**.  (esta información *se* mostrará públicamente).

**Contacto con soporte técnico** (se requiere): Para preguntas generales de soporte técnico.

**Contacto de ingeniería** (se requiere): Para preguntas técnicas.

**Contacto del administrador de canales** (se requiere): Para preguntas de revendedores relacionadas con el programa CSP.

#### <a name="files-and-images"></a>Archivos e imágenes

- **Documentos** (se requiere): agregue a su oferta documentos de marketing relacionados, en formato PDF. Incluya un proporciona un mínimo de un (1) documento por oferta y un máximo de tres (3).
- **Imágenes** (opcional): en todos los catálogos, hay varios lugares en los que pueden aparecer imágenes del logotipo de su oferta, que requieren los siguientes tamaños: Pequeño ( 48 x 48 píxeles _(se requiere),_ Mediano: 90 x 90 píxeles, Grande: 216 x 216 píxeles _(se requiere),_ Ancho: 255 x 115 píxeles y Héroe: 815 x 290 píxeles. Todas las imágenes deben estar en formato .PNG.
- **Capturas de pantalla** (se requiere): agregue capturas de pantalla que muestren la oferta. Se pueden agregar hasta cinco (5) y deben ser de 1280 x 720 píxeles. Todas las imágenes deben estar en formato .PNG.
- **Vídeos** (opcional): agregue vínculos a vídeos en que se muestre la oferta. Puede usar vínculos a vídeos de YouTube o Vimeo para que acompañen a la oferta para los clientes. También tendrá que incluir una imagen en miniatura del vídeo con un tamaño de 1280 x 720 píxeles y en formato PNG. Puede mostrar un máximo de cuatro vídeos por oferta.

No olvide **guardar** antes de pasar a la siguiente sección.

#### <a name="additional-marketplace-listing-resources"></a>Recursos adicionales de la descripción de Marketplace

- [Procedimientos recomendados la descripción de ofertas en Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)


## <a name="preview"></a>Vista previa

La pestaña **Versión preliminar** permite definir un **público preliminar** limitado para publicar la oferta antes de ponerla a disposición de un público más amplio de Marketplace.

> [!IMPORTANT]
> Debe seleccionar **Publicar** para que su oferta se publique en vivo para el público de Marketplace después de comprobar que la oferta está en versión preliminar.

- **Definición de un público preliminar: incorporación de un correo electrónico individual de la cuenta de AAD/MSA por línea, junto con una descripción opcional.**

Agregue un máximo de diez (10) direcciones de correo electrónico manualmente, o veinte (20) si carga un archivo CSV, para que las cuentas existentes de Azure Active Directory (AAD) o de Microsoft (MSA) para ayudar a validar la oferta antes de publicarla. Al agregar estas cuentas, define el público al que se le proporcionará acceso preliminar a la oferta antes de que se publique en Marketplace. Aunque la oferta ya esté publicada, puede definir un público preliminar para probar los cambios o las actualizaciones que ofrece.

> [!NOTE]
> El público preliminar no es el mismo que un público privado. A una audiencia preliminar se le permite acceder a la oferta _antes_ de que se publique en los catálogos de soluciones. También puede elegir crear un plan y ponerlo a disposición únicamente de un público privado. En la pestaña **Lista del plan** , puede definir un público privado con la casilla **This is a private plan** (Este es un plan privado). Puede definir un público privado de hasta 20 000 clientes mediante los identificadores de los inquilinos de Azure.

## <a name="technical-configuration"></a>Configuración técnica

La pestaña **Configuración técnica** define los detalles técnicos (ruta de acceso de la dirección URL, webhook, identificador de inquilino e identificador de aplicación) que se usan para conectarse a su oferta. Esta conexión nos permite aprovisionar su oferta para el cliente final, en caso de que este elija adquirirla. En la documentación de las [API de suministro de SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) hay diagramas que describen el uso de los campos recopilados.

- **Dirección URL de la página de destino** (se requiere): defina la dirección URL a la que se dirigirán a los clientes después de adquirir la oferta en Marketplace. Dicha URL será el punto de conexión que reciba un token cuando un cliente se enrute a la página. Dicho token se puede intercambiar para aprovisionar detalles mediante el uso de resolver en las API de suministro. Tanto estos detalles como otros que recopile pueden utilizarse como parte de una página web interactiva con los clientes basada en su experiencia para completar el registro y activar su compra.

- **Webhook de conexión** (se requiere): Para todos los eventos asincrónicos que Microsoft necesita enviarle en nombre del cliente (ejemplo: la suscripción de SaaS ha dejado de ser válida), le solicitamos que indique un webhook de conexión. Si aún no tiene un sistema de webhooks activo, la configuración más sencilla es una aplicación de lógica de punto de conexión HTTP que escuche los eventos que se publiquen en ella y que, después, los gestione adecuadamente (p. ej., https:\//prod-1westus.logic.azure.com:443/work).). Para obtener más información, consulte [Llamada, desencadenamiento o anidamiento de flujos de trabajo con puntos de conexión HTTP en aplicaciones lógicas](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Identificador de inquilino de Azure AD** (se requiere): En Azure Portal, es preciso que [cree una aplicación de Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) para que podamos validar que la conexión entre nuestros dos servicios está detrás de una comunicación autenticada. Para buscar el [identificador de inquilino](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id), vaya a Azure Active Directory y seleccione **Propiedades** y, después, busque el número de **identificador de directorio** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Identificador de aplicación de Azure AD** (se requiere): También necesita su [identificador de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key) y una clave de autenticación. Para obtener dichos valores, vaya a Azure Active Directory, seleccione **Registros de aplicaciones** y busque el número de**identificador de la aplicación** que se muestra (p.ej., 50c464d3-4930-494c-963c-1e951d15360e). Para buscar la clave de autenticación, vaya a **Configuración** y seleccione **Claves**. Deberá proporcionar una descripción y una duración y, después, se proporcionara un valor numérico.

 Tenga en cuenta que el identificador de aplicación de Azure está asociado a su identificador de anunciante, así que asegúrese de que se usa el mismo identificador de aplicación en todas las ofertas.

## <a name="plan-overview"></a>Información general del plan

La pestaña **Información general del plan** le permite ofrecer una gran variedad de opciones de planes dentro de la misma oferta. Estos planes (a veces denominados SKU) pueden diferir en lo tocante a versión, monetización o niveles de servicio. Para vender su oferta en Marketplace, debe configurar al menos un plan.

Una vez creados, verá los nombres de los planes, los identificadores, los modelos de precios, disponibilidad (pública o privada), el estado actual de la publicación y las acciones disponibles.

Las **acciones** disponibles en **Información general del plan** varían en función del estado actual del plan y pueden incluir:

- Si el estado del plan es **Borrador**: Eliminar borrador
- Si el estado del plan es **Activo**: Detener venta del plan o Sincronizar público privado

**Crear nuevo plan** (un mínimo de un plan para quienes seleccionen vender a través de Microsoft)

- **Id. de plan:** cree un identificador de plan único para cada plan de esta oferta. Los clientes lo verán en la dirección URL del producto y en las plantillas de Azure Resource Manager (si procede). Use solo caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. El identificador de este plan no puede tener más de 50 caracteres. Tenga en cuenta que el identificador no se puede modificar después de seleccionar que se cree.
- **Nombre del plan:** Los clientes verán este nombre al decidir que plan van a seleccionar en su oferta. Cree un nombre de oferta único para cada uno de los planes de esta oferta. El nombre del plan se usa para diferenciar los planes de software que pueden formar parte de la misma oferta (p. ej. Nombre de oferta: Windows Server; planes: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Lista del plan

La pestaña **Lista del plan** muestra los idiomas (y los mercados) en los que el plan está disponible; actualmente el inglés (Estados Unidos) es la única ubicación disponible. Además, esta página muestra el estado de la descripción específica del idioma y la fecha y hora en que se agregó. Tendrá que definir los detalles de Marketplace (nombre de la oferta, descripción, términos de búsqueda, etc.) para cada idioma y mercado.

#### <a name="plan-listing-details"></a>Detalles de la lista del plan

Si se selecciona uno de los idiomas del plan, se mostrará la información de la **lista del plan**, lo que incluye el **nombre** y la **descripción.**

- **Nombre**: rellenado previamente en función de la entrada de **nuevo plan** y aparecerá como título del "Plan de software" de su oferta que aparece en Marketplace.
- **Descripción:** Esta descripción es una oportunidad para explicar lo que hace que este plan de software sea único y las diferencias con respecto a otros planes de software de su oferta. Puede contener un máximo de 500 caracteres.

Después de completar estos campos seleccione **Guardar**.

#### <a name="plan-pricing-and-availability"></a>Precios y disponibilidad del plan

La pestaña **Precios y disponibilidad** le permite configurar los mercados en los que estará disponible este plan, el modelo de monetización deseado, el precio y el periodo de facturación. Además, puede indicar si desea que el plan sea visible a todos los usuarios o solo a clientes específicos (una audiencia privada).

#### <a name="markets"></a>Mercados

- **Editar mercados** (opcional)

Todos los planes debe estar disponible al menos en un mercado. Seleccione la casilla de la ubicación del mercado en la que desea que este plan esté disponible. Se incluyen un cuadro de búsqueda y un botón para seleccionar los países con "Impuesto remitido", en los que Microsoft remite de los impuestos de uso y ventas en su nombre, que sirven de ayuda. 


Si ya se ha establecido los precios del plan en dólares de Estados Unidos (USD) y agrega otra ubicación del mercado, el precio del nuevo mercado se calculará en función de los tipos de cambio actuales. Siempre debe revisar el precio de cada mercado antes de la publicación. Los precios se pueden consultar mediante el vínculo "Exportar precios (xlsx)" después de guardar los cambios.

#### <a name="pricing"></a>Precios

- **Modelo de precios**: Tarifa plana o basad en puestos

**Tarifa plana:** habilite el acceso a su oferta con un única tarifa plana mensual o anual. A veces esto se denomina precios basados en puesto.

**Basado en puestos:** habilite el acceso a su oferta con el precio en función del número de usuarios que acceden a la oferta u que ocupan los *puestos*. Este modelo basado en puestos le permite establecer el número mínimo y máximo de puestos permitido en función del precio. De esta forma, se pueden configurar diferentes puntos de precio en función del número de usuarios mediante la configuración de varios planes.  Estos campos son opcionales. Si se deja vacío, se interpretará que el número de puestos no tiene límite (el mínimo es 1 y el máximo es tantos como el sistema puede admitir). Estos campos se pueden editar como parte de una actualización del plan.

Una vez publicado, la elección de modelo de precios facturación no se puede cambiar. Además, todos los planes de la misma oferta deben compartir el mismo modelo de precios.

- **Periodo de facturación**: mensual o anual

Seleccione la frecuencia con que los clientes deben pagar el precio de la lista. Los clientes deben tener a su disposición al menos un precio mensual o anual, pero también pueden estar disponibles ambas opciones.

- **Precio**: USD al mes o USD al año

Los precios establecidos en la moneda local (USD = dólares de Estados Unidos) se convierten en la moneda local de todos los mercados seleccionados con los tipos de cambio actuales disponibles durante la instalación. Valide estos precios antes de realizar la publicación. Para ello, debe exportar la hoja de cálculo de precios y examinar el precio de cada mercado. Si desea establecer precios personalizados en un mercado individual, modifique e importe la hoja de cálculo de precios. Usted es responsable de validar estos precios y posee estos valores.
**Primero debe guardar los cambios de precios para habilitar la exportación de los datos de precios.*

Revise los precios cuidadosamente antes de la publicación, ya que hay algunas restricciones en lo que se puede cambiar una vez publicado un plan:

- Una vez que se publica un plan, el modelo de precios no se puede cambiar.
- Una vez que se publica el periodo de facturación de un plan, no se puede eliminar más tarde.
- Una vez que se publica un precio para un mercado de su plan, no puede cambiarse más adelante.

### <a name="plan-audience"></a>Público del plan

Tiene la opción de configurar cada plan para que lo pueda ver todo el mundo o solo un público concreto que elija. Para asignar la pertenencia a este público utilice los identificadores de inquilino de Azure AD.

#### <a name="privacy"></a>Privacidad

- **Este plan es privado** (casilla opcional)

Seleccione esta casilla para que el plan sea privado y solo pueda verlo el público que elija. Una vez que se haya publicado como un plan privado, es posible actualizar el público o elegir que el plan esté disponible para todos los usuarios. Una vez que se publica un plan como visible para todos los usuarios, debe permanecer visible para todos (el plan no se puede volver a configurar como un plan privado).

- **Público restringido (identificadores de inquilino)**

Asigne a la audiencia que tendrá acceso a este plan privado. El acceso se asigna mediante identificadores de inquilino y existe la opción de incluir una descripción de cada identificador de inquilino asignado. Se pueden agregar un máximo de 10 identificadores de inquilino, o 20 000 identificadores de inquilino de clientes si se importa un archivo de hoja de cálculo .csv.

Un inquilino es una representación de una organización, con un identificador representado como un GUID (identificador único global, un número entero de 128 bits utilizado para identificar los recursos). Se trata de una instancia dedicada de Azure AD que una organización o el desarrollador de la aplicación reciben cuando crean una relación con Microsoft, como al registrarse en Azure, Microsoft Intune o Microsoft 365. Cada inquilino de Azure AD es distinto e independiente de los demás inquilinos de Azure AD. Para comprobar el inquilino, inicie sesión en Azure Portal con la cuenta que desee usar para administrar la aplicación. Si tiene un inquilino, iniciará sesión de forma automática y verá el nombre de este directamente bajo el nombre de la cuenta. Mantenga el puntero sobre el nombre de su cuenta en el lado superior derecho de Azure Portal para ver el nombre, el correo electrónico, el directorio e identificador de inquilino (un GUID) y el dominio. Si la cuenta está asociada a varios inquilinos, puede seleccionar el nombre de la cuenta para abrir un menú donde puede cambiar entre los inquilinos. Cada inquilino tiene su propio identificador de inquilino. El identificador de inquilino de su organización también lo puede buscar mediante una dirección URL de nombre de dominio en: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

Mientras que las ofertas de SaaS usan identificadores de inquilino para definir un público privado, otros tipos de oferta pueden usar los identificadores de suscripción de Azure (que también se representan como GUID).

> [!NOTE]
> El público privado (o un público limitado) no es el mismo que un público preliminar. En la pestaña **[Versión preliminar](#preview)** , puede definir un público preliminar. El público preliminar puede acceder a su oferta *antes de* que esta se publique en Marketplace. Aunque la designación de público privado solo se aplica a un plan concreto, el público preliminar puede ver todos los planes (privados o no), pero solo durante el período de versión preliminar, mientras el plan se prueba y valida.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Lista de planes de ejemplo en una oferta de Marketplace

![Descripción de plan de Marketplace de ejemplo con notas](./media/marketplace-plan.svg)

## <a name="test-drive"></a>Versión de prueba

La pestaña **Versión de prueba** permite configurar una demostración (o "versión limitada de prueba") que permitirá a los clientes probar su oferta antes de comprarla. Puede encontrar más información al respecto en el artículo [¿Qué es la versión de prueba?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Si ya no desea ofrecer una versión de prueba de la oferta, vuelva a la página **[Configuración de la oferta ](#offer-setup)** y desactive **Habilitar versión de prueba**.

### <a name="technical-configuration"></a>Configuración técnica
Los siguientes tipos de versiones de prueba están disponibles, cada uno de ellos con sus propios requisitos de configuración técnica.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Aplicación lógica](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (la configuración técnica no es necesaria)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Configuración técnica para la versión de prueba de Azure Resource Manager

Una plantilla de implementación que contiene todos los recursos de Azure que componen la solución. Los productos que se ajustan a este escenario usan solo recursos de Azure. Más información acerca de cómo configurar una [versión de prueba de Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regiones** (se requiere): Actualmente hay 26 regiones admitidas de Azure en las que se puede usar la versión de prueba. La habitual es que desee que su versión de prueba esté disponible en las regiones en que prevé un mayor número de clientes, con el fin de que puedan seleccionar la región más cercana para lograr el mejor rendimiento posible. Tendrá que asegurarse de que su suscripción puede implementar todos los recursos necesarios en cada una de las regiones que seleccione.

- **Instances**: seleccione el tipo (activa o inactiva) y el número de instancias disponibles, que va multiplicarán por el número de regiones en las que está disponible la oferta.

**Acceso frecuente**: Este tipo de instancia se implementa y está en espera de acceso por cada región seleccionada. Los clientes pueden acceder al instante a las instancias *frecuentes* de una versión prueba, en lugar de tener que esperar a una implementación. La contrapartida es que estas instancias se ejecutan siempre en su suscripción de Azure, por lo que incurrirán en un mayor costo de tiempo de actividad. Se recomienda tener al menos una instancia *frecuente*, ya que la mayoría de los clientes no quieren esperar a que terminen las implementaciones completas, lo que genera un descenso en la utilización por parte de los clientes si no hay ninguna instancia *frecuente*.

**Inactiva**: este tipo de instancia representa el número total de instancias que se pueden implementar por región. Las instancias inactivas requieren que se implemente toda la plantilla de la versión de prueba de Resource Manager en el momento en que el cliente solicita la versión de prueba, por lo que las instancias *inactivas* tardan mucho más tiempo en cargarse que las *activas*. A cambio solo tiene que pagar por la duración de la versión de prueba, *no* se ejecuta de forma ininterrumpida en su suscripción de Azure como las instancias *activas*.

- **Plantilla de versión de prueba de Azure Resource Manager**: Cargue el archivo .zip que contiene la plantilla de Azure Resource Manager.  Para más información acerca de cómo crear una plantilla de Azure Resource Manager, consulte el artículo del tutorial de inicio rápido [Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Duración de la versión de prueba (horas)** (se requiere): Especifique el período de tiempo que la versión de prueba permanecerá activa, en número de horas. La versión de prueba finaliza automáticamente cuando finaliza este período de tiempo. Esta duración solo se puede indicar por un número completo de horas (p. ej. "2" horas; "1,5" no es un valor válido).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Configuración técnica de la versión de prueba de Dynamics 365

Microsoft puede eliminar la complejidad de configurar una versión de prueba hospedando y manteniendo el aprovisionamiento y la implementación del servicio de mediante este tipo de versión de prueba. La configuración de este tipo de versión de prueba hospedada es el mismo, independientemente de que esta destinado a un público de Business Central, Customer Engagement u Operations.

- **Número máximo de versiones de prueba simultáneas** (se requiere): establezca el número máximo de clientes que puede usar la versión de prueba al mismo tiempo. Cada usuario simultáneo consumirá una licencia de Dynamics 365 mientras la versión de prueba esté activa, por lo que deberá asegurarse de que tiene suficientes licencias disponibles para admitir el límite máximo establecido. Se recomienda un valor entre 3 y 5.

- **Duración de la versión de prueba (horas)** (se requiere): Especifique el período de tiempo que la versión de prueba permanecerá activa. Para ello debe definir el número de horas. Después de este número de horas, la sesión finalizará y dejará de consumir una de las licencias. Se recomienda un valor que oscile entre 2 y 24 horas, en función de la complejidad de la oferta. Esta duración solo se puede indicar por un número completo de horas (p. ej. "2" horas; "1,5" no es un valor válido).  El usuario puede solicitar una nueva sesión si se agota el tiempo y desea volver a acceder a la versión de prueba.

- **Dirección URL de la instancia** (se requiere): la dirección URL en la que el cliente comenzará su versión de prueba. Normalmente es la dirección URL de la instancia de Dynamics 365 en la que se ejecuta la aplicación con los datos de ejemplo instalados (p. ej., https://testdrive.crm.dynamics.com).

- **Dirección URL de API web de la instancia** (se requiere): recupere la dirección URL de la API web de la instancia de Dynamics 365, para lo que debe iniciar sesión en su cuenta de Microsoft 365 e ir a **Configuración** \&gt; **Personalización** \&gt; **Recursos para desarrolladores** \&gt; **API web de instancia (URL raíz de servicio )** , copie la dirección URL que se encuentra aquí (p. ej. https://testdrive.crm.dynamics.com/api/data/v9.0).

- **Nombre de rol** (se requiere): Proporcione el nombre del rol de seguridad que ha definido en su versión de prueba de Dynamics 365 personalizada. Se le asignará al usuario durante su versión de prueba (por ejemplo, test-drive-role).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Configuración técnica de la versión de prueba de Logic App

Los productos personalizados deben usar este tipo de plantilla de implementación de versión de prueba, que engloba varias arquitecturas de soluciones complejas. Para más información acerca de cómo configurar versiones de prueba de Logic App, visite [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) y [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) en GitHub.

- **Región** (se requiere, una lista desplegable con un solo elemento seleccionado): actualmente hay 26 regiones admitidas de Azure en las que se puede usar la versión de prueba. Los recursos de la aplicación lógica se implementarán en la región que seleccione. Si la aplicación lógica tiene recursos personalizados almacenado en una región concreta, asegúrese de que se selecciona aquí dicha región. La mejor forma de hacerlo es implementar totalmente Logic App de forma local en la suscripción de Azure en el portal y comprobar que funciona antes de realizar esta selección.

- **Número máximo de versiones de prueba simultáneas** (se requiere): establezca el número máximo de clientes que puede usar la versión de prueba al mismo tiempo. Estas versiones de prueba ya están implementadas, lo que permitiendo a los clientes acceder al instante a ellas sin tener que esperar que se implementen.

- **Duración de la versión de prueba (horas)** (se requiere): Especifique el período de tiempo que la versión de prueba permanecerá activa, en número de horas. La versión de prueba termina automáticamente al finalizar este período.

- **Nombre del grupo de recursos de Azure** (se requiere): Escriba el nombre del [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) en el que se guarda la versión de prueba de Logic App.

- **Nombre de aplicación lógica de Azure** (se requiere): escriba el nombre de la aplicación lógica que asigna a la versión de prueba al usuario. Dicha aplicación lógica debe guardarse en el grupo de recursos de Azure anterior.

- **Desaprovisionar nombre de aplicación lógica** (se requiere): Escriba el nombre de la aplicación lógica que desaprovisiona la versión de prueba una vez que el cliente ha finalizado. Dicha aplicación lógica debe guardarse en el grupo de recursos de Azure anterior.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>No es necesaria configuración técnica para las versiones de prueba de Power BI

Los productos que desean demostrar que un objeto visual interactivo de Power BI puede usar un vínculo insertado para compartir un panel personalizado como versión de prueba, no se necesita ninguna configuración técnica. Más información acerca de cómo configurar aplicaciones de la plantilla[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview).

### <a name="deployment-subscription-details"></a>Detalles de suscripción de implementación

Para implementar la versión de prueba en su nombre, cree y proporcione una suscripción de Azure independiente y única. (no es necesario para las versiones de prueba de Power BI).

- **Id. de suscripción de Azure** (necesario para Azure Resource Manager y Logic Apps): Escriba el identificador de la suscripción para conceder acceso a los servicios de la cuenta de Azure para la generación de informes y facturación del uso de los recursos. Se recomienda considerar la posibilidad de [crear una suscripción de Azure independiente](https://docs.microsoft.com/azure/billing/billing-create-subscription) que se use para las versiones de prueba, en caso de que no se tenga ninguna. Para buscar su identificador de la suscripción de Azure inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a la pestaña **Suscripciones** del menú izquierdo. Al seleccionar la pestaña se mostrará el identificador de la suscripción (por ejemplo, "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **Identificador de inquilino de Azure AD** (se requiere): Escriba el [id. de inquilino](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id) de Azure Active Directory (AD). Para buscar dicho identificador, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Azure Active Directory del menú de la izquierda, seleccione **Propiedades** y busque el número de **identificador de directorio** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e). El id. de inquilino de su organización también lo puede buscar mediante la dirección URL de su nombre de dominio en: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

- **Nombre de inquilino de Azure AD** (se requiere para Dynamic 365): Escriba el nombre de Azure Active Directory (AD). Para buscar este nombre, inicie sesión en [Azure Portal](https://portal.azure.com/) y en la esquina superior derecha se mostrará su nombre del inquilino, debajo de su nombre de cuenta.

- **Identificador de aplicación de Azure AD** (se requiere): Escriba el [identificador de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Para buscarlo, inicie sesión en [Azure Portal](https://portal.azure.com/), seleccione la pestaña Active Directory del menú de la izquierda, seleccione **Registros de aplicaciones** y busque el número de **Id. de aplicación** que aparece (por ejemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Secreto de cliente de aplicación de Azure AD** (se requiere): Escriba el [secreto de cliente](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key)de su aplicación de Azure AD. Para encontrar este valor, inicie sesión en [Azure Portal](https://portal.azure.com/). Seleccione la pestaña **Azure Active Directory** en el menú izquierdo, seleccione **Registros de aplicaciones** y, después, seleccione la versión de prueba de su aplicación. A continuación, seleccione **Certificados y secretos**, seleccione **Nuevo secreto de cliente**, escriba una descripción, seleccione **Nunca** en **Expira** y, después, elija **Agregar**. No olvide copiar el valor. (no abandone la página sin hacerlo, ya que, de lo contrario no tendrá acceso al valor).

No olvide **guardar** antes de pasar a la siguiente sección.

### <a name="test-drive-listings-optional"></a>Anuncios de versión de prueba (opcional)

La opción **Anuncios de versión de prueba** de la pestaña **Versión de prueba** muestra los idiomas (y los mercados) en los que la versión de prueba; actualmente el inglés (Estados Unidos) es la única ubicación disponible. Además, esta página muestra el estado de la descripción específica del idioma y la fecha y hora en que se agregó. Deberá definir los detalles de unidad de prueba (descripción, manual del usuario, vídeos, etc.) para cada idioma o el mercado.

- **Descripción** (se requiere): se describe la versión de prueba, lo que se demostrará, los objetivos con los que experimentará el usuario, las características que se van a explorar y cualquier información relevante que ayude al usuario a determinar si debe adquirir la oferta. En este campo se puede escribir hasta 3000 caracteres de texto. 

- **Información de acceso** (se requiere para las versiones de prueba de Azure Resource Manager y Logic): se explica todo lo que los clientes necesitan saber para acceder a esta versión de prueba y usarla. se muestra un escenario para usar la oferta y exactamente lo que el cliente debe saber para acceder a las características en la versión de prueba. En este campo se puede escribir hasta 10 000 caracteres de texto.

- **Manual del usuario** (se requiere): un tutorial detallado de la experiencia con la versión de prueba. Debe abarcar exactamente lo que desee que el cliente aprenda al usar la versión de prueba y puede servir como referencia para las preguntas que puedan surgirle. El archivo debe estar en formato PDF y hay que asignarle un nombre (255 caracteres como máximo) después de cargarlo.

- **Vídeos: agregar vídeos** (opcional): Se pueden cargar vídeos en YouTube o Vimeo y hace referencia a ellos aquí con un vínculo o una imagen en miniatura (533 × 324 píxeles) para que los clientes puedan ver toda la información que pueda ayudarles a conocer mejor la versión de prueba, incluida la forma de usar correctamente las características de la oferta y los escenarios que destacan sus ventajas.
  - **Nombre** (se requiere)
  - **URL (solo YouTube o Vimeo)** (se requiere)
  - **Miniatura (533 x 324 px)** : El archivo de imagen debe estar en formato PNG.

Después de completar estos campos seleccione **Guardar**.

## <a name="publish"></a>Publicar

#### <a name="submit-offer-to-preview"></a>Envío de la oferta para tener una versión preliminar

Una vez que haya completado todas las secciones necesarias de la oferta, seleccione **Publicar** en la esquina superior derecha del portal. Se le redirigirá a la página **Review and publish** (Revisar y publicar). 

Si es la primera vez que publica esta oferta, puede:

- Ver el estado de finalización de cada sección de la oferta.
    - *No iniciada*: significa que la sección no se ha modificado y se debe completar.
    - *Incompleta*: significa que la sección tiene errores que deben corregirse o que requiere más información. Por favor, vuelva a las secciones y actualícelas.
    - *Completa*: significa que la sección está completa, que se ha proporcionado todos los datos necesarios y que no hay ningún error. Para poder enviar la oferta, todas sus secciones deben estar en estado Completa.
- Envíe instrucciones para la realización de pruebas al equipo de certificación para asegurarse de que la aplicación se prueba correctamente, además de todas las notas adicionales que pueden resultar útiles para conocer la aplicación.
- Enviar la oferta para su publicación, para lo que debe seleccionar **Enviar**. Le enviaremos un correo electrónico que le avise cuando haya una versión preliminar de la oferta disponible para su revisión y aprobación. Debe volver al Centro de partners y seleccionar **Transmitir** la oferta para publicar la oferta para el público (o si es una privada, al público privado).

## <a name="next-steps"></a>Pasos siguientes

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
