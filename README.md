### Edge の IEモードを使用して IE で使用していた ActiveX 利用等の特別なサイトを引き続き利用する手順

- 対象 URL を IEモードする為の XML を作成する
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
