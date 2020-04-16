---
title: Criar um pacote MSIX de qualquer instalador de área de trabalho
description: Criar um pacote MSIX de qualquer instalador de desktop (MSI, EXE, ClickOnce ou App-V)
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 034abce3f240275e2c4be87b62e6fe9e8e6345ee
ms.sourcegitcommit: 45bb7e2f642a0c7165366bc0867afe803abfc202
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81433762"
---
# <a name="create-an-msix-package-from-any-desktop-installer-msi-exe-clickonce-or-app-v"></a>Criar um pacote MSIX de qualquer instalador de desktop (MSI, EXE, ClickOnce ou App-V)

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Você pode usar a [ferramenta de empacotamento MSIX](tool-overview.md) para criar um pacote de aplicativo do MSIX de qualquer uma das seguintes opções:

- MSI
- EXE
- ClickOnce
- App-V
- Script
- Instalação manual

Este documento explicará como obter quaisquer ativos existentes que você tenha e convertê-los em MSIX.

Antes de iniciar a conversão, recomendamos garantir que você [compreenda o instalador](know-your-installer.md)e se ele será convertido.

Também recomendamos seguir as práticas recomendadas para [configurar seu ambiente](prepare-your-environment.md) e a [ferramenta de empacotamento MSIX](tool-best-practices.md) para conversão.

> [!NOTE]
> No momento, a Ferramenta de Empacotamento MSIX é compatível com o App-V 5.1. Se você tiver um pacote com o App-V 4. x, é recomendável usar o instalador de origem para converter em MSIX.

Quando a ferramenta for iniciada pela primeira vez, você deverá fornecer consentimento para enviar dados telemétricos. É importante observar que os dados de diagnóstico que você compartilha são provenientes apenas do aplicativo e nunca são usados para identificá-lo ou contatá-lo.

A criação de um **pacote de aplicativos** é a opção mais comumente usada. É aqui que você criará um pacote MSIX a partir de um instalador ou por meio da instalação manual da carga do aplicativo.

![pic1](images/pic1.PNG)

## <a name="packaging-method"></a>Método de compactação

Selecione uma opção no computador de conversão:

- Se você já estiver trabalhando em um ambiente limpo, selecione **criar pacote neste computador**
- Se você quiser se conectar a um computador virtual ou remoto existente, selecione **criar pacote em um computador remoto**
  - Você precisará [configurar seu computador remoto](remote-conversion-setup.md) antes de poder convertê-lo
- Se você tiver uma máquina virtual local no computador que deseja converter, selecione **criar pacote em uma máquina virtual local**
  - Observe que só há suporte para máquinas virtuais do Hyper-V, se você quiser usar outro produto de virtualização, poderá se conectar usando a opção de máquina remota.

- Clique em **Avançar**.
  
## <a name="prepare-computer"></a>Preparar o computador

Em seguida, a página **Preparar computador** fornece opções para preparar o computador para empacotamento.

O **Driver da ferramenta de empacotamento MSIX** é necessário e a ferramenta tentará habilitá-la automaticamente se não estiver habilitada. A ferramenta verificará primeiro com o DISM para ver se o driver está instalado. Se você se deparar com um problema, tente verificar nossa [documentação de solução de problemas](tool-known-issues.md)e, em seguida, o problema de um hub de [comentários](tool-known-issues.md#sending-feedback) será reparado.

> [!NOTE]
> O Driver da Ferramenta de Empacotamento MSIX monitora o sistema para capturar as alterações que um instalador faz no sistema, que permite à Ferramenta de Empacotamento MSIX criar um pacote com base nessas alterações.

O **Windows Update está ativo** Desativaremos temporariamente Windows Update durante o empacotamento para que não coletemos dados estranhos.

- A caixa de seleção **Reinicialização pendente** está desabilitada por padrão. Você precisará reiniciar o computador manualmente e, em seguida, iniciar a ferramenta novamente caso receba um prompt informando que as operações pendentes precisam de reinicialização. Isso não é obrigatório, somente recomendado.

- [Opcional] Marque a caixa **O Windows Search está Ativo** e selecione **Desabilitar selecionado** se você optar por desabilitar o serviço de pesquisa.
    - Isso não é obrigatório, somente recomendado.
    - Quando essa opção estiver desabilitada, a ferramenta atualizará o campo de status para **Desabilitado**.

- Adicional Marque a caixa o **host do SMS está ativa** e selecione **desabilitar selecionado** se você optar por desabilitar o serviço de host.
    - Isso não é obrigatório, somente recomendado.
    - Quando essa opção estiver desabilitada, a ferramenta atualizará o campo de status para **Desabilitado**.

Quando terminar de preparar o computador, clique em **Avançar**.

## <a name="choose-the-installer-you-want-to-package"></a>Escolher o instalador que você deseja empacotar

A primeira coisa que você vai querer fazer é entender o que acontecerá com o instalador que você deseja converter. Com qualquer um desses instaladores, você pode especificá-los aqui para simplificar o fluxo de trabalho ou pode executá-lo manualmente no momento da instalação mais tarde no fluxo de trabalho.

### <a name="msi-installers"></a>Instaladores MSI

Se você estiver convertendo um instalador. msi, poderá simplesmente procurá-lo e especificar o. msi. Se você tiver um arquivo. MST ou. msp fornecido, poderá especificá-lo no campo argumentos do instalador. Um dos benefícios de especificar seu. msi aqui é que podemos extrair todas as informações do pacote, economizando seu tempo na próxima etapa da conversão.

### <a name="app-v-installers"></a>Instaladores do App-V

Se você estiver convertendo usando um App-V, esse é um processo realmente simples para você. Tudo o que você precisa fazer é especificar um arquivo App-V e obter rastreamento rápido para a página criar uma MSIX. Isso ocorre porque o manifesto do pacote simplesmente precisa ser traduzido para um pacote MSIX e, em seguida, funciona como um MSIX. A limitação aqui é que a ferramenta só dá suporte ao App-V 5,1-se a sua App-V for a versão 4. x, recomendamos que você use o instalador de origem e, em seguida, converta-o diretamente em MSIX.

### <a name="exe-installers"></a>Instaladores EXE

Se você estiver convertendo um instalador. exe, poderá especificar o instalador neste ponto. Devido à falta de consistência de formato com um exe, você precisará inserir manualmente as informações do pacote para o instalador. 

### <a name="clickonce-installers"></a>Instaladores do ClickOnce

Se você estiver convertendo um instalador do ClickOnce, poderá especificar o instalador neste ponto. Como um. exe, você precisará inserir manualmente as informações do pacote para o instalador. 

### <a name="scripts"></a>Scripts
Se você estiver usando um script para instalar o aplicativo, poderá especificar a linha de comando aqui. Como alternativa, você pode deixar esse campo em branco e executar o script manualmente durante a [fase de instalação](#installation).

### <a name="manual-installation"></a>Instalação manual
Se você quiser executar o instalador manualmente ou executar as ações do instalador manualmente, poderá deixar o campo do instalador em branco e, durante a fase de [instalação](#installation), executar as ações necessárias para o instalador. 

Se você estiver tentando gerar um arquivo de modelo de conversão, não poderá fazer isso sem especificar um instalador.

Se você tiver argumentos de instalador, poderá inserir o argumento desejado no campo fornecido. Esse campo aceita qualquer cadeia de caracteres.

### <a name="signing-preference"></a>Preferência de assinatura 

Em **preferência de assinatura**, selecione uma opção de assinatura. Também é possível definir como padrão nas configurações. Desse modo, você pula algumas etapas na hora da conversão.

- **Assinar com assinatura do Device Guard** Essa opção permite que você entre em sua conta do Microsoft Active Directory que você configurou para usar com a assinatura do Device Guard, que é um serviço de assinatura que a Microsoft fornece onde você não precisa fornecer seu próprio certificado. Saiba mais sobre como configurar sua conta e sobre a assinatura do Device Guard [aqui](../package/signing-package-device-guard-signing.md). 
- **Assinar com um certificado (. pfx)** Navegue até e selecione seu arquivo de certificado. pfx. Se o certificado for protegido por senha, digite a senha na caixa de senha.
- **Especificar um arquivo. cer (não assina)** Essa opção permite que você especifique um arquivo. cer. Isso é útil quando você não deseja assinar o pacote, mas deseja garantir que as informações do Publicador correspondam ao assunto do certificado que será usado para assinatura. 
- Não **assinar pacote** Selecione esta opção se você estiver assinando seu pacote posteriormente. Observação: não é possível instalar um pacote MSIX se ele não estiver assinado
- Ao assinar, é altamente recomendável adicionar um **carimbo de data/hora** ao seu certificado para que a validade do seu certificado possa Outlast sua data de expiração. O formato aceito é uma [URL do servidor com carimbo de data/hora RFC 3161](https://docs.microsoft.com/windows/win32/seccrypto/signtool).

> [!NOTE]
> Não há suporte para a assinatura de um aplicativo de formato de pacote MSIX com um certificado SHA1.

Clique em **Próximo** para continuar.

## <a name="package-information"></a>Informações do pacote

Depois de optar por empacotar o aplicativo em uma máquina virtual existente, você precisará fornecer informações sobre o aplicativo. A ferramenta tentará preencher automaticamente esses campos com base nas informações disponíveis no instalador. Você sempre terá uma opção para atualizar as entradas, conforme necessário. Se o campo tiver um asterisco *, ele será obrigatório. A ajuda embutida será fornecida se a entrada não for válida.

- Nome do pacote:
    - Obrigatório; corresponde ao Nome de identidade do pacote no manifesto para descrever o conteúdo do pacote.
    - Não é mostrado ao usuário final.
    - Diferencia maiúsculas de minúsculas e não pode ter um espaço.
    - Pode aceitar uma cadeia de caracteres entre 3 e 50 caracteres que consiste em caracteres alfanuméricos, ponto e traço.
    - Não é possível terminar com um ponto e ser um destes: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8" e "LPT9".
- Nome de exibição do pacote:
    - Obrigatório; corresponde ao pacote no manifesto para exibir um nome de pacote amigável para o usuário no menu Iniciar e nas páginas de configurações.
    - O campo aceita uma cadeia de caracteres entre 1 e 256 caracteres e é localizável.
- Nome do fornecedor:
    - Obrigatório; corresponde ao pacote que descreve as informações do fornecedor.
    - O atributo Publisher precisa corresponder às informações da entidade do fornecedor do certificado usado para assinar um pacote.
    - Este campo aceita uma cadeia de caracteres entre 1 e 8.192 caracteres que ajusta a expressão regular de um nome diferenciado: "(CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")(, ((CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")))*".
- Nome de exibição do fornecedor:
    - Obrigatório; corresponde ao pacote no manifesto para exibir um nome de fornecedor amigável para o usuário no instalador do Aplicativo e nas páginas de configurações.
    - O campo aceita uma cadeia de caracteres entre 1 e 256 caracteres e é localizável.
- Versão:
    - Obrigatório; corresponde ao pacote no manifesto para descrever o número de versão do pacote.
    - Este campo aceita uma cadeia de caracteres de versão na notação quádrupla: "Major. Minor. Build. Revision".
- Descrição:
    - Este campo é opcional.
- Local de instalação:
    - Esse é o local em que o instalador copiará o conteúdo do aplicativo (normalmente, a pasta Arquivos de Programas).
    - Esse campo é opcional, mas recomendado quando a carga do aplicativo está sendo instalada fora das pastas de arquivos de programas.
    - Procure e selecione um caminho de pasta.
    - Verifique se esse arquivo corresponde ao local de instalação do instalador enquanto você executa a operação de instalação do aplicativo. 

## <a name="installation"></a>Instalação

- Esta é a fase de instalação em que a ferramenta está monitorando e capturando as operações de instalação do aplicativo.
- A ferramenta iniciará o instalador no ambiente especificado anteriormente e você precisará passar pelo assistente de instalação para instalar o aplicativo.
    - Verifique se o caminho de instalação corresponde ao que foi definido anteriormente na página de informações do pacote.
    - Talvez seja necessário criar um atalho na área de trabalho para o aplicativo recentemente instalado.
    - Depois de concluir o assistente de instalação do aplicativo, conclua ou feche o assistente de instalação.
    - Caso precise executar vários instaladores, faça isso manualmente neste momento.
    - Se o aplicativo precisar de outros pré-requisitos, você precisará instalá-los agora.
    - Se o aplicativo precisar do .Net 3.5/20, adicione o recurso opcional ao Windows.
- Se você não especificou um instalador anteriormente, aqui é onde você pode executar manualmente o instalador ou o script.
- Se o [instalador exigir uma](support-restart.md)reinicialização, você poderá executar uma reinicialização manual ou usar o botão ' reiniciar ' para executar a reinicialização e retornará a esse ponto no processo de conversão após a reinicialização.
- Quando concluir a instalação do aplicativo, clique em **Avançar**.

## <a name="manage-first-launch-tasks"></a>Gerenciar as primeiras tarefas de inicialização

Essa página mostra os executáveis do aplicativo capturados pela ferramenta. Recomendamos iniciar o aplicativo pelo menos uma vez para capturar as primeiras tarefas de inicialização.

Você pode iniciar o executável selecionando-o e, em seguida, clicando em **executar**. Você também pode remover qualquer ponto de entrada desnecessário selecionando-o e clicando em **remover**.

Se houver vários aplicativos, marque a caixa que corresponde ao ponto de entrada principal. Se o aplicativo .exe não for exibido aqui, procure-o manualmente e execute-o. Em seguida, **atualize a lista**.

Clique em **Avançar** e você verá um pop-up que solicitará a confirmação de que você concluiu a instalação do aplicativo e o gerenciamento das primeiras tarefas de inicialização.

- Se você tiver concluído, clique em **Sim, continuar**.
- Se você não tiver concluído, clique em **Não, não terminei**. Você será levado novamente à última página, em que pode iniciar aplicativos, instalar ou copiar outros arquivos e DLLs/executáveis.

## <a name="services-report"></a>Relatório de serviços

A partir da versão 1.2019.1220.0 da ferramenta de empacotamento MSIX, você pode converter um [instalador com serviços](convert-an-installer-with-services.md)e, portanto, adicionamos uma página de relatório de serviços. Se nenhum serviço for detectado, você ainda verá essa página, mas ela estará vazia com uma mensagem informando que nenhum serviço foi detectado na parte superior da página. 

A página de relatório de serviços lista os serviços que foram detectados no instalador durante a conversão. Os serviços que têm todas as informações de que precisam e que têm suporte serão mostrados na tabela **incluída** . Os serviços que precisam de informações adicionais, precisam de uma correção ou não têm suporte serão mostrados na tabela **excluída** .

Para corrigir um serviço ou ver dados adicionais sobre o serviço, clique duas vezes na entrada de serviço na tabela para exibir um pop-up com mais informações sobre o serviço. Você pode editar algumas dessas informações se precisar.

- **Nome da chave:** O nome do serviço. Isso não é editável.
- **Descrição:** A descrição da entrada de serviço.
- **Nome para exibição:** O nome de exibição do serviço.
- **Caminho da imagem:** Local do executável do serviço. Isso não é editável.
- **Conta inicial:** A conta inicial do serviço.
- **Tipo de inicialização:** Tipo de inicialização para o serviço. Dá suporte a **automático**, **manual**e **desabilitado**.
- **Argumentos:** Argumentos a serem executados quando o serviço for iniciado.
- **Dependências:** Dependências para o serviço.

Depois que um serviço for corrigido, você poderá movê-lo para a tabela **incluída** ou pode optar por deixá-lo na tabela **excluída** se não quiser em seu pacote final. Para obter informações adicionais, confira a [documentação de serviços](convert-an-installer-with-services.md).

## <a name="create-package"></a>Criar pacote

- Forneça uma localização para salvar o pacote MSIX.
- Por padrão, os pacotes são salvos na pasta de dados do aplicativo local.
- Você pode definir a localização de salvamento padrão no menu Configurações.
- Se você estiver gerando um arquivo de modelo de conversão, também poderá especificar um local de salvamento diferente para esse arquivo de modelo se não quiser que ele esteja no mesmo local que o pacote MSIX.
- Se desejar continuar editando o conteúdo e as propriedades do pacote antes de salvar o pacote MSIX, você poderá selecionar o [Editor de pacote](package-editor.md) e ser levado ao editor de pacotes.
- Clique em **Criar** para criar o pacote MSIX.

Você verá um pop-up quando o pacote for criado. Esse pop-up incluirá o local de salvamento, vinculado ao local do arquivo do pacote recém-criado. Ele também inclui um link para o local dos arquivos de log para a ferramenta de empacotamento MSIX. Feche esse pop-up e seja redirecionado para a página inicial. Você também pode selecionar o [Editor de pacote](package-editor.md) para ver e modificar as propriedades e o conteúdo do pacote.
