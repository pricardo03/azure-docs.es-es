---
title: Las directivas de compatibilidad con Azure Kubernetes Service (AKS)
description: Obtenga información sobre las directivas de soporte técnico de Azure Kubernetes Service (AKS), responsabilidad compartida, las características de vista previa, Alpha o Beta.
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 04/01/2019
ms.author: jenoller
ms.openlocfilehash: f173fc7c794729eae8c60cceefa88d153800a816
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032481"
---
# <a name="azure-kubernetes-service-aks-support-policies"></a>Las directivas de compatibilidad con Azure Kubernetes Service (AKS)

Este artículo proporcionan detalles sobre las directivas de soporte técnico de AKS, limitaciones, y los detalles, incluida la administración de nodo de trabajo, managed plano componentes, componentes de código abierto de terceros y seguridad de control / administración de revisiones.

## <a name="service-updates--releases"></a>Las versiones y actualizaciones de servicio

* Para obtener información de versión, consulte el [notas de la versión de AKS][1]
* Para las características en versión preliminar pública, consulte [características de versión preliminar de AKS y proyectos relacionados][2]

## <a name="what-is-managed"></a>¿Qué es 'managed'

A diferencia de IaaS en la nube los componentes básicos, como compute o redes, que exponen los controles de bajo nivel y opciones de personalización para que los usuarios aprovechar, el servicio AKS proporciona una implementación de Kubernetes de llave en mano que representa el conjunto común de configuraciones y funcionalidades necesarias para Kubernetes. Los clientes que usan este servicio tienen un número limitado de las personalizaciones, implementación y otras opciones. Esto también significa que los clientes no necesitan preocuparse ni administrar los clústeres de Kubernetes directamente.

Con AKS, el cliente obtiene totalmente administrado **plano de control** : donde el plano de control contiene todos los componentes y servicios necesarios para ofrecer y clústeres de Kubernetes para los usuarios finales. Todos los componentes de Kubernetes se mantienen y operados por Microsoft.

Con el administrado **plano de control** los siguientes componentes administrados y supervisados por Microsoft:

* Kubelet / servidores de la API de Kubernetes
* Etcd o un almacén clave/valor compatible, incluida la calidad de servicio, la escalabilidad y el tiempo de ejecución
* Los servicios DNS (por ejemplo, kube-dns / CoreDNS)
* Kubernetes Proxy y la red

AKS no es un 100% administrados **clúster** solución. Ciertos componentes (por ejemplo, nodos de trabajo) tienen ciertos **responsabilidades compartidas** casos donde las acciones para mantener el clúster de AKS requieren interacción del usuario. Por ejemplo, nodo de trabajo aplicación de revisiones de seguridad del sistema operativo.

 **Managed**, significa que Microsoft y el equipo AKS se implementa, funciona y es responsable de la disponibilidad y funcionalidad de estos servicios. **Los clientes no pueden modificar estos componentes**. Personalización está limitada a garantizar una experiencia de usuario coherente y escalable. Para una solución totalmente personalizable, consulte [AKS motor](https://github.com/Azure/aks-engine).

> [!NOTE]
> Es importante comprender que los nodos de trabajo de Azure Kubernetes Service aparecen en el Portal de Azure como recursos de IaaS de Azure normales, aunque estas máquinas virtuales se implementan en un grupo de recursos de Azure personalizada (prefijo MC\\*). Un usuario podría modificar, SSH en ellos como máquinas virtuales normales (sin embargo, no es posible, cambie la imagen de sistema operativo base, y los cambios no pueden persistir a través de una actualización o reiniciar), y puede asociar otros recursos de Azure a las mismas o modificarlos. **Sin embargo, hacerlo de fuera de la personalización y administración de banda significa que el propio clúster AKS puede llegar a ser incompatible. Evitar la modificación de nodos de trabajo a menos que indique Microsoft Support.**

## <a name="what-is-shared-responsibility"></a>¿Qué es la responsabilidad compartida

En tiempo de creación del clúster, AKS crea una serie de nodos de trabajo de Kubernetes definidos por el cliente. Estos nodos, como se indicó son donde se ejecutan cargas de trabajo cliente. Los clientes poseen y pueden ver o modificar estos nodos de trabajo.

Dado que estos nodos ejecutan código privado y almacenan información confidencial, soporte técnico de Microsoft tiene **acceso limitado** al cliente todos los nodos del clúster. Soporte técnico de Microsoft no se puede iniciar sesión en, ejecutar comandos o ver los registros de estos nodos sin permiso expreso del cliente o asistencia para ejecutar comandos de depuración, tal como se indica en el equipo de soporte técnico.

Dada la naturaleza confidencial de estos nodos de trabajo, Microsoft toma mucho cuidado para limitar la administración "en segundo plano" de estos nodos. Incluso si el maestro de Kubernetes, nodos, etcd y otros errores de componentes (administrada por Microsoft) seguirá la carga de trabajo ejecutar en muchos casos. Si se modifican los nodos de trabajo sin atención, puede provocar la pérdida de datos o carga de trabajo y representar el clúster de incompatibilidad.

## <a name="azure-kubernetes-service-support-coverage"></a>Cobertura de soporte técnico de servicio de Kubernetes de Azure

**Microsoft proporciona soporte técnico para lo siguiente:**

* Conectividad a todos los componentes de Kubernetes proporcionado y compatible con el servicio de Kubernetes (por ejemplo, el servidor de API)
* Administración, tiempo de actividad, QoS y las operaciones de Kubernetes controlan servicios plano (etcd de nodos, el servidor de la API, maestro de Kubernetes, kube-dns, por ejemplo.
* Soporte técnico Etcd incluye las copias de seguridad automatizadas y transparentes de todos los datos de etcd cada 30 minutos para la restauración del estado del clúster y planificación ante desastres. Estas copias de seguridad no están disponibles directamente a los clientes o usuarios y se usan para garantizar la coherencia y confiabilidad de los datos
* Los puntos de integración en el controlador de proveedor en la nube de Azure para Kubernetes como integraciones a otro Azure los servicios, como los equilibradores de carga, los volúmenes persistentes, Networking (Kubernetes y Azure CNI)
* Preguntas o problemas en torno a la personalización de los componentes del plano de control como el servidor de API de Kubernetes, etcd y kube-dns.
* Problemas sobre temas de red, como Azure CNI, Kubenet u otra red de problemas de acceso y la funcionalidad (DNS resolución, pérdida de paquetes, enrutamiento y así sucesivamente).
  * Escenarios de redes admitidos incluyen Kubenet (Basic) y Advanced Networking (Azure CNI) dentro de un clúster y los componentes asociados, conectividad a otras aplicaciones y servicios de Azure. Además, los controladores de entrada y configuración del equilibrador de carga/entrada, el rendimiento de red y latencia son compatibles con Microsoft.

**Microsoft no ofrece soporte técnico para lo siguiente:**

* Asesoramiento / uso "Procedimientos" Kubernetes preguntas, por ejemplo de cómo crear controladores de entrada personalizado, preguntas sobre la carga de trabajo de aplicación, y los paquetes de OSS de entidad de otro o herramientas que están fuera del ámbito.
  * Vales de asesoramiento para AKS clúster funcionalidad, personalización, operaciones de Kubernetes, por ejemplo, problemas o how-tos están dentro de ámbito – de optimización.
* Proyectos de código abierto de terceros que no se proporciona como parte de la Kubernetes plano de control o implementan con clústeres AKS, por ejemplo, Istio, Helm, Envoy y otros usuarios.
  * Para los proyectos de código abierto de terceros, como Helm y Kured, se proporciona compatibilidad de mejor esfuerzo para ver ejemplos y aplicaciones que se muestran en la documentación de Microsoft y donde esa herramienta de código abierto de terceros se integra con el proveedor de nube de Kubernetes Azure o otros errores específicos de AKS.
* Software de terceros origen cerrado: Esto puede incluir herramientas, software o dispositivos de red de análisis de seguridad.
* No se admiten los problemas de las salidas de compilación "en varias nubes" o de varios proveedores, no se admite la ejecución como una solución de proveedor federados múltiple en la nube pública.
* Personalizaciones de red específico, que no estén documentadas en el sitio oficial [documentación de AKS][3].
  > [!NOTE]
  > Problemas y errores en torno a grupos de seguridad de red es compatible. Por ejemplo, soporte técnico puede responder preguntas en torno a los NSG no se puede actualizar o comportamiento inesperado de NSG o equilibrador de carga.

## <a name="azure-kubernetes-service-support-coverage-worker-nodes"></a>Cobertura de soporte técnico de servicio de Kubernetes de Azure (nodos de trabajo)

### <a name="microsoft-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>Responsabilidades de Microsoft para los nodos de trabajo de Azure Kubernetes Service

Como se indicó en la `shared responsibility` sección, Kubernetes, nodos de trabajo se dividen en un modelo de responsabilidad de la junta, donde:

* Proporciona la imagen de sistema operativo base con las adiciones necesarias (por ejemplo, supervisión y los agentes de red)
* Entrega automática de revisiones del sistema operativo para los nodos de trabajo
* La corrección automática de problemas con Kubernetes controlar los componentes del plano que se ejecutan en los nodos de trabajo, como:
  * kube-proxy
  * componentes del maestro de túneles de red que proporcionan rutas de comunicación para Kubernetes
  * kubelet
  * demonio de docker/moby

> [!NOTE]
> Si un componente de plano de control no está operativo en un nodo de trabajo, el equipo AKS es posible que deba reiniciar el nodo de trabajo completo para resolver el problema. Esto se realiza en nombre de un usuario y no se realiza a menos que una extensión de cliente se realiza (debido al acceso a la carga de trabajo activa a los clientes y los datos). Siempre que sea posible al personal AKS funcionará para realizar cualquier requiere reiniciar el equipo que no son de aplicación que afectan a.

### <a name="customer-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>Responsabilidades del cliente para los nodos de trabajo de Azure Kubernetes Service

**Microsoft no es así:**

- Automáticamente las revisiones para que surta efecto de nivel de nodos de trabajo de reinicio de sistema operativo **(actualmente, consulte más abajo)**

Revisiones del sistema operativo se entregan a los nodos de trabajo, aunque no se la **responsabilidad del cliente** reiniciar los nodos de trabajo para que los cambios surtan efecto. Esta aplicación automática de revisiones incluye las bibliotecas compartidas, demonios como SSHD y otros componentes de nivel de sistema/OS.

Las actualizaciones de Kubernetes, **los clientes son responsables de la ejecución de la actualización** mediante el panel de Control de Azure o la CLI de Azure. Esto se aplica a las actualizaciones que contiene mejoras de funcionalidad en Kubernetes o de seguridad.

> [!NOTE]
> Como AKS es un `managed service` nuestros objetivos finales del servicio incluyen la eliminación responsable de las revisiones, actualizaciones, recopilación de registros y mucho más para que sea un servicio completamente administrado y sin intervención humana más. Estos elementos (reiniciar el nodo, aplicación automática de revisiones) pueden quitarse en versiones futuras a medida que aumentan de nuestras capacidades de extremo a extremo de administración.

### <a name="security-issues-and-patching"></a>Problemas de seguridad y aplicación de revisiones

En algunos casos (por ejemplo, los identificadores), un error de seguridad puede encontrarse en uno o varios de los componentes del servicio AKS. En estos escenarios, AKS patch todos los clústeres afectados para mitigar el problema si es posible o proporcionar instrucciones de actualización a los usuarios.

Para nodos de trabajo afectados por este tipo de vulnerabilidad de seguridad, si está disponible una revisión sin tiempo de inactividad del problema, el equipo de AKS aplicará esa corrección y notificar a los usuarios del cambio.

Si requiere una revisión de seguridad de nodo de trabajo se reinicia, Microsoft enviará una notificación de cliente de este requisito y el cliente es responsable de ejecutar el reinicio o actualización para obtener la revisión de su clúster. Si los usuarios no aplican las revisiones de acuerdo con las instrucciones de AKS, su clúster seguirá sea vulnerable a los problemas.

### <a name="node-maintenance-and-access"></a>Acceso y el mantenimiento de nodo

Dado que los nodos de trabajo son una responsabilidad compartida y en la propiedad de los clientes, los clientes pueden iniciar sesión en estos trabajos y realizar cambios potencialmente dañinos, como actualizaciones del kernel, quitar paquetes e instalar nuevos paquetes.

Si los clientes realizan acciones destructivas o acciones que desencadenan los componentes del plano de control para poner sin conexión o en caso contrario, se convierten en no funcionales, el servicio AKS detectará este error y realizar la corrección automática para restaurar el nodo de trabajo para el trabajo anterior estado.

Aunque los clientes pueden iniciar sesión en y modificar nodos de trabajo, es *desaconseja* ya que esto puede que el clúster de incompatibilidad.

## <a name="network-ports-access-and-network-security-groups"></a>Puertos de red, acceso y los grupos de seguridad de red

Como un servicio administrado, AKS tiene requisitos específicos de red y conectividad. Estos requisitos son menos flexibles que los componentes normales de IaaS. A diferencia de otros componentes de IaaS, determinadas operaciones (por ejemplo, la personalización de reglas del grupo de seguridad de red, bloqueo de puertos específico, creación de listas blancas de dirección URL etc.) pueden representar el incompatibilidad de clúster (por ejemplo, la reglas de firewall bloqueando el puerto de salida 443).

> [!NOTE]
> Bloqueo completo (por ejemplo, dominio/puerto explícito whitelisting) de salida desde el clúster no es un escenario admitido de AKS en este momento. La lista de puertos y direcciones URL está sujeta a cambios sin previo aviso y puede proporcionarse a través de una incidencia de soporte técnico de Azure. La lista proporcionada es solo para los clientes que están dispuestos a aceptar que *podría verse afectada la disponibilidad del clúster en cualquier momento.*

## <a name="alphabeta-kubernetes-features-not-supported"></a>Características de alfa o Beta Kubernetes (no compatibles)

AKS sólo admite características estables en el proyecto ascendente de Kubernetes. No se admiten características alfa o Beta disponibles en Kubernetes ascendente a menos que en caso contrario, se comunican y documentado.

Hay dos casos donde es posible que se implanta características alfa o Beta antes de la disponibilidad general:

* Los clientes han cumplido con el producto AKS, soporte técnico o los equipos de ingeniería y ha solicitado explícitamente para probar estas nuevas características.
* Estas características han sido [habilita a través de una marca de características] [ 2] (explícita opt-in)

## <a name="preview-features--feature-flags"></a>Características de versión preliminar / marcas de características

Para las características y funcionalidad que requieren la Comunidad de prueba, extendido y comentarios de los usuarios, Microsoft publicará las nuevas características de vista previa o características detrás de una marca de características. Estas características deben considerarse preliminares / Beta y se exponen para dar una oportunidad para probar estas nuevas características de los usuarios.

Sin embargo, estos obtener una vista previa / marca de características características no están diseñados para su uso en producción: API, cambio de comportamiento, correcciones de errores y otros cambios se pueden hacer que puede dar lugar a tiempos de inactividad y clústeres inestables. Compatibilidad con estas características se limita a los informes de error o problema. No habilite estas características en los sistemas de producción o suscripciones.

> [!NOTE]
> Habilitación de características de vista previa tiene efecto en el Azure **suscripción** nivel. No instale las características de vista previa en la suscripción de producción tal como puede cambiar el comportamiento de API predeterminado que afectan a las operaciones normales.

## <a name="upstream-bugs-and-issues"></a>Problemas y errores de nivel superior

Dada la velocidad de desarrollo en el proyecto ascendente de Kubernetes, existen invariablemente errores que no se puede revisar o trabajado alrededor dentro del sistema AKS y en su lugar, requieren mayor revisiones a los proyectos de nivel superior (como Kubernetes, sistemas operativos de nodo y de trabajo y núcleos). Para los componentes de que nuestra propiedad (por ejemplo, el proveedor de nube de Azure), el personal de AKS y Azure se confirman para corregir el problema de nivel superior de la Comunidad.

Para los casos donde un problema de soporte técnico es la causa raíz para uno o varios errores de nivel superior, compatible con AKS e ingeniería hará lo siguiente:

* Identifique y vincular los errores de nivel superior con los detalles adicionales sobre por qué esto afecta a su clúster o la carga de trabajo. Se proporcionará a los clientes con vínculos a los repositorios necesarios/problemas para ver los problemas y ver cuándo una nueva versión de Kubernetes y otros incluirá el fix
* Posibles soluciones o mitigaciones: En algunos casos, es posible solucionar el problema: en este caso, un "[conocido problema](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue)" se almacenará en el repositorio AKS que explica:
  * El problema y el vínculo a los errores de nivel superior
  * La solución y los detalles en torno a la actualización/otro persistencia de la solución
  * Escalas de tiempo aproximado para su inclusión en función de la cadencia de versión ascendente

[1]: https://github.com/Azure/AKS/releases
[2]: https://github.com/Azure/AKS/blob/master/previews.md
[3]: https://docs.microsoft.com/azure/aks/
