---
title: Red del consorcio de Hyperledger Fabric en Azure
description: Plantilla de la solución para implementar y configurar una red del consorcio de Hyperledger Fabric
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/23/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: caleteet
manager: femila
ms.openlocfilehash: ce1afbd5499e798888e77f52d7b652e7e1f548fc
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005814"
---
# <a name="hyperledger-fabric-consortium-network"></a>Red del consorcio de Hyperledger Fabric

Puede usar la plantilla de la solución del consorcio de Hyperledger Fabric para implementar y configurar una red del consorcio de Hyperledger Fabric en Azure.

Después de leer este artículo, habrá aprendido lo siguiente:

- Conocimientos de trabajo de cadena de bloques, Hyperledger Fabric y arquitecturas de red de consorcio más complicadas
- Obtenga información sobre cómo implementar y configurar una red del consorcio de Hyperledger Fabric desde Azure Portal.

## <a name="about-blockchain"></a>Acerca de la cadena de bloques

Si no está familiarizado con la comunidad de cadena de bloques, esta plantilla de solución es una oportunidad excelente para conocer la tecnología de una manera sencilla y configurable en Azure. La cadena de bloques es la tecnología que se esconde detrás de Bitcoin; sin embargo, es mucho más que un habilitador de una moneda virtual. Es un compuesto de bases de datos existentes, sistemas distribuidos y tecnologías de cifrado que permite el cálculo seguro de varias partes con garantías en torno a inmutabilidad, verificabilidad, auditabilidad y resiliencia a los ataques. Diferentes protocolos emplean diferentes mecanismos para proporcionar estos atributos. [Hyperledger Fabric](https://github.com/hyperledger/fabric) es uno de estos protocolos.

## <a name="consortium-architecture-on-azure"></a>Arquitectura de consorcio en Azure

Para habilitar Hyperledger Fabric en Azure, hay dos tipos de implementación principales que son compatibles. Estas implementaciones están diseñadas para adaptarse a diferentes topologías, en función del destino deseado.

- **Máquina virtual única, servidor de desarrollador**: este tipo de implementación está diseñada como un entorno de desarrollo para compilar y probar soluciones creadas en Hyperledger Fabric.
- **Varias máquinas virtuales, implementación de escalabilidad horizontal**: este tipo de implementación está diseñada para entornos que modelan un consorcio de diferentes participantes que se usa como un entorno compartido.

En cualquiera de las implementaciones, los bloques de creación que se convierten en el núcleo de Hyperledger Fabric son los mismos.  Las diferencias entre las implementaciones radican en cómo estos componentes se escalan horizontalmente.

- **Nodos de entidad de certificación**: un nodo que ejecuta la entidad de certificación para generar certificados que se usan para las entidades en la red.
- **Nodos solicitantes**: un nodo que ejecuta el servicio de comunicación que implementa una garantía de entrega, como difusión total de pedidos o transacciones atómicas.
- **Nodos del mismo nivel**: un nodo que confirma las transacciones y mantiene el estado y una copia del libro de contabilidad distribuida.
- **Nodos de CouchDB**: un nodo que puede ejecutar el servicio de CouchDB y que puede almacenar la base de datos de estado y proporcionar consultas enriquecidas de datos chaincode, con la expansión de pares clave-valor sencillos a un almacenamiento tipo JSON.

### <a name="single-virtual-machine-architecture"></a>Arquitectura de máquina virtual única

Como se mencionó anteriormente, la arquitectura de máquina virtual única se creó para que los desarrolladores dispongan de un servidor de poca superficie para desarrollar aplicaciones. Todos los contenedores mostrados se ejecutan en una máquina virtual única. El servicio de pedidos usa [SOLO](https://github.com/hyperledger/fabric/tree/master/orderer) para esta configuración. Esta configuración *no* es un servicio de pedidos tolerante a errores, pero está diseñada para ser ligera para fines de desarrollo.

![Arquitectura de máquina virtual única](./media/hyperledger-fabric-consortium-blockchain/hlf-single-arch.png)

### <a name="multiple-virtual-machine-architecture"></a>Arquitectura de varias máquinas virtuales

La arquitectura de escalabilidad horizontal con varias máquinas virtuales se creó con alta disponibilidad y escalabilidad de cada componente en el núcleo. Esta arquitectura es mucho más adecuada para implementaciones a nivel de producción.

![Arquitectura de varias máquinas virtuales](./media/hyperledger-fabric-consortium-blockchain/hlf-multi-arch.png)

## <a name="getting-started"></a>Introducción

Para comenzar, necesita una suscripción a Azure que pueda admitir la implementación de varias máquinas virtuales y cuentas de almacenamiento estándar. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita de Azure](https://azure.microsoft.com/free/).

Una vez que tenga una suscripción, vaya a [Azure Portal](https://portal.azure.com). Seleccione **Crear un recurso > Cadena de bloques > Consorcio de Hyperledger Fabric**.

![Plantilla Hyperledger Fabric Single Member Blockchain de Marketplace](./media/hyperledger-fabric-consortium-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementación

En la plantilla de **Consorcio de Hyperledger Fabric**, seleccione **Crear**.

La implementación de la plantilla le guiará en la configuración de la red de [Hyperledger 1.3](https://hyperledger-fabric.readthedocs.io/en/release-1.3/) de varios nodos. El flujo de implementación se divide en cuatro pasos: aspectos básicos, configuración de red del consorcio, configuración de Fabric y componentes opcionales.

### <a name="basics"></a>Aspectos básicos

En **Aspectos básicos**, especifique los valores para los parámetros estándar de cualquier implementación. como la suscripción, el grupo de recursos y las propiedades básicas de máquina virtual.

![Aspectos básicos](./media/hyperledger-fabric-consortium-blockchain/basics.png)

| Nombre de parámetro | DESCRIPCIÓN | Valores permitidos |
|---|---|---|
**Prefijo de recurso** | Prefijo del nombre de los recursos aprovisionados como parte de la implementación. |6 caracteres o menos |
**Nombre de usuario** | El nombre de usuario del administrador de cada una de las máquinas virtuales implementadas para este miembro. |Entre 1 y 64 caracteres |
**Tipo de autenticación** | El método de autenticación en la máquina virtual. |Contraseña o clave pública SSH|
**Contraseña (Tipo de autenticación = Contraseña)** |La contraseña de la cuenta de administrador para cada una de las máquinas virtuales implementadas. La contraseña debe contener tres de los siguientes tipos de caracteres: 1 letra mayúscula, 1 letra minúscula, 1 número y 1 carácter especial.<br /><br />Aunque todas las máquinas virtuales tienen inicialmente la misma contraseña, puede cambiarla después del aprovisionamiento.|Entre 12 y 72 caracteres|
**Clave SSH (Tipo de autenticación = Clave pública SSH)** |La clave de shell seguro que se usa para el inicio de sesión remoto. ||
**Suscripción** |La suscripción que se va a implementar. ||
**Grupos de recursos** |El grupo de recursos en el que se va a implementar la red del consorcio. ||
**Ubicación** |La región de Azure en la que se va a implementar el primer miembro. ||

Seleccione **Aceptar**.

### <a name="consortium-network-settings"></a>Configuración de red del consorcio

En **Configuración de red**, especifique las entradas para crear una red para el consorcio o unirse a una existente y defina la configuración de la organización.

![Configuración de red del consorcio](./media/hyperledger-fabric-consortium-blockchain/network-settings.png)

| Nombre de parámetro | DESCRIPCIÓN | Valores permitidos |
|---|---|---|
**Configuración de la red** |Puede elegir entre crear una red o unirse a una existente. Si elige *Unirse a existente*, debe proporcionar valores adicionales. |Nueva red <br/> Unirse a existente |
**Contraseña de la entidad de certificación HLF** |Una contraseña utilizada para los certificados generada por las entidades de certificación que se crean como parte de la implementación. La contraseña debe contener tres de los siguientes tipos de caracteres: 1 letra mayúscula, 1 letra minúscula, 1 número y 1 carácter especial.<br /><br />Aunque todas las máquinas virtuales tienen inicialmente la misma contraseña, puede cambiarla después del aprovisionamiento.|Entre 1 y 25 caracteres |
**Configuración de la organización** |Puede personalizar el nombre y el certificado de su organización o usar valores predeterminados.|Valor predeterminado <br/> Avanzado |
**Configuración de red VPN** | Puede aprovisionar una puerta de enlace de un túnel de VPN para acceder a las máquinas virtuales. | Sí <br/> Sin  |

Seleccione **Aceptar**.

### <a name="fabric-specific-settings"></a>Configuración específica del tejido

En **Configuración de Fabric**, configure el tamaño y rendimiento de la red y especifique las entradas para la disponibilidad de la red. Por ejemplo, número de nodos de solicitante y del mismo nivel, motor de persistencia utilizado por cada nodo y tamaño de máquina virtual.

![Configuración de Fabric](./media/hyperledger-fabric-consortium-blockchain/fabric-specific-settings.png)

| Nombre de parámetro | DESCRIPCIÓN | Valores permitidos |
|---|---|---|
**Tipo de escala** |El tipo de implementación de una única máquina virtual con varios contenedores o de varias máquinas virtuales en un modelo de escalabilidad horizontal.|Máquina virtual única o varias máquinas virtuales |
**Tipo de disco de máquina virtual** |El tipo de almacenamiento que respalda cada uno de los nodos implementados. <br/> Para más información acerca de los tipos de disco disponibles, visite [Selección de un tipo de disco](../../virtual-machines/windows/disks-types.md).|SSD estándar <br/> SSD Premium |

### <a name="multiple-vm-deployment-additional-settings"></a>Implementación de varias máquinas virtuales (configuración adicional)

![Configuración de Fabric para implementaciones de varias máquinas virtuales](./media/hyperledger-fabric-consortium-blockchain/multiple-vm-deployment.png)

| Nombre de parámetro | DESCRIPCIÓN | Valores permitidos |
|---|---|---|
**Número de nodos solicitantes** |El número de nodos que ordenan (organizan) transacciones en un bloque. <br />Para más información sobre el servicio de ordenación, consulte la [documentación](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html) de Hyperledger. |1-4 |
**Tamaño de la máquina virtual del nodo solicitante** |El tamaño de máquina virtual usado para los nodos solicitantes de la red.|Estándar BS,<br />Estándar DS,<br />Estándar FS |
**Número de nodos del mismo nivel** | Nodos que pertenecen a los miembros de un consorcio que ejecutan transacciones y mantienen el estado y una copia del libro de contabilidad.<br />Para más información sobre el servicio de ordenación, consulte la [documentación](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html) de Hyperledger.|1-4 |
**Persistencia de estado de nodo** |El motor de persistencia utilizado por los nodos del mismo nivel. Puede configurar este motor por cada nodo del mismo nivel. Vea los detalles siguientes para varios nodos del mismo nivel.|CouchDB <br />LevelDB |
**Tamaño de la máquina virtual del nodo del mismo nivel** |El tamaño de máquina virtual usado para todos los nodos de la red.|Estándar BS,<br />Estándar DS,<br />Estándar FS |

### <a name="multiple-peer-node-configuration"></a>Configuración de varios nodos del mismo nivel

Esta plantilla permite elegir el motor de persistencia por cada nodo del mismo nivel. Por ejemplo, si tiene tres nodos del mismo nivel, puede utilizar CouchDB en uno y LevelDB en los otros dos.

![Configuración de varios nodos del mismo nivel](./media/hyperledger-fabric-consortium-blockchain/multiple-peer-nodes.png)

Seleccione **Aceptar**.

### <a name="deploy"></a>Implementación

En **Resumen**, revise las entradas especificadas y ejecute una validación básica previa a la implementación.

![Resumen](./media/hyperledger-fabric-consortium-blockchain/summary.png)

Revise los términos legales y de privacidad y seleccione **Comprar** para implementar. Dependiendo del número de máquinas virtuales que se están aprovisionando, el tiempo de implementación puede oscilar entre unos minutos y varios minutos.

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para centrarse en el desarrollo de aplicaciones y códigos de cadena con respecto a la red de cadena de bloques de consorcio de Hyperledger.
