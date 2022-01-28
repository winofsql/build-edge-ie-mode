### Edge の IEモードを使用して IE で使用していた ActiveX 利用等の特別なサイトを引き続き利用する手順

<br>

## 対象 URL を IEモードする為の XML を作成する
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
