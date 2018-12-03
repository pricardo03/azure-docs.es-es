---
title: ¿Qué es Dedicated HSM? | Microsoft Docs
description: Azure Dedicated HSM proporciona funcionalidades de almacenamiento de claves dentro de Azure que cumplen con la certificación FIPS 140-2 nivel 3
services: dedicated-hsm
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 11/26/2018
ms.author: barclayn
ms.openlocfilehash: 92d77ec886a0f37c28f5e3031a7e14f63299c8aa
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427121"
---
# <a name="what-is-dedicated-hsm"></a>¿Qué es Dedicated HSM?

Azure Dedicated HSM proporciona un almacenamiento de claves criptográfico en Azure que cumple con los más estrictos requisitos de seguridad. Dedicated HSM es la solución idónea para los clientes que necesitan dispositivos validados con la certificación FIPS 140-2 nivel 3 y un control completo y exclusivo de la aplicación HSM. Los dispositivos HSM se implementan globalmente en varias regiones de Azure y se pueden aprovisionar fácilmente como un par de dispositivos y configurar para obtener una alta disponibilidad. También se pueden aprovisionar los dispositivos HSM de las regiones para protegerse frente a conmutaciones por error en el nivel regional. Microsoft ha lanzado el servicio Dedicated HSM mediante la aplicación [SafeNet Luna Network HSM 7 (Modelo A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) de Gemalto. Este dispositivo ofrece los mayores niveles de rendimiento y opciones de integración criptográficas. Cuando se aprovisionan, los HSM se conectan directamente a la red virtual de un cliente y las aplicaciones locales y herramientas de administración pueden acceder a ellos mediante la configuración de conectividad VPN de punto a sitio o de sitio a sitio. Los clientes adquirirán el software y la documentación para configurar y administrar los dispositivos HSM desde el portal de soporte técnico de Gemalto.

## <a name="why-use-azure-dedicated-hsm"></a>Motivos para usar Azure Dedicated HSM

### <a name="fips-140-2-level-3-compliance"></a>Cumplimiento de la norma FIPS 140-2 nivel 3

Muchas organizaciones tienen estrictas regulaciones normativas del sector que dictan que el almacenamiento de claves criptográfico debe cumplir con los requisitos de [FIPS 140-2 nivel 3](https://csrc.nist.gov/publications/detail/fips/140/2/final). El servicio multiinquilino Azure Key Vault de Microsoft solo proporciona actualmente la certificación FIPS 140-2 nivel 2. Azure Dedicated HSM viene a dar respuesta a una necesidad real que tiene el sector de los servicios financieros, las agencias gubernamentales y otros destinatarios que deben cumplir con los requisitos de la certificación FIPS 140-2 nivel 3.

### <a name="single-tenant-devices"></a>Dispositivos de inquilino único

Muchos de nuestros clientes requieren un único inquilino en el dispositivo de almacenamiento criptográfico. El servicio Azure Dedicated HSM permite el aprovisionamiento de un dispositivo físico desde uno de los centros de datos distribuidos globalmente de Microsoft. Una vez aprovisionado para un cliente, solo ese cliente podrá acceder al dispositivo.

### <a name="full-administrative-control"></a>Control administrativo completo

Además de dispositivos de un solo inquilino, muchos clientes requieren un control administrativo completo y acceso exclusivo para fines administrativos. Una vez aprovisionado, solo ese cliente tiene acceso de nivel administrativo o de aplicación al dispositivo. Microsoft no tendrá ningún control administrativo después del primer acceso del cliente, lo cual requiere el cambio de contraseña. A partir de ese punto, el cliente es un único inquilino verdadero con control administrativo completo y capacidad de administración de aplicaciones. Microsoft conserva un acceso en el nivel de supervisión (no con un rol de administrador) para los datos de telemetría a través de una conexión al puerto serie que abarca indicadores del hardware como temperatura, estado del suministro eléctrico y estado del ventilador. El cliente es libre de deshabilitar esta opción si es necesario, pero, si lo hace, no recibirá las alertas de estado proactivas de Microsoft.

### <a name="high-performance"></a>Alto rendimiento

Se seleccionó el dispositivo Gemalto para este servicio por su amplia gama de compatibilidad con algoritmos criptográficos, la variedad de sistemas operativos que admite y una amplia compatibilidad con API. El modelo específico implementado ofrece un rendimiento excelente con 10 000 operaciones por segundo para RSA-2048. Admite 10 particiones que se pueden usar para las instancias de aplicaciones únicas. Se trata de un dispositivo de baja latencia, alta capacidad y alto rendimiento.

### <a name="unique-cloud-based-offering"></a>Oferta única basada en la nube

Microsoft ha detectado una necesidad concreta en un conjunto exclusivo de clientes y es el único proveedor de servicios en la nube que ofrece un servicio HSM que cumple con la certificación FIPS 140-2 nivel 3 y que ofrece una integración tan amplia de aplicaciones basadas en la nube y locales.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>¿Es Azure Dedicated HSM adecuado para usted?

Azure Dedicated HSM es un servicio especializado que permite cumplir los requisitos únicos de un tipo específico de organización a gran escala. Como resultado, es de esperar que la mayoría de los clientes de Azure no se encuadren en el perfil de cliente que usará este servicio. Muchas descubrirán que el servicio Azure Key Vault es más adecuado y más rentable. Para ayudarle a decidir la adecuación para sus requisitos hemos identificado los siguientes criterios.

### <a name="best-fit"></a>Mejor adecuación

Es el más adecuado para escenarios de "migración mediante lift-and-shift" que requieren un acceso directo y exclusivo a los dispositivos HSM. Algunos ejemplos son:

- Migración de aplicaciones locales a Azure Virtual Machines
- Migración de aplicaciones de Amazon AWS EC2 a Azure Virtual Machines que usan el servicio AWS Cloud HSM Classic (Amazon no ofrece este servicio para los nuevos clientes)
- Ejecución de un software empaquetado en Azure Virtual Machines como Apache/Ngnix SSL Offload, Oracle TDE y ADCS

### <a name="not-a-fit"></a>No adecuado para

Los servicios en la nube de Microsoft que admiten el cifrado con claves administradas por los clientes (como las claves de cliente de Azure Information Protection, Azure Data Lake Storage, Azure Storage, Azure SQL y Office 365) no están integrados con Azure Dedicated HSM.

### <a name="it-depends"></a>Depende

Muchos escenarios dependen de una combinación posiblemente compleja de requisitos y qué compromisos se pueden hacer o no. Un ejemplo es el requisito de la certificación FIPS 140-2 nivel 3 que, se exige a menudo, y para el que Dedicated HSM es actualmente la única opción.  Si estos requisitos obligatorios no son importantes, la decisión se tomaría entre Azure Key Vault y Dedicated HSM según la evaluación de una combinación de requisitos. Algunos ejemplos son:

- Nuevo código que se ejecuta en una máquina virtual de Azure del cliente
- TDE de SQL Server en una máquina virtual de Azure
- Cifrado en el lado de cliente de Azure Storage
- SQL Server y Azure SQL Database Always Encrypted

## <a name="next-steps"></a>Pasos siguientes

Teniendo en cuenta la naturaleza altamente especializada de este servicio, se recomienda que algunos de los conceptos claves que se encuentran en este conjunto de documentación se comprendan por completo e, igualmente, que se comprenda totalmente la información sobre precios, soporte técnico y acuerdos de nivel de servicio y, posteriormente, se puede realizar un tutorial que está disponible para facilitar el aprovisionamiento de HSM en un entorno de red virtual ya existente. Las [guías de integración de Gemalto](https://safenet.gemalto.com/partners/microsoft/) y las guías de procedimientos para decidir la arquitectura de implementación son también un excelente recurso.

* [Alta disponibilidad](high-availability.md)
* [Seguridad física](physical-security.md)
* [Redes](networking.md)
* [Compatibilidad](supportability.md)
* [Supervisión](monitoring.md)
