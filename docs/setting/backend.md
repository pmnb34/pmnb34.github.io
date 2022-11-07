---
layout: default
title: Backend
parent: Setting
nav_order: 1
---

# Prisma + Apollo + Grahpql 2022
{: .no_toc .fw-500 }


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Github
{: .no_toc }
[Github](https://github.com/pmnb34/Basic_Backend_Setting_2022)
```bash
$ git clone https://github.com/pmnb34/Basic_Backend_Setting_2022.git
```

## apollo, graphql 설치

apollo 는 graphql 을 쉽게 사용할수 있게 해주는 라이브러리 입니다.  
apollo 는 server 와 client 에서 사용 가능합니다.

> apollo-client 는 상태관리 라이브러리로 redux, zustand 동일하지만  
> apollo-client 는 Graphql 기반으로 데이터를 가져오며,  
> redux, zustand는 REST API 기반으로 데이터를 가져오기때문에  
> REST API 의 문제점인, overfetching, underfetching 등의 문제를 보완할 수 있다.

> [apollo 공식문서 (https://www.apollographql.com/docs/)](https://www.apollographql.com/docs/)  
> [graphql 공식문서 (https://graphql.org/learn/)](https://graphql.org/learn/)


```bash
$ npm i @apollo/server graphql
```

## babel, nodemon 설치 

babel 은 ES6 이상의 최신 문법의 자바스크립트를 ES5 이하의 이전 문법으로 컴파일해주는 라이브러리입니다.  
최신 문법을 지원하지 않는 환경에서도 최신 문법을 사용할수 있게 해준다.  
  
nodemon 은 서버의 코드가 변경될때마다 자동 재시작해주는 모듈입니다. 

```bash
$ npm i @babel/node
$ npm i @babel/preset-env
$ npm i @babel/core

$ npm i nodemon
```

```bash
# babel.config.json 
# 파일 생성 및 수정
{
  "presets": ["@babel/preset-env"]
}
```

```bash
# package.json
# scripts 부분 수정
{
  "dev": "nodemon --exec babel-node src/server --delay 2",
}
```

## prisma 설치 

prisma 는 SQL 문법을 사용하지 않고, js문법으로 DB를 관리할 수 있는 ORM(Object Relational Model) 입니다.

> 1. schema 를 작성하여 모델에 대해 정의합니다.  
> 2. migrate 를 실행하여 SQL 테이블을 생성합니다.

> [prisma 공식문서 (https://www.prisma.io/docs/)](https://www.prisma.io/docs/)

```bash
$ npm i prisma --save-dev
$ npx prisma init
```

```bash
# schema.prisma
# schema.prisma 파일 생성 및 수정
# schema 를 작성하여 모델에 대해 정의
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int     @id @default(autoincrement())
  firstName String
  lastName  String?
  userName  String  @unique
  email     String  @unique
  password  String
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  published Boolean @default(true)
  authorId  Int
  author    User    @relation(fields: [authorId], references: [id])
}
```
```bash
# migrate 를 실행하여 SQL 테이블을 생성
$ npx prisma migrate dev
```

### Client.js 파일 생성

prisma client 는 DB에 대한 CRUD를 수행한다. 

```js
// client.js
// prisma client 를 생성한다.
// 필요한 곳에서 client를 import 하여 사용한다. 
// import client from "../client";
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();
export default client;
```

### prisma studio 실행

prisma studio 는 웹에서 GUI형태의 DB를 확인할 수 있다.

```bash
$ npx prisma studio
```

## graphql-tools 설치

graphql-tools 는 분리된 graphql을 합쳐주는 역할을 한다.

```bash
$ npm i @graphql-tools/schema 
$ npm i @graphql-tools/load-files 
$ npm i @graphql-tools/merge
```

```js
// schema.js
// 분리되어있는 typeDefs.js , queries.js, mutations.js 를 찾아서 합쳐준다.
// schema 로 import 하여 사용한다. 
// import schema from "./schema";

import { makeExecutableSchema } from "@graphql-tools/schema";
import { loadFilesSync } from "@graphql-tools/load-files";
import { mergeResolvers, mergeTypeDefs } from "@graphql-tools/merge";

const loadedTypes = loadFilesSync(`${__dirname}/**/*.typeDefs.js`);
const loadedResolvers = loadFilesSync(
  `${__dirname}/**/*.{queries,mutations}.js`
);
const typeDefs = mergeTypeDefs(loadedTypes);
const resolvers = mergeResolvers(loadedResolvers);

const schema = makeExecutableSchema({ typeDefs, resolvers });

export default schema;
```

## server.js 설정
```js
// server.js
require("dotenv").config();
import http from "http";
import express from "express";
import logger from "morgan";
import { ApolloServer } from "apollo-server-express";
import schema from "./schema";

const startApolloServer = async (schema) => {
  const PORT = process.env.PORT;
  const apollo = new ApolloServer({
    schema,
  });
  await apollo.start();
  const app = express();
  app.use(logger("dev"));
  apollo.applyMiddleware({ app });

  const httpServer = http.createServer(app);

  httpServer.listen(PORT, () => {
    console.log(`🚀Server is running on http://localhost:${PORT} ✅`);
  });
}

startApolloServer(schema);
```

```bash
$ npm run dev
```