---
title: Distribuir um aplicativo do Windows 10 de um aplicativo web do Azure
description: Este tutorial demonstra como configurar um servidor Web do Azure. Verifique se o aplicativo Web pode hospedar pacotes de aplicativo host, invocar e usar o Instalador de aplicativo de maneira eficaz.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais, servidor Web do Azure
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 63c7163c9db550293da93b56434694c742462562
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828808"
---
# <a name="distribute-a-windows-10-app-from-an-azure-web-app"></a>Distribuir um aplicativo do Windows 10 de um aplicativo web do Azure

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

Este tópico descreve as etapas para configurar um servidor de Web do Azure para hospedar pacotes de aplicativo do Windows 10 e como usar o aplicativo instalador de aplicativo para instalar os pacotes de aplicativo.

Neste tutorial, veremos como configurar um servidor IIS para verificar localmente se o aplicativo da Web pode hospedar corretamente os pacotes de aplicativos, invocar e usar o aplicativo de Instalador de aplicativo de forma eficiente. Também há tutoriais para hospedar seus aplicativos Web corretamente nos serviços Web populares na nuvem no campo (Azure e AWS) para garantir que ele atendam aos requisitos de instalação Web do Instalador de aplicativo. Este tutorial passo a passo não exige nenhum conhecimento e é muito fácil de seguir. 

## <a name="setup"></a>Configuração

Para seguir com sucesso este tutorial, você precisará do seguinte:
 
1. Assinatura do Microsoft Azure 
2. Pacote de aplicativo do Windows 10 - o pacote do aplicativo que você distribuirá

Opcional: [Projeto Starter](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tem um pacote do aplicativo ou página da Web para trabalhar, mas ainda quer aprender a usar esse recurso.

### <a name="step-1---get-an-azure-subscription"></a>Etapa 1: obter uma assinatura do Azure
Para obter uma assinatura do Azure, acesse a [página da conta do Azure](https://azure.microsoft.com/free/). Para os fins deste tutorial, você pode usar uma associação gratuita.

### <a name="step-2---create-an-azure-web-app"></a>Etapa 2: criar um aplicativo Web do Azure 
Na página de Portal do Azure, clique no botão **Criar um recurso** e, em seguida, selecione **Aplicativo Web**

![Captura de tela da criação de um aplicativo do Azure](images/azure-create-app.png)

Crie um **Nome do aplicativo** único e deixar o restante dos campos como padrão. Clique em **Criar** para concluir o Assistente de criação de aplicativo Web. 

![Captura de tela de criação de um aplicativo web](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>Etapa 3: hospedar o pacote do aplicativo e a página da Web 
Depois que o aplicativo Web é criado, você pode acessá-lo no painel no Portal do Azure. Nesta etapa, vamos criar uma página da Web simples com a GUI do Portal do Azure.

Depois de selecionar o aplicativo Web recém-criado no painel, use o campo de pesquisa para localizar e abrir o **Editor de serviço de aplicativo**. 

No editor, há um arquivo padrão `hostingstart.html`. Clique com botão direito do mouse no espaço vazio do painel do Explorador de arquivos e selecione **Carregar arquivos** para começar a carregar seus pacotes de aplicativo.

> [!NOTE]
> Você pode usar o pacote do aplicativo que faz parte do repositório de [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) fornecido no GitHub se não tiver um pacote do aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. O certificado deve ser instalado em seu dispositivo antes de instalar o aplicativo.

![Captura de tela de carregamento de pacotes](images/azure-upload-file.png)

Clique com botão direito do mouse no espaço vazio do painel do Explorador de arquivos e selecione **Novos arquivos** para criar um novo arquivo. Nome do arquivo: `default.html`.

Se você estiver usando o pacote do aplicativo fornecido no [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp), copie o seguinte código HTML para a página da Web `default.html` recém-criada. Se estiver usando seu próprio pacote do aplicativo, modifique a URL do serviço de aplicativo (a URL depois de `source=`). Você pode obter a URL do serviço de aplicativo na página de visão geral do aplicativo no Portal do Azure.

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>Etapa 4: configurar o aplicativo Web para tipos MIME do pacote do aplicativo

Adicione um novo arquivo ao aplicativo Web chamado: `Web.config`. Abra o arquivo `Web.config` do explorador e adicione as seguintes linhas. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
  </system.webServer>
</configuration>
```

### <a name="step-5---run-and-test"></a>Etapa 5: executar e testar

Para iniciar a página da Web que você criou, use a URL da etapa 3 no navegador seguida por `/default.html`. 

![Captura de tela de instalação do aplicativo de página da Web](images/edge.png)

Clique em "Instalar meu aplicativo de exemplo", para iniciar o Instalador de Aplicativo e instalar o pacote do aplicativo. 

## <a name="troubleshooting-issues"></a>Solução de problemas

### <a name="app-installer-app-fails-to-install"></a>Falha na instalação do aplicativo pelo Instalador de Aplicativo 
Ocorre falha de instalação do aplicativo se o certificado usado pelo pacote do aplicativo não está instalado no dispositivo. Para corrigir isso, você precisará instalar o certificado antes da instalação do aplicativo. Se estiver hospedando um pacote do aplicativo para distribuição pública, é recomendado que o pacote do aplicativo usar um certificado de autoridade de certificação. 

![Captura de tela de falha de certificação](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>Nada acontece quando você clica no link 
Certifique-se de que o Instalador de Aplicativo está instalado. Acesse **Configurações** -> **Aplicativos e recursos** e encontre o Instalador de Aplicativo na lista de aplicativos instalados. 
