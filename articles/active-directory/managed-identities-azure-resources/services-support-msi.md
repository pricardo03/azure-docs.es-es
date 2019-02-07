---
title: Servicios de Azure que admiten identidades administradas para recursos de Azure
description: Lista de servicios que admiten identidades administradas para recursos de Azure y autenticación de Azure AD
services: active-directory
author: priyamohanram
ms.author: priyamo
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: daveba
ms.openlocfilehash: 8274909b40d872309230f9c9f6c045b4283fd6eb
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55752337"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Servicios que admiten identidades administradas para recursos de Azure

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Mediante una identidad administrada puede autenticarse en cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. Estamos en el proceso de integración de identidades administradas para recursos de Azure y de la autenticación de Azure AD en Azure. Compruebe con frecuencia si existen actualizaciones.

> [!NOTE]
> Identidades administradas para recursos de Azure es el nombre con el que ahora se conoce al servicio Managed Service Identity (MSI).

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Servicios de Azure que admiten identidades administradas de recursos de Azure

Los siguientes servicios de Azure admiten identidades administradas para recursos de Azure:

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

|Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | Vista previa | Vista previa | Vista previa | Vista previa |
| Asignado por el usuario | Vista previa | Vista previa | Vista previa | Vista previa | Vista previa

Consulte la lista siguiente para configurar la identidad administrada para Azure Virtual Machines (en las regiones donde esté disponible):

- [Azure Portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [CLI de Azure](qs-configure-cli-windows-vm.md)
- [Plantillas del Administrador de recursos de Azure](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Conjuntos de escalado de máquinas virtuales de Azure

|Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | Vista previa | Vista previa | Vista previa |
| Asignado por el usuario | Vista previa | Vista previa | Vista previa | Vista previa

Consulte la lista siguiente para configurar la identidad administrada para Azure Virtual Machine Scale Sets (en las regiones donde esté disponible):

- [Azure Portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [CLI de Azure](qs-configure-cli-windows-vm.md)
- [Plantillas del Administrador de recursos de Azure](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

|Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | Disponible | Disponible | Disponible |
| Asignado por el usuario | Vista previa | No disponible | No disponible | No disponible

Consulte la lista siguiente para configurar la identidad administrada para Azure App Service (en las regiones donde esté disponible):

- [Azure Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [CLI de Azure](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Plantilla de Azure Resource Manager](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-functions"></a>Azure Functions

Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | Disponible | Disponible | Disponible |
| Asignado por el usuario | Vista previa | No disponible | No disponible | No disponible

Consulte la lista siguiente para configurar la identidad administrada para Azure Functions (en las regiones donde esté disponible):

- [Azure Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [CLI de Azure](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Plantilla de Azure Resource Manager](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | Disponible | Disponible | Disponible |
| Asignado por el usuario | No disponible | No disponible | No disponible | No disponible

Consulte la lista siguiente para configurar la identidad administrada para Azure Logic Apps (en las regiones donde esté disponible):

- [Azure Portal](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Plantilla de Azure Resource Manager](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | No disponible | No disponible | No disponible |
| Asignado por el usuario | No disponible | No disponible | No disponible | No disponible

Consulte la lista siguiente para configurar la identidad administrada para Azure Data Factory V2 (en las regiones donde esté disponible):

- [Azure Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Disponible | Disponible | No disponible | No disponible |
| Asignado por el usuario | No disponible | No disponible | No disponible | No disponible

Consulte la lista siguiente para configurar la identidad administrada para Azure API Management (en las regiones donde esté disponible):

- [Plantilla de Azure Resource Manager](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

Tipo de identidad administrada |  Regiones globales de Azure<br>con disponibilidad general | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Asignado por el sistema | Linux: Vista previa<br>Windows: No disponible | No disponible | No disponible | No disponible |
| Asignado por el usuario | Linux: Vista previa<br>Windows: No disponible | No disponible | No disponible | No disponible

Consulte la lista siguiente para configurar la identidad administrada para Azure Container Instances (en las regiones donde esté disponible):

- [CLI de Azure](~/articles/container-instances/container-instances-managed-identity.md)
- [Plantilla de Azure Resource Manager](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)


## <a name="azure-services-that-support-azure-ad-authentication"></a>Servicios de Azure que admiten la autenticación de Azure AD

Los siguientes servicios admiten la autenticación de Azure AD y se han probado con los servicios de cliente que usan identidades administradas para recursos de Azure.

| Servicio | Id. de recurso | Status | Asignación de acceso |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Disponible | [Azure Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[CLI de Azure](howto-assign-access-CLI.md) <br>[Plantilla de Azure Resource Manager](../../role-based-access-control/role-assignments-template.md) |
| Azure Key Vault | `https://vault.azure.net` | Disponible |  
| Azure Data Lake | `https://datalake.azure.net/` | Disponible |
| Azure SQL | `https://database.windows.net/` | Disponible |
| Azure Event Hubs | `https://eventhubs.azure.net` | Vista previa |
| Azure Service Bus | `https://servicebus.azure.net` | Vista previa |
| Azure Storage | `https://storage.azure.com/` | Vista previa |
