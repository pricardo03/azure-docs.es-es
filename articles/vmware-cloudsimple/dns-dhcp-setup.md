---
title: 'Azure VMware Solutions (AVS): configuración de DNS y DHCP de carga de trabajo para la nube privada de AVS'
description: Describe cómo configurar DNS y DHCP para las aplicaciones y cargas de trabajo que se ejecutan en el entorno de la nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f2a5cae868f2d8f3689f05dd9d466715ab2008a3
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024694"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-avs-private-cloud"></a>Configuración de cargas de trabajo y aplicaciones de DNS y DHCP en la nube privada de AVS

Las aplicaciones y cargas de trabajo que se ejecutan en un entorno de nube privada de AVS requieren servicios de resolución de nombres y DHCP para la búsqueda y asignación de direcciones IP. Para proporcionar estos servicios se requiere una infraestructura de DHCP y DNS adecuada. Puede configurar una máquina virtual para proporcionar estos servicios en el entorno de nube privada de AVS. 

## <a name="prerequisites"></a>Prerequisites

* Un grupo de puertos distribuidos con VLAN configurada
* Ruta configurada a servidores DNS basados en Internet o locales
* Plantilla de máquina virtual o archivo ISO para crear una máquina virtual

## <a name="linux-based-dns-server-setup"></a>Configuración del servidor DNS basado en Linux

Linux ofrece diversos paquetes para la configuración de servidores DNS. A continuación se muestra un [ejemplo de configuración de DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04) con instrucciones para configurar un servidor DNS BIND de código abierto.

## <a name="windows-based-setup"></a>Configuración basada en Windows

En estos temas de Microsoft se describe cómo configurar un servidor de Windows como un servidor DNS y como un servidor DHCP.

* [Windows Server como servidor DNS](https://docs.microsoft.com/windows-server/networking/dns/dns-top)
* [Windows Server como servidor DHCP](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)
