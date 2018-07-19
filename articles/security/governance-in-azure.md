---
title: Sistema de gobierno en Azure | Microsoft Docs
description: Información acerca de los servicios de informática en la nube que se pueden escalar o reducir verticalmente para satisfacer las necesidades de su aplicación o empresa.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 579e900ee6616af8fd197e501364acd8e18d3e37
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970530"
---
# <a name="governance-in-azure"></a>Sistema de gobierno en Azure

Azure ofrece muchas opciones de seguridad y la capacidad de controlarlas de manera que se cumplan los requisitos exclusivos de las implementaciones de su organización.

Con las funcionalidades de gobierno en la nube de Azure nos referimos a los procesos de toma de decisiones, los criterios y las directivas implicadas en el planeamiento, la arquitectura, la adquisición, la implementación, el uso y la administración de un entorno de informática en la nube. El sistema de gobierno en la nube de Azure proporciona un enfoque de auditoría y consultoría integradas que tiene como objetivo asesorar a las organizaciones y revisar el uso que hacen de la plataforma de Azure. 

Para crear un plan de gobierno de Azure en la nube, deben considerarse en detalle las personas, los procesos y las tecnologías actuales. A continuación, puede crear marcos que faciliten al departamento de TI la satisfacción de las necesidades empresariales de manera coherente, al tiempo que proporcionan a los usuarios flexibilidad para usar las características de Azure.

## <a name="implementation-of-policies-processes-and-standards"></a>Implementación de directivas, procesos y estándares 

El departamento de administración de Azure ha establecido roles y responsabilidades para supervisar la implementación de directivas de seguridad de la información y la continuidad de las operaciones mediante Azure. El departamento de administración de Azure se ocupa de supervisar la seguridad y las prácticas continuidad en sus respectivos equipos (incluidos los de terceros). También facilita el cumplimiento de estándares, procesos y directivas de seguridad.

### <a name="account-provisioning"></a>Aprovisionamiento de cuentas

Definir la jerarquía de las cuentas constituye un paso importante para usar y estructurar los servicios de Azure en una compañía. Es la estructura de gobierno básica. Los clientes con contrato Enterprise pueden subdividir el entorno en departamentos, cuentas y suscripciones.

![Aprovisionamiento de cuentas](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Si no tiene un contrato Enterprise, considere la posibilidad de usar las [etiquetas de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) en el nivel de suscripción para definir la jerarquía. Una suscripción de Azure es la unidad básica donde se encuentran todos los recursos. También define varios límites en Azure, como el número de núcleos y los recursos. Las suscripciones pueden contener [grupos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), que pueden incluir recursos. Los principios del [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) se aplican en esos tres niveles.

Cada empresa es diferente. Para las compañías no empresariales, jerarquía de uso de etiquetas de Azure permite flexibilidad de organización en Azure. Antes de implementar recursos en Azure, debe modelar la jerarquía y comprender el acceso a los recursos, la complejidad y el impacto que tiene en la facturación.

### <a name="subscription-controls"></a>Controles de suscripción

Las suscripciones determinan cómo se notifica y factura el uso de los recursos. Las suscripciones se pueden configurar para que la facturación y los pagos sean independientes. Una cuenta de Azure puede contener varias suscripciones. Las suscripciones se pueden utilizar para determinar el uso de los recursos de Azure de varios departamentos de una compañía.

Por ejemplo, una compañía tiene departamentos de TI, de recursos humanos y de marketing con diferentes proyectos en marcha. La compañía puede basar su facturación en el uso que haga cada departamento de los recursos de Azure (por ejemplo, las máquinas virtuales). De este modo se controlan las finanzas de cada departamento.

Las suscripciones de Azure establecen tres parámetros:

- Identificador de suscriptor único

- Ubicación de facturación

- Conjunto de recursos disponibles

Para un usuario individual, esos parámetros incluyen un identificador de cuenta Microsoft, un número de tarjeta de crédito y el conjunto completo de servicios de Azure. Microsoft aplica límites de consumo, según el tipo de suscripción.

Las jerarquías de inscripción Azure definen cómo se estructuran los servicios en un contrato Enterprise. El portal del contrato Enterprise permite a los clientes dividir el acceso a los recursos de Azure relacionados con un contrato Enterprise según jerarquías flexibles personalizables en función de las necesidades de la organización. El patrón de la jerarquía debe coincidir con la estructura geográfica y de administración de una organización para que se pueda tener en cuenta el acceso a los recursos y las facturas asociadas.

Los tres patrones de alto nivel de jerarquía son uno funcional, otro de unidad empresarial y el último, geográfico. Los departamentos son una construcción administrativa para la agrupación de cuentas. Dentro de cada departamento, las cuentas pueden tener suscripciones asignadas, con lo que se generan silos de facturación y varios límites importantes en Azure (por ejemplo, número de máquinas virtuales y cuentas de almacenamiento).

![Controles de suscripción](./media/governance-in-azure/security-governance-in-azure-fig2.png)


En lo que respecta a las organizaciones con un contrato Enterprise, las suscripciones de Azure siguen una jerarquía de cuatro niveles:

1. Administrador de inscripciones de la empresa

2. Administrador de departamentos

3. Propietario de cuenta

4. Administrador de servicios

Esta jerarquía controla lo siguiente:

- Relación de facturación.

- Administración de cuentas.

- Acceso a los recursos mediante RBAC.

- Límites:

  - Uso y facturación (lista de tarifas basada en números de oferta)

  - límites

  - Red virtual

- Conexión con Azure Active Directory (Azure AD). Una instancia de Azure AD puede asociarse a varias suscripciones.

- Relación con una cuenta de inscripción empresarial.

### <a name="role-based-access-control"></a>Control de acceso basado en rol

[RBAC](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) permite la administración de acceso detallada a los recursos de Azure. Con RBAC, puede conceder únicamente el grado de acceso que los usuarios necesiten para realizar su trabajo. Las compañías deben centrarse en conceder a los empleados exactamente los permisos que necesiten. Un número elevado de permisos provocará que la cuenta esté expuesta a los atacantes. Si se conceden muy pocos, los empleados no podrán realizar su trabajo de manera eficaz. 

En lugar de proporcionar a todos los empleados permisos no restringidos en los recursos o la suscripción de Azure, puede permitir solo determinadas acciones. Por ejemplo, puede usar RBAC para dejar que un empleado administre máquinas virtuales en una suscripción y que otro pueda administrar bases de datos SQL en la misma suscripción.

Para conceder acceso, se asignan roles a usuarios, los grupos o las aplicaciones en un ámbito determinado. El ámbito de una asignación de roles puede ser una suscripción, un grupo de recursos o un único recurso. Un rol asignado en un ámbito principal también concede acceso a los elementos secundarios dentro del mismo. Por ejemplo, un usuario con acceso a un grupo de recursos puede administrar todos los recursos que contiene, como sitios web, máquinas virtuales y subredes. Dentro de cada suscripción, puede crear hasta 2000 asignaciones de roles.

Un rol es una colección de permisos y RBAC tiene varios roles integrados. Los siguientes roles integrados se aplican a todos los tipos de recursos:

- **Propietario**: tiene acceso total a todos los recursos, incluido el derecho a delegar este acceso a otros.

- **Colaborador**: puede crear y administrar todos los tipos de recursos de Azure, pero no puede conceder acceso a otros.

- **Lector**: puede ver todos los recursos de Azure.

Los demás roles integrados permiten la administración de recursos específicos de Azure. Por ejemplo, el rol de colaborador de máquina virtual permite al usuario crear y administrar máquinas virtuales. Para una lista de todos los roles integrados y sus operaciones, consulte los [roles RBAC integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

RBAC es compatible con operaciones de administración de los recursos de Azure de Azure Portal y de las API de Azure Resource Manager. En la mayoría de los casos, RBAC no puede autorizar operaciones de nivel de datos para los recursos de Azure. Para información sobre cómo se extiende RBAC a las operaciones de datos, consulte [Descripción de la definición de roles](https://docs.microsoft.com/azure/role-based-access-control/role-definitions).

Si los roles integrados no satisfacen sus necesidades de acceso específicas, puede [crear su propio rol personalizado](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Igual que los roles integrados, los roles personalizados pueden asignarse a usuarios, grupos y aplicaciones en ámbitos de suscripciones, grupos de recursos y recursos. Se pueden crear roles personalizados con [Azure PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell), la [CLI de Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) o la [API REST](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

### <a name="resource-management"></a>Administración de recursos

Azure ofrece dos modelos de implementación: el [clásico](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) y el de Azure Resource Manager.

En el modelo clásico, cada recurso existe de forma independiente. No hay ninguna manera de agrupar recursos relacionados. El seguimiento de los recursos que componían la solución o aplicación es manual y debe acordarse de administrarlos de manera coordinada. La unidad básica de administración es la suscripción. Es difícil desglosar los recursos dentro de una suscripción, por lo que es necesario crear un gran número de suscripciones.

El modelo de implementación de Resource Manager incluye el concepto de [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Un grupo de recursos es un contenedor para los recursos que comparten un ciclo de vida común. Puede incluir todos los recursos de la solución o solo aquellos que se desean administrar como grupo. Para decidir cómo asignar los recursos a los grupos de recursos, tenga en cuenta lo que más conviene a su organización.

El modelo de implementación de Resource Manager ofrece varias ventajas:

- Puede implementar, administrar y supervisar todos los servicios para su solución como grupo, en lugar de controlar estos servicios individualmente.

- Puede implementar la solución repetidamente a lo largo del ciclo de vida y tener la seguridad de que los recursos se implementan de forma coherente.

- Puede aplicar control de acceso a todos los recursos del grupo. Estas directivas se aplican automáticamente al agregar nuevos recursos al grupo.

- Puede aplicar etiquetas a los recursos para organizar de manera lógica todos los recursos de la suscripción.

- Puede usar la notación de objetos JavaScript (JSON) para definir la infraestructura de la solución. El archivo JSON se conoce como una plantilla de Resource Manager.

- Puede definir las dependencias entre recursos de modo que se implementen en el orden correcto.

![Resource Manager](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Para más recomendaciones sobre platillas, consulte [Procedimientos recomendados para crear plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analiza las dependencias para asegurarse de que los recursos se crean en el orden correcto. Si un recurso se basa en un valor de otro recurso (por ejemplo, una máquina virtual que necesita una cuenta de almacenamiento para los discos), [establezca una dependencia](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies) en la plantilla.

También puede utilizar la plantilla para las actualizaciones de la infraestructura. Por ejemplo, puede agregar un recurso a la solución y agregar reglas de configuración para los recursos que ya están implementados. Si la plantilla especifica la creación de un recurso pero este ya existe, Resource Manager realiza una actualización, en lugar de crear un nuevo recurso. Resource Manager actualiza el recurso existente al mismo estado que tendría si fuese nuevo.

Resource Manager proporciona extensiones para escenarios en los que se necesitan operaciones adicionales, como instalar un software que no está incluido en el programa de instalación.

### <a name="resource-tracking"></a>Seguimiento de recursos

A medida que los usuarios de su organización agregan recursos a la suscripción, cada vez adquiere más importancia asociar los recursos al departamento, el cliente y el entorno adecuados. Puede asociar metadatos a los recursos a través de las [etiquetas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags). Estas sirven para proporcionar información sobre el recurso o el propietario. Además, no solo permiten agregar y agrupar los recursos de varias maneras, sino también emplear esos datos para fines de contracargo.

Use etiquetas cuando tenga una colección compleja de grupos de recursos y recursos; debe visualizar estos recursos de la manera más conveniente en su caso. Por ejemplo, puede etiquetar recursos que cumplan una función similar en la organización o que pertenezcan al mismo departamento.

Sin etiquetas, los usuarios de su organización pueden crear varios recursos que después resulten difíciles de identificar y administrar. Por ejemplo, quizá quiera eliminar todos los recursos de un proyecto. Si dichos recursos no están etiquetados en el proyecto, tendrá que buscarlos manualmente. El etiquetado puede ser un aspecto importante para reducir costos innecesarios en la suscripción.

Los recursos no tienen que residir en el mismo grupo de recursos para compartir una etiqueta. Puede crear su propia taxonomía de etiquetas para asegurarse de que todos los usuarios de la organización utilizan etiquetas comunes y no aplican accidentalmente etiquetas ligeramente diferentes (por ejemplo, "dept" en lugar de "departamento").

Las directivas de recursos le permiten crear reglas estándar para su organización. Puede crear directivas que se aseguren de que los recursos están etiquetados con los valores adecuados.

También puede ver recursos etiquetados mediante Azure Portal. El [informe de uso](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) de la suscripción incluye los nombres y los valores de las etiquetas, para poder desglosar los costos por etiquetas.

Para más información sobre las etiquetas, consulte [Iniciativa de directiva de etiquetas de facturación](../azure-policy/scripts/billing-tags-policy-init.md).

Se aplican las siguientes limitaciones a las etiquetas:

- Cada recurso o grupo de recursos puede tener un máximo de 15 pares de clave/valor de etiquetas. Esta limitación solo se aplica a las etiquetas que se aplican directamente al recurso o grupo de recursos. Un grupo de recursos puede contener muchos recursos con 15 pares de clave/valor de etiquetas cada uno.

- El nombre de la etiqueta está limitado a 512 caracteres.

- El valor de la etiqueta está limitado a 256 caracteres.

- Los recursos de un grupo de recursos no heredan las etiquetas aplicadas a este.

Si necesita asociar más de 15 valores a un recurso, utilice una cadena JSON como valor de la etiqueta. La cadena JSON puede contener muchos valores que se aplican a una sola clave de etiqueta.

#### <a name="tags-for-billing"></a>Etiquetas de facturación

Las etiquetas le permiten agrupar los datos de facturación. Por ejemplo, si va a ejecutar varias máquinas virtuales para organizaciones diferentes, con etiquetas se puede agrupar el uso por centro de costo. También puede usar etiquetas para clasificar los costos por entorno de tiempo de ejecución; por ejemplo, el uso de facturación en máquinas virtuales que se ejecutan en el entorno de producción.

Puede recuperar información sobre las etiquetas a través de las [API de RateCard y de uso de recursos de Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) o mediante el archivo de valores separados por coma (CSV). Puede descargar el archivo de uso en el [Portal de cuentas de Azure](https://account.windowsazure.com/) o el [portal del contrato Enterprise](https://ea.azure.com/).

Para obtener más información sobre el acceso a información de facturación mediante programación, vea [Obtención de información sobre el consumo de recursos de Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Para las operaciones de API de REST, vea [Referencia de API de REST de facturación de Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Al descargar el archivo .csv de uso correspondiente a los servicios que admiten etiquetas con facturación, estas aparecerán en la columna Etiquetas.

### <a name="critical-resource-controls"></a>Controles de recursos críticos

A medida que su organización agrega servicios básicos a la suscripción, cada vez reviste más importancia asegurarse de que estos servicios estén disponibles para evitar la interrupción de la actividad empresarial. Gracias a los [bloqueos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources), se pueden restringir las operaciones en recursos de gran valor donde modificarlas o eliminarlas tendría un gran impacto en las aplicaciones o la infraestructura en la nube. Puede aplicar bloqueos a una suscripción, un recurso o un grupo de recursos. Normalmente, los bloqueos se aplican a recursos fundamentales como redes virtuales, puertas de enlace y cuentas de almacenamiento.

Además, en estos momentos, admiten dos valores: **CanNotDelete** y **ReadOnly**. **CanNotDelete** significa que los usuarios (con los derechos adecuados) pueden leer o modificar un recurso, pero no eliminarlo. **ReadOnly** significa que los usuarios autorizados no pueden eliminar ni modificar un recurso.

Los bloqueos de recursos solo se aplican a las operaciones que se producen en el plano de la administración, que consta de las operaciones enviadas a <https://management.azure.com>. Los bloqueos no restringen cómo los recursos realizan sus propias funciones. Los cambios de recursos están restringidos, pero no así las operaciones de recursos. Por ejemplo, un bloqueo **ReadOnly** de SQL Database evita tener que eliminar o modificar la base de datos, pero no evita la creación, actualización o eliminación de datos.

Al aplicar **ReadOnly**, pueden producirse resultados inesperados, ya que algunas operaciones que parecen ser similares a operaciones de lectura requieren acciones adicionales. Por ejemplo, al colocar un bloqueo **ReadOnly** en una cuenta de almacenamiento, evita que se muestren las claves a todos los usuarios. La operación de enumeración de claves se administra mediante solicitudes POST, ya que las claves devueltas están disponibles para las operaciones de escritura.

![Controles de recursos críticos](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Otro ejemplo: al colocar un bloqueo **ReadOnly** en un recurso de servicio de Azure App Service, evita que el Explorador de servidores de Visual Studio muestre los archivos del recurso, ya que esa interacción requiere acceso de escritura.

Al diferencia del control de acceso basado en rol, los bloqueos de administración se usan para aplicar una restricción a todos los usuarios y roles. Para información acerca de cómo establecer permisos, consulte [Administración del acceso mediante RBAC y Azure Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

Cuando se aplica un bloqueo en un ámbito primario, todos los recursos heredan el mismo bloqueo. Incluso los recursos que agregue posteriormente heredarán el bloqueo del elemento primario. El bloqueo más restrictivo de toda la herencia tiene prioridad.

Para crear o eliminar bloqueos de administración, debe tener acceso a las acciones Microsoft.Authorization/ o Microsoft.Authorization/locks/. Entre los roles integrados, solamente se conceden esas acciones al propietario y al administrador de acceso de usuarios.

### <a name="api-access-to-billing-information"></a>Acceso de las API a la información de facturación

Use las API de facturación de Azure para extraer datos de uso y de recursos e incorporarlos en las herramientas de análisis de datos de su preferencia. Las API de RateCard y de uso de recursos de Azure pueden ayudarlo a predecir y administrar los costos de forma precisa. Las API se implementan como proveedor de recursos, como parte de la familia de API expuesta por Azure Resource Manager.

#### <a name="resource-usage-api-preview"></a>API de uso de recursos (versión preliminar)

Use la [API de uso de recursos](https://msdn.microsoft.com/library/azure/mt219003) de Azure para obtener los datos estimados de consumo de Azure. La API incluye:

- **RBAC**: configure las directivas de acceso en [Azure Portal](https://portal.azure.com/) o mediante [cmdlets de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar qué usuarios o aplicaciones pueden acceder a los datos de uso de la suscripción. Los autores de llamadas deben utilizar tokens de Azure Active Directory estándar para la autenticación. Agregue el autor de la llamada al rol Lector de facturación, Lector, Propietario o Colaborador para que obtenga acceso a los datos de uso de una suscripción de Azure específica.

- **Agregaciones cada hora o diarias**: los autores de llamadas pueden especificar si desean sus datos de uso de Azure en incrementos de una hora o diarios. El valor predeterminado es diario.

- **Metadatos de instancia (incluye etiquetas de recursos)**: obtenga detalles de nivel de instancia, como el URI de recurso completo (/subscriptions/{id-suscripción}/..), la información del grupo de recursos y las etiquetas de recursos. Estos metadatos le ayudarán de forma determinista y mediante programación a asignar el uso mediante etiquetas, para casos de uso como cargos cruzados.

- **Metadatos de recursos**: detalles de recursos, como el nombre del medidor, la categoría del medidor, la subcategoría del medidor, la unidad y la región permiten que el autor de la llamada comprenda mejor lo que se ha consumido. También estamos trabajando para alinear la terminología de metadatos de recursos en todo Azure Portal, CSV de uso de Azure, CSV de facturación de EA y otras experiencias orientadas al público, para ayudarle a poner en correlación los datos en todas las experiencias.

- **Uso para todos los tipos de ofertas**: los datos de uso están disponibles para todos los tipos de ofertas, incluidos el pago por uso, MSDN, compromiso monetario, crédito monetario y EA.

#### <a name="resource-ratecard-api"></a>Resource RateCard API

Use Azure Resource RateCard API para obtener la lista de recursos de Azure disponibles y los precios estimados de cada uno. La API incluye:

- **RBAC**: configure las directivas de acceso en Azure Portal o mediante cmdlets de Azure PowerShell para especificar qué usuarios o aplicaciones pueden acceder a los datos de RateCard. Los autores de llamadas deben utilizar tokens de Azure Active Directory estándar para la autenticación. Agregue el autor de la llamada al rol Lector, Propietario o Colaborador para que pueda acceder a los datos de uso de una suscripción de Azure determinada.

- **Compatibilidad con ofertas de pago por uso, MSDN, compromiso monetario y crédito monetario (no con EA)**: esta API proporciona información de tarifas de nivel de oferta de Azure. El autor de llamada de esta API debe pasar la información de oferta para obtener detalles y tarifas de recursos Los contratos Enterprise aún no son compatibles porque las ofertas de EA tienen tarifas personalizadas por inscripción. 

#### <a name="scenarios"></a>Escenarios

Las API de uso y de RateCard hacen posibles estos escenarios:

- **Comprensión del gasto en Azure durante el mes**: use la combinación de las API de uso y de RateCard para obtener información más detallada sobre el gasto por usar la nube durante el mes. Puede analizar las estimaciones de uso y gastos diarios y por hora.

- **Configuración de alertas**: use las API de uso y de RateCard para obtener una estimación del consumo en la nube y los cargos, y configure alertas basadas en los recursos o en importes monetarios.

- **Predicción de facturas**: obtenga el gasto en la nube y el consumo estimados, y aplique algoritmos de aprendizaje automático para predecir cuál sería la factura al final del ciclo de facturación.

- **Análisis de costos previos al consumo**: use la API de RateCard para predecir cuál sería el importe de su factura para el uso esperado cuando mueva las cargas de trabajo a Azure. Si tiene cargas de trabajo existentes en otras nubes o nubes privadas, también puede asignar su uso con las tarifas de Azure para obtener una mejor estimación del gasto de Azure. Esta estimación ofrece la capacidad de dinamizar una oferta, y de comparar y contrastar los distintos tipos de ofertas además del pago por uso, como son el compromiso monetario y el crédito monetario.

- **Análisis de hipótesis**: puede determinar si es más rentable ejecutar las cargas de trabajo en otra región o en otra configuración del recurso de Azure. Los costos de los recursos de Azure pueden variar en función de la región de Azure que utilice. También puede determinar si otro tipo de oferta de Azure ofrece una mejor tarifa en un recurso de Azure.

### <a name="networking-controls"></a>Controles de red

El acceso a los recursos puede ser interno (dentro de la red corporativa) o externo (a través de Internet). Es muy fácil que los usuarios de una organización coloquen accidentalmente los recursos en la zona incorrecta y, posiblemente, que queden expuestos al acceso malintencionado. Al igual que con los dispositivos locales, las empresas deben agregar los controles adecuados para asegurarse de que los usuarios de Azure toman las decisiones correctas.

![Controles de red](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Para el gobierno de suscripciones, los siguientes recursos principales proporcionan un control de acceso básico.

#### <a name="network-connectivity"></a>Conectividad de red

Las [redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) son objetos de contenedor de las subredes. Aunque no es estrictamente necesario, se suele utilizar una red virtual al conectar las aplicaciones a los recursos corporativos internos. El servicio Azure Virtual Network le permite conectar de forma segura los recursos de Azure entre sí por medio de redes virtuales.

Una red virtual es una representación de su propia red en la nube. Una red virtual es un aislamiento lógico de la nube de Azure dedicada a su suscripción. También puede conectar redes virtuales a la red local propia.

A continuación, se muestran las funcionalidades de las redes virtuales de Azure:

- **Aislamiento**: las redes virtuales están aisladas entre ellas. Puede crear redes virtuales independientes para el desarrollo, la prueba y la producción que usen los mismos bloques de direcciones de CIDR. Por el contrario, puede crear varias redes virtuales que usen diferentes bloques de direcciones CIDR y que conecten las redes entre sí. También puede segmentar una red virtual en varias subredes. Azure proporciona la resolución de nombres interna para las instancias de rol de Virtual Machines y de Azure Cloud Services conectadas a una red virtual. Si lo desea, puede configurar una red virtual para usar sus propios servidores DNS, en lugar de usar la resolución de nombres interna de Azure.

- **Conectividad a Internet**: todas las instancias de roles de Cloud Services y Azure Virtual Machines conectadas a una red virtual tienen acceso a Internet, de forma predeterminada. También puede habilitar el acceso entrante a recursos específicos, según sea necesario.

- **Conectividad de los recursos de Azure**: los recursos de Azure como Cloud Services y Virtual Machines se pueden conectar a la misma red virtual. Los recursos pueden conectarse entre sí mediante direcciones IP privadas, aunque estén en subredes diferentes. Azure proporciona el enrutamiento predeterminado entre subredes, redes virtuales y redes locales, por lo que no tendrá que configurar ni administrar rutas.

- **Conectividad de red virtual**: puede conectar redes virtuales entre sí. Los recursos conectados a cualquier red virtual pueden comunicarse con cualquier otro recurso de cualquier otra red virtual.

- **Conectividad local**: las redes virtuales se pueden conectar a redes locales mediante conexiones de redes privadas entre la red y Azure, o mediante una conexión de red privada virtual (VPN) de sitio a sitio mediante Internet.

- **Filtrado de tráfico**: el tráfico (de entrada y salida) de Cloud Services y Virtual Machines se puede filtrar por dirección IP y puerto de origen, dirección IP y puerto de destino, y por protocolo.

- **Enrutamiento**: de manera opcional, puede invalidar el enrutamiento predeterminado de Azure mediante la configuración de sus propias rutas, o bien utilizando rutas BGP mediante una puerta de enlace de red.

#### <a name="network-access-controls"></a>Controles de acceso a la red

Los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) son similares a los firewalls y proporcionan reglas de cómo un recurso puede comunicarse a través de la red. Proporcionan control sobre cómo o bajo qué condición una subred (o máquina virtual) puede conectarse a Internet o a otras subredes de la misma red virtual.

Un grupo de seguridad de red contiene una lista de reglas de seguridad que permiten o deniegan el tráfico a recursos conectados a redes virtuales de Azure. Los grupos de seguridad de red se pueden asociar a subredes, máquinas virtuales individuales (clásicas) o interfaces de red (NIC) individuales conectadas a máquinas virtuales (Resource Manager).

Cuando un grupo de seguridad de red está asociado a una subred, las reglas se aplican a todos los recursos conectados a la subred. El tráfico se puede restringir aún más si se asocia también un grupo de seguridad de red a una máquina virtual o interfaz de red.

## <a name="security-and-compliance-with-organizational-standards"></a>Seguridad y cumplimiento de las normas de las organizaciones

Cada empresa tiene necesidades distintas y todas obtendrán diferentes ventajas de las soluciones en la nube. No obstante, clientes de todo tipo tienen las mismas preocupaciones básicas en lo que respecta a cómo realizar la migración a la nube. Esto es lo que buscan los clientes de un proveedor de nube:

- **Protección de los datos**: los líderes de TI confirman que la nube puede ofrecer mayor seguridad y control administrativo de los datos. Pero la mayor vulnerabilidad de la migración a la nube ante los hackers que las soluciones internas actuales sigue siendo una cuestión preocupante.

- **Privacidad de los datos**: los servicios en la nube conllevan desafíos específicos en materia de privacidad. Del mismo modo que las compañías recurren a la nube para ahorrar en los costos de infraestructura y mejorar su flexibilidad, también les preocupa perder el control de dónde se almacenan sus datos, quién tiene acceso a ellos y cómo se utilizan.

- **Control**: aunque las compañías puedan sacar partido de la nube para implementar soluciones más innovadoras, les preocupa perder el control de sus datos. Debido a las noticias recientes de agencias gubernamentales que acceden a datos de clientes mediante medios tanto legales como ilegales, algunos CIO no se atreven a almacenar sus datos en la nube.

- **Fomento de la transparencia**: los empresarios que toman las decisiones comprenden la importancia de la seguridad, la privacidad y el control. Pero también desean poder verificar cómo se almacenan sus datos, cómo se accede a ellos y cómo se protegen.

- **Garantía de cumplimiento**: a medida que las compañías adoptan más las tecnologías de nube, la complejidad y el ámbito de las normas y los reglamentos seguirán evolucionando. Las compañías necesitan saber que se cumplirán las normas de cumplimiento.

## <a name="security-configuration-for-monitoring-logging-and-auditing"></a>Configuración de seguridad para la supervisión, el registro y la auditoría

Los suscriptores de Azure pueden administrar sus entornos en la nube desde distintos dispositivos. Estos dispositivos pueden incluir las estaciones de trabajo de administración, los equipos de los desarrolladores e incluso dispositivos para el usuario final con privilegios con permisos específicos de la tarea. 

En algunos casos, las funciones administrativas se realizan mediante consolas web como Azure Portal. En otros casos, puede haber conexiones directas a Azure desde sistemas locales a través de redes privadas virtuales, Terminal Services, protocolos de aplicación cliente o Azure Service Management API (SMAPI) (mediante programación). Además, los puntos de conexión de cliente pueden estar unidos a un dominio o aislados y no administrados, como tabletas o smartphones.

Esta variabilidad puede suponer mayor riesgo para una implementación en la nube y dificultar la administración, el seguimiento y la auditoría de las acciones administrativas. Esta variabilidad también puede presentar amenazas para la seguridad debidas al acceso no regulado a los puntos de conexión de cliente que se usan para administrar los servicios en la nube. El uso de estaciones de trabajo personales o generales para desarrollar y administrar infraestructuras abre la puerta a amenazas impredecibles que se producen tanto en la exploración web (por ejemplo, ataques a los sitios web más utilizados) como en el correo electrónico (tales como ingeniería social y suplantación de identidad [phishing]).

La supervisión, el registro y la auditoría son un principio de seguimiento y descripción de las actividades administrativas. A causa de la cantidad de datos generados, puede que no sea posible auditar todas las acciones con todo lujo de detalles. Sin embargo, auditar la efectividad de las directivas de administración es un procedimiento recomendado.

El sistema de gobierno de seguridad de Azure utiliza GPO de Azure Active Directory Domain Services (AD DS) que le ayudarán a controlar todas las interfaces de Windows de los administradores, como el uso compartido de archivos. En los procesos de auditoría, supervisión y registro puede incluir estaciones de trabajo de administración. Realice un seguimiento del acceso y uso de los administradores y programadores.

### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) proporciona una visión centralizada del estado de la seguridad de los recursos en las suscripciones. Se proporcionan recomendaciones que ayudan a evitar los recursos de riesgo. Asimismo, puede habilitar directivas más detalladas (por ejemplo, aplicar directivas a grupos de recursos concretos que permitan a la empresa adaptar su postura con respecto al riesgo que están abordando).

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center proporciona funcionalidades de administración de directivas y supervisión de la seguridad integradas en las suscripciones de Azure, ayuda a detectar las amenazas que podrían pasar desapercibidas y funciona con un amplio ecosistema de soluciones de seguridad. Después de habilitar las [directivas de seguridad](https://docs.microsoft.com/azure/security-center/security-center-policies) para los recursos de una suscripción, Security Center analizará la seguridad de los recursos con el fin de identificar posibles vulnerabilidades. La información acerca de la configuración de la red está disponible de inmediato.

Azure Security Center representa una combinación de procedimientos recomendados, análisis y administración de directivas de seguridad para todos los recursos de una suscripción de Azure. Permite a los equipos de seguridad y a los responsables de riesgos evitar y detectar amenazas de seguridad (además de responder a ellas), ya que recopila y analiza automáticamente los datos de seguridad de los recursos de Azure, la red y las soluciones de asociados, como programas antimalware y firewalls.

Además, Azure Security Center aplica análisis avanzados, aprendizaje automático y análisis de comportamientos incluidos. Se emplean la inteligencia de amenazas globales de los productos y servicios de Microsoft, Microsoft Digital Crimes Unit (DCU), Microsoft Security Response Center (MSRC) y fuentes externas. Puede aplicar [regulación de la seguridad](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) ampliamente en el nivel de suscripción. O bien, puede restringirla a requisitos específicos y aplicarla a recursos específicos mediante la definición de directivas.

Finalmente, Azure Security Center analiza el estado de seguridad de los recursos en función de esas directivas, y aprovecha esta información para proporcionar paneles detallados y alertas de eventos como la detección de malware o la conexión de IP malintencionadas. Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Azure Security Center supervisa los siguientes recursos de Azure:

- Máquinas virtuales (qué incluyen servicios en la nube)

- Redes virtuales

- Bases de datos SQL

- Soluciones de asociados integradas en su suscripción de Azure, como un firewall de aplicaciones web en las máquinas virtuales y en [App Service Environment](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme)

La primera vez que se accede a Azure Security Center la recopilación de datos se habilita en todas las máquinas virtuales de la suscripción. Se recomienda que tenga habilitada la recopilación de datos, pero puede [deshabilitarla](https://docs.microsoft.com/azure/security-center/security-center-faq) en la directiva de Security Center.

### <a name="log-analytics"></a>Log Analytics

La seguridad de la información del equipo de desarrollo de software y servicios de Azure Log Analytics y el [programa de gobierno](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) respalda sus requisitos empresariales. Cumple la legislación y los reglamentos tal como se describe en [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) y [Microsoft Trust Center Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). En estas páginas también se describe cómo Log Analytics establece los requisitos de seguridad, identifica los controles de seguridad, y administra y supervisa los riesgos. Cada año, el equipo revisa las directivas, las normas, los procedimientos y las directrices.

Cada miembro del equipo de desarrollo de Log Analytics recibe aprendizaje formal sobre seguridad de aplicaciones. Un sistema de control de versiones ayuda a proteger cada proyecto de software en desarrollo.

Microsoft cuenta con un equipo de seguridad y cumplimiento normativo que supervisa y evalúa todos los servicios de Microsoft. El equipo está formado por los responsables de seguridad de la información, quienes no están relacionados con los departamentos de ingeniería que desarrollan Log Analytics. Los responsables de la seguridad tienen su propia cadena de administración. Llevan a cabo evaluaciones independientes de los productos y servicios para ayudar a garantizar la seguridad y el cumplimiento.

Un conjunto de servicios que se ejecutan en Azure proporciona la funcionalidad principal de Log Analytics. Cada servicio ofrece una función de administración específica, y se pueden combinar los servicios para lograr escenarios de administración diferentes.

![Servicios de administración de Azure](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Estos servicios de administración se diseñaron en la nube. Se hospedan totalmente en Azure, por lo que no necesitan implementar recursos ni administrarlos en el entorno local. La configuración es mínima, y puede estar listo y rindiendo en cuestión de minutos.

Coloque a un agente en cualquier equipo Windows o Linux del centro de datos y enviará datos a Log Analytics. Allí se pueden analizar junto con los demás datos recopilados de la nube o de servicios locales. Use Azure Backup y Azure Site Recovery para aprovechar la nube para las copias de seguridad de los recursos locales y para lograr alta disponibilidad.

![Servicios de administración en el panel de Azure](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Los runbooks en la nube normalmente no pueden acceder a los recursos locales, pero puede instalar un agente en uno o varios equipos que hospeden los runbooks en su centro de datos. Cuando se inicia un runbook, especifique si desea que se ejecute en la nube o en un trabajo local.

Hay disponibles diferentes soluciones de Microsoft y de asociados que se pueden agregar a la suscripción de Azure para aumentar el valor de su inversión en Log Analytics. Por ejemplo, Azure ofrece [soluciones de administración](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions): conjuntos de lógica preempaquetados que implementan un escenario de administración concreto aprovechando uno o varios servicios de administración.

![Galería de soluciones de administración en Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Como asociado puede crear sus propias soluciones para que sean compatibles con los servicios y aplicaciones, y proporcionarlas a los usuarios mediante las plantillas de inicio rápido o Azure Marketplace.

## <a name="performance-alerting-and-monitoring"></a>Supervisión y alertas de rendimiento

### <a name="alerting"></a>Alertas

Sirven para supervisar las métricas, los eventos o los registros de los recursos de Azure. Notifican cuándo se cumple una condición especificada.

Las alertas están disponibles en los diferentes servicios, incluidos:

- **Azure Application Insights**: admite las alertas de métricas y pruebas web. Para más información, vea [Definición de alertas en Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) y [Supervisión de la disponibilidad y la capacidad de respuesta de cualquier sitio web](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- **Log Analytics**: habilita el enrutamiento de los registros de actividad y diagnóstico para Log Analytics. Admite las alertas de métricas y registros, y de otro tipo. Para más información, vea [Alertas de Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- **Azure Monitor**: admite alertas basadas en valores de métricas y en eventos de registro de actividades. Puede usar la [API de REST de Azure Monitor](https://msdn.microsoft.com/library/dn931943.aspx) para administrar las alertas. Para más información, vea la información sobre cómo [utilizar Azure Portal, PowerShell o la interfaz de la línea de comandos para crear alertas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Supervisión

Los problemas de rendimiento de la aplicación en la nube pueden afectar a su negocio. Con varios componentes interconectados y versiones frecuentes, las degradaciones pueden ocurrir en cualquier momento. Y, si va a desarrollar una aplicación, los usuarios normalmente encuentran problemas que no se han detectado durante las pruebas. Debe tener conocimiento de estos problemas de inmediato y disponer de las herramientas de diagnóstico y solución de problemas.

Existe un abanico de herramientas para la supervisión de servicios y aplicaciones de Azure. Algunas de sus características se superponen. Se debe en parte al desenfoque entre el desarrollo y el funcionamiento de una aplicación.

Estas son las herramientas principales:

- **Azure Monitor** es una herramienta básica para la supervisión de los servicios que se ejecutan en Azure. Proporciona datos a nivel de infraestructura sobre el rendimiento de un servicio y el entorno circundante. Si va a administrar todas las aplicaciones en Azure y tiene que decidir si quiere ampliar o reducir los recursos verticalmente, Azure Monitor le ayudará a empezar.

- **Application Insights** puede utilizarse para el desarrollo y como una solución de supervisión de producción. Funciona mediante la instalación de un paquete en la aplicación y, por tanto, ofrece una perspectiva más interna de lo que está sucediendo. Los datos incluyen tiempos de respuesta de dependencias, seguimientos de excepciones, instantáneas de depuración y perfiles de ejecución. Proporciona herramientas para analizar toda esta telemetría para ayudarle a depurar una aplicación y que conozca qué hacen los usuarios con ella. Puede indicar si un pico en los tiempos de respuesta se debe a un elemento de una aplicación o a algún problema de recursos externo. Si utiliza Visual Studio y la aplicación presenta errores, puede recurrir directamente a las líneas de código del problema para poder corregirlo.

- **Log Analytics** está destinado a quienes necesitan ajustar el rendimiento y planificar el mantenimiento de aplicaciones que se ejecutan en producción. Recopila y agrega los datos de muchos orígenes, aunque con un retraso de 10 a 15 minutos. Proporciona una solución integral de administración de TI para Azure, entornos locales e infraestructuras de terceros basadas en la nube (por ejemplo, Amazon Web Services). Proporciona herramientas para analizar datos de distintos orígenes, permite realizar consultas complejas en todos los registros y alerta cuando se cumplen condiciones específicas. Incluso recopila datos personalizados en el repositorio central para que pueda consultarlos y visualizarlos.

- **System Center Operations Manager** está diseñado para administrar y supervisar grandes instalaciones en la nube. Es posible que ya esté familiarizado con esta solución como herramienta de administración para Windows Server local y nubes basadas en Hyper-V, pero también se integra con aplicaciones de Azure y las administra. Entre otras cosas, puede instalar Application Insights en las aplicaciones dinámicas existentes. Si una aplicación deja de funcionar, Operations Manager se lo notifica en cuestión de segundos.


## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos recomendados para crear plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)

- [Ejemplos de implementación de un sistema de gobierno de suscripciones de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-examples)

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/)
