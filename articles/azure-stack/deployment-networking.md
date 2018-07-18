---
title: Tráfico de red de implementación de Azure Stack | Microsoft Docs
description: En este artículo se describe lo que puede esperar acerca de los procesos de red de implementación de Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: b808875e66e867b84e2971c6a5bd031d108d003b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652618"
---
# <a name="about-deployment-network-traffic"></a>Acerca del tráfico de red de implementación
Comprender cómo fluye el tráfico de red durante la implementación de Azure Stack es fundamental para garantizar una correcta implementación. Este artículo le guía por el tráfico de red esperado durante el proceso de implementación para ayudarle a comprender lo que puede anticipar.

En esta ilustración se muestran todos los componentes y las conexiones que intervienen en el proceso de implementación:

![Topología de red de implementación de Azure Stack](media/deployment-networking/figure1.png)

> [!NOTE]
> En este artículo se describen los requisitos para una implementación conectada; para aprender sobre otros métodos de implementación, consulte [Modelos de conexión de implementación de Azure Stack](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>La máquina virtual de implementación
La solución de Azure Stack incluye un grupo de servidores que se usan para hospedar componentes de Azure Stack, y un servidor adicional llamado Host de ciclo de vida de hardware (HLH). Este servidor se usa para implementar y administrar el ciclo de vida de la solución y hospeda la máquina virtual de implementación (DVM) durante la implementación.

## <a name="deployment-requirements"></a>Requisitos de implementación
Antes de iniciar la implementación, hay algunos requisitos mínimos que puede validar su OEM para asegurarse de que la operación se realiza correctamente. Comprender estos requisitos le ayudará a preparar el entorno y a asegurarse de que la validación se realiza correctamente. Los requisitos son:

-   [Certificates](azure-stack-pki-certs.md)
-   [Suscripción de Azure](https://azure.microsoft.com/free/?b=17.06)
-   Acceso a Internet
-   DNS
-   NTP

> [!NOTE]
> Este artículo se centra en los tres últimos requisitos. Para más información sobre los dos primeros, consulte los vínculos anteriores.

## <a name="deployment-network-traffic"></a>Tráfico de red de implementación
La DVM está configurada con una dirección IP de la red BMC y requiere acceso de red a Internet. Aunque no todos los componentes de la red BMC requieren enrutamiento externo o acceso a Internet, algunos componentes específicos del OEM que usan direcciones IP de esta red podrían también necesitarlo.

Durante la implementación, la DVM se autentica en Azure Active Directory (Azure AD) mediante una cuenta de Azure desde su suscripción. Para ello, la DVM necesita acceso a través de Internet a una lista de direcciones URL y puertos específicos. Puede encontrar la lista completa en la documentación [Publicación de puntos de conexión](azure-stack-integrate-endpoints.md). La DVM usará un servidor DNS para reenviar las solicitudes de DNS que realizan los componentes internos a las direcciones URL externas. El DNS interno reenvía estas solicitudes a la dirección del reenviador DNS que se proporciona al OEM antes de la implementación. Lo mismo puede decirse del servidor NTP: se necesita un servidor horario confiable para mantener la coherencia y la sincronización de la hora en todos los componentes de Azure Stack.

El acceso a Internet que requiere la DVM durante la implementación es solo saliente; no se realizan llamadas entrantes durante este proceso. Tenga en cuenta que esta máquina usa su IP como origen y que Azure Stack no admite configuraciones de proxy. Por lo tanto, si es necesario, debe proporcionar un proxy o NAT transparentes para el acceso a Internet. Una vez finalizada la implementación, toda la comunicación entre Azure y Azure Stack se realiza mediante la red externa usando direcciones IP virtuales públicas.

Las configuraciones de red en los modificadores de Azure Stack contienen listas de control de acceso (ACL) que restringen el tráfico entre determinados orígenes y destinos de red. La DVM es el único componente con acceso sin restricciones; incluso el HLH es muy restrictivo. Puede preguntar a los OEM sobre las opciones de personalización para facilitar la administración y el acceso desde sus redes. Debido a estas ACL, es importante evitar cambiar las direcciones de servidor DNS y NTP durante la implementación. Si lo hace, deberá volver a configurar todos los modificadores de la solución.

Después de finalizar la implementación, los componentes del sistema seguirán usando directamente las direcciones de servidor DNS y NTP proporcionadas. Por ejemplo, si comprueba las solicitudes DNS una vez finalizada la implementación, el origen cambiará de la dirección IP de DVM a una dirección del intervalo de la red externa.

Una vez que Azure Stack se ha implementado correctamente, su asociado OEM podría usar la DVM en otras tareas posteriores a la implementación. Sin embargo, cuando se completen todas las tareas de implementación y configuraciones posteriores a la implementación, el OEM debe quitar y eliminar la DVM del HLH.

## <a name="next-steps"></a>Pasos siguientes
[Validación del registro de Azure](azure-stack-validate-registration.md)
