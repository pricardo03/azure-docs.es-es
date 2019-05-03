---
title: Introducción al desarrollo de Azure Blockchain Service
description: Introducción sobre el desarrollo de soluciones en Azure Blockchain Service.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 388a5d8c80c3e2462602959e9d5cbc1452974d1f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027906"
---
# <a name="azure-blockchain-service-development-overview"></a>Introducción al desarrollo de Azure Blockchain Service

Con el servicio de la cadena de bloques de Azure, puede crear redes de cadena de bloques para habilitar escenarios empresariales como seguimiento de activos, token digital, fidelidad y recompensa, cadena de suministro financieros y procedencia consortium. En este artículo es una introducción a la información general sobre el desarrollo de Azure Blockchain Service y los temas clave para implementar la cadena de bloques para empresa.

## <a name="client-connection-to-azure-blockchain-service"></a>Conexión de cliente al servicio de Azure Blockchain

Hay distintos tipos de clientes de redes de cadena de bloques incluidas nodos completos, nodos de luz y los clientes remotos. Azure Blockchain Service crea una red de cadena de bloques que incluya los nodos. Puede usar a distintos clientes como puerta de enlace al servicio de la cadena de bloques de Azure para el desarrollo de la cadena de bloques. Azure Blockchain Service ofrece autenticación básica o la clave de acceso como un punto de conexión de desarrollo. Los siguientes son los clientes conocidos que puede usar conectarse.

### <a name="metamask"></a>MetaMask

MetaMask es una cartera basada en explorador (cliente remoto), cliente RPC y el Explorador de contrato básico. A diferencia de otros wallets explorador, MetaMask inserta una instancia web3 en el contexto del explorador de JavaScript, que actúa como un cliente RPC que se conecta a una variedad de las cadenas de bloques Ethereum (*mainnet*, *Ropsten testnet*, *Kovan testnet*, nodo local de RPC, etcetera.). Puede configurar RPC personalizada fácilmente para conectarse al servicio de la cadena de bloques de Azure e iniciar el desarrollo de la cadena de bloques mediante Remix.

### <a name="geth"></a>Geth

Geth es la interfaz de línea de comandos para ejecutar un nodo Ethereum completo implementado en Go. No es necesario ejecutar completo de nodo, pero puede iniciar su consola interactiva que proporciona un entorno de tiempo de ejecución de JavaScript exponer una API de JavaScript para interactuar con el servicio de la cadena de bloques de Azure.

## <a name="development-framework-configuration"></a>Configuración del marco de desarrollo

Para desarrollar soluciones empresariales sofisticadas de cadena de bloques, es necesario un marco de desarrollo para conectarse a redes de la cadena de bloques diferentes, administrar el ciclo de vida de contrato inteligente, automatizar las pruebas, implementar un contrato inteligente con secuencias de comandos y equipar una consola interactiva.

Truffle es un marco de desarrollo más populares de la cadena de bloques para escribir, compilar, implementar y probar aplicaciones descentralizadas en las cadenas de bloques Ethereum. También se puede considerar Truffle como un marco de trabajo que intenta integrar sin problemas el desarrollo de contrato inteligente y desarrollo web tradicional.

Incluso en el proyecto más pequeño interactúa con al menos dos nodos de la cadena de bloques: Uno en el equipo del desarrollador y otro que representa la red donde el desarrollador implementa su aplicación. Por ejemplo, la red de Ethereum pública principal o servicio de Azure Blockchain. Truffle proporciona un sistema para administrar los artefactos de compilación e implementación para cada red y lo hace de manera que simplifica la implementación de la aplicación final. Para más información, consulte [Inicio rápido: Usar Truffle para conectarse a una red de un servicio de Azure Blockchain](connect-truffle.md).

## <a name="ethereum-quorum-private-transaction"></a>Transacción privada Ethereum quórum

Quórum es un protocolo de libro de contabilidad distribuida de Ethereum con transacción además de privacidad de contrato y nuevos mecanismos de consenso. Las mejoras claves a través de Go-Ethereum incluyen:

* Privacidad: quórum admite transacciones privadas y contratos privados mediante la separación del Estado públicas y privadas y usa los intercambios de mensajes cifrados de peer-to-peer dirigido transferencia de datos privados a los participantes de la red.
* Otros mecanismos de consenso - sin necesidad de consenso de prueba de trabajo o de prueba de juego en una red tiene permiso. Quórum ofrece varios mecanismos consenso que están diseñados para las cadenas de consorcio como BALSA y IBFT.  Servicios de Azure Blockchain utiliza el mecanismo de consenso IBFT.

* Del mismo nivel de permisos - permisos de nodo y del mismo nivel con los contratos inteligentes, lo que garantiza solo las entidades conocidas pueden unirse a la red
* Un mayor rendimiento - quórum ofrece un mayor rendimiento respecto Geth pública

Vea [Tutorial: Enviar una transacción mediante el servicio de Azure Blockchain](send-transaction.md) para obtener un ejemplo de transacción privada.

## <a name="block-explorers"></a>Exploradores de bloque

Los exploradores de bloque son los exploradores de cadena de bloques en línea que muestran contenido de bloques individuales, datos de direcciones de la transacción y el historial. Información de bloque básico está disponible a través de Azure Monitor en el servicio de Azure Blockchain, sin embargo, si necesita información más detallada durante el desarrollo, exploradores de bloque pueden ser útiles.  Hay exploradores de bloque de código abierto populares que puede usar. La siguiente es una lista de exploradores de bloque que funcionan con el servicio de la cadena de bloques de Azure:

* [Explorador de servicios de Azure Blockchain](https://web3labs.com/azure-offer) de Web3 Labs
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

## <a name="tps-measurement"></a>Medición de TP

Como cadena de bloques se usa en escenarios empresariales más, las transacciones por segundo (TPS) de velocidad es importante para evitar cuellos de botella y deficiencias de sistema. Altas tasas de transacción pueden ser difíciles de mantener dentro de una cadena de bloques descentralizada. Una medida precisa de TP puede verse afectada por distintos factores, como el subproceso del servidor, tamaño de la cola de transacciones, latencia de red y seguridad. Si necesita medir la velocidad TPS durante el desarrollo, es una herramienta de código abierto popular [ChainHammer](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Usar Truffle para conectarse a una red de un servicio de Azure Blockchain](connect-truffle.md)
