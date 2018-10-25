---
title: Actualización de una oferta existente para Azure Marketplace | Microsoft Docs
description: En este artículo se explica cómo actualizar una oferta de Azure Marketplace existente mediante Cloud Partner Portal.
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: bfe2b0c70c8b912f6489ed461f5bcf6f233ed60d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807453"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Actualización de una oferta existente para Azure Marketplace
==============================================

Hay varios tipos de actualizaciones que puede aplicar a su oferta una vez que esté publicada.   [Cloud Partner Portal](https://cloudpartner.azure.com/) tiene varias características que le ayudarán a editar una oferta para asegurarse de que los cambios sean correctos, incluidas:

-  Agregar una nueva versión de la imagen de máquina virtual (VM) a una SKU existente.
-  Cambiar las regiones donde existe una SKU disponible.
-  Agregar nuevas SKU.
-  Actualizar los metadatos del Marketplace para las SKU o las ofertas. 
-  Actualizar precios e ofertas de pago por uso.
-  Comparar características.
-  Historial de características.

Después de modificar una oferta o una SKU, esta debe volver a publicarse antes que los cambios.
Este artículo le guiará por los distintos aspectos de la actualización de la oferta de la VM.


<a name="unpermitted-changes-to-vm-offersku"></a>Cambios en la oferta de la VM/SKU no permitidos
-----------------------------------

Hay algunos atributos de una oferta o SKU de VM que no se pueden modificar una vez que la oferta está publicada en Azure Marketplace.

-  Id. de la oferta e id. del publicador de la oferta.
-  ID de la SKU de SKU existentes.
-  Recuento del disco de datos de las SKU existentes.
-  Cambios en el modelo de facturación/licencia en las SKU existentes.


<a name="updating-the-vm-image-version-for-a-sku"></a>Actualización de la versión de imagen de la VM para una SKU
---------------------------------------

La imagen de la VM para una SKU de su oferta puede haberse actualizado con características adicionales, revisiones de seguridad, etc. En estos escenarios, le recomendamos actualizar la imagen de la VM a la que hace referencia la SKU. Use los pasos siguientes para actualizar una imagen de la VM. 

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En **Todas las ofertas**, busque la oferta que desea actualizar.
3.  En el formulario de **SKU**, haga clic en la SKU cuya imagen de la VM desea actualizar.
4.  En **Versión de disco**, haga clic en **+Nueva versión de disco** para agregar una nueva imagen de la VM.
5.  Proporcione la **versión de disco** de las imágenes nuevas de la VM. La versión del disco debe seguir el formato de [versión semántica](http://semver.org/). Las versiones deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros. Asegúrese de que la nueva versión que proporcione sea posterior a las versiones anteriores, o el número de versión nuevo no se mostrará en Azure Portal o Azure Marketplace al volver a publicar.
6.  En **Dirección URL del VHD del OS**, escriba el URI de firma de acceso compartido (SAS) creado para el VHD del sistema operativo. Tenga en cuenta que no se puede cambiar el recuento de discos de datos entre diferentes versiones de la SKU. Si las versiones anteriores tenían discos de datos configurados, esta nueva versión también debe tener los discos de datos configurados.
7.  Haga clic en **Publicar** para iniciar el flujo de trabajo de publicación para que la nueva versión de la VM se active en Azure Marketplace y Azure Portal.


<a name="changing-regions-a-sku-is-available-in"></a>Cambio de las regiones donde la SKU está disponible
--------------------------------------

Con el tiempo, puede querer que su oferta/SKU esté disponible en más regiones.
O bien, puede que desee dejar de ofrecer la oferta/SKU en una región determinada.
Para implementar estos cambios, realice los pasos siguientes.

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En **Todas las ofertas**, busque la oferta que desea actualizar.
3.  En el formulario de **SKU**, haga clic en la SKU para la que desea actualizar su disponibilidad en una región.
4.  Haga clic en el botón **Seleccionar países** situado bajo el campo **Disponibilidad en país/región**.
5.  En el elemento emergente de disponibilidad por región, agregue o elimine las regiones que desee para esta SKU.
6.  Haga clic en **Publicar** a fin de iniciar el flujo de trabajo de publicación para actualizar su disponibilidad por región de las SKU.

Si una SKU se pone a disposición en una región nueva, tendrá la posibilidad de especificar el precio para esa región determinada a través de la funcionalidad **Exportar datos de precios**. Si va a agregar de nuevo una región que ya estaba disponible anteriormente, no podrá actualizar su precio ya que no se permiten cambios en los precios.


<a name="adding-a-new-sku"></a>Incorporación de una SKU nueva
----------------

Puede hacer que una SKU nueva esté disponible para su oferta existente mediante los pasos que se indican a continuación.

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En **Todas las ofertas**, busque la oferta que desea actualizar.
3.  En el formulario de **SKU**, haga clic en **Agregar nueva SKU** y proporcione un **ID de SKU** en el elemento emergente.
4.  Siga el resto de los pasos detallados en [Publicación de una máquina virtual en Azure Marketplace](./cloud-partner-portal-publish-virtual-machine.md).
5.  Haga clic en **Publicar** a fin de iniciar el flujo de trabajo de publicación para que la SKU nueva se publique.


<a name="updating-offer-marketplace-metadata"></a>Actualización de los metadatos del Marketplace de la oferta
------------------------------------

Puede que existan escenarios donde tenga que actualizar los metadatos del Marketplace asociado con su oferta, como la actualización de los logotipos de la empresa, etc. Utilice los pasos siguientes para actualizar los metadatos de la oferta.

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En **Todas las ofertas**, busque la oferta que desea actualizar.
3.  Vaya al formulario del **Marketplace** y siga las instrucciones que aparecen aquí para realizar los cambios necesarios.
4.  Haga clic en **Publicar** a fin de iniciar el flujo de trabajo de publicación para que los cambios se publiquen.


<a name="updating-pricing-on-published-offers"></a>Actualización de precios en ofertas publicadas
------------------------------------

Una vez publicada su oferta de pago por uso, no puede subir el precio de una SKU existente, pero puede crear una nueva SKU en la misma oferta, eliminar la SKU antigua y, a continuación, volver a publicar la oferta. También se permite bajar el precio de las ofertas ya publicadas. Para bajar el precio de oferta:

1.  Haga clic en la SKU cuyo precio quiere bajar.
2.  Si ha establecido el precio en la GUI 1x1, puede cambiar el precio directamente en la interfaz de usuario. Si establece los precios a través de las opciones de importación o exportación de hoja de cálculo, solo puede bajar los precios a través de la característica de importación o exportación.
3.  Haga clic en **Save**(Guardar).
4.  Vuelva a publicar la oferta y transmítala.

Los precios serán visibles para los clientes nuevos una vez que se complete la publicación en el sitio web. Todos los clientes nuevos pagarán el nuevo precio reducido.

Para los clientes existentes, la reducción de precio se reflejarán con carácter retroactivo al principio del ciclo de facturación en que la reducción de precio entró en vigor.
Si ya se hayan facturado para el ciclo durante el cual se produjo la reducción de precio, recibirán un reembolso durante el ciclo de facturación siguiente para cubrir el precio reducido.


<a name="simplified-currency-pricing"></a>Precios en moneda simplificada
---------------------------

Desde septiembre de 2018, debería ver una nueva sección denominada **Precios en moneda simplificada**. Microsoft está simplificando el negocio de Azure Marketplace mediante la habilitación de precios más predecibles y colecciones de sus clientes en todo el mundo. Esta optimización se conseguirá reduciendo el número de monedas en que se factura a los clientes.

La nueva sección mostrará los precios en estas nuevas monedas. Una vez migrados todos los clientes a estas nuevas monedas de liquidación, la sección de precios original se retirará y solo se conservará la sección "Precios en la moneda simplificada".

Hasta el 1 de noviembre de 2018 podrá establecer un nuevo precio para las regiones donde se esté modificando la moneda de liquidación. No podrá subir el precio para las regiones donde no cambie la moneda de liquidación. Estas son las regiones en la que está cambiando la moneda:

| País                  | Código ISO2 | Moneda de facturación simplificada |
|--------------------------|-----------|-----------------------------|
| **Argelia**              | DZ        | USD                         |
| **Argentina**            | AR        | USD                         |
| **Baréin**              | BH        | USD                         |
| **Bielorrusia**              | BY        | USD                         |
| **Brasil**               | BR        | BRL                         |
| **Bulgaria**             | BG        | EUR                         |
| **Chile**                | CL        | USD                         |
| **Colombia**             | CO        | USD                         |
| **Costa Rica**           | CR        | USD                         |
| **Croacia**              | HR        | EUR                         |
| **República Checa**       | CZ        | EUR                         |
| **Egipto**                | EG        | USD                         |
| **Guatemala**            | GT        | USD                         |
| **Hong Kong**            | HK        | USD                         |
| **Hungría**              | HU        | EUR                         |
| **Islandia**              | IS        | EUR                         |
| **Indonesia**            | ID        | USD                         |
| **Israel**               | IL        | USD                         |
| **Jordania**               | JO        | USD                         |
| **Kazajistán**           | KZ        | USD                         |
| **Kenia**                | KE        | USD                         |
| **Kuwait**               | KW        | USD                         |
| **Liechtenstein**        | LI        | EUR                         |
| **ERY de Macedonia**     | MK        | USD                         |
| **Malasia**             | MY        | USD                         |
| **México**               | MX        | USD                         |
| **Montenegro**           | ME        | USD                         |
| **Marruecos**              | MA        | USD                         |
| **Nigeria**              | NG        | USD                         |
| **Omán**                 | OM        | USD                         |
| **Pakistán**             | PK        | USD                         |
| **Paraguay**             | PY        | USD                         |
| **Perú**                 | PE        | USD                         |
| **Filipinas**          | PH        | USD                         |
| **Polonia\***             | PL        | EUR                         |
| **Qatar**                | QA        | USD                         |
| **Rumania**              | RO        | EUR                         |
| **Arabia Saudí**         | SA        | USD                         |
| **Serbia**               | RS        | USD                         |
| **Singapur**            | SG        | USD                         |
| **Sudáfrica**         | ZA        | USD                         |
| **Tailandia**             | TH        | USD                         |
| **Trinidad y Tobago**  | TT        | USD                         |
| **Túnez**              | TN        | USD                         |
| **Turquía**               | TR        | USD                         |
| **Ucrania**              | UA        | USD                         |
| **Emiratos Árabes Unidos** | AE        | USD                         |
| **Uruguay**              | UY        | USD                         |
|  |  |  |

> [!NOTE]
> Si usa las API para publicar su oferta, es posible que vea una nueva sección en la oferta denominada **JSON**. Esta podría estar anotada como `virtualMachinePricingV2` o `monthlyPricingV2`, según el tipo de oferta.

Si tiene alguna pregunta en torno a este cambio, póngase en contacto con el servicio de soporte técnico de Azure Marketplace.


<a name="compare-feature"></a>Característica Comparar
---------------

Al realizar cambios en una oferta ya publicada, puede usar la característica *Comparar* para auditar los cambios realizados. Para usar la característica Comparar:

1.  En cualquier punto del proceso de edición, puede hacer clic en el botón **Comparar** de la oferta.

2.  Vea las versiones en paralelo de los activos de marketing y metadatos.


<a name="history-of-publishing-actions"></a>Historial de acciones de publicación
-----------------------------

Para ver toda la actividad de publicación histórica, haga clic en la pestaña **Historial** en el panel de navegación izquierdo de Cloud Partner Portal. Aquí podrá ver las acciones con marca de tiempo que se han realizado durante la vigencia de sus ofertas de Azure Marketplace.
