---
title: Creación de una oferta de máquina virtual | Microsoft Docs
description: Publique una máquina virtual en Azure Marketplace.
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
ms.openlocfilehash: d77dad52e75253de02fd079d791861356c4c5e1f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247082"
---
# <a name="publish-a-virtual-machine-to-azure-marketplace"></a>Publicación de una máquina virtual en Azure Marketplace

En este artículo proporcionan los pasos necesarios para publicar una oferta de máquina virtual en Azure Marketplace.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos técnicos y de otra índole se aplican a la publicación de una máquina virtual en Azure Marketplace.

### <a name="technical"></a>Requisitos previos técnicos

-   [Requisitos previos técnicos para la creación de una imagen de máquina virtual para Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Creación y carga de un disco duro virtual de Linux](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Creación y prueba de una máquina virtual de Linux a partir de una imagen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Creación y carga de un disco duro virtual con Windows ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Creación y prueba de una máquina virtual de Windows a partir de una imagen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [Solución de problemas comunes detectados durante la creación del VHD](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

-   [Recomendaciones de seguridad para imágenes de Azure Marketplace](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images)


### <a name="non-technical-business-requirements"></a>Requisitos previos no técnicos (empresariales)

 -   Su empresa (o sus subsidiarias) se encuentra en uno de los países de venta admitidos por Azure Marketplace.

-   El producto debe contar con una licencia compatible con los modelos de facturación admitidos por Azure Marketplace.

-   Usted es el responsable de poner el soporte técnico a disposición de los clientes de forma comercialmente razonable. Dicho soporte puede ser gratuito, de pago o a través del soporte técnico de la comunidad.

-   Asimismo, es responsable de la concesión de licencias para su software y las dependencias de software de terceros.

-   Proporcione contenido que cumpla los criterios para que su oferta se muestre en Azure Marketplace y en el Portal de administración de Azure.

-   Acepte los términos del contrato del anunciante y de las directivas de participación en Azure Marketplace.

-   Acepte los [términos de uso](https://azure.microsoft.com/support/legal/website-terms-of-use/), la [declaración de privacidad de Microsoft](https://www.microsoft.com/privacystatement/default.aspx) y el [contrato del programa Microsoft Azure Certified](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="before-you-begin"></a>Antes de empezar

Después de cumplir todos los requisitos previos, puede empezar a crear la oferta de plantilla de solución. Antes de comenzar, revise la siguiente información de la oferta y la SKU.

**Oferta**

Una oferta de aplicación de Azure se corresponde con una clase de oferta de producto de un publicador. Si tiene un nuevo tipo de solución o aplicación que quiere que esté disponible en Azure Marketplace, la mejor opción es una nueva oferta. Una oferta es una colección de SKU. Cada oferta aparece como su propia entidad en Azure Marketplace.

**SKU**

Una SKU es la unidad de compra más pequeña de una oferta. Dentro de la misma clase de producto (oferta), las SKU permiten distinguir entre las diferentes características admitidas. Por ejemplo, la oferta puede ser administrada o no administrada, y se admiten distintos modelos de facturación.

Agregue varias SKU en los escenarios siguientes:
- Si quiere admitir diferentes modelos de facturación, como traiga su propia licencia (BYOL) o pago por uso (PAYG).
- Si cada SKU admite un conjunto de características distinto y si cada uno de ellos tiene precios diferentes.

Una SKU se presenta en la oferta principal de Azure Marketplace y se muestra como una entidad de compra propia en Azure Portal.

## <a name="to-create-a-new-offer"></a>Procedimiento para crear una nueva oferta

1.  Inicie sesión en [Cloud Partner Portal](http://cloudpartner.azure.com/).

2.  En la barra de navegación izquierda, haga clic en **+ Nueva oferta** y seleccione **Virtual Machines**.

    ![Nueva oferta para máquinas virtuales](./media/cloud-partner-portal-publish-virtual-machine/publishvm1.png)

3.  En **Nueva oferta**, seleccione **Editor**.

![Editor de oferta nueva](./media/cloud-partner-portal-publish-virtual-machine/publishvm2.png)

4.  En **Editor**, deberá proporcionar información de las vistas siguientes:
    - Configuración de oferta
    - SKU
    - Marketplace
    - Soporte técnico. Cada vista contiene un conjunto de campos que debe rellenar. Los campos obligatorios se marcan con un asterisco rojo (\*).

## <a name="to-configure-offer-settings"></a>Para definir la configuración de la oferta

1. Configure los siguientes campos de **Offer Identity** (Identidad de la oferta) en Configuración de la oferta.

    **Identificador de oferta**

     Identificador único de la oferta en un perfil del anunciante. Este identificador se muestra en las direcciones URL de producto, en las plantillas de Azure Resource Manager y en los informes de facturación. Solo puede usar caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. Por ejemplo, si el anunciante **contoso** publica una oferta con el identificador de oferta **sample-vm**, aparecerá en Azure Marketplace como **https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview**  
    >[!Note]
    >Este campo queda bloqueado en cuanto se lanza una oferta.

    **Id. del editor**

    Lista desplegable de perfiles del anunciante. Elija el perfil en el que quiera publicar la oferta. 
    >[!Note]
    >Este campo queda bloqueado en cuanto se lanza una oferta.

    **Nombre**

    Nombre para mostrar de la oferta. Este nombre se mostrará en Azure Marketplace y en Azure Portal. Puede tener un máximo de 50 caracteres. Utilice las siguientes instrucciones para el nombre de la oferta:
    -  Incluya un nombre de marca que identifique el producto. 
    - No incluya aquí el nombre de su empresa a menos que sea así como se comercializa la oferta.
    - Si comercializa esta oferta en su propio sitio web, asegúrese de que el nombre sea idéntico al que aparece en su sitio web.

2. Seleccione **Guardar** para cerrar la ventana Configuración de la oferta

## <a name="to-create-a-sku"></a>Para crear una SKU

1. Seleccione **SKU**. 
2. Seleccione **Agregar una SKU** para introducir el identificador de la SKU. El identificador de la SKU es un identificador único para la SKU dentro de una oferta. Este identificador se muestra en las direcciones URL de producto, en las plantillas de Azure Resource Manager y en los informes de facturación. El identificador de SKU:
    - Puede tener un máximo de 50 caracteres.
    - Puede contener solo caracteres alfanuméricos en minúscula o guiones (-).
    - El identificador no puede terminar con un guión.

    ![Agregar una SKU](./media/cloud-partner-portal-publish-virtual-machine/publishvm4.png)


## <a name="configure-sku-details"></a>Configuración de los detalles de la SKU

Después de agregar una SKU, esta aparece en la lista de SKU de la vista de SKU. Para ver los detalles de la SKU, seleccione el nombre de la SKU. Puede usar la vista de detalles para agregar información en los siguientes campos.

### <a name="hide-this-sku"></a>Hide this SKU (Ocultar esta SKU)

Use este valor para administrar la visibilidad de SKU. Si "Hide this SKU" (Ocultar esta SKU) está desactivado, los clientes ven la SKU en [Azure Marketplace](https://azuremarketplace.microsoft.com) y en [Azure Portal](https://portal.azure.com/). Puede ocultar la SKU si desea que solo esté disponible a través de plantillas de soluciones y no para adquirirla por separado.

### <a name="cloud-availability"></a>Cloud Availability (Disponibilidad en la nube)

Este campo permite definir la disponibilidad de la SKU en las distintas nubes de Azure.

**Public Azure** (Azure público)

Esta SKU se implementar en clientes de todas las regiones públicas de Azure que tengan la integración de Marketplace.

**Azure Government Cloud (Nube de Azure Government)**

Esta SKU se puede implementar en Azure Government Cloud. Antes de publicar en [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), se recomienda que los publicadores prueben y validen que la solución funciona según lo previsto. Para realizar copias intermedias y pruebas, [solicite una cuenta de prueba](https://azure.microsoft.com/offers/ms-azr-usgov-0044p). 

>[!Note]
>Microsoft Azure Government es una nube de la comunidad gubernamental con acceso controlado para los clientes de asociados tribales, locales, estatales y federales de Estados Unidos Y asociados aptos para abastecer a tales entidades.

### <a name="countryregion-availability"></a>Country/Region availability (Disponibilidad por país/región)

Este campo identifica las regiones en que la SKU va a estar disponible para su compra. Debe ser cauteloso a la hora de poner a disposición del público sus SKU. Algunos países se clasifican como "Microsoft Tax Remitted Country" (País sujeto a recaudación fiscal por parte de Microsoft).

-   En caso de los países categorizados como "Microsoft Tax Remitted Country" (País sujeto a recaudación fiscal por parte de Microsoft), Microsoft recauda los impuestos de los clientes y los devuelve a la Administración Pública.

-   En otros países, los asociados son los responsables de recaudar los impuestos de los clientes y de devolverlos al gobierno. Si decide vender en estos países, debe tener la capacidad de calcular y pagar los impuestos en ellos.

![Seleccionar la disponibilidad de país o región](./media/cloud-partner-portal-publish-virtual-machine/publishvm5.png)

### <a name="pricing"></a>Precios

Actualmente se admiten dos modelos de precios.

#### <a name="bring-your-own-license-byol"></a>Traiga su propia licencia (BYOL)

Usted administra la concesión de licencias del software que se ejecuta en la máquina virtual. Microsoft solo cargará los costos de infraestructura.

#### <a name="usage-based-monthly-billed-sku"></a>Facturación mensual de SKU en función del uso

A los clientes se les cobra por hora según las tarifas que hayan establecido los publicadores por los tamaños de máquina virtual. En el caso del modelo de **facturación horaria** de las SKU, el precio total será la suma de los costos de software que cobra el publicador y del costo de infraestructura que cobra Microsoft. Este costo total se mostrará al cliente como precio mensual y por hora cuando se plantee realizar la compra. La facturación en este caso será mensual.

En el modelo basado en el uso, debe definir valores de configuración adicionales.

**prueba gratuita**

Puede especificar si desea proporcionar una prueba gratuita a los clientes.
Al cliente no se le cobra el costo de software durante los primeros 30 o 90 días (en función de lo que se haya seleccionado) después de implementar la máquina virtual. Cuando transcurra el periodo de la versión de prueba gratuita se les cobra por hora según las tarifas que hayan establecido los publicadores por los tamaños de máquina virtual.

**Precio por núcleo**

Puede establecer el precio por núcleo para la SKU. Para ello, basta con que escriba un precio base para un único núcleo y los precios para el resto de los núcleos se calcularán automáticamente. Escriba los precios en USD en el portal y los precios para otras regiones se calcularán automáticamente. Puede comprobar los precios en las demás regiones mediante la opción **Export Pricing Data** (Exportar datos de precios).

![Precio por núcleo](./media/cloud-partner-portal-publish-virtual-machine/publishvm6.png)


**Precios discretos**

Puede definir el precio de cada conjunto de núcleos de forma individual si desea definir el precio de cada núcleo por separado.

![Precios discretos](./media/cloud-partner-portal-publish-virtual-machine/publishvm7.png)

**Precios de exportación e importación**

Tiene la flexibilidad de exportar los precios configurados en el portal para poder realizar cambios en la interfaz de Excel. Esto también permite verificar los precios por región y los precios en la divisa local.
Al hacer clic en **Export-Pricing** (Exportar precios), se descargará un archivo de Excel con los detalles de precios rellenados previamente. Podrá editarlos en el archivo de Excel y usar después la opción **Import-Pricing** (Importar precios) para importar los cambios realizados.
Los precios importados también se reflejarán en el portal.

En este archivo de Excel, los precios de las diferentes regiones se muestran en la divisa local. El tipo de cambio aplicado se actualiza diariamente.

![Precios de exportación e importación con ejemplos de tasa de cambio](./media/cloud-partner-portal-publish-virtual-machine/publishvm8.png)

>[!IMPORTANT]
>-   Los precios no se pueden cambiar una vez lanzada una oferta. Sin embargo, sí podrá agregar o quitar regiones admitidas.
>-   Este precio se cobra al usuario aparte de los  [precios de máquinas virtuales de Azure](https://aka.ms/vmpricingdetails).
>-   Los precios se establecen para todas las regiones en la moneda local y se usa el tipo de cambio disponible en el momento en que se fijan los precios.
>-   Para establecer o ver el precio de cada región individualmente, exporte la hoja de cálculo de precios e impórtela con precios personalizados.

## <a name="vm-images"></a>Imágenes de máquinas virtuales

La siguiente sección que hay que completar será la de las imágenes de máquina virtual. Antes de pasar a esta sección, debe tener preparado el disco duro virtual que desea publicar. A continuación se proporcionan algunos vínculos que facilitarán la creación del disco duro virtual:

-   [Requisitos previos técnicos para la creación de una imagen de máquina virtual para Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Creación y carga de un disco duro virtual con Linux](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Creación y prueba de una máquina virtual de Linux a partir de una imagen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Creación y carga de un disco duro virtual con Windows ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Creación y prueba de una máquina virtual de Windows a partir de una imagen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [Solución de problemas comunes detectados durante la creación del VHD](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

En cuanto el disco duro virtual esté preparado, puede empezar a rellenar esta sección.
A continuación se exponen algunos detalles de varios campos.

### <a name="recommended-vm-sizes"></a>Recommended VM Sizes (Tamaños de máquina virtual recomendados)

Seleccione hasta seis tamaños de máquina virtual recomendados. Se trata de las recomendaciones que se muestran al cliente en Azure Marketplace y en la hoja del plan de tarifa de Azure Portal cuando este decide comprar e implementar su imagen. **Estas son solo recomendaciones. El cliente puede seleccionar cualquier tamaño de máquina virtual que admita los discos especificados en su imagen.**  La siguiente captura de pantalla muestra los tamaños de máquina virtual recomendados que verán un cliente en el Azure Portal.


![Recommended VM Sizes (Tamaños de máquina virtual recomendados)](./media/cloud-partner-portal-publish-virtual-machine/publishvm9.png)


### <a name="open-ports"></a>Open Ports (Abrir puertos)

Especifique los puertos que le gustaría que estén abiertos y disponibles. Estos puertos están abiertos durante la implementación de las máquinas virtuales.

### <a name="adding-vm-images"></a>Adición de imágenes de máquina virtual

El siguiente paso es agregar una imagen de máquina virtual para la SKU. Puede agregar hasta ocho versiones de disco por SKU. En Azure Marketplace solo aparecerá el número de versión de disco más alto de una SKU concreta. Las demás versiones se pueden consultar a través de las API.

En **Disk version** (Versión de disco), seleccione **+ Nueva versión**. Esto muestra los siguientes campos que se deben rellenar.

#### <a name="vm-image-version"></a>Versión de la imagen de máquina virtual

La versión de imagen de máquina virtual debe seguir el formato de [versión semántica](http://semver.org/). Las versiones deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros. Imágenes de SKU diferentes pueden tener distintas versiones principales y secundarias. Las versiones dentro de una SKU solo deberían ser los cambios incrementales que aumenten la versión de revisión (Z de X.Y.Z).

#### <a name="os-vhd-url"></a>DIRECCIÓN URL DEL VHD DEL SO

Escriba el [URI de firma de acceso compartido](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) creado para el disco duro virtual del sistema operativo.

Si hay discos de datos asociados a esta SKU, puede agregarlos. Para ello, debe hacer clic en el vínculo **+ New data disk** (+ Nuevo disco de datos). Esta acción muestra otros campos que debe rellenar.

#### <a name="lun-vhd-url"></a>DIRECCIÓN URL DEL VHD DEL LUN

Escriba el [URI de firma de acceso compartido](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) del disco de datos.

#### <a name="lun-number"></a>Número LUN

Asigne un número a este LUN. Este número se reservará para este disco de datos dentro de esta SKU.

>[!Note]
>Una vez que se publica una SKU con una versión concreta de máquina virtual y los discos de datos, estos se bloquean y no se pueden cambiar. Para otras versiones de máquina virtual adicionales agregadas a la SKU, el número de discos de datos que debe admitir no se puede cambiar.

#### <a name="common-sas-url-issues--fixes"></a>Problemas y soluciones comunes de la dirección URL de SAS

| Problema                                                                 | Message                                                                           | Solución                                                           |  Vínculo a la documentación                                                                                |
|---------------------------------------------------------------------  |-------------------------------------------------------------------------------    |-----------------------------------------------------------    |---------------------------------------------------------------------------------------------------    |
| Error al copiar imágenes - "?" no se encuentra en la dirección URL de SAS                | Error al copiar imágenes No se puede descargar el blob mediante el identificador URI de SAS indicado.       | Actualización de la dirección URL de SAS mediante las herramientas recomendadas                    | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Error al copiar imágenes: los parámetros "st" y "se" no están en la dirección URL de SAS   | Error al copiar imágenes No se puede descargar el blob mediante la dirección URL de SAS indicada.        | Actualización de la dirección URL de SAS incluyendo las fechas de inicio y finalización             | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Error al copiar imágenes: "sp=rl" no está en la dirección URL de SAS                    | Error al copiar imágenes No se puede descargar el blob mediante la dirección URL de SAS indicada.         | Actualización de la dirección Url de SAS con los permisos establecidos como "Lectura" y "Lista"     | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Error al copiar imágenes: la dirección URL de SAS tiene espacios en blanco en el nombre de disco duro virtual.     | Error al copiar imágenes No se puede descargar el blob mediante la dirección URL de SAS indicada.        | Actualización de la dirección URL de SAS sin espacios en blanco                       | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Error al copiar imágenes: error de autorización de dirección Url de SAS               | Error al copiar imágenes No se puede descargar blob debido a un error de autorización.     | Regeneración de la dirección URL de SAS                                        | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |


## <a name="to-configure-the-marketplace"></a>Para configurar el Marketplace

Use la vista del Marketplace para configurar los campos que se muestran para la oferta en [Azure Marketplace](https://azuremarketplace.microsoft.com) y en [Azure Portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview Subscription Ids (Id. de suscripción de versión preliminar)

Lista de identificadores de suscripciones de Azure a los que le gustaría conceder acceso a la oferta una vez publicada. Estas suscripciones permitidas le dan la posibilidad de probar la oferta en versión preliminar antes de publicarla. En el portal de asociados puede habilitar como permitidas hasta 100 suscripciones.

### <a name="suggested-categories"></a>Suggested Categories (Categorías sugeridas)

Seleccione hasta cinco categorías de la lista proporcionada con las que la oferta puede asociarse mejor. Las categorías seleccionadas se usarán para asociar la oferta con las categorías de productos disponibles en [Azure Marketplace](https://azuremarketplace.microsoft.com) y [Azure Portal](https://portal.azure.com/).

Los ejemplos siguientes muestran información del Marketplace en Azure Marketplace y en Azure Portal.

**Azure Marketplace**


![publishvm10](./media/cloud-partner-portal-publish-virtual-machine/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-virtual-machine/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-virtual-machine/publishvm15.png)


**Azure Portal**


![publishvm12](./media/cloud-partner-portal-publish-virtual-machine/publishvm12.png)



![publishvm13](./media/cloud-partner-portal-publish-virtual-machine/publishvm13.png)


### <a name="logo-guidelines"></a>Directrices para logotipos

Siga estas directrices para los logotipos cargados en Cloud Partner Portal:

-   El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.

-   Los colores del tema del Portal de Azure son el blanco y el negro. Evite emplear estos colores como fondo de los logotipos. Utilice un color que permita destacar los logotipos en Azure Portal. Nosotros recomendamos usar colores primarios simples.

    >[!Note] 
    >Si utiliza un fondo transparente, asegúrese de que el texto y los logotipos no sean de color azul, blanco o negro.

-   No utilice un fondo degradado en el logotipo.

-   Evite colocar texto en el logotipo. Esto incluye el nombre de su empresa o marca. El aspecto del logotipo debe ser *plano* y debe evitar degradados.

-   El logotipo no se debe ajustar.

#### <a name="hero-logo"></a>Logotipo de imagen prominente

El logotipo de imagen prominente es opcional. El publicador puede decidir no cargar un logotipo de imagen prominente. Sin embargo, el logotipo no se puede eliminar una vez cargado. El asociado debe seguir las directrices de Azure Marketplace para imágenes prominentes.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Instrucciones para el logotipo de imagen prominente

-   El nombre para mostrar del anunciante, el título del plan y el resumen largo de la oferta se muestran con una fuente de color blanco. Evite usar colores claros en el fondo. Los fondos transparentes y de color negro o blanco no pueden usarse en las imágenes prominentes.

-   El nombre para mostrar del anunciante, el título del plan, el resumen largo de la oferta y el botón Crear se incrustan mediante programación dentro del logotipo de imagen prominente cuando se publica la oferta. No escriba ningún texto cuando diseñe el logotipo de imagen prominente. Deje un espacio en blanco a la derecha del logotipo. Este espacio debe ser de 415 x 100 píxeles y estar desplazado 370 píxeles de la izquierda.

![Ejemplo de logotipo de imagen prominente](./media/cloud-partner-portal-publish-virtual-machine/publishvm14.png)

### <a name="lead-management"></a>Administración de clientes potenciales

Para configurar los valores de administración de clientes potenciales de la oferta, siga [estás instrucciones](./cloud-partner-portal-get-customer-leads.md).

## <a name="to-configure-support"></a>Procedimiento para configurar el soporte técnico

Use la vista de Soporte técnico para proporcionar la siguiente información:

- Contactos de soporte técnico de su empresa, como, por ejemplo, de ingeniería.
- Contactos de asistencia al cliente.

## <a name="to-publish-the-offer"></a>Procedimiento para publicar la oferta

El último paso es publicar la oferta. Siga [estas instrucciones para publicar su oferta](./cloud-partner-portal-make-offer-live-on-azure-marketplace.md).
