---
title: Infraestructura y conectividad con SAP HANA en Azure (Instancias grandes) | Microsoft Docs
description: Configuración de la infraestructura de conectividad necesaria para usar SAP HANA en Azure (Instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c61dffc6fd9d0c65f5e925daebdf2a02cfb5d6ba
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161363"
---
# <a name="sap-hana-large-instances-deployment"></a>Implementación de SAP HANA (instancias grandes) 

Una vez finalizada la compra de SAP HANA en Azure (Instancias grandes) con el equipo de cuentas de empresa de Microsoft, Microsoft necesita la siguiente información para implementar unidades de HANA (Instancias grandes):

- Nombre del cliente
- Información del contacto de la empresa (incluida la dirección de correo electrónico y el número de teléfono)
- Información del contacto técnico (incluida la dirección de correo electrónico y el número de teléfono)
- Información del contacto técnico de red (incluida la dirección de correo electrónico y el número de teléfono)
- Región de implementación de Azure (oeste de EE. UU., este de EE. UU., este de Australia, sudeste de Australia, Europa Occidental y Europa del Norte a partir de julio de 2017)
- Confirmación de la SKU de SAP HANA en Azure (Instancias grandes) (configuración)
- Como se ha detallado en el documento Información general y arquitectura de HANA (Instancias grandes), para cada región de Azure en la que se implemente:
    - Un intervalo de direcciones IP /29 para conexiones ER-P2P para conectar redes virtuales de Azure con HANA (Instancias grandes)
    - Un bloque CIDR /24 usado para el grupo de direcciones IP de servidor de HANA (Instancias grandes)
- Los valores del intervalo de direcciones IP usados en el atributo Espacio de direcciones de red virtual de cada red virtual de Azure que se conecta a HANA (Instancias grandes)
- Datos para cada sistema Instancias grandes de HANA:
  - Nombre de host deseado, preferiblemente el nombre de dominio completo.
  - Dirección IP deseada para la unidad de HANA (Instancias grandes) fuera del intervalo de direcciones del grupo de direcciones IP de servidor. Tenga en cuenta que las primeras 30 direcciones IP del intervalo de direcciones del grupo de direcciones IP de servidor están reservadas para uso interno en HANA (Instancias grandes)
  - Nombre del SID de SAP HANA para la instancia de SAP HANA (necesario para crear los volúmenes de disco necesarios relacionados con SAP HANA). El SID de HANA se necesita para crear los permisos para sidadm en los volúmenes NFS, que se están conectando a la unidad de HANA (Instancias grandes). También se usa como uno de los componentes del nombre de los volúmenes de disco que se montan. Si quiere ejecutar más de una instancia de HANA en la unidad, debe enumerar varios SID de HANA. Cada uno obtiene un conjunto independiente de volúmenes asignados.
  - El identificador de grupo que tiene el usuario sidadm en el sistema operativo Linux es necesario para crear los volúmenes de disco necesarios relacionados con SAP HANA. La instalación de SAP HANA normalmente crea el grupo sapsys con un identificador de grupo de 1001. El usuario sidadm forma parte de ese grupo.
  - El identificador de usuario que tiene el usuario sidadm en el sistema operativo Linux es necesario para crear los volúmenes de disco necesarios relacionados con SAP HANA. Si está ejecutando varias instancias de HANA en la unidad, debe enumerar todos los usuarios <sid>adm 
- Identificador de la suscripción de Azure a la que SAP HANA en Azure (Instancias grandes) vaya a estar directamente conectado. Este identificador de suscripción hace referencia a la suscripción de Azure, que se va a cargar con las unidades de HANA (Instancias grandes).

Después de proporcionar la información, Microsoft aprovisiona SAP HANA en Azure (Instancias grandes) y devuelve la información necesaria para vincular las redes virtuales de Azure a HANA Instancias grandes y para acceder a las unidades de HANA Instancias grandes.

Antes de leer este artículo, familiarícese con [términos comunes de instancias grandes de HANA](hana-know-terms.md) y las [SKU de instancias grandes de HANA](hana-available-skus.md).

Puede usar la siguiente secuencia para conectarse a instancias grandes de HANA una vez implementada por Microsoft:

1. [Conexión de las máquinas virtuales de Azure a HANA (instancias grandes)](hana-connect-azure-vm-large-instances.md)
2. [Conexión de una red virtual a ExpressRoute de instancias grandes de HANA](hana-connect-vnet-express-route.md)
3. [Requisitos de red adicionales (opcional)](hana-additional-network-requirements.md)

**Pasos siguientes**

- Consulte[Conexión de máquinas virtuales de Azure a instancias grandes de HANA](hana-connect-azure-vm-large-instances.md).
- Consulte [Conexión de una red virtual a ExpressRoute de HANA (instancias grandes)](hana-connect-vnet-express-route.md).