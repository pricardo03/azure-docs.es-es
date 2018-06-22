---
title: Consorcio de Hyperledger Fabric
description: Use la plantilla de la solución Consorcio de Hyperledger Fabric para implementar y configurar una red de un único miembro.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 7b60c086896506e5883607db48a64d2a2efbd967
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659900"
---
# <a name="hyperledger-fabric-single-member-network"></a>Red de un único miembro de Hyperledger Fabric

Puede usar la plantilla de la solución Consorcio de Hyperledger Fabric para implementar y configurar una red de un único miembro de Hyperledger Fabric (varios nodos).

Después de leer este artículo, habrá aprendido lo siguiente:

- Conocimientos de trabajo de cadena de bloques, Hyperledger Fabric y arquitecturas de red de consorcio más complicadas
- Cómo implementar y configurar una red de consorcio de Hyperledger Fabric de un único miembro desde Azure Portal

## <a name="about-blockchain"></a>Acerca de la cadena de bloques

Si no está familiarizado con la comunidad de cadena de bloques, esta es una oportunidad excelente para conocer la tecnología de una manera sencilla y configurable en Azure. La cadena de bloques es la tecnología que se esconde detrás de Bitcoin; sin embargo, es mucho más que un habilitador de una moneda virtual. Es un compuesto de bases de datos existentes, sistemas distribuidos y tecnologías de cifrado que permite el cálculo seguro de varias partes con garantías en torno a inmutabilidad, verificabilidad, auditabilidad y resiliencia a los ataques. Diferentes protocolos emplean diferentes mecanismos para proporcionar estos atributos. [Hyperledger Fabric](https://github.com/hyperledger/fabric) es uno de estos protocolos.

## <a name="consortium-architecture-on-azure"></a>Arquitectura de consorcio en Azure

Esta plantilla implementa una topología que ayuda a probar y simular la producción de los usuarios dentro de una única organización (único miembro). Esta implementación consta de una red de varios nodos en una única región y pronto se expandirá a varias regiones.

La red está formada por tres tipos de nodos:

1. **Nodo miembro**: un nodo que ejecuta el servicio de pertenencia de Fabric que registra y administra miembros de la red. Este nodo se puede agrupar en clústeres a la larga para obtener escalabilidad y alta disponibilidad; sin embargo, en este laboratorio, se usará un nodo de un único miembro.
2. **Nodo solicitante**: un nodo que ejecuta el servicio de comunicación que implementa una garantía de entrega, como difusión total de pedidos o transacciones atómicas.
3. **Nodo del mismo nivel**: un nodo que confirma las transacciones y mantiene el estado y una copia del libro de contabilidad distribuido.

## <a name="getting-started"></a>Introducción

Para comenzar, necesitará una suscripción a Azure que pueda admitir la implementación de varias máquinas virtuales y cuentas de almacenamiento estándar. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita de Azure](https://azure.microsoft.com/free/).

De forma predeterminada, la mayoría de los tipos de suscripción admiten una topología de implementación pequeña sin necesidad de aumentar la cuota. La implementación más pequeña posible para un miembro necesitará:

- 5 máquinas virtuales (5 núcleos)
- 1 red virtual
- 1 equilibrador de carga
- 1 dirección IP pública

Una vez que tenga una suscripción, vaya a [Azure Portal](https://portal.azure.com). Seleccione **+**, **Cadena de bloques** y **Hyperledger Fabric Single Member Blockchain**.

![Plantilla Hyperledger Fabric Single Member Blockchain de Marketplace](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementación

Para comenzar, seleccione **Hyperledger Fabric Single Member Blockchain** y haga clic en **Crear**. Se abre la hoja **Aspectos básicos** en el asistente.

La implementación de la plantilla le guiará en la configuración de la red de varios nodos. El flujo de implementación se divide en tres pasos: aspectos básicos, configuración de red y configuración de Fabric.

### <a name="basics"></a>Aspectos básicos

En la hoja **Aspectos básicos**, especifique valores para los parámetros estándar de cualquier implementación, como la suscripción, el grupo de recursos y las propiedades básicas de la máquina virtual.

![Aspectos básicos](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Nombre de parámetro| DESCRIPCIÓN| Valores permitidos|Valor predeterminado
---|---|---|---
**Prefijo de recurso**| Una cadena que se usa como base para designar los recursos implementados.|6 caracteres o menos|N/D
**Nombre de usuario de máquina virtual**| El nombre de usuario del administrador de cada una de las máquinas virtuales implementadas para este miembro.|Entre 1 y 64 caracteres|azureuser
**Tipo de autenticación**| El método de autenticación en la máquina virtual.|Contraseña o clave pública SSH|Password
**Contraseña (Tipo de autenticación = Contraseña)**|La contraseña de la cuenta de administrador para cada una de las máquinas virtuales implementadas. La contraseña debe cumplir 3 de los siguientes requisitos: 1 letra mayúscula, 1 letra minúscula, 1 número y 1 carácter especial.<br /><br />Aunque todas las máquinas virtuales tienen inicialmente la misma contraseña, puede cambiarla después del aprovisionamiento.|Entre 12 y 72 caracteres|N/D
**Clave SSH (Tipo de autenticación = Clave pública)**|La clave de shell seguro que se usa para el inicio de sesión remoto.||N/D
**Restringir el acceso por dirección IP**|Opción para determinar si el acceso al punto de conexión de cliente está o no restringido.|Sí/No| Sin 
**Dirección IP o subred permitidas (restringir el acceso por dirección IP = Sí)**|La dirección IP o el conjunto de direcciones IP que pueden tener acceso al punto de conexión de cliente cuando está habilitado el control de acceso.||N/D
**Suscripción** |La suscripción que se va a implementar.
**Grupo de recursos** |El grupo de recursos en el que se va a implementar la red del consorcio.||N/D
**Ubicación** |La región de Azure en la que se va a implementar la superficie de red del primer miembro.

### <a name="network-size-and-performance"></a>Tamaño de red y rendimiento

A continuación, en **Network size and performance** (Tamaño de red y rendimiento), especifique las entradas para el tamaño de la red de consorcio, como el número de miembros, los nodos solicitantes o los nodos del mismo nivel. Elija las opciones de infraestructura y el tamaño de la máquina virtual.

![Tamaño de red y rendimiento](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Nombre de parámetro| DESCRIPCIÓN| Valores permitidos|Valor predeterminado
---|---|---|---
**Número de nodos de pertenencia**|El número de nodos que ejecutan el servicio de pertenencia. Para más información sobre el servicio de pertenencia, examine los servicios de seguridad y pertenencia en la [documentación](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf) de Hyperledger.<br /><br />Este valor está restringido actualmente a 1 nodo, pero está prevista la compatibilidad con el escalado horizontal mediante la agrupación en clústeres en la próxima revisión.|1| 1
**Número de nodos solicitantes** |El número de nodos que ordenan (organizan) transacciones en un bloque. Esta afirmación es farragosa y confusa. Para más información sobre el servicio de ordenación, consulte la [documentación](http://hyperledger-fabric.readthedocs.io/en/latest/orderingservice.html) de Hyperledger.<br /><br />Este valor está restringido actualmente a 1 nodo. |1 |1
**Número de nodos del mismo nivel**| Nodos que pertenecen a los miembros de un consorcio que ejecutan transacciones y mantienen el estado y una copia del libro de contabilidad.<br /><br />Para más información sobre el servicio de ordenación, consulte la [documentación](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html) de Hyperledger.|3| Entre 3 y 9
**Rendimiento del almacenamiento**|El tipo de almacenamiento que respalda cada uno de los nodos implementados. Para más información sobre el almacenamiento, visite [Introducción a Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) y [Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Estándar o Premium|Estándar
**Tamaño de la máquina virtual** |El tamaño de máquina virtual usado para todos los nodos de la red.|Estándar A,<br />Estándar D,<br />Estándar D-v2,<br />Serie F estándar,<br />Estándar DS,<br />y Estándar FS|Estándar D1_v2

### <a name="fabric-specific-settings"></a>Configuración específica de Fabric

Por último, en **Fabric Settings** (Configuración de Fabric), especifique la configuración relativa a Fabric.

![Configuración de Fabric](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Nombre de parámetro| DESCRIPCIÓN| Valores permitidos|Valor predeterminado
---|---|---|---
**Nombre de usuario de arranque**| El usuario autorizado inicial que se registrará en los servicios miembros de la red implementada.|9 caracteres como máximo|admin
**Contraseña de usuario de arranque para la entidad emisora de certificados de Fabric**|La contraseña de administrador que se usa para proteger la cuenta de entidad de certificación de Fabric importada en el nodo de pertenencia.<br /><br />Debe contener una letra mayúscula, una letra minúscula y un número.|12 o más caracteres|N/D

### <a name="deploy"></a>Implementación

En **Resumen**, revise las entradas especificadas y ejecute una validación básica previa a la implementación.

![Resumen](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Revise los términos legales y de privacidad y haga clic en **Comprar** para implementar. Dependiendo del número de máquinas virtuales que se están aprovisionando, el tiempo de implementación puede oscilar entre unos minutos y varios minutos.

### <a name="accessing-nodes"></a>Acceso a los nodos

Una vez finalizada la implementación, se muestra una hoja de **Información general**.

![Implementaciones](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Si la pantalla no aparece automáticamente (quizás porque se movió por el portal de administración mientras se ejecutaba la implementación), siempre puede encontrarla en la pestaña Grupo de recursos de la barra de navegación izquierda. Haga clic en el nombre del grupo de recursos que escribió en el paso 1 para ir a la página **Información general**.

En la información general se muestran todos los recursos que se implementaron mediante la plantilla de la solución. Puede explorarlos a su antojo, pero desde esta pantalla también puede acceder a los _parámetros de salida_ generada por la plantilla. Estos parámetros de salida proporcionarán información útil al conectarse a la red de Hyperledger Fabric.

Para acceder a los parámetros de salida, primero haga clic en la pestaña **Implementaciones** en la hoja Grupo de recursos. Se muestra el historial de implementación.

![Historial de implementación](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

En el historial de implementación, haga clic en la primera implementación de la lista para ver los detalles.

![Detalles de la implementación](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

La pantalla de detalles muestra un resumen de la implementación, seguido de tres parámetros de salida útiles:

- _API-ENDPOINT_ se puede usar después de implementar una aplicación en la red.
- _PREFIX_, también denominado _prefijo de implementación_, identifica de forma única los recursos y la implementación. Se usará al emplear las herramientas basadas en la línea de comandos.
- _SSH-TO-FIRST-VM_ proporciona un comando ssh ya ensamblado, con todos los parámetros adecuados necesarios, para conectarse a la primera máquina virtual de la red; en el caso de Hyperledger Fabric, será el nodo de la entidad emisora de certificados de Fabric.

Puede conectarse de forma remota a las máquinas virtuales de cada nodo mediante SSH con el nombre de usuario de administrador y la contraseña o la clave SSH proporcionados. Puesto que las máquinas virtuales de nodo no tienen sus propias direcciones IP públicas, debe volver a pasar por el equilibrador de carga y especificar el número de puerto. El comando de SSH para acceder al primer nodo de transacción es la tercera salida de plantilla, **SSH-TO-FIRST-VM (en la implementación de ejemplo: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Para llegar a nodos de transacción adicionales, aumente el número de puerto en uno (por ejemplo, el primer nodo de transacción está en el puerto 3000, el segundo en el 3001, el tercero en el 3002, etc.).

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para centrarse en el desarrollo de aplicaciones y códigos de cadena con respecto a la red de cadena de bloques de consorcio de Hyperledger.
