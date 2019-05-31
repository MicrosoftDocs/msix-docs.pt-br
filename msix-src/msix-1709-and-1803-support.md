---
author: c-don
title: Suporte MSIX no Windows 10 versão 1709 e 1803
description: Este artigo resume o suporte para MSIX com nossas atualizações mais recentes a partir de 22/1/2019.
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: Windows 10, uwp, msix, 1709, 1803
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 37fd4538d7942f92316139f9187a4016997a0048
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400732"
---
# <a name="msix-support-on-windows-10-version-1709-and-1803"></a>Suporte MSIX no Windows 10 versão 1709 e 1803

Este artigo resume o suporte MSIX e limitações com nossas atualizações mais recentes.

Por demanda popular, adicionamos suporte para MSIX em mais versões do Windows 10. Notavelmente, isso abrange as versões 1709 e 1803. Esse suporte permite aos usuários implantar pacotes MSIX em versões anteriores do Windows, aproveitando as vantagens de todos os benefícios do MSIX, incluindo o uso de contêineres e segurança por meio de certificados. Para tirar proveito dessas atualizações, verifique se você tiver o 1.0.30311 ou versão posterior do instalador de aplicativo e estar na atualização do Windows 10 1709 ou posterior. 

Abaixo, discutiremos recursos e limitações de suporte MSIX em versões anteriores do sistema operacional.

##  <a name="msix-double-click-support"></a>Suporte a dois cliques MSIX

Um dos benefícios de implantar um pacote do MSIX no Windows 10 versão 1809 e posteriores é que o usuário pode instalar o pacote clicando nele. Com a versão mais recente do instalador do aplicativo (1.0.30311.0) a capacidade de instalar um pacote MSIX clicando nele está disponível no Windows 10 versão 1709 e 1803 também.

Clicar no pacote fornece o mesmo suporte de instalação, como no 1809, ou seja, o instalador do aplicativo orienta o usuário a instalação do aplicativo. O instalador de aplicativo é pré-instalado e obtém as atualizações da Microsoft Store, portanto, podemos pode sempre colocar você a melhor experiência de instalação.

O instalador de aplicativo podem ser baixado para uso offline na empresa da Microsoft Store para empresas [portal da web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1). Você pode aprender mais sobre a distribuição off-line [aqui](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

Abaixo está um resumo do suporte à instalação clique MSIX disponíveis no Windows 10 versão 1709 e 1803.

### <a name="support-matrix"></a>Matriz de suporte

| Versão do Sistema Operacional|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Windows 10, versão 1709 (build 16299) | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Windows 10, versão 1803 (build 17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Windows 10, versão 1809 (build 17763) e posterior | &#x2713; | &#x2713; | &#x2713; | &#x2713; |

> [!NOTE]
> Não há suporte para o formato .msixbundle no Windows 10 versão 1709 e 1803.  Os desenvolvedores que desejam para criar pacotes compatíveis com essas versões do Windows podem aproveitar o formato. appxbundle.

## <a name="mdm-support"></a>Suporte ao MDM

Microsoft Intune e o System Center Configuration Manager (SCCM) oferece suporte à instalação MSIX no Windows 10 versão 1709 e 1803. Pacotes MSIX podem ser instalados nessas versões da mesma forma como é possível em Windows 10 versão 1809 e posterior.

## <a name="store-support"></a>Suporte a Store

A versão mínima com suporte do sistema operacional de um pacote MSIX está listado no arquivo de manifesto do pacote como `MinVersion` no `TargetDeviceFamily` elemento. Por exemplo um pacote MSIX pode listar `MinVersion="10.0.17701.0"` como a versão mínima com suporte, o que significa que o pacote MSIX pode ser executado sobre isso e mais tarde as versões do sistema operacional.

Em versões do Windows 10 1709 e 1803 1809, oferecemos suporte os cenários de implantação corporativos básicos. Eles incluem a instalação por meio do SCCM, o Microsoft Intune, o PowerShell ou clique duas vezes em instalação.

Atualmente, a instalação MSIX por meio da Microsoft Store e a Microsoft Store para empresas exigem Windows 10 versão 1809.

## <a name="packaging-and-signing"></a>Empacotamento e a assinatura

No momento, para empacotar e assinar um arquivo MSIX, você precisa do Windows 10 versão 1809 SDK. Empacotamento e a assinatura podem ser feitos por meio das ferramentas de linha de comando do SDK do Windows 10 (MakeAppx.exe e SignTool.exe), a ferramenta de empacotamento de MSIX, ou o Visual Studio.

## <a name="auto-elevation"></a>Elevação automática

Em casos raros, alguns aplicativos exigem privilégios elevados.

Usuários que executam um pacote MSIX no Windows 10 versão 1809 e posterior podem usar privilégios elevados, basta clicar no bloco do aplicativo. A plataforma detecta que a elevação é necessária e fornece automaticamente quando o usuário tem as credenciais necessárias.

Em versões do Windows 10 1709 e 1803, os usuários ainda podem executar aplicativos com privilégios elevados. No entanto, eles precisam selecionar explicitamente para executar o aplicativo como administrador. Uma maneira de fazer isso é clicando duas vezes no bloco do aplicativo e selecionando a opção "Executar como administrador".

## <a name="digital-certificate-verification"></a>Verificação do certificado digital

Como mencionado anteriormente, impomos a assinatura apropriada dos pacotes MSIX no Windows 10 versão 1709 e posterior. No entanto, em versões do Windows 10 1709 e 1803, os profissionais de TI precisam seguir algumas etapas adicionais para verificar os certificados digitais antes de liberar um aplicativo para seus usuários finais. Para o usuário final, a experiência MSIX não altera de forma alguma. O usuário final continua a instalar, iniciar e usar o aplicativo como antes, já que a verificação do certificado é tratada por profissionais de TI.

No Windows 10 versão 1809 e posterior, um profissional de TI pode verificar o certificado digital de um pacote MSIX do PowerShell ou por meio das propriedades do pacote MSIX.

No entanto, em versões do Windows 10 1709 e 1803, os profissionais de TI não é possível verificar o certificado a partir propriedades do pacote MSIX, a menos que o Windows 10 versão 1809 SDK está instalado. Se o profissional de TI tem a versão do Windows 10 que SDK 1809 em um dispositivo com Windows 10 versão 1709 por meio do 1803, a assinatura pode ser o certificado pode ser verificado por meio de ferramentas do SDK do PowerShell.
