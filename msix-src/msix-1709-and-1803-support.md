---
author: c-don
title: Suporte MSIX em compilações 1709 e 1803
description: Este artigo resume o suporte para MSIX com nossas atualizações mais recentes a partir de 22/1/2019.
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: Windows 10, uwp, msix, 1709, 1803
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: d3cfed5d986551ee72ab174b28409927a365cf6e
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795397"
---
# <a name="msix-support-on-windows-10-builds-1709-and-1803"></a>Suporte MSIX em compilações do Windows 10 1709 e 1803

Este artigo resume o suporte MSIX e limitações com nossas atualizações mais recentes.

Por demanda popular, adicionamos suporte para MSIX em mais versões do Windows 10. Notavelmente, isso abrange as versões 1709 e 1803. Esse suporte permite aos usuários implantar pacotes MSIX em versões anteriores do Windows, aproveitando as vantagens de todos os benefícios do MSIX, incluindo o uso de contêineres e segurança por meio de certificados. Para tirar proveito dessas atualizações, verifique se você tiver o 1.0.30311 ou versão posterior do instalador de aplicativo e estar na atualização do Windows 10 1709 ou posterior. 

Abaixo, discutiremos recursos e limitações de suporte MSIX em versões anteriores do sistema operacional.

##  <a name="msix-double-click-support"></a>Suporte a dois cliques MSIX
Um dos benefícios de implantar um MSIX na versão 1809 e posteriores é que o usuário pode instalar o pacote clicando nele. Com a versão mais recente do instalador do aplicativo - 1.0.30311.0 - a capacidade de instalar um pacote MSIX clicando nele está disponível nas atualizações do Windows 10 1709 e 1803 também. 

Clicar no pacote fornece o mesmo suporte de instalação, como no 1809, ou seja, o aplicativo do instalador de aplicativos da Microsoft mostra a interface do usuário para o usuário que o orienta a instalação do aplicativo. O instalador do aplicativo é pré-instalado e obtém atualizações da loja, portanto, podemos pode sempre colocar você a melhor experiência de instalação. 

O instalador de aplicativo podem ser baixado para uso offline na empresa da Microsoft Store para empresas [portal da web](https://businessstore.microsoft.com/en-us/store/details/app-installer/9NBLGGH4NNS1). Você pode aprender mais sobre a distribuição off-line [aqui](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

Abaixo está um resumo do suporte à instalação clique MSIX disponível nas atualizações do Windows 10 1709 e 1803.

### <a name="support-matrix"></a>Matriz de suporte

| Versão do Sistema Operacional|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| 1709(16299) Win 10 | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Win 10 1803(17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| 1809(17763) Win 10 e acima | &#x2713; | &#x2713; | &#x2713; | &#x2713; |


> [!NOTE] 
> MSIXbundles não têm suporte em versões do Windows 10 1709 e 1803.  Os desenvolvedores que desejam para criar pacotes compatíveis com essas versões do Windows podem aproveitar a tecnologia. appxbundle.

## <a name="mdm-support"></a>Suporte ao MDM
Microsoft Intune e o SCCM oferece suporte à instalação MSIX em 1709 e 1803. MSIX pode ser instalado nessas compilações da mesma maneira possível no 1809 e versões posteriores. 

## <a name="store-support"></a>Suporte a Store
A versão mínima suportada do sistema operacional de um MSIX está listada no arquivo de manifesto do pacote como MinVersion no elemento TargetDeviceFamily. Por exemplo um pacote MSIX pode listar MinVersion = "10.0.17701.0" como a versão mínima com suporte, que significa que o MSIX pode executar sobre isso e mais tarde as versões do sistema operacional.

No 1709, 1803 e 1809 oferecemos suporte os cenários de implantação corporativos básicos. Eles incluem a instalação por meio de script do System Center Configuration Manager (SCCM), Microsoft Intune, por meio do PowerShell ou clique duas vezes em instalação.

Atualmente, a instalação MSIX por meio da Microsoft Store e a Microsoft Store para empresas exigem Windows 10 versão 1809.

## <a name="packaging--signing"></a>Empacotando e assinatura
No momento para empacotar e assinar um MSIX, você precisa do SDK de 1809. Empacotamento e a assinatura podem ser feitos por meio das ferramentas de linha de comando do SDK do Windows 10 (MakeAppx.exe e SignTool.exe), a ferramenta de empacotamento MSIX ou o Visual Studio. 

## <a name="auto-elevation"></a>Elevação automática
Em casos raros, alguns aplicativos exigem privilégios elevados. 

Os usuários que MSIX em execução em compilações posteriores 1809 pode usar privilégios elevados simplesmente clicando no bloco do aplicativo. A plataforma da detecta que a elevação é necessária e fornece automaticamente quando o usuário tem as credenciais necessárias. 

Em compilações 1709 e 1803, os usuários podem ainda executar aplicativos com privilégios elevados, no entanto, eles precisarão selecionar explicitamente para executar o aplicativo como um administrador. Uma maneira de fazer isso é clicando duas vezes no bloco do aplicativo e selecionando a opção "Executar como administrador".

## <a name="signature-verification"></a>Verificação de assinatura
Como mencionado anteriormente, impomos a assinatura apropriada dos pacotes MSIX em todas as versões do Windows 1709 e posteriores. No entanto, em que as versões de Windows mais antigas (1709 e 1803), os profissionais de TI precisa seguir algumas etapas adicionais para verificar a assinatura antes de liberar um aplicativo para seus usuários finais. Observe que, para o usuário final a experiência MSIX não altera de forma alguma. O usuário final continua a instalar, iniciar e usar o aplicativo como antes, já que a verificação da assinatura é tratada por profissionais de TI. 

Versões do Windows 10 1809 e posteriores, um profissional de TI podem verificar assinatura do aplicativo do PowerShell ou por meio das propriedades do pacote MSIX. 

No entanto, em versões 1709 e 1803 os profissionais de TI não é possível verificar a assinatura do pacote MSIX propriedades do, a menos que o SDK 1809 está instalado. Se o profissional de TI tiver o SDK 1809 em um dispositivo com Windows 1709 por meio do 1803, a assinatura pode ser verificada por meio de ferramentas do SDK do PowerShell. 
