---
description: Uma visão geral das soluções de CI/CD para o desenvolvimento de aplicativos MSIX
title: Visão geral de pipelines de MSIX e de CI/CD
ms.date: 01/11/2021
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d406a101dddaa0a83f773f2ab6425cfea2cdf67d
ms.sourcegitcommit: 059f215a0804adeeefeaaa09b376684caa4382eb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98770963"
---
# <a name="msix-and-cicd-pipeline-overview"></a>Visão geral de pipelines de MSIX e de CI/CD

Você pode usar o Azure Pipelines para criar compilações automatizadas para seu projeto MSIX no Azure DevOps usando uma extensão do Azure DevOps baseada na interface do usuário ou configurando o próprio arquivo YAML. Também mostraremos como realizar essas tarefas usando a linha de comando para que seja possível integrar com qualquer outro sistema de build.

## <a name="create-a-new-azure-pipeline"></a>Crie um novo pipeline do Azure

Comece [inscrevendo-se no Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up) caso ainda não tenha feito isso.

Em seguida, crie um pipeline que possa ser usado para criar seu código-fonte. Para obter um tutorial sobre como criar um pipeline para criar um repositório GitHub, consulte [Crie seu primeiro pipeline](/azure/devops/pipelines/get-started-yaml). O Azure Pipelines é compatível com os tipos de repositório listados [neste artigo](/azure/devops/pipelines/repos).

Para configurar o pipeline do build propriamente dito, navegue para o portal Azure DevOps em dev.azure.com/\<organization\> e crie um novo projeto. Se não tiver uma conta, crie uma gratuitamente. Depois de entrar e criar um projeto, você poderá enviar o código-fonte por push ao repositório Git configurado para você em https://\<organization\>@dev.azure.com/<organization\>/\<project\>/_git/\<project\> ou usar qualquer outro provedor, como o GitHub. Você poderá escolher a localização do repositório ao criar um pipeline no portal, basta primeiro clicar no botão **Pipelines**, depois em **Novo Pipeline**.


## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Adicione o certificado do projeto à biblioteca Arquivos seguros

> [!NOTE]
>Se possível, você deve evitar enviar certificados para o seu repositório e o git os ignorará por padrão. Para gerenciar o tratamento seguro de arquivos confidenciais como certificados, o Azure DevOps oferece suporte ao recurso [arquivos seguros](/azure/devops/pipelines/library/secure-files?view=azure-devops).

Para carregar um certificado em seu build automatizado:

1. No Azure Pipelines, expanda **Pipelines** no painel de navegação e clique em **Biblioteca**.
2. Clique na guia **Arquivos seguros** e clique em **+ Arquivo seguro**.
3. Navegue até o arquivo de certificado e clique em **OK**.
4. Depois de carregar o certificado, selecione-o para visualizar as propriedades dele. Em **Permissões de pipeline**, habilite a opção **Autorizar para uso em todos os pipelines**.
5. Se a chave privada no certificado tiver uma senha, recomendamos que você a armazene no [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) e a vincule a um [grupo de variáveis](/azure/devops/pipelines/library/variable-groups). Você pode usar a variável para acessar a senha pelo pipeline. Observe que uma senha é compatível apenas com a chave privada. No momento, não é possível usar um arquivo de certificado que seja, em si, protegido por senha.

> [!NOTE]
> A partir do Visual Studio 2019, um certificado temporário não é mais gerado nos projetos MSIX. Para criar ou exportar certificados, use os cmdlets do PowerShell descritos [neste artigo](../package/create-certificate-package-signing.md).

## <a name="configure-your-pipeline"></a>Configurar seu pipeline
| Tópico | Descrição |
|----------|-----------|------------|
| [Extensão de empacotamento do MSIX](msix-packaging-extension.md) | Aproveite a extensão Azure DevOps que orientará você na criação e assinatura de um pacote MSIX |
| [Configurar o pipeline de CI/CD com o arquivo YAML](azure-dev-ops.md) | Configurar manualmente o próprio arquivo YAML |
