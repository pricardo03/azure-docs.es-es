---
title: Recomendaciones sobre alta disponibilidad de Azure Advisor | Microsoft Docs
description: Utilice Azure Advisor para mejorar la alta disponibilidad de las implementaciones de Azure.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 297a213fe4219b834187f977e3281eb939352f60
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39249070"
---
# <a name="advisor-high-availability-recommendations"></a>Recomendaciones sobre alta disponibilidad de Advisor

Azure Advisor lo ayuda a garantizar y mejorar la continuidad de las aplicaciones empresariales críticas. Puede obtener recomendaciones sobre alta disponibilidad de Advisor en la pestaña **High Availability** (Alta disponibilidad) del panel de Advisor.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Garantía de la tolerancia a errores en máquinas virtuales

Para proporcionar redundancia a la aplicación, recomendamos que agrupe dos máquinas virtuales o más en un conjunto de disponibilidad. Advisor identifica las máquinas virtuales que no forman parte de un conjunto de disponibilidad y recomienda moverlas a un conjunto de disponibilidad. Esta configuración garantiza que, durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual esté disponible y cumpla el Acuerdo de Nivel de Servicio de máquina virtual de Azure. Puede elegir crear un conjunto de disponibilidad de la máquina virtual o agregar la máquina virtual a uno existente.

> [!NOTE]
> Si decide crear un conjunto de disponibilidad, debe agregar al menos una máquina virtual más a él. Se recomienda que agrupe dos o más máquinas virtuales en un conjunto de disponibilidad para garantizar que al menos una de ellas esté disponible durante una interrupción.

## <a name="ensure-availability-set-fault-tolerance"></a>Garantía de la tolerancia a errores del conjunto de disponibilidad 

Para proporcionar redundancia a la aplicación, recomendamos que agrupe dos máquinas virtuales o más en un conjunto de disponibilidad. Advisor identifica conjuntos de disponibilidad que contienen una sola máquina virtual y recomienda que se agregue una o más máquinas virtuales a él. Esta configuración garantiza que, durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual esté disponible y cumpla el Acuerdo de Nivel de Servicio de máquina virtual de Azure. Puede elegir crear una máquina virtual o agregar una existente al conjunto de disponibilidad.  

## <a name="ensure-application-gateway-fault-tolerance"></a>Garantía de la tolerancia a errores de Application Gateway
Para garantizar la continuidad empresarial de aplicaciones críticas que se basan en Application Gateway, Advisor identifica instancias de Application Gateway que no están configuradas para la tolerancia a errores y recomienda acciones correctoras posibles. Advisor identifica instancias únicas de Application Gateway medianas o grandes, y recomienda que agregue al menos una instancia más. También identifica instancias múltiples de Application Gateway pequeñas y recomienda migrar a SKU medianas o grandes. Advisor recomienda estas acciones para asegurarse de que las instancias de Application Gateway estén configuradas para satisfacer los requisitos actuales del Acuerdo de Nivel de Servicio para estos recursos.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Mejora del rendimiento y la confiabilidad de los discos de máquina virtual

Advisor identifica las máquinas virtuales con discos estándar y recomienda actualizarlos a discos premium.
 
Azure Premium Storage ofrece compatibilidad con discos de alto rendimiento y latencia baja para máquinas virtuales que ejecutan cargas de trabajo intensivas de E/S. Los discos de máquinas virtuales que usan cuentas de Premium Storage almacenan datos en unidades de estado sólido (SSD). Para lograr un rendimiento óptimo en la aplicación, se recomienda migrar todos los discos de máquinas virtuales que requieran E/S por segundo elevados a Premium Storage. 

Si los discos no requieren E/S por segundo elevadas, puede limitar los costos si los mantiene en una cuenta de almacenamiento Estándar. Con la cuenta de almacenamiento Estándar, se almacenan los datos de discos de máquinas virtuales en unidades de disco duro (HDD) en lugar de SSD. Puede migrar los discos de máquinas virtuales a discos premium. Los discos premium son compatibles con la mayoría de las SKU de máquinas virtuales. Sin embargo, en algunos casos, si desea usar discos premium, es posible que deba actualizar también las SKU de máquina virtual.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Protección de los datos de máquinas virtuales contra la eliminación accidental

La configuración de la copia de seguridad de máquinas virtuales garantiza la disponibilidad de los datos críticos y ofrece protección contra la eliminación accidental o los daños.  Advisor identifica las máquinas virtuales que no tienen la copia de seguridad habilitada y recomienda habilitarla. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Asegúrese de tener acceso a los expertos en la nube de Azure cuando lo necesite

Cuando se ejecuta una carga de trabajo crítica para la empresa, es importante tener acceso al soporte técnico cuando sea necesario. Advisor identifica posibles suscripciones críticas para la empresa que no tienen soporte técnico incluido en su plan de soporte técnico y recomienda actualizar a una opción que incluya soporte técnico.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Creación de alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten

Es recomendable configurar alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten. [Azure Service Health](https://azure.microsoft.com/features/service-health/) es un servicio gratuito que ofrece una guía personalizada y soporte técnico cuando experimenta un problema con algún servicio de Azure. Advisor identifica las suscripciones que no tienen alertas configuradas y recomienda crearlas.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Configuración de puntos de conexión de Traffic Manager para lograr resistencia

Los perfiles de Traffic Manager con más de un punto de conexión experimentan una mayor disponibilidad si se produce un error en cualquier punto de conexión determinado. Colocar puntos de conexión en diferentes regiones mejora aún más la confiabilidad del servicio. Advisor identifica los perfiles de Traffic Manager donde hay solo un punto de conexión y recomienda agregar al menos uno más en otra región.

Si todos los puntos de conexión de un perfil de Traffic Manager configurado para el enrutamiento de proximidad están en la misma región, los usuarios de otras regiones pueden experimentar retrasos en la conexión. Si agrega o mueve un punto de conexión a otra región, mejorará el rendimiento global y proporcionará una mejor disponibilidad en caso de que se produzcan errores en todos los puntos de conexión de una región. Advisor identifica los perfiles de Traffic Manager configurados para el enrutamiento de proximidad en los que todos los puntos de conexión están en la misma región y recomienda agregar o mover un punto de conexión a otra región de Azure.

Si un perfil de Traffic Manager está configurado para el enrutamiento geográfico, el tráfico se enrutará a puntos de conexión en función de las regiones definidas. Si se produce un error en una región, no hay ninguna conmutación por error predefinida. Tener un punto de conexión en el que la agrupación regional está configurada en "All (World)" evitará que se quite tráfico y mejorará la disponibilidad del servicio. Advisor identifica los perfiles de Traffic Manager configurados para enrutamiento geográfico, en los que no hay ningún punto de conexión que tenga la agrupación regional configurada como "All (World)" y recomienda realizar ese cambio de configuración.

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Obtención de acceso a las recomendaciones sobre alta disponibilidad en Advisor

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

2.  En el panel de Advisor, haga clic en la pestaña **Alta disponibilidad**.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Azure Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)

