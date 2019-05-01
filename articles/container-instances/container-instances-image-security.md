---
title: Consideraciones de seguridad de Azure Container Instances
description: Recomendaciones para proteger las imágenes y los secretos de Azure Container Instances y consideraciones generales de seguridad para cualquier plataforma de contenedor
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/29/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: dc516277d79e37500e73e1aee6b88c908acb9b1c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64944002"
---
# <a name="security-considerations-for-azure-container-instances"></a>Consideraciones de seguridad de Azure Container Instances

Este artículo presentan las consideraciones de seguridad para usar Azure Container Instances para ejecutar aplicaciones de contenedor. Contenido de los temas:

> [!div class="checklist"]
> * **Recomendaciones de seguridad** para administrar imágenes y los secretos de Azure Container Instances
> * **Consideraciones para el ecosistema de contenedor** durante el ciclo de vida del contenedor, para cualquier plataforma de contenedor

## <a name="security-recommendations-for-azure-container-instances"></a>Recomendaciones de seguridad para Azure Container Instances

### <a name="use-a-private-registry"></a>Usar un registro privado

Los contenedores se crean a partir de imágenes que están almacenadas en uno o varios repositorios. Estos repositorios pueden pertenecer a un registro público, como [Docker Hub](https://hub.docker.com), o en un registro privado. Un ejemplo de un registro privado es el [Registro de confianza de Docker](https://docs.docker.com/datacenter/dtr/2.0/), que puede instalarse de forma local o en una nube privada virtual. También puede usar los servicios de registro de contenedor privado en la nube, incluidos [Azure Container Registry](../container-registry/container-registry-intro.md). 

Una imagen de contenedor disponible públicamente no garantiza la seguridad. Imágenes de contenedor constan de varias capas de software, y cada capa de software podría tener vulnerabilidades. Para ayudar a reducir la amenaza de ataques, debe almacenar y recuperar imágenes desde un registro privado, como Azure Container Registry o Docker Trusted Registry. Además de proporcionar un registro privado administrado, Azure Container Registry admite [autenticación basado en la entidad de servicio](../container-registry/container-registry-authentication.md) a través de Azure Active Directory para los flujos de autenticación básica. Esta autenticación incluye acceso basado en rol para (extracción) de solo lectura, escritura (inserción) y permisos de propietario.

### <a name="monitor-and-scan-container-images"></a>Supervisar y analizar las imágenes de contenedor

Supervisión y análisis, como soluciones de seguridad [Twistlock](https://azuremarketplace.microsoft.com/marketplace/apps/twistlock.twistlock?tab=Overview) y [Aqua Security](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) están disponibles a través de Azure Marketplace. Puede usarlos para examinar imágenes de contenedor en un registro privado e identificar posibles vulnerabilidades. Es importante comprender la profundidad de análisis que proporcionan las distintas soluciones. 

### <a name="protect-credentials"></a>Proteger las credenciales

Los contenedores pueden abarcar varios clústeres y las regiones de Azure. Por lo tanto, debe proteger las credenciales necesarias para los inicios de sesión o acceso de API, como contraseñas o tokens. Asegúrese de que solo los usuarios con privilegios pueden tener acceso a esos contenedores en tránsito y en reposo. Inventario de todos los secretos de la credencial y, a continuación, pida a los desarrolladores usar nuevas herramientas de administración de secretos que están diseñadas para plataformas de contenedores.  Asegúrese de que la solución incluye bases de datos cifradas, cifrado TLS para datos de los secretos en tránsito y con privilegios mínimos [control de acceso basado en roles](../role-based-access-control/overview.md). [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md) es un servicio en la nube que protege las claves de cifrado y los secretos (por ejemplo, los certificados, las cadenas de conexión y contraseñas) para aplicaciones en contenedores. Dado que estos datos son confidenciales y crítico para la empresa, proteger el acceso a la clave de almacenes para que sólo las aplicaciones autorizadas y los usuarios pueden tener acceso a ellos.

## <a name="considerations-for-the-container-ecosystem"></a>Consideraciones para el ecosistema de contenedor

Las siguientes medidas de seguridad, también implementa y administra de forma eficaz, pueden ayudar a asegurar y proteger su ecosistema de contenedor. Estas medidas se aplican en todo el ciclo de vida del contenedor desde el desarrollo hasta la implementación de producción y a una gama de plataformas, hosts y los orquestadores de contenedores. 

### <a name="use-vulnerability-management-as-part-of-your-container-development-lifecycle"></a>Usar administración de vulnerabilidades como parte de su ciclo de vida de desarrollo de contenedor 

Mediante el uso de administración de vulnerabilidades eficaz a lo largo del ciclo de vida de desarrollo de contenedor, mejorar las probabilidades que identificar y resolver problemas de seguridad antes de que sean un problema más grave. 

### <a name="scan-for-vulnerabilities"></a>Detectar vulnerabilidades 

Se descubren nuevas vulnerabilidades todo el tiempo, por lo que buscando e identificar las vulnerabilidades son un proceso continuo. Incorporar a lo largo del ciclo de vida del contenedor de examen de vulnerabilidades:

* Como una comprobación final en la canalización de desarrollo, debe realizar una exploración de vulnerabilidades en los contenedores antes de insertar las imágenes en un registro público o privado. 
* Continúe buscar imágenes de contenedor en el registro para identificar cualquier error que se perdieron durante el desarrollo de algún modo y para tratar las vulnerabilidades descubiertas recientemente que puedan existir en el código usado en las imágenes de contenedor.  

### <a name="map-image-vulnerabilities-to-running-containers"></a>Vulnerabilidades de imagen de mapa para contenedores en ejecución 

Debe tener un medio de las vulnerabilidades de asignación identificadas en imágenes de contenedor a la ejecución de contenedores, por lo que se pueden mitigar o resolver problemas de seguridad.  

### <a name="ensure-that-only-approved-images-are-used-in-your-environment"></a>Asegúrese de que solo las imágenes aprobadas se usan en su entorno 

Hay suficiente cambio y la volatilidad en un ecosistema de contenedor sin permitir que los contenedores desconocidos. Permitir que las imágenes de contenedor aprobadas única. Tenga las herramientas y procesos para supervisar y evitar el uso de imágenes de contenedor no aprobados. 

Es una forma eficaz de reducir la superficie de ataque e impida que los desarrolladores cometan críticas de seguridad controlar el flujo de imágenes de contenedor en el entorno de desarrollo. Por ejemplo, podría autorizar una distribución de Linux solo como una imagen base, preferiblemente uno que sea eficiente (Alpine o CoreOS en lugar de Ubuntu), para minimizar la superficie de ataques potenciales. 

Imagen de la firma o creación de huellas digitales puede proporcionar una cadena de custodia que le permite comprobar la integridad de los contenedores. Por ejemplo, Azure Container Registry es compatible con Docker [contenido confianza](https://docs.docker.com/engine/security/trust/content_trust) del modelo, que permite a los editores de imagen firmar las imágenes que se insertan en un registro, y los consumidores de imágenes para extraer solo firmado imágenes.

### <a name="permit-only-approved-registries"></a>Permitir solo aprueba los registros 

Es una extensión de garantizar que su entorno usa solo las imágenes aprobadas permitir únicamente el uso de los registros de contenedor aprobadas. Requerir el uso de los registros de contenedor aprobadas reduce la exposición a riesgos al limitar la posibilidad de que la introducción de los problemas de seguridad o vulnerabilidades desconocidas. 

### <a name="ensure-the-integrity-of-images-throughout-the-lifecycle"></a>Garantizar la integridad de las imágenes a lo largo del ciclo de vida 

Parte de la administración de seguridad en todo el ciclo de vida del contenedor es garantizar la integridad de las imágenes de contenedor en el registro y en cuanto se modifica o se implementa en producción. 

* Las imágenes con vulnerabilidades, incluso menores, no deberían poder ejecutar en un entorno de producción. Idealmente, todas las imágenes implementadas en producción deben guardarse en un registro privado accesible para unos seleccionados. Mantener pequeños para asegurarse de que se puede administrar eficazmente el número de imágenes de producción.

* Dado que es difícil identificar el origen del software desde una imagen de contenedor disponible públicamente, crear imágenes desde el origen para garantizar el conocimiento del origen de la capa. Cuando surge una vulnerabilidad en una imagen de contenedor generada automáticamente, los clientes pueden encontrar una ruta más rápida a una resolución. Con una imagen pública, los clientes tendrían que encontrar la raíz de una imagen pública para corregirla u obtener otra imagen segura del publicador. 

* No se garantiza una imagen digitalizada exhaustivamente implementada en producción esté actualizada para la duración de la aplicación. Se puede informar de las vulnerabilidades de seguridad para las capas de la imagen que no se conocían anteriormente o que se introdujeron después de la implementación de producción. 

  Audite periódicamente las imágenes implementadas en producción para identificar las imágenes que no están actualizados o no se han actualizado durante cierto tiempo. Puede usar las metodologías de implementación blue-green y activar mecanismos de actualización para actualizar las imágenes de contenedor sin tiempo de inactividad. Puede examinar imágenes mediante las herramientas descritas en la sección anterior. 

* Use una canalización de integración continua (CI) con seguridad integrada de análisis para crear imágenes seguras y transmitirlos a su registro privado. El análisis de vulnerabilidad creado en la solución de CI garantiza que las imágenes que pasan todas las pruebas se incluyan en el registro privado desde el que se implementan las cargas de trabajo de producción. 

  Un error de canalización de CI garantiza que las imágenes vulnerables no se incluyan en el registro privado que se usa para las implementaciones de carga de trabajo de producción. También automatiza el examen si hay un número significativo de imágenes de la seguridad de imagen. De lo contrario, auditar las imágenes manualmente para buscar vulnerabilidades de seguridad puede ser bastante lento y estar propenso a errores. 

### <a name="enforce-least-privileges-in-runtime"></a>Exigir privilegios mínimos en tiempo de ejecución 

El concepto de privilegios mínimos es una práctica recomendada de seguridad básica que también se aplica a los contenedores. Cuando se aprovecha una vulnerabilidad, generalmente proporciona el acceso del atacante y privilegios iguales a los de la aplicación en peligro o proceso. Asegúrese de que los contenedores que funcionan con los privilegios más bajos y acceso necesario para realizar el trabajo reduce la exposición a riesgos. 

### <a name="reduce-the-container-attack-surface-by-removing-unneeded-privileges"></a>Reducir la superficie de ataque de contenedor mediante la eliminación de privilegios innecesarios 

También puede minimizar la superficie de ataque potencial mediante la eliminación de privilegios ni procesos innecesarios o no utilizados desde el tiempo de ejecución del contenedor. Ejecutan contenedores con privilegios como raíz. Si un usuario malintencionado o carga de trabajo se convierte en un contenedor con privilegios, el contenedor, a continuación, se ejecutará como raíz en el sistema.

### <a name="whitelist-files-and-executables-that-the-container-is-allowed-to-access-or-run"></a>Los archivos de la lista de permitidos y los archivos ejecutables que se puede tener acceso o ejecutar el contenedor 

Reducir el número de variables o valores desconocidos le ayuda a mantener un entorno estable y confiable. Limitar por lo que puede tener acceso a los contenedores o solo la cuenta de ejecución o archivos en la lista blanca y ejecutables es un método de eficacia probada de limitar la exposición a riesgos.  

Es mucho más fácil de administrar una lista de permitidos cuando se implementa desde el principio. Una lista blanca proporciona una medida de control y facilidad de uso a medida que aprende los archivos y los archivos ejecutables son necesarios para la aplicación funcione correctamente. 

Una lista blanca no sólo reduce la superficie de ataque, pero puede también proporcionan una línea base para encontrar anomalías y evitar que los casos de uso de los escenarios de trabajo de contenedor y "vecino ruidoso". 

### <a name="enforce-network-segmentation-on-running-containers"></a>Aplicar la segmentación de la red en contenedores en ejecución  

Para ayudar a proteger los contenedores en una subred de riesgos de seguridad en otra subred, mantener la segmentación de la red (o nano segmentación) o segregación entre contenedores en ejecución. Mantener la segmentación de red también puede ser necesaria para el uso de contenedores en sectores que deben cumplir los mandatos de cumplimiento.  

Por ejemplo, la herramienta de asociado [Aqua](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) proporciona un enfoque automatizado para la segmentación de nano. Aguamarina supervisa las actividades de red de contenedor en tiempo de ejecución. Identifica todas las conexiones de red entrantes y salientes a y desde otros contenedores, servicios, las direcciones IP y la red internet pública. Segmentación de nano se crea automáticamente en función de tráfico supervisado. 

### <a name="monitor-container-activity-and-user-access"></a>Supervisar el acceso de usuario y actividad de contenedor 

Al igual que con cualquier entorno de TI, debe supervisar constantemente acceso de usuario y la actividad en el ecosistema de contenedor para identificar rápidamente cualquier actividad sospechosa o malintencionada. Azure proporciona soluciones incluidas de supervisión de contenedores:

* [Azure Monitor para contenedores](../azure-monitor/insights/container-insights-overview.md) para supervisar el rendimiento de las cargas de trabajo que se implementan en entornos de Kubernetes hospedados en Azure Kubernetes Service (AKS). Azure Monitor para contenedores le brinda la posibilidad de visibilizar el rendimiento mediante la recopilación de métricas del procesador y de la memoria de los controladores, nodos y contenedores disponibles en Kubernetes mediante la API de métricas. 

* El [solución de supervisión de contenedores de Azure](../azure-monitor/insights/containers.md) le ayuda a ver y administrar otros Docker y Windows hosts de contenedores en una sola ubicación. Por ejemplo: 

  * Ver información detallada de auditoría que muestra los comandos usados con contenedores. 
  * Solucionar los problemas de ver y buscar registros centralizados sin tener que ver los hosts de Docker o Windows de forma remota.  
  * Encontrar los contenedores que pueden ser ruidoso y consumo excesivo de recursos en un host.
  * Ver información de uso y el rendimiento de red de contenedores, memoria, almacenamiento y centralizada CPU.  

  La solución es compatible con los orquestadores de contenedores como Docker Swarm, DC/OS, no administrado Kubernetes, Service Fabric y Red Hat OpenShift. 

### <a name="monitor-container-resource-activity"></a>Supervisar la actividad de recursos de contenedor 

Supervisar la actividad de recursos, como archivos, red y otros recursos que tienen acceso los contenedores. Supervisión de consumo y la actividad de recursos resulta útil para supervisar el rendimiento tanto como medida de seguridad. 

[Azure Monitor](../azure-monitor/overview.md) habilita la supervisión básica del servicio de Azure al permitir la recopilación de métricas, registros de actividad y registros de diagnóstico. Por ejemplo, el registro de actividad le indica cuándo se crean o modifican los recursos. 

Se encuentran disponibles métricas que ofrecen estadísticas de rendimiento de diferentes recursos e incluso del sistema operativo de una máquina virtual. Puede ver estos datos con uno de los exploradores en Azure Portal y crear alertas basadas en estas métricas. Azure Monitor proporciona que las métricas más rápidas de canalización (5 minutos hasta 1 minuto), por lo que debe usar para las notificaciones y alertas críticas en el tiempo. 

### <a name="log-all-container-administrative-user-access-for-auditing"></a>Registrar todos los accesos de usuario administrativo de contenedor para la auditoría 

Mantener una pista de auditoría precisa de acceso administrativo a su ecosistema de contenedor, el registro de contenedor y las imágenes de contenedor. Estos registros podrían ser necesarios para fines de auditoría y serán útiles como prueba forense después de los incidentes de seguridad. Puede usar el [solución de supervisión de contenedores de Azure](../azure-monitor/insights/containers.md) para lograr este objetivo. 

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de cómo administrar las vulnerabilidades de contenedor con las soluciones de [Twistlock](https://www.twistlock.com/solutions/microsoft-azure-container-security/) y [Aqua Security](https://www.aquasec.com/solutions/azure-container-security/).

* Obtenga más información sobre [seguridad del contenedor en Azure](https://azure.microsoft.com/resources/container-security-in-microsoft-azure/).