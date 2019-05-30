---
Description: Este artigo contém problemas conhecidos com a Ponte de Desktop.
title: Problemas conhecidos (Ponte de Desktop)
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 60ad8b3aa64265f80a1cf87b8fdec12b56238030
ms.sourcegitcommit: c3bdc2150bba942dc95811746c7a0f14ce54fbc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985556"
---
# <a name="known-issues-with-packaged-desktop-applications"></a>Problemas conhecidos com pacotes de aplicativos da área de trabalho

Este artigo contém problemas conhecidos que podem ocorrer quando você cria um pacote de aplicativo do Windows para o seu aplicativo da área de trabalho.

<a id="app-converter" />

## <a name="known-issues-with-the-desktop-app-converter"></a>Problemas conhecidos com o Desktop App Converter

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED um E_STARTING_ISOLATED_ENV_FAILED erros    

Se você receber qualquer um desses erros, verifique se você está usando uma imagem base válida da [central de download](https://aka.ms/converterimages).
Se você estiver usando uma imagem base válida, tente usar ``-Cleanup All`` em seu comando.
Se isso não funcionar, envie-nos seus registros para converter@microsoft.com para nos ajudar a investigar.

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork: O objeto já existe o erro

Você pode receber esse erro ao configurar uma nova imagem base. Isso pode acontecer se você tiver versão de pré-lançamento do Windows Insider em uma máquina de desenvolvimento que anteriormente tinha o Desktop App Converter instalado.

Para resolver este problema, tente executar o comando `Netsh int ipv4 reset` a partir de um prompt de comando elevado e reinicie sua máquina.

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>Seu aplicativo .NET é compilado com a opção de compilação "AnyCPU" e Falha na instalação

Isso pode acontecer se o executável principal ou qualquer uma das dependências foram colocadas em qualquer lugar na hierarquia de pastas **Arquivos de Programas** ou **Windows\System32**.

Para resolver este problema, tente usar seu instalador de desktop específico da arquitetura (32 bits ou 64 bits) para gerar um pacote de aplicativos do Windows.

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>Publicar conjuntos públicos de Fusion lado a lado não funcionará

 Durante a instalação, um aplicativo pode publicar assemblies Fusion públicos lado a lado, acessíveis para qualquer outro processo. Durante a criação do contexto de ativação do processo, esses assemblies são recuperados por um processo do sistema denominado CSRSS.exe. Quando isso é feito em um processo convertido, a criação do contexto de ativação e o carregamento do módulo desses assemblies falhará. Os conjuntos Fusion lado a lado estão registrados nos seguintes locais:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de arquivos: % windir %\\SideBySide

Essa é uma limitação conhecida e não há atualmente nenhuma solução alternativa. Dito isso, as montagens da caixa de entrada, como o ComCtl, são fornecidas com o sistema operacional, portanto, ter uma dependência delas é seguro.

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>Erro encontrado no XML. O atributo 'Executável' é inválido - O valor 'MyApp.EXE' é inválido de acordo com seu tipo de dados

Isso pode acontecer se os executáveis ​​em seu aplicativo tiverem uma extensão **.EXE** maiúscula. No entanto, o uso de maiusculas e minúsculas desta extensão não deve afetar se seu aplicativo é executado, isso pode causar o DAC gerar esse erro.

Para resolver esse problema, tente especificar o **AppExecutable -** sinalizar quando você empacota e use as letras minúsculas ".exe" como a extensão de seu executável principal (por exemplo: MYAPP.exe).    Como alternativa, você pode alterar as maiusculas e minúsculas para todos os executáveis em seu aplicativo de letra minúscula para maiuscula (por exemplo: do. EXE .exe).

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Assinaturas Authenticode corrompidas ou malformadas

Esta seção contém detalhes sobre como identificar problemas com arquivos PE em seu pacote de aplicativo do Windows que pode conter assinaturas Authenticode corrompidas ou malformadas. As assinaturas Authenticode inválidas em seus arquivos PE, que podem estar em qualquer formato binário (por exemplo, .exe,. dll,. chm, etc.), impedirão que o pacote seja assinado adequadamente e, portanto, impedirá que ele seja implantado de um pacote de aplicativo do Windows.

O local da assinatura Authenticode de um arquivo PE é especificado pela entrada da tabela de certificados nos diretórios de dados de cabeçalho opcionais e na tabela de certificados de atributos associados. Durante a verificação da assinatura, as informações especificadas nessas estruturas são usadas para localizar a assinatura em um arquivo PE. Se esses valores forem corrompidos, será possível que um arquivo pareça estar assinado de modo inválido.

Para a assinatura Authenticode estar correta, o seguinte deverá ser verdadeiro para a assinatura Authenticode:

- O início da entrada **WIN_CERTIFICATE** no arquivo PE não pode ultrapassar o final do executável
- A entrada **WIN_CERTIFCATE** deve estar localizada no final da imagem
- O tamanho da entrada **WIN_CERTIFICATE** deve ser positiva
- A entrada **WIN_CERTIFICATE** deve ser iniciada após a estrutura **IMAGE_NT_HEADERS32** para executáveis de 32 bits e a estrutura IMAGE_NT_HEADERS64 para executáveis de 64 bits

Para obter mais detalhes, consulte a [Especificação de Authenticode Portal Executable](https://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) e a [Especificação de formato de arquivo PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Observe que SignTool.exe pode gerar uma lista dos binários corrompidos ou malformados ao tentar assinar um pacote de aplicativo do Windows. Para fazer isso, ative o registro detalhado, definindo a variável de ambiente APPXSIP_LOG como 1 (por exemplo, ```set APPXSIP_LOG=1``` ) e execute novamente SignTool.exe.

Para corrigir esses binários malformados, certifique-se de que eles estejam em conformidade com os requisitos acima.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Você recebe o erro MSB4018 A tarefa "GenerateResource" falhou inesperadamente

Isso poderá acontecer quando você estiver tentando converter assemblies de satélite em arquivos PRI (Índice de Recurso do Pacote).

Estamos cientes desse problema e estamos trabalhando em um solução de longo prazo. Como uma solução temporária, você pode desabilitar o gerador de recursos, adicionando esta linha de XML ao primeiro elemento PropertyGroup do arquivo de projeto de hospedagem:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Tela azul com código de erro 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Depois de instalar ou iniciar o Microsoft Store de determinados aplicativos, ele pode inesperadamente reiniciar com o erro: **0x139 (KERNEL\_segurança\_Verifique\_ falha)** .

Entre os aplicativos afetados conhecidos estão Kodi, JT2Go, Ear Trumpet, Teslagrad e outros.

Uma [atualização do Windows (versão 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) foi lançada em 27/10/16 incluindo correções importantes que resolvem esse problema. Se você enfrentar esse problema, atualize o computador. Se não conseguir atualizar o computador porque ele reinicia antes de você conseguir fazer logon, você deverá usar a restauração do sistema para restaurar o sistema até um ponto anterior ao momento em que instalou um dos aplicativos afetados. Para obter informações sobre como usar a restauração do sistema, consulte [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Se a atualização não corrigir o problema ou se você não souber como recuperar o computador, entre em contato com o [Suporte da Microsoft](https://support.microsoft.com/contactus/).

Se for um desenvolvedor, você desejará impedir a instalação do aplicativo empacotado em versões do Windows que não incluam essa atualização. Observe que, ao fazer isso em seu aplicativo não estará disponível para os usuários que ainda não tem instalado a atualização. Para limitar a disponibilidade do seu aplicativo para usuários que instalaram essa atualização, modifique o arquivo Appxmanifest XML da seguinte maneira:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Os detalhes a respeito do Windows Update podem ser encontrados em:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Erros comuns que podem aparecer quando você assina seu aplicativo

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>Incompatibilidade de publicador e cert faz com que o Signtool erro "Erro: Falha de SignerSign()"(-2147024885/0x8007000b)

A entrada de Fornecedor no manifesto do pacote de aplicativo do Windows deve corresponder ao Assunto do certificado que você está assinando.  Você pode usar qualquer um dos seguintes métodos para ver o assunto do certificado.

**Opção 1: Powershell**

Execute o seguinte comando do PowerShell: Ambos .cer ou .pfx pode ser usado como o arquivo de certificado, pois eles têm as mesmas informações de fornecedor.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Opção 2: Explorador de arquivos**

Clique duas vezes no certificado no Explorador de Arquivos, selecione a guia *Detalhes* e depois o campo *Assunto* na lista. Em seguida, você pode copiar o conteúdo.

**Opção 3: CertUtil**

Execute **certutil** da linha de comando no arquivo PFX e copiar a *assunto* campo da saída.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>Certificado incorreto de PE (0x800700C1)

Isso pode acontecer quando o pacote contenha um binário que tem um certificado corrompido. Eis algumas das razões por que isso pode acontecer:

* O início do certificado não está no final de uma imagem.  

* O tamanho do certificado não é positivo.

* Não é o início do certificado após o `IMAGE_NT_HEADERS32` estrutura para um executável de 32 bits ou após o `IMAGE_NT_HEADERS64` estrutura para um executável de 64 bits.

* O ponteiro de certificado não está alinhado corretamente para uma estrutura WIN_CERTIFICATE.

Para localizar arquivos que contêm um certificado incorreto do PE, abra uma **Prompt de comando**e defina a variável de ambiente denominada `APPXSIP_LOG` como um valor de 1.

```
set APPXSIP_LOG=1
```

Em seguida, do **Prompt de comando**, assinar o aplicativo novamente. Por exemplo: 

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

Informações sobre os arquivos que contêm um certificado incorreto do PE aparecerão na **janela do Console**. Por exemplo: 

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
