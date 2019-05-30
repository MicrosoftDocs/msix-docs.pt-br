---
title: Criar um pacote de aplicativo com todos os outros instaladores
description: Criar um pacote MSIX com nossa ferramenta de conversão com todos os outros instaladores
author: dianmsft
ms.author: diahar
ms.date: 04/04/2019
ms.topic: article
keywords: Ferramenta de empacotamento de MSIX MSIX, FUTUR,
ms.localizationpriority: medium
ms.openlocfilehash: eaafc015baa320eacfd977e070f7fafdd8c8990e
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795265"
---
# <a name="create-an-msix-package-with-all-other-installer-types"></a>Criar um pacote MSIX com outro instalador todos os tipos

Você pode usar a ferramenta de empacotamento MSIX para criar um pacote de aplicativo MSIX de outros tipos de instalador. O dispositivo ou VM que você está usando para converter seus aplicativos em MSIX deve atender aos seguintes critérios:

- Ele deve ser configurado para receber comandos remotos (executados o comando Enable-PSRemoting na VM).
- Ele deve estar executando o Windows 10, versão 1809 ou uma versão posterior do Windows.
- Quando a ferramenta é iniciado pela primeira vez, você será solicitado a fornecer consentimento para enviar dados de telemetria. É importante observar que os dados de diagnóstico que você compartilhar apenas o aplicativo é proveniente e nunca são usados para identificar ou contatar você. Isso apenas nos ajuda a corrigir as coisas mais rápidas para você.

## <a name="choose-the-installer-you-want-to-package"></a>Escolha o instalador do pacote

Sob **escolher o instalador que você deseja pacote**, localize o instalador em seu computador. Se você não tiver um instalador, você pode ignorar esta etapa. É recomendável que você assinar seu pacote.

- Marque a caixa sob **assinar o pacote**, navegue até e selecione o arquivo de certificado. pfx. Se o certificado for protegido por senha, digite a senha na caixa de senha.
- Marque a caixa sob **usar argumentos de instalador** e insira o argumento desejado no campo fornecido. Isso é opcional. Este campo aceita qualquer cadeia de caracteres.

## <a name="packaging-method"></a>Método de compactação

- Selecione seu método de empacotamento e clique em **próxima**.

## <a name="package-information"></a>Informações do pacote

Depois que você escolher empacotar o aplicativo em uma máquina virtual existente, você deve fornecer informações sobre o aplicativo. A ferramenta tentará preencher automaticamente esses campos com base nas informações disponíveis do instalador (se aplicável). Você sempre terá uma opção para atualizar as entradas, conforme necessário. Se o campo como um asterisco *, isso é necessário. Ajuda embutida será fornecida se a entrada não é válida.

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

Neste momento, se você não forneceu-nos com um instalador, vá em frente e execute todos os instaladores necessários no momento. Isso inclui programas e scripts personalizados. A ferramenta está monitorando as alterações no dispositivo, incluindo os itens que estão sendo instalados no dispositivo.

## <a name="manage-first-launch-tasks"></a>Gerenciar tarefas de inicialização primeiro

Esta página mostra os executáveis do aplicativo que a ferramenta capturada. Se houver vários aplicativos, marque a caixa que corresponde ao ponto de entrada principal. Se você não vir o .exe aplicativo aqui, procure-o manualmente e executá-lo.

Clique em **próxima** você verá um pop-up que pede confirmação que tiver terminado com a instalação do aplicativo e gerenciar tarefas de inicialização primeiro.

- Se você tiver terminado, clique em **Sim, passaremos**.
- Se você não tiver concluído, clique em **não, eu não terminei**. Você será levado volta para a última página onde você pode iniciar os aplicativos, instalar ou copiar outros arquivos e executáveis/dlls.

## <a name="create-package"></a>Criar pacote

- Fornece um local para salvar o pacote MSIX.
- O local de salvamento padrão é a área de trabalho.
- Você pode definir o local de salvamento padrão no menu Configurações.
- Se você gostaria de continuar a editar o conteúdo e as propriedades do pacote antes de salvar o MSIX do pacote, você pode selecionar **editor de pacote** e levado para [editor de pacote]("https://docs.microsoft.com/en-us/windows/msix/packaging-tool/package-editor")
- Clique em **criar** para criar o pacote MSIX.

Você verá o pop-se quando o pacote é criado. Isso pop até incluem o nome, editor e salve o local dos logs e salvar o local do pacote recém-criado. Você pode fechar este pop para cima e redirecionados para a página de boas-vinda. Você também pode selecionar um editor de pacote para ver e modificar as propriedades e o conteúdo do pacote.