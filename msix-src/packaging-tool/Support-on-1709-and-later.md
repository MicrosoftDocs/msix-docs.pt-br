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
ms.openlocfilehash: bf737f108cc3c248fe5008a27fde15b79abfbaf9
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795464"
---
# <a name="msix-package-support-on-windows-10-1709-and-later"></a>Suporte de pacote MSIX no Windows 10 1709 e posterior

Se você converter um aplicativo existente em MSIX, convém usar o aplicativo em versões anteriores do Windows que 1809 (build 17701). Esta postagem de blog descreve como habilitar esses aplicativos em compilações mais cedo 16299, também conhecido como o Windows 10 versão 1709. 
 
 
## <a name="problem"></a>Problema:
Portanto, você converteu o seu aplicativo existente para MSIX usando a ferramenta de empacotamento MSIX e funcionar bem no 1809 e versões posteriores. Mas agora que você sabe que estamos adicionando suporte para MSIX iniciando build 16299, você deseja executar seu aplicativo MSIX sobre isso, ou qualquer compilação mais tarde. Atualmente, se você tentar instalar seu aplicativo MSIX em um computador com o build 16299 para 17700, você obterá esta mensagem de erro: 

![Instalação do PowerShell MSIX](images/mpt_blog_0.jpg)

## <a name="solution"></a>Solução:
Estamos trabalhando para atualizar a ferramenta de empacotamento MSIX para lidar com isso, mas enquanto isso aqui está o que você pode para fazer para configurar seu aplicativo para ser executado nessas compilações:
 
Abra a ferramenta de empacotamento MSIX e navegue até o editor de pacote.

![open](images/mpt_blog_1.jpg) 
![navigate](images/mpt_blog_2.jpg)


Navegue até seu pacote MSIX. No meu caso, ele é o pacote de Test_App.msix. Clique em "Abrir pacote"

![Abrir pacote](images/mpt_blog_3.jpg)

Na parte inferior da guia "Informações de pacote", observe a opção para abrir o arquivo de manifesto. 

![Abra o manifesto](images/mpt_blog_4.jpg)

Selecione "Abrir arquivo" e edite MinVersion para igualar 16299 conforme mostrado abaixo:

![observar manifesto](images/mpt_blog_5.jpg)
![Editar manifesto](images/mpt_blog_6.jpg)
![Editar manifesto2](images/mpt_blog_7.jpg)

Depois que você terminar de editar o manifesto, feche o arquivo. Isso retornará ao Editor de pacote.
Não se esqueça de assinar novamente o pacote editado, conforme mostrado abaixo:

![sign](images/mpt_blog_9.jpg)

Depois que a atualização foi feita, salve suas alterações.

![salvar](images/mpt_blog_10.jpg)

Neste ponto, você pode instalar o aplicativo em um dispositivo com o build 16299 ou posterior.
 


 
## <a name="troubleshooting-tips"></a>Dicas de solução de problemas:
Por enquanto, em dispositivos que executam builds 16299 para 17700, você pode instalar aplicativos MSIX por meio do PowerShell. Especificamente, você precisa que esse comando no PowerShell: Adicionar-appxpackage <path to MSIX package>

![Comando do PowerPoint](images/mpt_blog_11.jpg)

Você também pode usar o Intune, o SCCM ou a API do Gerenciador de empacotamento.



