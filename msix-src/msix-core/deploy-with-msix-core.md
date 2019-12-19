---
title: Implantar um pacote MSIX com o MSIX Core
description: Este artigo fornece instruções passo a passo sobre como aproveitar o bootstrapper MSIX Core, que cria um aplicativo usando o ClickOnce que permitirá que os usuários baixem apenas um setup. exe e instalem seu aplicativo MSIX por meio do instalador do MSIX Core.
ms.date: 11/15/2019
ms.topic: article
keywords: Windows 10, Windows 7, Windows 8, Windows Server, UWP, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 03d9c1630ffa9b956cdfd432199500672e18966a
ms.sourcegitcommit: 8aafaa9ac4087ef2e95030343add8fe2ee1cccc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/18/2019
ms.locfileid: "75186742"
---
# <a name="deploy-an-msix-package-with-msix-core"></a>Implantar um pacote MSIX com o MSIX Core
Conforme mencionado na visão geral, o MSIX Core oferece suporte à implantação do MSIX para o Windows 7 SP1, Windows 8.1, atualmente com suporte no Windows Server (com experiência desktop) e versões do Windows 10 anteriores à 1709 (atualização de aniversário de outono). Para começar, você deve garantir que o MSIX Core esteja instalado no sistema operacional.

Para sua conveniência, fornecemos instaladores MSI. Para localizar o instalador de sua escolha, vá para nossa [página de lançamento](https://github.com/microsoft/msix-packaging/releases) e, em **ativos** , você encontrará o seguinte:

1. msixmgrSetup-x64. msi
2. msixmgrSetup-86. msi

## <a name="msi-installation"></a>Instalação do MSI 
Recomendamos a instalação do MSI porque ele adiciona automaticamente o msixmgr. exe ao caminho de pesquisa e associa a extensão MSIX ao instalador.

Baixe o **msixmgrSetup-x64. msi** ou o **msixmgrSetup-x86. msi** (dependendo da sua arquitetura) e instale-o em seu dispositivo Windows. 

> [!NOTE]
> é importante que você escolha o instalador correto para sua arquitetura. Isso afetará o local em que o instalador armazenará arquivos importantes. O nome do arquivo pode ser alterado com base na versão do instalador. 

## <a name="installing-your-certificate"></a>Instalando seu certificado
Os pacotes MSIX devem ser assinados. Antes de instalar os pacotes. msix, verifique se você instalou o certificado usado para assinar seus pacotes. Você pode fazer isso usando fluxos de trabalho normais para instalar o certificado de sua ferramenta de gerenciamento. 

Se você quiser instalar um certificado manualmente, poderá executar esse comando em um prompt de comando elevado: 
```
certutil -addstore root <insert certificate.cert>
```
> [!NOTE]
> você deve adicionar seu certificado confiável na autoridade de certificação raiz confiável em todos os cenários.

## <a name="using-the-command-line"></a>Usando a linha de comando
Depois que a ferramenta msixmgr. exe for instalada, ela poderá ser usada para gerenciar seus pacotes do MSIX neste computador pesquisando, instalando e removendo. O utilitário de linha de comando msixmgr. exe destina-se a administradores de sistema. Ele é mais útil quando executado do prompt administrativo. Nem todos os comandos quando executados a partir de um prompt de comando regular serão exibidos para o console do. Veja abaixo para obter mais detalhes.

### <a name="install"></a>Instalar
Usando o prompt de comando ou o PowerShell, navegue até o diretório que contém os executáveis e execute o comando a seguir para instalar o notepadplus. msix. O parâmetro-quietUX também pode ser adicionado no final do comando para que os usuários não vejam a interface do usuário do instalador. Por exemplo: 
```
msixmgr.exe -AddPackage C:\SomeDirectory\notepadplus.msix -quietUX
```
### <a name="querying-for-a-specific-msix-package"></a>Consultando um pacote MSIX específico
Pesquisar um pacote específico é possível por packageFullName, packageFamilyName e/ou usar curingas também. Os curingas com suporte são * (corresponder a qualquer caractere) e? (corresponder um único caractere). 
```
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```
### <a name="uninstall"></a>Uninstall
Para desinstalar, use o seguinte comando: 
```
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```
> [!NOTE]
> os comandos acima usam **notepadplus. msix** é um dos nossos [pacotes de exemplo](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests).