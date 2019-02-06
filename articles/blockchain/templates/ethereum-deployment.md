---
title: Plantilla de solución del consorcio de prueba de trabajo de Ethereum
description: Usar la plantilla de solución del consorcio de prueba de trabajo de Ethereum para implementar y configurar una red con varios miembros del consorcio Ethereum
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/28/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: 266e2be2775a6f9b74c714bd9112e38837bb6a6c
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098345"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Plantilla de solución del consorcio de prueba de trabajo de Ethereum

La plantilla de solución del consorcio de prueba de trabajo de Ethereum está diseñada para que sea más fácil y rápido implementar y configurar una red con varios miembros del consorcio Ethereum con un conocimiento mínimo de Azure y Ethereum.

Mediante una plantilla de Azure Resource Manager, cada miembro puede aprovisionar su superficie de red mediante los servicios de proceso, redes y almacenamiento de Microsoft Azure. La superficie de red de cada miembro se compone de un conjunto de nodos de transacción de carga equilibrada con el que una aplicación o un usuario interactúa para enviar las transacciones, un conjunto de nodos de minería de datos para registrar transacciones y una puerta de enlace de VPN. Después de la implementación, puede conectar las puertas de enlace para crear una red de cadena de bloques de varios miembros totalmente configurada.

## <a name="about-blockchain"></a>Acerca de la cadena de bloques

Para aquellos que no están familiarizados con la comunidad de cadena de bloques, el lanzamiento de esta solución es una excelente oportunidad para obtener información sobre la tecnología de una manera fácil y que se puede configurar en Azure. Pero, para empezar, se recomienda implementar la topología de red de Ethereum independiente más sencilla mediante este tutorial guiado, antes de crear redes del consorcio con varios miembros.

### <a name="mining-node-details"></a>Detalles del nodo de minería de datos

Los nodos que extraen las transacciones están separados de los que aceptan las transacciones para asegurarse de que las dos acciones no compiten por los mismos recursos.

Un miembro del consorcio puede aprovisionar hasta cinco regiones que contengan uno o más nodos de minería de datos, respaldados por un disco administrado. Uno o más nodos en la región se configuran como nodo de arranque para que admita la detección dinámica de los nodos de la red. Los nodos de minería de datos se comunican con otros nodos de minería de datos para llegar a un consenso sobre el estado del libro de contabilidad distribuida subyacente. No es necesario que la aplicación tenga en cuenta estos nodos ni que se comunique con ellos. Al centrarse en redes privadas, los nodos de minería de datos están aislados del tráfico de Internet público entrante para agregar una capa de protección secundaria. Se permite el tráfico saliente, pero no para el puerto de detección de Ethereum.

Todos los nodos tienen una versión estable del cliente de Go Ethereum (Geth) y están configurados como nodos de minería de datos. Si no proporcionó un bloque génesis personalizado, todos los nodos usan la misma dirección de Ethereum y el par de claves que está protegido por la contraseña de la cuenta de Ethereum. La frase de contraseña de Ethereum proporcionada se usa para generar la cuenta predeterminada (coinbase) para cada nodo de minería de datos. A medida que los nodos de minería de datos extraen información, recopilan las cuotas que se agregan a esta cuenta.

El número de nodos de minería de datos por cada miembro del consorcio depende del tamaño global de la red deseada y la cantidad de poder de hash que se dedica a cada miembro. Cuanto mayor sea la red, más nodos se deben comprometer para obtener una ventaja desleal. La plantilla admite hasta 15 nodos de minería de datos por región aprovisionados con conjuntos de escalado de máquinas virtuales.

### <a name="transaction-node-details"></a>Detalles del nodo de transacción

Un miembro del consorcio también tiene un conjunto de nodos de transacción de carga equilibrada. Estos nodos son accesibles desde fuera de la red virtual, para que las aplicaciones puedan utilizarlos para enviar las transacciones o ejecutar contratos inteligentes dentro de la red de cadena de bloques. Todos los nodos tienen una versión estable del cliente de Go Ethereum (Geth) y están configurados para que mantengan una copia completa del libro de contabilidad distribuida. Si no se proporciona un bloque génesis personalizado, estos nodos usan la misma cuenta de Ethereum, protegida por la contraseña de la cuenta de Ethereum.

Los nodos de transacción tienen la carga equilibrada dentro de un conjunto de disponibilidad para mantener una alta disponibilidad. La plantilla admite hasta cinco nodos de transacción aprovisionados con conjuntos de escalado de máquinas virtuales.

### <a name="log-analytics-details"></a>Detalles de Log Analytics

Cada implementación también crea una nueva instancia de Log Analytics o puede unirse a una instancia existente. Log Analytics permite la supervisión de diversas métricas de rendimiento de cada máquina virtual que conforma la red implementada.

## <a name="deployment-architecture"></a>Arquitectura de implementación

### <a name="description"></a>DESCRIPCIÓN

Esta plantilla de solución puede implementar una red con varios miembros del consorcio Ethereum basada en una o en varias regiones. La red virtual de cada región está conectada a otra región en una topología de cadena mediante las puertas de enlace de red virtual y los recursos de conexión. También proporciona un registrador que contiene la información necesaria de todos los nodos de transacciones y de minería de datos implementados en cada región.

### <a name="standalone-and-consortium-leader-overview"></a>Información general de líder independiente y del consorcio

![Información general de líder independiente y del consorcio](./media/ethereum-deployment/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Información general de los miembros que se unen al consorcio

![Información general de los miembros que se unen al consorcio](./media/ethereum-deployment/member-overview.png)

## <a name="getting-started"></a>Introducción

Este proceso requiere una suscripción de Azure que puede admitir la implementación de varios conjuntos de escalado de máquinas virtuales y discos administrados. Si es necesario, cree una cuenta de Azure gratuita para comenzar.

Una vez que una suscripción está protegida, vaya al Azure Portal. Seleccione **+ Create a resource** (+ Crear un recurso), Marketplace (Ver todos) y busque **Ethereum Proof-of-Work Consortium** (Consorcio de prueba de trabajo de Ethereum).

La implementación de plantilla le guiará por el proceso de configuración del consumo de red del primer miembro. El flujo de implementación se divide en cinco pasos: aspectos básicos, Operations Management Suite, regiones de implementación, tamaño de red y rendimiento, configuración de Ethereum.

### <a name="basics"></a>Aspectos básicos

En **Aspectos básicos**, especifique valores para los parámetros estándares para cualquier implementación, como la suscripción, el grupo de recursos y las propiedades básicas de la máquina virtual.

![Aspectos básicos](./media/ethereum-deployment/sample-deployment.png)

Nombre de parámetro|DESCRIPCIÓN| Valores permitidos|Valores predeterminados
---|---|---|---
¿Crear una nueva red o unirse a una existente?|¿Crear una nueva red o unirse a una red existente del consorcio?|Crear nueva Unirse a existente|Crear nuevo
¿Implementar una red que formará parte de un consorcio?|Una red del consorcio permite que las futuras implementaciones se unan a esta red (visible cuando *Crear nueva* está seleccionada más arriba)|Consorcio independiente|Independiente
Prefijo de recurso |Cadena usada como base para asignar nombres a recursos (de 2 a 4 caracteres alfanuméricos). Se antepone un hash único a la cadena de algunos recursos, mientras que se anexa información específica de recursos.|Caracteres alfanuméricos con una longitud de 2 a 4|N/D
Nombre de usuario de máquina virtual| Nombre de usuario del administrador de cada máquina virtual implementada (solo caracteres alfanuméricos)|Entre 1 y 64 caracteres |gethadmin
Tipo de autenticación|El método de autenticación en la máquina virtual. |Contraseña o clave pública SSH|Contraseña
Contraseña (Tipo de autenticación = Contraseña)|La contraseña de la cuenta de administrador para cada una de las máquinas virtuales implementadas. La contraseña debe cumplir 3 de los siguientes requisitos: 1 letra mayúscula, 1 letra minúscula, 1 número y 1 carácter especial. <br />Aunque todas las máquinas virtuales tienen inicialmente la misma contraseña, puede cambiarla después del aprovisionamiento.|Entre 12 y 72 caracteres|N/D
Clave SSH (Tipo de autenticación = Clave pública)|La clave de shell seguro que se usa para el inicio de sesión remoto.|| N/D
Subscription| La suscripción en la que se va a implementar la red del consorcio.||N/D
Grupo de recursos| El grupo de recursos en el que se va a implementar la red del consorcio.||N/D
Ubicación| La región de Azure para el grupo de recursos. ||N/D

### <a name="operations-management-suite"></a>Operations Management Suite

Operations Management Suite (OMS) le permite configurar un recurso de OMS para la red. OMS recopilará y expondrá métricas y registros útiles de la red, lo que proporciona la capacidad de comprobar rápidamente el estado de la red o los problemas de depuración. La oferta gratuita de OMS producirá errores leves cuando que se alcance la capacidad.

![Creando nueva OMS](./media/ethereum-deployment/new-oms.png)

Nombre de parámetro|DESCRIPCIÓN| Valores permitidos|Valores predeterminados
---|---|---|---
Conectar a OMS existente|Crear una nueva instancia de Log Analytics o unirse a una instancia existente|Crear nueva Unirse a existente|Crear nueva ubicación de Log Analytics|La región donde se implementará el nuevo Log Analytics (Visible si *Crear nuevo* está seleccionado)
Identificador de área de trabajo de OMS existente|Nivel de servicio de OMS del identificador de área de trabajo de la instancia existente (visible si *Unirse a existente* está seleccionado)|Elija el plan de tarifa de la nueva instancia. Más información en https://azure.microsoft.com/pricing/details/log-analytics/ (visible si *Unirse a existente* está seleccionado)|Gratuito Independiente Por nodo|Gratuito
Clave principal de OMS existente|La clave principal utilizada para conectarse a la instancia existente de OMS (visible si *Unirse a existente* está seleccionado)

### <a name="deployment-regions"></a>Regiones de implementación

Después, en **Regiones de implementación**, especifique las entradas para **Número de regiones** para implementar la red del consorcio y la selección de regiones de Azure en función del número de regiones dado. El usuario puede implementarse en un máximo de cinco regiones.

![Regiones de implementación](./media/ethereum-deployment/deployment-regions.png)

Nombre de parámetro| DESCRIPCIÓN| Valores permitidos |Valores predeterminados
---|---|---|---
Número de regiones| Número de regiones para implementar la red del consorcio|1, 2, 3, 4, 5| 2
Primera región| Primera región para implementar la red del consorcio|Todas las regiones de Azure permitidas| Oeste de EE. UU.
Segunda región |Segunda región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 2)|Todas las regiones de Azure permitidas| Este de EE. UU
Tercera región| Tercera región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 3)|Todas las regiones de Azure permitidas| Centro de EE. UU.
Cuarta región| Cuarta región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 4)|Todas las regiones de Azure permitidas| Este de EE. UU. 2
Quinta región| Quinta región para implementar la red del consorcio (visible únicamente cuando se selecciona el número de regiones como 5)|Todas las regiones de Azure permitidas| Oeste de EE. UU. 2

### <a name="network-size-and-performance"></a>Tamaño de red y rendimiento

Después, en **Tamaño de red y rendimiento** especifique las entradas para el tamaño de la red del consorcio. Por ejemplo, el número y el tamaño de los nodos de minería y de transacción.

![Tamaño de red y rendimiento](./media/ethereum-deployment/network-size-performance.png)

Nombre de parámetro |DESCRIPCIÓN |Valores permitidos| Valores predeterminados
---|---|---|---
Número de nodos de minería de datos|El número de nodos de minería de datos implementado por región.|De 2 a 15| 2
Rendimiento del almacenamiento de los nodos de minería de datos|El tipo de disco administrado que realiza una copia de seguridad de cada uno de los nodos de minería de datos implementado.|Estándar o Premium|Estándar
Tamaño de la máquina virtual del nodo de minería de datos|El tamaño de la máquina virtual usada para los nodos de minería de datos.|Estándar A, <br />Estándar D, <br />Estándar D-v2, <br />Serie F estándar, <br />Estándar DS, <br />y Estándar FS|Estándar D1v2
Número de nodos de transacción de carga equilibrada|El número de nodos de transacción para aprovisionamiento como parte de la red.|De 1 a 5| 2
Rendimiento del almacenamiento de los nodos de transacción|El tipo de disco administrado que realiza una copia de seguridad de cada uno de los nodos de transacción implementado.|Estándar o Premium|Estándar
Tamaño de la máquina virtual del nodo de transacción|El tamaño de la máquina virtual usada para los nodos de transacción.|Estándar A, <br />Estándar D, <br />Estándar D-v2, <br />Serie F estándar, <br />Estándar DS, <br />y Estándar FS|Estándar D1v2

### <a name="ethereum-settings"></a>Configuración de Ethereum

Después, en **Configuración de Ethereum**, especifique las opciones de configuración relacionadas con Ethereum, como el identificador de red y la contraseña de la cuenta de Ethereum, o el bloque génesis.

![Configuración de Ethereum](./media/ethereum-deployment/standalone-leader-deployment.png)

Nombre de parámetro |DESCRIPCIÓN |Valores permitidos|Valores predeterminados
---|---|---|---
Identificador ConsortiumMember|El identificador asociado con cada miembro que participa en la red del consorcio usada para configurar los espacios de direcciones IP para evitar la colisión. <br /><br />El identificador de miembro debe ser único en las diferentes organizaciones de la misma red. Es necesario un identificador de miembro único incluso si la misma organización se implementa en varias regiones.<br /><br />Tome nota del valor de este parámetro, ya que necesitará compartirlo con otros miembros que se estén uniendo.|De 0 a 255
Identificador de red de Ethereum|El identificador de red para la red del consorcio de Ethereum que se está implementando. Cada red de Ethereum tiene su propio identificador de red, siendo 1 el identificador de la red pública. Aunque el acceso a la red está restringido para los nodos de minería de datos, todavía sigue siendo recomendable usar un número grande para evitar las colisiones.|De 5 a 999 999 999| 10101010
Bloque génesis personalizado|Posibilidad de generar automáticamente un bloque génesis o proporcionar uno personalizado.|Sí/No| Sin 
Contraseña de la cuenta de Ethereum (Bloque de génesis personalizado = No)|La contraseña de administrador usada para proteger la cuenta de Ethereum importada en cada nodo. La contraseña debe contener: 1 letra mayúscula, 1 letra minúscula y 1 número.|12 o más caracteres|N/D
Frase de contraseña de clave privada de Ethereum (Bloque de génesis personalizado = No)|La frase de contraseña usada para generar la clave privada de ECC asociada con la cuenta de Ethereum predeterminada que se genera. No es necesario que una clave privada generada previamente se apruebe de forma explícita.<br /><br />Considere la posibilidad de una frase de contraseña con aleatoriedad suficiente para garantizar una clave privada segura y que no se superponga con otros miembros del consorcio. La frase de contraseña debe contener como mínimo: 1 letra mayúscula, 1 letra minúscula y 1 número.<br /><br />Tenga en cuenta que si dos miembros utilizan la misma frase de contraseña las cuentas generadas serán iguales. Es útil usar la misma frase de contraseña si una sola organización está intentando implementar en diferentes regiones y quiere compartir una sola cuenta (coinbase) en todos los nodos.|12 o más caracteres|N/D
Bloque génesis (Bloque génesis personalizado = Sí)|Cadena JSON que representa el bloque génesis personalizado. Puede encontrar más detalles sobre el formato del bloque génesis aquí, en Redes personalizadas.<br /><br />Todavía se crea una cuenta de Ethereum al proporcionar un bloque génesis personalizado. Considere la posibilidad de especificar una cuenta de Ethereum con fondos en el bloque génesis para no tener que esperar por la minería de datos.|JSON válido |N/D
Clave compartida para la conexión|Una clave compartida para la conexión entre las puertas de enlace de red virtual.| 12 o más caracteres|N/D
Dirección URL de datos del consorcio|La dirección URL que apunta a los datos de configuración relevantes del consorcio proporcionados por la implementación de otro miembro. <br /><br />Esta información la proporciona un miembro ya conectado que tiene una implementación. Si ha implementado el resto de la red, la dirección URL es la salida de implementación de la plantilla, denominada CONSORTIUM-DATA.||N/D
Puerta de enlace de red virtual a la que conectarse|La ruta del recurso de la puerta de enlace de red virtual a la que se va a conectar.<br />Esta información la proporciona un miembro ya conectado que tiene una implementación. Si ha implementado el resto de la red, la dirección URL está en la salida de implementación de la plantilla, denominada CONSORTIUM_MEMBER_GATEWAY_ID. Nota: Debe usarse la dirección URL de datos del consorcio y el recurso de la puerta de enlace de red virtual del mismo miembro.||N/D
Punto de conexión del registrador de información del mismo nivel|Punto de conexión de información del mismo nivel proporcionado por la implementación de otro miembro|Punto de conexión válido del primer miembro en el consorcio|N/D
Clave del registrador de información del mismo nivel|Clave principal de información del mismo nivel proporcionada por la implementación de otro miembro|Clave principal válida del primer miembro en el consorcio|N/D

### <a name="summary"></a>Resumen

Haga clic en el resumen para revisar las entradas especificadas y ejecutar una validación básica anterior a la implementación.

![Resumen](./media/ethereum-deployment/summary.png)

Revise los términos legales y de privacidad y haga clic en **Comprar** para implementar. Si la implementación tiene más de una región o si es para una red del consorcio, esta plantilla implementa previamente las puertas de enlace de VPN necesarias para admitir la conectividad de red con otros miembros. La implementación de la puerta de enlace puede tardar entre 45 y 50 minutos.

## <a name="post-deployment-sanity-checks"></a>Comprobaciones de integridad posteriores a la implementación

### <a name="administrator-page"></a>Página del administrador

Una vez que la implementación se haya completado correctamente y se hayan aprovisionado todos los recursos, puede ir a la página del administrador para obtener una vista de la red de cadena de bloques y comprobar la integridad del estado de la implementación. La dirección URL de la página del administrador es el nombre DNS del equilibrador de carga; está presente en la sección de salida de la implementación de plantilla denominada ADMIN_SITE.

Para encontrarla, seleccione el grupo de recursos que se implementó. Después, seleccione **Información general** y haga clic en el vínculo justo debajo de **Implementaciones** que muestra el número correcto.

![Información general del grupo de recursos](./media/ethereum-deployment/resource-overview.png)

La nueva pantalla muestra el historial de implementaciones. Seleccione el primer recurso de implementación (por ejemplo, microsoft-azure-blockchain.azure-blockchain-servi...) y busque la sección **Salidas** en la mitad inferior de la página. Podrá ver la dirección URL de la página del administrador listada en el parámetro de salida de la implementación de plantilla como ADMIN_SITE.

![Implementaciones de recursos](./media/ethereum-deployment/resource-deployments.png)

![Salida de la implementación](./media/ethereum-deployment/deployment-output.png)

Para ir a la página del administrador, copie la salida **ADMIN-SITE** y ábrala en otra ficha.

En la página del administrador puede obtener una visión general de la topología que implementó, si revisa la sección Estado del nodo de Ethereum. Incluye todos los nombres de host de nodos, su recuento de nodos del mismo nivel y el último bloque visto. El número de nodos del mismo nivel está entre el valor mínimo de uno menos que el *número total de nodos* y el número máximo configurado de nodos del mismo nivel. Tenga en cuenta que el número de nodos del mismo nivel no limita el número de nodos que se pueden implementar en la red.
En ocasiones, verá que el número de nodos del mismo nivel fluctúa y es inferior al (número total de nodos - 1). La diferencia en el número no es siempre un indicio de que los nodos tienen un estado incorrecto, puesto que las bifurcaciones en el libro de contabilidad pueden provocar cambios menores en el número de nodos del mismo nivel. Por último, puede inspeccionar el último bloque visto por cada nodo de la red para determinar bifurcaciones o retrasos en el sistema.

![Estado del nodo](./media/ethereum-deployment/node-status.png)

El estado del nodo se actualiza cada 10 segundos. Vuelva a cargar la página mediante el explorador o el botón **Recargar** para actualizar la vista.

### <a name="oms-portal"></a>Portal de OMS

Puede localizar el portal de OMS bien a través del vínculo en la salida de implementación (OMSPORTALURL) o bien seleccionando el recurso de OMS en el grupo de recursos implementado.

![DIRECCIÓN URL DE OMS](./media/ethereum-deployment/oms-url.png)

![Recurso de OMS](./media/ethereum-deployment/oms-resource.png)

El portal mostrará primero las estadísticas de red de alto nivel como una información general.

![Información general de OMS](./media/ethereum-deployment/oms-overview.png)

Al hacer clic en la información general se le dirigirá a un portal para ver las estadísticas por nodo.

![Estadísticas por nodo](./media/ethereum-deployment/per-node-stats.png)

### <a name="accessing-nodes"></a>Acceso a los nodos

Puede conectarse de forma remota a las máquinas virtuales para los nodos de transacción a través de SSH con el nombre de usuario y contraseña de administrador provistos o clave SSH. Puesto que las máquinas virtuales de nodos de transacción no tienen sus propias direcciones IP públicas, debe volver a pasar por el equilibrador de cargas y especificar el número de puerto. El comando SSH que se debe ejecutar para tener acceso al primer nodo de transacción se muestra en el parámetro de salida de la implementación de plantilla como **SSH_TO_FIRST_TX_NODE** (para la implementación de ejemplo: ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Para obtener nodos de transacción adicionales, aumente el número de puerto en uno (por ejemplo, el primer nodo de transacción está en el puerto 4000).

Puesto que las máquinas virtuales en las que se ejecutan los nodos de minería de datos no son accesibles de forma externa, debe pasar por uno de los nodos de transacción. Una vez que tenga una sesión SSH en un nodo de transacción, instale la clave privada en el nodo de transacción o use la contraseña para iniciar una sesión SSH en cualquiera de los nodos de minería de datos.

**Nota:**

Los nombres de host pueden obtenerse en el sitio de administrador o en Azure Portal. En Azure Portal, los nombres de host de los nodos presentes en el recurso de conjunto de escalado de máquinas virtuales (VMSS) aparecen en **Instancias**, lo que difiere de los nombres de host reales. Por ejemplo, el nombre de host en Azure Portal puede ser similar a **mn-asdfmv-reg1_0**, pero el nombre de host real tendría el siguiente aspecto **mn-asdfmv-reg**.

Por ejemplo: 

Nombre de host de Azure Portal| Nombre de host real
---|---
mn-ethwvu-reg1_0| mn-ethwvu-reg1000000
mn-ethwvu-reg1_1 |mn-ethwvu-reg1000001
mn-ethwvu-reg1_2 |mn-ethwvu-reg1000002

## <a name="adding-a-new-consortium-member"></a>Agregar un nuevo miembro de consorcio

### <a name="sharing-data"></a>Uso compartido de datos

Como primer miembro (o un miembro conectado) del consorcio, deberá proporcionar a los demás miembros cierta información para que puedan unirse y establecer conexión. Concretamente:

1. **Datos compartidos de configuración de consorcio**: Hay un conjunto de datos que se usan para coordinar la conexión de Ethereum entre dos miembros. La información necesaria, incluido el bloque génesis, el identificador de red del consorcio y los nodos de arranque, se escribe en un archivo en los nodos de transacción del líder u otro miembro implementado. La ubicación de este archivo se muestra en el parámetro de salida de la implementación de plantilla denominado **CONSORTIUM-DATA**.
2. **Punto de conexión de información del mismo nivel**: El punto de conexión del registrador de información del mismo nivel para obtener información de todos los nodos ya conectados a la red de Ethereum desde la implementación del líder o de otro miembro. La base de datos almacena un conjunto de información sobre cada nodo conectado en la red, información como el nombre de host del nodo, dirección IP privada, etc. Este es el parámetro de salida de la implementación de plantilla denominado **PEER_INFO_ENDPOINT**.
3. **Clave principal de información del mismo nivel**: La clave principal del registrador de información del mismo nivel se usa para acceder a la clave principal de información del mismo nivel del líder o de otro miembro. Este es el parámetro de salida de la implementación de plantilla denominado **PEER_INFO_PRIMARY_KEY**.


4. **Puerta de enlace de red virtual**: Cada miembro establece una conexión a toda la red de cadena de bloques a través de un miembro existente. Para conectar la red virtual, es necesaria la ruta del recurso para la puerta de enlace de red virtual del miembro al que se está conectando. Este es el parámetro de salida de la implementación de plantilla denominado **CONSORTIUM_MEMBER_GATEWAY_ID**.
5. **Clave compartida:** Un secreto establecido previamente entre dos miembros de la red del consorcio que establecen una conexión. Se trata de una cadena alfanumérica (entre 1 y 128 caracteres) que se haya acordado fuera del contexto de la implementación. (Por ejemplo, **MySharedKeyAbc123**).

### <a name="acceptance-of-new-member"></a>Aceptación de nuevo miembro

Este paso debe realizarse después de que el miembro que se está uniendo haya implementado correctamente su red. Antes de que un miembro pueda unirse a la red y ver el tráfico de transacción, un miembro existente debe realizar una configuración final en su puerta de enlace de VPN para aceptar la conexión. Esto significa que los nodos de Ethereum del miembro que se está uniendo no se ejecutarán hasta que se establezca una conexión. Esta configuración puede realizarse a través de PowerShell o la CLI de xPlat. También se almacenan un script de la CLI de xPlat y un módulo de PowerShell en el nodo de transacción, junto con los datos del consorcio. La ubicación del script son los parámetros de salida de la implementación denominados **PAIR-GATEWAY-PS-MODULE** y **PAIR-GATEWAY-AZURE-CLISCRIPT**, respectivamente.

**Instalación de PowerShell o CLI**

Puede encontrar información adicional sobre cómo empezar a trabajar con cmdlets de Azure PowerShell y la CLI de Azure xPlat en la documentación de Azure.

Necesitará tener la versión más reciente de los cmdlets de Azure instalada localmente y una sesión abierta. Asegúrese de iniciar sesión con sus credenciales de la suscripción de Azure.

**PowerShell: Establecer conexión**

Descargue el módulo de PowerShell y almacénelo localmente. La ubicación del módulo de PowerShell se especifica como el parámetro de salida de la implementación de plantilla **PAIR-GATEWAY-PS-MODULE**.

Si aún no está habilitado, use el cmdlet **Set-ExecutionPolicy** para la sesión local para permitir la ejecución de un módulo no firmado.

**Set-ExecutionPolicy Unrestricted CurrentUser**

Después, inicie sesión en la suscripción de Azure en la que ha implementado la implementación líder mediante:

**Login-AzureRmAccount**

Luego importe el módulo:

**Import-Module <filepath to downloaded file>**

Por último, ejecute la función con la entrada adecuada:

- **MyGatewayResourceId:** Ruta de acceso de recursos de la puerta de enlace. Este es el parámetro de salida de la implementación de plantilla denominado **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId:** Ruta del recurso de puerta de enlace del miembro que se está uniendo. Esto lo proporciona el miembro que se está uniendo y es el parámetro de salida de la implementación de plantilla, también denominado **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName:** Nombre para que identifique esta conexión de puerta de enlace.
- **Clave compartida**: Secreto previamente establecido entre los dos miembros de la red del consorcio que establecen una conexión.

**CreateConnection** -MyGatewayResourceId <resource path of your Gateway> - OtherGatewayResourceId <ruta del recurso de puerta de enlace del miembro que se está uniendo> -ConnectionName myConnection - SharedKey "MySharedKeyAbc123"

**CLI de xPlat: Establecer conexión**

Descargue el script de la CLI de Azure y almacénelo localmente. La ubicación del script de la CLI de Azure se especifica en el parámetro de la implementación de plantilla denominado **PAIR-GATEWAY-AZURE-CLI-SCRIPT**.

Ejecute el script con la entrada adecuada:

- **MyGatewayResourceId:** Ruta de acceso de recursos de la puerta de enlace. Este es el parámetro de salida de la implementación de plantilla denominado **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId:** Ruta del recurso de puerta de enlace del miembro que se está uniendo. Esto lo proporciona el miembro que se está uniendo y el parámetro de la implementación de plantilla de su implementación, también denominado **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName:** Nombre para que identifique esta conexión de puerta de enlace.
- **Clave compartida**: Secreto previamente establecido entre los dos miembros de la red del consorcio que establecen una conexión.
- **Ubicación:** Región de Azure donde se implementa el recurso de puerta de enlace.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

La arquitectura será la siguiente, después de haber configurado correctamente la conexión entre las implementaciones del **líder** y el **miembro**.

![Arquitectura del líder y el miembro](./media/ethereum-deployment/leader-member.png)

## <a name="fund-new-member-account"></a>Fondos de la cuenta de nuevo miembro

### <a name="generated-genesis-block"></a>Bloque génesis generado

Como el primer miembro, su cuenta de Ethereum se financia con un billón de Ether si la implementación genera el bloque génesis (Bloque génesis personalizado = No). Los demás miembros tendrán una cuenta que no tiene fondos previos y deben esperar a acumular Ether a medida que los nodos de minería de datos comienzan a extraer bloques. Para evitar que los nuevos miembros tengan que esperan por Ether, debe agregar fondos explícitamente a las cuentas de Ethereum de los miembros que se están uniendo.

Para ello, los miembros que se están uniendo deben proporcionarle la cuenta de Ethereum que se muestra en su sitio web de administración. A continuación, puede usar su sitio web de administración para transferir Ether de su cuenta a la de los miembros especificando la cuenta proporcionada.

### <a name="custom-genesis-block"></a>Bloque génesis personalizado

Si se proporciona un bloque génesis personalizado con una cuenta de Ethereum determinada, puede usar MetaMask u otra herramienta para transferir Ether de la cuenta especificada a la cuenta de Ethereum pregenerada visible en el sitio web de administración. Para obtener instrucciones sobre cómo usar MetaMask, vea [Crear una cuenta de Ethereum](#create-ethereum-account).

Si se proporciona un bloque génesis personalizado sin una cuenta o no tiene acceso a ninguna cuenta preasignada, deberá esperar a que los nodos de minería de datos comiencen a extraer para generar Ether en su cuenta (coinbase). La rapidez con la que se generan los fondos depende del nivel de dificultad que especifique en el bloque génesis personalizado.

## <a name="create-ethereum-account"></a>Crear una cuenta de Ethereum

Para crear una cuenta adicional, puede utilizar una variedad de soluciones. Una de estas soluciones es MetaMask, una extensión de Chrome que proporciona un almacén de identidades y conexión a una red de Ethereum pública, de prueba o personalizada. MetaMask formula una transacción para registrar la cuenta en la red.

Esta transacción, al igual que cualquier otra, irá a uno de los nodos de transacción y finalmente se extraerá en un bloque, como se muestra a continuación.

![Nodo de transacción](./media/ethereum-deployment/transaction-node.png)

Para instalar la extensión en Chrome, vaya a Personaliza y controla Google Chrome (botón de desbordamiento) > Más herramientas > Extensiones > Obtener más extensiones y busque MetaMask.

![Extensión MetaMask](./media/ethereum-deployment/metamask-extension.png)

Una vez instalado, abra MetaMask y cree un nuevo almacén. De forma predeterminada, el almacén se conectará a la red de prueba Morden. Cambie esta opción para conectarse a la red del consorcio privada implementada, en concreto al equilibrador de carga delante de los nodos de transacción. En la salida de la plantilla, recupere el punto de conexión del RPC de Ethereum del puerto 8545, denominado `ETHEREUM-RPC-ENDPOINT` y escriba el RPC personalizado como se muestra a continuación.

![Configuración de MetaMask](./media/ethereum-deployment/metamask-settings.png)

Al crear el almacén, crea una cartera que contiene una cuenta. Para crear cuentas adicionales, seleccione **Switch Accounts** (Cambiar cuentas) y luego el botón **+**.

![Crear cuenta de MetaMask](./media/ethereum-deployment/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>Iniciar la asignación de Ether inicial

A través de la página del administrador puede formular una transacción para transferir Ether desde la cuenta preasignada a otra cuenta de Ethereum. Esta transferencia de Ether es una transacción que se envía al nodo de transacción y se extrae en un bloque, como se muestra a continuación.

![Nodo de transacción](./media/ethereum-deployment/transaction-node-mined.png)

Mediante el icono del Portapapeles en la cartera de MetaMask, copie la dirección de la cuenta de Ethereum a la que quiere transferir Ether y vuelva a la página del administrador. Pegue la cuenta copiada en el campo de entrada para transferir 1000 Ether de la cuenta de Ethereum preasignada a la cuenta recién creada. Haga clic en **Submit** (Enviar) y espere a que la transacción se extraiga en un bloque.

![Estado del nodo](./media/ethereum-deployment/node-status2.png)

Una vez que la transacción se confirma en un bloque extraído, el saldo de su cuenta en MetaMask reflejará la transferencia de 1000 Ether.

![Transferencia de MetaMask](./media/ethereum-deployment/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Transferencia de Ether entre cuentas

En este punto, está listo para ejecutar las transacciones dentro de la red privada del consorcio. La transacción más sencilla consiste en transferir Ether de una cuenta a otra. Para realizara una transacción de este tipo, puede volver a usar MetaMask, y transferir dinero de la primera cuenta usada anteriormente a una segunda cuenta.

En **Wallet 1** (Cartera 1) en MetaMask, haga clic en enviar. Copie la dirección de la segunda cartera creada en el campo de entrada **Recipient Address** (Dirección del destinatario) y la cantidad de Ether que va a transferir en el campo de entrada **Amount** (Cantidad). Haga clic en **Send** (Enviar) y acepte la transacción.

![Transacción de envío de MetaMask](./media/ethereum-deployment/metamask-send.png)

Una vez más, cuando la transacción se ha extraído y confirmado en un bloque, los saldos de las cuenta se reflejan como corresponde. Fíjese que en el saldo de **Wallet 1** (Cartera ) se restan más de 15 Ether, puesto que tuvo que pagar una cuota de minería de datos para procesar la transacción.

![Wallet 1](./media/ethereum-deployment/wallet1.png)

![Wallet 2](./media/ethereum-deployment/wallet2.png)

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para centrarse en el desarrollo de aplicaciones y contratos inteligentes con la red privada de cadena de bloques del consorcio.
