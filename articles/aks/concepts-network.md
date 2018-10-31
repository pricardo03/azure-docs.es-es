---
title: 'Conceptos: Redes en Azure Kubernetes Service (AKS)'
description: Conozca más información sobre las redes en Azure Kubernetes Service (AKS), incluyendo redes básicas y avanzadas, controladores de entrada, equilibradores de carga y direcciones IP estáticas.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: 62ba98f221041d5bbf9bb095a02d052218eb0fd0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49380890"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Conceptos de redes de aplicaciones en Azure Kubernetes Service (AKS)

En una aproximación de los microservicios basados en contenedores al desarrollo de aplicaciones, los componentes de la aplicación deben funcionar juntos para procesar sus tareas. Kubernetes proporciona diversos recursos que permiten esta comunicación con la aplicación. Puede conectarse a aplicaciones y exponer estas de forma interna o externa. Para compilar aplicaciones de alta disponibilidad, puede equilibrar la carga de sus aplicaciones. Las aplicaciones más complejas pueden requerir la configuración del tráfico de entrada para la terminación SSL/TLS o el enrutamiento de varios componentes. Por motivos de seguridad, es posible que deba restringir el flujo de tráfico de red entre pods y nodos.

En este artículo se presentan los conceptos básicos que proporcionan redes para sus aplicaciones en AKS:

- [Servicios](#services)
- [Redes virtuales de Azure](#azure-virtual-networks)
- [Controladores de entrada](#ingress-controllers)
- [Directivas de red](#network-policies)

## <a name="kubernetes-basics"></a>Aspectos básicos de Kubernetes

Para permitir el acceso a las aplicaciones o para que los componentes de las aplicaciones se comuniquen entre sí, Kubernetes proporciona una capa de abstracción para la red virtual. Los nodos de Kubernetes están conectados a una red virtual y pueden proporcionar conectividad de entrada y salida para los pods. El componente *kube-proxy* se ejecuta en cada nodo para proporcionar estas características de red.

En Kubernetes, los *servicios* agrupan lógicamente los pods para permitir el acceso directo a través de una dirección IP o nombre DNS y en un puerto específico. También puede distribuir el tráfico mediante un *equilibrador de carga*. También se puede lograr un enrutamiento del tráfico de la aplicación más complejo mediante los *controladores de entradas*. La seguridad y el filtrado del tráfico de red de los pods es posible con las *directivas de red* de Kubernetes.

La plataforma Azure también ayuda a simplificar las redes virtuales de los clústeres de AKS. Cuando se crea un equilibrador de carga de Kubernetes, se crea y se configura el recurso de equilibrador de carga de Azure subyacente. Cuando abre los puertos de red a los pods, se configuran las reglas de los grupos de seguridad de red de Azure correspondientes. Para el enrutamiento de aplicaciones HTTP, Azure también puede configurar un *DNS externo* a medida que se configuran nuevas rutas de entrada.

## <a name="services"></a>Services

Para simplificar la configuración de red de las cargas de trabajo de la aplicación, Kubernetes usa *servicios* para agrupar lógicamente un conjunto de pods y proporcionar conectividad de red. Están disponibles los siguientes tipos de servicio:

- **Dirección IP de clúster**: crea una dirección IP interna para su uso dentro del clúster de AKS. Es útil solo para aplicaciones internas que admiten otras cargas de trabajo dentro del clúster.

    ![Diagrama que muestra el flujo de tráfico de Dirección IP de clúster en un clúster de AKS][aks-clusterip]

- **NodePort**: crea una asignación de puerto en el nodo subyacente que permite que se pueda acceder a la aplicación directamente con la dirección IP del nodo y el puerto.

    ![Diagrama que muestra el flujo de tráfico de NodePort en un clúster de AKS][aks-nodeport]

- **LoadBalancer**: crea un recurso de equilibrador de carga de Azure, configura una dirección IP externa y conecta los pods solicitados al grupo de back-end del equilibrador de carga. Para permitir que el tráfico de los clientes llegue a la aplicación, se crean las reglas de equilibrio de carga en los puertos deseados. 

    ![Diagrama que muestra el flujo de tráfico de LoadBalancer en un clúster de AKS][aks-loadbalancer]

    Para mayor control y el enrutamiento del tráfico entrante, puede usar en su lugar un [controlador de entrada](#ingress-controllers).

- **ExternalName**: crea una entrada DNS específica para facilitar el acceso a la aplicación.

La dirección IP de los equilibradores de carga y de los servicios se puede asignar dinámicamente, o puede especificar una dirección IP estática existente para su uso. Se pueden asignar direcciones IP estáticas internas y externas. Esta dirección IP estática existente está a menudo asociada a una entrada DNS.

Se pueden crear equilibradores de carga *internos* y *externos*. Solo se asigna una dirección IP privada a los equilibradores de carga internos, por lo que no se puede acceder a ellos desde Internet.

## <a name="azure-virtual-networks"></a>Redes virtuales de Azure

En AKS, puede implementar un clúster que use uno de los dos siguientes modelos de red:

- Red *básica*: los recursos de la red se crean y se configuran cuando se implementa el clúster de AKS.
- Red *avanzada*: el clúster de AKS se conecta a los recursos y configuraciones de la red virtual existentes.

### <a name="basic-networking"></a>Red básica

La opción de red *básica* es la configuración predeterminada para la creación del clúster de AKS. La plataforma Azure administra la configuración de red del clúster y los pods. Red básica es adecuada para implementaciones que no requieren una configuración de red virtual personalizada. Cuando selecciona la red Básica, no puede definir la configuración de red, como los nombres de las subredes o los intervalos de direcciones IP asignados al clúster de AKS.

Los nodos de un clúster de AKS configurado para usar la red Básica utilizan el complemento [kubenet][kubenet] de Kubernetes.

La red Básica proporciona las siguientes características:

- Exponer un servicio de Kubernetes de forma externa o de forma interna mediante Azure Load Balancer.
- Los pods pueden acceder a recursos públicos de Internet.

### <a name="advanced-networking"></a>Redes avanzadas

La red *Avanzada* colocará sus pods en una red virtual de Azure que configure. Esta red virtual proporciona conectividad automática a otros recursos de Azure y la integración con un amplio conjunto de funcionalidades. La red Avanzada es adecuada para implementaciones que requieren configuraciones de red virtual específicas, como aquellas para utilizar una subred y la conectividad existente. Con la opción de red Avanzada, puede definir estos nombres de subred y los intervalos de direcciones IP.

Los nodos de un clúster de AKS configurado para el uso de red Avanzada utilizan el complemento [Azure Container Networking Interface (CNI)][cni-networking] de Kubernetes.

![Diagrama que muestra dos nodos con puentes que conectan cada uno a una única red virtual de Azure][advanced-networking-diagram]

La red Avanzada proporciona las siguientes características en comparación con la red Básica:

- Implementar el clúster de AKS en una red virtual existente o crear una nueva red virtual y subred para el clúster.
- Cada pod del clúster se asigna una dirección IP de la red virtual. Los pods pueden comunicarse directamente con otros pods del clúster y con otros nodos de la red virtual.
- Un pod se puede conectar con otros servicios de una red virtual emparejada y con redes locales a través de ExpressRoute y conexiones VPN de sitio a sitio (S2S). Los pods también son accesibles desde el entorno local.
- Los pods de una subred que tienen puntos de conexión de servicio habilitados pueden conectarse de forma segura a los servicios de Azure, como Azure Storage o SQL Database.
- Puede crear rutas definidas por el usuario (UDR) para redirigir el tráfico desde los pods a una aplicación virtual de red.

Para más información, consulte [Configurar redes avanzadas en Azure Kubernetes Service (AKS)][aks-configure-advanced-networking].

## <a name="ingress-controllers"></a>Controladores de entrada

Cuando se crea un servicio del tipo LoadBalancer, se crea un recurso subyacente de equilibrador de carga de Azure. El equilibrador de carga está configurado para distribuir el tráfico a los pods en su servicio en un puerto determinado. LoadBalancer solo funciona en la capa 4: el servicio no conoce las aplicaciones reales y no puede realizar ninguna consideración de enrutamiento adicional.

Los *controladores de entrada* funcionan en la capa 7 y puede usar reglas más inteligentes para distribuir el tráfico de la aplicación. Un uso habitual de un controlador de entrada es enrutar el tráfico HTTP a diferentes aplicaciones basadas en la URL de entrada.

![Diagrama que muestra el flujo de tráfico de entrada en un clúster de AKS][aks-ingress]

En AKS, puede crear un recurso de entrada con algo parecido a NGINX, o usar la característica de enrutamiento de aplicación HTTP de AKS. Cuando se habilita el enrutamiento de aplicación HTTP para un clúster de AKS, la plataforma Azure crea el controlador de entrada y un controlador de *DNS externo*. Cuando se crean nuevos recursos de entrada en Kubernetes, se crean los registros DNS A necesarios en una zona DNS específica del clúster. Para más información, consulte información sobre la implementación del [Enrutamiento de aplicación HTTP][aks-http-routing].

Otra característica común de los controladores de entrada es la terminación SSL/TLS. En aplicaciones web de gran tamaño, a las que se accede a través de HTTPS, la terminación TLS puede controlarse mediante el recurso de entrada en lugar de en la propia aplicación. Para proporcionar una configuración y generación automática de certificados TLS, puede configurar el recurso de entrada para que use proveedores como Let's Encrypt. Para más información sobre cómo configurar un controlador de entrada de NGINX con Let's Encrypt, consulte [Entrada y TLS][aks-ingress-tls].

## <a name="network-security-groups"></a>Grupos de seguridad de red

Un grupo de seguridad de red filtra el tráfico de las máquinas virtuales como, por ejemplo, el de los nodos de AKS. Al crear los servicios, como LoadBalancer, la plataforma Azure configura automáticamente las reglas de grupo de seguridad de red que son necesarias. No configure manualmente las reglas del grupo de seguridad de red para filtrar el tráfico de los pods en un clúster de AKS. Defina los puertos necesarios y el reenvío como parte de los manifiestos de servicio de Kubernetes y permita que la plataforma Azure cree o actualice las reglas correspondientes.

Existen reglas predeterminadas de grupos de seguridad de red para tráfico como el de SSH. Estas reglas predeterminadas son para solucionar problemas de acceso y administración de clústeres. La eliminación de estas reglas predeterminadas puede causar problemas con la administración de AKS y rompe el objetivo de nivel de servicio (SLO).

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con redes de AKS, consulte [Configurar redes avanzadas en Azure Kubernetes Service (AKS)][aks-configure-advanced-networking].

Para obtener más información sobre los conceptos básicos de Kubernetes y AKS, consulte los artículos siguientes:

- [Kubernetes / AKS clusters and workloads][aks-concepts-clusters-workloads] (Clústeres y cargas de trabajo de Kubernetes/AKS)
- [Kubernetes / AKS access and identity][aks-concepts-identity] (Acceso e identidad de Kubernetes/AKS)
- [Kubernetes / AKS security][aks-concepts-security] (Seguridad de Kubernetes/AKS)
- [Kubernetes / AKS storage][aks-concepts-storage] (Almacenamiento de Kubernetes/AKS)
- [Kubernetes / AKS scale][aks-concepts-scale] (Escala de Kubernetes/AKS)

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-advanced-networking]: configure-advanced-networking.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md