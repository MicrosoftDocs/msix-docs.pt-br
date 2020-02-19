---
description: Este artigo descreve o processo completo para criar um pacote de aplicativo moderno do Windows 10 para o Windows Forms, WPF, ou jogo ou aplicativo Win32.
title: Aplicativos da área de trabalho do pacote
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf8ca8e769683ac2c66f558ef4e85f36671d42ce
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072606"
---
# <a name="package-desktop-applications-msix"></a>Empacotar aplicativos de área de trabalho (MSIX)

Pegue seu aplicativo da área de trabalho existente e adicione experiências modernas para usuários do Windows 10. Em seguida, obtenha maior alcance nos mercados internacionais ao distribuí-lo pela Microsoft Store. Você pode monetizar seu aplicativo de maneiras muito mais simples, aproveitando os recursos integrados da Microsoft Store. Obviamente, você não precisa usar a Microsoft Store. Fique à vontade para usar seus canais existentes.

Quando você cria um pacote MSIX para o aplicativo da área de trabalho, ele recebe uma identidade e, com essa identidade, o aplicativo da área de trabalho tem acesso às APIs da UWP (Plataforma Universal do Windows). Você pode usá-las para facilitar experiências envolventes e modernas, como blocos dinâmicos e notificações. Use a compilação condicional simples e as verificações de tempo de execução para executar código UWP somente quando o aplicativo for executado no Windows 10.

Além do código usado para facilitar experiências do Windows 10 o aplicativo permanece inalterado e você pode continuar a distribuí-lo para os usuários em versões anteriores do Windows. No Windows 10, o aplicativo continua a ser executado no modo de usuário com confiança assim como atualmente.

## <a name="prepare"></a>Preparar

Primeiro, prepare o aplicativo depois de analisar o artigo [Preparar para empacotar seu aplicativo da área de trabalho](desktop-to-uwp-prepare.md) e então resolva todos os problemas relevantes para o aplicativo antes de criar um pacote de aplicativo do Windows para ele. Talvez você não precise fazer muitas alterações no aplicativo antes de criar o pacote. No entanto, há algumas situações nas quais poderá ser necessário ajustar o aplicativo antes de criar um pacote para ele.

<a id="convert" />

## <a name="build-an-msix-from-source-code-using-visual-studio"></a>Criar um MSIX do código-fonte usando o Visual Studio

Caso mantenha o aplicativo usando o Visual Studio e o aplicativo não tenha um instalador ou o instalador não execute tarefas muito complicadas, considere usar o Visual Studio em vez disso.

O Visual Studio facilita a criação de um pacote. Você adicionará um **Projeto de Pacote de Aplicativos do Windows** à solução, fará referência ao projeto de área de trabalho e pressionará F5 para depurar o aplicativo. Estas são algumas outras coisas que você pode fazer com ele.

:heavy_check_mark: Gerar ativos visuais automaticamente.

:heavy_check_mark: Fazer alterações no manifesto usando um designer visual.

:heavy_check_mark: Gerar o pacote usando um assistente.

:heavy_check_mark: Atribua com facilidade uma identidade ao aplicativo com um nome já reservado no [Partner Center](https://partner.microsoft.com/dashboard).

Para obter instruções, veja [Empacotar um aplicativo da área de trabalho usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md). A tabela a seguir mostra as versões compatíveis do Visual Studio, Windows 10 e os formatos de pacote.

|  Versões compatíveis do Visual Studio | Versões do sistema operacional compatíveis com a criação de pacotes  | Versões do sistema operacional compatíveis com os pacotes instalados  |  Formatos de pacote compatíveis  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5 e posterior       |  Windows 10, versão 1607 e posterior           |  Windows 10, versão 1607 e posterior            |  .msix (para Windows 10, versão 1709 e posterior)<br/>.appx (para Windows 10, versão 1607 e posterior)                 |


## <a name="add-modern-windows-10-experiences"></a>Adicionar experiências modernas do Windows 10

Depois de criar um pacote MSIX para o aplicativo de área de trabalho, é possível usar APIs da UWP, extensões de pacote e componentes UWP para possibilitar experiências modernas e envolventes do Windows 10 como notificações e blocos dinâmicos.

### <a name="enhance-with-uwp-apis"></a>Aprimore com as APIs da UWP

Depois de preparar o pacote do aplicativo, será possível aprimorá-lo com recursos como blocos dinâmicos e notificações por push. Alguns desses recursos podem melhorar significativamente o nível de envolvimento de seu aplicativo e levam muito pouco tempo para adicionar. Alguns aprimoramentos exigem um pouco mais de código.

Consulte [Usar as APIs da UWP em aplicativos de área de trabalho](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

### <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Caso o aplicativo precise se integrar ao sistema (por exemplo, estabelecer regras de firewall), descreva os itens no manifesto do pacote do aplicativo e o sistema vai se encarregar do resto. Não é necessário escrever códigos para a maioria dessas tarefas. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário fizer logon, integrar o aplicativo no Explorador de Arquivos e adicionar nele uma lista de destinos de impressão que aparecem em outros aplicativos.

Consulte [Integrar o aplicativo da área de trabalho com as extensões de pacote](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions).

### <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de aplicativo moderno. Normalmente, primeiro é necessário determinar se é possível adicionar sua experiência [Aprimorando](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance) o aplicativo da área de trabalho existente com APIs UWP. Caso seja necessário usar um componente UWP para obter a experiência, será possível adicionar um projeto UWP à solução e usar os serviços de aplicativo para fazer a comunicação entre o aplicativo da área de trabalho e o componente UWP.

Veja [Estender seu aplicativo da área de trabalho com componentes UWP](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

## <a name="test"></a>Teste

Para testar o aplicativo em um ambiente realista durante a preparação para distribuição, é melhor assinar o aplicativo e instalá-lo. Veja [Testar o aplicativo](desktop-to-uwp-debug.md#test-your-app).

>[!IMPORTANT]
> Se planeja publicar o aplicativo na Microsoft Store, garanta que ele esteja operando corretamente em dispositivos com o Windows 10 no modo S. Esse não é um requisito da Microsoft Store. Veja [Testar o aplicativo do Windows no Windows 10 no modo S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validar

Para aumentar as chances de seu aplicativo ser publicado na Microsoft Store ou obter a [Certificação do Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666), valide e teste-o localmente antes de enviá-lo para certificação.

Se estiver usando o DAC para empacotar o aplicativo, é possível usar o novo sinalizador ``-Verify`` para validar o pacote em relação ao aplicativo da área de trabalho empacotado e os requisitos da Microsoft Store. Veja [Empacotar um aplicativo, assiná-lo e prepará-lo para envio para a Microsoft Store](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Se estiver usando o Visual Studio, será possível validar o aplicativo no assistente **Criar Pacotes do Aplicativo**. Veja [Criar um arquivo de upload de pacote do aplicativo](../package/packaging-uwp-apps.md#generate-an-app-package-upload-file-for-store-submission).

Para executar a ferramenta manualmente, consulte [Kit de Certificação de Aplicativos Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit).

Para revisar a lista de testes que o Kit de Certificação de Aplicativos Windows usa para validar o aplicativo, consulte [Testes de aplicativo da Ponte de Desktop do Windows](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests).

## <a name="distribute"></a>Distribuir

Você pode distribuir o aplicativo publicando na Microsoft Store ou por sideload em outros sistemas.

Veja [Distribuir um aplicativo de área de trabalho empacotado](/windows/apps/desktop/modernize/desktop-to-uwp-distribute).

