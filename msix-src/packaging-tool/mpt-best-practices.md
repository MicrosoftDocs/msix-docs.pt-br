---
title: Melhores práticas da Ferramenta de Empacotamento MSIX
description: Este artigo aborda as melhores práticas para reempacotar seu aplicativo para MSIX e usar a Ferramenta de Empacotamento MSIX.
ms.date: 09/07/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 00c3831995383cea4b558c6353f4b545ae5b94ac
ms.sourcegitcommit: 0bd5ebc32feba8a4f4669f232129a8953d5cf773
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76022014"
---
# <a name="best-practices-for-the-msix-packaging-tool"></a>Melhores práticas da Ferramenta de Empacotamento MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Este artigo aborda as melhores práticas para reempacotar seu aplicativo para MSIX e usar a Ferramenta de Empacotamento MSIX.

## <a name="best-practices-for-environment-setup"></a>Práticas recomendadas para a configuração do ambiente
 
Verifique se você tem a [versão mais recente da ferramenta de empacotamento MSIX](mpt-overview.md#latest-public-version---1201912200). Para o processo de conversão, há algumas outras coisas que recomendamos que você considere antes de começar.

- O requisito mínimo de versão do sistema operacional para a ferramenta de empacotamento MSIX é o Windows 10 1809. Entendemos que nem todo mundo está usando a Atualização de outubro de 2018 para o Windows 10 ou nem mesmo o Windows 10. Portanto, é recomendável que você crie uma VM limpa pré-configurada para a versão mínima do suporte para MSIX.

- Uma máquina limpa para conversão é importante porque, durante a etapa de instalação da conversão usando a ferramenta de empacotamento MSIX, escutaremos tudo no computador para capturar o que o instalador está fazendo e ajudará a evitar dados estranhos em seu agrupa. Um computador limpo significa que não há aplicativos ou serviços estranhos em execução no seu computador. É recomendável configurar o computador de conversão para imitar o ambiente em que o pacote MSIX será executado, portanto, se houver serviços ou políticas que estarão lá, você poderá testar se o pacote realmente funcionará.

- Se não for algo que você tenha, oferecemos uma **VM de criação rápida**, o [ambiente da ferramenta de empacotamento MSIX](quick-create-vm.md) no Hyper-V, que está pronto para ser convertido com o Windows 10 1809 e a versão mais recente da ferramenta de empacotamento MSIX. 

- Siga as recomendações de práticas recomendadas para configurar a ferramenta de empacotamento MSIX e, em seguida, crie um ponto de verificação para a VM. Dessa forma, você pode usar a VM para converter, reverter para o ponto de verificação anterior e ela será uma máquina limpa e configurada pronta para conversão novamente ou para verificar se o pacote MSIX foi convertido com êxito.

- Também é bom saber que tipo de dependências você tem para que possa entender quais devem ser executadas com o seu aplicativo e quais devem ser empacotadas como um pacote de modificação. Por exemplo, se você tem dependências de runtime, é uma boa ideia incluí-las em seu aplicativo principal. Se você tiver um plug-in, deverá empacotá-lo como um pacote de modificação para associá-lo ao aplicativo principal. 

## <a name="best-practices-for-setting-up-the-msix-packaging-tool"></a>Práticas recomendadas para configurar a ferramenta de empacotamento MSIX

Antes de iniciar qualquer conversão, é recomendável definir suas configurações na ferramenta de empacotamento MSIX para simplificar o fluxo de trabalho a cada vez. 

### <a name="tool-defaults"></a>Padrões de ferramenta
- **Gerar uma linha de comando com cada pacote** Essa configuração fará com que você gere automaticamente um arquivo de linha de comando para que, se você estiver Reempacotando o mesmo aplicativo (como uma nova versão) por meio da linha de comando mais tarde, você pode ter um arquivo de linha de comando pré-configurado para esse aplicativo. 
- **Selecionar todas as correções por padrão para preparar computador** Essa configuração permite que você tenha todas as correções recomendadas previamente selecionadas para que, durante o estágio preparar computador, você possa simplesmente optar por desabilitar tudo sem precisar selecioná-las individualmente.
- **Impor requisitos de controle de versão Microsoft Store** Se você estiver planejando implantar seu aplicativo por meio do Microsoft Store, certifique-se de que isso esteja selecionado para que ele esteja em conformidade com os requisitos da loja (isso afetará os requisitos de versão do pacote e o suporte mínimo da versão do sistema operacional). Se essa opção estiver desmarcada, o pacote terá uma versão mínima definida como Windows 10 1709 e você terá controle total sobre os 4 dígitos da versão do pacote. Se esta opção estiver marcada, o pacote terá uma versão mínima definida para o Windows 10 1809 e a versão deverá terminar em. 0 (por exemplo, 1.5.6.0).
- **Adicionar integridade do pacote ao gerar um pacote** Se essa opção estiver selecionada, a integridade do pacote será adicionada automaticamente a todos os pacotes gerados. No entanto, ele só se aplicará a pacotes quando a integridade do pacote tiver suporte no sistema operacional no qual eles são implantados. 
- **Local de salvamento padrão** Especifique o local de salvamento padrão onde os pacotes gerados e os arquivos associados serão salvos.
- **Local de procura do instalador padrão** Especifique o local padrão para localizar instaladores a serem convertidos.
- **Número da porta do servidor** Especifique o número da porta do servidor para a ferramenta de empacotamento MSIX. Isso será relevante se você estiver planejando converter usando um [computador remoto](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup). 
- **Preferência de ambiente** Especifique o ambiente padrão para cada conversão.
- **Preferência de assinatura** Especifique a opção padrão para assinar quando você estiver convertendo aplicativos. É necessário assinar seu pacote MSIX para instalá-lo. Você pode escolher entre algumas opções para sua preferência de assinatura.
    - Assinar com assinatura do Device Guard-recomendamos essa opção se você não tiver um certificado confiável em sua empresa. Você pode saber mais sobre como configurar a assinatura do Device Guard aqui. 
    - Assinar com um certificado (. pfx)-recomendamos essa opção se você já tiver um certificado confiável que você está usando em sua empresa.
    - Especifique um arquivo. cer (não assina)-se você não deseja assinar no momento da conversão, mas quer garantir que as informações do Publicador sejam válidas no momento da assinatura, você pode escolher essa opção.
    - Não assinar pacote. -Se você quiser assinar seu pacote usando outro método ou posteriormente depois que o pacote tiver sido gerado, você poderá escolher essa opção.

Também recomendamos que você adicione uma URL do servidor de carimbo de data/hora à sua preferência siging (quando aplicável), para que seu aplicativo possa ser instalado, mesmo que seu certificado expire.
 
 ### <a name="other-settings"></a>Outras configurações
 - **Exlusions de arquivo e registro** Embora tenhamos um conjunto padrão de itens de exclusão, recomendamos dar uma olhada e adicionar ou remover quaisquer itens de exclusão para suas necessidades específicas. 
 - **Códigos de saída do instalador** Se você tiver códigos de saída do instalador específicos que deseja disparar uma reinicialização durante a conversão, poderá especificá-los aqui. Por padrão, temos as comuns já adicionadas, mas você pode removê-las se nunca quiser que as reinicializações sejam disparadas. Para observar, uma reinicialização nunca será disparada automaticamente pela ferramenta de empacotamento se você estiver usando a interface do usuário, mas será se você estiver usando a opção de linha de comando. 
 
 Você também pode importar ou exportar suas configurações para compartilhar ou garantir a conformidade entre os pares, usando estas [instruções](https://docs.microsoft.com/windows/msix/packaging-tool/duplicate-mpt-settings-across-devices). 

## <a name="best-practices-during-repackaging"></a>Melhores práticas durante o reempacotamento

Quando você está usando a ferramenta de empacotamento MSIX, há algumas coisas que também recomendamos fazer como prática recomendada durante o processo de reempacotamento:

- Ao empacotar os instaladores do ClickOnce, é necessário enviar um atalho para a área de trabalho caso o instalador ainda não esteja fazendo isso. Em geral, é uma boa prática sempre lembrar de enviar um atalho para a área de trabalho do executável do aplicativo principal.
- Ao criar pacotes de modificação, você precisa declarar o nome do pacote (nome da identidade) do aplicativo pai na interface do usuário da ferramenta para que a ferramenta defina a dependência de pacote correta no manifesto do pacote de modificação.
- Executar as etapas de preparação na página **preparar computador** é opcional, mas altamente recomendado, pois isso ajudará a reduzir os dados estranhos em seu pacote. 
- É necessário que você assine seu pacote para instalá-lo, mas também recomendamos que você faça o carimbo de data/hora do seu certificado para que seu aplicativo possa ser instalado, mesmo que seu certificado expire. 
- A declaração de um campo local de instalação na página **informações do pacote** é opcional. Certifique-se de que esse caminho corresponde ao local de instalação do instalador do aplicativo.

## <a name="best-practices-while-bundling-msix-packages"></a>Melhores práticas ao agrupar pacotes MSIX

É recomendável que você agrupe pacotes MSIX quando houver instaladores diferentes disponíveis para arquiteturas de processador diferentes, como x86, x64 e ARM para o seu aplicativo. Ao agrupar os instaladores, você pode permitir que o sistema de operacional Windows 10 determine a aplicabilidade do instalador apropriado em vez de o usuário precisar fazer a seleção correta. 

Ao [Agrupar pacotes MSIX](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages), você precisará já ter convertido os instaladores do Win32 em pacotes do MSIX. 

- A Ferramenta de Empacotamento MSIX assumirá a arquitetura do processador da versão do sistema operacional do Windows 10 em que a conversão está ocorrendo. Por exemplo, se você estiver usando uma versão do x64 do Windows 10 e estiver convertendo uma versão x86 de um instalador, o pacote MSIX resultante será x64. 
- Se você planeja agrupar pacotes MSIX, precisa converter os instaladores no mesmo ambiente em que espera implantá-los. Dessa forma, a Ferramenta de Empacotamento MSIX criará o pacote MSIX apropriado para esse ambiente. 

## <a name="best-practices-for-testing-your-msix-package"></a>Práticas recomendadas para testar seu pacote MSIX

É recomendável que você também teste seu pacote MSIX após a conversão em um ambiente limpo, conforme especificado acima durante a configuração do ambiente. Você deve testar seu pacote MSIX em um computador diferente que não instalou o instalador anterior nele, para que você possa garantir que, ao implantar o pacote MSIX, ele tenha todos os componentes de que precisa e não esteja selecionando nada do instalador anterior. Isso pode ser obtido por meio de uma nova VM, como a VM de criação rápida, ou revertendo sua máquina de conversão se você seguiu nossa recomendação de ponto de verificação acima.

