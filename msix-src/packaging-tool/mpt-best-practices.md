---
title: Melhores práticas da Ferramenta de Empacotamento MSIX
description: Este artigo aborda as melhores práticas para reempacotar seu aplicativo para MSIX e usar a Ferramenta de Empacotamento MSIX.
ms.date: 09/07/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 975ed03b8c3a0e436dff9dac8469e24556557b3c
ms.sourcegitcommit: 073a228653f004914851c3461b9ad6eef343f915
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74309017"
---
# <a name="best-practices-for-the-msix-packaging-tool"></a>Melhores práticas da Ferramenta de Empacotamento MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Este artigo aborda as melhores práticas para reempacotar seu aplicativo para MSIX e usar a Ferramenta de Empacotamento MSIX.

## <a name="best-practices-during-setup"></a>Melhores práticas durante a configuração
 
Para começar, verifique se você tem a [versão mais recente da ferramenta de empacotamento MSIX](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/mpt-overview#latest-public-version---1201910180). Para o processo de conversão, há algumas outras coisas que recomendamos que você considere antes de começar. 

- O requisito mínimo de versão do sistema operacional para a ferramenta de empacotamento MSIX é o Windows 10 1809. Entendemos que nem todo mundo está usando a Atualização de outubro de 2018 para o Windows 10 ou nem mesmo o Windows 10. Portanto, é recomendável que você crie uma VM limpa pré-configurada para a versão mínima do suporte para MSIX. Se não for algo que você também oferece uma VM de criação rápida, o [ambiente da ferramenta de empacotamento MSIX](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/quick-create-vm) no Hyper-V, que está pronto para ser convertido. 

- Outro motivo pelo qual isso é uma recomendação é que durante a conversão de GUI interativa usando a Ferramenta de Empacotamento MSIX, escutaremos tudo no dispositivo e isso ajudará a evitar dados irrelevantes no seu pacote. 

- Também é bom saber que tipo de dependências você tem para que possa entender quais devem ser executadas com o seu aplicativo e quais devem ser empacotadas como um pacote de modificação. Por exemplo, se você tem dependências de runtime, é uma boa ideia incluí-las em seu aplicativo principal. Se você tiver um plug-in, deverá empacotá-lo como um pacote de modificação para associá-lo ao aplicativo principal. 


## <a name="best-practices-during-repackaging"></a>Melhores práticas durante o reempacotamento 
Quando você está usando a Ferramenta de Empacotamento MSIX, há algumas melhores práticas que também recomendamos que você faça:
- Ao empacotar os instaladores do ClickOnce, é necessário enviar um atalho para a área de trabalho caso o instalador ainda não esteja fazendo isso. Em geral, é uma boa prática sempre lembrar de enviar um atalho para a área de trabalho do executável do aplicativo principal.
- Ao criar pacotes de modificação, você precisa declarar o nome do pacote (nome da identidade) do aplicativo pai na interface do usuário da ferramenta para que a ferramenta defina a dependência de pacote correta no manifesto do pacote de modificação.
- Executar as etapas de preparação na página **preparar computador** é opcional, mas altamente recomendado, pois isso ajudará a reduzir os dados estranhos em seu pacote. 
- É necessário que você assine seu pacote para instalá-lo, mas também recomendamos que você faça o carimbo de data/hora do seu certificado para que seu aplicativo possa ser instalado, mesmo que seu certificado expire. 
- A declaração de um campo local de instalação na página **informações do pacote** é opcional. Certifique-se de que esse caminho corresponde ao local de instalação do instalador do aplicativo.
MS. Custom: RS5


## <a name="best-practices-while-bundling-msix-packages"></a>Melhores práticas ao agrupar pacotes MSIX

É recomendável que você agrupe pacotes MSIX quando houver instaladores diferentes disponíveis para arquiteturas de processador diferentes, como x86, x64 e ARM para o seu aplicativo. Ao agrupar os instaladores, você pode permitir que o sistema de operacional Windows 10 determine a aplicabilidade do instalador apropriado em vez de o usuário precisar fazer a seleção correta. 

Ao agrupar pacotes MSIX, você precisará já ter convertido os instaladores Win32 em pacotes MSIX. 

- A Ferramenta de Empacotamento MSIX assumirá a arquitetura do processador da versão do sistema operacional do Windows 10 em que a conversão está ocorrendo. Por exemplo, se você estiver usando uma versão do x64 do Windows 10 e estiver convertendo uma versão x86 de um instalador, o pacote MSIX resultante será x64. 
- Se você planeja agrupar pacotes MSIX, precisa converter os instaladores no mesmo ambiente em que espera implantá-los. Dessa forma, a Ferramenta de Empacotamento MSIX criará o pacote MSIX apropriado para esse ambiente. 



