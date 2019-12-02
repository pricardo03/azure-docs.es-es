---
author: paulbouwer
ms.service: container-service
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: a4090172e926f21db01a8e374d8c4bb1c80402c7
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74197377"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Descarga e instalación del binario de cliente linkerd de Linkerd

En un shell basado en Bash en MacOS, use `curl` para descargar la versión de Linkerd como se indica a continuación:

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-darwin"
```

El binario de cliente `linkerd` se ejecuta en la máquina cliente y permite interactuar con la malla de servicio de Linkerd. Use los siguientes comandos para instalar el binario de cliente `linkerd` de Linkerd en un shell basado en Bash en MacOS. Estos comandos copian el binario de cliente `linkerd` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-darwin /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

Si desea completar la línea de comandos del binario de cliente `linkerd` de Linkerd, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && linkerd completion bash > ~/completions/linkerd.bash
source ~/completions/linkerd.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/linkerd.bash" >> ~/.bashrc
```
