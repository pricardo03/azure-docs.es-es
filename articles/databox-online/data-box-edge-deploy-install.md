---
title: Tutorial sobre la instalación del dispositivo físico de Azure Data Box Edge | Microsoft Docs
description: El segundo tutorial para instalar Azure Data Box Edge incluye cómo desempaquetar, colocar en el bastidor y realizar el cableado del dispositivo físico.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 17602abd9dab590c806c33779cf127f9a5305f64
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48833206"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Tutorial: Instalación de Azure Data Box Edge (versión preliminar)

Este tutorial describe cómo instalar un dispositivo físico de Data Box Edge. El procedimiento de instalación incluye el desempaquetado, el montaje en el bastidor y el cableado del dispositivo. 

La instalación puede tardar unas 2 horas en completarse.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Desempaquetar el dispositivo
> * Montar el dispositivo en el bastidor
> * Cablear el dispositivo

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Los requisitos previos para instalar un dispositivo físico son los siguientes.

### <a name="for-the-data-box-edge-resource"></a>Para el recurso de Data Box Edge

Antes de comenzar, asegúrese de que:

* Ha completado todos los pasos de [Prepare the portal for Data Box Edge](data-box-edge-deploy-prep.md) (Preparación del portal para Data Box Edge).
    * Ha creado el recurso de Data Box Edge para implementar el dispositivo.
    * Ha generado la clave de activación para activar el dispositivo con el recurso de Data Box Edge.

 
### <a name="for-the-data-box-edge-physical-device"></a>Para el dispositivo físico de Data Box Edge

Antes de implementar un dispositivo:

- Asegúrese de colocar el dispositivo de forma segura sobre una superficie de trabajo plana, estable y nivelada (o similar).
- Compruebe que el sitio donde desea realizar la instalación tiene:
    - Corriente alterna estándar de una fuente independiente, o
    - Una unidad de distribución de energía (PDU) en bastidor con un sistema de alimentación ininterrumpida (UPS).
- Asegúrese de que haya disponible una ranura 1U en el bastidor en el que desea montar el dispositivo.

### <a name="for-the-network-in-the-datacenter"></a>Para la red del centro de datos

Antes de empezar:

- Revise los requisitos de red para implementar Data Box Edge y configure la red del centro de datos según dichos requisitos. Para más información, consulte [Requisitos de red de Data Box Edge](data-box-gateway-system-requirements.md#networking-requirements).
- Asegúrese de que el ancho de banda mínimo de Internet es de 20 Mbps para que el dispositivo funcione de forma óptima.


## <a name="unpack-the-device"></a>Desempaquetado del dispositivo

Este dispositivo se suministra en una única caja. Realice los pasos siguientes para desempaquetar el dispositivo. 

1. Coloque la caja en una superficie plana y nivelada.
2. Compruebe si la caja y la espuma del embalaje presentan golpes, cortes, daños por agua o cualquier otro daño evidente. Si la caja o el embalaje están muy dañados, no abra la caja. Póngase en contacto con el soporte técnico de Microsoft para ayudarle a determinar si el dispositivo está en buen estado.
3. Desempaquete la caja. Después de desempaquetar la caja, asegúrese de que dispone de:
    - Un único dispositivo contenedor Edge
    - Dos cables de alimentación
    - Kit de montaje en bastidor deslizable sin herramientas (rieles de doble cara y materiales de montaje incluidos)
4. Si no recibió alguno de los elementos enumerados anteriormente, póngase en contacto con el soporte técnico de Data Box Edge. El paso siguiente es el montaje en bastidor del dispositivo.


## <a name="rack-the-device"></a>Montaje del dispositivo en el bastidor

El dispositivo debe instalarse en un bastidor estándar de 19 pulgadas. Utilice el procedimiento siguiente para montar el dispositivo en un bastidor estándar de 19 pulgadas con postes delanteros y traseros.

> [!IMPORTANT]
> Los dispositivos de Data Box Edge deben estar montados en un bastidor para funcionar correctamente.


1. Tire del enganche frontal para desbloquear el riel interno del ensamblaje deslizante. Suelte el mecanismo de bloqueo y empuje el riel intermedio hacia dentro para retirar el riel. Los rieles interno y externo están separados ahora.

    ![Instalar los rieles de montaje en el bastidor](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Ahora puede instalar los rieles externos en las guías verticales del armario del bastidor. Para ayudarle con la orientación, las guías disponen de la marca Front y ese es el extremo que se fija en la parte delantera del receptáculo. 
    
    1. Busque los pasadores de la guía en la parte frontal y trasera del soporte de la guía. Extienda los rieles para adaptarlos a la distancia entre los postes del bastidor. Fije en primer lugar el riel externo a la parte posterior del bastidor. Ajuste el soporte de montaje trasero hasta posicionarlo en los orificios de montaje traseros del bastidor.   
    2. Mantenga pulsado el gatillo del soporte trasero hasta que queden expuestos los ganchos metálicos. Alinee e inserte en los orificios de montaje y, a continuación, suelte el gatillo.
    3. Alinee el soporte frontal con el orificio de montaje.
    4. Ahora, se debe fijar el soporte frontal al bastidor. Opcionalmente, se pueden usar tornillos M5 X 10L para asegurar los rieles a los postes si es necesario. 

    ![Instalar los rieles de montaje en el bastidor](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. Para fijar el riel interno al chasis, asegúrese de que los orificios del riel interno están alineados con los pasadores de posición de los laterales del chasis. Asegúrese de que las cabezas de los pasadores de posición del chasis sobresalen por los orificios del riel interno. Tire del riel hacia la parte frontal del chasis hasta que el riel se bloquee en su posición con un clic audible. Repita la misma operación con el otro carril interno. Inserte el chasis con el riel interno en la guía deslizante para completar la instalación del bastidor.

    ![Instalar los rieles de montaje en el bastidor](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Cableado del dispositivo

Los siguientes procedimientos describen cómo realizar el cableado del dispositivo de Edge para la conexión a la corriente y a la red.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar a pasar los cables del dispositivo, necesitará:

- El dispositivo físico de Edge, desempaquetado y montado en el bastidor.
- Dos cables de alimentación. 
- Dos cables de red RJ-45 de 1-GbE y cuatro cables de cobre SFP+ de 25-GbE.
- Acceso a dos unidades de distribución de energía (recomendable).

El dispositivo de Edge tiene 8 SSD de NVMe. El panel frontal incluye también indicadores LED de estado y botones de alimentación. El dispositivo incluye dos fuentes de alimentación (PSU) redundantes en la parte posterior. El dispositivo tiene seis interfaces de red: dos interfaces de 1 Gbps y cuatro de 25 Gbps. El dispositivo cuenta con un controlador de administración de placa base (BMC). Identifique los distintos puertos de la placa posterior del dispositivo.
 
  ![Placa posterior de un dispositivo cableado](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Realice los pasos siguientes para realizar el cableado de los cables de alimentación y de red del dispositivo.

1. Conecte los cables de alimentación a cada una de las fuentes de alimentación del receptáculo. Para garantizar una alta disponibilidad, instale y conecte ambas fuentes de alimentación a distintas tomas de alimentación.
2. Conecte los cables de alimentación a las unidades de distribución de energía (PDU) del bastidor. Asegúrese de que las dos PSU usen tomas de alimentación independientes.
3. Conecte el PUERTO 1 de la interfaz de red de 1-GbE al equipo que se usa para configurar el dispositivo físico. El PUERTO 1 es la interfaz de administración dedicada.
4. Conecte el PUERTO 2 de la interfaz de red de 1-GbE mediante los cables de red RJ-45 a la red del centro de datos o a Internet. 
5. Conecte los PUERTOS 3, 4, 5 y 6 de las interfaces de red de 25-GbE mediante los cables de cobre SFP+ a la red del centro de datos o a Internet. 

> [!NOTE]
> - Al menos una de las interfaces de red de datos (PUERTOS 2, 3, 4, 5 o 6) debe estar conectada a Internet (para la conexión a Azure). 
> - Es recomendable que use una interfaz de red de 25-GbE como la de los PUERTOS 3, 4, 5 o 6 para enviar datos a Azure. 
> - El dispositivo de Edge debe estar conectado a la red del centro de datos para poder ingerir datos desde los servidores de origen de datos.  


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Edge, como:

> [!div class="checklist"]
> * Desempaquetado del dispositivo
> * Montaje del dispositivo en el bastidor
> * Cableado del dispositivo

Pase al siguiente tutorial para aprender a conectar, configurar y activar el dispositivo.

> [!div class="nextstepaction"]
> [Conexión y configuración de Data Box Edge](./data-box-edge-deploy-connect-setup-activate.md)


