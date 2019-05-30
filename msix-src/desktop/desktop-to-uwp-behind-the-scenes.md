---
Description: Este artigo se aprofunda no funcionamento da Ponte de Desktop nos bastidores.
title: Nos bastidores da ponte da área de trabalho
ms.date: 04/25/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: 4cfc81ec1e6927db2c9655f99878251e7d60f774
ms.sourcegitcommit: c3bdc2150bba942dc95811746c7a0f14ce54fbc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985666"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>Nos bastidores do seu aplicativo da área de trabalho de pacote

Este artigo fornece um aprofundamento sobre o que acontece com arquivos e entradas do registro quando você cria um pacote de aplicativo do Windows para o seu aplicativo da área de trabalho.

A meta principal de um pacote moderno é separar o estado do aplicativo do estado do sistema tanto quanto possível enquanto mantém a compatibilidade com outros aplicativos. Windows 10 faz isso colocando o aplicativo dentro de um pacote de plataforma Universal do Windows (UWP) e, em seguida, detecção e redirecionamento de algumas alterações que ele faz para o sistema de arquivos e registro em tempo de execução.

Pacotes que você cria para seu aplicativo da área de trabalho são aplicativos de confiança total, somente área de trabalho e não virtualizados ou na área restrita. Isso permite que eles interajam com outros aplicativos da mesma maneira que aplicativos da área de trabalho clássicos.

## <a name="installation"></a>Instalação

Os pacotes de aplicativos são instalados em *C:\Program Files\WindowsApps\package_name*, com o executável intitulado *app_name.exe*. Cada pasta de pacote contém um manifesto (chamado AppxManifest.xml) que contém um namespace XML especial para apps empacotados. Dentro desse arquivo de manifesto está um elemento ```<EntryPoint>```, que faz referência ao aplicativo de confiança total. Quando esse aplicativo é iniciado, ele não é executado dentro de um contêiner de aplicativo, mas em vez disso, ele é executado como o usuário como faria normalmente.

Depois da implantação, os arquivos do pacote serão marcados como somente leitura e totalmente bloqueados pelo sistema operacional. O Windows evitará a inicialização dos aplicativos se esses arquivos forem adulterados.

## <a name="file-system"></a>Sistema de arquivos

O sistema operacional dá suporte a níveis diferentes de operações do sistema de arquivos para os pacotes de aplicativos da área de trabalho, dependendo do local da pasta.

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Operações de dados de aplicativos no Windows 10, versão 1903 e posterior

Todos os recém-criados arquivos e pastas na pasta do AppData do usuário (por exemplo, *C:\Users\user_name\AppData*) são gravados em particular por usuário, local por aplicativo, mas são mescladas em tempo de execução aparecem no AppData local real. Isso permite que um certo grau de separação de estado para artefatos que são usadas somente pelo próprio aplicativo, e isso permite que o sistema limpar esses arquivos quando o aplicativo é desinstalado. Modificações em arquivos existentes na pasta do AppData do usuário tem permissão para impedir que um maior grau de compatibilidade e interatividade entre os aplicativos e o sistema operacional. Isso reduz o sistema de arquivos "rot" porque o sistema operacional está ciente de todas as alterações de arquivo ou diretório feitas por um aplicativo. Separação de estado também permite que os pacotes de aplicativos da área de trabalho recomeçar onde parou uma versão não empacotado do mesmo aplicativo. Observe que o sistema operacional não dá suporte a uma pasta (VFS) do sistema de arquivos virtual para a pasta do AppData do usuário.

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>Operações de dados de aplicativos no Windows 10, versão 1809 e versões anteriores

Todas as gravações para a pasta do AppData do usuário (por exemplo, *C:\Users\user_name\AppData*), incluindo create, delete e update, que são copiados na gravação privado por usuário, local por aplicativo. Isso cria a ilusão de que o aplicativo empacotado está editando o AppData real quando ele está, na verdade, modificando uma cópia privada. Redirecionando gravações dessa maneira, o sistema pode acompanhar todas as modificações de arquivo feitas pelo aplicativo. Isso permite que o sistema limpar esses arquivos quando o aplicativo for desinstalado, assim, reduzindo rot"sistema" e fornecendo uma remoção de aplicativo melhor experiência para o usuário.

### <a name="other-folders"></a>Outras pastas

Redirecionando AppData, além de pastas conhecidas do Windows (System32, arquivos de programas (x86), etc.) dinamicamente são mescladas com as pastas correspondentes no pacote do aplicativo. Cada pacote contém uma pasta chamada "VFS" na raiz. Todas as leituras de diretório ou arquivo no diretório VFS são mescladas em tempo de execução às respectivas contrapartes nativas. Por exemplo, um aplicativo pode conter *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* como parte do pacote do aplicativo, mas o arquivo apareceria para serem instaladas em *C:\Windows\System32\ vc10.dll*.  Isso mantém a compatibilidade com aplicativos da área de trabalho, que podem esperar que arquivos estejam em locais sem pacote.

As gravações em arquivos/pastas no pacote de apps não são permitidas. Gravações em arquivos e pastas que não fazem parte do pacote são ignoradas pelo sistema operacional e são permitidas desde que o usuário tem permissão.

### <a name="common-operations"></a>Operações comuns

Esta tabela de referência curta mostra operações de sistema de arquivos comuns e como o sistema operacional trata nos.

| Operação | Resultado | Exemplo |
| --- | --- | --- |
| Ler ou enumerar um arquivo ou uma pasta do Windows conhecida | Uma mescla dinâmica de *C:\Program Files\package_name\VFS\well_known_folder* à contraparte do sistema local. | A leitura de *C:\Windows\System32* retorna o conteúdo de *C:\Windows\System32* mais o conteúdo de *C:\Program Files\WindowsApps\package_name\VFS\SystemX86*. |
| Gravar em AppData | **Windows 10, versão 1903 e posterior:** Novos arquivos e pastas criadas sob os diretórios a seguir serão redirecionadas para cada usuário, local de privado por pacote:<ul><li>Local</li><li>Local\Microsoft</li><li>Roaming</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Iniciar\Programas</li></ul>Em resposta a um comando aberto do arquivo, o sistema operacional irá abrir o arquivo de cada usuário, por pacote local primeiro. Se esse local não existir, o sistema operacional tentará abrir o arquivo do AppData local real. Se o arquivo é aberto do AppData local real, sem virtualização para esse arquivo ocorre. Exclusões de arquivo sob AppData serão permitidas se o usuário tem permissões.<p/><p/>**Windows 10, versão 1809 e versões anteriores:** Copiar gravação por usuário, local de cada aplicativo. | AppData costuma ser *C:\Users\user_name\AppData*. |
| Gravar no pacote | Não permitido. O pacote é somente leitura. | Gravações em *C:\Program Files\WindowsApps\package_name* não são permitidas. |
| Gravações fora do pacote | Permitido se o usuário tiver permissões. | Uma gravação em *C:\Windows\System32\foo.dll* será permitida se o pacote não contiver *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll* e o usuário tiver permissões. |

### <a name="packaged-vfs-locations"></a>Locais dos pacotes VFS

A tabela a seguir mostra onde os arquivos fornecidos como parte do pacote são sobrepostos no sistema para o aplicativo. Seu aplicativo passarão a esses arquivos para estar em locais de sistema listados, quando na verdade estão nas localizações redirecionadas dentro *C:\Program Files\WindowsApps\package_name\VFS*. Os locais de FOLDERID são das constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

Local do sistema | Redirecionado local (em [PackageRoot] \VFS\) | Válido em arquiteturas
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | AppData comum | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Registro

Os pacotes de apps contêm um arquivo registry.dat, que funciona como o equivalente lógico de *HKLM\Software* no Registro real. Em tempo de execução, esse Registro virtual mescla o conteúdo desse hive ao hive do sistema nativo para oferecer uma visão singular de ambos. Por exemplo, se registry.dat contiver uma única chave "Foo", uma leitura de *HKLM\Software* em tempo de execução também conterá aparentemente "Foo" (além de todas as chaves do sistema nativo).

Somente as chaves em *HKLM\Software* fazem parte do pacote; as chaves em *HKCU* ou outras partes do Registro não fazem parte. Gravações em chaves ou valores no pacote não são permitidas. Grava as chaves ou valores não parte do pacote são permitidos desde que o usuário tem permissão.

Todas as gravações em HKCU são cópias em gravações para um local particular por usuário e por aplicativo. Tradicionalmente, os desinstaladores são conseguem limpar *HKEY_CURRENT_USER* porque os dados do Registro para usuários desconectados estão desmontados e não estão disponíveis.

Todas as gravações são mantidas durante a atualização de pacote e excluídas somente quando o aplicativo foi completamente removido.

### <a name="common-operations"></a>Operações comuns

Esta tabela de referência curta mostra operações de registro comuns e como o sistema operacional trata nos.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar *HKLM\Software* | Uma mesclagem dinâmica do hive do pacote à contraparte do sistema local. | Se registry.dat contiver uma única chave "Foo", em tempo de execução, uma leitura de *HKLM\Software* mostrará o conteúdo de *HKLM\Software* mais *HKLM\Software\Foo*.
Gravações em HKCU | Copiar gravação por usuário, local particular de cada aplicativo. | Igual a AppData para arquivos.
Grava no pacote. | Não permitido. O pacote é somente leitura. | As gravações em *HKLM\Software* não serão permitidas, se um par chave/valor correspondente existir no hive do pacote.
Gravações fora do pacote | Ignorado pelo sistema operacional. Permitido se o usuário tiver permissões. | As gravações em *HKLM\Software* são permitidas desde que um par chave/valor correspondente não exista no hive do pacote e o usuário tenha permissões de acesso corretas.

## <a name="uninstallation"></a>Desinstalação

Quando um pacote é desinstalado pelo usuário, todos os arquivos e pastas localizados sob *Files\WindowsApps\package_name C:\Program* forem removidos, bem como quaisquer gravações redirecionadas AppData ou no registro que foram capturados durante o processo de empacotamento.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).