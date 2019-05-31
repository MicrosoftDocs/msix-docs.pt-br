---
title: Suporte de pacote MSIX no Windows 10 versão 1709 e posterior
description: Instale pacotes MSIX no 1709 e posterior.
author: c-don
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, FUTUR, ferramenta de empacotamento MSIX, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bb0a4aaab44f9eed791200c3d31b04dbfe242ebf
ms.sourcegitcommit: bc3f2bf9fe105576d0cc047d95b3f0de36fbc8b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400676"
---
# <a name="msix-package-support-on-windows-10-version-1709-and-later"></a>Suporte de pacote MSIX no Windows 10 versão 1709 e posterior

Se você converter um aplicativo existente em MSIX, convém usar o aplicativo em versões do sistema operacional anteriores ao Windows 10, versão 1809 (build 17763). Este artigo descreve como dar suporte a seu pacote MSIX no Windows 10 versão 1709 (build 16299) e posterior.

## <a name="problem"></a>Problema

Portanto, você converteu o seu aplicativo existente para MSIX usando a ferramenta de empacotamento MSIX e funcionar bem no Windows 10, versão 1809 e posterior. Mas agora que você sabe que estamos adicionando suporte para MSIX iniciando compilação Windows 10 versão 1709, você deseja executar seu aplicativo MSIX neste ou em qualquer versão mais recente do Windows 10. Atualmente, se você tentar instalar o seu pacote MSIX em um computador com Windows 10 versão 1709 ou Windows 10 versão 1803, você obterá esta mensagem de erro:

![Instalação do PowerShell MSIX](images/mpt_blog_0.jpg)

## <a name="solution"></a>Solução

Estamos trabalhando para atualizar a ferramenta de empacotamento MSIX para lidar com isso, mas enquanto isso aqui está o que você pode para fazer para configurar seu aplicativo para ser executado nessas compilações.

1. Abra a ferramenta de empacotamento MSIX e clique em **editor de pacote**.
  ![open](images/mpt_blog_1.jpg)

2. Navegue até seu pacote MSIX e clique em **Abrir pacote**.
  ![Abrir pacote](images/mpt_blog_3.jpg)

3. Na parte inferior do **informações do pacote** guia, em **arquivo de manifesto**, clique em **abrir arquivo**.
  ![Abra o manifesto](images/mpt_blog_4.jpg)

4. No arquivo de manifesto, edite o **MinVersion** para igualar 16299, conforme mostrado abaixo.
  ![edit manifest2](images/mpt_blog_7.jpg)

5. Depois que você terminar de editar o manifesto, feche o arquivo. Você voltará para o **editor de pacote**. Não se esqueça de assinar novamente o pacote editado, conforme mostrado abaixo: ![entrada](images/mpt_blog_9.jpg)

6. Depois de concluir as atualizações, salve suas alterações.
  ![save](images/mpt_blog_10.jpg)

Neste ponto, você pode instalar o aplicativo em um computador executando o Windows 10, versão 1709 ou posterior.

## <a name="troubleshooting-tips"></a>Dicas para solução de problemas

Por enquanto, em computadores que executam o Windows 10, versão 1709 (build 16299) para compilar 17700, você pode instalar aplicativos MSIX por meio do PowerShell.
Especificamente, você precisa que esse comando no PowerShell:

```powershell
add-appxpackage <path to MSIX package>
```

![Comando do PowerShell](images/mpt_blog_11.jpg)

Você também pode usar o Intune, o SCCM ou a API do Gerenciador de empacotamento.