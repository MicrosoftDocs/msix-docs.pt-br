---
title: Criar um pacote MSIX de um instalador da área de trabalho (MSI, EXE ou App-V) em uma VM
description: Criar um pacote MSIX de um instalador da área de trabalho (MSI, EXE ou App-V) em uma VM
author: mcleanbyron
ms.author: mcleans
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 457f9a0358cb8e72abd9539d1a4fe3131ffd2a54
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400811"
---
# <a name="create-an-msix-package-from-a-desktop-installer-msi-exe-or-app-v-on-a-vm"></a>Criar um pacote MSIX de um instalador da área de trabalho (MSI, EXE ou App-V) em uma VM

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

Você pode usar o [ferramenta de empacotamento MSIX](../mpt-overview.md) para criar um pacote de aplicativo MSIX de um instalador MSI, EXE ou App-V existente em uma máquina virtual do Hyper-V (VM). A VM deve atender a esses requisitos:

- Ele deve ser configurado para [receber comandos remotos](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/remotely-manage-hyper-v-hosts) (executar o [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting?view=powershell-5.1) comando na VM)
- Ele deve estar executando o Windows 10, versão 1809 ou uma versão posterior do Windows.

Quando a ferramenta é iniciado pela primeira vez, você será solicitado a fornecer consentimento para enviar dados de telemetria. É importante observar que os dados de diagnóstico que você compartilhar apenas o aplicativo é proveniente e nunca são usados para identificar ou contatar você. Isso apenas nos ajuda a corrigir as coisas mais rápidas para você.

Criando um pacote de aplicativo é a opção mais comumente usada. Isso é onde você irá criar um pacote MSIX de um instalador ou por instalação manual de carga do aplicativo.

![pic1](images/pic1.png)

## <a name="choose-the-installer-you-want-to-package"></a>Escolha o instalador do pacote

![pic2](images/pic2.png)

Navegue até seu instalador MSI ou o App-V clicando **procurar** e selecionando o instalador no seletor de arquivos. Em seguida, clique em **Avançar**.

Opcionalmente:
- Marque a caixa sob **usar pacote existente de MSIX**, navegue e selecione um pacote existente de MSIX você gostaria de atualizar.
- Marque a caixa sob **usar argumentos de instalador** e insira o argumento desejado no campo fornecido. Este campo aceita qualquer cadeia de caracteres.
- Marque a caixa sob **assinar o pacote para teste**, navegue até e selecione o arquivo de certificado. pfx. Se o certificado for protegido por senha, digite a senha na caixa de senha.

## <a name="packaging-method"></a>Método de compactação

![images/pic3](images/pic3.png)

- Selecione a máquina virtual para o ambiente de empacotamento.
  - Selecione **criar o pacote em uma máquina virtual existente** e na lista suspensa, selecione um nome de máquina virtual existente. Você verá os campos usuário e senha para fornecer credenciais para a VM, caso haja algum.
  - Clique em **Avançar**.

## <a name="package-information"></a>Informações do pacote

![imagens/pic4](images/pic4.png)

Depois que você escolher empacotar o aplicativo em uma máquina virtual existente, você deve fornecer informações sobre o aplicativo. A ferramenta tentará preencher automaticamente esses campos com base nas informações disponíveis no instalador. Você sempre terá uma opção para atualizar as entradas, conforme necessário. Se o campo como um asterisco *, é obrigatório, mas você já sabia disso. Ajuda embutida será fornecida se a entrada não é válida.
- Nome do pacote:
    - Necessário e que correspondem à identidade de nome no manifesto para descrever o conteúdo do pacote do pacote.
    - Deve corresponder as informações de nome de assunto do certificado usado para assinar um pacote.
    - Não é mostrado ao usuário final.
    - Diferencia maiusculas de minúsculas e não pode ter um espaço.
    - Pode aceitar uma cadeia de caracteres entre 3 e 50 caracteres de comprimento que consiste em alfanuméricos, período e caracteres de traço.
    - Não pode terminar com um ponto e ser um destes procedimentos: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8" e "LPT9".
- Nome de exibição do pacote:
    - Necessária e corresponde ao pacote no manifesto para exibir o nome amigável do pacote para o usuário, no menu Iniciar e páginas de configurações.
    - Campo aceita uma cadeia de caracteres entre 1 e 256 caracteres de comprimento e é localizável.
- Nome do Editor:
    - Necessária e corresponde ao pacote que descreve as informações do Editor.
    - O atributo Publisher deve corresponder as informações do editor da entidade do certificado usado para assinar um pacote.
    - Este campo aceita uma cadeia de caracteres entre 1 e 8192 caracteres de comprimento que se ajusta a expressão regular de um nome distinto: "(CN | L | S | UO | E | C | S | RUA | T | G | Eu | SN | CONTROLADOR DE DOMÍNIO | SERIALNUMBER | Descrição | PostalCode | Caixa postal | Telefone | X21Address | dnQualifier | (OID. (0 | [1-9] [0-9]) (. (0 | [1-9] [0-9])) +)) = (([^, + = "<> #;]) + | ".") (, ((CN | L | S | UO | E | C | S | RUA | T | G | Eu | SN | CONTROLADOR DE DOMÍNIO | SERIALNUMBER | Descrição | PostalCode | Caixa postal | Telefone | X21Address | dnQualifier | (OID. (0 | [1-9] [0-9]) (. (0 | [1-9] [0-9])) +)) = (([^, + = "<> #;]) + | "."))) *".
- Nome de exibição do publicador:
    - Necessária e corresponde ao pacote no manifesto para exibir o nome amigável do publicador para o usuário, nas páginas de configurações e o instalador de aplicativo.
    - Campo aceita uma cadeia de caracteres entre 1 e 256 caracteres de comprimento e é localizável.
- Versão:
    - Necessária e corresponde ao pacote no manifesto para descrever o número de versão do pacote.
    - Este campo aceita uma cadeia de caracteres de versão na notação quadrangular: "Major".
- Local de instalação:
    - Esse é o local que o instalador vai copiar a carga do aplicativo (normalmente, a pasta de arquivos de programas).
    - Este campo é opcional mas recomendado especialmente quando a carga do aplicativo está sendo instalada fora as pastas de arquivos de programas.
    - Procure e selecione um caminho de pasta.
    - Verifique se que esse arquivo corresponde ao local de instalação do instalador enquanto você percorre a operação de instalação do aplicativo.

## <a name="prepare-computer"></a>Preparar o computador

![imagens/pic5](images/pic5.png)

Em seguida, o **preparar computador** página fornece opções para preparar o computador para o empacotamento.

O Driver de ferramenta de empacotamento MSIX é necessário e a ferramenta tentará automaticamente habilitá-lo se ele não está habilitado. A ferramenta verificará primeiro com o DISM para ver se o driver está instalado.

> [!NOTE]
> O Driver de ferramenta de empacotamento MSIX monitora o sistema para capturar as alterações que um instalador está fazendo no sistema que permite que a ferramenta de empacotamento MSIX criar um pacote com base nessas alterações.

- [Opcional] Marque a caixa **do Windows Search está ativo** e selecione **Disable selecionado** se você optar por desabilitar o serviço de pesquisa.
    - Isso não é necessário, só é recomendado.
    - Quando desabilitado, a ferramenta atualizará o campo status como "desabilitado"

- [Opcional] Marque a caixa **Windows Update está ativo** e selecione **Disable selecionado** se você optar por desabilitar o serviço de atualização.
    - Isso não é necessário, só é recomendado.
    - Quando desabilitado, a ferramenta atualizará o campo de status para **desabilitada**.

- O **reinicialização pendente** caixa de seleção está desabilitada por padrão. Você precisará reiniciar manualmente o computador e, em seguida, inicie a ferramenta novamente se você for solicitado que as operações pendentes precisam de reinicialização. Isso não obrigatório, somente o recomendado.

Quando você terminar Preparando a máquina, clique em **próxima**.

## <a name="installation"></a>Instalação

![images/pic6](images/pic6.png)

- Esta é a fase de instalação em que a ferramenta está monitorando e capturar o aplicativo instalar operações.
- A ferramenta iniciará o instalador na janela da máquina Virtual que ele aberto em um estágio anterior e você precisará passar pelo Assistente de instalação para instalar o aplicativo.
    - Verifique se que o caminho de instalação corresponde ao que foi definido anteriormente na página de informações do pacote.
    - Você precisará criar um atalho na área de trabalho para o aplicativo recém-instalado.
    - Depois que você concluir o Assistente de instalação do aplicativo, certifique-se de concluir ou fechar o Assistente de instalação.
    - Se você precisar executar vários instaladores você pode fazer isso manualmente no momento.
    - Se o aplicativo precisar de outros pré-requisitos, você precisará instalá-los agora.
    - Se o aplicativo precisa que o .net 3.5/20, adicione o recurso opcional para o Windows.
- Quando você concluir a instalação do aplicativo, clique em **próxima**.

## <a name="manage-first-launch-tasks"></a>Gerenciar tarefas de inicialização primeiro

![images/pic7](images/pic7.png)

Esta página mostra os executáveis do aplicativo que a ferramenta capturada. É recomendável iniciá-lo pelo menos uma vez para capturar qualquer primeiras tarefas de inicialização.

Se houver vários aplicativos, marque a caixa que corresponde ao ponto de entrada principal. Se você não vir o .exe aplicativo aqui, procure-o manualmente e executá-lo.

Clique em **próxima** você verá um pop-up que pede confirmação que tiver terminado com a instalação do aplicativo e gerenciar tarefas de inicialização primeiro.
- Se você tiver terminado, clique em **Sim, passaremos**.
- Se você não tiver concluído, clique em **não, eu não terminei**. Você será levado volta para a última página onde você pode iniciar os aplicativos, instalar ou copiar outros arquivos e executáveis/dlls.

## <a name="create-package"></a>Criar pacote

![images/pic8](images/pic8.png)

- Fornece um local para salvar o pacote MSIX.
- Por padrão, os pacotes são salvos na pasta de dados de aplicativo local.
- Você pode definir o local de salvamento padrão no menu Configurações.
- Se você quiser continuar a editar o conteúdo e as propriedades do pacote antes de salvar o pacote MSIX, você pode selecionar "Editor de pacote" e levado para o editor de pacote.
- Se você preferir assinar o pacote com um certificado predefinido para testes, navegue até e selecione o certificado.
- Clique em **criar** para criar o pacote MSIX.

Você verá o pop-se quando o pacote é criado. Isso pop até incluirá o nome, publicador e salve o local do pacote recém-criado. Você pode fechar este pop para cima e redirecionados para a página de boas-vinda. Você também pode selecionar um editor de pacote para ver e modificar as propriedades e o conteúdo do pacote.
