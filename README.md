## Docker + Next.js 環境構築

```bash
mkdir <任意のフォルダ名>
cd <任意のフォルダ名>
```

## Dockerfile & docker-compose.yml を作成

- Linux,Mac をお使いはおなじみの touch で作りましょう。

- Windows で PowerShell をインストールしている場合

```powershell
new-item Dockerfile, docker-compose.yml
```

## Dockerfile

```dockerfile
FROM node:20.10.0-alpine

WORKDIR /app

RUN npm install -g npm@10.2.5
```

## docker-compose.yml

```yml
version: '3.9'

services:
  app:
    build:
      context: .
    tty: true
    environment:
      - WATCHPACK_POLLING=true
    volumes:
      - ./next-app:/app
    ports:
      - '3000:3000'
    command: sh -c "npm run dev"
```

- Tips: ホットリロードを有効にしておく記述

```yml
environment:
  - WATCHPACK_POLLING=true
```

## image を構築

```bash
docker-compose build
```

## Next.js のインストール

対話形式に沿ってインストール

```bash
docker-compose run --rm app sh -c 'npx create-next-app@latest next-app --ts --tailwind --eslint --app --src-dir --import-alias --use-npm'
```

※options を指定してインストールする場合は公式の[`APIリファレンス`](https://nextjs.org/docs/pages/api-reference/create-next-app)を参照し適宜自分の合ったものに変更してください。

## Next.js を動かしてみる

```bash
docker-compose up
```

これで localhost:3000 でアクセスできれば OK です。
