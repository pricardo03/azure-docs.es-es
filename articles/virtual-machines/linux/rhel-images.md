---
title: Imágenes de Red Hat Enterprise Linux en Azure | Microsoft Docs
description: Obtenga información sobre las imágenes de Red Hat Enterprise Linux en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/18/2019
ms.author: borisb
ms.openlocfilehash: fb3c0e46324a22bdd95bf7d93c28e69c195927e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542448"
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
Todas las imágenes de RHEL publicadas actualmente usan el modelo de pago por uso y están conectadas a la [Infraestructura de actualización de Red Hat (RHUI) en Azure](https://aka.ms/rhui-update). Debido a una limitación de diseño de RHUI, se ha adoptado una nueva convención de nomenclatura para las imágenes de la familia de RHEL 7. La nomenclatura de la familia de RHEL 6 no se ha cambiado en este momento.

La limitación está relacionada con el hecho de que, cuando una `yum update` no selectiva se ejecuta con una VM conectada a RHUI, la versión de RHEL se actualiza a la versión más reciente de la familia actual. Para más información, consulte [este vínculo](https://aka.ms/rhui-update). Esto puede causar confusión cuando una imagen aprovisionada de RHEL 7.2 se convierte en RHEL 7.6 después de una actualización. Aún se puede aprovisionar a partir de una imagen anterior, como se muestra en los ejemplos siguientes, si se especifica la versión necesaria de manera explícita. Si la versión necesaria no se especifica al aprovisionar una nueva imagen de RHEL 7, se aprovisionará la última imagen.

>[!NOTE]
> En el conjunto de imágenes de RHEL for SAP, la versión de RHEL permanece fija. Por lo tanto, su convención de nomenclatura incluye una versión específica en la SKU.

>[!NOTE]
> El conjunto de imágenes de RHEL 6 no se ha cambiado a la nueva convención de nomenclatura.

Las ofertas siguientes son SKU que están disponibles actualmente para uso general:

Oferta| SKU | Creación de particiones | Aprovisionamiento | Notas
:----|:----|:-------------|:-------------|:-----
RHEL | 7-RAW | RAW | Agente Linux | Familia de imágenes de RHEL 7
| | 7-LVM | LVM | Agente Linux | Familia de imágenes de RHEL 7
| | 7-RAW-CI | RAW-CI | Cloud-Init | Familia de imágenes de RHEL 7
| | 6.7 | RAW | Agente Linux | Imágenes de RHEL 6.7, convención de nomenclatura anterior
| | 6,8 | RAW | Agente Linux | Igual que anteriormente para RHEL 6.8
| | 6.9 | RAW | Agente Linux | Igual que anteriormente para RHEL 6.9
| | 6.10 | RAW | Agente Linux | Igual que anteriormente para RHEL 6.10
| | 7,2 | RAW | Agente Linux | Igual que anteriormente para RHEL 7.2
| | 7.3 | RAW | Agente Linux | Igual que anteriormente para RHEL 7.3
| | 7.4 | RAW | Agente Linux | Igual que anteriormente para RHEL 7.4
| | 7.5 | RAW | Agente Linux | Igual que anteriormente para RHEL 7.5
RHEL-SAP | 7.4 | LVM | Agente Linux | RHEL 7.4 for SAP HANA and Business Apps
| | 7.5 | LVM | Agente Linux | RHEL 7.5 for SAP HANA and Business Apps
RHEL-SAP-HANA | 6.7 | RAW | Agente Linux | RHEL 6.7 for SAP HANA
| | 7,2 | LVM | Agente Linux | RHEL 7.2 for SAP HANA
| | 7.3 | LVM | Agente Linux | RHEL 7.3 for SAP HANA
RHEL-SAP-APPS | 6,8 | RAW | Agente Linux | RHEL 6.8 for SAP Business Applications
| | 7.3 | LVM | Agente Linux | RHEL 7.3 for SAP Business Applications

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
