---
title: Tipos de cuota en Azure Stack | Microsoft Docs
description: Repase los diferentes tipos de cuota disponibles para los servicios y recursos de Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2018
ms.author: sethm
ms.reviewer: xiaofmao
ms.openlocfilehash: 3c0ab236dd6fce10be0a50c435f04517e14c1387
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077602"
---
# <a name="quota-types-in-azure-stack"></a>Tipos de cuota en Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Las [cuotas](azure-stack-plan-offer-quota-overview.md#plans) definen los límites de recursos que puede aprovisionar o consumir una suscripción de usuario. Por ejemplo, una cuota podría permitir que un usuario creara hasta cinco máquinas virtuales. Cada recurso puede tener sus propios tipos de cuotas.

## <a name="compute-quota-types"></a>Tipos de cuota de proceso 
| **Tipo** | **Valor predeterminado** | **Descripción** |
| --- | --- | --- |
| Número máximo de máquinas virtuales | 20 | El número máximo de máquinas virtuales que puede crear una suscripción en esta ubicación. |
| Número máximo de núcleos de máquinas virtuales | 50 | El número máximo de núcleos que puede crear una suscripción en esta ubicación (por ejemplo, una máquina virtual A3 tiene cuatro núcleos). |
| Número máximo de conjuntos de disponibilidad | 10 | El número máximo de conjuntos de disponibilidad que se pueden crear en esta ubicación. |
| Número máximo de conjuntos de escalado de máquinas virtuales | 20 | El número máximo de conjuntos de escalado de máquinas virtuales que se pueden crear en esta ubicación. |

## <a name="storage-quota-types"></a>Tipos de cuotas de almacenamiento 
| **Elemento** | **Valor predeterminado** | **Descripción** |
| --- | --- | --- |
| Capacidad máxima (GB) |500 |Capacidad de almacenamiento total que puede consumir una suscripción en esta ubicación. |
| Número total de cuentas de almacenamiento |20 |El número máximo de cuentas de almacenamiento que puede crear una suscripción en esta ubicación. |

> [!NOTE]  
> Puede que el proceso tarde hasta dos horas antes de aplicar una cuota de almacenamiento.


## <a name="network-quota-types"></a>Tipos de cuota de red
| **Elemento** | **Valor predeterminado** | **Descripción** |
| --- | --- | --- |
| Número máximo de direcciones IP públicas |50 |El número máximo de direcciones IP públicas que puede crear una suscripción en esta ubicación. |
| Número máximo de redes virtuales |50 |El número máximo de redes virtuales que puede crear una suscripción en esta ubicación. |
| Número máximo de puertas de enlace de red virtual |1 |El número máximo de puertas de enlace de red virtual (puertas de enlace de VPN) que puede crear una suscripción en esta ubicación. |
| Número máximo de conexiones de red |2 |El número máximo de conexiones de red (punto a punto o sitio a sitio) que puede crear una suscripción en todas las puertas de enlace de red virtual de esta ubicación. |
| Número máximo de equilibradores de carga |50 |El número máximo de equilibradores de carga que puede crear una suscripción en esta ubicación. |
| Nº máx. NIC |100 |El número máximo de interfaces de red que puede crear una suscripción en esta ubicación. |
| Número máximo de grupos de seguridad de red |50 |El número máximo de grupos de seguridad de red que puede crear una suscripción en esta ubicación. |

## <a name="view-an-existing-quota"></a>Visualización de una cuota existente
1. En el panel predeterminado del portal de administración, busque el icono **Proveedores de recursos**.
2. Seleccione el servicio con la cuota que desea ver como **Compute** o **Storage**.
3. Seleccione **Cuotas** y luego seleccione la cuota que desea ver.


## <a name="edit-a-quota"></a>Edición de una cuota  
Puede elegir editar la configuración original de una cuota en lugar de [utilizar un plan complementario](create-add-on-plan.md). Al editar una cuota, la nueva configuración se aplica automáticamente de forma global a todos los planes que utilizan esa cuota y a todas las suscripciones existentes que usan esos planes. La edición de una cuota es diferente a cuando se utiliza un plan complementario para proporcionar una cuota modificada a la que un usuario decide suscribirse. 

### <a name="to-edit-a-quota"></a>Para editar una cuota  
1. En el panel predeterminado del portal de administración, busque el icono **Proveedores de recursos**.
2. Seleccione el servicio con la cuota que desea ver como **Compute**, **Network** o **Storage**.
3. A continuación seleccione **Cuotas** y luego la cuota que desea cambiar.
4. En el panel **Establecer cuotas**, edite los valores y luego seleccione **Guardar**. 

Los nuevos valores para la cuota se aplican globalmente a todos los planes que utilizan la cuota modificada y a todas las suscripciones existentes que usan esos planes. 



## <a name="next-steps"></a>Pasos siguientes

- [Más información acerca de planes, ofertas y cuotas.](azure-stack-plan-offer-quota-overview.md)
- [Crear cuotas durante la creación de un plan.](azure-stack-create-plan.md)
