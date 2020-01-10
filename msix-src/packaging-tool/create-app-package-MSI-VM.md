---
title: Criar um pacote MSIX com base em um instalador da Área de Trabalho (MSI, EXE ou App-V) em uma VM
description: Criar um pacote MSIX com base em um instalador da Área de Trabalho (MSI, EXE ou App-V) em uma VM
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e9cd7b8967c65f9cfcc4fb11d66650975b6bbda9
ms.sourcegitcommit: 71c49de79d061909fb1ab632ec7550227d2287bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754884"
---
# <a name="create-an-msix-package-from-a-desktop-installer-msi-execlickonce-or-app-v"></a>Criar um pacote MSIX de um instalador de desktop (MSI, EXE, ClickOnce ou App-V)

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Você pode usar a [ferramenta de empacotamento MSIX](../mpt-overview.md) para criar um pacote de aplicativos do MSIX de um instalador MSI, exe, ClickOnce ou app-v existente em uma VM (máquina virtual) do Hyper-v. A VM precisa atender a estes requisitos:

É recomendável seguir as [práticas recomendadas](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-best-practices) para configurar seu ambiente e a ferramenta de empacotamento MSIX para conversão. 

> [!NOTE]
> No momento, a Ferramenta de Empacotamento MSIX é compatível com o App-V 5.1. Se você tiver um pacote com o App-V 4.x, recomendamos que o converta para o App-V 5.1 antes de usar a Ferramenta de Empacotamento MSIX para converter em MSIX. 

Quando a ferramenta for iniciada pela primeira vez, você deverá fornecer consentimento para enviar dados telemétricos. É importante observar que os dados de diagnóstico que você compartilha são provenientes apenas do aplicativo e nunca são usados para identificá-lo ou contatá-lo.

A criação de um pacote do aplicativo é a opção mais comumente usada. É aqui que você criará um pacote MSIX a partir de um instalador ou por meio da [instalação manual](https://docs.microsoft.com/windows/msix/packaging-tool/create-other-installer) da carga do aplicativo.

![pic1](images/pic1.PNG)

## <a name="packaging-method"></a>Método de compactação

Selecione uma opção no computador de conversão:
- Se você já estiver trabalhando em um ambiente limpo, selecione **criar pacote neste computador**
- Se você quiser se conectar a uma VM existente ou a um computador remoto, selecione **Criar pacote em um computador remoto**
  - Você precisará [configurar seu computador remoto](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup) antes de poder convertê-lo
- Se você tiver uma VM local no computador em que deseja converter, selecione **Criar pacote em uma máquina virtual local**
  - Clique em **Next**
  
## <a name="prepare-computer"></a>Preparar o computador

Em seguida, a página **Preparar computador** fornece opções para preparar o computador para empacotamento.

O **Driver da ferramenta de empacotamento MSIX** é necessário e a ferramenta tentará habilitá-la automaticamente se não estiver habilitada. A ferramenta verificará primeiro com o DISM para ver se o driver está instalado. Se você se deparar com um problema, tente verificar nossa [documentação de solução de problemas](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-known-issues)e, em seguida, o problema de um hub de [comentários](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-known-issues#sending-feedback) será reparado. 

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

Navegue até o MSI, App-V ou outro instalador do Win 32 clicando em **procurar** e selecionando o instalador no seletor de arquivos.

Se você tiver argumentos de instalador, poderá inserir o argumento desejado no campo fornecido. Esse campo aceita qualquer cadeia de caracteres.

Em **preferência de assinatura**, selecione uma opção de assinatura. Também é possível definir como padrão nas configurações. Desse modo, você pula algumas etapas na hora da conversão. 
- **Assinar com assinatura do Device Guard** Essa opção permite que você entre em sua conta do Microsoft Active Directory que você configurou para usar com a assinatura do Device Guard, que é um serviço de assinatura que a Microsoft fornece onde você não precisa fornecer seu próprio certificado. Saiba mais sobre como configurar sua conta e sobre a assinatura do Device Guard [aqui](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing). 
- **Assinar com um certificado (. pfx)** Navegue até e selecione seu arquivo de certificado. pfx. Se o certificado for protegido por senha, digite a senha na caixa de senha.
- **Especificar um arquivo. cer (o sinal de nota)** Essa opção permite que você especifique um arquivo. cer. Isso é útil quando você não deseja assinar o pacote, mas deseja garantir que as informações do Publicador correspondam ao assunto do certificado que será usado para assinatura. 
- Não **assinar pacote** Selecione esta opção se você estiver assinando seu pacote posteriormente. Observação: não é possível instalar um pacote MSIX se ele não estiver assinado
- Ao assinar, é altamente recomendável adicionar um **carimbo de data/hora** ao seu certificado para que a validade do seu certificado possa Outlast sua data de expiração. O formato aceito é uma [URL do servidor com carimbo de data/hora RFC 3161](https://docs.microsoft.com/windows/win32/seccrypto/signtool). 

Clique em **Próximo** para continuar.

## <a name="package-information"></a>Informações do pacote

Depois de optar por empacotar o aplicativo em uma máquina virtual existente, você precisará fornecer informações sobre o aplicativo. A ferramenta tentará preencher automaticamente esses campos com base nas informações disponíveis no instalador. Você sempre terá uma opção para atualizar as entradas, conforme necessário. Se o campo tiver um asterisco *, ele será obrigatório, mas você já sabia disso. A ajuda embutida será fornecida se a entrada não for válida.
- Nome do pacote:
    - Obrigatório; corresponde ao Nome de identidade do pacote no manifesto para descrever o conteúdo do pacote.
    - Precisa ser correspondente às informações da entidade do Nome do certificado usado para assinar um pacote.
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
- Local de instalação:
    - Esse é o local em que o instalador copiará o conteúdo do aplicativo (normalmente, a pasta Arquivos de Programas).
    - Esse campo é opcional, mas recomendado, especialmente quando o conteúdo do aplicativo está sendo instalado fora das pastas Arquivos de Programas.
    - Procure e selecione um caminho de pasta.
    - Verifique se esse arquivo corresponde ao local de instalação do instalador enquanto você executa a operação de instalação do aplicativo.
- Descrição:
    - Esse campo é opcional. 

## <a name="installation"></a>Instalação

- Esta é a fase de instalação em que a ferramenta monitora e captura as operações de instalação do aplicativo.
- A ferramenta iniciará o instalador na Janela da Máquina Virtual que ele abriu em um estágio anterior, e você precisará passar pelo assistente de instalação para instalar o aplicativo.
    - Verifique se o caminho de instalação corresponde ao que foi definido anteriormente na página de informações do pacote.
    - Você precisará criar um atalho na área de trabalho para o aplicativo recém-instalado.
    - Depois de concluir o assistente de instalação do aplicativo, conclua ou feche o assistente de instalação.
    - Caso precise executar vários instaladores, faça isso manualmente neste momento.
    - Se o aplicativo precisar de outros pré-requisitos, você precisará instalá-los agora.
    - Se o aplicativo precisar do .Net 3.5/20, adicione o recurso opcional ao Windows.
- Se o instalador exigir uma reinicialização, você poderá executar uma reinicialização manual ou usar o botão ' reiniciar ' para executar a reinicialização e retornará a esse ponto no processo de conversão após a reinicialização.
- Quando concluir a instalação do aplicativo, clique em **Avançar**.

## <a name="manage-first-launch-tasks"></a>Gerenciar as primeiras tarefas de inicialização

Essa página mostra os executáveis do aplicativo capturados pela ferramenta. Recomendamos iniciar o aplicativo pelo menos uma vez para capturar as primeiras tarefas de inicialização.

Se houver vários aplicativos, marque a caixa que corresponde ao ponto de entrada principal. Se o aplicativo .exe não for exibido aqui, procure-o manualmente e execute-o.

Clique em **Avançar** e você verá um pop-up que solicitará a confirmação de que você concluiu a instalação do aplicativo e o gerenciamento das primeiras tarefas de inicialização.
- Se você tiver concluído, clique em **Sim, continuar**.
- Se você não tiver concluído, clique em **Não, não terminei**. Você será levado novamente à última página, em que pode iniciar aplicativos, instalar ou copiar outros arquivos e DLLs/executáveis.

## <a name="create-package"></a>Criar pacote

- Forneça uma localização para salvar o pacote MSIX.
- Por padrão, os pacotes são salvos na pasta de dados do aplicativo local.
- Você pode definir a localização de salvamento padrão no menu Configurações.
- Caso deseje continuar editando o conteúdo e as propriedades do pacote antes de salvar o pacote MSIX, selecione “Editor de pacote” para acessar o editor de pacote.
- Clique em **Criar** para criar o pacote MSIX.

Você verá o pop-se quando o pacote for criado. Esse pop-up incluirá o nome, o fornecedor e a localização de salvamento do pacote recém-criado. Feche esse pop-up e seja redirecionado para a página inicial. Você também pode selecionar o [Editor de pacote](https://docs.microsoft.com/windows/msix/packaging-tool/package-editor) para ver e modificar as propriedades e o conteúdo do pacote.

