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
ms.openlocfilehash: 59f38d2f23470824687a739eb87a808baac29562
ms.sourcegitcommit: fe59d6b39d81eb4a155887fa8fe9a08b6fe48584
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65488519"
---
# <a name="best-practices-for-msix-packaging-tool"></a>Práticas recomendadas da Ferramenta de Empacotamento MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

Este artigo aborda as práticas recomendadas para seu aplicativo para MSIX de reempacotamento e usando a ferramenta de empacotamento MSIX.

## <a name="best-practices-during-setup"></a>Práticas recomendadas durante a instalação
 
Para começar, você deve ter a versão da ferramenta de empacotamento MSIX para os Windows (também conhecido como a versão 1809) de atualização de 10 de outubro de 2018. Para o processo de conversão, há algumas outras coisas, recomendamos que você considerar antes de começar. 

- Entendemos que nem todo mundo está no Windows 10 atualização de 2018 ou até mesmo o Windows 10 de outubro. Portanto, é recomendável que você crie uma VM limpa que é pré-configurado para a versão mínima do suporte para MSIX. 

- Outro motivo que isso é uma recomendação é que, durante a conversão de GUI interativa, usando a ferramenta de empacotamento MSIX, podemos estarão escutando a tudo no dispositivo e ele ajudará a impedir que os dados estranhos em seu pacote. 

- Seu também bom saber que tipo de dependências, você tem para que você possa entender quais você deve ser executado com o seu aplicativo e qual deve ser empacotado como um pacote de modificação. Por exemplo, se você tiver dependências de tempo de execução, é uma boa ideia para incluí-los em seu aplicativo principal. Se você tiver um plug-in, você deve empacotar que como um pacote de modificação associado. 


## <a name="best-practices-during-repackaging"></a>Práticas recomendadas durante o reempacotamento 
Quando você estiver usando a ferramenta de empacotamento MSIX, há algumas coisas que também recomendamos que você como práticas recomendadas:
- Ao empacotar os instaladores do ClickOnce, é necessário enviar um atalho para a área de trabalho se o instalador não está fazendo isso já. Em geral, é recomendável sempre Lembre-se de enviar um atalho para a área de trabalho para o aplicativo principal executável.
- Ao criar pacotes de modificação, você precisa declarar o pacote de nome (identidade) do pai aplicativo na ferramenta de interface do usuário para que a ferramenta define a dependência do pacote correto no manifesto do pacote de modificação.
- Declarar um campo de local de instalação nos **informações de pacote** página é opcional mas recomendado. Certifique-se de que esse caminho corresponde o local de instalação do instalador do aplicativo.
- Executar a preparação as etapas na **preparar computador** página é opcional, mas altamente recomendado.
ms.custom: RS5


## <a name="best-practices-while-bundling-msix-packages"></a>Práticas recomendadas ao agrupamento de pacotes MSIX

É recomendável que você agrupar pacotes MSIX quando há instaladores diferentes disponíveis para arquiteturas de processador diferente, como x86, x64 e ARM para o seu aplicativo. Reunindo os instaladores juntos, você pode permitir que o sistema de operacional Windows 10 determinar a aplicabilidade do instalador apropriado em vez do usuário precise fazer a seleção correta. 

Ao agrupar pacotes MSIX, você precisa já ter convertido instaladores Win32 em pacotes MSIX. 

- A ferramenta de empacotamento MSIX assumirá que a arquitetura do processador da versão do sistema operacional do Windows 10 que a conversão está ocorrendo. Por exemplo, se você estiver usando uma versão do Windows 10 e você estiver convertendo uma versão de um instaladores, o pacote MSIX resultante será de x86 de x64 ser x64. 
- Se você planeja MSIX pacotes, você precisará converter seus instaladores no mesmo ambiente onde você pretende implantá-los. Dessa forma, a ferramenta de empacotamento MSIX criará o pacote MSIX apropriado para esse ambiente. 



