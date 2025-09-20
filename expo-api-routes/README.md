
# 🚀 API Routes 


Neste projeto realizamos uma API simples utilizando **Next.js**, implementamos uma tela de autenticação (SignIn) que utiliza essa API e, por fim, fizemos o deploy do backend na **Vercel**.

---

## ✅ Pré-requisitos

- Node.js (versão LTS recomendada)  
- npm (instalado junto com o Node.js)  
- Editor de código (VSCode sugerido)  
- Cliente HTTP 
- [Documentação oficial do Next.js]

---

## ⚡ Criando o Projeto Next.js

1. Criar o projeto:
   ```bash
   npx create-next-app@latest meu-app


Entre na pasta criada:

```
cd meu-app

```

Instalar dependência para requisições:

```
npm install axios

```

Para rodar o servidor de desenvolvimento:

```
npx expo start

```

O app estará rodando em:

```
http://localhost:NumPorta

```

## Configurando API Routes 



No **Next.js**, todas as rotas de API ficam em `pages/api`.

**Exemplo: rota de login**

Crie o arquivo `pages/api/login.js`:

```
 export default function handler(req, res) {
  if (req.method === "POST") {
    const { email, password } = req.body;

    if (email === "admin@teste.com" && password === "123456") {
      return res.status(200).json({ message: "Login realizado com sucesso!" });
    }

    return res.status(401).json({ error: "Credenciais inválidas" });
  }

  return res.status(405).json({ error: "Método não permitido" });
}


O que o código faz:

-   Aceita somente requisições **POST**
    
-   Verifica email e senha
    
-   Retorna **200** com mensagem de sucesso se os dados forem válidos
    
-   Retorna **401** em caso de erro de credenciais
    
-   Para outros métodos → **405**

Testando o endpoint

-   URL: `POST http://localhost:3000/api/login`
-   Body (JSON):
{
  "email": "admin@teste.com",
  "password": "123456"
}
Resposta esperada:
-   Sucesso: `{ "message": "Login realizado com sucesso!" }`
    
-   Erro: `{ "error": "Credenciais inválidas" }`


```


````
## Criando a tela de SignIn



Arquivo: `pages/signin.js`: 

import { useState } from "react";
import axios from "axios";

export default function SignIn() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [msg, setMsg] = useState("");

  async function handleLogin(e) {
    e.preventDefault();
    try {
      const res = await axios.post("/api/login", { email, password });
      setMsg(res.data.message);
    } catch (err) {
      setMsg(err.response.data.error);
    }
  }

  return (
    <div style={{ padding: 20 }}>
      <h1>Login</h1>
      <form onSubmit={handleLogin}>
        <input
          type="email"
          placeholder="Digite seu email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        /><br/>
        <input
          type="password"
          placeholder="Digite sua senha"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        /><br/>
        <button type="submit">Entrar</button>
      </form>
      {msg && <p>{msg}</p>}
    </div>
  );
}

```

Funcionamento:
-   Armazena email, senha e mensagem no estado com `useState`
    
-   Envia dados para `/api/login` usando **axios**
    
-   Exibe a resposta vinda do backend
    

Acesse em:  
👉 `http://localhost:NumPorta/signin`


**Acessando Endpoints no App** Com o servidor rodando em npm run dev, abra em: [http://localhost:numPorta/signin]



Deploy na Vercel

Para colocar o backend no ar:

1.  Crie uma conta gratuita na Vercel
    
2.  Instalar uma CLI Vercel:
    
    npm install -g vercel
    
3.  Na pasta do projeto, rodar:
    
    vercel
    

Seguir as instruções:

1. Fazer login.
2.  Associar repositório.
3.  Confirmar configurações.
4.  Aguardar a publicação.

````
## Tecnologias utilizadas
````-   **Next.js** → Framework React com suporte nativo a API Routes
    
-   **Axios** → Cliente HTTP para consumo da API
    
-   **React Hooks (useState)** → Controle de estados (email, senha e mensagens)
    
-   **Vercel** → Plataforma de deploy serverless

````
## Problemas e Soluções:
````
1. Pergunta da Vercel: 
? Want to modify these settings? (y/N)

  O que significa: A Vercel pergunta se você deseja modificar manualmente os comandos de build, pasta de saída, etc.
    
 Solução: Digite "N" e pressione  Enter para usar as configurações padrão.
 
 2. Alguns comandos estavam desatualizados:

 Antes ->  o  `expo-router/server` exportava os tipos `ExpoResponse` e `ExpoRequest`.
 
 Agora -> nas versões recentes (`expo-router@3` + `expo@53`), eles  foram removidos.

3 . Correção do meu código Versão Atual Expo Router:

export async function POST(request: Request): Promise<Response> {
  const { email, password } = await request.json();

  return new Response(
    JSON.stringify({
      email,
      password,
    }),
    {
      headers: { "Content-Type": "application/json" },
    }
  );
}
4. Diferenças:
- Use Request  e Response (nativos da Web API, já globais, não precisa importar).
- `request.json()` funciona igual no fetch.
- `new Response()` retorna o JSON.
