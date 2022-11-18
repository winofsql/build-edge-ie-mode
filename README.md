### Edge の IEモードを使用して IE で使用していた ActiveX 利用等の特別なサイトを引き続き利用する手順

<br>

## 対象 URL を IEモードにする為の XML を作成する
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge]
"EnterpriseModeSiteListManagerAllowed"=dword:00000001

```

- 上記テキストを .reg で保存してエクスプローラからインポートするか、以下のコマンドを管理者権限のコマンドプロンプトから実行

```
reg add HKLM\SOFTWARE\Policies\Microsoft\Edge /v EnterpriseModeSiteListManagerAllowed /t REG_DWORD /d 1 /f
```

- Edge を起動してアドレスバーに edge://compat と入力
- エンタープライズ サイト リスト マネージャーを選択
- サイトの追加をクリック

![image](https://user-images.githubusercontent.com/1501327/151492507-56006468-02af-4f8b-9738-c2a4498bb7a6.png)

![image](https://user-images.githubusercontent.com/1501327/151492626-61115803-f1d0-48d3-ad8f-5108e5443ee4.png)

- 登録したサイトを XML ファイルとして取得する為に、XML にエクスポートをクリックして保存します
  - ( 保存後は、保存した XML を使うように設定するのでエントリは削除します )

<br>

## XML に記録したサイトを IEモードで開けれるようにする
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge]
"InternetExplorerIntegrationLevel"=dword:00000001
"InternetExplorerIntegrationSiteList"="C:\\Users\\ユーザ名\\AppData\\Roaming\\sites.xml"

```

<br>

- 上記テキストを .reg で保存してエクスプローラからインポートするか、以下のコマンドを管理者権限のコマンドプロンプトから実行
  - ファイルのパスやファイル名は自由です
  - %appdata% を使えば、多くの PC で共通のインストーラを作成する場合に適していると思います

<br>

```
reg add HKLM\SOFTWARE\Policies\Microsoft\Edge /v InternetExplorerIntegrationSiteList /t REG_SZ /d C:\Users\ユーザ名\AppData\Roaming\sites.xml /f
```

<br>

![image](https://user-images.githubusercontent.com/1501327/151495246-3e7c82bf-b7be-43b2-87c9-db4df1ec1104.png)

```xml
<site-list version="1.0">
  <site url="http://localhost">
    <compat-mode>Default</compat-mode>
    <open-in>IE11</open-in>
  </site>
</site-list>
```

## 対象サイトのインターネットオプションを設定する

- UI からはコントロールパネルのインターネットオプションを使用します

![image](https://user-images.githubusercontent.com/1501327/151499552-5764f687-adbc-473f-9c2f-ff8075090544.png)


- 信頼済サイトはレジストリから以下のようになります( http のみのサンプル / サブドメインはツリーになります )
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\localhost]
"http"=dword:00000002


```

| Value | Setting Zones |
| ------------- | ------------- |
| 0  | My Computer  |
| 1  | Local Intranet Zone  |
| 2  | **Trusted sites Zone**  |
| 3  | Internet Zone  |
| 4  | Restricted Sites Zone  |


- スクリプトを実行しても安全だとマークされていない ActiveX コントロールの初期化とスクリプトの実行
- ドメイン間でのデータ ソースのアクセス
- **上記を可能にするには以下のようになります**

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\Zones\2]
"1201"=dword:00000000
"1406"=dword:00000000

```

<br>

- [Internet Explorerユーザーのセキュリティ ゾーン レジストリ エントリの管理](https://docs.microsoft.com/ja-JP/troubleshoot/developer/browsers/security-privacy/ie-security-zones-registry-entries)<br><br>
  - 特に指定しない限り、各 DWORD 値は 0、1、または 3 に等しくなります。 
  - 通常、**0 の設定は、許可されている特定のアクションを設定します**
  - 1 の設定を指定するとプロンプトが表示されます
  - 3 の設定では特定のアクションが禁止されます



### デバッグ方法
```
%systemroot%\system32\f12\IEChooser.exe
```
