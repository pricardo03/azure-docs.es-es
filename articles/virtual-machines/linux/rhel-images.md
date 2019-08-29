---
title: Imágenes de Red Hat Enterprise Linux en Azure | Microsoft Docs
description: Obtenga información sobre las imágenes de Red Hat Enterprise Linux en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 8/14/2019
ms.author: borisb
ms.openlocfilehash: 25b0ccfa5ed2b7b9ff031565fe7f14b1477769c4
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543853"
---
# <a name="red-hat-enterprise-linux-images-in-azure"></a>Imágenes de Red Hat Enterprise Linux en Azure
En este artículo, se describen las imágenes disponibles de Red Hat Enterprise Linux (RHEL) en Azure Marketplace, además de directivas en relación con su nomenclatura y retención.

Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux).

>[!Important]
> Las imágenes RHEL actualmente disponibles en Azure Marketplace admiten los modelos de licencia "traiga su propia suscripción" (BYOS) y pago por uso (PAYG). No se admiten la [Ventaja para uso híbrido de Azure](../windows/hybrid-use-benefit-licensing.md) ni el cambio dinámico entre BYOS y pago por uso. Para cambiar el modo de licencia es necesario volver a implementar la máquina virtual desde la imagen correspondiente.

>[!Note]
> Si tiene cualquier problema con las imágenes RHEL de la Galería de Azure Marketplace, abra una incidencia de soporte técnico de Microsoft.

## <a name="images-available-in-the-ui"></a>Imágenes disponibles en la interfaz de usuario
Al buscar “Red Hat” en Marketplace o al crear un recurso en la interfaz de usuario de Azure Portal, verá un subconjunto de imágenes de RHEL disponibles y productos de Red Hat relacionados. En cualquier momento, puede obtener el conjunto completo de imágenes de VM disponibles mediante PowerShell, la API o la CLI de Azure.

Para ver el conjunto completo de imágenes de Red Hat disponibles en Azure, ejecute este comando:

```azurecli-interactive
az vm image list --publisher RedHat --all
```

## <a name="naming-convention"></a>Convención de nomenclatura
Las imágenes de VM en Azure se organizan por editor, oferta, SKU o versión. La combinación de editor:oferta:SKU:versión es el URN de la imagen, que identifica de forma única la imagen que se usará.

Por ejemplo, `RedHat:RHEL:7-RAW:7.6.2018103108` hace referencia a la imagen con partición sin formato de RHEL 7.6 compilada el 31 de octubre de 2018.

A continuación, se muestra un ejemplo de cómo crear una VM de RHEL 7.6.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>El moniker “latest” (último)
La API REST de Azure permite usar el moniker “latest” (último) para la versión, en lugar de la versión específica. Al usar “latest”, se aprovisionará la última imagen disponible para el editor, la oferta o la SKU específicos.

Por ejemplo, `RedHat:RHEL:7-RAW:latest` hace referencia a la última imagen con partición sin formato de la familia de RHEL 7 disponible.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:latest --no-wait
```

>[!NOTE]
> En general, la comparación de versiones para determinar la más reciente sigue las reglas del [método CompareTo](https://msdn.microsoft.com/library/a5ts8tb6.aspx).

### <a name="current-naming-convention"></a>Convención de nomenclatura actual
Todas las imágenes de RHEL publicadas actualmente usan el modelo de pago por uso y están conectadas a la [Infraestructura de actualización de Red Hat (RHUI) en Azure](https://aka.ms/rhui-update). Se ha adoptado una nueva convención de nomenclatura para las imágenes de familia de RHEL 7 en la que el esquema de particiones del disco (sin procesar, LVM) se especifica en la SKU en lugar de la versión. La versión de imagen de RHEL contendrá 7-RAW o 7-LVM. La nomenclatura de la familia de RHEL 6 no se ha cambiado en este momento.

En esta convención de nomenclatura habrá 2 tipos de SKU de imagen de RHEL 7: SKU que enumeren la versión secundaria y SKU que no. Si quiere utilizar una SKU 7-RAW o 7-LVM, puede especificar la versión secundaria de RHEL que quiera implementar en la versión. Si elige la versión "más reciente", se aprovisionará la versión secundaria más reciente de RHEL.

>[!NOTE]
> En el conjunto de imágenes de RHEL for SAP, la versión de RHEL permanece fija. Por lo tanto, su convención de nomenclatura incluye una versión específica en la SKU.

>[!NOTE]
> El conjunto de imágenes de RHEL 6 no se ha cambiado a la nueva convención de nomenclatura.

## <a name="extended-update-support-eus"></a>Compatibilidad de actualización extendida (EUS)
A partir de abril de 2019, están disponibles las imágenes de RHEL que se adjuntan a los repositorios de Compatibilidad de actualización extendida (EUS) de forma predeterminada. Para obtener más información sobre la EUS de RHEL EUS, consulte la [documentación de Red Hat](https://access.redhat.com/articles/rhel-eus).

Encontrará instrucciones sobre cómo cambiar la máquina virtual a EUS y más información acerca de las fechas de final de ciclo de vida de soporte técnico de EUS [aquí](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> EUS no es compatible en RHEL Extras. Esto significa que si va a instalar un paquete que está disponible normalmente en el canal de RHEL Extras, no podrá hacerlo mientras se encuentre en EUS. [Aquí](https://access.redhat.com/support/policy/updates/extras/) se detalla el ciclo de vida del producto Red Hat Extras.

### <a name="for-customers-that-want-to-use-eus-images"></a>Para los clientes que quieran usar imágenes de EUS:
Los clientes que quieran utilizar las imágenes que están asociadas a los repositorios de EUS deben usar la imagen de RHEL que contiene un número de versión secundaria de RHEL en la SKU. Estas imágenes serán particiones sin procesar (es decir, no LVM).

Por ejemplo, puede ver las siguientes 2 imágenes de RHEL 7.4 disponibles:
```bash
RedHat:RHEL:7-RAW:7.4.2018010506
RedHat:RHEL:7.4:7.4.2019041718
```
En este caso, se adjuntará `RedHat:RHEL:7.4:7.4.2019041718` a repositorios de EUS de forma predeterminada y `RedHat:RHEL:7-RAW:7.4.2018010506` se adjuntará a repositorios que no son de EUS de forma predeterminada.

### <a name="for-customers-that-dont-want-to-use-eus-images"></a>Para los clientes que no quieran usar imágenes de EUS:
Si no quiere usar una imagen que esté conectada a EUS de forma predeterminada, implemente mediante una imagen que no contenga un número de versión secundaria en la SKU.

#### <a name="rhel-images-with-eus"></a>Imágenes de RHEL con EUS
La tabla siguiente se aplicará a las imágenes de RHEL que contienen una versión secundaria de la SKU.

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


## <a name="list-of-rhel-images-available"></a>Lista de imágenes de RHEL disponibles
Las ofertas siguientes son SKU que están disponibles actualmente para uso general:

Oferta| SKU | Creación de particiones | Aprovisionamiento | Notas
:----|:----|:-------------|:-------------|:-----
RHEL          | 7-RAW    | RAW    | Agente Linux | Familia de imágenes de RHEL 7. <br> No se adjuntan a repositorios de EUS de forma predeterminada.
|             | 7-LVM    | LVM    | Agente Linux | Familia de imágenes de RHEL 7. <br> No adjuntada a repositorios de EUS de forma predeterminada.
|             | 7-RAW-CI | RAW-CI | Cloud-Init  | Familia de imágenes de RHEL 7. <br> No adjuntada a repositorios de EUS de forma predeterminada.
|             | 6.7      | RAW    | Agente Linux | Imágenes de RHEL 6.7, convención de nomenclatura anterior
|             | 6,8      | RAW    | Agente Linux | Igual que anteriormente para RHEL 6.8
|             | 6.9      | RAW    | Agente Linux | Igual que anteriormente para RHEL 6.9
|             | 6.10     | RAW    | Agente Linux | Igual que anteriormente para RHEL 6.10
|             | 7,2      | RAW    | Agente Linux | Igual que anteriormente para RHEL 7.2
|             | 7.3      | RAW    | Agente Linux | Igual que anteriormente para RHEL 7.3
|             | 7.4      | RAW    | Agente Linux | Igual que anteriormente para RHEL 7.4. <br> Adjuntada a repositorios de EUS de forma predeterminada a partir de abril de 2019
|             | 7.5      | RAW    | Agente Linux | Igual que anteriormente para RHEL 7.5. <br> Adjuntada a repositorios de EUS de forma predeterminada a partir de junio de 2019
|             | 7.6      | RAW    | Agente Linux | Igual que anteriormente para RHEL 7.6. <br> Adjuntada a repositorios de EUS de forma predeterminada a partir de mayo de 2019
RHEL-SAP      | 7.4      | LVM    | Agente Linux | RHEL 7.4 for SAP HANA and Business Apps
|             | 7.5      | LVM    | Agente Linux | RHEL 7.5 for SAP HANA and Business Apps
RHEL-SAP-HANA | 6.7      | RAW    | Agente Linux | RHEL 6.7 for SAP HANA
|             | 7,2      | LVM    | Agente Linux | RHEL 7.2 for SAP HANA
|             | 7.3      | LVM    | Agente Linux | RHEL 7.3 for SAP HANA
RHEL-SAP-APPS | 6,8      | RAW    | Agente Linux | RHEL 6.8 for SAP Business Applications
|             | 7.3      | LVM    | Agente Linux | RHEL 7.3 for SAP Business Applications
RHEL-HA       | 7.4      | RAW    | Agente Linux | RHEL 7.4 con el complemento de alta disponibilidad
|             | 7.5      | RAW    | Agente Linux | RHEL 7.5 con el complemento de alta disponibilidad
|             | 7.6      | RAW    | Agente Linux | RHEL 7.6 con el complemento de alta disponibilidad
RHEL-SAP-HA   | 7.4      | RAW    | Agente Linux | RHEL 7.4 for SAP con el complemento de alta disponibilidad
|             | 7.5      | RAW    | Agente Linux | RHEL 7.5 for SAP con el complemento de alta disponibilidad
|             | 7.6      | RAW    | Agente Linux | RHEL 7.6 for SAP con el complemento de alta disponibilidad

### <a name="old-naming-convention"></a>Convención de nomenclatura anterior
La familia de imágenes de RHEL 7 y la familia de imágenes de RHEL 6 usan versiones específicas en sus SKU hasta el cambio de convención de nomenclatura explicado anteriormente.

Encontrará las SKU numéricas en la lista completa de imágenes. Microsoft y Red Hat crearán nuevas SKU numéricas cuando se publique una nueva versión secundaria.

### <a name="other-available-offers-and-skus"></a>Otras ofertas y SKU disponibles
La lista completa de ofertas y SKU disponibles puede contener imágenes adicionales más allá de lo que se indica en la tabla siguiente (por ejemplo, `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`). Estas ofertas pueden usarse para proporcionar soporte técnico para soluciones específicas de Marketplace, o bien pueden publicarse con fines de pruebas o para versiones preliminares. Estas pueden cambiarse o quitarse en cualquier momento sin advertencia alguna. No las use, excepto si su presencia se ha documentado públicamente por Microsoft o Red Hat.

## <a name="publishing-policy"></a>Directiva de publicación
Microsoft y Red Hat actualizan las imágenes cuando se publican nuevas versiones secundarias, según sea necesario para solucionar CVE específicos, o bien para actualizaciones o cambios de configuración ocasionales. Nos esforzamos por proporcionar imágenes actualizadas lo antes posible (en un plazo de tres días laborables después de la publicación o disponibilidad de una corrección de CVE).

Solo actualizamos la versión secundaria actual en una familia de imágenes especifica. Cuando se publique una versión secundaria más reciente, dejaremos de actualizar la versión secundaria anterior. Por ejemplo, con la publicación de RHEL 7.6, las imágenes de RHEL 7.5 ya no se actualizarán.

>[!NOTE]
> Las VM de Azure activas aprovisionadas de imágenes de pago por uso de RHEL están conectadas a RHUI de Azure y pueden recibir actualizaciones y correcciones en cuanto Red Hat las publique y las replique en RHUI de Azure (normalmente, en un plazo inferior a 24 horas después de la publicación oficial por Red Hat). Estas VM no necesitan una nueva imagen publicada para obtener las actualizaciones y los clientes tienen control total sobre el momento en que quieran iniciar la actualización.

## <a name="image-retention-policy"></a>Directiva de retención de imágenes
Nuestra directiva actual es mantener todas las imágenes publicadas anteriormente. Nos reservamos el derecho de quitar las imágenes que se conoce que causan algún tipo de problema. Por ejemplo, puede que se quiten las imágenes con configuraciones incorrectas debido a actualizaciones posteriores de componentes o plataformas. Las imágenes que puedan quitarse seguirán la directiva actual de Marketplace para proporcionar notificaciones a un plazo de 30 días antes de la eliminación de la imagen.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre la infraestructura de actualización de Azure Red Hat [aquí](https://aka.ms/rhui-update).
* Puede encontrar información sobre las directivas de soporte técnico de Red Hat para todas las versiones de RHEL en la página [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) (Ciclo de vida de Red Hat Enterprise Linux).
