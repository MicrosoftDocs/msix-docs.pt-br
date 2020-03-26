---
title: Distribuir um aplicativo do Windows 10 por um serviço Web AWS
description: Tutorial para configurar o servidor Web AWS para validar a instalação do aplicativo por meio do aplicativo instalador de aplicativos
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, instalador de aplicativos, AppInstaller, Sideload, conjunto relacionado, pacotes opcionais, AWS
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 4e999f4939432c6490da380c722c31ae87d669d8
ms.sourcegitcommit: f5936c95c0f5b6f080e51b8d47a7cd62ccf6a600
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80241949"
---
# <a name="distribute-a-windows-10-app-from-an-aws-web-service"></a>Distribuir um aplicativo do Windows 10 por um serviço Web AWS

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

Este tópico descreve as etapas para configurar um site Amazon Web Services (AWS) para hospedar pacotes de aplicativos do Windows 10 e como usar o aplicativo instalador de aplicativos para instalar os pacotes de aplicativos.

## <a name="setup"></a>Instalação

Para seguir com sucesso este tutorial, você precisará do seguinte:
 
1. Assinatura do AWS 
2. Página da Web
3. Pacote de aplicativos do Windows 10-o pacote do aplicativo que será distribuído

Opcional: [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tem um pacote do aplicativo ou página da Web para trabalhar, mas ainda quer aprender a usar esse recurso.

Este tutorial abordará como configurar uma página da Web e pacotes de host em AWS. Isso exigirá uma assinatura do AWS. Dependendo da escala da operação, você pode usar sua associação gratuita para seguir este tutorial. 

## <a name="step-1---aws-membership"></a>Etapa 1-Associação do AWS
Para obter uma associação do AWS, visite a [página de detalhes da conta do AWS](https://aws.amazon.com/free/). Para os fins deste tutorial, você pode usar uma associação gratuita.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Etapa 2-criar um Bucket S3 do Amazon

O Amazon S3 (Simple Storage Service) é uma oferta AWS para coletar, armazenar e analisar dados. Os buckets S3 são uma maneira conveniente de hospedar pacotes de aplicativos do Windows 10 e páginas da Web para distribuição. 

Depois de fazer logon no AWS com suas credenciais, em `Services` localizar `S3`. 

Selecione **criar Bucket**e insira um **nome de Bucket** para seu site. Siga os prompts de diálogo para definir as propriedades e as permissões. Para garantir que seu aplicativo do Windows 10 possa ser distribuído do seu site, habilite as permissões de **leitura** e **gravação** para o Bucket e selecione **conceder acesso de leitura público a este Bucket**.

![Definir permissões no Bucket S3 da Amazon](images/aws-permissions.png) 

Examine o resumo para certificar-se de que as opções selecionadas sejam refletidas. Clique em **criar Bucket** para concluir esta etapa. 

## <a name="step-3---upload-windows-10-app-package-and-web-pages-to-an-s3-bucket"></a>Etapa 3 – carregar o pacote de aplicativos do Windows 10 e páginas da Web em um Bucket S3

Depois de criar um Bucket do Amazon S3, você poderá vê-lo em sua exibição do Amazon S3. Veja um exemplo de como é a aparência de nosso Bucket de demonstração:

![Captura de tela da exibição de Bucket S3 do Amazon](images/aws-post-create.png)

Agora estamos prontos para carregar os pacotes de aplicativos e páginas da Web que gostaríamos de hospedar em nosso Bucket do Amazon S3. 

Clique no Bucket recém-criado para carregar o conteúdo. O Bucket está vazio no momento, pois nada foi carregado ainda. Clique no botão **carregar** e selecione os pacotes de aplicativos e os arquivos de página da Web que você deseja carregar.

> [!NOTE]
> Você pode usar o pacote do aplicativo que faz parte do repositório de [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) fornecido no GitHub se não tiver um pacote do aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. O certificado deve ser instalado em seu dispositivo antes de instalar o aplicativo.

![Captura de tela de carregar pacote de aplicativos UX](images/aws-upload-package.png)

De forma semelhante às permissões para criar um Bucket S3 do Amazon, o conteúdo no Bucket também deve ter permissões de **leitura**, **gravação**e **concessão pública para esse (s) objetos** .

Se você quiser testar o carregamento de uma página da Web, mas não tiver uma, poderá usar a página HTML de exemplo (default. html) do [projeto inicial](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Antes de carregar a página da Web, confirme se a referência do pacote do aplicativo na página da Web está correta. 

Para obter a referência do pacote do aplicativo, carregue o pacote do aplicativo primeiro e copie a URL do pacote do aplicativo. Edite a página da Web HTML para refletir o caminho correto do pacote do aplicativo. Consulte o exemplo de código para obter mais detalhes. 

Selecione o arquivo de pacote do aplicativo carregado para obter o link de referência para o pacote do aplicativo. 

**Copie** o link para o pacote do aplicativo e adicione a referência em sua página da Web. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.msixbundle"> Install My Sample App</a>
    </body>
</html>
```
Carregue o arquivo HTML em seu Bucket S3 do Amazon. Lembre-se de definir as permissões para permitir acesso de **leitura** e **gravação** .

## <a name="step-4---test"></a>Etapa 4-testar

Depois que a página da Web for carregada em seu Bucket do Amazon S3, obtenha o link para a página da Web selecionando o arquivo HTML carregado.

Use o link para abrir a página da Web. Como definimos permissões para conceder acesso público ao pacote do aplicativo e à página da Web, qualquer pessoa com o link para a página da Web poderá acessá-la e instalar seus pacotes de aplicativos do Windows 10 usando o instalador do aplicativo. Observe que o instalador do aplicativo faz parte da plataforma Windows 10. Como desenvolvedor, você não precisa adicionar nenhum código ou recurso adicional ao seu aplicativo para habilitar o uso do instalador do aplicativo. 

## <a name="troubleshooting"></a>Solução de problemas

### <a name="app-installer-fails-to-install"></a>Falha na instalação do instalador de aplicativos 

A instalação do aplicativo falhará se o certificado com o qual o pacote do aplicativo for assinado não estiver instalado no dispositivo. Para corrigir isso, você precisará instalar o certificado antes da instalação do aplicativo. Se você estiver hospedando um pacote de aplicativo para distribuição pública, é recomendável assinar o pacote do aplicativo com um certificado de uma autoridade de certificação. 

