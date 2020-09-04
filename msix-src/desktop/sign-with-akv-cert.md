---
title: Assinar pacotes com o Azure Key Vault
description: Este artigo descreve como assinar o pacote do aplicativo com um certificado do Azure Key Vault.
ms.date: 05/07/2020
ms.topic: article
keywords: Windows 10, MSIX, UWP, Azure Key Vault, Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: ac69ae105dbd1fff5d64c20f50af644f9d27504f
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090424"
---
# <a name="sign-packages-with-azure-key-vault"></a>Assinar pacotes com o Azure Key Vault

No Visual Studio 2019 versão 16.6 (Versão Prévia 3 e versões posteriores), você poderá assinar pacotes do aplicativo da área de trabalho e UWP com um certificado armazenado no Azure Key Vault para cenários de desenvolvimento e de teste. Essa ferramenta extrai as chaves públicas e privadas do Azure Key Vault e as carrega no repositório de certificados do computador de desenvolvimento a fim de assinar o pacote com a SignTool.exe.

> [!IMPORTANT]
> O processo descrito neste artigo destina-se apenas a cenários de desenvolvimento e de teste. Esse processo não é considerado uma melhor prática para as chaves privadas usadas para distribuição. Para garantir as melhores práticas de segurança, as chaves privadas para distribuição devem ser manipuladas somente pelas ferramentas recomendadas pela plataforma de CI/CD (integração contínua e implantação contínua).

## <a name="prerequisites"></a>Pré-requisitos

- Uma conta do Azure. Caso ainda não tenha uma conta do Azure, inicie [aqui](https://azure.microsoft.com/free/).
- Um Azure Key Vault. Para obter mais informações, confira [Criar um Key Vault](/azure/key-vault/secrets/quick-create-portal#create-a-vault).
- Um certificado de assinatura de pacote válido importado para o Azure Key Vault. O certificado padrão gerado pelo Azure Key Vault não funcionará para a assinatura de código. Para obter detalhes sobre como criar um certificado de assinatura de pacote, confira [Criar um certificado para assinatura de pacote](../package/create-certificate-package-signing.md).

## <a name="import-a-certificate-to-your-key-vault"></a>Importar um certificado para o Key Vault

Adicionar um certificado ao Key Vault é muito simples. Neste exemplo, adicionamos um certificado de assinatura de código UWP válido chamado **UwpSigningCert.pfx**.

1. Nas páginas de propriedades do Key Vault, selecione **Certificados**.
2. Clique em **Gerar/importar**.
3. Na tela **Criar um certificado**, escolha os seguintes valores:
    - **Método de criação de certificado**: Importar
    - **Nome do certificado**: UwpSigningCert
    - **Carregar arquivo do certificado**: UwpSigningCert.pfx
    - **Descriptografar certificado**: se o certificado estiver protegido por senha, informe-a no campo **Senha**.
4. Clique em **Criar**.

> [!NOTE]
> Esse certificado autoassinado não será aceito como confiável pelo Windows, a menos que tenha sido importado e aceito como confiável por um administrador. Mantenha todos os certificados seguros, incluindo os autoassinados.

## <a name="configure-the-access-policies-for-your-key-vault"></a>Configurar as políticas de acesso para o Key Vault

Você poderá controlar quem tem acesso ao conteúdo do Key Vault usando as **políticas de acesso**. As políticas de acesso ao Key Vault concedem permissões separadamente a chaves, segredos e certificados. Você poderá permitir a um usuário acesso apenas a chaves e não a segredos. As permissões de acesso a chaves, segredos e certificados são gerenciadas no nível do cofre. Para obter mais informações, confira [Segurança do Azure Key Vault](/azure/key-vault/general/overview-security#identity-and-access-management).

> [!NOTE]
> Quando você cria um Key Vault em uma assinatura do Azure, ele é automaticamente associado ao locatário do Azure Active Directory da assinatura. Qualquer pessoa que tentar gerenciar ou recuperar conteúdo de um Key Vault deverá ser autenticada pelo Azure AD.

1. Nas páginas de propriedades do Key Vault, selecione **Políticas da acesso**.
2. Selecione **+ Adicionar política de acesso**.
3. Clique no menu suspenso **Permissões de chave** e marque as caixas **Obter** e **Listar** em **Operações de gerenciamento de chaves**.
4. Clique em **Selecionar principal**, procure o usuário ao qual você está permitindo acesso e clique em **Selecionar**.
5. Clique em **Adicionar**.
6. Não se esqueça de salvar as alterações clicando em **Salvar**.

> [!NOTE]
> Permitir acesso direto a um cofre de chaves aos usuários **não é recomendável**. O ideal é que os usuários sejam adicionados a um grupo do Azure AD, que, por sua vez, receba permissão de acesso ao cofre de chaves.

## <a name="select-a-certificate-from-your-key-vault-in-visual-studio"></a>Selecionar um certificado do Key Vault no Visual Studio

O assistente **Criar pacotes do aplicativo** no Visual Studio permite que você escolha o certificado que será usado para assinar o pacote do aplicativo. Você poderá escolher o certificado de assinatura do pacote por meio do Azure Key Vault. Você deverá fornecer o URI do Key Vault que contém o certificado e a conta Microsoft autenticada no Visual Studio deverá ter as permissões corretas para acessá-lo.

1. Abra o projeto **do aplicativo UWP** ou o desktop do **projeto de empacotamento de aplicativo do Windows** no Visual Studio.
2. Selecione **Publicar** -> **Pacote** -> **Criar pacotes do aplicativo...** para abrir o assistente **Criar pacotes do aplicativo**.
3. Na página **Selecionar método de distribuição**, selecione **Sideload**.
4. Na página **Selecionar método de assinatura**, clique em **Selecionar do Azure Key Vault...** .
5. Depois que a caixa de diálogo **Selecionar um certificado do Azure Key Vault** for exibida, use o seletor de conta a fim de escolher a conta para a qual você configurou uma política de acesso.
6. Insira o URI do Key Vault. O URI pode ser encontrado na página **Visão geral** do Key Vault e é identificado pelo **Nome DNS**.
7. Clique no botão **Exibir metadados**.
8. Depois que o carregamento dos certificados tiver terminado, selecione o certificado desejado na lista (por exemplo, **UwpSigningCert**).
9. Clique em **OK**.

> [!NOTE]
> O certificado será importado para o repositório de certificados local, no qual será usado para assinatura de pacotes.

## <a name="decrypt-your-certificate-with-a-password-from-azure-key-vault"></a>Descriptografar o certificado com uma senha do Azure Key Vault

Se você estiver usando um certificado local protegido por senha (.pfx) para assinar o pacote do aplicativo, poderá ser difícil gerenciar a senha usada para descriptografá-lo. Ao importar o certificado por meio do assistente **Criar pacotes do aplicativo**, será solicitado que você insira a senha manualmente. Como alternativa, há a opção de escolher a senha do Azure Key Vault.

1. Abra o projeto **do aplicativo UWP** ou o desktop do **projeto de empacotamento de aplicativo do Windows** no Visual Studio.
2. Selecione **Publicar** -> **Pacote** -> **Criar pacotes do aplicativo...** para abrir o assistente **Criar pacotes do aplicativo**.
3. Na página **Selecionar método de distribuição**, selecione **Sideload**.
4. Na página **Selecionar método de assinatura**, clique em **Selecionar do arquivo..** .
5. Depois que a caixa de diálogo **Certificado protegido por senha** for exibida, clique em **Selecione senha do Key Vault**.
6. Depois que a caixa de diálogo **Selecionar senha do Azure Key Vault** for exibida, use o seletor de conta a fim de escolher a conta para a qual você configurou uma política de acesso.
7. Insira o URI do Key Vault. O URI pode ser encontrado na página **Visão geral** do Key Vault e é identificado pelo **Nome DNS**.
8. Clique no botão **Exibir metadados**.
9. Depois que o carregamento das senhas tiver terminado, selecione a senha desejada na lista (por exemplo, **UwpSigningCertPassword**).
10. Clique em **OK**.