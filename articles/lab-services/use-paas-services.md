---
title: Usar servicios de plataforma como-servicio (PaaS) en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo usar los servicios de plataforma como-servicio (Pass) en Azure DevTest Labs.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2019
ms.author: spelluru
ms.openlocfilehash: 7cdd185cddbd2403b72ff0e06530913af0b031de
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233126"
---
# <a name="use-platform-as-a-service-paas-services-in-azure-devtest-labs"></a>Usar servicios de plataforma como-servicio (PaaS) en Azure DevTest Labs
PaaS se admite en DevTest Labs a través de la característica de entornos. Se admiten los entornos en DevTest Labs mediante plantillas preconfiguradas de Azure Resource Manager en un repositorio Git. Los entornos pueden contener recursos de IaaS y PaaS. Le permiten crear sistemas complejos que pueden incluir recursos de Azure como máquinas virtuales, bases de datos, las redes virtuales y aplicaciones Web, que se personalizan para que trabajen juntos. Estas plantillas permiten una implementación coherente y mejor administración de entornos con control de código fuente. 

Una correcta configuración de sistema permite los siguientes escenarios: 

- A los desarrolladores tener varios entornos e independiente
- Pruebas en diferentes configuraciones de forma asincrónica
- Integración en las canalizaciones de ensayo y producción sin realizar ningún cambio de plantilla
- Tiene equipos de desarrollo y entornos en el mismo laboratorio aumenta la facilidad de administración e informes de costo.  

El proveedor de recursos de DevTest Labs crea recursos en nombre del usuario de laboratorio, por lo que no se necesita ningún permiso adicional que se asignará a los usuarios de laboratorio para habilitar el uso de los recursos de PaaS. La siguiente imagen muestra un clúster de Service Fabric como un entorno en el laboratorio.

![Clúster de Service Fabric como un entorno](./media/create-environment-service-fabric-cluster/cluster-created.png)

Para obtener más información acerca de cómo configurar entornos, encontrarás [crear entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md). DevTest Labs tiene un repositorio público de plantillas de Azure Resource Manager que puede usar para crear entornos sin tener que conectarse a un origen externo de GitHub por sí mismo. Para obtener más información en entornos públicos, consulte [configurar y usar entornos públicos de Azure DevTest Labs](devtest-lab-configure-use-public-environments.md).

En organizaciones grandes, los equipos de desarrollo suelen proporcionan entornos, como entornos de prueba aislada y personalizada. Puede haber entornos que se utilizan por todos los equipos de una división o una unidad de negocio. El grupo de TI puede proporcionar sus entornos que pueden usarse por todos los equipos de la organización.  

## <a name="customizations"></a>Personalizaciones

#### <a name="sandbox"></a>Espacio aislado 
El propietario de laboratorio puede personalizar los entornos de laboratorio para cambiar el rol del usuario de **lector** a **colaborador** dentro del grupo de recursos. Esta funcionalidad está en el **configuración del laboratorio** página bajo el **configuración y directivas** del laboratorio. Este cambio en el rol permite al usuario agregar o quitar recursos dentro de ese entorno. Si desea restringir aún más el acceso, use las directivas de Azure. Esta funcionalidad le permite personalizar la configuración sin el acceso en el nivel de suscripción o los recursos.

#### <a name="custom-tokens"></a>Tokens personalizados
Hay cierta información de laboratorio personalizado que está fuera del grupo de recursos y es específica para los entornos que puede tener acceso la plantilla. Estos son algunos de ellos: 

- Identificación de la red de laboratorio
- Ubicación
- Cuenta de almacenamiento en el que se almacenan los archivos de plantillas de Resource Manager. 
 
#### <a name="lab-virtual-network"></a>Red virtual de laboratorio
El [conectar entornos a red virtual del laboratorio](connect-environment-lab-virtual-network.md) artículo describe cómo modificar la plantilla de Resource Manager para usar el `$(LabSubnetId)` token. Cuando se crea un entorno, el `$(LabSubnetId)` símbolo (token) se reemplaza por la primera marca de la subred donde la **utilizar en la máquina virtual para crear** opción está establecida en **true**. Permite que nuestro entorno de redes ha creado anteriormente. Si desea utilizar las mismas plantillas de Resource Manager en entornos de prueba como ensayo y producción, use `$(LabSubnetId)` como valor predeterminado en un parámetro de plantilla de Resource Manager. 

#### <a name="environment-storage-account"></a>Cuenta de almacenamiento del entorno
DevTest Labs admite el uso de [plantillas de Resource Manager anidadas](../azure-resource-manager/resource-group-linked-templates.md). El [cómo Azure DevTest Labs facilita el Administrador de recursos anidados las implementaciones de plantilla para entornos de prueba](https://azure.microsoft.com/updates/azure-devtest-labs-streamlined-nested-arm-template-deployment-support-for-arm-template-based-environments) artículo explica cómo usar `_artifactsLocation` y `_artifactsLocationSasToken` muestras para crear un URI a una plantilla de Resource Manager en la misma carpeta, como o en una carpeta anidada de la plantilla principal. Para obtener más información acerca de estos dos tokens, consulte el **artefactos de implementación** sección de [Azure Resource Manager: Guía de mejores prácticas](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md).

## <a name="user-experience"></a>Experiencia del usuario

## <a name="developer"></a>Desarrollador
Los desarrolladores usar el mismo flujo de trabajo para crear una máquina virtual para crear un entorno específico. Seleccionan el entorno frente a la imagen de máquina y escriba la información necesaria mediante la plantilla. Cada desarrollador que tenga un entorno permite la implementación de cambios y depuración mejorado bucle interno. El entorno se puede crear en cualquier momento mediante la plantilla más reciente.  Esta característica permite a los entornos se destruye y volver a crear para ayudar a reducir el tiempo de inactividad de tener que crear el sistema de forma manual o recuperarse de pruebas de errores.  

### <a name="testing"></a>Pruebas
Los entornos de DevTest Labs permiten para las comprobaciones independientes de las configuraciones y código específico de forma asincrónica. Lo más habitual es configurar el entorno con el código de la solicitud de incorporación de cambios individuales e iniciar las pruebas automatizadas. Una vez que las pruebas automatizadas se ejecutan hasta completarse, se puede ejecutar el ninguna prueba manual en el entorno específico. Normalmente, este proceso se realiza como parte de la canalización de CI/CD. 

## <a name="management-experience"></a>Experiencia de administración

### <a name="cost-tracking"></a>Seguimiento de costos
La característica de seguimiento del costo incluye recursos de Azure en los distintos entornos como parte del total tendencia de costos. El costo por los recursos no se interrumpe los distintos recursos dentro del entorno, pero muestra el entorno como un solo costo.

### <a name="security"></a>Seguridad
Una suscripción de Azure configurada correctamente con DevTest Labs puede [limitar el acceso a recursos de Azure solo a través de la práctica](devtest-lab-add-devtest-user.md). Con los entornos, un propietario de laboratorio puede permitir que los usuarios accedan a los recursos de PaaS con configuraciones aprobadas sin permitir el acceso a otros recursos de Azure. En el escenario donde los usuarios del laboratorio personalización entornos, el propietario de laboratorio puede permitir el acceso de colaborador. Acceso de colaborador permite al usuario de laboratorio agregar o quitar recursos de Azure solo dentro del grupo de recursos administrados. Permite para facilitar el seguimiento y administración y permitir al usuario acceso de colaborador a la suscripción.

### <a name="automation"></a>Automation
La automatización es un componente clave para una gran escala, un ecosistema eficaz. La automatización es necesaria controlar la administración o para varios entornos de seguimiento a través de suscripciones y laboratorios.

### <a name="cicd-pipeline"></a>Canalización de CI/CD
Servicios de PaaS en DevTest Labs pueden ayudar a mejorar la canalización de CI/CD por tener centrado implementaciones donde el acceso se controla el laboratorio.

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes artículos para obtener más información acerca de los entornos: 

- 
- [Creación de entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md)
- [Configurar y usar entornos públicos en Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)
- [Crear un entorno con clúster de Service Fabric independiente en Azure DevTest Labs](create-environment-service-fabric-cluster.md)
- [Conectar un entorno a red virtual de su laboratorio en Azure DevTest Labs](connect-environment-lab-virtual-network.md)
- [Integrar entornos en las canalizaciones de CI/CD de Azure DevOps](integrate-environments-devops-pipeline.md)
 





