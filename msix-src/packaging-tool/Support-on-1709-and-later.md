---
title: Suporte ao pacote MSIX no Windows 10 versão 1709 e posterior
description: Instale pacotes MSIX na versão 1709 e posterior.
author: c-don
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, Ferramenta de Empacotamento MSIX, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bb0a4aaab44f9eed791200c3d31b04dbfe242ebf
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "66400676"
---
# <a name="msix-package-support-on-windows-10-version-1709-and-later"></a>Suporte ao pacote do MSIX no Windows 10 versão 1709 e posterior

Se você converter um aplicativo existente em MSIX, o ideal será usar o aplicativo em versões do sistema operacional anteriores ao Windows 10, versão 1809 (build 17763). Este artigo descreve como dar suporte ao pacote MSIX no Windows 10 versão 1709 (build 16299) e posterior.

## <a name="problem"></a>Problema

Você converteu seu aplicativo existente em MSIX usando a Ferramenta de Empacotamento MSIX e ele está funcionando bem no Windows 10, versão 1809 e posterior. Mas agora que você sabe que estamos adicionando suporte ao MSIX no build do Windows 10 versão 1709, você deseja executar seu aplicativo MSIX nessa ou em qualquer versão posterior do Windows 10. Atualmente, se você apenas tentar instalar o pacote MSIX em um computador com o Windows 10 versão 1709 ou o Windows 10 versão 1803, você receberá esta mensagem de erro:

![Instalação do MSIX por meio do PowerShell](images/mpt_blog_0.jpg)

## <a name="solution"></a>Solução

Estamos trabalhando para atualizar a Ferramenta de Empacotamento MSIX para resolver esse problema, mas enquanto isso, veja a seguir o que você pode fazer para configurar seu aplicativo para que ele seja executado nesses builds.

1. Abra a Ferramenta de Empacotamento MSIX e clique em **Editor de pacote**.
  ![open](images/mpt_blog_1.jpg)

2. Procure o pacote MSIX e clique em **Abrir pacote**.
  ![abrir pacote](images/mpt_blog_3.jpg)

3. Na parte inferior da guia **Informações do Pacote**, em **Arquivo de manifesto**, clique em **Abrir arquivo**.
  ![abrir manifesto](images/mpt_blog_4.jpg)

4. No arquivo de manifesto, edite a **MinVersion** para que seja igual a 16299, conforme mostrado abaixo.
  ![editar manifesto2](images/mpt_blog_7.jpg)

5. Depois de terminar de editar o manifesto, feche o arquivo. Você retornará ao **Editor de pacote**. Não se esqueça de assinar novamente o pacote editado, conforme mostrado abaixo: ![assinar](images/mpt_blog_9.jpg)

6. Depois de concluir as atualizações, salve as alterações.
  ![salvar](images/mpt_blog_10.jpg)

Neste ponto, você pode instalar o aplicativo em um computador que executa o Windows 10, versão 1709 ou posterior.

## <a name="troubleshooting-tips"></a>Dicas de solução de problemas

Por enquanto, em computadores que executam o Windows 10, versão 1709 (build 16299 ao 17700), você poderá instalar aplicativos MSIX por meio do PowerShell.
Especificamente, você precisará deste comando no PowerShell:

```powershell
add-appxpackage <path to MSIX package>
```

![Comando do PowerShell](images/mpt_blog_11.jpg)

Use também o Intune, o SCCM ou a API do Gerenciador de Empacotamento.