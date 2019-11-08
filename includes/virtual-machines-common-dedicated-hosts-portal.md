---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 262880997c6b065dc5293a18d9a07c52ac836f37
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73591430"
---
> [!IMPORTANT]
> Los hosts dedicados de Azure están actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> **Limitaciones conocidas de la versión preliminar**
> - Actualmente, los conjuntos de escalado de máquinas virtuales no se admiten en los hosts dedicados.
> - La versión preliminar inicial admite las siguientes series de máquinas virtuales: DSv3 y ESv3. 


## <a name="create-a-host-group"></a>Creación de un grupo host

Un **grupo host** es un nuevo recurso que representa una colección de hosts dedicados. Puede crear un grupo host en una región y una zona de disponibilidad, y agregarle hosts. Al planear la alta disponibilidad, hay otras opciones. Puede usar una o ambas de las dos opciones siguientes con los hosts dedicados: 
- Abarcar varias zonas de disponibilidad. En este caso, es necesario tener un grupo host en cada una de las zonas que quiera usar.
- Abarcar varios dominios de error que se asignan a bastidores físicos. 
 
En cualquier caso, es necesario proporcionar el número de dominios de error del grupo host. Si no quiere abarcar dominios de error en el grupo, use un número de dominios de error de 1. 

También puede usar zonas de disponibilidad y dominios de error a la vez. 

En este ejemplo, se creará un grupo host con una zona de disponibilidad y dos dominios de error. 


1. Abra el [portal](https://portal.azure.com).
1. Seleccione **Crear un recurso** en la esquina superior izquierda.
1. Busque el **grupo host** y, a continuación, seleccione los **grupos host (versión preliminar)** en los resultados.

    ![Resultado de la búsqueda de grupos host.](./media/virtual-machines-common-dedicated-hosts-portal/host-group.png)
1. En la página de los **grupos host (versión preliminar)** , seleccione **Crear**.
1. Seleccione la suscripción que quiere usar y, a continuación, seleccione **Crear nuevo** para crear un nuevo grupo de recursos.
1. Escriba *myDedicatedHostsRG* como **nombre** y seleccione **Aceptar**.
1. En el **nombre del grupo host**, escriba *myHostGroup*.
1. En **Ubicación**, seleccione **Este de EE.UU.**
1. Para la **zona de disponibilidad**, seleccione **1**.
1. En el **recuento de dominios de error**, seleccione **2**.
1. Seleccione **Revisar y crear** y, a continuación, espere a la confirmación.

    ![Configuración del grupo host](./media/virtual-machines-common-dedicated-hosts-portal/host-group-settings.png)
1. Cuando reciba el mensaje de **Validación superada**, seleccione **Crear** para crear el grupo host.

Solo tardará unos minutos en crear el grupo host.

## <a name="create-a-dedicated-host"></a>Creación de un host dedicado

Ahora crearemos un host dedicado en el grupo host. Además de un nombre para el host, se le pedirá que proporcione el SKU del host. El SKU del host registra la serie de máquinas virtuales admitidas, así como la generación de hardware del host dedicado.  Durante la versión preliminar, se admitirán los siguientes valores de SKU de host: DSv3_Type1 y ESv3_Type1.

Para más información sobre los precios y los SKU de host, consulte [Precios de hosts dedicados de Azure](https://aka.ms/ADHPricing).

Si establece un número de dominios de error para el grupo host, se le pedirá que especifique el dominio de error para su host.  

1. Seleccione **Crear un recurso** en la esquina superior izquierda.
1. Busque un **host dedicado** y, a continuación, seleccione los **hosts dedicados (versión preliminar)** en los resultados.

    ![Resultado de la búsqueda de grupos host.](./media/virtual-machines-common-dedicated-hosts-portal/host.png)
1. En la página de **hosts dedicados ( versión preliminar)** , seleccione **Crear**.
1. Seleccione la suscripción que quiere usar.
1. Seleccione *myDedicatedHostsRG* como **grupo de recursos**.
1. En los **detalles de la instancia**, escriba *myHost* en el **nombre** y seleccione *Este de EE. UU.* como ubicación.
1. En el **perfil de hardware**, seleccione *familia Es3 estándar: tipo 1* para la **familia de tamaños**, seleccione *myHostGrup* en el **grupo host** y, a continuación, seleccione *1* en el **dominio de error**. En el resto de los campos, deje los valores predeterminados.
1. Cuando termine, seleccione **Revisar y crear** y, a continuación, espere a la confirmación.

    ![Configuración de host](./media/virtual-machines-common-dedicated-hosts-portal/host-settings.png)
1. Cuando reciba el mensaje de **Validación superada**, seleccione **Crear** para crear el host.


