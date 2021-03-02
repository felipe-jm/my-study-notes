# NextJS

<!-- TOC -->

- [NextJS](#nextjs)
  - [Definição](#definição)
  - [O que tem/faz?](#o-que-tem/faz?)
  - [Casos de uso](#casos-de-uso)
  - [Static Site Generation](#static-site-generation)
  - [Client Side Rendering](#client-side-rendering)
  - [Server Side Rendering](#server-side-rendering)

<!-- /TOC -->

> Artigo recomendado e usado de inspiração: [NextJS, Gatsby ou Create React App? Entendendo os conceitos de SSR, SSG e SPA - Willian Justen](https://willianjusten.com.br/nextjs-gatsby-ou-create-react-app-entendendo-os-conceitos-de-ssr-ssg-e-spa/)

# Definição

NextJS é um Framework Web desenvolvido em ReactJS lançado em 2016 por Guillermo Rauch (Fundador da Vercel).

> Um Framework Web é um sistema opinativo com estrutura e ferramentas já definidas. (Sistema de rotas, geração de estático, geração de build para produção). Sua preocupação fica mais em cima do produto do que das ferramentas usadas para construí-lo.

# O que tem/faz?

- [x] Renderização no servidor (Server Side Rendering - SSR)
- [x] Geração de estáticos (Static Site Generation - SSG)
- [x] CSS-in-JS (Styled-jsx, Styled Components, Emotion, etc)
- [x] Zero Configuration (rotas, hot reloading, code splitting...)
- [x] Completamente extensível (controle completo do Babel/Webpacl, plugins..)
- [x] Otimizado para produção

# Casos de uso

Netflix, Github, Twitch, Uber...

https://nextjs.org/showcase

# Static Site Generation

Site gerado apenas em **Build Time**

https://youtu.be/1zhT23VDVDc 🎥

Vantagens:
  - Uso quase nulo do servidor
  - Pode ser servido numa CDN (melhor cache)
  - Melhor performance entre todos
  - Flexibilidade para usar qualquer servidor
  - Ótimo SEO

Desvantagens:
  - Tempo de build pode ser muito alto
  - Dificuldade para escalar em aplicações grandes
  - Dificuldade para realizar atualizações constantes

# Client Side Rendering

Renderizado pelo browser do cliente

https://youtu.be/4-Lel1oaV7M 🎥

Vantagens:
  - Permite páginas ricas em interações sem recarregar
  - Site rápido após o load inicial
  - Ótimo para aplicações web
  - Possui diversas bibliotecas

Desvantagens:
  - Load inicial pode ser muito grande
  - Performance imprevisível
  - Dificuldades no SEO
  - A maioria do conteúdo não é indexado

# Server Side Rendering

Renderizado pelo server e entregue ao cliente como html, css e js

https://youtu.be/0bvo6UKkNDA 🎥

Vantagens:
  - Ótimo em SEO
  - Meta tags com previews mais adequados
  - Melhor performance para o usuário (o conteúdo vai ser visto mais rápido)
  - Código compartilhado com o backend em Node
  - Menor processamento no lado do usuário

Desvantagens:
  - TTFB (Time to first byte) maior, o servidor vai preparar todo o conteúdo para entregar
  - HTML maior
  - Reload completo nas mudanças de rota
