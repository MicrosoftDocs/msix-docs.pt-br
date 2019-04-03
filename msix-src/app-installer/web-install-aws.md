---
author: mcleanbyron
title: Instalam de pacotes de aplicativos no AWS para web hospedando o Windows 10 | Microsoft Docs
description: Tutorial para configurar o servidor de web do AWS validar a instalação do aplicativo por meio do instalador do aplicativo
ms.author: mcleans
ms.date: 05/30/2018
ms.topic: article
keywords: o Windows 10, Windows 10, UWP, fazer sideload do instalador, AppInstaller, aplicativo, relacionadas a pacotes definidos, opcionais, AWS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 015d7b18e541ba63cb4a8b41a38c4270d156c004
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900708"
---
# <a name="hosting-windows-10-app-packages-on-aws-for-web-install"></a>Instalam de pacotes de aplicativos no AWS para web hospedando o Windows 10

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

Este tópico descreve as etapas para configurar um site do Amazon Web Services (AWS) para hospedar pacotes de aplicativo do Windows 10 e como usar o aplicativo instalador de aplicativo para instalar os pacotes de aplicativo.

## <a name="setup"></a>Configuração

Para seguir com sucesso este tutorial, você precisará do seguinte:
 
1. Assinatura do AWS 
2. Página da Web
3. Pacote de aplicativo do Windows 10 - o pacote do aplicativo que você distribuirá

Opcional: [Projeto Starter](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tem um pacote do aplicativo ou página da Web para trabalhar, mas ainda quer aprender a usar esse recurso.

Este tutorial apresentará como configurar uma página da web e hospedar pacotes em AWS. Isso exigirá uma assinatura do AWS. Dependendo da escala de sua operação, você pode usar sua participação gratuita para acompanhar este tutorial. 

## <a name="step-1---aws-membership"></a>Etapa 1 - associação do AWS
Para obter uma associação do AWS, visite o [página de detalhes de conta do AWS](https://aws.amazon.com/free/). Para os fins deste tutorial, você pode usar uma associação gratuita.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Etapa 2: criar um bucket do Amazon S3

Amazon Simple Storage Service (S3) é uma oferta para coletar, armazenar e analisar dados AWS. S3 buckets são uma maneira conveniente de páginas da web para distribuição e pacotes de aplicativos de host Windows 10. 

Depois de fazer logon AWS com suas credenciais, sob `Services` localizar `S3`. 

Selecione **criar bucket**e insira um **nome do Bucket** para seu site. Siga os prompts de caixa de diálogo para definir as propriedades e permissões. Para garantir que seu aplicativo do Windows 10 pode ser distribuído do seu site, habilite **leitura** e **escrever** permissões para o bucket e selecione **conceder acesso de leitura público para esse recipiente**.

![Definir permissões no bucket do Amazon S3](images/aws-permissions.png) 

Examine o resumo para verificar se que as opções selecionadas são refletidas. Clique em **criar bucket** para concluir esta etapa. 

## <a name="step-3---upload-windows-10-app-package-and-web-pages-to-an-s3-bucket"></a>Etapa 3: carregar páginas de web e o pacote de aplicativo do Windows 10 para um bucket S3

Um você ter criado um bucket do Amazon S3, você poderá vê-lo no modo de exibição do Amazon S3. Aqui está um exemplo da aparência de nossa bucket de demonstração:

![Captura de tela do Amazon S3 bucket exibição](images/aws-post-create.png)

Agora estamos prontos para carregar os pacotes de aplicativos e páginas da web que gostaríamos de hospedar no nosso bucket do Amazon S3. 

Clique no bucket de recém-criado para carregar o conteúdo. O número de buckets está vazio, pois nada tiver sido carregado. Clique o **carregar** botão e selecione os pacotes de aplicativos e arquivos de página da web que você deseja carregar.

> [!NOTE]
> Você pode usar o pacote do aplicativo que faz parte do repositório de [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) fornecido no GitHub se não tiver um pacote do aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. O certificado deve ser instalado em seu dispositivo antes de instalar o aplicativo.

![Captura de tela de carregamento de pacote do aplicativo UX](images/aws-upload-package.png)

Assim como as permissões para a criação de um bucket do Amazon S3, o conteúdo no bucket também deve ter **leia**, **escrever**, e **conceder acesso de leitura público para esse objeto (s)** permissões.

Se você gostaria de testar o carregamento de uma página da web, mas não tiver uma, você pode usar a página html de exemplo (default. HTML) do [projeto Starter](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Antes de carregar a página da web, confirme se a referência de pacote de aplicativo em sua página da web está correta. 

Para obter a referência de pacote de aplicativo, carregue o pacote do aplicativo pela primeira vez e copie a URL do pacote de aplicativo. Edite a página da web html para refletir o caminho do pacote de aplicativo correto. Consulte o exemplo de código para obter mais detalhes. 

Selecione o arquivo de pacote do aplicativo carregado para obter o link de referência para o pacote do aplicativo, ele deve ser semelhante a este exemplo.

![Captura de tela do caminho de pacote carregado](images/aws-package-path.png)

**Cópia** o link para o aplicativo de pacote e adicionar a referência em sua página da web. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
Carregue o arquivo html para o bucket do Amazon S3. Lembre-se de definir as permissões para permitir **leia** e **gravar** acesso.

## <a name="step-4---test"></a>Etapa 4 - teste

Depois que a página da web é carregada para o bucket do Amazon S3, obter o link para a página da web, selecionando o arquivo html carregado.

Use o link para abrir a página da web. Uma vez que podemos definir permissões para conceder acesso público para o pacote do aplicativo e a página da web, qualquer pessoa com o link para a página da web poderá acessá-lo e instalar seus pacotes de aplicativo do Windows 10 usando o instalador do aplicativo. Observe que o instalador de aplicativo é parte da plataforma do Windows 10. Como desenvolvedor, você não precisará adicionar qualquer código adicional ou recursos ao seu aplicativo para habilitar o uso do instalador do aplicativo. 

## <a name="troubleshooting"></a>Solução de problemas

### <a name="app-installer-fails-to-install"></a>Falha na instalação do instalador de aplicativo 

Instalação do aplicativo falhará se o certificado assinado com o pacote do aplicativo não estiver instalado no dispositivo. Para corrigir isso, você precisará instalar o certificado antes da instalação do aplicativo. Se você estiver hospedando um pacote do aplicativo para distribuição pública, é recomendável para assinar seu pacote de aplicativo com um certificado de autoridade de certificação. 

