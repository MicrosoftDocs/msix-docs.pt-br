---
title: Criar um pacote MSIX com base em um instalador da Área de Trabalho (MSI, EXE ou App-V) em uma VM
description: Criar um pacote MSIX com base em um instalador da Área de Trabalho (MSI, EXE ou App-V) em uma VM
author: mcleanbyron
ms.author: mcleans
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2997fa46ebf2c7c027d20772e619234a93a821e6
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555571"
---
# <a name="create-an-msix-package-from-a-desktop-installer-msi-exe-or-app-v-on-a-vm"></a>Criar um pacote MSIX com base em um instalador da área de trabalho (MSI, EXE ou App-V) em uma VM

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

Use a [Ferramenta de Empacotamento MSIX](../mpt-overview.md) para criar um pacote do aplicativo MSIX com base em um instalador MSI, EXE ou App-V existente em uma VM (máquina virtual) do Hyper-V. A VM precisa atender a estes requisitos:

- Ela precisa ser configurada para [receber comandos remotos](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/remotely-manage-hyper-v-hosts) (execute o comando [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting?view=powershell-5.1) na VM)
- Ele precisa executar o Windows 10, versão 1809 ou uma versão posterior do Windows.

> [!NOTE]
> No momento, a Ferramenta de Empacotamento MSIX é compatível com o App-V 5.1. Se você tiver um pacote com o App-V 4.x, recomendamos que o converta para o App-V 5.1 antes de usar a Ferramenta de Empacotamento MSIX para converter em MSIX. 

Quando a ferramenta for iniciada pela primeira vez, você deverá fornecer consentimento para enviar dados telemétricos. É importante observar que os dados de diagnóstico que você compartilha são provenientes apenas do aplicativo e nunca são usados para identificá-lo ou contatá-lo. Isso apenas nos ajuda a corrigir as coisas mais rapidamente para você.

A criação de um pacote do aplicativo é a opção mais comumente usada. É nesse local em que você criará um pacote MSIX com base em um instalador ou pela instalação manual do conteúdo do aplicativo.

![pic1](images/pic1.png)

## <a name="choose-the-installer-you-want-to-package"></a>Escolher o instalador que você deseja empacotar

![pic2](images/pic2.png)

Navegue até o instalador MSI ou App-V clicando em **Procurar** e selecionando o instalador no seletor de arquivos. Em seguida, clique em **Avançar**.

Opcionalmente:
- Marque a caixa em **Usar pacote MSIX existente**, procure e selecione um pacote MSIX existente que deseja atualizar.
- Marque a caixa em **Usar argumentos do instalador** e insira o argumento desejado no campo fornecido. Esse campo aceita qualquer cadeia de caracteres.
- Marque a caixa em **Assinar pacote para teste**, procure e selecione o arquivo de certificado .pfx. Se o certificado for protegido por senha, digite a senha na caixa de senha.

## <a name="packaging-method"></a>Método de compactação

![images/pic3](images/pic3.png)

- Selecione a máquina virtual para o ambiente de empacotamento.
  - Selecione **Criar o pacote em uma máquina virtual existente** e, na lista suspensa, selecione um nome de máquina virtual existente. Você verá os campos de usuário e senha para fornecer credenciais para a VM, se houver.
  - Clique em **Avançar**.

## <a name="package-information"></a>Informações do pacote

![images/pic4](images/pic4.png)

Depois de optar por empacotar o aplicativo em uma máquina virtual existente, você precisará fornecer informações sobre o aplicativo. A ferramenta tentará preencher automaticamente esses campos com base nas informações disponíveis no instalador. Você sempre terá uma opção para atualizar as entradas, conforme necessário. Se o campo tiver um asterisco *, ele será obrigatório, mas você já sabia disso. A ajuda embutida será fornecida se a entrada não for válida.
- Nome do pacote:
    - Obrigatório; corresponde ao Nome de identidade do pacote no manifesto para descrever o conteúdo do pacote.
    - Precisa ser correspondente às informações da entidade do Nome do certificado usado para assinar um pacote.
    - Não é mostrado ao usuário final.
    - Diferencia maiúsculas de minúsculas e não pode ter um espaço.
    - Pode aceitar uma cadeia de caracteres entre 3 e 50 caracteres que consiste em caracteres alfanuméricos, ponto e traço.
    - Não pode terminar com um ponto nem ser um dos seguintes: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8" e "LPT9."
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
    - Este campo aceita uma cadeia de caracteres de versão na notação quadrupleto: "Major.Minor.Build.Revision".
- Local de instalação:
    - Esse é o local em que o instalador copiará o conteúdo do aplicativo (normalmente, a pasta Arquivos de Programas).
    - Esse campo é opcional, mas recomendado, especialmente quando o conteúdo do aplicativo está sendo instalado fora das pastas Arquivos de Programas.
    - Procure e selecione um caminho de pasta.
    - Verifique se esse arquivo corresponde ao local de instalação do instalador enquanto você executa a operação de instalação do aplicativo.

## <a name="prepare-computer"></a>Preparar o computador

![images/pic5](images/pic5.png)

Em seguida, a página **Preparar computador** fornece opções para preparar o computador para empacotamento.

O Driver da Ferramenta de Empacotamento MSIX é obrigatório e a ferramenta tentará automaticamente habilitá-lo se ele não estiver habilitado. A ferramenta verificará primeiro com o DISM para ver se o driver está instalado.

> [!NOTE]
> O Driver da Ferramenta de Empacotamento MSIX monitora o sistema para capturar as alterações que um instalador faz no sistema, que permite à Ferramenta de Empacotamento MSIX criar um pacote com base nessas alterações.

- [Opcional] Marque a caixa **O Windows Search está Ativo** e selecione **Desabilitar selecionado** se você optar por desabilitar o serviço de pesquisa.
    - Isso não é obrigatório, somente recomendado.
    - Quando essa opção estiver desabilitada, a ferramenta atualizará o campo de status para “desabilitado”

- [Opcional] Marque a caixa **O Windows Update está Ativo** e selecione **Desabilitar selecionado** se você optar por desabilitar o serviço de Atualização.
    - Isso não é obrigatório, somente recomendado.
    - Quando essa opção estiver desabilitada, a ferramenta atualizará o campo de status para **Desabilitado**.

- A caixa de seleção **Reinicialização pendente** está desabilitada por padrão. Você precisará reiniciar o computador manualmente e, em seguida, iniciar a ferramenta novamente caso receba um prompt informando que as operações pendentes precisam de reinicialização. Isso não é obrigatório, somente recomendado.

Quando terminar de preparar o computador, clique em **Avançar**.

## <a name="installation"></a>Instalação

![images/pic6](images/pic6.png)

> [!NOTE]
> Durante a conversão, os instaladores podem executar os serviços. Os serviços não são capturados durante a conversão. Como resultado, o aplicativo pode ser instalado, mas ser executado com problemas.

- Esta é a fase de instalação em que a ferramenta monitora e captura as operações de instalação do aplicativo.
- A ferramenta iniciará o instalador na Janela da Máquina Virtual que ele abriu em um estágio anterior, e você precisará passar pelo assistente de instalação para instalar o aplicativo.
    - Verifique se o caminho de instalação corresponde ao que foi definido anteriormente na página de informações do pacote.
    - Você precisará criar um atalho na área de trabalho para o aplicativo recém-instalado.
    - Depois de concluir o assistente de instalação do aplicativo, conclua ou feche o assistente de instalação.
    - Caso precise executar vários instaladores, faça isso manualmente neste momento.
    - Se o aplicativo precisar de outros pré-requisitos, você precisará instalá-los agora.
    - Se o aplicativo precisar do .Net 3.5/20, adicione o recurso opcional ao Windows.
- Quando concluir a instalação do aplicativo, clique em **Avançar**.

## <a name="manage-first-launch-tasks"></a>Gerenciar as primeiras tarefas de inicialização

![images/pic7](images/pic7.png)

Essa página mostra os executáveis do aplicativo capturados pela ferramenta. Recomendamos iniciar o aplicativo pelo menos uma vez para capturar as primeiras tarefas de inicialização.

Se houver vários aplicativos, marque a caixa que corresponde ao ponto de entrada principal. Se o aplicativo .exe não for exibido aqui, procure-o manualmente e execute-o.

Clique em **Avançar** e você verá um pop-up que solicitará a confirmação de que você concluiu a instalação do aplicativo e o gerenciamento das primeiras tarefas de inicialização.
- Se você tiver concluído, clique em **Sim, continuar**.
- Se você não tiver concluído, clique em **Não, não terminei**. Você será levado novamente à última página, em que pode iniciar aplicativos, instalar ou copiar outros arquivos e DLLs/executáveis.

## <a name="create-package"></a>Criar pacote

![images/pic8](images/pic8.png)

- Forneça uma localização para salvar o pacote MSIX.
- Por padrão, os pacotes são salvos na pasta de dados do aplicativo local.
- Você pode definir a localização de salvamento padrão no menu Configurações.
- Caso deseje continuar editando o conteúdo e as propriedades do pacote antes de salvar o pacote MSIX, selecione “Editor de pacote” para acessar o editor de pacote.
- Caso prefira assinar o pacote com um certificado predefinido para testes, navegue até o certificado e selecione-o.
- Clique em **Criar** para criar o pacote MSIX.

Você verá o pop-se quando o pacote for criado. Esse pop-up incluirá o nome, o fornecedor e a localização de salvamento do pacote recém-criado. Feche esse pop-up e seja redirecionado para a página inicial. Selecione também um editor de pacote para ver e modificar as propriedades e o conteúdo do pacote.

