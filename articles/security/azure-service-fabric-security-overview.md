---
title: Introducción a la seguridad de Azure Service Fabric | Microsoft Docs
description: En este artículo se proporciona información general de la seguridad de Azure Service Fabric.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: c5b5f80a43530fe6d0b90e65c3aef89a815157e4
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57241398"
---
# <a name="azure-service-fabric-security-overview"></a>Introducción a la seguridad de Azure Service Fabric
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) es una plataforma de sistemas distribuidos que facilita el empaquetamiento, la implementación y la administración de microservicios escalables y confiables. Service Fabric aborda los desafíos de desarrollar y administrar aplicaciones en la nube. Los desarrolladores y administradores pueden evitar problemas complejos de infraestructura y centrarse en la implementación de cargas de trabajo exigentes y críticas que son escalables y confiables.

En este artículo se proporciona información general de las consideraciones de seguridad para una implementación de Service Fabric.

## <a name="secure-your-cluster"></a>Protección del clúster
Azure Service Fabric es un orquestador de servicios a través de un clúster de máquinas. Los clústeres deben estar protegidos para evitar que usuarios no autorizados se conecten a ellos, sobre todo cuando ejecutan cargas de trabajo en entornos de producción. Aunque es posible crear un clúster no protegido, esto podría permitir que usuarios anónimos se conectaran al clúster (si expone los puntos de conexión de administración a la red pública de Internet).

En el caso de los clústeres que se ejecutan tanto de forma independiente como en Azure, dos escenarios que se deben tener en cuenta son la seguridad de nodo a nodo y la seguridad de cliente a nodo. Para implementar dichos escenarios se pueden usar varias tecnologías.

### <a name="node-to-node-security"></a>Seguridad de nodo a nodo
La seguridad de nodo a nodo se aplica a la comunicación entre máquinas virtuales o entre las máquinas de un clúster. Con este tipo de seguridad, solo los equipos que están autorizados a unirse al clúster pueden participar en el hospedaje de aplicaciones y servicios en el clúster.

Los clústeres que se ejecutan en Azure o los independientes que se ejecutan en Windows pueden utilizar una [seguridad basada en certificados](https://msdn.microsoft.com/library/ff649801.aspx) o la [seguridad de Windows](https://msdn.microsoft.com/library/ff649396.aspx) para las máquinas con Windows Server.

#### <a name="node-to-node-certificate-security"></a>Seguridad basada en certificados de nodo a nodo

Service Fabric usa certificados de servidor X.509 que se especifican cuando se crea un clúster. Para obtener una descripción rápida de qué son estos certificados y cómo se pueden adquirir o crear, consulte [Trabajo con certificados](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

La seguridad de los certificados se configura al crear el clúster desde Azure Portal, las plantillas de Azure Resource Manager o una plantilla JSON independiente. Puede especificar un certificado principal y un certificado secundario opcional que se utiliza para las sustituciones de certificados. Los certificados principales y secundarios que especifique deben ser diferentes de los certificados de cliente de solo lectura y de los de cliente de administración que determine para la [seguridad de cliente a nodo](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Seguridad de cliente a nodo
La seguridad de cliente a nodo se configura mediante identidades de cliente. Para establecer la confianza entre un cliente y el clúster, debe configurar el clúster para que sepa en qué identidades de cliente puede confiar.

Service Fabric admite dos tipos de control de acceso para los clientes que están conectados a un clúster de Service Fabric:

-   **Administrator**: acceso total a las funcionalidades de administración, que incluyen funcionalidades de escritura y de lectura.
-   **User**: acceso de solo lectura tanto a las funcionalidades de administración (por ejemplo, funcionalidades de consulta) como a la capacidad para resolver aplicaciones y servicios.

Mediante el uso de control de acceso, los administradores de clústeres pueden limitar el acceso a ciertos tipos de operaciones de clúster. Esto hace que el clúster sea más seguro.

#### <a name="client-to-node-certificate-security"></a>Seguridad basada en certificados de cliente a nodo

La seguridad de los certificados de cliente a nodo se configura al crear un clúster mediante Azure Portal, las plantillas de Resource Manager o una plantilla JSON independiente. Debe especificar un certificado de cliente de administración o un certificado de cliente de usuario. Asegúrese de que estos certificados no son los mismos que los certificados principales y secundarios que se especifican para la seguridad de nodo a nodo.

Los clientes que se conectan al clúster mediante el certificado de administración tienen acceso completo a las funcionalidades de administración. Los clientes que se conectan al clúster mediante el certificado de cliente de usuario de solo lectura tienen acceso de este tipo a las funcionalidades de administración. Es decir, estos certificados se usan para el control de acceso basado en rol (RBAC).

Para aprender a configurar la seguridad de certificado en un clúster, consulte [Creación de un clúster de Service Fabric con Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

#### <a name="client-to-node-azure-active-directory-security"></a>Seguridad de Azure Active Directory de cliente a nodo

Los clústeres que se ejecutan en Azure también pueden proteger el acceso a los puntos de conexión de administración con Azure Active Directory (Azure AD). Para información sobre cómo crear los artefactos de Azure Active Directory necesarios, cómo rellenarlos durante la creación de clústeres y cómo conectar a dichos clústeres, consulte [Creación de un clúster de Service Fabric con Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure AD permite a las organizaciones (conocidas como inquilinos) administrar el acceso de los usuarios a las aplicaciones. Hay aplicaciones con una interfaz de usuario de inicio de sesión basada en web y otras que tienen una experiencia de cliente nativa.

Un clúster de Service Fabric ofrece diversos puntos de entrada a su funcionalidad de administración, como Service Fabric Explorer y Visual Studio basados en web. Como consecuencia, creará dos aplicaciones de Azure AD para controlar el acceso al clúster: una aplicación web y una aplicación nativa.

Para los clústeres de Azure, se recomienda utilizar la seguridad de Azure AD para autenticar a los clientes y certificados para la seguridad de nodo a nodo.

Para los clústeres de Windows Server independientes con Windows Server 2012 R2 y Active Directory, es aconsejable utilizar la seguridad de Windows con cuentas de servicio administradas de grupo (gMSA). De lo contrario, use la seguridad de Windows con cuentas de Windows.

## <a name="understand-monitoring-and-diagnostics-in-service-fabric"></a>Información acerca de supervisión y el diagnóstico en Azure Service Fabric
[La supervisión y el diagnóstico](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) son fundamentales para el desarrollo, la prueba y la implementación de aplicaciones y servicios en cualquier entorno. Las soluciones de Service Fabric funcionan mejor cuando implementa la supervisión y el diagnóstico para asegurarse de que las aplicaciones y los servicios funcionen según lo previsto en un entorno de desarrollo local o en producción.

Desde la perspectiva de la seguridad, los objetivos principales de la supervisión y el diagnóstico son:

-   Detectar y diagnosticar problemas de hardware y de infraestructura que puede provocar un evento de seguridad.
-   Detectar problemas de software y de aplicaciones que pueden ser un indicador de riesgo (IoC).
-   Conocer el consumo de recursos para ayudar a evitar una denegación de servicio involuntaria.

El flujo de trabajo de la supervisión y el diagnóstico consta de tres pasos:

1.  **Generación de eventos**: incluye eventos (registros, seguimientos y eventos personalizados) tanto a nivel de infraestructura (clúster) como a nivel de aplicación o servicio. Para saber lo que se proporciona y cómo agregar más instrumentación, lea más información acerca de los [eventos de nivel de infraestructura](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) y los [eventos de nivel de aplicación](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app).

2.  **Agregación de eventos**: para poder mostrar los eventos generados antes hay que recopilarlos y agregarlos. Normalmente, se recomienda usar [Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (similar a la recopilación de registros basada en agentes) o [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (recopilación de registros en proceso).

3.  **Análisis**: para poder analizar y mostrar eventos, es necesario que puedan visualizarse y que permitan acceder a ellos en algún formato. Hay varias plataformas para el análisis y la visualización de datos de supervisión y diagnóstico. Se recomienda [registros de Azure Monitor](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) y [Azure Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) porque integran correctamente con Service Fabric.

También puede usar [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) para supervisar muchos de los recursos de Azure que incorporan un clúster de Service Fabric.

Un guardián es un servicio independiente que vigila el estado y la carga en los servicios e informa sobre el estado de cualquier componente en la jerarquía del modelo de mantenimiento. Puede ayudar a evitar errores que se pasan por alto con la vista de un solo servicio. 

Los guardianes son también un buen lugar para hospedar código que realiza acciones correctoras sin interacción del usuario. Un ejemplo es la limpieza de los archivos de registro del almacenamiento a determinados intervalos de tiempo. Puede encontrar una implementación de servicio guardián de ejemplo en [Ejemplo de guardián de Azure Service Fabric](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-communication-by-using-certificates"></a>Protección de la comunicación mediante el uso de certificados
Los certificados le ayudan a proteger la comunicación entre los diversos nodos de un clúster de Windows independiente. Mediante el uso de certificados X.509, también se puede autenticar a los clientes que se conectan a este clúster. Esto garantiza que solo los usuarios autorizados puedan tener acceso al clúster. Se recomienda que habilite un certificado en el clúster al crearlo.

Los certificados digitales X509 se usan habitualmente para autenticar a clientes y servidores. También se usan para cifrar mensajes y firmarlos digitalmente.

En la siguiente tabla se enumeran los certificados que va a necesitar en su instalación del clúster:

|Configuración de la información de certificado |DESCRIPCIÓN|
|-------------------------------|-----------|
|ClusterCertificate|    Este certificado es necesario para proteger la comunicación entre los nodos de un clúster. Puede utilizar dos certificados de clúster, uno principal y otro secundario, para la actualización.|
|ServerCertificate| Este certificado se presenta al cliente cuando intenta conectarse a este clúster. Puede utilizar dos certificados de servidor diferentes, uno principal y otro secundario, para la actualización.|
|ClientCertificateThumbprints|  Se trata de un conjunto de certificados para instalar en los clientes autenticados.|
|ClientCertificateCommonNames|  Es el nombre común del primer certificado de cliente para CertificateCommonName. CertificateIssuerThumbprint es la huella digital del emisor de este certificado.|
|ReverseProxyCertificate|   Se trata de un certificado opcional que se puede especificar para proteger un [proxy inverso](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Para más información acerca de la protección de certificados, consulte [Protección de un clúster independiente en Windows mediante certificados X.509](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="understand-role-based-access-control"></a>Información acerca del control de acceso basado en rol
Especifique los roles de cliente de usuario y de administrador cuando cree el clúster con identidades independientes (incluidos los certificados) para cada uno. Para más información acerca de la configuración del control de acceso predeterminada y de cómo cambiarla, consulte [Control de acceso basado en rol para clientes de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-clusters-by-using-windows-security"></a>Protección de clústeres independientes mediante la seguridad de Windows
Para evitar accesos no autorizados a un clúster de Service Fabric, debe proteger el clúster. La seguridad es especialmente importante cuando el clúster ejecuta cargas de trabajo de producción. Tanto la seguridad de nodo a nodo como la de cliente a nodo se configura mediante la seguridad de Windows en el archivo ClusterConfig.JSON.

Cuando Service Fabric tenga que ejecutarse en una gMSA, para configurar la seguridad de nodo a nodo es preciso establecer [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security). Para crear relaciones de confianza entre los nodos, es preciso hacer que ambos conozcan su existencia mutuamente.

Si desea usar un grupo de máquinas dentro de un dominio de Active Directory, la seguridad de nodo a nodo se configura estableciendo ClusterIdentity. Para más información, consulte el artículo [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347) (Creación de un grupo de máquinas en Active Directory).

La seguridad de cliente a nodo se configura mediante ClientIdentities. Debe configurar el clúster para que reconozca en qué identidades de cliente puede confiar. La confianza se puede establecer de dos maneras:

-   Especifique los usuarios del grupo de dominio que se pueden conectar.
-   Especifique los usuarios del nodo de dominio que se pueden conectar.

## <a name="configure-application-security-in-service-fabric"></a>Configuración de la seguridad de la aplicación en Service Fabric
### <a name="manage-secrets-in-service-fabric-applications"></a>Administración de los secretos en aplicaciones de Service Fabric
Los secretos pueden ser cualquier información confidencial, como cadenas de conexión de almacenamiento, contraseñas u otros valores que no se deben administrar en texto sin formato.

Puede usar [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) para administrar las claves y los secretos. Sin embargo, el uso de secretos en una aplicación no se basa en una plataforma en la nube concreta. Las aplicaciones se pueden implementar en un clúster que se hospede en cualquier lugar. Hay cuatro pasos principales en este flujo:

1.  Obtener un certificado de cifrado de datos.
2.  Instalar el certificado en el clúster.
3.  Cifrar los valores de secreto al implementar una aplicación con el certificado e insértelos en un archivo de configuración Settings.xml del servicio.
4.  Leer los valores cifrados de Settings.xml y usar el mismo certificado de cifrado para descifrarlos.

Para más información, consulte [Administración de secretos en aplicaciones de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-an-application"></a>Configuración de directivas de seguridad para una aplicación
La seguridad de Azure Service Fabric permite proteger las aplicaciones que se ejecutan en distintas cuentas de usuario en el clúster. La seguridad de Service Fabric también ayuda a proteger los recursos que usan las aplicaciones en el momento de la implementación en las cuenta de usuario (por ejemplo, archivos, directorios y certificados). Esto aumenta la seguridad entre aplicaciones en ejecución, incluso en un entorno hospedado compartido.

Las tareas para configurar las directivas de seguridad incluyen:

-   Configuración de la directiva de un punto de entrada del programa de instalación del servicio
-   Inicio de comandos de PowerShell desde un punto de entrada de configuración
-   Uso de la redirección de la consola para la depuración local
-   Configuración de una directiva para los paquetes de código del servicio
-   Asignación de una directiva de acceso de seguridad a puntos de conexión HTTP y HTTPS

## <a name="secure-communication-for-services"></a>Protección de la comunicación en los servicios
La seguridad es uno de los aspectos más importantes de la comunicación. El marco de trabajo de la aplicación de Reliable Services proporciona una serie de pilas de comunicación creadas previamente y herramientas que puede utilizar para mejorar la seguridad. Para más información, consulte [Protección de las comunicaciones remotas de un servicio](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).

## <a name="next-steps"></a>Pasos siguientes
- Para obtener información conceptual acerca de la seguridad de los clústeres, consulte [Creación de un clúster de Service Fabric con Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) y [Creación de un clúster de Service Fabric en Azure mediante Azure Portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Para más información acerca de la seguridad de los clústeres en Service Fabric, consulte [Escenarios de seguridad de los clústeres de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
