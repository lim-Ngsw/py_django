# py_django

## 1. VSCodeを使いRemote-Containerを開く
## 2. コンテナに入った後、/src直下へ移動し、プロジェクトを立ち上げる
        - ターミナルへ入力django-admin startproject <任意のプロジェクト名>
        - docker-compose.yml内の『mysite』部分の記載を、上記で設定した<任意のプロジェクト名>に変更する

## 3. プロジェクト内のsettings.pyを編集
        - データベースの接続設定を変更
        DATABASES = {
        'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': 'django_db',
                'USER': 'django_user',
                'PASSWORD': 'djangopass',
                'PORT': '3306',
                'HOST': 'db',
                }
        }

        - Static files (CSS, JavaScript, Images)に追加
        STATIC_URL = '/static/'
        STATIC_ROOT = '/static/' # ←追加

## 4. 引き続きプロジェクト内のsettings.pyを編集（管理者がサイトを見れるように設定する）
        - ALLOWED_HOSTS = []

## 5. Djangoのデータベース構築と管理者ユーザー作成
        - プロジェクトフォルダ内の'manage.py'のある階層に移動する
        - DBをマイグレーション
        python3 manage.py migrate

        - 管理者の設定
        python3 manage.py createsuperuser
        （nameはroot、Emailはブランクでよい、passはnameと同じフレーズは不可）

## 6.静的ファイルをコピー
        - staticディレクトリにCSSやJavaScriptをコピー

## 7.コンテナをRebuild
        - VSCode左下の緑ボタンからRemote-ContainerのRebuildを実行し、コンテナに設定を反映させる。

        - 再度VSCodeのRemote-Containerでコンテナ環境に入る
        （docker-desktopでDocker DesktopでdjangoコンテナがRUNNINGになり、内部でdjango_db_1,django_nginx_1,django_python_1がRUNNINGになっていればOK）

## 8.サーバーを起動しアクセスしてみる
        - /src/プロジェクト名直下へ移動し、サーバーを起動しアクセスしてみる
        python3 manage.py runserver

## 注意事項
        - 上記サーバーへローカルPCのchromeブラウザからアクセスできなかった場合、VSCodeのターミナルにALLOWED_HOSTSに追加すべきIPアドレスが提示されるので、settings.pyを開き、ALLOWED_HOSTSにIPアドレスを追加し、コンテナをリビルドする。(IPアドレスは""で囲う必要があることに注意。記入例 = ["localhost","192.168.64.8",]

        - もしリビルドが上手くいかない場合はdocker desktopからimagesとcontainersを全消去し、VSCodeで開いている.devcontainerを削除した後にVSCodeでRemote Containerで再度コンテナをビルドする

サーバーのアドレスの後に /admin をつけると管理画面にアクセスできるので先ほど設定した管理者ユーザー（nameとpass）でログインが可能
