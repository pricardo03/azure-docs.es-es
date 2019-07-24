---
title: Azure Lighthouse y el programa Proveedor de soluciones en la nube
description: Al usar la administración de recursos delegados de Azure, es importante tener en cuenta la seguridad y el control de acceso.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 09552c66d2dc841cff8b5cb52e26dc657568e08f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810999"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse y el programa Proveedor de soluciones en la nube

Si es un asociado [CSP (proveedor de soluciones en la nube)](https://docs.microsoft.com/partner-center/csp-overview), puede tener acceso a las suscripciones a Azure creadas para sus clientes desde el programa CSP mediante la funcionalidad [Administrar en nombre de (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO). Este acceso le permite admitir, configurar y administrar directamente las suscripciones de sus clientes.

El mecanismo AOBO concede acceso completo a los entornos de los clientes. El uso de la administración de recursos delegados de Azure junto con AOBO ayuda a mejorar la seguridad, ya que permite reducir el acceso innecesario al habilitar permisos más detallados para los usuarios. 

## <a name="administer-on-behalf-of-aobo"></a>Administrar en nombre de (AOBO)

Con AOBO, cualquier usuario con el rol [Agente de administración](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) en el inquilino tendrá acceso AOBO a las suscripciones a Azure que cree desde el programa CSP. Cualquier usuario que necesite tener acceso a las suscripciones de los clientes debe ser miembro de este grupo. AOBO no permite la flexibilidad de crear grupos distintos que funcionen con distintos clientes ni de habilitar diferentes roles para grupos o usuarios.

![Administración de inquilinos mediante AOBO](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Administración de recursos delegados de Azure

Con la administración de recursos delegados de Azure, puede asignar grupos diferentes a distintos clientes o roles, tal y como se muestra en el diagrama siguiente. Dado que los usuarios tendrán el nivel de acceso adecuado a través de la administración de recursos delegados de Azure, puede reducir el número de usuarios que tienen el rol de agente de administración (y, por tanto, tienen acceso AOBO completo). Esto proporciona una mayor seguridad, ya que limita el acceso innecesario a los recursos de los clientes. También le ofrece más flexibilidad para administrar varios clientes a escala.

Para incorporar una suscripción creada desde el programa CSP, siga los pasos descritos en [Onboard a subscription to Azure delegated resource management](../how-to/onboard-customer.md) (Incorporación de una suscripción a la administración de recursos delegados de Azure). Cualquier usuario que tenga el rol de agente de administración en el inquilino puede realizar esta incorporación.

Tenga en cuenta que, para las suscripciones creadas a través de los programas CSP, solo pueden generar solicitudes de soporte técnico los usuarios con el rol de agente de administración en el inquilino del proveedor de servicios. Los usuarios agregados mediante la administración de recursos delegados de Azure no podrán abrir solicitudes de soporte técnico para los recursos delegados en estas suscripciones.

![Administración de inquilinos mediante AOBO y la administración de recursos delegados de Azure](../media/csp-2.jpg)

## <a name="partner-admin-link"></a>Vínculo de administración de asociados

Puede asociar el identificador de Microsoft Partner Network (MPN) con las suscripciones incorporadas para realizar un seguimiento de su impacto en las involucraciones de clientes.

Si [publica una oferta de servicios administrados en Azure Marketplace](../how-to/publish-managed-services-offers.md), el id. de MPN se asocia con su perfil de editor y se asocia automáticamente con la oferta. Los ingresos que generen los recursos de Azure mediante esta oferta se atribuirán a su organización. En los sistemas de informes de asociados, como el Centro de partners o MPN, la atribución aparecerá como Vínculo de administración de asociados (PAL).

Si [incorpora clientes para la administración de recursos delegados de Azure con plantillas de Azure Resource Manager](../how-to/onboard-customer.md), puede asociar igualmente el identificador de MPN para recibir el reconocimiento de su impacto en las involucraciones de los clientes, pero tendrá que hacerlo manualmente. Para más información, consulte [Vinculación de un Id. de partner a cuentas de Azure](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started). 

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [experiencias de administración entre inquilinos](cross-tenant-management-experience.md).
- Más información acerca del [programa Proveedor de soluciones en la nube](https://docs.microsoft.com/partner-center/csp-overview).
