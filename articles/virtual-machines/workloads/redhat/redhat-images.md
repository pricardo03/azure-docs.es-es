---
title: Imágenes de Red Hat Enterprise Linux en Azure | Microsoft Docs
description: Obtenga información sobre las imágenes de Red Hat Enterprise Linux en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: 5ed5d9337dd4e7acdbba25c4cb66d2690793f250
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77134386"
---
# <a name="overview-of-red-hat-enterprise-linux-images"></a>Introducción a las imágenes de Red Hat Enterprise Linux
En este artículo, se describen las imágenes disponibles de Red Hat Enterprise Linux (RHEL) en Azure Marketplace, además de directivas en relación con su nomenclatura y retención.

Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux). Para información detallada sobre los precios, visite la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

>[!IMPORTANT]
> Las imágenes RHEL actualmente disponibles en Azure Marketplace admiten los modelos de licencia "traiga su propia suscripción" (BYOS) y pago por uso (PAYG). No se admiten la [Ventaja para uso híbrido de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) ni el cambio dinámico entre BYOS y pago por uso. Para cambiar el modo de licencia es necesario volver a implementar la máquina virtual desde la imagen correspondiente.

>[!NOTE]
> Si tiene cualquier problema con las imágenes RHEL de Azure Marketplace, abra una incidencia de soporte técnico de Microsoft.

## <a name="viewing-images-available-in-azure"></a>Visualización de imágenes disponibles en Azure
Al buscar “Red Hat” en Marketplace o al crear un recurso en la interfaz de usuario de Azure Portal, verá solamente un subconjunto de todas imágenes de RHEL disponibles. En cualquier momento, puede obtener el conjunto completo de imágenes de VM disponibles mediante PowerShell, la API o la CLI de Azure.

Para ver el conjunto completo de imágenes de Red Hat disponibles en Azure, ejecute este comando:

```azurecli-interactive
az vm image list --publisher RedHat --all
```

### <a name="naming-convention"></a>Convención de nomenclatura
Las imágenes de VM en Azure se organizan por editor, oferta, SKU o versión. La combinación de editor:oferta:SKU:versión es el URN de la imagen, que identifica de forma única la imagen que se usará.

Por ejemplo, `RedHat:RHEL:7-LVM:7.6.2018103108` hace referencia a una imagen con partición LVM de RHEL 7.6 compilada el 31 de octubre de 2018.

A continuación, se muestra un ejemplo de cómo crear una VM de RHEL 7.6.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-LVM:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>El moniker “latest” (último)
La API REST de Azure permite usar el moniker “latest” (último) para la versión, en lugar de la versión específica. Al usar “latest”, se aprovisionará la última imagen disponible para el editor, la oferta o la SKU específicos.

Por ejemplo, `RedHat:RHEL:7-LVM:latest` hace referencia a la imagen con partición LVM más reciente de la familia de RHEL 7 disponible.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-LVM:latest --no-wait
```

>[!NOTE]
> En general, la comparación de versiones para determinar la más reciente sigue las reglas del [método CompareTo](https://msdn.microsoft.com/library/a5ts8tb6.aspx).
Esta comparación de la versión de la imagen se realiza comparando los valores como un objeto [Version](https://docs.microsoft.com/dotnet/api/system.version.-ctor?view=netframework-4.8), no como una cadena.

## <a name="rhel-6-image-types"></a>Tipos de imagen de RHEL 6
En el caso de las imágenes de RHEL 6.x, los tipos de imagen son los siguientes:

|Publicador | Oferta | Valor de SKU | Versión | Detalles
|----------|-------|-----------|---------|--------
|RedHat | RHEL | Versión secundaria (por ejemplo, 6.9) | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 6.9.2018010506) | Todas las imágenes estándar de RHEL 6.x siguen esta convención
|RedHat | rhel-byos | rhel-raw69 | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 6.9.20181023) | Se trata de una imagen BYOS de RHEL 6.9.
|RedHat | RHEL | RHEL-SAP-APPS | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 6.8.2017053118) | Se trata de una imagen de RHEL 6.8 for SAP Applications. Tiene derecho de acceso a los repositorios de SAP Applications, así como a los repositorios de RHEL base.
|RedHat | RHEL | RHEL-SAP-HANA | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 6.7.2017053121) | Se trata de una imagen de RHEL 6.7 para SAP HANA. Tiene derecho de acceso a los repositorios de SAP HANA, así como a los repositorios de RHEL base.

## <a name="rhel-7-image-types"></a>Tipos de imagen de RHEL 7
En el caso de las imágenes de RHEL 7.x, hay algunos tipos de imagen diferentes. En la tabla siguiente se muestran los diferentes conjuntos de imágenes que ofrecemos. Puede ver una lista completa con el comando Az de la CLI `az vm image list --publisher redhat --all`.

>[!NOTE]
> A menos que se indique lo contrario, todas las imágenes tienen particiones LVM y se conectarán a los repositorios de RHEL convencionales (es decir, que no son EUS ni E4S). A partir de entonces, solamente vamos a publicar imágenes con particiones LVM, pero abiertas a los comentarios sobre esta decisión. Los detalles sobre la Compatibilidad de actualización extendida y los Servicios de actualización para SAP están disponibles en la [página sobre el ciclo de vida de Red Hat Enterprise Linux](https://access.redhat.com/support/policy/updates/errata).

|Publicador | Oferta | Valor de SKU | Versión | Detalles
|----------|-------|------------|---------|--------
|RedHat | RHEL | Versión secundaria (por ejemplo, 7.6) | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019102813) | Las imágenes publicadas antes de abril de 2019 se adjuntarán a los repositorios de RHEL estándar. Las imágenes publicadas después de abril de 2019 se adjuntarán a los repositorios de la Compatibilidad de actualización extendida (EUS) de Red Hat para permitir el bloqueo de versión de una versión secundaria concreta. Los clientes que quieran repositorios convencionales deben usar las imágenes que contienen 7-LVM o 7-RAW en el valor de SKU (a continuación figuran los detalles). Las imágenes de RHEL 7.7 y versiones posteriores tendrán particiones LVM. Todas las demás imágenes de esta categoría tienen particiones sin formato.
|RedHat | RHEL | 7-RAW | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019102813) | Estas imágenes tienen particiones sin formato (es decir, no se han agregado volúmenes lógicos).
|RedHat | RHEL | 7-RAW-CI | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019072418) | Estas imágenes tienen particiones sin formato (es decir, no se han agregado volúmenes lógicos) y usarán cloud-init para el aprovisionamiento.
|RedHat | RHEL | 7-LVM | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019062414) | Estas imágenes tienen particiones LVM.
|RedHat | rhel-byos | rhel-{lvm,raw} | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.7.20190819) | Estas imágenes son las imágenes BYOS de RHEL 7. No se adjuntan a ningún repositorio y no cobrarán el precio de RHEL prémium. Si está interesado en las imágenes BYOS de RHEL, [solicite acceso](https://aka.ms/rhel-byos). Los valores de SKU finalizan con la versión secundaria y denotan si la imagen no tiene formato o tiene particiones LVM. Por ejemplo, un valor de SKU de rhel-lvm77 indica una imagen de RHEL 7.7 con particiones LVM.
|RedHat | RHEL | RHEL-SAP | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019071300) | Estas imágenes son imágenes de RHEL para SAP. Tienen derecho de acceso a los repositorios de SAP HANA y SAP Applications, así como a los repositorios de RHEL E4S. La facturación incluye RHEL prémium y SAP prémium por encima de la tarifa de proceso base.
|RedHat | RHEL | RHEL-SAP-HA | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019062320) | Estas imágenes son RHEL for SAP con imágenes de Alta disponibilidad y de Servicios de actualización. Tienen derecho de acceso a los repositorios de SAP HANA y SAP Applications y a los repositorios de alta disponibilidad, así como a los repositorios de RHEL E4S. La facturación incluye RHEL prémium, SAP prémium y alta disponibilidad prémium por encima de la tarifa de proceso base.
|RedHat | RHEL | RHEL-HA | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.6.2019062019) | Se trata de imágenes de RHEL que también tienen derecho de acceso al complemento de alta disponibilidad. Se cobrarán ligeramente por encima de la tarifa de proceso base y RHEL debido al complemento de alta disponibilidad prémium.
|RedHat | RHEL | RHEL-SAP-APPS | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.3.2017053118) | Estas imágenes no están actualizadas, ya que los repositorios de SAP Applications y SAP HANA se han combinado en los repositorios de SAP. Se trata de imágenes de RHEL for SAP Applications. Tienen derecho a acceder a los repositorios de aplicaciones de SAP, así como a los repositorios de RHEL base.
|RedHat | RHEL | RHEL-SAP-HANA | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 7.3.2018051421) | Estas imágenes no están actualizadas, ya que los repositorios de SAP Applications y SAP HANA se han combinado en los repositorios de SAP. Se trata de imágenes de RHEL for SAP HANA. Tienen derecho a acceder a los repositorios de SAP HANA, así como a los repositorios de RHEL base.

## <a name="rhel-8-image-types"></a>Tipos de imagen de RHEL 8
A continuación se muestran los detalles de los tipos de imagen RHEL 8.

|Publicador | Oferta | Valor de SKU | Versión | Detalles
|----------|-------|------------|---------|--------
|RedHat | RHEL | 8 | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 8.0.20191023) | Estas imágenes tienen particiones LVM de RHEL 8.0 conectadas a repositorios estándar de Red Hat.
|RedHat | RHEL | 8-gen2 | Valores concatenados de la versión secundaria de RHEL y la fecha de publicación (por ejemplo, 8.0.20191024) | Estas imágenes tienen particiones LVM de RHEL 8.0 de Hyper-V generación 2 conectadas a repositorios estándar de Red Hat. Puede encontrar más información sobre las máquinas virtuales de generación 2 en Azure [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2).

## <a name="rhel-longer-support-add-ons"></a>RHEL admite complementos por más tiempo

### <a name="extended-update-support-eus"></a>Compatibilidad de actualización extendida (EUS)
A partir de abril de 2019, están disponibles las imágenes de RHEL que se adjuntan a los repositorios de Compatibilidad de actualización extendida (EUS) de forma predeterminada. Para obtener más información sobre la EUS de RHEL EUS, consulte la [documentación de Red Hat](https://access.redhat.com/articles/rhel-eus).

El cambio a los repositorios de EUS es posible y se admite. Encontrará instrucciones sobre cómo cambiar la máquina virtual a EUS y más información acerca de las fechas de final de ciclo de vida de soporte técnico de EUS [aquí](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> EUS no es compatible con RHEL Extras. Esto significa que si va a instalar un paquete que está disponible normalmente en el canal de RHEL Extras, no podrá hacerlo mientras se encuentre en EUS. [Aquí](https://access.redhat.com/support/policy/updates/extras/) se detalla el ciclo de vida del producto Red Hat Extras.

#### <a name="differentiating-between-regular-and-eus-images"></a>Diferenciación entre imágenes convencionales y de EUS.
Los clientes que quieran utilizar las imágenes que están asociadas a los repositorios de EUS deben usar la imagen de RHEL que contiene un número de versión secundaria de RHEL en la SKU.

Por ejemplo, puede ver las dos siguientes imágenes de RHEL 7.4 disponibles:
```bash
RedHat:RHEL:7-LVM:7.6.2019062414
RedHat:RHEL:7.6:7.6.2019102813
```
En este caso, se adjuntará `RedHat:RHEL:7.6:7.6.2019102813` a repositorios de EUS de forma predeterminada (valor de SKU de 7.4) y `RedHat:RHEL:7-LVM:7.6.2019062414` se adjuntará a repositorios que no son de EUS de forma predeterminada (valor de SKU value of 7-LVM).

Si desea utilizar repositorios convencionales (que no sean EUS), realice la implementación mediante una imagen que no contenga un número de versión menor en el SKU.

#### <a name="rhel-images-with-eus"></a>Imágenes de RHEL con EUS
La tabla siguiente se aplicará a las imágenes de RHEL conectadas a repositorios de EUS.

>[!NOTE]
> En el momento de escribir este artículo, solo RHEL 7.4 y versiones posteriores secundarias tienen compatibilidad con EUS. Esta característica ya no es compatible con la versión 7.3, o versiones anteriores, de RHEL.
>
> Puede encontrar más información sobre la disponibilidad de EUS de RHEL [aquí](https://access.redhat.com/support/policy/updates/errata).

Versión secundaria |Ejemplo de imagen de EUS              |Estado de EUS                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7.4      |RedHat:RHEL:7.4:7.4.2019041718 | Las imágenes publicadas en abril de 2019 y más adelante serán de EUS de forma predeterminada|
RHEL 7.5      |RedHat:RHEL:7.5:7.5.2019060305 | Las imágenes publicadas en junio de 2019 y más adelante serán de EUS de forma predeterminada |
RHEL 7.6      |RedHat:RHEL:7.6:7.6.2019052206 | Las imágenes publicadas en mayo de 2019 y más adelante serán de EUS de forma predeterminada  |
RHEL 8.0      |N/D                            | No hay EUS disponible de Red Hat                               |

### <a name="update-services-for-sap-e4s"></a>Servicios de actualización para SAP (E4S)
Las imágenes más recientes de RHEL for SAP se conectarán a los servicios de actualización para las suscripciones de soluciones SAP (E4S). Puede encontrar más información sobre E4S en la [documentación](https://access.redhat.com/support/policy/updates/errata#Update_Services_for_SAP_Solutions) de Red Hat.

#### <a name="rhel-images-with-e4s"></a>Imágenes de RHEL con E4S
Las imágenes de las siguientes ofertas creadas después de diciembre de 2019 se conectarán a los repositorios de E4S.

* RHEL-SAP (RHEL for SAP)
* RHEL-SAP-HA (RHEL for SAP con alta disponibilidad y servicios de actualización)

## <a name="other-available-offers-and-skus"></a>Otras ofertas y SKU disponibles
La lista completa de ofertas y SKU disponibles puede contener imágenes adicionales más allá de lo que se indica en la tabla siguiente (por ejemplo, `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`). Estas ofertas pueden usarse para proporcionar soporte técnico para soluciones específicas de Marketplace, o bien pueden publicarse con fines de pruebas o para versiones preliminares. Estas pueden cambiarse o quitarse en cualquier momento sin advertencia alguna. No las use, excepto si su presencia se ha documentado públicamente por Microsoft o Red Hat.

## <a name="publishing-policy"></a>Directiva de publicación
Microsoft y Red Hat actualizan las imágenes cuando se publican nuevas versiones secundarias, según sea necesario para solucionar CVE específicos, o bien para actualizaciones o cambios de configuración ocasionales. Nos esforzamos por proporcionar imágenes actualizadas lo antes posible (en un plazo de tres días laborables después de la publicación o disponibilidad de una corrección de CVE).

Solo actualizamos la versión secundaria actual en una familia de imágenes especifica. Cuando se publique una versión secundaria más reciente, dejaremos de actualizar la versión secundaria anterior. Por ejemplo, con la publicación de RHEL 7.6, las imágenes de RHEL 7.5 ya no se actualizarán.

>[!NOTE]
> Las VM de Azure activas aprovisionadas de imágenes de pago por uso de RHEL están conectadas a RHUI de Azure y pueden recibir actualizaciones y correcciones en cuanto Red Hat las publique y las replique en RHUI de Azure (normalmente, en un plazo inferior a 24 horas después de la publicación oficial por Red Hat). Estas VM no necesitan una nueva imagen publicada para obtener las actualizaciones y los clientes tienen control total sobre el momento en que quieran iniciar la actualización.

## <a name="image-retention-policy"></a>Directiva de retención de imágenes
Nuestra directiva actual es mantener todas las imágenes publicadas anteriormente. Nos reservamos el derecho de quitar las imágenes que se conoce que causan algún tipo de problema. Por ejemplo, puede que se quiten las imágenes con configuraciones incorrectas debido a actualizaciones posteriores de componentes o plataformas. Las imágenes que puedan quitarse seguirán la directiva actual de Marketplace para proporcionar notificaciones a un plazo de 30 días antes de la eliminación de la imagen.

## <a name="next-steps"></a>Pasos siguientes
* Consulte la lista completa de [imágenes de RHEL en Azure](./redhat-imagelist.md).
* Obtenga más información sobre la infraestructura de actualización de Azure Red Hat [aquí](https://aka.ms/rhui-update).
* Obtenga más información sobre la [oferta BYOS de RHEL](./byos.md).
* Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux).
