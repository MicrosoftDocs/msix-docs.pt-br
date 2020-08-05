---
title: Solucionar problemas de instalação com o arquivo do Instalador de Aplicativo
description: Problemas comuns ao fazer sideload de aplicativos com o arquivo do Instalador de Aplicativo.
ms.date: 5/2/2018
ms.topic: article
keywords: windows 10, uwp, instalador do aplicativo, AppInstaller, sideload
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 99f38075d1ad586186261bfb9c39e7f3253ed05e
ms.sourcegitcommit: f1c366459764cf1f3c0bc9edcac4f845937794bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87754516"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>Solucionar problemas de instalação com o arquivo do Instalador de Aplicativo

Se você encontrar problemas ao instalar um aplicativo do arquivo do Instalador de Aplicativo, este tópico fornecerá algumas diretrizes de solução de problemas que poderão ajudar.

## <a name="prerequisites"></a>Pré-requisitos

Para poder fazer sideload de aplicativos no Windows 10, o dispositivo do usuário deve satisfazer os seguintes requisitos:

- O dispositivo deve estar habilitado para o Modo de Desenvolvedor ou para aplicativos de Sideload. Consulte [Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) para saber mais.
- O certificado usado para assinar o pacote deve ser confiável pelo dispositivo. Consulte a seção **Certificados confiáveis** abaixo para obter mais detalhes.
- A versão do Windows 10 deve dar suporte ao esquema de arquivo `.appinstaller` e ao protocolo de distribuição.

## <a name="common-issues"></a>Problemas comuns

Há alguns problemas comuns ao fazer sideload um aplicativo pela primeira vez na máquina do usuário. As próximas seções descrevem os problemas mais comuns e suas soluções.

### <a name="windows-version"></a>Versão do Windows

Cada versão do Windows 10 aprimora a experiência de sideload, na tabela a seguir você vai encontrar quais recursos estão disponíveis em cada versão principal. Se você tentar fazer o sideload de um aplicativo usando um método que não possui suporte na sua versão do Windows 10, você receberá um erro de implantação.

| Versão | Notas de sideload |
|---------|----------------|
| Build 17134 (atualização de abril de 2018, versão 1803)    | O arquivo `.appinstaller` pode ser acessado pela pastas UNC/compartilhadas. As verificações de atualização configuráveis também estão disponíveis. |
| Build 16299 (Fall Creators Update, versão 1709) | Introduziu o arquivo `.appinstaller` para fornecer atualizações automáticas ao seu aplicativo. Esta versão só possui suporte a pontos de extremidade HTTP. Verificações de atualização não são configuráveis e ocorrem a cada 24 horas. |
| Build 15063 (Creators Update, versão 1703)      | O aplicativo do Instalador de Aplicativo pode baixar dependências do aplicativo (somente no modo versão) da Store. |
| Build 14393 (Atualização de Aniversário, versão 1607)   | Não há suporte para o aplicativo do Instalador de Aplicativo para instalar os arquivos .appx e arquivos .appxbundle, e o arquivo .appinstaller não é suportado. |
| Build 10586 (Atualização de novembro, versão 1511)      | Fazer o sideload só está disponível por meio do PowerShell usando o [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) comando. |
| Build 10240 (Windows 10, versão 1507)           | Fazer o sideload só está disponível por meio do PowerShell usando o [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) comando. |

### <a name="trusted-certificates"></a>Certificados confiáveis

Os pacotes de aplicativos devem ser assinados com um certificado confiável pelo dispositivo. Os certificados fornecidos por autoridades de certificação comuns são confiáveis por padrão no sistema operacional Windows.

No entanto, se o certificado usado para assinar um pacote de aplicativo não for confiável ou for um certificado gerado localmente/autoassinado usado durante o desenvolvimento, o instalador do aplicativo poderá relatar que o pacote não é confiável e impedirá que ele seja instalado:

![MSIX assinado com certificado ausente ou não confiável](..\images\msix-bad-cert.png)

Para resolver esse problema, um usuário com direitos de administrador local para o dispositivo deve usar a ferramenta **certificados de computador** para importar o certificado em um dos seguintes contêineres:

1. Computador local: pessoas confiáveis
2. Computador local: autoridades de raiz confiáveis (não recomendado)

>[!IMPORTANT]
> Não **importe certificados de assinatura de pacote para o repositório de certificados do usuário**. O instalador do aplicativo não pesquisa certificados de usuário ao verificar a identidade do pacote.

A ferramenta de gerenciamento de certificados de computador pode ser facilmente encontrada com a pesquisa no menu iniciar:

![Localizar a ferramenta de certificados do computador local por meio do menu iniciar](..\images\start-comp-cert.png)

Depois que o certificado de autenticação for importado com êxito, executar novamente o instalador do aplicativo mostrará que o pacote é confiável e pode ser instalado:

![MSIX assinado com um certificado confiável](..\images\msix-good-cert.png)

### <a name="dependencies-not-installed"></a>Dependências não instaladas

Os aplicativos do Windows 10 podem ter dependências de estrutura baseadas na plataforma de aplicativo usada para gerar o aplicativo. Se você estiver usando C# ou VB, o aplicativo precisará usar o .NET Runtime e os pacotes do .NET Framework. Aplicativos C++ exigem o VCLibs.

>[!IMPORTANT]
> Se o pacote de aplicativo é compilado na configuração de modo Versão, as dependências de estrutura serão obtidas da Microsoft Store. No entanto, se o aplicativo é compilado na configuração de modo Depuração, as dependências serão obtidas a partir do local especificado no arquivo `.appinstaller`.

### <a name="files-not-accessible"></a>Os arquivos não estão acessíveis

Ao instalar em um ponto de extremidade HTTP, é importante verificar que todos os arquivos estejam acessíveis com o tipo MIME correto. O método mais fácil de verificar esses arquivos é seguindo os links fornecidos na página HTML gerada pelo Visual Studio. Você deve verificar esses arquivos:

- `.appinstaller`arquivo, disponível como um`application/xml`
- `.appx`e `.appxbundle` arquivos, disponíveis como`application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>Isolar problemas de aplicativo do Instalador de Aplicativo

Se o instalador do aplicativo não puder instalar o aplicativo, essas etapas ajudarão a identificar o problema de instalação.

### <a name="verify-app-package-file-installation"></a>Verificar a instalação do arquivo do pacote do aplicativo

- Baixe o arquivo de pacote do aplicativo em uma pasta local e tente instalá-lo usando o comando do PowerShell [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) .

- Baixe o arquivo `.appinstaller` para uma pasta local e tentar instalá-lo usando o comando `Add-AppxPackage -Appinstaller` do PowerShell.

### <a name="app-installer-event-logs"></a>Logs de eventos do instalador de aplicativos

A infraestrutura de implantação de aplicativo emite logs que geralmente são úteis para depurar problemas de instalação por meio do Windows Visualizador de Eventos:`Application and Services Logs -> Microsoft -> Windows -> AppxDeployment-Server`
