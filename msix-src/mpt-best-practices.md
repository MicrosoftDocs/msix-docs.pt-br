---
title: Práticas recomendadas para a ferramenta de empacotamento MSIX
description: Práticas recomendadas para a ferramenta de empacotamento MSIX
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 419ea7ab48d9199629570d58efc2bc5ff85ebc05
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900428"
---
# <a name="best-practices-for-msix-packaging-tool"></a>Práticas recomendadas para a ferramenta de empacotamento MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

Este artigo aborda as práticas recomendadas para seu aplicativo para MSIX de reempacotamento e usando a ferramenta de empacotamento MSIX.

## <a name="best-practices-during-setup"></a>Práticas recomendadas durante a instalação
 
Para começar, você deve ter a versão da ferramenta de empacotamento MSIX para os Windows (também conhecido como a versão 1809) de atualização de 10 de outubro de 2018. Para o processo de conversão, há algumas outras coisas, recomendamos que você considerar antes de começar. 

- Entendemos que nem todo mundo está no Windows 10 atualização de 2018 ou até mesmo o Windows 10 de outubro. Portanto, é recomendável que você crie uma VM limpa que é pré-configurado para a versão mínima do suporte para MSIX. 

- Outro motivo que isso é uma recomendação é que, durante a conversão de GUI interativa, usando a ferramenta de empacotamento MSIX, podemos estarão escutando a tudo no dispositivo e ele ajudará a impedir que os dados estranhos em seu pacote. 

- Seu também bom saber que tipo de dependências, você tem para que você possa entender quais você deve ser executado com o seu aplicativo e qual deve ser empacotado como um pacote de modificação. Por exemplo, se você tiver dependências de tempo de execução, é uma boa ideia para incluí-los em seu aplicativo principal. Se você tiver um plug-in, você deve empacotar que como um pacote de modificação associado. 

---
## Best practices during repackaging 
'When you are using the MSIX Packaging Tool, there are a few things that we also recommend you do as best practice':
  - 'When packaging ClickOnce installers, it is necessary to send a shortcut to desktop if the installer is not doing so already. In general, it is good practice to always remember to send a shortcut to desktop for the main app executable.'
  - 'When creating modification packages, you need to declare the package Name (identity name) of the parent application in the tool UI so that the tool sets the correct package dependency in the manifest of the modification package.'
  - Declaring an installation location field in the **Package information** page is optional but recommended. Make sure that this path matches the installation location of application installer.
  - Performing the preparation steps in the **Prepare computer** page is optional but highly recommended.
ms.custom: RS5
---

## <a name="best-practices-while-bundling-msix-packages"></a>Práticas recomendadas ao agrupamento de pacotes MSIX

É recomendável que você agrupar pacotes MSIX quando há instaladores diferentes disponíveis para arquiteturas de processador diferente, como x86, x64 e ARM para o seu aplicativo. Reunindo os instaladores juntos, você pode permitir que o sistema de operacional Windows 10 determinar a aplicabilidade do instalador apropriado em vez do usuário precise fazer a seleção correta. 

Ao agrupar pacotes MSIX, você precisa já ter convertido instaladores Win32 em pacotes MSIX. 

- A ferramenta de empacotamento MSIX assumirá que a arquitetura do processador da versão do sistema operacional do Windows 10 que a conversão está ocorrendo. Por exemplo, se você estiver usando uma versão do Windows 10 e você estiver convertendo uma versão de um instaladores, o pacote MSIX resultante será de x86 de x64 ser x64. 
- Se você planeja MSIX pacotes, você precisará converter seus instaladores no mesmo ambiente onde você pretende implantá-los. Dessa forma, a ferramenta de empacotamento MSIX criará o pacote MSIX apropriado para esse ambiente. 



