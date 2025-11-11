## Projeto Tópicos Vue

![Top Language](https://img.shields.io/github/languages/top/mluizaguedes/topicos-vue?style=for-the-badge)

Este é um projeto frontend desenvolvido com Vue 3 e Vite, que simula um sistema de gerenciamento de usuários, anotações e comentários, interagindo com uma API de backend.

### Tecnologias Utilizadas

*   **Vue 3**
*   **Vite**
*   **TypeScript**
*   **Pinia** (Gerenciamento de Estado)
*   **Vue Router** (Roteamento)
*   **Axios** (Requisições HTTP)
*   **ESLint** (Linting)
*   **Prettier** (Formatação de Código)

### Pré-requisitos

Certifique-se de ter as seguintes ferramentas instaladas em sua máquina:

*   **Node.js**: Versão 20.19.0 ou superior (conforme `package.json`).
*   **npm**: Gerenciador de pacotes do Node.js.

### Como Instalar e Configurar

Siga os passos abaixo para configurar e rodar o projeto localmente:

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/mluizaguedes/topicos-vue
    ```

2.  **Navegue até o diretório do projeto:**
    ```bash
    cd topicos-vue
    ```

3.  **Instale as dependências:**
    ```bash
    npm install
    ```

4.  **Configuração da API:**
    Este frontend espera uma [API de backend](https://github.com/mluizaguedes/topicos-springboot) rodando em `http://localhost:8080/`. Certifique-se de que a API esteja disponível neste endereço, ou ajuste a `baseURL` em `src/main.ts` se necessário.

### Como Executar

Para iniciar a aplicação em modo de desenvolvimento:

```bash
npm run dev
```

Para compilar e minificar para produção:

```bash
npm run build
```
