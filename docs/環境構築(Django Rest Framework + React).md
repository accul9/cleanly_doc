# DRF + React 環境構築(Django Rest Framework + React)
※ Mac で構築しています。Windows の方は適宜読み替えていただければと思います  
※ とりあえず動作することを優先しましたので、セキュリティなどは一切考慮していないです  
※ 勉強のために毎回インストールしていますが慣れている方は requirements.txt でまとめてインストールでいいと思います  
※ おかしい部分があれば随時修正していきます(React は構築したことがないのでご確認いただければと思います)  

---

1.アプリ名に沿ったフォルダを作成します(今回は cleanly としています)

### Backend 構築

2.各フォルダとファイルを作成
```
- cleanly(フォルダ)
    - backend_project(フォルダ)
        - backend(フォルダ)
        - Dockerfile(ファイル)
    - compose.yml(ファイル)
```

---

・Dockerfile 
```Dockerfile
FROM python:3.13.2-slim

RUN mkdir /backend
WORKDIR /backend

RUN pip install --upgrade pip
RUN pip install --upgrade setuptools
```

・compose.yml  
```yml
services:
  backend:
    build:
      context: ./backend_project
      dockerfile: Dockerfile
    container_name: 'cleanly_backend'
    tty: true
    ports:
      - 8000:8000
    volumes:
      - ./backend_project/backend:/backend
```

---

3.cleanly 内で下記コマンドで docker を起動します
```bash
% docker compose up -d
```

4.コンテナ内へ入ります
```bash
% docker exec -it cleanly_backend /bin/bash
```

5.DRF に必要なパッケージをインストール  
※バージョンは固定にした方がいいかもですが、今のところ指定なしでインストールされるのは下記バージョンになります。  
`django: 5.1.6 / djangorestframework: 3.15.2 / python-dotenv: 1.0.1`
```bash
% pip install django
% pip install djangorestframework
% pip install python-dotenv
```

6.django でプロジェクトを作成
```bash
% mkdir cleanly_backend
% cd cleanly_backend
% django-admin startproject config .
```

#### 以下から cleanly_backend内で作業

---

7.下記コマンドをたたき app を作成
```bash
% python manage.py startapp apiv1
```

8.env ファイルを追加
```env
DJANGO_SECRET_KEY=... # config/settings.py に記載されている SECRET_KEY
DEBUG=True
```

9.設定ファイル(config/settings.py)を編集  
・ファイル上部に追記  
```python
from dotenv import load_dotenv
import os

# .envファイルの内容を読み込む
load_dotenv() 
```

・対応する場所に追加、変更  
SECRET_KEY を `os.getenv('DJANGO_SECRET_KEY', '')` に変更  
DEBUG を `os.getenv('DEBUG', 'True')` に変更  
INSTALLED_APPS に `'rest_framework'`、`'apiv1'` を追加  
LANGUAGE_CODE を `'ja'` に変更  
TIME_ZONE を `'Asia/Tokyo'` に変更  

---

10.マイグレーション
```bash
% python manage.py migrate
```

11.管理ユーザーを作成します  
(ユーザ名、メールアドレス(Skip 可)、パスワードを任意で設定してください)
```bash
% python manage.py createsuperuser
```

12.サーバー起動
```bash
% python manage.py runserver 0.0.0.0:8000
```

・管理画面へアクセス  
```
http://localhost:8000/admin/
```

---

### ここからはサンプル API を作成していきます
※追記と記載がない場合は、ファイルを上書きで大丈夫です

1.モデルを作成(apiv1/model.py)  
```python
import uuid

from django.db import models

class Task(models.Model):
    class Meta:
        db_table = 'task'
        verbose_name = verbose_name_plural = 'タスク'

    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    title = models.CharField(verbose_name='タイトル', max_length=20)
    created_at = models.DateTimeField(verbose_name='登録日時', auto_now_add=True)

    def __str__(self):
        return self.title
```

2.マイグレーションファイルを作成し、反映
```bash
% python manage.py makemigrations apiv1
% python manage.py migrate
```

3.シリアライザーを作成(`apiv1/serializers.py(ファイル追加)`)  
```python
from rest_framework import serializers

from .models import Task

class TaskSerializer(serializers.ModelSerializer):
    class Meta:
        model = Task
        fields = ['id', 'title']
        extra_kwargs = {
            'title': {
                'error_messages': {
                    'blank': 'タイトルは空にできません。',
                }
            }
        }
```

4.ビューを作成(apiv1/views.py)  
※実際には React で表示しますが、データ確認のため作成
```python
from rest_framework import viewsets

from .models import Task
from .serializers import TaskSerializer

class TaskViewSet(viewsets.ModelViewSet):
    queryset = Task.objects.all()
    serializer_class = TaskSerializer
```

5.apiv1 のルーターを作成(`apiv1/urls.py(ファイル追加)`)
```python
from django.urls import include, path
from rest_framework import routers

from .views import TaskViewSet

router = routers.DefaultRouter()
router.register('tasks', TaskViewSet)

app_name = 'apiv1'
urlpatterns = [
    path('', include(router.urls)),
]
```

6.ルートのルータに追記(config/urls.py)

```python
from django.urls import include, path

urlpatterns = [
    ...
    path('api/v1/', include('apiv1.urls')),
]
```

7.サーバー起動
```bash
% python manage.py runserver 0.0.0.0:8000
```

・ビューへアクセス
```
http://localhost:8000/api/v1/tasks/
```

・管理画面からテーブルを編集できるようにする  
下記を追記し管理画面へアクセスすると、テーブルが閲覧できるようになりこちらからもデータの追加などができます(apiv1/admin.py)
```python
from .models import Task

admin.site.register(Task) 
```

**※ 後ほど React でデータを表示するので、何か値を登録しておいてください** 

---

### Frontend(Vite) 構築

#### ・Backend の設定
・React との連携のため、cleanly_backend で必要なものを設定していきます  
cleanly_backend で作業してください

1.React との連携に必要なパッケージをインストール  
※バージョンは固定にした方がいいかもですが、今のところ指定なしでインストールされるのは下記バージョンになります。  
`django-cors-headers: 4.7.0`
```bash
% pip install django-cors-headers
```

2.設定ファイルを編集(config/settings.py)
```python
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware', # 追記
    '....CommonMiddleware',
    ...
]

...

# ファイルの末尾に下記を追加

# CORS
CORS_ALLOW_ALL_ORIGINS = False
CORS_ALLOWED_ORIGINS = [
  'http://localhost:3000',
]
```

---

#### ・frontend の設定

3.各フォルダとファイルを作成
```
- cleanly(フォルダ)
    - backend_project
        - ...
    - frontend_project(フォルダ)
        - frontend(フォルダ)
        - Dockerfile(ファイル)
    - compose.yml
```

・Dockerfile 
```Dockerfile
FROM node:22.13.1-slim
WORKDIR /usr/src/app
```

・compose.yml
```yml
services:
  ...
  frontend:
    build:
      context: ./frontend_project
      dockerfile: Dockerfile
    container_name: 'cleanly_frontend'
    tty: true
    ports:
      - 3000:3000
    volumes:
      - ./frontend_project/frontend:/usr/src/app
```

---

4.cleanly 内で下記コマンドで docker を起動します
```bash
% docker compose up -d
```

5.コンテナ内へ入ります
```bash
% docker exec -it cleanly_frontend /bin/bash
```

6.React(vite) に必要なパッケージをインストール
```bash
% npm create vite@latest
- create-vite@6.2.0
- Project name: cleanly_frontend
- Select a framework: React
- Select a variant: JavaScript + SWC

% cd cleanly_frontend/
% npm install
```

7.設定ファイル編集(cleanly_frontend/vite.config.js)
```js
export default defineConfig({
  ...
  // 下記追記
  server: {
    host: "0.0.0.0",
    port: 3000,
  },
});
```

8.サーバー起動
```bash
% npm run dev
```

・アクセス
```
http://localhost:3000
```

---

### ここからはサンプルビューを作成していきます

1.ビューを作成(cleanly_frontend/src/App.jsx)  
確認のためだけなので、元のコードを全てコメントアウトし、下記を追記
```js
import React, { useEffect, useState } from 'react';
import './App.css';

function App() {
  const [items, setItems] = useState([]);

  useEffect(() => {
    fetch('http://localhost:8000/api/v1/tasks/')
      .then(response => response.json())
      .then(data => setItems(data));
  }, []);

  return (
    <div className="App">
      <ul>
        {items.map(item => (
          <li key={item.id}>{item.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

**・drf / react をそれぞれ起動した状態で、react の方にアクセスすると登録したデータが閲覧できます**
**(次回起動から DockerFile に CMD を記載でいいと思います)**
