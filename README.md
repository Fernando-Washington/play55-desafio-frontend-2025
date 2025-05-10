# DevFinder - Desafio Técnico Estágio Front-end Web Play55

## Sobre o Desafio

Este projeto foi desenvolvido como parte do desafio técnico da Play55, com o objetivo de avaliar conhecimentos técnicos no desenvolvimento web, focando principalmente na utilização do framework Vue 3 e Nuxt 3, e na utilização de bibliotecas para criar uma boa experiência de usuário, com um projeto funcional e otimizado.

O desafio consistiu em criar um buscador de perfil do GitHub, com integração com o Recaptcha, seguindo o design proposto pelo [Frontend Mentor](https://www.frontendmentor.io/challenges/github-user-search-app-Q09YOgaH6).

![image](https://github.com/user-attachments/assets/49e609b2-ba88-43a4-83e8-b25c1e5f3c61)
![image](https://github.com/user-attachments/assets/a5affffb-f4b9-43ed-b253-dfd4861f341d)



## Funcionalidades

O DevFinder é uma aplicação moderna e elegante que permite buscar usuários do GitHub e visualizar seus perfis em uma interface belamente projetada. Construído com Vue.js e Nuxt, oferece uma experiência perfeita com temas claro e escuro.

### Requisitos Implementados:

- 🔍 **Busca de Usuários do GitHub** - Encontre qualquer usuário do GitHub pelo nome de usuário
- 👤 **Perfil Detalhado** - Visualize informações completas do usuário:
  - Foto de perfil
  - Nome
  - Nome do perfil ("@ do perfil") - clicável, abrindo o GitHub no perfil do usuário
  - Descrição do perfil
  - Quantidade de repositórios
  - Quantidade de seguidores
  - Quantidade de pessoas que o usuário está seguindo
  - Localização do perfil
  - Links sociais (clicáveis, redirecionando para os respectivos links)
- 🌓 **Modo Claro/Escuro** - Alterne entre temas claro e escuro com um único clique
- 📱 **Design Responsivo** - Funciona perfeitamente em dispositivos móveis, tablets e desktops
- 📝 **Formulário de Contato** - Entre em contato através do formulário integrado com validação reCAPTCHA
- 📜 **Histórico de Busca** - Acompanhe suas pesquisas anteriores, salvas no localStorage
- 🔒 **Integração com reCAPTCHA** - Envios de formulários seguros com Google reCAPTCHA v3

## 🛠 Tecnologias

- **Framework Frontend**: [Vue.js 3](https://vuejs.org/) com [Nuxt.js 3](https://nuxt.com/)
- **Estilização**: [Tailwind CSS](https://tailwindcss.com/) para estilização baseada em utilitários
- **Ícones**: [Lucide Vue](https://lucide.dev/) para ícones bonitos e consistentes
- **API**: API do GitHub para buscar dados de usuários
- **Segurança**: Google reCAPTCHA v3 para proteção de formulários
- **Armazenamento**: LocalStorage para salvar o histórico de pesquisas
- **Renderização**: Client-side Rendering (CSR)
- **Implantação**: Vercel

## Como Começar

### Pré-requisitos

- Node.js (v16 ou superior)
- npm ou yarn

### Instalação

1. Clone o repositório:

```bash
git clone https://github.com/seuusuario/devfinder.git
cd devfinder
