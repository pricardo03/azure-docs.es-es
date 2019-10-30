---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 06479b4396ab26c280cc6246d774bc30b5ea1c76
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529832"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Descarga e instalación del binario de cliente istioctl de Istio

En un shell basado en Bash de MacOS, utilice `curl` para descargar la versión de Istio y, a continuación, realice la extracción con `tar`, tal como se indica a continuación:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.3.2

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

El binario de cliente `istioctl` se ejecuta en su equipo cliente y le permite interactuar con la malla de servicio de Istio. Use los siguientes comandos para instalar el binario de cliente `istioctl` de Istio en un shell basado en Bash en MacOS. Estos comandos copian el binario de cliente `istioctl` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Si desea completar la línea de comandos del binario de cliente `istioctl` de Istio, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```