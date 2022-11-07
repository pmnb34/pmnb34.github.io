---
layout: default
title: Frontend
parent: Setting
nav_order: 2
---

# React + Apollo/Client + Graphql 2022
{: .no_toc .fw-500 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Github
{: .no_toc }
[Github](https://github.com/pmnb34/Basic_Frontend_Setting_2022)
```bash
$ git clone https://github.com/pmnb34/Basic_Frontend_Setting_2022.git
```


## react 설치

react 를 처음부터 세팅하려면 많은 세팅이 필요합니다.  
create-react-app은 이부분을 해결하고  
간편하게 react를 바로 시작할 수 있습니다. 

```bash
$ npx create-react-app react_apollo_graphql
```
```bash
$ npm start
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
$ npm i @apollo/client graphql
```
```js
// apollo.js
// apollo 서버와 연결
import { ApolloClient, InMemoryCache } from "@apollo/client";
export const client = new ApolloClient({
  uri: 'http://localhost:4000',
  cache: new InMemoryCache(),
});
```
## react 실행
### app.js 설정
```js
// app.js
// ApolloProvider 세팅 
import { ApolloProvider } from '@apollo/client';
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import { client } from './apollo';
import { routes } from './routes';
import { Layout } from './components/Layout';
import { Home } from './pages/Home';

function App() {
  return (
    <ApolloProvider client={client}>
      <Router>
        <Switch>
          <Route path={routes.home} exact>
              <Layout>
                  <Home />
              </Layout>
          </Route>
          <Route path={routes.signUp} exact></Route>
        </Switch>
      </Router>
    </ApolloProvider>
  );
}

export default App;
```

### routes.js 설정
```js
// routes.js
// route 세팅 
export const routes = {
  home: "/",
  signUp: "/sign-up",
};
```

### styled-components 설정
```js
import styled from "styled-components";
import Header from "./Header";

const Content = styled.main`
  margin: 0 auto;
  margin-top: 45px;
  max-width: 930px;
  width: 100%;
`;

function Layout({ children }) {
  return (
    <>
      <Header />
      <Content>{children}</Content>
    </>
  );
}

export default Layout;

```