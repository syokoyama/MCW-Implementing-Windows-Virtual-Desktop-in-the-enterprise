![マイクロソフト クラウド ワークショップ](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "マイクロソフト クラウド ワークショップ")

<div class="MCWHeader1">
エンタープライズ向け Windows Virtual Desktop の実装
</div>
<div class="MCWHeader2">
ステップバイステップ ハンズオン ラボ
</div>
<div class="MCWHeader3">
2020 年 9 月
</div>
このドキュメントに記載されている情報 (URL 等のインターネット Web サイトに関する情報を含む) は、将来予告なしに変更されることがあります。特に断りがない限り、ここで使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベントの例は、架空のものであり、実在する会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベントなどとは一切関係ありません。お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

製造元や製品の名前、URL は情報の提供のみを目的としており、マイクロソフトは、これらの製造元、またはマイクロソフトの技術での製品の使用について、明示的、黙示的、または法的にいかなる表示または保証も行いません。製造元または製品の使用は、マイクロソフトによるその製造元または製品の推奨を意味するものではありません。サード パーティのサイトへのリンクが提供されている場合があります。このようなサイトはマイクロソフトの管理下にはなく、マイクロソフトは、リンクされたサイトの内容またはリンクされたサイトに含まれるリンク、あるいはこのようなサイトの変更または更新について責任を負いません。マイクロソフトは、リンクされたサイトから受信された Web キャストまたは他のいかなる形態の転送にも責任を負いません。マイクロソフトは、これらのリンクを便宜のみを目的として提供しており、いかなるリンクの使用も、マイクロソフトによるサイトまたはそこに含まれる製品の推奨を意味するものではありません。

© 2020 Microsoft Corporation.All rights reserved.

Microsoft および <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> に記載されている商標は、Microsoft グループの商標です。その他すべての商標は、該当する各社が所有しています。

**目次**

<!-- TOC -->

- [エンタープライズ向け Windows Virtual Desktop の実装のステップバイステップ ハンズオン ラボ](#エンタープライズ向け-windows-virtual-desktop-の実装のステップバイステップ-ハンズオン-ラボ)
    - [要約と学習目的](#要約と学習目的)
    - [概要](#概要)
    - [ソリューションのアーキテクチャ](#ソリューションのアーキテクチャ)
    - [要件](#要件)
    - [ハンズオン ラボの前に](#ハンズオン-ラボの前に)
    - [演習 1: AD DS を使用した Azure AD Connect の構成](#演習-1-ad-ds-を使用した-azure-ad-connect-の構成)
        - [タスク 1: ドメイン コントローラーへの接続](#タスク-1-ドメイン-コントローラーへの接続)
        - [タスク 2: IE セキュリティ強化の無効化](#タスク-2-ie-セキュリティ強化の無効化)
        - [タスク 3: ドメイン管理アカウントの作成](#タスク-3-ドメイン管理アカウントの作成)
        - [タスク 4: Azure AD Connect の構成](#タスク-4-azure-ad-connect-の構成)
    - [演習 2: WVD 用 Azure AD グループの作成](#演習-2-wvd-用-azure-ad-グループの作成)
        - [タスク 1: Azure AD グループの作成](#タスク-1-azure-ad-グループの作成)
        - [タスク 2: グループへのユーザーの割り当て](#タスク-2-グループへのユーザーの割り当て)
    - [演習 3: FSLogix 用の Azure ファイル共有の作成](#演習-3-fslogix-用の-azure-ファイル共有の作成)
        - [タスク 1: ストレージ アカウントの作成](#タスク-1-ストレージ-アカウントの作成)
        - [タスク 2: Azure ファイル共有の作成](#タスク-2-azure-ファイル共有の作成)
        - [タスク 3: ストレージ アカウントの AD 認証の有効化](#タスク-3-ストレージ-アカウントの-ad-認証の有効化)
        - [タスク 4: 共有アクセス許可の構成](#タスク-4-共有アクセス許可の構成)
        - [タスク 5: ファイル共有の NTFS アクセス許可の構成](#タスク-5-ファイル共有の-ntfs-アクセス許可の構成)
        - [タスク 6: コンテナーの NTFS アクセス許可の構成](#タスク-6-コンテナーの-ntfs-アクセス許可の構成)
    - [演習 4: WVD のマスター イメージの作成](#演習-4-wvd-のマスター-イメージの作成)
        - [タスク 1: Azure での仮想マシン (VM) の新規作成](#タスク-1-azure-での仮想マシン-vm-の新規作成)
        - [タスク 2: Windows Update の実行](#タスク-2-windows-update-の実行)
        - [タスク 3: WVD イメージの準備](#タスク-3-wvd-イメージの準備)
        - [タスク 4: Sysprep の実行](#タスク-4-sysprep-の実行)
        - [タスク 5: マスター イメージ VM からの管理対象イメージの作成](#タスク-5-マスター-イメージ-vm-からの管理対象イメージの作成)
        - [タスク 6: カスタム イメージを使用したホスト プールのプロビジョニング](#タスク-6-カスタム-イメージを使用したホスト-プールのプロビジョニング)
    - [演習 5: 個人用デスクトップのホスト プールの作成](#演習-5-個人用デスクトップのホスト-プールの作成)
        - [タスク 1: ホスト プールとワークスペースの新規作成](#タスク-1-ホスト-プールとワークスペースの新規作成)
        - [タスク 2: ワークスペースのフレンドリ名の作成](#タスク-2-ワークスペースのフレンドリ名の作成)
        - [タスク 3: アプリケーション グループへの Azure AD グループの割り当て](#タスク-3-アプリケーション-グループへの-azure-ad-グループの割り当て)
    - [演習 6: ホスト プールの作成とプールされたリモート アプリの割り当て](#演習-6-ホスト-プールの作成とプールされたリモート-アプリの割り当て)
        - [タスク 1: ホスト プールとワークスペースの新規作成](#タスク-1-ホスト-プールとワークスペースの新規作成-1)
        - [タスク 2: ワークスペースのフレンドリ名の作成](#タスク-2-ワークスペースのフレンドリ名の作成-1)
        - [タスク 3: ホスト プールへのリモート アプリの追加](#タスク-3-ホスト-プールへのリモート-アプリの追加)
    - [演習 7: Web クライアントを使用した WVD への接続](#演習-7-web-クライアントを使用した-wvd-への接続)
        - [タスク 1: HTML5 Web クライアントを使用した接続](#タスク-1-html5-web-クライアントを使用した接続)
    - [演習 8: WVD の監視の設定](#演習-8-wvd-の監視の設定)
        - [タスク 1: Log Analytics ワークスペースの作成](#タスク-1-log-analytics-ワークスペースの作成)
        - [タスク 2: WVD の診断ログ記録の有効化](#タスク-2-wvd-の診断ログ記録の有効化)
        - [タスク 3: ホスト プールのログ記録の有効化](#タスク-3-ホスト-プールのログ記録の有効化)
        - [タスク 4: アプリケーション グループのログ記録の有効化](#タスク-4-アプリケーション-グループのログ記録の有効化)
        - [タスク 5: ワークスペースのログ記録の有効化](#タスク-5-ワークスペースのログ記録の有効化)
        - [タスク 6: セッション ホストに対する Azure Monitor の有効化](#タスク-6-セッション-ホストに対する-azure-monitor-の有効化)
    - [ハンズオン ラボの後に](#ハンズオン-ラボの後に)
        - [タスク 1: リソース グループを削除してラボ環境を削除する](#タスク-1-リソース-グループを削除してラボ環境を削除する)

<!-- /TOC -->
# エンタープライズ向け Windows Virtual Desktop の実装のステップバイステップ ハンズオン ラボ

## 要約と学習目的

このハンズオン ラボでは、Windows Virtual Desktop インフラストラクチャを実装し、標準的なエンタープライズ モデルで動作する WVD 環境をエンドツーエンドで設定する方法を学習します。参加者がラボの手順を最後まで実施すると、Azure で実行される Active Directory ドメイン コントローラーに、Azure AD Connect を使用する Azure Active Directory テナントがデプロイされます。また参加者は、Windows Virtual Desktop テナント、ホスト プール、セッション ホストのために Azure インフラストラクチャをデプロイし、サポートされるさまざまなデバイスとブラウザーを使用して WVD セッションに接続します。Windows Virtual Desktop とリモート アプリを公開し、FSLogix を使用してユーザー プロファイルとファイル共有を構成します。最後に、Windows Virtual Desktop インフラストラクチャの監視とセキュリティを構成し、マスター イメージを管理する手順を学習します。

## 概要

このラボでは、参加者は [Windows Virtual Desktop (WVD)](https://azure.microsoft.com/ja-jp/services/virtual-desktop/) ソリューションをデプロイします。Azure のクラウド サービスとして提供される Windows Virtual Desktop では、企業の Azure クラウド戦略に最もよく適合するエンド ユーザー仮想化アプリケーション、またはデスクトップ配信モデルを柔軟に選択できます。WVD を使用すれば、統合された管理により、モダンとレガシの両方のデスクトップ アプリ エクスペリエンスを仮想化してデプロイする IT モデルが簡素化され、診断、ネットワーキング、接続ブローカー、ゲートウェイなどのコンポーネントのホスティング、インストール、構成、管理を行う必要がありません。WVD は、Microsoft Office 365 と Azure を組み合わせて、比類のないスケールで唯一のマルチセッション Windows 10 エクスペリエンスを実現し、IT コストを削減しながら今日のモダン デジタル ワークスペースを強化します。

## ソリューションのアーキテクチャ

![これは、以下の本文で説明されるソリューションのアーキテクチャ図です。](images/wvdsolutiondiagramv2.png "ソリューションのアーキテクチャ")

この図は、Active Directory 用のオンプレミス サーバーを使用する Windows Virtual Desktop アーキテクチャを示しています。この図の中で、ホスト プールは各種のサポートされるデバイスに WVD セッションを提供します。Azure Monitor、Network Watcher、Log Analytics は、アクティビティとパフォーマンスのメトリックを監視し、ログに記録します。

## 要件

Windows Virtual Desktop ワークスペースの設定を開始する前に、次の項目が用意されていることを確認します。

- Windows Virtual Desktop ユーザー用の Azure Active Directory のテナント ID。

- Azure Active Directory テナント内の全体管理者アカウント。
  
  - これは、顧客のために Windows Virtual Desktop ワークスペースを作成するクラウド ソリューション プロバイダー (CSP) の組織にも適用されます。もしお客様が CSP 組織に属している場合は、顧客の Azure Active Directory テナントの全体管理者としてサインインできる必要があります。
  
  - 管理者アカウントは、Windows Virtual Desktop ワークプレースの作成先の Azure Active Directory テナントから供給する必要があります。このプロセスでは、Azure Active Directory B2B (ゲスト) アカウントはサポートされません。
  
  - 管理者アカウントは、職場または学校アカウントである必要があります。

- Azure サブスクリプション。
  
  - 4 台の 4 コア サーバーを構築するのに十分な数のクォータ コア。
  
  - 新規または既存の Azure Active Directory テナントのための Azure Active Directory 全体管理者アカウントへのアクセス許可。
  
  - すべての Azure サブスクリプションに対する所有者の権限。

## ハンズオン ラボの前に

- ラボの演習に進む前に、「ハンズオン ラボの前に」設定ガイドを参照してください。

- 使用しているユーザー アカウントと、これらのアカウントを使用する場所を必ず記録してください。

- リージョンと場所については、可能な限り一貫性を保つようにします。

## 演習 1: AD DS を使用した Azure AD Connect の構成

所要時間: 60 分

この演習では、[Azure AD Connect](https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/whatis-azure-ad-connect) を構成します。Windows Virtual Desktop では、WVD テナント環境内にあるすべてのセッション ホスト VM が AD DS にドメイン参加済みであることが必要で、ドメインは Azure AD と同期している必要があります。オブジェクトの同期を管理するために、Azure にデプロイされているドメイン コントローラー上で Azure AD Connect を構成します。

> **注**: ドメイン コントローラーへの RDP アクセスにパブリック IP アドレスを使用することはベスト プラクティスではなく、このラボの手順を簡単にする目的でのみ行われています。PIP の削除、Just-In-Time アクセスの有効化、Bastion ホストの活用など、より適切なセキュリティ対策を使用してセキュリティを強化する必要があります。

**追加リソース**

 |              |            |  
|----------|:----------:|
| 説明 | リンク |
| Windows Virtual Desktop Spring Update のパブリック プレビュー開始 |https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245 |
| ARM ベース モデル (パブリック プレビュー) のデプロイ手順の説明 |https://www.christiaanbrinkhoff.com/2020/05/01/windows-virtual-desktop-technical-2020-spring-update-arm-based-model-deployment-walkthrough/#NewAzurePortal-Dashboard |
| | 

### タスク 1: ドメイン コントローラーへの接続

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 検索フィールドに「**リソース グループ**」と入力して、リストから選択します。

3. \[Resource groups\] ブレードで、「**ハンズオン ラボの前に**」のテンプレートのデプロイ時に作成したリソース グループ名を選択します。

4. \[Infra Resource group\] ブレードで、使用可能なリソースのリストを確認します。「**AdPubIP1**」という名前のリソースを見つけて選択します。リソースの種類が \[Public IP address\] であることに注意してください。
   
   ![ドメイン コントローラー VM のパブリック IP アドレスを見つけます](images/publicip.png "ドメイン コントローラー VM のパブリック IP アドレス")

5. AdPubIP1 の \[Overview\] ページで、\[IP address\] フィールドを見つけます。IP アドレスを安全な場所にコピーします。

6. ローカル マシンで \[RUN\] ダイアログ ウィンドウを開き、「**MSTSC**」と入力して Enter キーを押します。
   
   ![\[Run\] ダイアログ ウィンドウを開いて MSTSC を実行します](images/run.png "Windows の [Run]")

7. \[Remote Desktop Connection\] ウィンドウで、前のステップからのパブリック IP アドレスを貼り付けます。\[Connect\] を選択します。
   
   ![ドメイン コントローラー VM のパブリック IP アドレスを入力するためのリモート デスクトップ接続のウィンドウが開きます。](images/remoteDesktop.png "リモート デスクトップ接続のウィンドウ")

8. プロンプトが表示されたら、AD ドメイン UPN の資格情報を使用してサインインします。たとえば、[「ハンズオン ラボの前に」設定ガイド]()からの ARM テンプレートを使用した場合、資格情報は [adadmin@MyADDomain.com](mailto:adadmin@MyADDomain.com) とパスワード **WVD@zureL@b2019!** のような組み合わせになります。プロンプトが表示された場合は、\[Yes\] を選択して RDP 認証に関する警告を受け入れます。
   
   > **注**: これは ARM テンプレートからの Active Directory アカウントであり、Azure AD の全体管理者アカウントではありません。サインインに問題が生じた場合は、資格情報の手入力を試してください。コピーと貼り付けを行うと不要なスペースが入る場合があり、認証が失敗する原因になります。

### タスク 2: IE セキュリティ強化の無効化

このラボでのタスクを簡素化するために、最初に [IE セキュリティ強化](https://docs.microsoft.com/ja-jp/windows-hardware/customize/desktop/unattend/microsoft-windows-ie-esc)を無効化します。

1. ドメイン コントローラーに接続した後、自動的に開始されない場合はサーバー マネージャーを開きます。

2. サーバー マネージャーで、左側の \[Local Server\] を選択します。

3. \[IE Enhanced Security Configuration\] オプションを見つけて \[On\] を選択します。
   
   ![サーバー マネージャーの \[Local Server\] プロパティで、\[Enhanced Security\] の構成を見つけます。](images/IEESC.png "サーバー マネージャー内の [Local Server] プロパティ")

4. \[Internet Explorer Enhanced Security Configuration\] ウィンドウで、\[Administrators\] の下にある \[Off\] ラジオ ボタンを選択して、\[OK\] を選択します。
   
   ![現在の構成を選択すると、新規ウィンドウが開いて、セキュリティ強化の構成を無効にすることができます。](images/disablesecurity.png "セキュリティ強化の構成の無効化")

### タスク 3: ドメイン管理アカウントの作成

既定では、Azure AD Connect は組み込みのドメイン管理者アカウント [ADAdmin@MyADDomain.com](mailto:ADAdmin@MyADDomain.com) を同期しません。このシステム アカウントの属性 isCriticalSystemObject は true に設定されており、このためにアカウントの同期が行われません。この属性を変更することは可能ですが、変更しないことがベスト プラクティスです。

1. サーバー マネージャーで、右上隅の \[Tools\] を選択して、\[Active Directory Users and Computers\] を選択します。
   
   ![右上隅の \[Tools\] を見つけて、サーバー マネージャー ツールにアクセスします。](images/serverMangerTools.png "サーバー マネージャー ツール")

2. \[Active Directory Users and Computers\] で、\[Users\] 組織単位を右クリックして、メニューから \[New\] > \[User\] を選択します。
   
   ![ユーザーのフォルダー パスを見つけ、右クリックして新規ユーザーを追加します](images/newUser.png "新規ユーザーのフォルダー パス")

3. \[New User\] ウィザードの手順を実行します。
   
   ![新規ユーザーについて入力するフィールドがあるウィンドウが開きます。](images/newuserobject.png "ユーザーの新規作成")
   
   ![次のウィンドウでは、パスワードを割り当てることができます。](images/newUserWizard.png "[New User] ウィザードのウィンドウ")
   
   ![ウィザードの最後の画面では、新規ユーザーの設定を確認して完了できます。](images/finishnewuser.png "新規ユーザーの設定の完了")
   
   > **注**: このアカウントは、以降のタスクを実行するために重要です。作成したユーザー名とパスワードのメモを取っておいてください。パスワードを設定する際には、ボックス \[User must change password at next logon\] をオフにします。

4. \[Active Directory Users and Computers\] で、新規ユーザー アカウント オブジェクトを右クリックして、\[Add to a group\] を選択します。
   
   ![新規ユーザーが作成されたら、そのユーザー名を見つけて右クリックし、ユーザーをグループに追加します。](images/addusertogroup.png "新規ユーザーをグループに追加")

5. \[Select Groups\] ダイアログ ウィンドウで、「Domain Admins」と入力して \[OK\] を選択します。
   
   > **注**: このアカウントは、ホスト プール作成プロセスを実行する際に、ホストをドメインに参加させるために使用されます。ドメイン管理者のアクセス許可を付与することで、ラボの手順が簡単になります。ただし、Active Directory アカウントが以下のアクセス許可を付与されていれば目的には十分です。これは、[Active Directory の制御の委任](https://danielengberg.com/domain-join-permissions-delegate-active-directory/)を使用して実行できます。
   
   ![次のウィンドウでは、このユーザーを Domain Admins グループに追加します。](images/addusertodomainadmins.png "Domain Admins グループへのユーザーの追加")

### タスク 4: Azure AD Connect の構成

1. ドメイン コントローラーのデスクトップで、**Azure AD Connect** のアイコンを見つけて開きます。
   
   ![ドメイン コントローラー VM のデスクトップで Azure AD Connect のアイコンを見つけます。](images/azureadconnect.png "Azure AD Connect のデスクトップ アイコン")

2. ライセンス条項とプライバシーに関する声明に同意してから、\[continue\] を選択します。次の画面では \[Use express settings\] を選択します。必要なコンポーネントがインストールされます。
   
   ![これにより、Azure AD Connect の設定画面に進みます。](images/AzureADconnectExpressSetting.png "Azure AD Connect の設定画面")

3. \[Connect to Azure AD\] ページで、Azure AD 全体管理者の資格情報を入力します。たとえば、[azadmin@MyAADdomain.onmicrosoft.com](mailto:azadmin@MyAADdomain.onmicrosoft.com) と正しいパスワードを入力します。\[Next\] を選択します。
   
   ![\[Use express settings\] を選択した後、次のウィンドウでは Azure Active Directory のユーザー名とパスワードを入力する必要があります。](images/adconnectazuresub.png "Azure AD Connect - Azure AD ログイン")
   
   > **注**: これは Azure サブスクリプションに関連付けられたアカウントです。

4. \[Connect to AD DS\] ページで、ドメイン管理者アカウントの Active Directory 資格情報を入力します。たとえば、ドメイン コントローラーの ARM テンプレートによるデプロイを使用した場合、資格情報は **[\[MyADDomain.com\]](http://myaddomain.com/) \\ADadmin** とパスワード **WVD@zureL@b2019!** のような組み合わせになります。\[Next\] を選択します。
   
   ![次のウィンドウで、AD DS ドメインと、管理者のユーザー名とパスワードを入力します。](images/azureadconnectdclogin.png "Azure AD Connect - ドメイン ログイン")
   
   > **注**: パスワードをコピーして貼り付ける場合は、末尾にスペースが入っていないことを確認してください。スペースがあると認証が失敗します。

5. \[Install\] を選択して、構成と同期を開始します。
   
   ![次のウィンドウで、すべての UPN サフィックスが一致していなくても続行するよう指定するボックスをオンにして、\[Next\] を選択して先に進みます。](images/azureadsigninconfig.png "Azure AD のサインインの構成")
   
   ![最後の設定ウィンドウで、同期プロセスを開始するよう指定するボックスをオンにして、\[Install\] を選択します。](images/azureadready.png "Azure AD Connect の [Ready to configure]")

6. 数分後、Azure AD Connect のインストールが完了します。\[Exit\] を選択します。
   
   ![インストールが完了すると、\[Configuration complete\] ウィンドウが表示されます。](images/AADCcomplete.png "[Configuration is completed] ウィンドウ")

7. ドメイン コントローラーの RDP セッションを最小化して、AD アカウントが Azure AD と同期するまで数分待ちます。

8. [Azure ポータル](https://portal.azure.com/)にサインインします。

9. 検索フィールドに「**Azure Active Directory**」と入力して、リストから選択します。

10. \[Azure Active Directory\] ブレードで、\[Manage\] の下の \[Users\] を選択します。

11. ユーザー アカウント オブジェクトのリストを調べて、テスト アカウントが同期していることを確認します。
    
    ![この画像は、Azure Active Directory に表示される、Azure AD Connect によって Active Directory から同期されたユーザーのリストを示しています](images/adconnectsync.png "同期されたユーザーのリスト")
    
    > **注**: Active Directory オブジェクトが Azure AD テナントと同期するまで、最長で 15 分かかることがあります。

## 演習 2: WVD 用 Azure AD グループの作成

所要時間: 30 分

この演習では、WVD 内のアプリケーション グループへのアクセスの割り当てを管理するために役立つ、Azure Active Directory (Azure AD) のグループについて学習します。WVD 用の新しい ARM ポータルは、Azure AD グループを使用したアクセスの割り当てをサポートします。この機能によって、アクセス管理が大幅に簡素化されます。このガイドでは、Azure Files で FSLogix に対する共有アクセス許可を管理するためにもグループを活用します。

3 つの Azure AD グループを作成して、個人用、プール、RemoteApp の各種アプリケーション グループへのアクセスを管理します。このガイドでは、RemoteApps 用に 1 つのグループのみを作成しますが、実稼働のシナリオでは、お客様によって定義されるアプリやペルソナに基づいて別々のグループを使用することが一般的です。作成するグループは以降の演習で使用するので、必ずメモを取っておいてください。

また、重要な留意点として、これらのグループを Windows Active Directory 環境で作成して、Azure AD Connect によって同期することもできます。グループ管理のプロセスがすでにオンプレミスで定義されているお客様の場合は、このシナリオも一般的です。

**追加リソース**

 |              |            |  
|----------|:----------:
| 説明| リンク
| Azure AD で基本グループを作成してメンバーを追加する| https://docs.microsoft.com/ja-jp/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal
| Azure AD Connect Sync| https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts
 |              |            | 

### タスク 1: Azure AD グループの作成

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**Azure Active Directory**」と入力します。リストから \[Azure Active Directory\] を選択します。

3. \[Azure Active Directory\] ページで、左側の \[Groups\] を選択して \[+ New group\] を選択します。

4. \[New Group\] ページで、以下のオプションを指定して \[Create\] を選択します。
   
   - **Group type:** Security
   
   - **Group name:** WVD Pooled Desktop User
   
   - **Membership type:** Assigned
   
   ![セキュリティ タイプの新規グループを作成し、グループ名は「WVD Pooled Desktop user」と入力します。](images/newGroup2.png "[New Group] ウィンドウ")

5. \[+ New group\] をもう一度選択し、以下のオプションを指定して \[Create\] を選択します。
   
   - **Group type:** Security
   
   - **Group name:** WVD Remote App All Users
   
   - **Membership type:** Assigned
   
   ![セキュリティ タイプの新規グループを作成し、グループ名は「WVD Remote App All users」と入力します。](images/newGroup1.png "[New Group] ウィンドウ")

6. \[+ New group\] をもう一度選択し、以下のオプションを指定して \[Create\] を選択します。
   
   - **Group type:** Security
   
   - **Group name:** WVD Persistent Desktop User
   
   - **Membership type:** Assigned
   
   ![セキュリティ タイプの新規グループを作成し、グループ名は「WVD Persistent Desktop user」と入力します。](images/newGroup3.png "[New Group] ウィンドウ")

7. \[Azure Active Directory\] に進み、\[Groups\] を選択して、グループが追加されたことを確認します。グループのリストの一番下までスクロールすると、作成した 3 つのグループがリストに含まれているはずです。
   
   ![Azure Active Directory の \[Groups\] に進んで、グループのリストを表示します。](images/aadgroups.png "Azure Active Directory の [Groups]")
   
   ![リストの一番下までスクロールして、作成された 3 つの新規グループを確認します。](images/aadnewgroups.png "Azure Active Directory の [Groups]")

### タスク 2: グループへのユーザーの割り当て

Azure AD グループが準備できたので、テスト用のユーザーを割り当てます。グループにユーザーを割り当てたら、WVD リソースの作成後にリソースへのアクセスを割り当てるためにこれらのグループを利用できます。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**Azure Active Directory**」と入力します。リストから \[Azure Active Directory\] を選択します。

3. \[Azure Active Directory\] ページで、左側の \[Groups\] を選択して、\[WVD Persistent Desktop User\] グループを選択します。

4. \[Members\] を選択して \[+ Add Members\] を選択します。
   
   ![\[Azure AD\] ブレード内で \[persistent desktop user\] グループにメンバーを追加します。](images/newMember.png "[Azure AD] ブレード")

5. 検索フィールドで、追加するユーザーの名前を入力し、**選択**してグループに追加します。

6. \[WVD Pooled Desktop User\] グループと \[WVD Remote App All Users\] グループに対して、ステップ 4 ～ 6 を繰り返します。
   
   この時点では、メンバーが割り当てられた新規 Azure AD グループが 3 つ存在します。このガイドで後ほど使用するので、追加したグループ名とアカウントのメモを取っておいてください。これらのグループを使用して、WVD アプリケーション グループにアクセス許可を割り当てます。
   
   ![ここには、それぞれのグループに追加する必要があるユーザーのリストが示されています。](images/aadwvdusers.png "Azure AD グループ")

## 演習 3: FSLogix 用の Azure ファイル共有の作成

所要時間: 90 分

この演習では、Azure ファイル共有を作成し、Active Directory 認証を使用して SMB アクセスを有効にします。Azure Files はプラットフォーム サービス (PaaS) で、WVD ユーザーの FSLogix コンテナーのホスティング用に推奨されるソリューションの 1 つです。この演習の手順を最後まで実施すると、以下のコンポーネントが作成されます。

- Azure サブスクリプション内の新規ストレージ アカウント。

- FSLogix プロファイル コンテナー用の新規 Azure ファイル共有。

- Azure ストレージ アカウントに対して有効な AD の認証。

- ファイル共有へのユーザー アクセスに適用されるアクセス許可。

**追加リソース**

| | | 
|----------|:----------:
| 説明| リンク
| Windows ファイル サーバー| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/create-host-pools-user-profile
| NetApp Files| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/create-FSLogix-profile-container
| Azure Files| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/create-profile-container-adds
| | | 

### タスク 1: ストレージ アカウントの作成

Azure ファイル共有を使用するには、まず Azure ストレージ アカウントを作成する必要があります。Azure ポータルで汎用 v2 ストレージ アカウントを作成するには、以下の手順を実行します。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**storage accounts**」と入力します。リストから \[Storage accounts\] を選択します。
   
   ![検索メニュー バーから、ストレージ アカウントを検索します。](images/storageaccount.png "ストレージ アカウントの検索")

3. 表示される \[Storage Accounts\] ウィンドウで、\[+ Add\] を選択します。

4. ストレージ アカウントに関して必要なパラメーターを指定します。使用可能なパラメーターの詳細については、以下の例を参照してください。\[Resource group\] と \[Storage account name\] に指定した値を含むメモを取っておいてください。Storage account nameは**15文字以下**で指定してください。これらは、この演習で後ほど必要になります。
   
   ![\[Add\] アイコンを選択してストレージ アカウントを新規に作成します。](images/addstorageaccount.png "ストレージ アカウントの追加")
   
   ![このブレードで、ストレージ アカウントを新規に作成するための情報を入力します。](images/createstorageaccount.png "ストレージ アカウントの作成")

5. \[Review + Create\] を選択してストレージ アカウントの設定を確認し、アカウントを作成します。

6. \[Create\] を選択します。

### タスク 2: Azure ファイル共有の作成

1. Azure ポータル ページの一番上にある \[Search resources\] フィールドに、「**storage accounts**」と入力します。リストから \[Storage accounts\] を選択します。

2. \[Storage accounts\] ブレードで、タスク 1 で作成したストレージ アカウントを選択します。

3. ストレージ アカウントの \[Overview\] ページで、\[File shares\] を選択します。
   
   ![ストレージ アカウントが作成されたら、\[overview\] ブレードで \[File shares\] を選択します。](images/storagefileshare.png "ファイル共有の作成")

4. \[File shares\] ブレードで、\[+ File Share\] を選択します。
   
   ![\[File shares\] の追加アイコンを選択して、ファイル共有を新規に作成します。](images/addfileshare.png "ファイル共有の追加")

5. 新規ファイル共有の名前を入力し、クォータをギガビット単位で入力して、**Hot** 層を選択、\[Create\] を選択します。
   
   ![ファイル共有に名前を付け、ストレージ クォータをギガビット単位で指定します。](images/newfileshare.png "新規ファイル共有")
   
   > **注**: ファイル共有クォータがサポートする最大容量は 5,120 GiB で、管理は \[File shares\] ブレードで行うことができます。

### タスク 3: ストレージ アカウントの AD 認証の有効化

**前提条件**

1. このタスクの手順は、ドメイン参加済みのコンピューターから実行する必要があります。**AzFilesHybrid** モジュールは AD PowerShell モジュールを使用するので、サーバーから実行することをお勧めします。
   
   ![VM デスクトップの PowerShell ISE アイコンを見つけて、そのアイコンを選択して開きます。](images/openpowersellise.png "PowerShell ISE を開く")

2. このタスクで使用するアカウントは、以下の要件を満たしている必要があります。
   
   - Azure AD と同期済み。
   
   - Active Directory にユーザーまたはコンピューター オブジェクトを作成するためのアクセス許可。
   
   - ストレージ アカウントに対する所有者または共同作成者の権限。

このタスクでは、全体管理者とドメイン管理者の割り当てを受けたアカウントを使用して、Azure のドメイン コントローラー上で手順を実行します。実稼働環境では、上記の最小要件を満たしている限り、この許可や権限の範囲を縮小できます。

**設定**

1. ドメイン参加済みのコンピューターから、[AzFilesHybrid モジュール](https://github.com/Azure-Samples/azure-files-samples/releases)をダウンロードして解凍します。
   
   **リンク アドレス**: https://github.com/Azure-Samples/azure-files-samples/releases
   
   ![これは、Azure サンプルの github サイトにアクセスすると表示される内容です](images/azfileshybriddownload.png "Azure サンプル")

2. GitHub リポジトリから、AzFilesHybrid.zip ファイルを選択して、ドメイン参加済みコンピューターの \[Documents\] フォルダーにダウンロードします。
   
   ![ファイルを保存するようにプロンプトで指示されたら、\[save as\] を選択して場所を選択します。](images/filesaveas.png)
   
   ![ウィンドウが開いたら、ファイルを保存する \[documents\] フォルダーを見つけます。](images/filedownload.png)

3. ダウンロードが完了した後、ファイル エクスプローラーでファイルの場所まで移動します。
   
   ![github リンクにアクセスして AzFilesHybrid ファイルをダウンロードした後、保存先のフォルダー内でこのファイルを見つけます。](images/azfileshybridzip.png "AzFilesHybrid モジュールの zip ファイル")

4. ローカル ドメイン コントローラーの \[Documents\] フォルダーにこのファイルを展開します。
   
   ![zip ファイルを開いて \[extract all\] を選択します。](images/extractzipfile.png "zip ファイルを [documents] に展開")
   
   ![zip ファイル内のファイルを展開する場所として \[documents\] フォルダーを選択します。](images/extractlocation.png "[documents] に展開")

5. デスクトップにある **PowerShell ISE** のアイコンを見つけて、管理者特権で PowerShell ISE ウィンドウを開きます。アイコンを右クリックし、\[Run as administrator\] を選択します。
   
   ![ドメイン コンピューターのデスクトップにある PowerShell アイコンを見つけて、右クリックして \[run as administrator\] を選択します。](images/runasadministrator.png)

6. 現在のユーザーに対する PowerShell 実行ポリシーを **Unrestricted** に設定します。
   
   ```
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. AzFilesHybrid を解凍した場所に移動します。次に例を示します。
   
   ```
   cd C:\Users\ADAdmin\Documents\AzFilesHybrid
   ```
   
   ![ファイルのパスは、ファイル エクスプローラーの \[documents\] フォルダーの場所であることが必要です](images/filelocation.png "[Documents] フォルダーのパス")

8. Az PowerShell モジュールをインストールします。
   
   ```
   if ($PSVersionTable.PSEdition -eq 'Desktop' -and (Get-Module -Name AzureRM -ListAvailable)) {
   Write-Warning -Message ('Az module not installed. Having both the AzureRM and ' +
     'Az modules installed at the same time is not supported.')
   } else {
   Install-Module -Name Az -AllowClobber -Scope CurrentUser
   }
   ```

9. AzFilesHybrid モジュールをインストールします。
   
   ```
   .\CopyToPSPath.ps1
   ```

10. AzFilesHybrid モジュールをインポートします。
    
    ```
    Import-Module -Name AzFilesHybrid
    ```
    
    ![これらのコマンドを実行した後、結果はこのスクリーンショットのようになります。](images/azimportresults.png "コマンドの結果")

11. 前提条件を満たすアカウントを使用してサインインします。
    
    ```
    Connect-AzAccount
    ```

12. サブスクリプション ID、リソース グループ名、ストレージ アカウントを所属するラボ環境に固有の情報に置き換えて、以下の PowerShell 変数を作成します。
    
    ```
    $SubscriptionId = "<subscription-id>"
    $ResourceGroupName = "<resource-group-name>"
    $StorageAccountName = "<storage-account-name>"
    ```
    
    > **注**: リソース グループ名とストレージ アカウント名は、タスク 1 で割り当てたものです。
    
    > **注**: **Get-AzSubscription** を実行して、使用可能なサブスクリプション名を検索できます。
    
    ![Get-AzSubscription コマンドを実行すると、ここでサブスクリプション ID が見つかります。](images/subscriptionid.png "サブスクリプション ID")

13. 現在のセッションのターゲット サブスクリプションを選択します。
    
    ```
    Select-AzSubscription -SubscriptionId $SubscriptionId
    ```

14. ストレージ アカウントを Active Directory ドメインに登録します。
    
    ```
    Join-AzStorageAccount -ResourceGroupName $ResourceGroupName
    
    ```
    
    > **注**: このコマンドを実行した後、Azure ストレージ アカウント名の入力を求めるプロンプトが表示されます。プロンプトは以下のスクリーンショットのようなものです。
    
    ![これは、join コマンドを実行した後に Azure ストレージ アカウントを入力するためのプロンプトです。](images/enterstorage.png)

15. スクリプトが完了すると、ストレージ アカウントに接続していることの確認が表示されます。
    
    ![これは、ストレージ アカウントの接続の確認です。](images/storageconfirmed.png "ストレージ アカウントの確認")

16. \[Active Directory Users and Computers\] で \[Domain controllers\] に進み、Azure ストレージ アカウントのコンピューター オブジェクトを見つけて、オブジェクトが正常に作成されたことを確認します。
    
    ![ここでは、新規に作成されたコンピューター オブジェクトが Active Directory にどのように表示されるかを示しています。](images/confirmnewobject.png "Active Directory オブジェクト")

17. 機能が有効になっていることを確認します。
    
    ```
    $storageaccount = Get-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName
    ```

18. 選択したサービス アカウントのディレクトリ サービスのリストを表示します。
    
    ```
    $storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions
    ```

19. ストレージ アカウントがファイル共有に対する AD 認証を有効に設定している場合は、ディレクトリ ドメイン情報のリストを表示します。
    
    ```
    $storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
    ```
    
    ![ここでは、前述の Powershell タスクを実行した場合の応答を示しています。](images/confirmpowershell.png "PowerShell タスクの応答")

20. Azure ポータルに移動し、ストレージ アカウントに進んで、\[Settings\] の下の \[Configuration\] を選択することによって、ドメインに対するアクティブ化を確認することもできます。以下の例に示すように、Active Directory (AD) 上のグループを参照してください。(**注:UIが新しくなっている場合は、Azure Active Directory Domain Services (Azure AD DS)を有効にしないようにしてください**)
    
    ![ストレージ アカウント構成で、Active Directory ドメイン サービスが有効になっていることがわかります](images/portalconfirm.png "ストレージ アカウントの構成")

これで、SMB 経由での AD 認証を正しく有効にして、AD の ID を使用して Azure ファイル共有へのアクセスを提供するカスタム ロールを割り当てることができました。

### タスク 4: 共有アクセス許可の構成

ユーザーやグループに共有レベルのアクセス許可を付与するために、Azure には以下の 3 つの組み込みロールが用意されています。

- **Storage File Data SMB Share Reader:** SMB を使用した Azure Storage ファイル共有の読み取りアクセスを許可します。

- **Storage File Data SMB Share Contributor:** SMB を使用した Azure Storage ファイル共有の読み取り、書き込み、削除のアクセスを許可します。

- **Storage File Data SMB Share Elevated Contributor:** SMB を使用した Azure Storage ファイル共有の読み取り、書き込み、削除、変更の NTFS アクセス許可を付与します。

ID ベースの認証によって Azure Files リソースにアクセスするには、ID (ユーザー、グループ、またはサービス プリンシパル) が共有レベルで必要なアクセス許可を受けていなければなりません。このプロセスは、Windows 共有アクセス許可を指定する場合と同様です (この場合は、特定のユーザーがファイル共有に対して実行できるアクセスの種類を指定します)。このタスクの説明では、ファイル共有に対する読み取り、書き込み、または削除のアクセス許可を ID に割り当てる方法を示します。

管理を簡単にするために、共有アクセス許可を管理するための 4 つのセキュリティ グループを Active Directory で新規に作成します。

1. ドメイン参加済みのコンピューターから、\[Active Directory Users and Computers\] を開きます。
   
   ![ドメイン コントローラーの VM サーバー マネージャー上でウィンドウを開き、\[Active Directory users and computers\] に進んでセキュリティ グループを新規に作成します。](images/adgroups.png "グループの新規作成")

2. Azure AD と同期している OU で、以下の Active Directory セキュリティ グループを作成します。
   
   - **AZF FSLogix Contributor**
     
     ![「AZF FSLogix Contributor」という名前のグループ オブジェクトを新規に作成します。](images/azfcontributor.png "AZF FSLogix Contributor")
   
   - **AZF FSLogix Elevated Contributor**
     
     ![「AZF FSLogix Elevated Contributor」という名前のグループ オブジェクトを新規に作成します。](images/azfelevcontributor.png "AZF FSLogix Elevated Contributor")
   
   - **AZF FSLogix Reader**
     
     ![「AZF FSLogix Reader」という名前のグループ オブジェクトを新規に作成します。](images/azfreader.png "AZF FSLogix Reader")
   
   - **WVD Users**
     
     ![「WVD User」という名前のグループ オブジェクトを新規に作成します。](images/wvduser.png "WVD User")

3. 前に作成した WVD 管理アカウントをグループ **AZF FSLogix Elevated Contributor** に追加します。このアカウントには、ファイル共有のアクセス許可を変更するためのアクセス許可が与えられます。
   
   ![前に作成した WVD 管理ユーザーを見つけ、右クリックしてグループに追加します](images/chooseadmin.png)

4. 「**AZF FSLogix Elevated Contributor**」と入力して、確認のために \[Check Names\] を選択します。\[Ok\] を選択して保存します。
   
   ![ここでは、このユーザーに AZF FSLogix Elevated Contributor グループを追加する方法を示しています。](images/addadmin.png)

5. Builtin グループに進み、「WVD Users」を見つけて右クリックし、\[Add to a group\] を選択して、グループ **WVD Users** をグループ **AZF FSLogix Contributor** に追加します。
   
   ![これは、WVD Users グループを見つけてグループに追加する方法を示しています。](images/wvduseraddtogroup.png)
   
   ![ここに FSLogix contributor グループの名前を入力し、追加する前に名前を確認します。](images/wvduseraddgroup.png)

6. **OrgUsers** を選択して、リストにあるユーザーすべてを選択することで、グループ **WVD Users** にユーザー アカウントを追加します。すべてのユーザーを選択し、右クリックしてグループに追加します。これらのユーザーには、FSLogix プロファイルを使用するためのアクセス許可が付与されます。また、このグループに **ADAdmin** ユーザーを必ず追加してください。
   
   ![組織のユーザーのリストに進み、ユーザーを選択して WVD Users グループに追加します。](images/wvdaddusers.png "WVD users グループへのユーザーの追加")

7. 新規グループが Azure AD と同期するまで待つか、`Start-ADSyncSyncCycle -PolicyType Delta`を PowerShell で実行します。これらのグループを確認するには、**Azure Active Directory** 内で \[Groups\] に進み、リストにある名前を調べてください。
   
   ![ここでは、ドメイン コントローラー上で作成されたグループが Azure AD と同期したことを確認します。](images/newgroups.png)
   
   Azure AD で新しいセキュリティ グループが使用可能になったら、以下の手順を実行して、Azure ポータルでこれらのグループをストレージ アカウントに割り当てます。これにより、AD セキュリティ グループを使用した共有アクセス許可の管理が可能になります。

8. Azure ポータルで、\[Search resources\] フィールドに「**storage accounts**」と入力し、リストから \[Storage accounts\] を選択します。
   
   ![Azure ポータルの検索バーで、ストレージ アカウントを検索します。](images/storageaccount.png "ストレージ アカウントの検索")

9. \[Storage accounts\] ブレードで、タスク 1 で作成したストレージ アカウントを選択します。

10. ストレージ アカウントのブレードで、\[File shares\] を見つけて選択します。

11. \[File shares\] ブレードで、ファイル共有を選択します。

12. \[Access Control (IAM)\] を選択します。

13. \[+ Add\] を選択して、\[Add role assignment\] を選択します。
    
    ![ストレージ アカウントの \[access control\] で、\[add a role assignment\] の下の \[add\] を見つけて選択します。](images/addroleassign.png "Azure AD のロール割り当ての追加")

14. \[Add role assignment\] フライ アウトで、以下のオプションを指定して \[Save\] を選択します。
    
    - **Role:** Storage File Data SMB Share Contributor
    
    - **Assign access to:** Azure AD user, group, or service principal
    
    - **Select:** AZF FSLogix Contributor

    ![Add the storage file data SMB share contributor role to the AZF FSLogix contributor role that were created within Active Directory.](images/azureadroleassigncontrib.png "Add FSLogix roles to Azure AD File share")

15. 残りの 2 つのロールに対して、以下を繰り返します。
    
    - Storage File Data SMB Share Elevated Contributor > AZF FSLogix Elevated Contributor
    
        ![Add the storage file data SMB share elevated contributor role to the AZF FSLogix elevated contributor role that were created within Active Directory.](images/azureadroleassignelev.png "Add FSLogix roles to Azure AD File share")

    - Storage File Data SMB Share Reader > AZF FSLogix Reader
    
    ![Active Directory 内で作成された AZF FSLogix Reader ロールに、\[storage file data SMB share reader\] ロールを追加します。](images/azureadroleassignreader.png "FSLogix ロールを Azure AD ファイル共有に追加")

### タスク 5: ファイル共有の NTFS アクセス許可の構成

Azure RBAC を使用して共有レベルのアクセス許可を割り当てた後、ルート、ディレクトリ、またはファイルのレベルで、適切な NTFS アクセス許可を割り当てる必要があります。共有レベルのアクセス許可は、ユーザーが共有にアクセスできるかどうかを決定する大まかなレベルのゲートキーパーと考えることができます。一方の NTFS アクセス許可は、より細かいレベルで効力を及ぼし、ディレクトリまたはファイルのレベルでユーザーが実行できる操作を決定します。

Azure Files は、NTFS アクセス許可を基本から高度なものまですべてサポートします。Azure ファイル共有のディレクトリとファイルに対する NTFS アクセス許可を表示し、構成するには、共有をマウントしてから Windows ファイル エクスプローラーを使用するか、Windows の icacls コマンドまたは Set-ACL コマンドを実行します。

NTFS アクセス許可を初めて構成するときには、スーパーユーザーのアクセス許可を使用して構成作業を行います。このためには、ストレージ アカウント キーを使用してファイル共有をマウントします。

> **注**: このタスクを実行するには、ストレージ アカウントでセキュア転送を無効にする必要があります。この設定にはストレージ アカウントの \[Configuration\] からアクセスでき、\[Secure transfer required\] の下の \[Disabled\] を選択します。\[Save\] を選択して、変更内容を保存します。

![\[configuration\] で、\[secure transfer required\] を無効にして保存します。](images/disablesecuretransfer.png)

1. Azure ポータルで、\[Search resources\] フィールドに「**storage accounts**」と入力し、リストから \[Storage accounts\] を選択します。

2. \[Storage accounts\] ブレードで、タスク 1 で作成したストレージ アカウントからファイル共有を選択します。

3. ストレージ アカウントのブレードで、\[Settings\] の下の \[Properties\] を選択します。**プライマリ ファイル サービス エンドポイント**のアドレスを見つけます。これは、ファイル共有へのアクセスに使用するパスです。
   
   ![ストレージ アカウントの \[properties\] ブレードを使用して、ストレージ アカウントのパスを見つけます。](images/storagefileendpoint.png)

4. パスを UNC フォーマットに変更して、メモ帳のファイルにコピーします。次に例を示します。
   
   https://mydomainazfiles.file.core.windows.net/ == \\\\mydomainazfiles.file.core.windows.net\\\<file-share-name>
   
   ![ここでは、フォーマットを変更した名前がドメイン コントローラーのメモ帳でどのように表示されるかを示しています。](images/notepadreformatted.png)

5. ストレージ アカウントのブレードで、\[Settings\] の下の \[Access keys\] を選択します。\[key1\] の値をコピーして、同じメモ帳ファイルに貼り付けます。
   
   ![ここでは、メモ帳にコピーするストレージ アカウント キーが見つかる場所を示しています。](images/copykey.png)

6. ドメイン参加済みのコンピューターから、標準のコマンド プロンプトを開き、ストレージ アカウント キーを使用してファイル共有をマウントします。管理者特権でのコマンド プロンプトを使用**しないでください**。このようにすると、マウント ポイントがファイル エクスプローラーに表示されなくなります。
   
   ![Windows で検索に進み、コマンド プロンプトを見つけて開きます。](images/opencommandprompt.png)
   
   > **注**: コマンドを作成するには、以下の例を参照してください。(スペース) と記載されている場所では、必ずスペースを入力します。net use z:(スペース) \\\\\<storage-account-name>.file.core.windows.net\\\<share-name>(スペース) \<storage-account-key>(スペース) /user:Azure\\\<storage-account-name>
   
       Example with sample values:
       
       net use z: \\mydomainazfiles.file.core.windows.net\FSLogix uPCvi+gP2qbCQcn3EATgbALE0H8nxhspyLRO2Nf9Hm2gMxfn/389/M33XHh7YEqNJ2GhbJXgStiifPwMBXk38Q== /user:Azure\mydomainazfiles
   
   ![コマンド プロンプトで、上記のスクリプト リストを実行して、ストレージ アカウントをネットワーク ドライブとして接続します。](images/cmdprompt.png "ドライブをマッピングするためのコマンド プロンプト スクリプト")
   
   > **注**: これはポート 445 での SMB 接続です。ほとんどのコンシューマー向け ISP は、既定でこのポートをブロックしています。ラボでこの演習を実施していて、ローカル コンピューターから共有をマウントする際に問題が生じた場合は、Azure でドメイン参加済みの VM からの接続を試してください。
   
   ![net use コマンドが正常に完了した後、コマンドが正常に完了したことを示すプロンプトが表示されます。また、ファイル エクスプローラーでネットワークの場所としてドライブが表示されるようになります。](images/successfulstoragemap.png)

7. \[File Explorer\] を開き、**Z:** ドライブを右クリックして \[Properties\] を選択します。

8. \[properties\] ウィンドウで、\[Security\] タブを選択して \[Advanced\] を選択します。
   
   ![ドライブのプロパティで、\[security\] フォルダーを選択して \[advanced\] を選択します。](images/drivesecurity.png)

9. \[Add\] を選択して、タスク 4 で適切なアクセス許可を指定して作成したそれぞれの AD セキュリティ グループを追加します。それぞれの名前を入力する際に \[check names\] を選択して、接続を確認します。
   
   ![セキュリティ設定で \[add\] を選択して新しいオブジェクトを追加します。](images/addsecurity.png)
   
   > **注**: この画像は、追加する必要があるすべてのオブジェクトを示していますが、一度に追加できるオブジェクトは 1 つだけです。  1 つを追加してから、4 つすべてが追加されるまでプロセスを繰り返します。

10. \[OK\] を選択して、変更内容を保存します。
    
    ![\[select a principal\] を選択して、\[select user, computer, service account, or group\] ウィンドウを開きます。\[enter the object name\] ウィンドウで、前に作成した FSLogix グループを入力します。名前を確認して \[ok\] を選択します。](images/addobjects.png)
    
    ![4 つのオブジェクトすべてをプリンシパルとして追加した後、これらのオブジェクトがアクセス許可エントリのリストに表示されます。](images/addsecuritycomplete.png)

### タスク 6: コンテナーの NTFS アクセス許可の構成

ルート ファイル共有で NTFS アクセス許可が適用されたので、FSLogix フォルダー構造と推奨される NTFS アクセス許可を作成できます。プロファイル コンテナーと Office コンテナーで使用するための、セキュアかつ機能的なストレージ アクセス許可を作成する方法は多数あります。以下に示すものは構成オプションの一例で、新規ユーザー機能を備え、管理アクセス許可をユーザーに付与する必要がありません。

このタスクでは、FSLogix プロファイルの種類ごとにディレクトリを作成し、推奨されるアクセス許可を割り当てます。

1. ファイル エクスプローラーでネットワーク ドライブに移動します。
   
   ![ここでは、前のタスクでマウントしたネットワーク ドライブが見つかります。](images/networkdrive.png)

2. ルート共有内に 3 つのフォルダー ディレクトリを新規に作成します。
   
   - **Profiles**
   
   - **ODFC**
   
   - **MSIX**
   
   ![これらのフォルダーを追加した後、その共有ドライブのファイル エクスプローラーはこのように表示されます。](images/newfolders.png)

3. \[Profiles\] ディレクトリを右クリックして、\[Properties\] を選択します。

4. \[Properties\] ウィンドウで、\[Security\] タブを選択して \[Advanced\] を選択します。

5. \[Disable inheritance\] を選択し、\[Remove all inherited permissions from this object\] を選択します。
   
   ![これは、継承されたアクセス許可を削除する画面です。](images/removeinheritedperm.png)

6. \[Add\] を選択し、\[AZF FSLogix Elevated Contributor\] を選択します。\[Full Control\] を付与して、\[Only apply these permissions to objects and/or containers within this container\] にチェックマークを付けます。\[OK\] を選択します。
   
   ![これらは、\[ok\] を選択する前に行う必要がある選択です。](images/addfullcontrol.png)

7. \[Add\] を選択して「**Creator owner**」を追加します。\[Full Control\] アクセス許可を付与し、\[Only apply these permissions to objects and/or containers within this container\] をオンにします。\[OK\] を選択します。
   
   ![これは、作成所有者オブジェクトを追加すると表示される内容です。](images/addcreatorowner.png)
   
   ![これらは、作成所有者にフル コントロールを付与するアクセス許可です。](images/addfullcontrolcreator.png)

8. \[Add\] を選択して「**WVD Users**」を追加します。以下のadvanced permissionsを付与し、\[Only apply these permissions to objects and/or containers within this container\] をオンにします。\[OK\] を選択します。
   
   - Traverse folder / execute file
   
   - List folder / read data
   
   - Read attributes
   
   - Create folders / append data
   
   ![これは、指定する必要がある WVD ユーザーの特殊なアクセス許可を示しています。](images/userfolderpermissions.png)

9. 両方のプロパティ ウィンドウで \[OK\] を選択して、変更内容を適用します。
   
   ![これは、作成したアクセス許可オブジェクトのリストです。](images/permissionscomplete.png)

10. \[ODFC\] ディレクトリに対してステップ 3 ～ 9 を繰り返します。

11. \[MSIX\] ディレクトリを右クリックして、\[Properties\] を選択します。

12. \[Properties\] ウィンドウで、\[Security\] タブを選択して \[Advanced\] を選択します。

13. \[Disable inheritance\] を選択し、\[Remove all inherited permissions from this object\] を選択します。
    
    ![これは、継承されたアクセス許可を削除する画面です。](images/removeinheritedperm.png)

14. \[Add\] を選択し、\[AZF FSLogix Elevated Contributor\] を選択します。\[Full Control\] アクセス許可を付与し、\[Only apply these permissions to objects and/or containers within this container\] をオンにします。\[OK\] を選択します。
    
    ![これらは、\[ok\] を選択する前に行う必要がある選択です。](images/addfullcontrol.png)

15. \[Add\] を選択して「**WVD Users**」を追加します。\[Read \& execute\] アクセス許可を付与し、\[Only apply these permissions to objects and/or containers within this container\] をオンにします。\[OK\] を選択します。
    
    ![これらは、MSIX フォルダーに対する WVD ユーザーのカスタム アクセス許可です。](images/msixwvdusers.png)

16. アクセス許可がスクリーンショットと一致していることを確認してください。

17. 両方のプロパティ ウィンドウで \[OK\] を選択して、変更内容を適用します。

これで、FSLogix プロファイル コンテナーのために Azure ファイル共有を使用する準備ができました。UNC パスをコピーして、FSLogix のデプロイ環境 (イメージ、GPO など) に追加してください。

## 演習 4: WVD のマスター イメージの作成

所要時間: 90 分

この演習では、WVD ホスト プール用のマスター イメージを作成する手順について説明します。マスター イメージの基本的な考え方は、Windows のクリーン ベース インストールから始めて、必須の更新、アプリケーション、構成を上に重ねていくことです。WVD 用のイメージを作成して管理する方法は多数あります。この演習で説明する手順では、コア アプリケーションと WVD の推奨構成オプションを含む、基本的なビルドとキャプチャのプロセスを進めます。

**追加リソース**

 |              |            |   
|----------|:----------:
| 説明| リンク
| Azure で一般化された VM の管理対象イメージを作成する| https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/capture-image-resource
| Azure で仮想マシンをデプロイする方法に関する詳細情報| https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/quick-create-portal
| Azure で Bastion ホストを設定する方法に関する詳細情報| https://docs.microsoft.com/ja-jp/azure/bastion/bastion-create-host-portal
 |              |            |   

### タスク 1: Azure での仮想マシン (VM) の新規作成

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. Azure ポータルのホーム ページで、\[Create a resource\] を選択します。

3. \[New\] ページで、**Microsoft Windows 10** を検索します。\[Windows 10 Enterprise multi-session, Version 1909\] を選択して、\[Create\] を選択します。
   
   ![このウィンドウには、ソフトウェア プラン Windows 10 Enterprise multi-session, Version 1909 を使用して Microsoft Windows 10 VM を新規に作成する画面が表示されます。](images/windows10VM.png "ソフトウェア プラン Windows 10 Enterprise multi-session, Version 1909 を使用した Microsoft Windows 10 VM の新規作成")
   
   > **注**: この演習では、最初にベース Windows 10 イメージを選択し、カスタム デプロイ スクリプトを使用して Office 365 ProPlus をインストールします。また、ここでは利用可能な最新の Windows 10 Enterprise マルチセッションを使用しますが、要件に応じたバージョンを選択できます。

4. \[Create a virtual machine\] ページで、必須フィールドを指定し、\[Review + create\] を選択して VM を作成します。
   
   ![これは、必要な構成の内容を示しています。](images/win10vmcreate.png "仮想マシンの作成")
   
   > **注**: VM の作成に使用した**ユーザー名**と**パスワード**のメモを取っておいてください。この情報は、作成後に VM にアクセスするために必要になります。
   
   > **注**: このガイドでは、Azure の VM の作成手順については説明しません。ただし、\[Inbound port rules\] では必ず \[RDP (3389)\] を許可するか、リモート アクセス用の要塞ホストをデプロイしてください。
   
   ![Windows 10 VM の Azure ポータル内の \[Create a virtual machine\] ページで、受信ポートとしてポート 3389 を許可します。](images/windows10VMcreate.png "Azure ポータル内の Windows 10 VM の [Create a virtual machine] ページ")

5. VM が正常にデプロイされた後、そのリソースに進み、RDP を使用して接続します。VM の作成時に指定した資格情報を使用してサインインします。
   
   ![Windows 10 VM の概要で \[connect\] を選択して、RDP で VM に接続します。](images/connectwin10vm.png)

6. RDP ファイルをダウンロードして、接続する RDP ファイルを開きます。
   
   ![この画像は、\[download RDP\] ボタンと、VM に接続するためにダウンロードされるファイルを示しています。](images/connectrdp.png)

### タスク 2: Windows Update の実行

Azure サポート チームは最善の努力を行っていますが、Marketplace のイメージは常に最新とは限りません。最も適切でセキュアな手段は、マスター イメージを最新の状態に保つことです。

1. マスター イメージ VM で、\[Settings\] アプリを開いて \[Updates \& Security\] を選択します。
   
   ![新規 Windows 10 VM イメージで、\[Settings\] ウィンドウに進んで \[Updates \& Security\] を選択します。](images/w10VMSettings.png "Windows 10 VM 内の [Settings] ウィンドウ")

2. 適用されていない更新をすべてインストールし、必要に応じて再起動します。

3. VM に修正プログラムが完全に適用された後、Windows Update の \[Settings\] ページは以下のスクリーンショットのようになります。
   
   ![すべての更新を確認して実行した後、Windows update が最新の状態であることを示す \[Settings\] ウィンドウ。](images/w10vmSettingsUpToDate.png "Windows update が最新の状態であることを示す [Settings] ウィンドウ")

### タスク 3: WVD イメージの準備

**スクリプトの概要**

このコンテンツの作成者は、いくつかの一般的なベースライン イメージのビルド タスクを自動化するために、スクリプトを使用したソリューションを作成しました。このスクリプトには UI フォームが含まれ、実行するアクションを簡単に選択できるようになっています。最終的な結果として、最適なユーザー エクスペリエンスのために必要なポリシーや設定と共に、Microsoft の主要なビジネス アプリケーションを取り込んだカスタム マスター イメージが作成されます。

スクリプトと関連ツールは、GitHub で管理されています。[ダウンロード リンク](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/shawntmeyer/WVD/tree/master/Image-Build/Customizations)

https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/shawntmeyer/WVD/tree/master/Image-Build/Customizations

スクリプトに関する詳しい説明については (例: パラメーター、機能など)、**Prepare-WVDImage.ps1** 内のコメントを参照してください。

スクリプト実行のトラブルシューティングについては、ターゲット マシン上でログ ディレクトリ **C:\\Windows\\Logs\\ImagePrep** を参照してください。

このスクリプトは、[Microsoft Security Compliance Toolkit (SCT)](https://docs.microsoft.com/ja-jp/windows/security/threat-protection/security-compliance-toolkit-10) の[ローカル グループ ポリシー オブジェクト (LGPO)](https://docs.microsoft.com/ja-jp/windows/security/threat-protection/security-compliance-toolkit-10#what-is-the-local-group-policy-object-lgpo-tool) ツールを活用して、イメージに設定を適用します。設定のドキュメントとエクスポートされた設定は、ターゲット マシンの **C:\\Windows\\Logs\\ImagePrep\\LGPO** にあります。このアプローチを採用した目的は、トラブルシューティングを簡素化し、グループ ポリシーの結果を利用できるようにすることです。

UI フォームは、以下のアクションを実行します。

**Office 365 ProPlus**

- Office 365 ProPlus 月次チャネルの**最新**バージョンをインストールします。

- 推奨設定を適用します。

- ソース ドキュメント: [マスター VHD イメージに Office をインストールする](https://docs.microsoft.com/ja-jp/azure/virtual-desktop/install-office-on-wvd-master-image)。

**OneDrive for Business**

- OneDrive for Business の**最新**バージョンをマシンごとにインストールします。

- ソース ドキュメント: [マスター VHD イメージに Office をインストールする](https://docs.microsoft.com/ja-jp/azure/virtual-desktop/install-office-on-wvd-master-image)。

**Microsoft Teams**

- Microsoft Teams の**最新**バージョンをマシンごとにインストールします。

- ソース ドキュメント: [Windows Virtual Desktop で Microsoft Teams を使用する](https://docs.microsoft.com/ja-jp/azure/virtual-desktop/teams-on-wvd)。

**Microsoft Edge Chromium**

- Microsoft Edge Enterprise の**最新**バージョンをインストールします。

- 推奨設定を適用します。

- ソース ドキュメント: [System Center Configuration Manager を使用して Microsoft Edge を展開する](https://docs.microsoft.com/ja-jp/deployedge/deploy-edge-with-configuration-manager)。

**FSLogix プロファイル コンテナー**

- FSLogix Agent の**最新**バージョンをインストールします。

- 推奨設定を適用します。

- ソース ドキュメント: [FSLogix をダウンロードしてインストールする](https://docs.microsoft.com/en-us/FSLogix/install-ht)。

**OS 設定**

- イメージ キャプチャ用の推奨 WVD 設定を適用します。

- ソース ドキュメント: [マスター VHD イメージを準備してカスタマイズする](https://docs.microsoft.com/ja-jp/azure/virtual-desktop/set-up-customize-master-image)。

- Azure VM のキャプチャ用の推奨設定を適用します。

- ソース ドキュメント: [Azure にアップロードする Windows VHD または VHDX を準備する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/prepare-for-upload-vhd-image)。

- ディスク クリーンアップを実行します。

- ソース ドキュメント: [cleanmgr](https://docs.microsoft.com/ja-jp/windows-server/administration/windows-commands/cleanmgr)。

**スクリプトの実行**

1. .zip ファイルをローカル ワークステーションに[**ダウンロード**](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/shawntmeyer/WVD/tree/master/Image-Build/Customizations)します。
   
   https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/shawntmeyer/WVD/tree/master/Image-Build/Customizations
   
   ![Microsoft Edge を開き、上記のリンクをブラウザーに貼り付けてファイルをダウンロードします。](images/savecustomizations.png)

2. .zip ファイルをローカル ワークステーションに**保存**します。マスター イメージ VM への RDP ウィンドウを開きます。.zip ファイルを \[documents\] フォルダーに**名前を付けて保存**します。

3. マスター イメージ VM 上で、デスクトップの .zip ファイルを右クリックして \[Extract All...\] を選択します。
   
   ![ファイルのダウンロード後、\[customizations\] ドキュメントを選択して、ファイルを \[documents\] に展開します。](images/extractcustomizations.png)

4. ファイルを **C:\\Documents** に展開します。

5. Windows 10 VM で「PowerShell」を検索して、管理者特権での PowerShell ウィンドウを開きます。右クリックして管理者として実行します。
   
   ![PowerShell アプリケーションを検索して右クリックし、管理者として実行します。](images/adminpowershell.png)

6. 「C:\\Documents\\Customizations」に移動します。
   
   ```
   cd C:\Documents\Customizations
   ```

7. 以下のコマンドを実行してスクリプトの実行を可能にします。
   
   ```
   Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force
   ```

8. 以下のコマンドを実行して、スクリプトを実行します。
   
   ```
   .\Prepare-WVDImage.ps1 -DisplayForm
   ```
   
   ![これは、PowerShell で実行する必要があるコマンドです。](images/prepareimage.png)

これにより、起動のための PowerShell フォームが開きます。以下の入力情報に基づいて、適切なオプションを選択してください。

![このスクリプトは、\[WVD golden image preparation\] ウィンドウを開きます。](images/wvdgoldenimage.png)

- Teams、Groove、Skype を除いた Office 365 ProPlus をインストールする場合は、\[Install Office 365\] を選択します。これを選択すると、下の電子メールとカレンダーのキャッシュに関する設定が有効になります。
  
  > **注**: これらの設定は必要に応じて更新してください。Microsoft の推奨設定は事前に選択されています。これらの設定をイメージに適用しないようにする場合は、それぞれを \[Not Configured\] に設定します。

- FSLogix Agent をインストールする場合は、\[Install FSLogix Agent\] を選択します。このオプションを選択すると、FSLogix ユーザー プロファイル コンテナーの VHD パスを指定するオプションが有効になります。イメージ内でこのオプションを指定しないようにする場合は、この設定を空白にします。

- マシンごとに OneDrive 同期クライアントをインストールする場合は、\[Install OneDrive per Machine\] を選択します。このオプションを選択すると、\[AAD Tenant ID\] フィールドが有効になります。ここにテナント ID を入力すると、イメージ内でサイレントの Known Folder Move 機能が有効になります。イメージ内でこのオプションを使用しない場合は、この値を空白にします。

- マシンごとに Teams をインストールする場合は、\[Install Microsoft Teams per Machine\] を選択します。

- Chromium ベースの Microsoft Edge Enterprise ブラウザーをインストールする場合は、\[Install Microsoft Edge Chromium v80+\] を選択します。

- イメージ内で Windows Update を無効にする場合は、\[Disable Windows Update\] を選択します。

- ディスク クリーンアップを実行する場合は、\[Run System Clean Up (CleanMgr.exe)\] を選択します。

![上記のオプションを選択した後、\[Execute\] を選択する前の準備はここに示すようになります。](images/goldenimagesettings.png)

9. 必要なオプションを選択したら、\[Execute\] を選択します。
   
   この時点でフォームが閉じ、スクリプトはイメージの構成を開始します。**スクリプトの実行が完了するまで、表示されている残りのウィンドウはどれも閉じないでください。** ウィンドウを閉じるとプロセスが中断し、最初からやり直す必要があります。
   
   選択したオプションに応じて、このスクリプトの完了には数分間かかります。この段階でユーザーからの追加入力は必要ないので、RDP セッションは最小化してその他の作業を行っていても構いません。

- Office 365 のインストールを選択した場合は、実行中に setup.exe のウィンドウが表示されます。

- OneDrive のインストールを選択した場合は、実行中に OneDrive のウィンドウが表示されます。

- システム クリーンアップの実行を選択した場合は、実行中に \[Disk Cleanup\] ウィザードが表示されます。Windows 内の古いファイルを並行して消去している間、数分間にわたってこのウィンドウが "Windows Update クリーンアップ" タスクを実行したままになる場合があります。
  
  ![WVD イメージ準備スクリプトのウィンドウが開き、スクリプトを実行できる状態になります。](images/WVHScript.png "WVD イメージ準備スクリプトのウィンドウ")
  
  > **注**: このスクリプトの実行には多少の時間がかかるので、しばらく何も起こらないように見えてもそのまま待ってください。しばらくすると、アプリケーションのインストールが始まります。PowerShell 内で状況を監視できます。\[Disk Cleanup\] ウィザードが閉じた後、PowerShell ウィンドウが更新されなくなる場合があります。このウィンドウは cleanmgr.exe プロセスの終了を待っており、これにもしばらく時間がかかることがあります。PowerShell ウィンドウを選択し、キーボードの上矢印を押し続けることで、アクティブなプロンプトを表示できます。
  
  ![これは、アプリケーションが WVD ゴールデン イメージにインストールされている間、PowerShell に表示される内容です。](images/powershellstatus.png)

10. スクリプトが完了した後、Windows のスタート アイコンを選択して、Office、Microsoft Edge Chromium、Microsoft Teams がインストールされていることを確認してください。
    
    ![ここで、新しくインストールされたアプリケーションを確認できます。](images/newapplications.png)

11. スクリプトの実行が完了した後、以下の最終タスクを実行します。

- C:\\BuildArtifacts ディレクトリを削除します。(存在する場合)

- デスクトップの .zip ファイルを削除します。

- ごみ箱を空にします。

- C:\\Windows\\Logs\\ImagePrep\\LGPO ディレクトリをローカル ワークステーションにコピーします。

- VM を再起動します。
  
  ![イメージの準備が完了した後、ダウンロードしたファイルを削除して、ごみ箱を空にします。](images/deletescripts.png)
  
  ![Windows のスタート メニューに移動して、Windows 10 VM を再起動します。](images/win10reboot.png)

### タスク 4: Sysprep の実行

1. VM の再起動後、RDP セッションを再接続してサインインします。

2. 管理コマンド プロンプトを開きます。

3. **C:\\Windows\\System32\\Sysprep** に移動します。
   
   ```
   cd C:\Windows\System32\Sysprep
   ```

4. 次のコマンドを実行して VM の sysprep を実行し、シャットダウンします。
   
   ```
   sysprep.exe /oobe /generalize /shutdown
   ```

システムは自動的にシャットダウンし、RDP セッションを切断します。

### タスク 5: マスター イメージ VM からの管理対象イメージの作成

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**virtual machines**」と入力します。リストから \[Virtual machines\] を選択します。
   
   ![Azure ポータルの検索バーで、「Virtual machines」を検索してサービスを選択します。](images/searchvm.png "仮想マシンの検索")

3. \[Virtual machines\] ブレードで、マスター イメージに使用した VM を見つけて、名前を**選択**します。

4. VM の \[Overview\] ブレードで、\[Status\] に \[Stopped\] と表示されていることを確認します。メニュー バーで \[Stop\] を選択して、割り当て解除の状態にします。
   
   ![これは、VM が実行中の場合に表示される内容です。\[stop\] を選択して VM の割り当てを解除してください。](images/vmrunning.png)
   
   ![VM が停止すると、停止済み、割り当て解除の状態が表示されます。](images/vmstopped.png)

5. 完了したら、メニュー バーの \[Capture\] を選択します。
   
   ![VM が停止した後、\[capture\] を選択して VM イメージをキャプチャできます。](images/vmcapture.png)

6. \[Create image\] ブレードで、必要なフィールドを指定して \[Create\] を選択します。
   
   ![これにより、Azure の \[Create Image\] ブレードが表示されます。](images/w10VMImage.png "Azure の [Create Image] ブレード")

7. 完了したら、ページの一番上にある**リソース検索フィールド**に「**images**」と入力します。リストから \[Images\] を選択します。

8. \[Images\] ブレードで、イメージを見つけて名前を**選択**します。
   
   ![イメージを検索する際には、このアイコンを選択する必要があります。](images/findimage.png)

9. イメージの \[Overview\] ブレードで、\[Name\] フィールドと \[Resource group\] フィールドのメモを取ります。これらの属性は、ホスト プールのプロビジョニングの際に必要になります。
   
   ![これは、メモを取っておく必要がある名前とリソース グループの情報です。](images/newimage.png)

### タスク 6: カスタム イメージを使用したホスト プールのプロビジョニング

1. 作成したこのカスタム イメージは、[演習 6](#演習-6-ホスト-プールの作成とプールされたリモート-アプリの割り当て) のホスト プールのプロビジョニングで利用します。

2. [演習 6](#演習-6-ホスト-プールの作成とプールされたリモート-アプリの割り当て) のステップ 5 で**仮想マシンの設定**を構成する際に、\[Browse all images and disks\] を選択してから、\[My Items\] のタブ オプションを選択して、作成されたイメージを選択します。
   
   ![ここでは、ホスト プールに追加するカスタム イメージを見つけます。](images/hostpoolcustom.png)

## 演習 5: 個人用デスクトップのホスト プールの作成

所要時間: 45 分

この演習では、プールされたデスクトップのために Windows Virtual Desktop ホスト プールを作成します。これは、必要に応じて稼働するコンピューターまたはホストの集合です。プール構成では、複数の非永続セッションのホスティングを行い、ユーザー プロファイル情報はローカルに保存されません。ここでは、FSLogix プロファイル コンテナーがホストにユーザー プロファイルを動的に提供します。このため、単一のホストのコンピューティング リソースを最大限に活用でき、リモート ワークステーションの全体的なオーバーヘッド、コスト、台数を削減できます。

**追加リソース**

  |              |            | 
|----------|:----------:
| 説明| リンク
| Azure ポータルを使用してホスト プールを作成する| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/create-host-pools-azure-marketplace
  |              |            | 

### タスク 1: ホスト プールとワークスペースの新規作成

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 「Windows Virtual Desktop」を検索して、リストから選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Manage\] の下の \[Host pools\] を選択し、\[+ Add\] を選択します。
   
   ![\[manage\] の下の \[host pools\] を選択し、\[add\] を選択して新規ホスト プールを追加します。](images/wvdHostPool.png "[Windows Virtual Desktop] ブレード")

4. \[Basics\] ページで、以下のスクリーンショットを参照して必要なフィールドを指定します。完了したら、\[Next: **Virtual Machines\] を選択します。**
   
   ![ここには、ホスト プールの情報を入力します。](images/createhostpool.png "[Create host pool] ページ")

5. \[Virtual Machines\] ページで、\[Windows 10 multi-user + M365 apps\] を指定して仮想マシンをプロビジョニングします。完了したら、\[Next: Workspace\] を選択します。仮想マシンの数は 2 でも問題ありません。

6. \[Image\] では、\[Browse all images and disks\] を選択し、検索して \[Windows 10 Enterprise multi-session, Version 1909 + Microsoft 365 Apps\] を見つけ、そのイメージを選択します。
   
   > **注**: このイメージを選択することは非常に重要です。この演習でアプリを割り当てるには、Microsoft 365 が必要です。
   
   ![これは、ホスト プール仮想マシンのために必要なイメージです。](images/vmwith365.png)
   
   ![このブレードでは、ホスト プール名の情報を入力し、仮想マシンの \[next\] を選択します。](images/nextworkspace.png)

7. \[Workspace\] ページで、\[Yes\] を選択して新規デスクトップ アプリ グループを登録します。\[Create new\] を選択して、**ワークスペース名**を指定します。\[OK\] を選択し、\[Review + create\] を選択します。
   
   ![\[Create a host pool\] の \[workspace\] タブで、必要な情報を入力します。](images/hostpoolWorkspace.png "[Create a host pool] の [workspace] タブ")

8. \[Create a host pool\] ページで、\[Create\] を選択します。

### タスク 2: ワークスペースのフレンドリ名の作成

ユーザーがサインインすると、ワークスペースの名前が表示されます。使用可能なリソースは、ワークスペース別に分類されます。ユーザーにわかりやすいように、新規ワークスペースにフレンドリ名を付けます。

> **注**: タスク 1 のデプロイが完了するまで、ワークスペースは表示されません。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 「**Windows Virtual Desktop**」を検索して、リストから選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Manage\] の下の \[Workspaces\] を選択します。更新するワークスペースを見つけて名前を選択します。
   
   ![タスク 1 で作成されたワークスペースを見つけて選択します。](images/workspaceproperties.png)

4. \[Settings\] の下の \[Properties\] を選択します。

5. \[Friendly name\] フィールドを適切な名前に更新します。
   
   ![ワークスペースのプロパティで、フレンドリ名の項目に名前を入力して保存します。](images/savefriendlyname.png)

6. \[Save\] を選択します。
   
   ![ワークスペースのプロパティ タブで、作成したワークスペースを確認します。](images/workspaceFriendlyName.png "ワークスペースのプロパティ タブ")

### タスク 3: アプリケーション グループへの Azure AD グループの割り当て

新しい Windows Virtual Desktop ARM ポータルでは、Azure Active Directory グループを使用してホスト プールへのアクセスを管理できるようになりました。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 「**Windows Virtual Desktop**」を検索して、リストから選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Manage\] の下の \[Application groups\] を選択します。

4. タスク 1 の手順の中で作成されたアプリケーション グループを見つけます。名前を選択します。
   
   ![ここでは、タスク 1 で作成したアプリケーション グループを見つけます。](images/wvdappgroups.png)

5. \[Manage\] の下の \[Assignments\] を選択し、\[+ Add\] を選択します。
   
   ![メニューで \[manage\] を見つけて \[assignments\] を選択し、\[add\] を選択します。](images/addassignments.png)

6. フライ アウトで、検索に「**WVD**」と入力して Azure AD グループの名前を見つけます。この演習では、**WVD Pooled Desktop Users** <!--と **AAD DC Administrators** -->を選択します。
<!--
   > **注**: AAD DC Administrators を選択することにより、演習 7 でリソースにアクセスするための Azure テナント ログインを使用できるようになります。
-->
   ![ここには、選択して保存する必要があるグループが示されています。](images/wvdpooleduseradd.png)

7. \[Select\] を選択して、変更内容を保存します。
   
   ![ユーザーとグループのリストで、\[WVD Pooled desktop users\] を見つけて選択します。](images/hostpoolusers.png "WVD のホスト プール ユーザー")

割り当てを追加したら、次の演習に進むことができます。Azure AD グループのユーザーを使用して、後の演習で新規ホスト プールへのアクセスを検証できます。

## 演習 6: ホスト プールの作成とプールされたリモート アプリの割り当て

所要時間: 45 分

この演習では、リモート アプリを公開するために非永続のホスト プールを作成します。これにより、デスクトップ全体ではなく、特定のアプリケーションへのアクセスをユーザーに割り当てることができます。この種類のアプリケーション デプロイは、さまざまな目的に役立ち、WVD よりも前から使用されてきた機能で、長年にわたって Windows Server リモート デスクトップ サービスに存在していたものです。

**追加リソース**

  |              |            | 
  |----------|:----------:
| 説明| リンク
| Windows Virtual Desktop で組み込みアプリを公開する| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/publish-apps
| Azure ポータルを使用してアプリ グループを管理する| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/manage-app-groups
  |              |            |  

### タスク 1: ホスト プールとワークスペースの新規作成

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 「**Windows Virtual Desktop**」を検索して、リストから選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Manage\] の下の \[Host pools\] を選択し、\[+ Add\] を選択します。
   
   ![\[manage\] の下の \[host pools\] を選択し、\[add\] を選択して新規ホスト プールを追加します。](images/wvdHostPool.png "[Windows Virtual Desktop] ブレード")

4. \[Basics\] ページで、以下のスクリーンショットを参照して必要なフィールドを指定します。ホスト プールの種類として \[Pooled\] を選択します。完了したら、\[Next: Virtual Machine\] を選択します。
   
   ![このブレードでは、リモート アプリのホストとなる仮想マシンの情報を入力し、ワークスペースの \[next\] を選択します。](images/remoteapppool.png)

5. **仮想マシンの設定**を構成する際に、\[Browse all images and disks\] を選択してから、\[My Items\] のタブ オプションを選択して、作成されたイメージを選択します。VM の数は 2 でも問題ありません。
   
   ![ここでは、ホスト プールに追加するカスタム イメージを見つけます。](images/hostpoolcustom.png)
   
   > **注**: このイメージを選択することは非常に重要です。この演習でアプリを割り当てるには、Microsoft 365 が必要です。また、複数の VM を指定してください。
   
   ![このブレードでは、ホスト プール名の情報を入力し、仮想マシンの \[next\] を選択します。](images/nextworkspace.png)

6. \[Workspace\] ページで、\[Yes\] を選択して新規デスクトップ アプリ グループを登録します。\[Create new\] を選択して、**ワークスペース名**を指定します。\[OK\] を選択し、\[Review + create\] を選択します。
   
   ![このブレードでは \[yes\] を選択してワークスペースを新規に作成します。完了したら \[review + create\] を選択します。](images/newworkspaceremoteapps.png)

7. \[Create a host pool\] ページで、\[Create\] を選択します。

### タスク 2: ワークスペースのフレンドリ名の作成

ユーザーがサインインすると、ワークスペースの名前が表示されます。使用可能なリソースは、ワークスペース別に分類されます。ユーザーにわかりやすいように、新規ワークスペースにフレンドリ名を付けます。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 「**Windows Virtual Desktop**」を検索して、リストから選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Manage\] の下の \[Workspaces\] を選択します。リモート アプリのために作成されたワークスペースを見つけて、名前を選択します。
   
   ![タスク 1 で作成されたワークスペースを見つけて選択します。](images/workspaceproperties.png)

4. \[Settings\] の下の \[Properties\] を選択します。

5. \[Friendly name\] フィールドを目的の名前に更新します。
   
   ![ワークスペースのプロパティで、フレンドリ名の項目に名前を入力して保存します。](images/savefriendlyname.png)

6. \[Save\] を選択します。
   
   ![ワークスペースのプロパティ タブで、作成したワークスペースを確認します。](images/workspaceFriendlyName.png "ワークスペースのプロパティ タブ")

### タスク 3: ホスト プールへのリモート アプリの追加

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. 「**Windows Virtual Desktop**」を検索して、リストから選択します。

3. \[Manage\] の下の \[Host pools\] を選択して、タスク 1 で作成したホスト プールを選択します。\[Application groups\] を選択し、\[Add\] を選択してアプリケーション グループを新規に作成します。
   
   ![\[Windows Virtual Desktop\] ブレードで、ホスト プールを選択してから \[add\] を選択してアプリケーション グループを追加します。](images/newappgroup.png "アプリケーション グループの管理")

4. \[Basics\] タブで、アプリケーション グループに名前を付けて \[Next: Assignments\] を選択します。\(次がアプリケーションであることもあります\)
   
   ![このブレードでは、アプリケーション グループの名前を入力します。](images/appgroupname.png)

5. \[assignments\] タブで、\[Add assignments\] を選択します。このガイドで前に作成した「**WVD Remote App All Users**」<!--と「**AAD DC Administrators**」-->を検索して、\[Select\] を選択します。
<!--
   > **注**: AAD DC Administrators を選択することにより、演習 7 でリソースにアクセスするための Azure テナント ログインを使用できるようになります。
-->

6. \[Next: Applications\] を選択します。
   
   ![\[application group\] ブレードで、ユーザーまたはユーザー グループの追加を選択して、次に開くブレードから \[WVD Remote App All users\] を選択します。](images/assigngroup.png)

7. \[Applications\] ページで、\[+ Add Application\] を選択します。

8. \[Add Application\] フライ アウトで、\[Application source\] の横の \[Start Menu\] を選択し、以下のアプリケーションを追加して、選択のたびに \[Save\] を選択します。
   
   - Outlook
   
   - Microsoft Edge
   
   - Microsoft Teams
   
   - Word
   
   - PowerPoint
   
   - Excel
   
   ![それぞれのアプリケーションを選択して保存した後、そのアプリケーションがアプリケーション リストに追加されます。](images/selectapps.png)
   
   ![最終的なアプリケーション リストはこのようになります。](images/listofapps.png)

9. \[Next: Workspace\] を選択します。

10. \[Workspace\] ページで、\[Yes\] を選択してアプリケーション グループを登録します。
    
    > **注**: \[Register application group\] フィールドには、ワークスペース名が自動的に取り込まれます。

11. \[Review + Create\] を選択します。
    
    ![ワークスペース名が自動的に取り込まれます。\[review + create\] を選択します。](images/remoteappws.png)

12. \[Create\] を選択します。

公開されるアプリを指定して、リモート アプリの非永続ホスト プールを正しく作成できました。後の演習で環境に接続する際に、この構成を検証できます。

## 演習 7: Web クライアントを使用した WVD への接続

所要時間: 30 分

この演習では、HTML5 Web クライアントを使用して WVD 環境に接続し、デプロイを検証する手順を説明します。以下のオペレーティング システムとブラウザーがサポートされています。

**追加リソース**

WVD リソースにアクセスするために使用できるクライアントは複数あります。それぞれのクライアントについて詳しくは、以下の Docs を参照してください。
  |              |            | 
|----------|:-------------:|
| 説明 | リンク |
| Windows デスクトップ クライアントを使用して接続する |  https://docs.microsoft.com/ja-jp/azure/virtual-desktop/connect-windows-7-and-10 | 
 HTML5 Web クライアントを使用して接続する |  https://docs.microsoft.com/ja-jp/azure/virtual-desktop/connect-web |
  Android クライアントを使用して接続する | https://docs.microsoft.com/ja-jp/azure/virtual-desktop/connect-android |
  macOS クライアントを使用して接続する |  https://docs.microsoft.com/ja-jp/azure/virtual-desktop/connect-macos |  
  iOS クライアントを使用して接続する | https://docs.microsoft.com/ja-jp/azure/virtual-desktop/connect-ios  
  |              |            |

### タスク 1: HTML5 Web クライアントを使用した接続

1. サポートされる Web ブラウザーを開きます。

2. https://rdweb.wvd.microsoft.com/arm/webclient に移動します。
   
   > **注**: 上記の URL にアクセスすると、ログインするように求められます。使用する資格情報は、ラボから提供されるものです。

3. アプリケーション グループに割り当てられている、同期した ID を使用してサインインします。
<!--   
   > **注**: 以前の演習でグループに **AAD DC Administrators** を追加した場合は、全体管理者の情報を使用できます。このユーザーは、Azure AD Connect を使用して AD DS と同期したユーザーであることが**必要です**。確認するには、Azure Active Directory の \[users\] に進み、ディレクトリ同期ユーザーを確認します。
 -->
   
   ![ここで、ユーザーがドメイン コントローラーと同期していることを確認します。](images/confirmsync.png)
   
   ![別のアカウントを使用してログインのメール アドレスを入力する場合に選択します。](images/useanotheraccount.png)
   
   ![ラボの Azure テナントのメール アドレスを入力します。](images/signinwithtenantadmin.png)
   
   ![入力したユーザー名のパスワードを入力します。](images/enterpw.png)

4. Web クライアントから使用可能なリソースを選択します。この例では、プールされたデスクトップを含むホスト プールに接続します。
   
   ![ポータルにログインした後、使用可能なアプリが表示されます。](images/appsavailable.png)

5. \[Access local resources\] プロンプトで、使用可能なオプションを確認して \[Allow\] を選択します。
   
   ![ここでは、既定のデスクトップを選択し、ローカル リソースを許可します。](images/allowlocal.png)

6. \[Enter your credentials\] プロンプトで、ステップ 3 と同じアカウントを使用してサインインし、\[Submit\] を選択します。
   
   > **注**: WVD デスクトップにログインするためのユーザー名とパスワードは、最初のデプロイ時に作成したドメイン コントローラーのユーザー名とパスワードからなる資格情報です。ユーザーのメール アドレスを確認する必要がある場合は、ドメイン コントローラー VM に RDP で接続し、\[Active Directory Users and Groups\] と \[OrgUsers\] でそのユーザーを見つけます。
   
   ![ドメイン コントローラー VM では、ここでユーザー名を見つけることができます。](images/dcusername.png)
   
   ![ドメイン コントローラーからのユーザー名と、最初のラボでのデプロイ時に作成したパスワードを入力します。](images/dccreds.png)

7. 接続した後、構成に関係のあるコンポーネントを確認します。デスクトップには、Microsoft Edge と Microsoft Teams のアイコンが表示されている必要があります。Windows スタート メニューに進むと、Office アプリケーションを確認できます。
   
   ![WVD のデスクトップ画像はこのようになっている必要があります。](images/wvddesktopimage.png)

**トラブルシューティング**

**Web クライアントが応答しなくなる、または切断する**

別のブラウザーまたはクライアントを使用して接続を試みてください。

ブラウザーを切り替えた後も問題が解消しない場合は、ブラウザーではなくネットワークに問題がある可能性があります。ネットワーク サポートに問い合わせることをお勧めします。

**Web クライアントが資格情報のプロンプトを出し続ける**

Web クライアントが資格情報のプロンプトを出し続ける場合は、以下の手順に従います。

1. Web クライアントの URL が正しいことを確認します。

2. 使用している資格情報が、その URL に関連付けられた Windows Virtual Desktop 環境のものであることを確認します。

3. ブラウザーの Cookie を消去します。

4. ブラウザー キャッシュを消去します。

5. ブラウザーをプライベート モードで開きます。

## 演習 8: WVD の監視の設定

所要時間: 45 分

この演習では、WVD ホスト プールの監視を設定します。監視は、トラブルシューティング、パフォーマンス、セキュリティなど、さまざまな理由で重要な役割を果たします。また、WVD サービスを構成するコンポーネントもさまざまなので、監視を実装する方法はお客様によって多少の違いがあります (サードパーティ ソリューションの追加など)。この演習を最後まで実施すると、以下の監視機能が有効になります。

- WVD サービスの診断ログ記録

- セッション ホスト VM に対する Azure Monitor

- セッション ホスト VM に対する Log Analytics Monitoring Agent

**追加リソース**

  |              |            | 
|----------|:----------:
| 説明| リンク
| 診断機能に Log Analytics を使用する| https://docs.microsoft.com/ja-jp/azure/virtual-desktop/diagnostics-log-analytics
| Azure でリソース ログとメトリックを収集するための診断設定を作成する| https://docs.microsoft.com/ja-jp/azure/azure-monitor/platform/diagnostic-settings
| Log Analytics ダッシュボードと Azure Monitor に関する PG ブログ| https://techcommunity.microsoft.com/t5/windows-it-pro-blog/proactively-monitor-arm-based-windows-virtual-desktop-with-azure/ba-p/1508735
| 1 つの Azure VM に対する監視を有効にする| https://docs.microsoft.com/ja-jp/azure/azure-monitor/insights/vminsights-enable-single-vm#enable-monitoring-for-a-single-azure-vm
| Azure Policy を使用して Azure Monitor for VMs を有効にする| https://docs.microsoft.com/ja-jp/azure/azure-monitor/insights/vminsights-enable-at-scale-policy
| Azure テンプレートを使用して Azure Monitor for VMs を有効にする| https://docs.microsoft.com/ja-jp/azure/azure-monitor/insights/vminsights-enable-at-scale-powershell
  |              |            |

### タスク 1: Log Analytics ワークスペースの作成

Log Analytics ワークスペースは、この演習で説明する監視機能のそれぞれに必要です。必要に応じて、異なるワークスペースにログ データをストリーミングすることもできます。ワークスペースを 1 つにするか、複数にするか決める際には、考慮すべき要因がいくつかあります。たとえば、RBAC の制御、クエリのパフォーマンス、レポートの作成などが考慮事項です。WVD 環境の場合は、1 つの専用ワークスペースをすべての監視データの宛先にすることが一般的です。

この演習では、環境専用のワークスペースを作成します。すでにワークスペースが作成済みの場合は、タスク 2 に進みます。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**log analytics**」と入力します。リストから \[Log Analytics workspaces\] を選択します。
   
   ![Azure ポータルの検索バーで、「log analytics」を検索してサービスを選択します。](images/searchloganalytics.png "Log Analytics の検索")

3. \[Log Analytics workspaces\] ブレードで、\[+Add\] を選択します。
   
   ![\[Log Analytics\] ブレードで、\[add\] を選択してワークスペースを新規に作成します。](images/createlogworkspace.png "Log Analytics ワークスペースの追加")

4. \[Create Log Analytics workspace\] ブレードで、以下の情報を指定して \[Review + Create\] を選択します。
   
   - **Subscription:** Log Analytics ワークスペースのための、目的のサブスクリプションを選択します。
   
   - **Resource Group:** リソース グループを新規に作成するか、リストから選択します。
   
   - **Name:** ワークスペースの一意の名前を入力します。
   
   - **Region:** ワークスペースのリージョンを選択します。

5. \[Create\] を選択します。
   
   ![\[create a Log Analytics workspace\] ブレードで、リソース グループを選択して、Log Analytics ワークスペースの一意の名前を入力します。](images/configlogworkspace.png "Log Analytics ワークスペースの構成")

右上隅の通知ベルを注視して、デプロイが完了するまで待ちます。完了したら、タスク 2 に進みます。

### タスク 2: WVD の診断ログ記録の有効化

ほかの多くの Azure サービスと同様に、WVD は監視とアラートに Azure Monitor を使用します。診断データの収集を可能にするために、監視するそれぞれの ARM オブジェクトに対して Azure Monitor を有効にする必要があります (たとえば、ホスト プール、アプリケーション グループ、ワークスペースなど)。有効にした後、ワークスペースにデータが表示されるまで数時間かかることがあります。

WVD ARM オブジェクトごとに、使用可能な診断データのカテゴリは異なります。たとえば、ホスト プール オブジェクトで使用できるオプションのセットは、ワークスペースとは異なります。それぞれのデータ カテゴリと関連オブジェクトの概要については、以下の表を参照してください。

  |              |            |     |
|----------|:-------------:|:-------------:|
| **カテゴリ**  | **説明**| **ARM オブジェクト**
| チェックポイント| アクティビティの有効期間の中で到達した具体的なステップ。たとえば、セッション中にユーザーが負荷分散によって特定のホストに割り当てられたときや、その後の接続中にユーザーがサインオンしたときなど。| ホスト プール、アプリケーション グループ、ワークスペース
| 接続| ユーザーがサービスに対する接続を開始し、完了したとき。| ホスト プール
| エラー| 特定のアクティビティに関してユーザーに何か問題が生じたか。この機能により、情報がアクティビティと結び付いている限りアクティビティ データを追跡するテーブルを生成できる。| ホスト プール、アプリケーション グループ、ワークスペース
| フィード| ユーザーはワークスペースに正常にサブスクライブできるか。ユーザーはリモート デスクトップ クライアントで公開されているすべてのリソースを見ることができるか。| ワークスペース
| ホスト登録| セッション ホストが接続時にサービスに正常に登録されたか。| ホスト プール
| 管理| API または PowerShell を使用して Windows Virtual Desktop オブジェクトを変更する試みが正常に行われたかどうかを追跡する。たとえば、いずれかのユーザーが PowerShell を使用してホスト プールを正常に作成できたかどうか。| ホスト プール、アプリケーション グループ、ワークスペース

### タスク 3: ホスト プールのログ記録の有効化

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**windows virtual desktop**」と入力します。リストから \[Windows Virtual Desktop\] を選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Windows Virtual Desktop\] ブレードで、\[Manage\] の下の \[Host pools\] を選択します。

4. \[Windows Virtual Desktop \| Host pools\] ブレードで、ホスト プールを見つけて名前を選択します。

5. ホスト プールのブレードで、\[Monitoring\] の下の \[Diagnostic settings\] を選択します。

6. \[Diagnostics settings\] ブレードで、\[+Add diagnostic setting\] を選択します。
   
   ![ホスト プールのブレードで、\[monitoring\] メニュー見出しの下の \[diagnostic settings\] を見つけて、\[add diagnostic settings\] を選択します。](images/hostpooldiag.png "ホスト プールの診断設定の追加")

7. \[Diagnostic settings\] ページで、以下の情報を指定して \[Save\] を選択します。
   
   - **Diagnostic settings name:** これらの設定の名前を入力します。ARM オブジェクトには複数の診断設定を適用できます。
   
   - **Category details \| log:** 収集するログのボックスをオンにします。
   
   - **Destination details \| Send to Log Analytics:** このボックスをオンにします。
     
     - **Subscription:** 目的のサブスクリプションを選択します。
     
     - **Log Analytics workspace:** 目的のワークスペースを選択します。
   
   ![\[diagnostics settings\] ブレードで、名前を作成し、ログを選択して、\[send to Log Analytics\] をオンにします。](images/hostpooldiagsettings.png "ホスト プールの診断設定")

### タスク 4: アプリケーション グループのログ記録の有効化

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**windows virtual desktop**」と入力します。リストから \[Windows Virtual Desktop\] を選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Windows Virtual Desktop\] ブレードで、\[Manage\] の下の \[Application groups\] を選択します。

4. \[Windows Virtual Desktop \| Application groups\] ブレードで、アプリケーション グループを見つけて名前を選択します。

5. アプリケーション グループのブレードで、\[Monitoring\] の下の \[Diagnostic settings\] を選択します。

6. \[Diagnostics settings\] ブレードで、\[+Add diagnostic setting\] を選択します。
   
   ![作成されたアプリケーション グループを選択して、\[add diagnostic settings\] を選択します。](images/appgroupadddiag.png "アプリケーション グループの診断設定の追加")

7. \[Diagnostic settings\] ページで、以下の情報を指定して \[Save\] を選択します。
   
   - **Diagnostic settings name:** これらの設定の名前を入力します。ARM オブジェクトには複数の診断設定を適用できます。
   
   - **Category details \| log:** 収集するログのボックスをオンにします。
   
   - **Destination details \| Send to Log Analytics:** このボックスをオンにします。
     
     - **Subscription:** 目的のサブスクリプションを選択します。
     
     - **Log Analytics workspace:** 目的のワークスペースを選択します。
   
   ![\[diagnostics settings\] ブレードで、名前を作成し、ログを選択して、\[send to Log Analytics\] をオンにします。](images/appgroupdiagsettings.png "アプリケーション グループの診断設定")

### タスク 5: ワークスペースのログ記録の有効化

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**windows virtual desktop**」と入力します。リストから \[Windows Virtual Desktop\] を選択します。
   
   ![Azure ポータルの検索バーで、「windows virtual desktop」を検索してサービスを選択します。](images/searchwvd.png "Windows Virtual Desktop の検索")

3. \[Windows Virtual Desktop\] ブレードで、\[Manage\] の下の \[Workspaces\] を選択します。

4. \[Windows Virtual Desktop \| Workspaces\] ブレードで、ワークスペースを見つけて名前を選択します。

5. ワークスペースのブレードで、\[Monitoring\] の下の \[Diagnostic settings\] を選択します。

6. \[Diagnostics settings\] ブレードで、\[+Add diagnostic setting\] を選択します。
   
   ![作成されたワークスペースを選択して、\[add diagnostic settings\] を選択します。](images/workspaceadddiag.png "ワークスペース診断ログ記録の追加")

7. \[Diagnostic settings\] ページで、以下の情報を指定して \[Save\] を選択します。
   
   - **Diagnostic settings name:** これらの設定の名前を入力します。ARM オブジェクトには複数の診断設定を適用できます。
   
   - **Category details \| log:** 収集するログのボックスをオンにします。
   
   - **Destination details \| Send to Log Analytics:** このボックスをオンにします。
     
     - **Subscription:** 目的のサブスクリプションを選択します。
     
     - **Log Analytics workspace:** 目的のワークスペースを選択します。
   
   ![\[diagnostics settings\] ブレードで、名前を作成し、ログを選択して、\[send to Log Analytics\] をオンにします。](images/appgroupdiagsettings.png "ワークスペース診断設定")

この時点では、種類ごとに少なくとも 1 つの WVD ARM オブジェクトに関する診断データが有効になっています。追加のオブジェクトの監視を有効にする場合は、上記の手順を最初から繰り返します。

### タスク 6: セッション ホストに対する Azure Monitor の有効化

Azure Monitor は、セッション ホスト VM のパフォーマンスと正常性を監視するために WVD で活用されます。この機能は、いくつかの方法で Azure VM に対して有効にすることができます。この演習では、Azure ポータルを使用して VM 上で Azure Monitor を有効にする手順を説明します。自動化についてのガイダンスは、以下のリンクを参照してください。

1. [Azure ポータル](https://portal.azure.com/)にサインインします。

2. ページの一番上にある \[Search resources\] フィールドに、「**virtual machines**」と入力します。リストから \[Virtual machines\] を選択します。
   
   ![Azure ポータルの検索バーで、「Virtual machines」を検索してサービスを選択します。](images/searchvm.png "仮想マシンの検索")

3. \[Virtual Machines\] ブレードで、セッション ホスト VM を見つけて名前を選択します。

4. 仮想マシンのブレードで、\[Monitoring\] の下の \[Insights\] を選択します。
   
   ![ドメイン コントローラー VM のメニューで、\[monitoring\] の下の \[insights\] を選択します。](images/vminsights.png "仮想マシンの洞察")

5. \[Insights\] ブレードで、\[Enable\] を選択します。これにより、検証チェックが開始されます。
   
   ![\[insights\] ブレードで、\[enable\] を選択して仮想マシンに対する洞察を有効にします。](images/enableinsights.png "VM の洞察の有効化")
   
   > **注**: Azure Monitor を有効にするには、仮想マシンが実行されている必要があります。

6. \[Insights setup\] ページで、以下の情報を指定して \[Enable\] を選択します。
   
   - **Workspace Subscription:** 目的のサブスクリプションを選択します。
   
   - **Choose a Log Analytics Workspace:** 前のタスクで使用したワークスペースを選択します。
   
   ![\[enable insights\] ブレードで、Log Analytics ワークスペースを選択して \[enable\] を選択します。](images/vminsightsconfig.png "VM の洞察を得るための Log Analytics ワークスペースの構成")

右上隅の通知ベルを注視して、デプロイが完了するまで待ちます。この処理には 5 ～ 10 分かかります。完了すると、VM 上で構成された以下の項目が表示されます。

**新規に追加された VM 拡張**。Azure の \[VM Extensions\] ブレードに 2 つの拡張が新規に追加されます。

**新規にインストールされた監視エージェント**。2 つのエージェントが VM に新規にインストールされます。

**Log Analytics 用に構成された監視エージェント**。Log Analytics ワークスペース用に Microsoft Monitoring Agent が構成されます。

この時点では、少なくとも 1 つのセッション ホスト VM 上で Azure Monitor が有効になっています。追加のセッション ホストに対して Azure Monitor を有効にするには、上記の手順を繰り返します。自動化する場合は、Azure Policy または PowerShell を利用して複数の VM の監視を有効にします。

**Kusto クエリの例**

クエリの例の一覧については、以下の Docs 記事を参照してください。

[**診断機能に Log Analytics を使用する \| クエリの例**](https://docs.microsoft.com/ja-jp/azure/virtual-desktop/diagnostics-log-analytics#example-queries)

**トラブルシューティング**

- Log Analytics ワークスペースに進み、\[Logs\] を選択します。クエリの例の UI が自動的に表示されます。
- フィルタを \[Category\] に変更します。
- \[Windows Virtual Desktop\] を選択して、使用可能なクエリを確認します。
- \[Run\] を選択して、選択したクエリを実行します。

WVD のデプロイ中に、ドメインへのマシンの参加、または自動化によるエージェントのインストールに関して、問題が生じる場合がよくあります。

## ハンズオン ラボの後に

所要時間: 15 分

\| 警告: 続ける前に、このラボに使用したリソースをすべて削除する必要があります。**Azure ポータル**でこの手順を行うには、\[Resource groups\] をクリックします。作成したいずれかのリソース グループを選択します。\[resource group\] ブレードで、\[Delete Resource group\] をクリックして、リソース グループ名を入力し、\[Delete\] をクリックします。作成したその他のリソース グループがあれば、この手順を繰り返します。**この手順を実行しなければ、ほかのラボで問題が生じる可能性があります。**\|

### タスク 1: リソース グループを削除してラボ環境を削除する

1. **Azure ポータル**に移動します。

2. \[Resource groups\] に進みます。
   
   ![Azure ポータルの検索バーで、「resource groups」を検索してサービスを選択します。](images/searchresourcegroup.png "リソース グループの検索")

3. リソースを作成した**リソース グループ**を選択します。

4. \[Delete Resource group\] を選択します。
   
   ![このラボのために作成したリソース グループを見つけて選択し、これらのリソース グループの 1 つを選択して、\[delete resource group\] を選択します。](images/resourcegroup1.png "リソース グループの選択")

5. **リソース グループ**の名前を入力して、\[Delete\] を選択します。
   
   ![開いたブレードで、リソース グループの名前を完全に入力して \[delete\] を選択します。](images/deleteresourcegroup1.png "リソース グループの削除")

6. このラボで作成したすべての**リソース グループ**に対してこの手順を繰り返します (**Azure Monitor** と **Log Analytics** 用のものを含む)。

ここで挙げられた手順のすべては、ハンズオン ラボの "参加後" に行う必要があります。