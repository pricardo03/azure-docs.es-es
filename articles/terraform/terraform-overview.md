---
title: Uso de Terraform con Azure
description: Introducción al uso de Terraform para versionar e implementar la infraestructura de Azure.
services: terraform
ms.service: terraform
keywords: terraform, devops, información general, plan, aplicar, automatizar
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/31/2018
ms.openlocfilehash: 962631728f96e0551f9cc18d5d835928e5a7705a
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567985"
---
# <a name="terraform-with-azure"></a>Terraform con Azure

[Hashicorp Terraform](https://www.terraform.io/) es una herramienta de código abierto para aprovisionar y administrar la infraestructura de nube. Codifica la infraestructura en archivos de configuración que describen la topología de los recursos de nube, como máquinas virtuales, cuentas de almacenamiento e interfaces de red. La interfaz de línea de comandos (CLI) de Terraform proporciona un mecanismo sencillo para implementar y versionar los archivos de configuración en Azure o en cualquier otra nube compatible.

En este artículo se describen las ventajas de usar Terraform para administrar la infraestructura de Azure.

## <a name="automate-infrastructure-management"></a>Automatice la administración de la infraestructura.

Los archivos de configuración basados en plantilla de Terraform permiten definir, aprovisionar y configurar recursos de Azure de forma repetible y predecible. Automatizar la infraestructura tiene varias ventajas:

- Reduce el potencial de errores humanos al implementar y administrar la infraestructura.
- Implementa la misma plantilla varias veces para crear entornos de desarrollo, prueba y producción idénticos.
- Reduce el costo de entornos de desarrollo y prueba al crearlos a petición.

## <a name="understand-infrastructure-changes-before-they-are-applied"></a>Comprensión de los cambios de infraestructura antes de que se apliquen 

A medida que la topología de los recursos se vuelve más compleja, comprender el significado y el efecto de los cambios de infraestructura puede resultar difícil.

Terraform proporciona una interfaz de línea de comandos (CLI) que permite a los usuarios validar y obtener una vista previa de los cambios de infraestructura antes de implementarlos. Obtener una vista previa de los cambios de infraestructura de forma segura y productiva tiene varias ventajas:
- Los miembros del equipo pueden colaborar de forma más eficaz al comprender rápidamente los cambios propuestos y su impacto.
- Los cambios no deseados se pueden detectar al principio del proceso de desarrollo


## <a name="deploy-infrastructure-to-multiple-clouds"></a>Implementación de la infraestructura en varias nubes

Terraform es una herramienta popular para escenarios con varias nubes, donde se implementa una infraestructura similar en Azure y otros proveedores de soluciones en la nube o en centros de datos locales. Permite a los desarrolladores usar las mismas herramientas y archivos de configuración para administrar la infraestructura en varios proveedores de soluciones en la nube.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una visión general de Terraform y sus ventajas, estos son los próximos pasos sugeridos:

- Para empezar, [instale Terraform y configúrelo para que use Azure](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure).
- [Cree una máquina virtual de Azure con Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-create-complete-vm).
- Explore el [módulo Azure Resource Manager para Terraform](https://www.terraform.io/docs/providers/azurerm/). 