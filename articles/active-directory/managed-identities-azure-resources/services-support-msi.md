---
title: Servicios de Azure que admiten identidades administradas para recursos de Azure
description: Lista de servicios que admiten identidades administradas para recursos de Azure y autenticación de Azure AD
services: active-directory
author: daveba
ms.author: daveba
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: f09704a5befedb99625595e50587fa6bbd704899
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106578"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Servicios que admiten identidades administradas para recursos de Azure

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Mediante una identidad administrada puede autenticarse en cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. Estamos en el proceso de integración de identidades administradas para recursos de Azure y de la autenticación de Azure AD en Azure. Compruebe con frecuencia si existen actualizaciones.

> [!NOTE]
> Managed Identities for Azure Resources es el nombre por el que ahora se conoce al servicio Managed Service Identity (MSI).

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Servicios de Azure que admiten identidades administradas de recursos de Azure

Los siguientes servicios de Azure admiten identidades administradas para recursos de Azure:

| Servicio | Estado asignado por el sistema | Estado asignado por el usuario| Configuración | Obtención de un token |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Vista previa | Vista previa | [Azure Portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[CLI de Azure](qs-configure-cli-windows-vm.md)<br>[Plantillas del Administrador de recursos de Azure](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Virtual Machine Scale Sets | Vista previa | Vista previa | [Azure Portal](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[CLI de Azure](qs-configure-cli-windows-vmss.md)<br>[Plantillas del Administrador de recursos de Azure](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Azure App Service | Disponible | No disponible | [Azure Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[CLI de Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Plantilla de Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Azure Functions | Disponible | No disponible | [Azure Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[CLI de Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Plantilla de Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Azure Data Factory V2 | Vista previa | No disponible | [Azure Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| Azure API Management | Disponible | No disponible | [Plantilla de Azure Resource Manager](/azure/api-management/api-management-howto-use-managed-service-identity) | 


## <a name="azure-services-that-support-azure-ad-authentication"></a>Servicios de Azure que admiten la autenticación de Azure AD

Los siguientes servicios admiten la autenticación de Azure AD y se han probado con los servicios de cliente que usan identidades administradas para recursos de Azure.

| Servicio | Id. de recurso | Status | Date | Asignación de acceso |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Disponible | Septiembre de 2017 | [Azure Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[CLI de Azure](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Disponible | Septiembre de 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Disponible | Septiembre de 2017 | |
| Azure SQL | https://database.windows.net/ | Disponible | Octubre de 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Disponible | Diciembre de 2017 | |
| Azure Service Bus | https://servicebus.azure.net | Disponible | Diciembre de 2017 | |
| Azure Storage | https://storage.azure.com/ | Vista previa | Mayo de 2018 | |