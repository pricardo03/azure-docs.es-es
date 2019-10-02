---
title: Ofertas de servicios administrados en Azure Marketplace
description: Los servicios administrados permiten a los proveedores de servicios vender ofertas de administración de recursos a los clientes de Azure Marketplace.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 09/19/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 0a6371a1020182650d83d5e7ec9bb2378b44b557
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155268"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Ofertas de servicios administrados en Azure Marketplace

En este artículo se describe el nuevo tipo de oferta de **Servicios administrados** de [Azure Marketplace](https://azuremarketplace.microsoft.com). Las ofertas de servicios administrados le permiten ofrecer servicios de administración de recursos a los clientes con la administración de recursos delegados de Azure. Puede hacer que estas ofertas estén disponibles para todos los clientes potenciales o solo para uno o varios clientes específicos. Dado que los clientes facturan directamente los costos relacionados con estos servicios administrados, Microsoft no aplica ningún cargo.

## <a name="understand-managed-services-offers"></a>Comprender las ofertas de servicios administrados

Las ofertas de servicios administrados simplifican el proceso de incorporación de clientes para la administración de recursos delegados de Azure. Cuando un cliente compre una oferta en Azure Marketplace, podrá especificar los suscripciones o grupos de recursos que deben incorporarse. Tenga en cuenta que la incorporación de la suscripción se debe autorizar de antemano mediante el registro manual del proveedor de recursos **Microsoft.ManagedServices**.

Después, los usuarios de la organización podrán realizar tareas de administración para esos recursos desde dentro del inquilino de la organización en función del acceso que se haya definido al crear la oferta en [Cloud Partner Portal](https://cloudpartner.azure.com/). Esto se realiza mediante un manifiesto que especifica los usuarios, grupos y entidades de servicio de Azure AD que tendrán acceso a los recursos del cliente mediante la administración de recursos delegados de Azure, junto con los roles que definen su nivel de acceso. Al asignar permisos a un grupo de Azure AD, en lugar de una serie de cuentas de usuario o de aplicación individuales, puede agregar o quitar usuarios individuales cuando cambien sus requisitos de acceso.

## <a name="public-and-private-offers"></a>Ofertas públicas y privadas

Cada oferta de servicios administrados incluye uno o varios planes. Estos planes pueden ser privados o públicos.

Si quiere limitar su oferta a clientes específicos, puede publicar un plan privado. Al hacerlo, el plan solo se puede comprar para los identificadores de suscripción específicos que proporcione. Para obtener más información, consulte [Ofertas privadas](https://docs.microsoft.com/azure/marketplace/private-offers).

Los planes públicos permiten promover los servicios para nuevos clientes. Suelen ser más adecuadas cuando solo se necesita acceso limitado al inquilino del cliente. Una vez establecida una relación con un cliente, si decide conceder acceso adicional a su organización, puede hacerlo mediante la publicación de un nuevo plan privado solo para ese cliente, o bien [mediante su incorporación para ampliar su acceso mediante plantillas de Azure Resource Manager](../how-to/onboard-customer.md).

Tenga en cuenta que, una vez publicado un plan como público, no puede cambiarlo a privado. Además, no puede restringir la disponibilidad de un plan público a determinados clientes ni a un determinado número de clientes, aunque puede dejar de vender el plan por completo si decide hacerlo.

Si es necesario, puede incluir planes públicos y privados en la misma oferta.

## <a name="publish-managed-service-offers"></a>Publicar ofertas de servicio administrado

Para obtener información sobre cómo publicar una oferta de servicios administrados, consulte [Publish a Managed Services offer to Azure Marketplace](../how-to/publish-managed-services-offers.md) (Publicación de una oferta de servicios administrados en Azure Marketplace). Para obtener información general sobre la publicación en Azure Marketplace mediante Cloud Partner Portal, consulte [Azure Marketplace and AppSource Publishing Guide](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) (Guía de publicación de Azure Marketplace y AppSource) y [Administración de las ofertas del Marketplace de Azure y AppSource](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de la [administración de recursos delegados de Azure](azure-delegated-resource-management.md) y las [experiencias de administración entre inquilinos](cross-tenant-management-experience.md).
- [Publicar ofertas de servicios administrados](../how-to/publish-managed-services-offers.md) en Azure Marketplace.
