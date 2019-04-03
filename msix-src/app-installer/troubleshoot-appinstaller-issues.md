---
author: ridomin
title: Solucionar problemas de instalação com o arquivo do instalador de aplicativo | Microsoft Docs
description: Problemas comuns ao fazer sideload de aplicativos com o arquivo do Instalador de Aplicativo.
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
keywords: windows 10, uwp, instalador do aplicativo, AppInstaller, sideload
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: eb2c4e5f9e223e5dd741b53ff3943cc920a61c31
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900618"
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

O pacote do aplicativo deve ter assinatura com certificado de uma fonte confiável pelo dispositivo. Certificados fornecidos por Autoridades de Certificação comuns são confiáveis por padrão no sistema operacional Windows, no entanto, se o certificado não for confiável, ele deve ser instalado no dispositivo **antes** de instalar o aplicativo. Para confiar no certificado, ele deve estar presente em um dos seguintes repositórios de certificados do computador local no seu dispositivo:

- Fornecedores Confiáveis
- Pessoas de Confiança
- Autoridades Raiz Confiáveis (não recomendado)

 >[!IMPORTANT]
 > Instalar um certificado no repositório do Computador local requer acesso administrativo.

### <a name="dependencies-not-installed"></a>Dependências não instaladas 

Aplicativos do Windows 10 podem ter dependências de estrutura com base na plataforma do aplicativo usada para gerar o aplicativo. Se você estiver usando C# ou VB, o aplicativo precisará usar o .NET Runtime e os pacotes do .NET Framework. Aplicativos C++ exigem o VCLibs.

>[!IMPORTANT] 
> Se o pacote de aplicativo é compilado na configuração de modo Versão, as dependências de estrutura serão obtidas da Microsoft Store. No entanto, se o aplicativo é compilado na configuração de modo Depuração, as dependências serão obtidas a partir do local especificado no arquivo `.appinstaller`.

### <a name="files-not-accessible"></a>Os arquivos não estão acessíveis

Ao instalar em um ponto de extremidade HTTP, é importante verificar que todos os arquivos estejam acessíveis com o tipo MIME correto. O método mais fácil de verificar esses arquivos é seguindo os links fornecidos na página HTML gerada pelo Visual Studio. Você deve verificar esses arquivos:

- `.appinstaller` arquivo, disponível como `application/xml`
- `.appx` e arquivos `.appxbundle`, disponíveis como `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>Isolar problemas de aplicativo do Instalador de Aplicativo

Se o aplicativo do Instalador de Aplicativo não puder instalá-lo, essas etapas ajudarão a identificar o problema de instalação.

### <a name="verify-app-package-file-installation"></a>Verifique se a instalação de arquivo de pacote do aplicativo

- Baixe o arquivo de pacote de aplicativo para uma pasta local e tente instalá-lo usando o [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) comando do PowerShell.

- Baixe o arquivo `.appinstaller` para uma pasta local e tentar instalá-lo usando o comando `Add-AppxPackage -Appinstaller` do PowerShell.

## <a name="related-logs"></a>Logs relacionados

A infraestrutura de implantação do aplicativo fornece logs para depuração no Visualizador de Eventos do Windows. Esses logs são encontrados aqui: `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`


