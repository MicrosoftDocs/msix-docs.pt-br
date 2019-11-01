---
title: Suporte ao pacote MSIX no Windows 10 versão 1709 e posterior
description: Este artigo descreve como dar suporte ao pacote MSIX no Windows 10 versão 1709 (Build 16299) e posterior.
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, Ferramenta de Empacotamento MSIX, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4dbb6fdb5893835475d79a8052e35fc902680425
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328760"
---
# <a name="msix-package-support-on-windows-10-version-1709-and-later"></a>Suporte ao pacote do MSIX no Windows 10 versão 1709 e posterior

Se você converter um aplicativo existente em MSIX, o ideal será usar o aplicativo em versões do sistema operacional anteriores ao Windows 10, versão 1809 (build 17763). Este artigo descreve como dar suporte ao pacote MSIX no Windows 10 versão 1709 (build 16299) e posterior.

## <a name="msix-packaging-tool-version-120197010-and-later"></a>Ferramenta de empacotamento MSIX versão 1.2019.701.0 e posterior

Ao converter um aplicativo com a ferramenta de empacotamento MSIX na versão 1.2019.701.0 e posterior, a ferramenta de empacotamento tem a opção de definir a versão mínima como 1709.  Isso pode ser habilitado definindo o padrão de ferramenta a seguir.

1. Iniciar a ferramenta de empacotamento MSIX

2. Clique na engrenagem **configurações** no canto superior direito

3. Selecionar **padrões de ferramenta** no painel de navegação

4. Desmarque **impor Microsoft Store requisitos de controle de versão**

5. Clique em **salvar configurações**

Isso definirá a versão mínima para o Windows 1709 para conversões futuras.  Ele não alterará a versão mínima ao editar um pacote no editor de pacote.  


## <a name="problem"></a>Problema

Você converteu seu aplicativo existente para MSIX usando a ferramenta de empacotamento MSIX anterior ao 1.2019.701.0, importou Microsoft Store controle de versão no requiremnts ou usou outra ferramenta para criar seu pacote que não definiu a versão mínima para 10.0.16299.0 (compilação do Windows 10 1709 número).  Você deseja executar seu aplicativo MSIX no Windows 10 1709 ou em qualquer versão posterior do Windows 10. Atualmente, se você apenas tentar instalar o pacote MSIX em um computador com o Windows 10 versão 1709 ou o Windows 10 versão 1803, você receberá esta mensagem de erro:

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

## <a name="troubleshooting-tips"></a>Dicas para solução de problemas

Por enquanto, em computadores que executam o Windows 10, versão 1709 (build 16299 ao 17700), você poderá instalar aplicativos MSIX por meio do PowerShell.
Especificamente, você precisará deste comando no PowerShell:

```powershell
add-appxpackage <path to MSIX package>
```

![Comando do PowerShell](images/mpt_blog_11.jpg)

Use também o Intune, o SCCM ou a API do Gerenciador de Empacotamento.
