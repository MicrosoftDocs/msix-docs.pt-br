---
title: Suporte do MSIX no Windows 10 versão 1709 e 1803
description: Este artigo resume o suporte para o MSIX com as atualizações mais recentes de 22/1/2019.
ms.date: 04/04/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX, 1709, 1803
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 700656bb7a6de66a876d8d46a9ac724d01caa11a
ms.sourcegitcommit: 073a228653f004914851c3461b9ad6eef343f915
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74309029"
---
# <a name="msix-support-on-windows-10-version-1709-and-1803"></a>Suporte do MSIX no Windows 10 versão 1709 e 1803

Este artigo resume o suporte do MSIX e as limitações com as atualizações mais recentes.

Devido à demanda popular, adicionamos suporte ao MSIX em mais versões do Windows 10. Especialmente, isso abrange as versões 1709 e 1803. Esse suporte permite aos usuários implantar pacotes MSIX em versões anteriores do Windows, aproveitando todos os benefícios do MSIX, incluindo transporte em contêineres e segurança por meio de certificados. Para aproveitar essas atualizações, verifique se você tem a versão 1.0.30311 ou posterior do Instalador de Aplicativo e o Windows 10 atualização 1709 ou posterior. 

Abaixo, abordaremos os recursos e as limitações do suporte do MSIX em versões anteriores do sistema operacional.

##  <a name="msix-double-click-support"></a>Suporte de clique duplo do MSIX

Um dos benefícios da implantação de um pacote MSIX no Windows 10 versão 1809 e posterior é que o usuário pode instalar o pacote clicando nele. Com a última versão do Instalador de Aplicativo (1.0.30311.0), a capacidade de instalar um pacote MSIX clicando nele também está disponível no Windows 10 versões 1709 e 1803.

Clicar no pacote fornece o mesmo suporte de instalação como no 1809, ou seja, o Instalador de Aplicativo orienta o usuário pela instalação do aplicativo. O Instalador de Aplicativo é pré-instalado e obtém as atualizações da Microsoft Store; portanto, podemos sempre levar a você a melhor experiência de instalação.

O Instalador de Aplicativo pode ser baixado para uso offline na empresa por meio do [portal da Web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) da Microsoft Store para Empresas. Saiba mais sobre a distribuição offline [aqui](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

Veja abaixo um resumo do suporte do MSIX de Clique para instalar disponível no Windows 10 versões 1709 e 1803.

### <a name="support-matrix"></a>Matriz de suporte

| Versão do Sistema Operacional|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Windows 10, versão 1709 (build 16299) | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Windows 10, versão 1803 (build 17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Windows 10, versão 1809 (build 17763) e posterior | &#x2713; | &#x2713; | &#x2713; | &#x2713; |

> [!NOTE]
> Não há suporte para o formato .msixbundle no Windows 10, versões 1709 e 1803.  Os desenvolvedores que buscam criar pacotes compatíveis com essas versões do Windows podem aproveitar o formato .appxbundle.

## <a name="mdm-support"></a>Suporte ao MDM

O Microsoft Intune e o SCCM (System Center Configuration Manager) dão suporte à instalação do MSIX no Windows 10, versões 1709 e 1803. Os pacotes MSIX podem ser instalados nessas versões da mesma forma como no Windows 10 versão 1809 e posterior.

## <a name="store-support"></a>Suporte à Store

A versão mínima compatível do sistema operacional de um pacote MSIX está listada no arquivo de manifesto do pacote como `MinVersion` no elemento `TargetDeviceFamily`. Por exemplo, um pacote MSIX pode listar `MinVersion="10.0.17701.0"` como a versão mínima compatível, o que significa que o pacote MSIX pode ser executado nessa versão e em versões posteriores do sistema operacional.

No Windows 10, versões 1709, 1803 e 1809, damos suporte aos cenários de implantação corporativos convencionais. Eles incluem a instalação por meio do SCCM, do Microsoft Intune, do PowerShell ou da instalação de clique duplo.

Atualmente, a instalação do MSIX por meio da Microsoft Store e da Microsoft Store para Empresas exige o Windows 10 versão 1803.

## <a name="packaging-and-signing"></a>Empacotamento e assinatura

No momento, para empacotar e assinar um arquivo MSIX, é necessário ter o SDK do Windows 10 versão 1809. O empacotamento e a assinatura podem ser feitos por meio das ferramentas de linha de comando do SDK do Windows 10 (MakeAppx.exe e SignTool.exe), da Ferramenta de Empacotamento MSIX ou do Visual Studio.

## <a name="auto-elevation"></a>Elevação automática

Em casos raros, alguns aplicativos exigem privilégios elevados.

Os usuários que executam um pacote MSIX no Windows 10 versão 1809 e posterior podem usar privilégios elevados apenas clicando no bloco do aplicativo. A plataforma detecta que a elevação é necessária e a fornece automaticamente quando o usuário tem as credenciais necessárias.

No Windows 10 versões 1709 e 1803, os usuários ainda podem executar aplicativos com privilégios elevados. No entanto, eles precisam selecionar explicitamente para executar o aplicativo como administrador. Uma maneira de fazer isso é clicando com o botão direito do mouse no bloco do aplicativo e selecionando a opção "Executar como administrador".

## <a name="digital-certificate-verification"></a>Verificação de certificados digitais

Conforme mencionado anteriormente, impomos a assinatura apropriada dos pacotes MSIX no Windows 10 versão 1709 e posterior. No entanto, no Windows 10 versões 1709 e 1803, os profissionais de TI precisam seguir algumas etapas extras para verificar os certificados digitais antes de liberar um aplicativo para os usuários finais. Para o usuário final, a experiência do MSIX não é alterada de forma alguma. O usuário final continua instalando, iniciando e usando o aplicativo como antes, já que a verificação do certificado é feita pelo profissional de TI.

No Windows 10 versão 1809 e posterior, um profissional de TI pode verificar o certificado digital de um pacote MSIX por meio do PowerShell ou das propriedades do pacote MSIX.

No entanto, no Windows 10 versões 1709 e 1803, os profissionais de TI não podem verificar o certificado por meio das propriedades do pacote MSIX, a menos que o SDK do Windows 10 versão 1809 esteja instalado. Se o profissional de TI tiver o SDK do Windows 10 versão 1809 em um dispositivo com o Windows 10 versão 1709 a 1803, a assinatura e o certificado poderão ser verificados por meio das ferramentas do SDK no PowerShell.
