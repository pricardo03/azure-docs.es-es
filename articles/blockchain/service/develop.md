---
title: Introducción al desarrollo de Azure Blockchain Service
description: Introducción sobre el desarrollo de soluciones en Azure Blockchain Service.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/11/2019
ms.topic: conceptual
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: a9444847fb75bdf01cabba98057605afbe03c9fc
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73577444"
---
# <a name="azure-blockchain-service-development-overview"></a>Introducción al desarrollo de Azure Blockchain Service

Con Azure Blockchain Service, puede crear redes de cadenas de bloques de consorcio para habilitar escenarios empresariales, como seguimiento de recursos, token digital, fidelidad y recompensa, cadena de suministro financiero y procedencia. Este artículo es una introducción al desarrollo de Azure Blockchain Service y los temas clave para implementar la cadena de bloques para empresa.

## <a name="client-connection-to-azure-blockchain-service"></a>Conexión del cliente a Azure Blockchain Service

Hay distintos tipos de clientes de redes de cadenas de bloques, incluidos nodos completos, nodos de luz y clientes remotos. Azure Blockchain Service compila una red de cadenas de bloques que incluye nodos. Puede usar distintos clientes como puerta de enlace a Azure Blockchain Service para el desarrollo de cadenas de bloques. Azure Blockchain Service ofrece autenticación básica o la clave de acceso como punto de conexión de desarrollo. A continuación, se indican algunos clientes populares que puede usar para conectarse.

### <a name="metamask"></a>MetaMask

MetaMask es una cartera basada en explorador (cliente remoto), cliente RPC y explorador de contrato básico. A diferencia de otras carteras de explorador, MetaMask inserta una instancia web3 en el contexto de JavaScript del explorador, actuando como un cliente RPC que se conecta a una variedad de cadenas de bloques de Ethereum (*mainnet*, *Ropsten testnet*, *Kovan testnet*, nodo de RPC local, etc.). Puede configurar una RPC personalizada fácilmente para conectarse a Azure Blockchain Service e iniciar el desarrollo de la cadena de bloques mediante Remix.

### <a name="geth"></a>Geth

Geth es la interfaz de la línea de comandos para ejecutar un nodo de Ethereum completo implementado en Go. No es necesario ejecutar el nodo completo, pero puede iniciar su consola interactiva que proporciona un entorno en tiempo de ejecución de JavaScript que expone una API de JavaScript para interactuar con Azure Blockchain Service.

## <a name="development-framework-configuration"></a>Configuración del marco de desarrollo

Para desarrollar soluciones empresariales sofisticadas de cadenas de bloques, es necesario un marco de desarrollo para conectarse a diferentes redes de cadenas de bloques, administrar el ciclo de vida del contrato inteligente, automatizar las pruebas, implementar un contrato inteligente con scripts y equipar una consola interactiva.

Truffle es uno de los marcos de desarrollo de cadenas de bloques más populares para escribir, compilar, implementar y probar aplicaciones descentralizadas en las cadenas de bloques de Ethereum. Truffle también se puede considerar como un marco que intenta integrar sin problemas el desarrollo de contratos inteligentes y el desarrollo web tradicional.

Incluso el proyecto más pequeño interactúa con al menos dos nodos de la cadena de bloques: Uno en la máquina del desarrollador y otro que representa la red donde el desarrollador implementa su aplicación. Por ejemplo, la principal red de Ethereum pública o Azure Blockchain Service. Truffle proporciona un sistema para administrar los artefactos de compilación e implementación para cada red y lo hace de manera que simplifica la implementación de la aplicación final. Para más información, consulte [Inicio rápido: Uso de Truffle para conectarse a una red de Azure Blockchain Service](connect-truffle.md).

## <a name="ethereum-quorum-private-transaction"></a>Transacción privada de Quorum de Ethereum

Quorum es un protocolo de libro de contabilidad distribuida basado en Ethereum con transacción, además de privacidad de contrato y nuevos mecanismos de consenso. Las mejoras claves realizadas en Go-Ethereum incluyen:

* Privacidad: Quorum admite transacciones privadas y contratos privados mediante la separación del estado público y privado y usa los intercambios de mensajes cifrados punto a punto para transferencias dirigidas de datos privados para los participantes de la red.
* Otros mecanismos de consenso: sin necesidad de consenso de prueba de trabajo o de prueba de participación en una red con permisos. Quorum ofrece varios mecanismos de consenso que están diseñados para cadenas de consorcio, como RAFT e IBFT.  Azure Blockchain Service usa el mecanismo de consenso IBFT.

* Permisos del mismo nivel: permisos de nodo y del mismo nivel que usan contratos inteligentes, lo que garantiza que solo puedan unirse a la red entidades conocidas.
* Mayor rendimiento: Quorum ofrece un mayor rendimiento respecto a la interfaz Geth pública.

Vea [Tutorial: Envíe una transacción mediante Azure Blockchain Service](send-transaction.md) para obtener un ejemplo de transacción privada.

## <a name="block-explorers"></a>Exploradores de bloques

Los exploradores de bloques son los exploradores de cadenas de bloques en línea que muestran el contenido de bloques individuales, datos de direcciones de transacción y el historial. La información básica del bloque está disponible a través de Azure Monitor en Azure Blockchain Service; sin embargo, si necesita información más detallada durante el desarrollo, los exploradores de bloques pueden ser útiles.  Existen exploradores de bloques de código abierto populares que puede usar. A continuación, se incluye una lista de exploradores de bloques que funcionan con Azure Blockchain Service:

* [Epirus Azure Blockchain Service Explorer](https://azuremarketplace.microsoft.com/marketplace/apps/blk-technologies.azure-blockchain-explorer-template?tab=Overview) de Web3 Labs
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

## <a name="tps-measurement"></a>Medición de TPS

Como las cadenas de bloques se usan en más escenarios empresariales, la velocidad de las transacciones por segundo (TPS) es importante para evitar cuellos de botella y deficiencias de sistema. Las altas tasas de transacción pueden ser difíciles de mantener dentro de una cadena de bloques descentralizada. Una medida precisa de TPS puede verse afectada por distintos factores, como el subproceso del servidor, el tamaño de la cola de transacciones, la latencia de red y la seguridad. Si necesita medir la velocidad de TPS durante el desarrollo, una herramienta de código abierto popular es [ChainHammer](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Uso de Truffle para conectarse a una red de Azure Blockchain Service](connect-truffle.md)
