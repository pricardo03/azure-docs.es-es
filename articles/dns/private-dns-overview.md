---
title: Uso de Azure DNS para dominios privados
description: Información general del servicio de hospedaje de DNS privado en Microsoft Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 1/10/2019
ms.author: victorh
ms.openlocfilehash: e426e38ce5366f7c0d8b8bc20a639d827ea9e261
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200532"
---
# <a name="use-azure-dns-for-private-domains"></a>Uso de Azure DNS para dominios privados

El Sistema de nombres de dominio, o DNS, es responsable de traducir o resolver el nombre del servicio en su dirección IP. Azure DNS es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres mediante la infraestructura de Microsoft Azure. Además de los dominios DNS con conexión a Internet, Azure DNS ahora también admite dominios DNS privados como una característica en versión preliminar.

Azure DNS proporciona un servicio DNS confiable y seguro para administrar y resolver los nombres de dominio en una red virtual sin necesidad de agregar una solución DNS personalizada. Al utilizar zonas DNS privadas, puede usar sus propios nombres de dominio personalizados en lugar de los nombres proporcionados por Azure que están disponibles actualmente. El uso de los nombres de dominio personalizados facilitan la adaptación de la arquitectura de red virtual para satisfacer mejor las necesidades de su organización. Proporciona la resolución de nombres para las máquinas virtuales (VM) dentro de una red virtual y entre redes virtuales. Además, puede configurar nombres de zonas con una vista de horizonte dividido que permite que una zona DNS privada y otra pública compartan el nombre.

Si especifica una red virtual de registro, los registros DNS de las máquinas virtuales de dicha red que se registren en la zona privada no se pueden ver o recuperar desde Azure Powershell ni las API de la CLI de Azure, pero los registros de máquina virtual se registran y resuelven correctamente.

![Información general de DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Ventajas

Azure DNS proporciona las ventajas siguientes:

* **Eliminación de la necesidad de contar con soluciones DNS personalizadas**. Anteriormente, muchos clientes crearon soluciones DNS personalizadas para administrar zonas DNS en su red virtual. La administración de zonas DNS ahora se puede realizar con la infraestructura nativa de Azure, lo que elimina la carga de tener que crear y administrar soluciones DNS personalizadas.

* **Uso de todos los tipos de registros de DNS comunes**. Azure DNS admite los registros A, AAAA, CNAME, MX, NS, PTR, SOA, SRV y TXT.

* **Administración automática de registros de nombres de host**. Además de hospedar los registros de DNS personalizados, Azure mantiene automáticamente los registros de nombres de host de las máquinas virtuales en las redes virtuales especificadas. En este escenario, puede optimizar los nombres de dominio usados sin necesidad de crear soluciones DNS personalizadas ni de modificar aplicaciones.

* **Resolución de nombres de host entre redes virtuales**. A diferencia de los nombres de host proporcionados por Azure, las zonas DNS privadas pueden compartirse entre redes virtuales. Esta funcionalidad simplifica los escenarios de detección de servicios y de distintas redes, como el emparejamiento de red virtual.

* **Experiencia del usuario y herramientas familiares**. Para reducir la curva de aprendizaje, esta nueva oferta usa herramientas de Azure DNS establecidas (PowerShell, plantillas de Azure Resource Manager y la API de REST).

* **Compatibilidad con DNS de horizonte dividido**. Con Azure DNS, puede crear zonas con el mismo nombre que se resuelve en diferentes respuestas desde dentro de una red virtual y desde la red pública de Internet. Un escenario típico de DNS de horizonte dividido es proporcionar una versión dedicada de un servicio para usarla dentro de la red virtual.

* **Disponibilidad en todas las regiones de Azure**. La característica de zonas privadas de Azure DNS está disponible en todas las regiones de Azure de la nube pública de Azure.

## <a name="capabilities"></a>Capacidades

Azure DNS ofrece las funcionalidades siguientes:

* **Registro automático de máquinas virtuales desde una única red virtual vinculada a una zona privada como una red virtual de registro**. Las máquinas virtuales se registran (agregan) a la zona privada como registros A que apuntan a sus direcciones IP privadas. Cuando se elimina una máquina virtual de una red virtual de registro, Azure también quita automáticamente el registro DNS correspondiente de la zona privada vinculada. 

  De manera predeterminada, las redes virtuales de registro también funcionan como redes virtuales de resolución, en el sentido de que la resolución de DNS en la zona funciona desde cualquiera de las máquinas virtuales de la red virtual de registro.

  > [!NOTE]
  > Si especifica una red virtual de registro, los registros DNS de las máquinas virtuales de dicha red que se registren en la zona privada no se pueden ver o recuperar desde Azure Powershell ni las API de la CLI de Azure. Sin embargo, los registros de la máquina virtual se registran y resuelven de manera correcta.

* **Se admite la resolución DNS de reenvío entre redes virtuales vinculadas a la zona privada como redes virtuales de resolución**. Para la resolución DNS entre redes virtuales, no hay ninguna dependencia explícita de que las redes virtuales estén emparejadas unas con otras. Pero es posible que los clientes quieran emparejar redes virtuales en otras situaciones (por ejemplo, tráfico HTTP).

* **Se admite la búsqueda inversa de DNS dentro del ámbito de la red virtual**. La búsqueda inversa de DNS de una dirección IP privada dentro de la red virtual asignada a una zona privada devuelve el FQDN que incluye el nombre de host/registro, así como el nombre de zona como sufijo.

## <a name="limitations"></a>Limitaciones

Azure DNS tiene las siguientes limitaciones:

* Solo se permite una red virtual de registro por zona privada.
* Se permiten hasta diez redes virtuales de registro por zona privada.
* Una red virtual concreta se puede vincular a una sola zona privada como red virtual de registro.
* Una red virtual determinada se puede vincular a un máximo de diez zonas privadas como red virtual de resolución.
* Si especifica una red virtual de registro, los registros DNS de las máquinas virtuales de dicha red que se registren en la zona privada no se pueden ver o recuperar desde Azure Powershell ni las API de la CLI de Azure. Sin embargo, los registros de la máquina virtual se registran y resuelven de manera correcta.
* El DNS inverso solo funciona en el espacio de direcciones IP privadas de la red virtual de registro.
* El DNS inverso de una dirección IP privada que no está registrada en la zona privada (por ejemplo, la IP privada de una máquina virtual de una red virtual vinculada como red virtual de resolución a una zona privada) devuelve *internal.cloudapp.net* como sufijo de DNS. Sin embargo, este sufijo no se puede resolver.
* La red virtual debe estar completamente vacía cuando se vincula por primera vez con una zona privada como red virtual de registro o resolución. Pero la red virtual podría no estar vacía para una futura vinculación como red virtual de registro o resolución a otras zonas privadas.
* Actualmente, no se admite el reenvío condicional (por ejemplo, para habilitar la resolución entre redes de Azure y locales). Para obtener información sobre cómo los clientes pueden poner en funcionamiento este escenario con otros mecanismos, vea [Resolución de nombres para las máquinas virtuales e instancias de rol](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Para ver algunas preguntas y respuestas comunes sobre las zonas privadas en Azure DNS, como el comportamiento específico de registro y resolución DNS que se puede esperar con determinadas clases de operaciones, vea [Preguntas frecuentes](./dns-faq.md#private-dns).  

## <a name="pricing"></a>Precios

La característica de zonas DNS privadas es gratuita durante la versión preliminar pública. Con disponibilidad general, esta característica ofrece un modelo de precios basado en el uso similar a la oferta existente de Azure DNS. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo crear una zona privada en Azure DNS mediante [Azure PowerShell](./private-dns-getstarted-powershell.md) o la [CLI de Azure](./private-dns-getstarted-cli.md).

Obtenga información sobre algunos [escenarios de zona privada](./private-dns-scenarios.md) habituales que pueden realizarse con zonas privadas en Azure DNS.

Para ver algunas preguntas y respuestas comunes sobre las zonas privadas en Azure DNS, como el comportamiento específico que se puede esperar con determinadas clases de operaciones, vea [Preguntas frecuentes](./dns-faq.md#private-dns). 

Visite [Información general sobre zonas y registros de DNS](dns-zones-records.md) para obtener información sobre zonas y registros DNS.

Obtenga información sobre las demás [funcionalidades de red](../networking/networking-overview.md) clave de Azure.
