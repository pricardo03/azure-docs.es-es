---
title: Conexión a Azure Alemania mediante la CLI de Azure | Microsoft Docs
description: Información acerca de la administración de suscripciones en Azure Alemania mediante la CLI de Azure
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: d2e94a7bd154195e7956df1a28407b0933e0e526
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033529"
---
# <a name="connect-to-azure-germany-by-using-azure-cli"></a>Conexión a Azure Alemania mediante la CLI de Azure
Para usar la interfaz de línea de comandos de Azure (CLI de Azure), es preciso conectarse a Azure Alemania, en lugar de a Azure global. La CLI de Azure Portal se puede usar para administrar una suscripción de gran tamaño a través de scripts o para acceder a características que no están actualmente disponibles en Azure Portal. Si ha usado la CLI de Azure en Azure global, básicamente es lo mismo.  

## <a name="azure-cli"></a>Azure CLI
Hay varias maneras de [instalar la CLI de Azure](https://docs.microsoft.com/cli/azure/install-az-cli2).  

Para conectarse a Azure Alemania, configure la nube:

```
az cloud set --name AzureGermanCloud
```

Después de que se haya configurado la nube, puede iniciar sesión:

```
az login --username your-user-name@your-tenant.onmicrosoft.de
```

Para confirmar que la nube está configurada correctamente en AzureGermanCloud, ejecute cualquiera de los comandos siguientes y, después, compruebe que la marca `isActive` se establece en `true` para el elemento AzureGermanCloud:

```
az cloud list
```

```
az cloud list --output table
```

## <a name="azure-classic-cli"></a>CLI de Azure clásica
Hay varias formas de [instalar la CLI clásica de Azure](../xplat-cli-install.md). Si Node ya está instalado, lo más sencillo es instalar el paquete npm.

Para instalar la CLI desde un paquete npm, asegúrese de que ha descargado e instalado la versión [más reciente de Node.js y npm](https://nodejs.org/en/download/package-manager/). A continuación, ejecute **instalar npm** para instalar el paquete azure-cli:

```bash
npm install -g azure-cli
```

En distribuciones de Linux, es posible que tenga que usar **sudo** para ejecutar correctamente el comando **npm**, tal como se muestra a continuación:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Si necesita instalar o actualizar Node.js y npm en la distribución de Linux o el SO, recomendamos instalar la versión LTS de Node.js más reciente (4.x). Si utiliza una versión anterior, podrían producirse errores de instalación.


Después de instalar la CLI de Azure, inicie sesión en Azure Alemania:

```
azure login --username your-user-name@your-tenant.onmicrosoft.de  --environment AzureGermanCloud
```

Después de que haya iniciado sesión, puede ejecutar los comandos de la CLI de Azure tal y como lo haría normalmente:

```
azure webapp list my-resource-group
```

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de cómo conectarse a Azure Alemania, consulte los siguientes recursos:

* [Conexión a Azure Alemania mediante PowerShell](./germany-get-started-connect-with-ps.md)
* [Conexión a Azure Alemania mediante Visual Studio](./germany-get-started-connect-with-vs.md)
* [Conexión a Azure Alemania mediante Azure Portal](./germany-get-started-connect-with-portal.md)




