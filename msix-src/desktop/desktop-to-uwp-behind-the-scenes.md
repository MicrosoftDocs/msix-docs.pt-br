---
Description: Este artigo detalha a foma como a Ponte de Desktop funciona nos bastidores.
title: Nos bastidores da ponte da área de trabalho
ms.date: 01/30/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: c646f8c393c43a6aa01fc0cf594b269cef984433
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "77072737"
---
# <a name="understanding-how-packaged-desktop-apps-run-on-windows"></a>Noções básicas sobre como os aplicativos da área de trabalho empacotados são executados no Windows

Este artigo oferece mais detalhes sobre o que acontece com os arquivos e as entradas de Registro quando você cria um pacote do aplicativo do Windows para o aplicativo da área de trabalho.

Um dos principais objetivos de um pacote moderno é separar o estado do aplicativo do estado do sistema tanto quanto possível ao mesmo tempo que mantém a compatibilidade com outros aplicativos. O Windows 10 faz isso colocando o aplicativo dentro de um pacote MSIX e detectar e redirecionar algumas alterações feitas ao sistema de arquivos e no Registro no tempo de execução.

Os pacotes criados para o aplicativo da área de trabalho são aplicativos somente para área de trabalho, totalmente confiáveis e não são virtualizados ou estão em área restrita. Isso permite que eles interajam com outros aplicativos da mesma maneira que aplicativos da área de trabalho clássicos.

## <a name="installation"></a>Instalação

Os pacotes de aplicativos são instalados em *C:\Program Files\WindowsApps\package_name*, com o executável intitulado *app_name.exe*. Cada pasta do pacote contém um manifesto (chamado AppxManifest.xml) que contém um namespace XML especial para aplicativos empacotados. Dentro desse arquivo de manifesto está um elemento ```<EntryPoint>```, que faz referência ao aplicativo de confiança total. Quando o aplicativo é iniciado, ele não é executado em um contêiner de aplicativo e sim como normalmente o usuário o executaria.

Depois da implantação, os arquivos do pacote serão marcados como somente leitura e totalmente bloqueados pelo sistema operacional. O Windows evitará a inicialização dos aplicativos se esses arquivos forem adulterados.

## <a name="file-system"></a>Sistema de arquivos

O sistema operacional é compatível com diferentes níveis de operação de sistemas de arquivos para aplicativos de área de trabalho empacotados, dependendo do local da pasta.

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Operações de AppData no Windows 10, versão 1903 e posterior

Todos os arquivos e pastas recém-criados na pasta AppData do usuário (por exemplo, *C:\Users\nome_de_usuário\AppData*) são gravados em um local privado por aplicativo e por usuário, mas mesclados no tempo de execução para aparecer no local AppData real. Isso permite algum grau de separação de estado para artefatos usados apenas pelo aplicativo em si e também permite ao sistema limpar esses arquivos quando o aplicativo é desinstalado. As modificações nos arquivos existentes na pasta AppData do usuário são permitidas para oferecer um grau mais alto de compatibilidade e interatividade entre os aplicativos e o sistema operacional. Isso reduz o “rot” do sistema de arquivos, já que o sistema operacional está ciente de todas as alterações de arquivos e diretórios feitas por um aplicativo. A separação de estado também permite que aplicativos de área de trabalho empacotados continuem de onde uma versão não empacotada do mesmo aplicativo parou. Observe que o sistema operacional não é compatível com uma pasta VFS (sistema de arquivos virtual) para a pasta AppData do usuário.

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>Operações de AppData no Windows 10, versão 1809 e anteriores

Todas as gravações feitas na pasta AppData do usuário (por exemplo, *C:\Users\user_name\AppData*), inclusive criar, excluir e atualizar, são copiadas na gravação para um local privado por usuário e por aplicativo. Isso gera a ilusão de que o aplicativo empacotado está editando a AppData real quando está, na verdade, modificando uma cópia particular. Redirecionando gravações dessa maneira, o sistema pode acompanhar todas as modificações de arquivo feitas pelo aplicativo. Isso permite que o sistema limpe esses arquivos quando o aplicativo é desinstalado, o que reduz o "rot" do sistema e oferece uma experiência melhor de remoção de aplicativo para o usuário.

### <a name="other-folders"></a>Outras pastas

Além de redirecionar a AppData, as pastas conhecidas do Windows (System32, Program Files (x86) etc) são mescladas dinamicamente com diretórios correspondentes no pacote do aplicativo. Cada pacote contém uma pasta chamada "VFS" na raiz. Todas as leituras de diretório ou arquivo no diretório VFS são mescladas em runtime às respectivas contrapartes nativas. Por exemplo, um aplicativo pode conter *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* como parte do pacote de aplicativo, mas o arquivo apareceria instalado em *C:\Windows\System32\vc10.dll*.  Isso mantém a compatibilidade com aplicativos da área de trabalho, que podem esperar que arquivos estejam em locais sem pacote.

As gravações em arquivos/pastas no pacote do aplicativo não são permitidas. As gravações nos arquivos e nas pastas que não fazem parte do pacote são ignoradas pelo sistema operacional e são permitiras desde que o usuário tenha permissão.

### <a name="common-operations"></a>Operações comuns

Essa curta tabela de referência mostra operações comuns de sistema de arquivos e como o sistema operacional lida com elas.

| Operação | Resultado | Exemplo |
| --- | --- | --- |
| Ler ou enumerar um arquivo ou uma pasta do Windows conhecida | Uma mescla dinâmica de *C:\Program Files\package_name\VFS\well_known_folder* à contraparte do sistema local. | A leitura de *C:\Windows\System32* retorna o conteúdo de *C:\Windows\System32* mais o conteúdo de *C:\Program Files\WindowsApps\package_name\VFS\SystemX86*. |
| Gravar em AppData | **Windows 10, versão 1903 e posteriores:** Novos arquivos e pastas criados nos seguintes diretórios são redirecionados para um local privado por usuário e por pacote:<ul><li>Local</li><li>Local\Microsoft</li><li>Roaming</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Menu\Programs</li></ul>Em resposta ao comando para abrir o arquivo, o sistema operacional abrirá o arquivo primeiro do local por usuário e por pacote. Se esse local não existir, o sistema operacional tentará abrir o arquivo pelo local AppData real. Se o arquivo for aberto pelo local AppData real, não haverá virtualização do arquivo. As exclusões de arquivos em AppData serão permitidas se o usuário tiver permissões.<p/><p/>**Windows 10, versão 1809 e anteriores:** Copiar gravação por usuário, local de cada aplicativo. | AppData costuma ser *C:\Users\user_name\AppData*. |
| Gravar no pacote | Não permitido. O pacote é somente leitura. | Gravações em *C:\Program Files\WindowsApps\package_name* não são permitidas. |
| Gravações fora do pacote | Permitido se o usuário tiver permissões. | Uma gravação em *C:\Windows\System32\foo.dll* será permitida se o pacote não contiver *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll* e o usuário tiver permissões. |

### <a name="packaged-vfs-locations"></a>Locais dos pacotes VFS

A tabela a seguir mostra onde os arquivos fornecidos como parte do pacote são sobrepostos no sistema para o aplicativo. O aplicativo perceberá que os arquivos esses arquivos estão nos locais de sistema listados quando, na verdade, eles estão nos locais redirecionados em *C:\Program Files\WindowsApps\package_name\VFS*. Os locais de FOLDERID são das constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

Local do sistema | Local redirecionado (em [PackageRoot]\VFS\) | Válido em arquiteturas
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

Os pacotes do aplicativo contêm um arquivo registry.dat que serve como o equivalente lógico de *HKLM\Software* no Registro real. Em runtime, esse Registro virtual mescla o conteúdo desse hive ao hive do sistema nativo para oferecer uma visão singular de ambos. Por exemplo, se registry.dat contiver uma única chave "Foo", uma leitura de *HKLM\Software* em runtime também conterá aparentemente "Foo" (além de todas as chaves do sistema nativo).

Somente as chaves em *HKLM\Software* fazem parte do pacote; as chaves em *HKCU* ou outras partes do Registro não fazem parte. Gravações em chaves ou valores no pacote não são permitidas. As gravações em chaves ou valores que não fazem parte do pacote são permitidas desde que o usuário tenha permissão.

Todas as gravações em HKCU são cópias em gravações para um local particular por usuário e por aplicativo. Tradicionalmente, os desinstaladores são conseguem limpar *HKEY_CURRENT_USER* porque os dados do Registro para usuários desconectados estão desmontados e não estão disponíveis.

Todas as gravações são mantidas durante a atualização do pacote e são excluídas somente quando o aplicativo é totalmente removido.

### <a name="common-operations"></a>Operações comuns

Essa curta tabela de referência mostra operações de Registro comuns e como o sistema operacional lida com elas.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar *HKLM\Software* | Uma mesclagem dinâmica do hive do pacote à contraparte do sistema local. | Se registry.dat contiver uma única chave "Foo", em runtime, uma leitura de *HKLM\Software* mostrará o conteúdo de *HKLM\Software* mais *HKLM\Software\Foo*.
Gravações em HKCU | Copiar gravação por usuário, local particular de cada aplicativo. | Igual a AppData para arquivos.
Grava no pacote. | Não permitido. O pacote é somente leitura. | As gravações em *HKLM\Software* não serão permitidas, se um par chave/valor correspondente existir no hive do pacote.
Gravações fora do pacote | Ignorado pelo sistema operacional. Permitido se o usuário tiver permissões. | As gravações em *HKLM\Software* são permitidas desde que um par chave/valor correspondente não exista no hive do pacote e o usuário tenha permissões de acesso corretas.

## <a name="uninstallation"></a>Desinstalação

Quando um pacote é desinstalado pelo usuário, todos os arquivos e pastas localizados em *C:\Program Files\WindowsApps\package_name* são removidos, bem como quaisquer gravações redirecionadas para a AppData ou para o Registro que foram capturados durante o processo de empacotamento.