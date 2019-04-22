---
title: Consorcio de prueba de autoridad de Ethereum - Azure
description: Usar la solución del consorcio de prueba de autoridad de Ethereum para implementar y configurar una red con varios miembros del consorcio Ethereum
services: azure-blockchain
keywords: ''
author: CodyBorn
ms.author: coborn
ms.date: 04/08/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: vamelech
ms.openlocfilehash: 3531b43e6aee1eedef811e81e192873c5b5ed561
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59274797"
---
# <a name="ethereum-proof-of-authority-consortium"></a>Consorcio de prueba de autoridad de Ethereum

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Información general
[Esta solución](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) está diseñada para que sea más fácil implementar, configurar y gobernar una red con varios miembros del consorcio de prueba de autoridad de Ethereum con un conocimiento mínimo de Azure y Ethereum.

Con un conjunto de entradas de usuario y una implementación con un solo clic a través de Azure Portal, cada miembro puede aprovisionar un consumo de red mediante el uso de los servicios de proceso, redes y almacenamiento de Microsoft Azure en todo el mundo. El consumo de red de cada miembro se compone de un conjunto de nodos de validador de carga equilibrada con el que una aplicación o un usuario puede interactuar para enviar las transacciones de Ethereum.

## <a name="concepts"></a>Conceptos

### <a name="terminology"></a>Terminología

-   **Consenso**: acto de sincronización de datos en la red distribuida a través de la creación y validación de bloques.

-   **Miembro del consorcio**: entidad que participa en un consenso en la red de la cadena de bloques.

-   **Administrador**: cuenta de Ethereum que se usa para administrar la participación de un miembro determinado del consorcio.

-   **Validador**: máquina asociada a una cuenta de Ethereum que participa en un consenso en nombre de un administrador.

### <a name="proof-of-authority"></a>Prueba de autoridad

Para aquellos que no están familiarizados con la comunidad de cadena de bloques, el lanzamiento de esta solución es una excelente oportunidad para obtener información sobre la tecnología de una manera fácil y que se puede configurar en Azure. Prueba de trabajo es un mecanismo de resistencia de Sybil que aprovecha los costos de cálculo para regular automáticamente la red y permitir la participación razonable. Esto funciona bien en redes anónimas, redes de cadena de bloques abiertas donde la competencia por criptomoneda promueve la seguridad en la red. Sin embargo, en redes privadas y del consorcio, Ether subyacente no tiene ningún valor. Un protocolo alternativo, la prueba de autoridad, es más adecuado para las redes permitidas en la que se conocen todos los participantes de consenso y buena reputación. Sin la necesidad de minería, la prueba de autoridad resulta más eficaz y, al mismo tiempo, se conserva una tolerancia a errores bizantina.

### <a name="consortium-governance"></a>Gobernanza del consorcio

Puesto que la prueba de autoridad se basa en una lista de entidades de red para que la red esté en buen estado, es importante proporcionar un mecanismo razonable para realizar modificaciones en esta lista de permisos. Cada implementación incluye un conjunto de contratos inteligentes y el portal para el gobierno de la cadena de esta lista de permitidos. Una vez que un cambio propuesto alcanza el voto mayoritario de los miembros del consorcio, se realiza dicho cambio. Esto permite agregar nuevos participantes de consenso o quitar participantes comprometidos de una forma transparente que fomenta una red honesta.

### <a name="admin-account"></a>Cuenta de administrador

Durante la implementación de los nodos de prueba de autoridad, se le pedirá una dirección de administración Ethereum. Puede usar varios mecanismos diferentes para generar y proteger esta cuenta de Ethereum. Una vez que esta dirección se agrega como una autoridad en la red, puede utilizar esta cuenta para participar en la gobernanza. Esta cuenta de administrador también se utilizará para delegar la participación del consenso en los nodos de validador que se crean como parte de esta implementación. Puesto que se usa solo la dirección pública de Ethereum, cada administrador tiene la flexibilidad para proteger sus claves privadas de forma que sigue a su modelo de seguridad deseado.

### <a name="validator-node"></a>Nodo de validador

En el protocolo de prueba de autoridad, los nodos de validador ocupan el lugar de los nodos de minero tradicionales. Cada validador tiene una identidad única de Ethereum que se agrega a una lista de permisos de contrato inteligente. Una vez que un validador se encuentra en esta lista, puede participar en el proceso de creación de bloques. Para obtener más información sobre este proceso, consulte la documentación de Parity en el [consenso Authority Round](https://wiki.parity.io/Aura). Para redundancia geográfica, cada miembro del consorcio puede aprovisionar dos o más nodos de validador en cinco regiones. Los nodos de validador se comunican con otros nodos de validador para llegar a un consenso sobre el estado del libro de contabilidad distribuida subyacente.
Para garantizar la participación razonable en la red, se prohíbe a cada miembro del consorcio el uso de más validadores que el primer miembro de la red (si el primer miembro implementa tres validadores, cada miembro solo puede tener un máximo de tres validadores).

### <a name="identity-store"></a>Almacén de identidades

Dado que cada miembro tendrá varios nodos de validador que se ejecutan simultáneamente y cada nodo debe tener una identidad permitida, es importante que los validadores de forma segura pueden adquirir una identidad única activada en la red. Para facilitar esta tarea, hemos creado un Store de identidad que se implementa en la suscripción de cada miembro que contiene las identidades Ethereum generadas de forma segura. Tras la implementación, el contenedor de orquestaciones generará una clave privada Ethereum para cada control de validación y almacenarlos en Azure Key Vault. Antes de que el nodo de paridad se inicie, primero adquiere una concesión en una identidad no utilizada para asegurar que la identidad no sea captada por otro nodo. La identidad se proporciona al cliente, lo que la da la autoridad para comenzar a crear bloques. Si la máquina virtual anfitriona experimenta una interrupción, la concesión se liberará, permitiendo que un nodo de reemplazo reanude su identidad en el futuro.

### <a name="bootnode-registrar"></a>Registrador de nodos de arranque

Para habilitar la facilidad de conectividad, cada miembro hospedará un conjunto de información de conexión en el [punto de conexión de API de datos](#data-api). Estos datos incluyen una lista de bootnodes que se proporcionan como nodos entre pares para el miembro de unión. Como parte de esta API de datos, mantenemos esta lista de nodos de arranque actualizada

### <a name="bring-your-own-operator"></a>Traiga su propio operador

A menudo, un miembro del consorcio querrá participar en la gobernanza de la red, pero no quiere operar ni mantener su infraestructura. A diferencia de los sistemas tradicionales, tener un único operador en la red funciona en contra del modelo descentralizado de sistemas de cadena de bloques. En lugar de alquilar un intermediario centralizado para operar una red, cada miembro del consorcio puede delegar la administración de la infraestructura al operador que prefiera. Esto permite un modelo híbrido, donde puede elegir cada miembro para operar su propia infraestructura o delegar la operación a un socio diferente. El flujo de trabajo de la operación delegada funciona de la siguiente manera:

1.  El **miembro del consorcio** genera una dirección de Ethereum (contiene la clave privada)

2.  El **miembro del consorcio** proporciona la dirección pública de Ethereum al **operador**

3.  El **operador** implementa y configura los nodos de validador de la prueba de autoridad mediante nuestra solución de Azure Resource Manager

4.  El **operador** proporciona el punto de conexión RPC y de administración al **miembro del consorcio**

5.  El **miembro del consorcio** utiliza su clave privada para firmar una solicitud de aceptación de los nodos de validador que el **operador** ha implementado para participar en su nombre

### <a name="azure-monitor"></a>Azure Monitor

Esta solución también viene con Azure Monitor para realizar el seguimiento de las estadísticas de nodo y de red. Para los desarrolladores de aplicaciones, esto proporciona visibilidad en la cadena de bloques subyacente para realizar el seguimiento de las estadísticas de generación de bloques. Los operadores de red pueden usar Azure Monitor para detectar y evitar rápidamente las interrupciones de la red a través de las estadísticas de infraestructura y los registros consultables. Para obtener más información, consulte [supervisión del servicio](#service-monitoring).

### <a name="deployment-architecture"></a>Arquitectura de implementación

#### <a name="description"></a>DESCRIPCIÓN

Esta solución puede implementar una red del consorcio de Ethereum con varios miembros basada en una o en varias regiones. De forma predeterminada, se puede acceder a los puntos de conexión RPC y de emparejamiento a través de una dirección IP pública para habilitar la conectividad simplificada entre suscripciones y nubes. Se recomienda aprovechar los [contratos de permisos de Parity](https://wiki.parity.io/Permissioning) para controles de acceso de nivel de aplicación. También se admiten las redes que se ha implementado detrás de VPN, que aprovechan las puertas de enlace de red virtual para conectividad entre suscripciones. Estas implementaciones son más complejas, por lo que se recomienda comenzar con el modelo de dirección IP pública en primer lugar.

#### <a name="consortium-member-overview"></a>Información general de los miembros del consorcio

Cada implementación de los miembros del consorcio incluye:

-   Máquinas virtuales para ejecutar los validadores de prueba de autoridad

-   Azure Load Balancer para distribuir solicitudes de RPC, de emparejamiento y de aplicaciones descentralizadas de gobernanza

-   Azure Key Vault para proteger las identidades de validador

-   Azure Storage para hospedar la información de red persistente y coordinar la concesión

-   Azure Monitor para agregar registros y estadísticas de rendimiento

-   Puerta de enlace de red virtual (opcional) para permitir conexiones VPN entre redes virtuales privadas

![arquitectura de implementación](./media/ethereum-poa-deployment/deployment-architecture.png)

Hemos aprovechado los contenedores de Docker para la confiabilidad y la modularidad. Usamos Azure Container Registry para hospedar y proporcionar imágenes con versiones como parte de cada implementación. Las imágenes de contenedor constan de:

-   Orquestador

    -   Se ejecuta una vez durante la implementación

    -   Genera identidades y los contratos de gobernanza

    -   Almacena identidades en el almacén de identidades

-   Cliente de Parity

    -   Alquila la identidad del almacén de identidades

    -   Detecta los pares y se conecta a ellos

-   Agente EthStats

    -   Recopila estadísticas y registros locales a través de RPC y los envía a Azure Monitor

-   Aplicación descentralizada de gobernanza

    -   Interfaz web para interactuar con los contratos de gobernanza

## <a name="how-to-guides"></a>Guías de procedimientos
### <a name="governance-dapp"></a>Aplicación descentralizada de gobernanza

En el corazón de la prueba de autoridad está la gobernanza descentralizado. La aplicación descentralizada de gobernanza es un conjunto de [contratos inteligentes](https://github.com/Azure-Samples/blockchain/tree/master/ethereum-on-azure/) implementados previamente y una aplicación web que se usan para gobernar las autoridades en la red.
Las autoridades se dividen en identidades de administrador y nodos de validador.
Los administradores tienen la capacidad de delegar la participación de consenso en un conjunto de nodos de validador. Los administradores también pueden votar otros administradores dentro o fuera de la red.

![aplicación descentralizada de gobernanza](./media/ethereum-poa-deployment/governance-dapp.png)

-   **Gobierno descentralizado**: los cambios en las autoridades de la red se administran mediante la votación en cadena por parte de administradores seleccionados.

-   **Delegación de validador**: las autoridades pueden administrar sus nodos de validador que se configuran en cada implementación de prueba de autoridad.

-   **Historial de cambios auditable**: cada cambio se registra en la cadena de bloques, lo que proporciona transparencia y capacidad de auditoría.

#### <a name="getting-started-with-governance"></a>Introducción a la gobernanza
Para realizar cualquier tipo de transacciones a través de la DApp gobierno, deberá aprovechar una cartera de Ethereum.  La estrategia más sencilla es usar una cartera en el explorador, como [MetaMask](https://metamask.io); pero puesto que son contratos inteligentes implementados en la red, también puede automatizar las interacciones con el contrato de gobernanza.

Después de instalar MetaMask, vaya a la aplicación descentralizada de gobernanza en el explorador.  Puede encontrar la URL en el correo electrónico de confirmación de implementación o mediante Azure Portal, en el resultado de la implementación.  Si no tiene una cartera en el explorador instalado, no podrá realizar ninguna acción; Sin embargo, todavía puede leer el estado de administrador.  

#### <a name="becoming-an-admin"></a>Convertirse en administrador
Si es el primer miembro que implementa en la red, a continuación, le pone automáticamente un administrador y los nodos de paridad se mostrará como validadores.  Si va a unir a la red, deberá obtener votado como administrador por una mayoría (mayor que 50%) del conjunto de administrador existente.  Si prefiere no ser administrador, los nodos seguirán sincronizándose y validarán la cadena de bloques, pero no participarán en el proceso de creación de bloques. Para iniciar el proceso de votación para convertirse en administrador, haga clic en __Designar__ y escriba su alias y dirección de Ethereum.

![Designar](./media/ethereum-poa-deployment/governance-dapp-nominate.png)

#### <a name="candidates"></a>Candidatos
Al seleccionar la pestaña __Candidatos__, se le mostrará el conjunto actual de los candidatos a administrador.  Cuando un candidato alcance una mayoría de los votos de los administradores actuales, pasará a ser administrador.  Para votar un candidato, seleccione la fila y haga clic en “Votar”, en la parte superior.  Si cambia de opinión sobre un voto, seleccione el candidato y haga clic en “Rescindir voto”.

![Candidatos](./media/ethereum-poa-deployment/governance-dapp-candidates.png)


#### <a name="admins"></a>Administradores
En la pestaña __Administradores__ puede ver el conjunto actual de administradores y votar en contra.  Una vez que un administrador pierde más que el soporte del 50%, que se quiten como administrador en la red.  Los nodos de validador que posea este administrador perderán el estado de validador y se convertirán en nodos de transacción en la red.  Un administrador puede eliminarse por diversos motivos: Sin embargo, resulta para el consorcio que acordar una directiva de antemano.

![Administradores](./media/ethereum-poa-deployment/governance-dapp-admins.png)

#### <a name="validators"></a>Validadores
Al seleccionar la pestaña __Validadores__ en el menú de la izquierda se mostrarán los nodos de paridad implementados actualmente para esta instancia y su estado actual (tipo de nodo).  Cada miembro del consorcio tendrá un conjunto diferente de validadores en esta lista, ya que esta vista representa al miembro actual de consorcio implementada.  Si se trata de una instancia recién implementada y todavía no ha agregado los controles de validación, se le mostrará la opción para agregar controles de validación.  Al seleccionar esta automáticamente elegir un conjunto de nodos de paridad regionalmente equilibrado y asignarlos a su conjunto de validador.  Si ha implementado más nodos de la capacidad permitida, los nodos restantes se convertirán en nodos de la transacción en la red.

La dirección de cada validador se asigna automáticamente a través del [almacén de identidades](#identity-store) de Azure.  Si un nodo deja de funcionar, renunciará a su identidad y permitirá que otro nodo de la implementación ocupe su lugar.  Esto garantiza que su participación consensuada tenga alta disponibilidad.

![Validadores](./media/ethereum-poa-deployment/governance-dapp-validators.png)

#### <a name="consortium-name"></a>Nombre del consorcio
Cualquier administrador puede cambiar el nombre del consorcio que se muestra en la parte superior de la página.  Seleccione el icono de engranaje situado en la esquina superior izquierda para modificar el nombre del consorcio.

#### <a name="account-menu"></a>Menú de la cuenta
En la parte superior derecha se encuentra el icono de identidad y el alias de la cuenta de Ethereum.  Si es un administrador tendrá la posibilidad de actualizar el alias.

![Cuenta](./media/ethereum-poa-deployment/governance-dapp-account.png)

### <a name="deploy-ethereum-proof-of-authority"></a>Implementación de la prueba de autoridad de Ethereum

A continuación figura un ejemplo de un flujo de implementación de varias partes:

1.  Tres miembros generan una cuenta de Ethereum mediante MetaMask

2.  El *miembro A* implementa la prueba de autoridad de Ethereum, proporcionando su dirección pública de Ethereum

3.  El *miembro A* proporciona la dirección URL del consorcio al *miembro B* y al *miembro C*

4.  El *miembro B* y el *miembro C* implementan la prueba de autoridad de Ethereum, proporcionando sus direcciones públicas de Ethereum y la dirección URL del consorcio del *miembro A*

5.  El *miembro A* vota al *miembro B* como administrador

6.  El *miembro A* y el *miembro B* votan al *miembro C* como administrador

Este proceso requiere una suscripción de Azure que admita la implementación de varios discos administrados y máquinas virtuales. Si es necesario, [cree una cuenta de Azure](https://azure.microsoft.com/free/) gratuita para comenzar.

Una vez que una suscripción está protegida, vaya al Azure Portal. Seleccione '+', Marketplace (“Ver todo”) y busque el consorcio de prueba de autoridad de Ethereum.

La siguiente sección le guiará por el proceso de configuración del consumo de red del primer miembro. El flujo de implementación se divide en cinco pasos: aspectos básicos, regiones de implementación, tamaño de red y rendimiento, configuración de Ethereum y Azure Monitor.

#### <a name="basics"></a>Aspectos básicos

En **Aspectos básicos**, especifique valores para los parámetros estándar para cualquier implementación, como la suscripción, el grupo de recursos y las propiedades básicas de la máquina virtual.

A continuación se proporciona una descripción detallada de cada parámetro:

Nombre de parámetro|DESCRIPCIÓN|Valores permitidos|Valores predeterminados
---|---|---|---
¿Crear una nueva red o unirse a una existente?|¿Crear una nueva red o unirse a una red existente del consorcio?|Crear nueva Unirse a existente|Crear nuevo
Dirección de correo electrónico (opcional)|Recibirá una notificación por correo electrónico cuando se complete la implementación con información sobre esta.|Dirección de correo electrónico válida|N/D
Nombre de usuario de máquina virtual|Nombre de usuario del administrador de cada máquina virtual implementada (solo caracteres alfanuméricos)|De 1 a 64 caracteres|N/D
Tipo de autenticación|El método de autenticación en la máquina virtual.|Contraseña o clave pública SSH|Contraseña
Contraseña (Tipo de autenticación = Contraseña)|La contraseña de la cuenta de administrador para cada una de las máquinas virtuales implementadas.  La contraseña debe cumplir 3 de los siguientes requisitos: 1 letra mayúscula, 1 letra minúscula, 1 número y 1 carácter especial. Aunque todas las máquinas virtuales tienen inicialmente la misma contraseña, puede cambiarla después del aprovisionamiento.|De 12 a 72 caracteres|N/D
Clave SSH (Tipo de autenticación = Clave pública)|La clave de shell seguro que se usa para el inicio de sesión remoto.||N/D
Subscription|La suscripción en la que se va a implementar la red del consorcio.||N/D
Grupo de recursos|El grupo de recursos en el que se va a implementar la red del consorcio.||N/D
Ubicación|La región de Azure para el grupo de recursos.||N/D

A continuación se muestra una implementación de ejemplo: ![Aspectos básicos](./media/ethereum-poa-deployment/basic-blade.png)

#### <a name="deployment-regions"></a>Regiones de implementación

Después, en Regiones de implementación, especifique las entradas para el número de regiones para implementar la red del consorcio y la selección de regiones de Azure en función del número de regiones dado. El usuario puede implementarse en un máximo de cinco regiones. Se recomienda elegir la primera región para que coincida con la ubicación del grupo de recursos de la sección Aspectos básicos. Para las redes desarrollo o prueba, se recomienda una sola región por miembro. Para entornos de producción, se recomienda implementar en dos o más regiones para alta disponibilidad.

A continuación se proporciona una descripción detallada de cada parámetro:

  Nombre de parámetro|DESCRIPCIÓN|Valores permitidos|Valores predeterminados
  ---|---|---|---
  Número de regiones|Número de regiones para implementar la red del consorcio|1, 2, 3, 4, 5|1
  Primera región|Primera región para implementar la red del consorcio|Todas las regiones de Azure permitidas|N/D
  Segunda región|Segunda región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 2)|Todas las regiones de Azure permitidas|N/D
  Tercera región|Tercera región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 3)|Todas las regiones de Azure permitidas|N/D
  Cuarta región|Cuarta región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 4)|Todas las regiones de Azure permitidas|N/D
  Quinta región|Quinta región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 5)|Todas las regiones de Azure permitidas|N/D

A continuación se muestra una implementación de ejemplo: ![Regiones de implementación](./media/ethereum-poa-deployment/deployment-regions.png)

#### <a name="network-size-and-performance"></a>Tamaño de red y rendimiento

Después, en “Tamaño de red y rendimiento” especifique las entradas para el tamaño de la red del consorcio, como el número y tamaño de los nodos de validador.
El tamaño de almacenamiento del nodo de validador determinará el tamaño potencial de la cadena de bloques. Esto se puede cambiar después de la implementación.

A continuación se proporciona una descripción detallada de cada parámetro:

  Nombre de parámetro|DESCRIPCIÓN|Valores permitidos|Valores predeterminados
  ---|---|---|---
  Number of load balanced validator nodes (Número de nodos de validador con equilibrio de carga)|Número de nodos de validador para aprovisionamiento como parte de la red.|2-15|2
  Validator node storage performance (Rendimiento del almacenamiento de los nodos de validador)|Tipo de disco administrado que realiza una copia de seguridad de cada uno de los nodos de validador implementado.|SSD estándar o Premium|SSD estándar
  Validator node virtual machine size (Tamaño de la máquina virtual del nodo de validador)|Tamaño de la máquina virtual usada para los nodos de validador.|Estándar A, Estándar D, Estándar D-v2, serie Estándar F, Estándar DS y Estándar FS|Estándar D1 v2

[Detalles de precios de almacenamiento](https://azure.microsoft.com/pricing/details/managed-disks/)

[Detalles de precios de la máquina virtual](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)

Máquina virtual y la capa de almacenamiento afectará al rendimiento de red.  En función de la eficacia de costos deseada, recomendamos las siguientes SKU:

  SKU de máquina virtual|Capa de almacenamiento|Precio|Throughput|Latencia
  ---|---|---|---|---
  F1|SSD estándar|bajo|bajo|alta
  D2_v3|SSD estándar|medio|medio|medio
  F16s|SSD Premium|alta|alta|bajo

A continuación se muestra una implementación de ejemplo: ![Tamaño de red y rendimiento](./media/ethereum-poa-deployment/network-size-and-performance.png)

#### <a name="ethereum-settings"></a>Configuración de Ethereum

Después, en Configuración de Ethereum, especifique las opciones de configuración relacionadas con Ethereum, como el identificador de red y la contraseña de la cuenta de Ethereum, o el bloque génesis.

A continuación se proporciona una descripción detallada de cada parámetro:

  Nombre de parámetro|DESCRIPCIÓN|Valores permitidos|Valores predeterminados
  ---|---|---|---
Consortium Member ID (Identificador de miembro del consorcio)|El identificador asociado con cada miembro que participa en la red del consorcio usada para configurar los espacios de direcciones IP para evitar la colisión. En el caso de una red privada, el identificador de miembro debe ser único en las diferentes organizaciones de la misma red.  Es necesario un identificador de miembro único incluso si la misma organización se implementa en varias regiones. Tome nota del valor de este parámetro, ya que necesitará compartirlo con otros miembros para asegurarse de que no hay ningún conflicto de combinación.|0-255|N/D
Id. de red|El identificador de red para la red del consorcio de Ethereum que se está implementando.  Cada red de Ethereum tiene su propio identificador de red, siendo 1 el identificador de la red pública.|De 5 a 999 999 999|10101010
Admin Ethereum Address (Dirección de Ethereum del administrador)|Dirección de cuenta de Ethereum que se usa para participar en la gobernanza de prueba de autoridad.  Se recomienda usar MetaMask para generar una dirección de Ethereum.|42 caracteres alfanuméricos empezando con 0x|N/D
Opciones avanzadas|Opciones avanzadas de configuración de Ethereum|Habilitar o Deshabilitar|Disable
Dirección IP pública (Opciones avanzadas = Habilitar)|Implementa la red detrás de una puerta de enlace de red virtual y quita el acceso de emparejamiento. Si se selecciona esta opción, todos los miembros deben usar una puerta de enlace de red virtual para que la conexión sea compatible.|Red privada virtual de dirección IP pública|Dirección IP pública
Límite de gas del bloque (opciones avanzadas = Habilitar)|El límite de gas del bloque inicial de la red|Cualquier elemento numérico|50000000
Período de resellado del bloque (s)|La frecuencia con que se crearán bloques vacíos cuando no hay ninguna transacción en la red. Una frecuencia mayor finalizará más rápido, pero los costos de almacenamiento serán superiores.|Cualquier elemento numérico|15
Transaction Permission Contract (Contrato de permiso de transacciones) (Opciones avanzadas = Habilitar)|Código de bytes para el contrato de permisos de la transacción. Restringe la implementación de contrato inteligente y la ejecución de una lista de cuentas de Ethereum.|Código de bytes de contrato|N/D

A continuación se muestra una implementación de ejemplo: ![Configuración de Ethereum](./media/ethereum-poa-deployment/ethereum-settings.png)

#### <a name="monitoring"></a>Supervisión

La hoja supervisión le permite configurar un recurso de los registros de Azure Monitor para la red. El agente de supervisión recopilará y expondrá métricas y registros útiles de la red, lo que proporciona la capacidad de comprobar rápidamente el estado de la red o los problemas de depuración.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

  Nombre de parámetro|DESCRIPCIÓN|Valores permitidos|Valores predeterminados
  ---|---|---|---
Supervisión|Opción para habilitar la supervisión|Habilitar o Deshabilitar|Habilitar
Conectarse a los registros existentes de Azure Monitor|Crear una nueva instancia de los registros de Azure Monitor o unirse a una instancia existente|Crear nuevo o Unirse a existente|Crear nuevo
Supervisar la ubicación (conectar a los registros existentes de Azure Monitor = Create new)|La región donde el nuevo Monitor de Azure registra la instancia que se va a implementar|Monitor de Azure todos los registros de regiones|N/D
Identificador del área de trabajo existente log analytics (conectar a los registros existentes de Azure Monitor = unir existente)|Id. de área de trabajo del Monitor de Azure existentes de los registros de instancia||N/D
Clave principal de Existing log analytics (conectar a los registros existentes de Azure Monitor = unir existente)|La clave principal que se usa para conectarse a la instancia existente de los registros de Azure Monitor||N/D


A continuación se muestra una implementación de ejemplo: ![Azure Monitor](./media/ethereum-poa-deployment/azure-monitor.png)

#### <a name="summary"></a>Resumen

Haga clic en la hoja de resumen para revisar las entradas especificadas y ejecutar una validación básica anterior a la implementación. Antes de realizar la implementación, puede descargar la plantilla y los parámetros.

Revise los términos legales y de privacidad y haga clic en “Comprar” para implementar. Si la implementación incluye puertas de enlace de red virtual, la implementación tardará entre 45 y 50 minutos.

#### <a name="post-deployment"></a>Después de la implementación

##### <a name="deployment-output"></a>Salida de la implementación

Una vez completada la implementación, puede tener acceso a los parámetros necesarios mediante el correo electrónico de confirmación o a través del portal de Azure. En estos parámetros encontrará lo siguiente:

-   Punto de conexión RPC de Ethereum

-   Dirección URL del panel de gobernanza

-   Dirección URL de Azure Monitor

-   Dirección URL de los datos

-   VNet Gateway Resource ID (opcional)

##### <a name="confirmation-email"></a>Correo electrónico de confirmación

Si proporciona una dirección de correo electrónico ([sección Aspectos básicos](#basics)), se envía un correo electrónico a dicha dirección con la información de salida de la implementación.

![correo electrónico de implementación](./media/ethereum-poa-deployment/deployment-email.png)

##### <a name="portal"></a>Portal

Una vez que la implementación haya finalizado correctamente y todos los recursos se hayan aprovisionado puede ver los parámetros de salida en el grupo de recursos.

1.  Localice el grupo de recursos en el portal

2.  Vaya a *Implementaciones*

3.  Seleccione la implementación superior con el mismo nombre que el grupo de recursos

4.  Seleccione *Salidas*

### <a name="growing-the-consortium"></a>Crecimiento del consorcio

Para expandir el consorcio, primero debe conectar la red física.
Mediante la implementación basada en direcciones IP públicas, este primer paso no da problemas. Si implementa detrás de una VPN, consulte la sección [conexión de puerta de enlace de red virtual](#connecting-vnet-gateways) para realizar la conexión de red como parte de la nueva implementación de miembro.  Una vez finalizada la implementación, use la [aplicación descentralizada de gobernanza](#governance-dapp) para convertirse en administrador de red.

#### <a name="new-member-deployment"></a>Implementación de un nuevo miembro

1.  Comparta la siguiente información con el miembro que se une. Esta información puede encontrarse en el correo electrónico de posterior a la implementación o en la salida de implementación del portal.

    -  Dirección URL de datos del consorcio

    -  El número de nodos que se ha implementado

    -  Identificador del recurso de puerta de enlace de red virtual (si se usa VPN)

2.  El miembro que se implementa debe usar el [misma solución](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) al implementar su presencia en la red teniendo presente lo siguiente:

    -  Seleccione *Unirse a existente*

    -  Elija el mismo número de nodos de validador que el resto de los miembros de la red para garantizar una representación razonable

    -  Use la misma dirección de Ethereum que se proporcionó en el paso anterior

    -  Pase la *dirección URL de datos del consorcio* proporcionada en la pestaña *Configuración de Ethereum*

    -  Si el resto de la red está detrás de una VPN, seleccione *Red virtual privada* bajo la sección avanzada

#### <a name="connecting-vnet-gateways"></a>Conexión de puertas de enlace de red virtual

Puede omitir este paso si ha realizado la implementación con la configuración de la dirección IP pública predeterminada. En el caso de una red privada, los diferentes miembros se conectan a través de conexiones de puerta de enlace de red virtual. Antes de que un miembro puede unirse a la red y ver el tráfico de la transacción, un miembro existente debe realizar una configuración final en su puerta de enlace VPN para aceptar la conexión. Esto significa que los nodos Ethereum del miembro de unión no se ejecutarán hasta que se establezca una conexión. Se recomienda para crear conexiones de red redundantes (malla) en el consorcio para reducir las posibilidades de un único punto de error.

Después de que el nuevo miembro se implemente, el miembro existente debe completar la conexión bidireccional configurando una conexión de puerta de enlace de red virtual con el nuevo miembro. Para lograrlo, el miembro existente necesitará:

1.  El identificador del recurso de la puerta de enlace de red virtual del miembro que se conecta (consulte la salida de la implementación)

2.  La clave de conexión compartida

El miembro existente debe ejecutar el siguiente script de PowerShell para completar la conexión. Se recomienda usar Azure Cloud Shell ubicado en la barra de navegación de la parte superior derecha en el portal.

![cloud shell](./media/ethereum-poa-deployment/cloud-shell.png)

```Powershell
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

### <a name="service-monitoring"></a>Supervisión de servicios

Puede localizar el portal de Azure Monitor siguiendo el vínculo del correo electrónico de implementación o buscando el parámetro en la salida de la implementación \[OMS\_PORTAL\_URL\].

El portal mostrará primero las estadísticas de red de alto nivel e información general del nodo.

![categorías de monitor](./media/ethereum-poa-deployment/monitor-categories.png)

Al hacer clic en la **Node Overview** (Información general del nodo) se le dirigirá a un portal para ver las estadísticas por nodo.

![estadísticas del nodo](./media/ethereum-poa-deployment/node-stats.png)

Si selecciona **Network Stats** (Estadísticas de red) será útil para ver las estadísticas de red de Ethereum.

![estadísticas de red](./media/ethereum-poa-deployment/network-stats.png)

#### <a name="sample-kusto-queries"></a>Ejemplos de consultas de Kusto

Detrás de estos paneles hay un conjunto de registros sin procesar que se puede consultar. Puede usar estos registros sin procesar para personalizar los paneles, investigar los errores o configurar el umbral de alertas. A continuación encontrará un conjunto de consultas de ejemplo que se pueden ejecutar en la herramienta de búsqueda de registros:

##### <a name="lists-blocks-that-have-been-reported-by-more-than-one-validator-useful-to-help-find-chain-forks"></a>Enumera los bloques modificados por más de un validador. Resulta útil para ayudar a encontrar bifurcaciones de cadena.

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

##### <a name="get-average-peer-count-for-a-specified-validator-node-averaged-over-5-minute-buckets"></a>Obtenga el recuento de promedio de pares para un nodo de validador especificado promediado sobre cubos de cinco minutos.

```sql
let PeerCountRegex = @"Syncing with peers: (\d+) active, (\d+) confirmed, (\d+)";
ParityLog_CL
| where Computer == "vl-devn3lgdm-reg1000001"
| project RawData, TimeGenerated
| where RawData matches regex PeerCountRegex
| extend ActivePeers = extract(PeerCountRegex, 1, RawData, typeof(int))
| summarize avg(ActivePeers) by bin(TimeGenerated, 5m)
```

### <a name="ssh-access"></a>Acceso SSH

Por motivos de seguridad y de forma predeterminada, una regla de seguridad de grupo de red deniega el acceso al puerto SSH. Para obtener acceso a las instancias de máquina virtual en la red de PoA, deberá cambiar esta regla de \"permitir\"

1.  Empiece en la sección Información general del grupo de recursos implementados de Azure Portal.

    ![información general de ssh](./media/ethereum-poa-deployment/ssh-overview.png)

2.  Seleccione el grupo de seguridad de red para la región de la máquina virtual a la que quiere acceder

    ![nsg de ssh](./media/ethereum-poa-deployment/ssh-nsg.png)

3.  Seleccione la regla \"permitir-ssh\".

    ![ssh-allow](./media/ethereum-poa-deployment/ssh-allow.png)

4.  Cambie \"Acción\" a Permitir.

    ![habilitar permitir en ssh](./media/ethereum-poa-deployment/ssh-enable-allow.png)

5.  Haga clic en \"Guardar\" (los cambios pueden tardar unos minutos en aplicarse)

Ahora puede conectarse de forma remota a las máquinas virtuales para los nodos de validador a través de SSH con el nombre de usuario y la contraseña de administrador o la clave SSH que se haya provisto.
El comando SSH que se debe ejecutar para acceder al primer nodo de validador se muestra en el parámetro de resultado de la implementación de la plantilla como “SSH\_TO\_FIRST\_VL\_NODE\_REGION1” (para la implementación de ejemplo: ssh - p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com). Para obtener nodos de transacción adicionales, aumente el número de puerto en uno (por ejemplo, el primer nodo de transacción está en el puerto 4000).

Si realizó la implementación en más de una región, cambie el comando anterior por el nombre DNS o la dirección IP del equilibrador de carga en esa región. Para encontrar el nombre DNS o la dirección IP de las otras regiones, busque el recurso con la convención de nomenclatura \*\*\*\*\*-lbpip-reg\# y vea las propiedades de su nombre DNS y dirección IP.

### <a name="azure-traffic-manager-load-balancing"></a>Equilibrio de carga de Azure Traffic Manager

Azure Traffic Manager puede ayudar a reducir el tiempo de inactividad y a mejorar la capacidad de respuesta de la red de prueba de autoridad mediante el enrutamiento de tráfico entrante a través de varias implementaciones en diferentes regiones. Las comprobaciones de estado incorporadas y el reenrutamiento automático ayudan a garantizar una alta disponibilidad de los puntos de conexión RPC y la aplicación descentralizada de gobernanza. Esta característica es útil si ha implementado en varias regiones y están listas para producción.

Use Traffic Manager para:

-   Mejorar la disponibilidad de la red de prueba de autoridad con conmutación automática por error.

-   Aumentar la capacidad de respuesta de las redes enrutamiento a los usuarios finales a la ubicación de Azure con la menor latencia de red.

Si decide crear un perfil de Traffic Manager, puede usar el nombre DNS del perfil para acceder a la red. Una vez que otros miembros del consorcio se hayan agregado a la red, Traffic Manager también se puede usar para el equilibrio de carga entre sus validadores desplegados.

#### <a name="creating-a-traffic-manager-profile"></a>Creación de un perfil de Traffic Manager

Busque y seleccione \"Perfil de Traffic Manager\" tras hacer clic en el botón \"Crear un recurso\" en Azure Portal.

![buscar azure traffic manager](./media/ethereum-poa-deployment/traffic-manager-search.png)

Asigne un nombre único al perfil y seleccione el grupo de recursos que se creó durante la implementación de prueba de autoridad. Haga clic en el botón “Crear” para llevar a cabo la implementación.

![crear traffic manager](./media/ethereum-poa-deployment/traffic-manager-create.png)

Una vez implementado, a continuación, seleccione la instancia del grupo de recursos. El nombre DNS para acceder al administrador de tráfico puede encontrarse en la pestaña Información general

![Localizar DNS de Traffic Manager](./media/ethereum-poa-deployment/traffic-manager-dns.png)

Seleccione la pestaña Puntos de conexión y haga clic en el botón Agregar. Asigne un nombre único al punto de conexión. Cambie el tipo de recurso de destino a una dirección IP pública. Luego, seleccione la dirección IP pública del equilibrador de carga de la primera\' región.

![Enrutamiento de Traffic Manager](./media/ethereum-poa-deployment/traffic-manager-routing.png)

Repita para cada región de la red implementada. Una vez que los extremos están en el \"habilitado\" estado, se le cargan automáticamente y en el nombre DNS del Administrador de tráfico con equilibrio de región. Ahora puede usar este nombre DNS en lugar del parámetro \[CONSORTIUM\_DATA\_URL\] en otros pasos del documento.

### <a name="data-api"></a>API de datos

Cada miembro del consorcio hospeda la información necesaria para que otros usuarios se conecten a la red. El miembro existente le proporcionará la CONSORTIUM_DATA_URL antes de la implementación del miembro. Tras la implementación, un miembro de unión recuperará la información de la interfaz JSON en el punto de conexión siguiente:

`<CONSORTIUM_DATA_URL>/networkinfo`

La respuesta contendrá información útil para unirse a los miembros (bloque de génesis, contrato validador establece ABI, bootnodes) e información útil para el miembro existente (direcciones de control de validación). Se recomienda el uso de esta normalización para extender el consorcio a través de proveedores de nube. Esta API devolverá una respuesta con formato JSON con la estructura siguiente:
```json
{
  "$id": "",
  "type": "object",
  "definitions": {},
  "$schema": "https://json-schema.org/draft-07/schema#",
  "properties": {
    "majorVersion": {
      "$id": "/properties/majorVersion",
      "type": "integer",
      "title": "This schema’s major version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "minorVersion": {
      "$id": "/properties/minorVersion",
      "type": "integer",
      "title": "This schema’s minor version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "bootnodes": {
      "$id": "/properties/bootnodes",
      "type": "array",
      "items": {
        "$id": "/properties/bootnodes/items",
        "type": "string",
        "title": "This member’s bootnodes",
        "default": "",
        "examples": [
          "enode://a348586f0fb0516c19de75bf54ca930a08f1594b7202020810b72c5f8d90635189d72d8b96f306f08761d576836a6bfce112cfb6ae6a3330588260f79a3d0ecb@10.1.17.5:30300",
          "enode://2d8474289af0bb38e3600a7a481734b2ab19d4eaf719f698fe885fb239f5d33faf217a860b170e2763b67c2f18d91c41272de37ac67386f80d1de57a3d58ddf2@10.1.17.4:30300"
        ]
      }
    },
    "valSetContract": {
      "$id": "/properties/valSetContract",
      "type": "string",
      "title": "The ValidatorSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\n\nimport \"./SafeMath.sol\";\nimport \"./Utils.sol\";\n\ncontract ValidatorSet …"
      ]
    },
    "adminContract": {
      "$id": "/properties/adminContract",
      "type": "string",
      "title": "The AdminSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\nimport \"./SafeMath.sol\";\nimport \"./SimpleValidatorSet.sol\";\nimport \"./Admin.sol\";\n\ncontract AdminValidatorSet is SimpleValidatorSet { …"
      ]
    },
    "adminContractABI": {
      "$id": "/properties/adminContractABI",
      "type": "string",
      "title": "The Admin Contract ABI",
      "default": "",
      "examples": [
        "[{\"constant\":false,\"inputs\":[{\"name\":\"proposedAdminAddress\",\"type\":\"address\"},…"
      ]
    },
    "paritySpec": {
      "$id": "/properties/paritySpec",
      "type": "string",
      "title": "The Parity client spec file",
      "default": "",
      "examples": [
        "\n{\n \"name\": \"PoA\",\n \"engine\": {\n \"authorityRound\": {\n \"params\": {\n \"stepDuration\": \"2\",\n \"validators\" : {\n \"safeContract\": \"0x0000000000000000000000000000000000000006\"\n },\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\"\n }\n }\n },\n \"params\": {\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\",\n \"wasmActivationTransition\": \"0x0\"\n },\n \"genesis\": {\n \"seal\": {\n \"authorityRound\": {\n \"step\": \"0x0\",\n \"signature\": \"0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\"\n }\n },\n \"difficulty\": \"0x20000\",\n \"gasLimit\": \"0x2FAF080\"\n },\n \"accounts\": {\n \"0x0000000000000000000000000000000000000001\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ecrecover\", \"pricing\": { \"linear\": { \"base\": 3000, \"word\": 0 } } } },\n \"0x0000000000000000000000000000000000000002\": { \"balance\": \"1\", \"builtin\": { \"name\": \"sha256\", \"pricing\": { \"linear\": { \"base\": 60, \"word\": 12 } } } },\n \"0x0000000000000000000000000000000000000003\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ripemd160\", \"pricing\": { \"linear\": { \"base\": 600, \"word\": 120 } } } },\n \"0x0000000000000000000000000000000000000004\": { \"balance\": \"1\", \"builtin\": { \"name\": \"identity\", \"pricing\": { \"linear\": { \"base\": 15, \"word\": 3 } } } },\n \"0x0000000000000000000000000000000000000006\": { \"balance\": \"0\", \"constructor\" : \"…\" }\n }\n}"
      ]
    },
    "errorMessage": {
      "$id": "/properties/errorMessage",
      "type": "string",
      "title": "Error message",
      "default": "",
      "examples": [
        ""
      ]
    },
    "addressList": {
      "$id": "/properties/addressList",
      "type": "object",
      "properties": {
        "addresses": {
          "$id": "/properties/addressList/properties/addresses",
          "type": "array",
          "items": {
            "$id": "/properties/addressList/properties/addresses/items",
            "type": "string",
            "title": "This member’s validator addresses",
            "default": "",
            "examples": [
              "0x00a3cff0dccc0ecb6ae0461045e0e467cff4805f",
              "0x009ce13a7b2532cbd89b2d28cecd75f7cc8c0727"
            ]
          }
        }
      }
    }
  }
}

```
## <a name="tutorials"></a>Tutoriales

### <a name="programmatically-interacting-with-a-smart-contract"></a>Interacción mediante programación con un contrato inteligente

> [!WARNING]
> ¡Nunca envíe la clave privada de Ethereum a través de la red! Asegúrese de que cada transacción primero se firma localmente y la transacción firmada se envía a través de la red.

En el siguiente ejemplo, usamos *ethereumjs-wallet* para generar una dirección de Ethereum, *ethereumjs-tx* para firmar localmente y *web3* para enviar la transacción sin procesar al punto de conexión RPC de Ethereum.

Vamos a usar este sencillo contrato inteligente Hola mundo para este ejemplo:

```javascript
pragma solidity ^0.4.11;
contract postBox {
    string message;
    function postMsg(string text) public {
        message = text;
    }
    function getMsg() public view returns (string) {
        return message;
    }
}
```

En este ejemplo se da por supuesto que ya se ha implementado el contrato. Puede usar *solc* y *web3* para implementar un contrato mediante programación. Primero instale los siguientes módulos de nodo:
```
sudo npm install web3@0.20.2
sudo npm install ethereumjs-tx@1.3.6
sudo npm install ethereumjs-wallet@0.6.1
```
Este script de nodeJS realizará las siguientes acciones:

-   Construir una transacción sin procesar: postMsg

-   Firmar la transacción mediante la clave privada generada

-   Enviar la transacción firmada a la red de Ethereum

```javascript
var ethereumjs = require('ethereumjs-tx')
var wallet = require('ethereumjs-wallet')
var Web3 = require('web3')

// TODO Replace with your contract address
var address = "0xfe53559f5f7a77125039a993e8d5d9c2901edc58";
var abi = [{"constant": false,"inputs": [{"name": "text","type": "string"}],"name": "postMsg","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "getMsg","outputs": [{"name": "","type": "string"}],"payable": false,"stateMutability": "view","type": "function"}];

// Generate a new Ethereum account
var account = wallet.generate();
var accountAddress = account.getAddressString()
var privateKey = account.getPrivateKey();

// TODO Replace with your RPC endpoint
var web3 = new Web3(new Web3.providers.HttpProvider(
    "http://testzvdky-dns-reg1.eastus.cloudapp.azure.com:8545"));

// Get the current nonce of the account
web3.eth.getTransactionCount(accountAddress, function (err, nonce) {
   var data = web3.eth.contract(abi).at(address).postMsg.getData("Hello World");
   var rawTx = {
     nonce: nonce,
     gasPrice: '0x00',
     gasLimit: '0x2FAF080',
     to: address,
     value: '0x00',
     data: data
   }
   var tx = new ethereumjs(rawTx);

   tx.sign(privateKey);

   var raw = '0x' + tx.serialize().toString('hex');
   web3.eth.sendRawTransaction(raw, function (txErr, transactionHash) {
     console.log("TX Hash: " + transactionHash);
     console.log("Error: " + txErr);
   });
 });
```

### <a name="deploy-smart-contract-with-truffle"></a>Implementar un contrato inteligente con Truffle

-   Instalación de las bibliotecas necesarias

```javascript
npm init

npm install truffle-hdwallet-provider --save
```
-   En truffle.js, agregue el siguiente código para desbloquear su cuenta de MetaMask y configure el nodo PoA como punto de entrada proporcionando la frase mnemotécnica (MetaMask / Settings / Reveal Seed Words).

```javascript
var HDWalletProvider = require("truffle-hdwallet-provider");

var rpc_endpoint = "XXXXXX";
var mnemonic = "twelve words you can find in metamask/settings/reveal seed words";

module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    },
    poa: {
      provider: new HDWalletProvider(mnemonic, rpc_endpoint),
      network_id: 3,
      gasPrice : 0
    }
  }
};

```

-   Implemente en la red de PoA.

```javascript
$ truffle migrate --network poa
```

### <a name="debug-smart-contract-with-truffle"></a>Depurar un contrato inteligente con Truffle

Truffle tiene una red de desarrollo local que está disponible para depurar el contrato inteligente. Podrá encontrar el tutorial completo [aquí](https://truffleframework.com/tutorials/debugging-a-smart-contract).

### <a name="webassembly-wasm-support"></a>Compatibilidad con WebAssembly (WASM)

La compatibilidad con WebAssembly ya está habilitada automáticamente en redes de prueba de autoridad recién implementadas. Permite el desarrollo de contratos inteligentes en cualquier lenguaje que transpile a WebAssembly (Rust, C, C++). Consulte los vínculos siguientes para información adicional:

-   Información general de Parity de WebAssembly: <https://wiki.parity.io/WebAssembly-Home>

-   Tutorial de Parity Tech: <https://github.com/paritytech/pwasm-tutorial>

## <a name="reference"></a>Referencia

### <a name="faq"></a>Preguntas más frecuentes

#### <a name="i-notice-there-are-many-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>Observo que hay muchas transacciones en la red que no envié. ¿De dónde vienen?

No es seguro desbloquear la [API personal](https://web3js.readthedocs.io/en/1.0/web3-eth-personal.html). Los bots escuchan cuentas desbloqueadas de Ethereum e intentan vaciar los fondos. El bot supone que estas cuentas contienen Ether real e intenta ser el primero en desviar el saldo. No habilite la API personal en la red. En su lugar, firme previamente las transacciones ya sea manualmente, usando un monedero como MetaMask, o mediante programación, como se describe en la sección [Interacción mediante programación con un contrato inteligente](#programmatically-interacting-with-a-smart-contract).

#### <a name="how-to-ssh-onto-a-vm"></a>¿Cómo aplicar SSH a una máquina virtual?

El puerto SSH no se expone por motivos de seguridad. Siga [esta guía para habilitar el puerto SSH](#ssh-access).

#### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>¿Cómo configuro un miembro de auditoría o nodos de transacción?

Los nodos de transacciones son un conjunto de clientes de Parity que están emparejados con la red, pero no participan en el consenso. Estos nodos aún se pueden utilizar para enviar las transacciones de Ethereum y leer el estado del contrato inteligente.
Esto funciona bien como mecanismo para proporcionar capacidad de auditoría a los miembros del consorcio que no son de auditoría en la red. Para lograr esto simplemente siga el paso 2 de la sección Crecimiento del consorcio.

#### <a name="why-are-metamask-transactions-taking-a-long-time"></a>¿Por qué las transacciones de MetaMask tardan mucho tiempo?

Para asegurarse de que las transacciones se reciben en el orden correcto, cada transacción de Ethereum viene con un valor nonce incremental. Si ha usado una cuenta de MetaMask en una red distinta, deberá restablecer el valor nonce. Haga clic en el icono de configuración (3 barras), Configuración, Restablecer cuenta. El historial de transacciones se borrará y ahora podrá volver a enviar la transacción.

#### <a name="do-i-need-to-specify-gas-fee-in-metamask"></a>¿Es necesario especificar la cuota de gas en MetaMask?

La finalidad de Ethereum no es un consorcio de prueba de autoridad. Por lo tanto, no es necesario especificar una cuota de gas al enviar las transacciones en MetaMask.

#### <a name="what-should-i-do-if-my-deployment-fails-due-to-failure-to-provision-azure-oms"></a>¿Qué debo hacer si mi implementación falla debido a un error al aprovisionar Azure OMS?

La supervisión es una característica opcional. En algunos casos excepcionales, en los que la implementación falla debido a la incapacidad de aprovisionar correctamente el recurso de Azure Monitor, puede volver a realizar la implementación sin Azure Monitor.

#### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>¿Las implementaciones de IP pública son compatibles con las implementaciones de red privada?

No, el emparejamiento requiere una comunicación bidireccional, por lo que toda la red debe ser pública o privada.

#### <a name="what-is-the-expected-transaction-throughput-of-proof-of-authority"></a>¿Cuál es el rendimiento de la transacción esperado en la prueba de autoridad?

El rendimiento de las transacciones dependerá en gran medida de los tipos de transacciones y la topología de red.  Usando transacciones sencillas, hemos marcado un promedio de 400 transacciones por segundo con una red implementada en varias regiones.

#### <a name="how-do-i-subscribe-to-smart-contract-events"></a>¿Cómo puedo suscribirme a eventos de contrato inteligente?

Ahora la prueba de autoridad de Ethereum admite sockets web.  Consulte su correo electrónico de implementación o el resultado de la implementación para buscar el puerto y la URL de socket web.

## <a name="next-steps"></a>Pasos siguientes

Introducción al uso de la solución[Consorcio de prueba de autoridad de Ethereum](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium).
