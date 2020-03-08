---
title: Melhores práticas da Ferramenta de Empacotamento MSIX
description: Este artigo aborda as melhores práticas para reempacotar seu aplicativo para MSIX e usar a Ferramenta de Empacotamento MSIX.
ms.date: 09/07/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a22f4cbc2f96746fea48cb1bca1199e6006f938b
ms.sourcegitcommit: 536d6969cde057877ecdd8345cfb0dc12c9582f2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78909608"
---
# <a name="best-practices-for-the-msix-packaging-tool"></a>Melhores práticas da Ferramenta de Empacotamento MSIX

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Se você ainda não configurou seu ambiente para conversão, você pode seguir nossas recomendações de [práticas recomendadas de ambiente](prepare-your-environment.md) e, em seguida, voltar aqui para configurar a ferramenta de empacotamento MSIX. Antes de iniciar qualquer conversão, é recomendável definir suas configurações na ferramenta de empacotamento MSIX para simplificar o fluxo de trabalho a cada vez.

### <a name="tool-defaults"></a>Padrões de ferramenta

- **Gerar uma linha de comando com cada pacote** Essa configuração fará com que você gere automaticamente um arquivo de modelo de linha de comando para que, se você estiver Reempacotando o mesmo aplicativo (como uma nova versão) por meio da linha de comando mais tarde, você pode ter um arquivo de modelo de linha de comando pré-configurado para esse aplicativo. Você precisará fornecer um instalador para gerar um arquivo de modelo durante o fluxo de trabalho.
- **Selecionar todas as correções por padrão para preparar computador** Essa configuração permite que você tenha todas as correções recomendadas previamente selecionadas para que, durante o estágio preparar computador, você possa simplesmente optar por desabilitar tudo sem precisar selecioná-las individualmente.
- **Impor requisitos de controle de versão Microsoft Store** Se você estiver planejando implantar seu aplicativo por meio do Microsoft Store, certifique-se de que isso esteja selecionado para que ele esteja em conformidade com os requisitos da loja (isso afetará os requisitos de versão do pacote e o suporte mínimo da versão do sistema operacional). Se essa opção estiver desmarcada, o pacote terá uma versão mínima definida como Windows 10 1709 e você terá controle total sobre os 4 dígitos da versão do pacote. Se esta opção estiver marcada, o pacote terá uma versão mínima definida para o Windows 10 1809 e a versão deverá terminar em. 0 (por exemplo, 1.5.6.0).
- **Adicionar integridade do pacote ao gerar um pacote** Se essa opção estiver selecionada, a integridade do pacote será adicionada automaticamente a todos os pacotes gerados. A [integridade do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity) tem suporte no Windows 10 2004 e posterior.
- **Local de salvamento padrão** Especifique o local de salvamento padrão onde os pacotes gerados e os arquivos associados serão salvos.
- **Local de procura do instalador padrão** Especifique o local padrão para localizar instaladores a serem convertidos.
- **Número da porta do servidor** Especifique o número da porta do servidor para a ferramenta de empacotamento MSIX. Isso será relevante se você estiver planejando converter usando um [computador remoto](remote-conversion-setup.md). 
- **Preferência de ambiente** Especifique o ambiente padrão para cada conversão.
- **Preferência de assinatura** Especifique a opção padrão para assinar quando você estiver convertendo aplicativos. É necessário assinar seu pacote MSIX para instalá-lo. Você pode escolher entre algumas opções para sua preferência de assinatura.
    - Assinar com assinatura do Device Guard-recomendamos essa opção se você não tiver um certificado confiável em sua empresa. Há algumas etapas para habilitar a [assinatura do Device Guard](../package/signing-package-device-guard-signing.md) que você precisa executar antes de escolher essa opção. 
    - Assinar com um certificado (. pfx)-recomendamos essa opção se você já tiver um certificado confiável que você está usando em sua empresa.
    - Especifique um arquivo. cer (não assina)-se você não deseja assinar no momento da conversão, mas quer garantir que as informações do Publicador sejam válidas no momento da assinatura, você pode escolher essa opção.
    - Não assinar pacote. -Se você quiser assinar seu pacote usando outro método ou posteriormente depois que o pacote tiver sido gerado, você poderá escolher essa opção.
    Também recomendamos que você adicione uma URL do servidor de carimbo de data/hora à sua preferência de assinatura (quando aplicável), para que seu aplicativo possa ser instalado, mesmo que seu certificado expire.

### <a name="other-settings"></a>Outras configurações

- **Exclusões de arquivo e registro** Embora tenhamos um conjunto padrão de itens de exclusão, recomendamos dar uma olhada e adicionar ou remover quaisquer itens de exclusão para suas necessidades específicas. 
- **Códigos de saída do instalador** Se você tiver códigos de saída do instalador específicos que deseja disparar uma reinicialização durante a conversão, poderá especificá-los aqui. Por padrão, temos as comuns já adicionadas, mas você pode removê-las se nunca quiser que as reinicializações sejam disparadas. Para observar, uma reinicialização nunca será disparada automaticamente pela ferramenta de empacotamento se você estiver usando a interface do usuário, mas será se você estiver usando a opção de linha de comando. 
 
Você também pode importar ou exportar suas configurações para compartilhamento usando estas [instruções](duplicate-tool-settings-across-devices.md). 

## <a name="best-practices-during-repackaging"></a>Melhores práticas durante o reempacotamento

Quando você está usando a ferramenta de empacotamento MSIX, há algumas coisas que também recomendamos fazer como prática recomendada durante o processo de reempacotamento:

- Ao empacotar os instaladores do ClickOnce, é necessário enviar um atalho para a área de trabalho caso o instalador ainda não esteja fazendo isso. Em geral, é uma boa prática sempre lembrar de enviar um atalho para a área de trabalho do executável do aplicativo principal.
- Ao criar pacotes de modificação, você precisa declarar o nome do pacote (nome da identidade) do aplicativo pai na interface do usuário da ferramenta para que a ferramenta defina a dependência de pacote correta no manifesto do pacote de modificação.
- Executar as etapas de preparação na página **preparar computador** é opcional, mas altamente recomendado, pois isso ajudará a reduzir os dados estranhos em seu pacote.
- É necessário que você assine seu pacote para instalá-lo, mas também recomendamos que você faça o carimbo de data/hora do seu certificado para que seu aplicativo possa ser instalado, mesmo que seu certificado expire.
- A declaração de um campo local de instalação na página **informações do pacote** é opcional. Certifique-se de que esse caminho corresponde ao local de instalação do instalador do aplicativo.

## <a name="best-practices-for-testing-your-msix-package"></a>Práticas recomendadas para testar seu pacote MSIX

É recomendável que você também teste seu pacote MSIX após a conversão em um ambiente limpo, conforme especificado durante a configuração do ambiente. Você deve testar seu pacote MSIX em um computador diferente que não instalou o instalador anterior nele, para que você possa garantir que, ao implantar o pacote MSIX, ele tenha todos os componentes de que precisa e não esteja selecionando nada do instalador anterior. Isso pode ser obtido por meio de uma nova máquina virtual, como a [VM de criação rápida](Quick-Create-VM.md), ou revertendo seu computador de conversão se você tiver criado um ponto de verificação antes de iniciar a conversão.
