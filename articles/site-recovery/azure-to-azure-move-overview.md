---
title: Traslado de máquinas virtuales de Azure IaaS a otra región de Azure mediante el servicio Azure Site Recovery | Microsoft Docs
description: Use Azure Site Recovery para mover máquinas virtuales de IaaS de Azure de una región de Azure a otra.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 0d0009c833c313b5416998502601285e5b710a8d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112553"
---
# <a name="move-azure-vms-to-another-region"></a>Traslado de máquinas virtuales de Azure a otra región

Azure está creciendo ampliamente junto con la base de clientes y está agregando compatibilidad para nuevas regiones con demandas crecientes. También hay nuevas funcionalidades que se agregan mensualmente en todos los servicios. Debido a esto, hay momentos en los que querría trasladar las máquinas virtuales a una región diferente o a zonas de disponibilidad para aumentar la disponibilidad.

Este documento recorre los distintos escenarios en los que le gustaría trasladar las máquinas virtuales y es una guía sobre cómo debe configurarse la arquitectura en el destino para lograr una mayor disponibilidad. 
> [!div class="checklist"]
> * [Por qué trasladar máquinas virtuales de Azure](#why-would-you-move-azure-vms)
> * [Procedimiento para trasladar máquinas virtuales de Azure](#how-to-move-azure-vms)
> * [Arquitecturas típicas](#typical-architectures-for-a-multi-tier-deployment)
> * [Traslado de máquinas virtuales tal cual están a una región de destino](#move-azure-vms-to-another-region)
> * [Traslado de máquinas virtuales para aumentar la disponibilidad](#move-vms-to-increase-availability)


## <a name="why-would-you-move-azure-vms"></a>Por qué trasladar máquinas virtuales de Azure

Los clientes trasladan las máquinas virtuales por las razones siguientes:-

- Si ya se ha implementado en una región y se ha agregado compatibilidad con una nueva, que está más cerca de los usuarios finales de la aplicación o servicio, entonces querrá **trasladar las máquinas virtuales tal cual están a la nueva región** para reducir la latencia. El mismo enfoque se aplica si desea consolidar las suscripciones o si existen reglas de gobierno o de organización que requieren el traslado. 
- Si la máquina virtual se ha implementado como una máquina virtual de instancia única o como parte de un conjunto de disponibilidad y desea aumentar los Acuerdos de Nivel de Servicio (SLA) de disponibilidad, puede **trasladar las máquinas virtuales a una zona de disponibilidad**. 

## <a name="how-to-move-azure-vms"></a>Procedimiento para trasladar máquinas virtuales de Azure
El traslado de máquinas virtuales implica los pasos siguientes:

1. Verificar los requisitos previos 
2. Preparación de las máquinas virtuales de origen 
3. Preparación de la región de destino 
4. Copie los datos a la región de destino: utilice la tecnología de replicación de Azure Site Recovery para copiar datos de la máquina virtual de origen a la región de destino.
5. Pruebe la configuración: Una vez finalizada la replicación, pruebe la configuración mediante la realización de una conmutación por error de prueba a una red que no sea de producción.
6. Realización del traslado 
7. Descarte de los recursos en la región de origen 


> [!IMPORTANT]
> Actualmente Azure Site Recovery permite trasladar máquinas virtuales de una región a otra y no admite el traslado dentro de una región. 

> [!NOTE]
> En la documentación de cada uno de los escenarios se ofrece una guía detallada de estos pasos, tal como se menciona a continuación

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Arquitecturas típicas para una implementación de varios niveles
A continuación se describen las arquitecturas de implementación más comunes que adoptan los clientes para una aplicación de varios niveles en Azure. El ejemplo que estamos viendo aquí es de una aplicación de tres niveles con una IP pública. Cada uno de los niveles (web, aplicación y base de Datos) tienen dos máquinas virtuales cada uno, y están conectados por un equilibrador de carga a los otros niveles. El nivel de base de datos tiene replicación SQL Always ON entre las máquinas virtuales para alta disponibilidad.

1.  **Máquinas virtuales de instancia única implementadas en varios niveles**: cada máquina virtual en un nivel se configura como una máquina virtual de instancia única, conectada por equilibradores de carga a los otros niveles. Esta es la configuración más sencilla que adoptan los clientes.

       ![Máquinas virtuales únicas](media/move-vm-overview/regular-deployment.PNG)

2. **Máquinas virtuales de cada nivel implementadas por conjunto de disponibilidad**: cada máquina virtual de un nivel se configura en un conjunto de disponibilidad. Los [conjuntos de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) garantizan que las máquinas virtuales implementadas en Azure se distribuyan entre varios nodos de hardware aislados en un clúster. De este modo, se asegura de que, si se produce un error de hardware o software en Azure, solo un subconjunto de las máquinas virtuales se verá afectado y que la solución seguirá disponible y en funcionamiento. 
   
      ![Conjunto de disponibilidad](media/move-vm-overview/AVset.PNG)

3. **Máquinas virtuales de cada nivel implementadas por conjunto de disponibilidad**: cada máquina virtual de un nivel se configura en [zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview). Una zona de disponibilidad de una región de Azure es una combinación de un dominio de error y un dominio de actualización. Por ejemplo, si crea tres o más máquinas virtuales en tres zonas de una región de Azure, las máquinas virtuales se distribuyen eficazmente en tres dominios de error y tres dominios de actualización. La plataforma Azure reconoce esta distribución entre dominios de actualización para asegurarse de que las máquinas virtuales de distintas zonas no se actualizan al mismo tiempo.

      ![Implementación de zona](media/move-vm-overview/zone.PNG)



 ## <a name="move-vms-as-is-to-a-target-region"></a>Traslado de máquinas virtuales tal cual están a una región de destino

En función de las [arquitecturas](#typical-architectures-for-a-multi-tier-deployment) anteriormente mencionadas, aquí se muestra cómo se verán las implementaciones una vez que realice el movimiento tal y como está a la región de destino.


1. **Máquinas virtuales de instancia única implementadas en varios niveles** 

     ![single-zone.PNG](media/move-vm-overview/single-zone.PNG)

2. **Máquinas virtuales de cada nivel implementadas en un conjunto de disponibilidad**

     ![crossregionAset.PNG](media/move-vm-overview/crossregionAset.PNG)


3. **Máquinas virtuales de cada nivel implementadas en una zona de disponibilidad**
      

     ![AzoneCross.PNG](media/move-vm-overview/AzoneCross.PNG)

## <a name="move-vms-to-increase-availability"></a>Traslado de máquinas virtuales para aumentar la disponibilidad

1. **Máquinas virtuales de instancia única implementadas en varios niveles** 

     ![single-zone.PNG](media/move-vm-overview/single-zone.PNG)

2. **Máquinas virtuales de cada nivel implementadas en un conjunto de disponibilidad**: puede elegir configurar para colocar las máquinas virtuales en un conjunto de disponibilidad en zonas de disponibilidad independientes, cuando elija habilitar la replicación para la máquina virtual mediante Azure Site Recovery. El SLA de disponibilidad será del 99,9 % una vez finalizada la operación de traslado.

     ![aset-Azone.PNG](media/move-vm-overview/aset-Azone.PNG)


## <a name="next-steps"></a>Pasos siguientes

En este documento, va a leer la guía general para trasladar las máquinas virtuales. Para conocer la ejecución paso a paso, lea más sobre este tema:


> [!div class="nextstepaction"]
> * [Traslado de máquinas virtuales de Azure a otra región](azure-to-azure-tutorial-migrate.md)

> * [Traslado de máquinas virtuales de Azure a zonas de disponibilidad](move-azure-VMs-AVset-Azone.md)

