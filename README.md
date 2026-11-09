# Reserva de Salas — Front-end

Sistema web para reservar **salas de aula** e **laboratórios**. Este repositório é o front-end (Next.js); a API e o banco de dados ficam no repositório [reserva_salas](https://github.com/GeovannaDemesio/reserva_salas).

## O que o sistema faz

| Tela | Rota | Descrição |
| --- | --- | --- |
| Acesso ao sistema | `/login` | Entrada com CPF e senha |
| Cadastro | `/cadastro` | Criação de conta (nome, CPF, nascimento, e-mail, celular, senha) |
| Tela principal | `/` | Menu com atalhos e as reservas de hoje |
| Reservar | `/reservar` | Escolha de recurso, data e horário, com a situação do recurso no dia |
| Consultar reservas | `/reservas` | Filtros por data, usuário, tipo e recurso |
| Salas | `/salas` | Lista de salas (cadastro, edição e exclusão só para o administrador) |
| Laboratórios | `/laboratorios` | Lista de laboratórios (mesmas regras das salas) |

### Status das reservas

- **Livre**: o recurso não está reservado no horário.
- **Reservado**: reservado, mas ainda não está em uso.
- **Ocupado**: o horário reservado está acontecendo agora.
- **Bloqueado**: o recurso está em manutenção e não pode ser reservado.

### Regras de acesso

- Só o CPF configurado em `ADMIN_CPF` cria, edita e exclui salas e laboratórios, e só ele bloqueia um recurso.
- Cada usuário cancela apenas as próprias reservas; o administrador cancela qualquer uma.
- Não é possível reservar no passado nem em horário que conflite com outra reserva ou bloqueio.
- Essas regras são conferidas no servidor (rotas `/api`), e não só na tela. O navegador nunca fala direto com a API do back.

## Tecnologias

- [Next.js](https://nextjs.org) 16 (App Router, Route Handlers) e React 19
- Tailwind CSS 4
- JavaScript
- Sessão por cookie assinado (HMAC), sem dependências extras

## Como rodar

Pré-requisitos: Node.js 20 ou superior e o back-end rodando (veja o README do [reserva_salas](https://github.com/GeovannaDemesio/reserva_salas)).

```bash
npm install
cp .env.example .env.local
npm run dev
```

Abra [http://localhost:3000](http://localhost:3000).

### Variáveis de ambiente (`.env.local`)

| Variável | Descrição |
| --- | --- |
| `BACKEND_URL` | Endereço da API, por exemplo `http://localhost:3001` |
| `SESSION_SECRET` | Texto aleatório com 32 ou mais caracteres, usado para assinar a sessão |
| `ADMIN_CPF` | CPF (só números) do administrador |

Para gerar um `SESSION_SECRET`:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### Outros comandos

```bash
npm run build   # build de produção
npm run start   # roda o build de produção
npm run lint    # verifica o código
```

## Estrutura

```
app/
  api/            rotas do servidor (auth, salas, laboratorios, reservas)
  login/ cadastro/ reservar/ reservas/ salas/ laboratorios/
  page.jsx        tela principal
components/       telas e peças de interface
lib/              sessão, acesso ao back-end, regras de salas e reservas
```

## Observações

- A API do back guarda as senhas em texto puro. Antes de usar em produção, vale trocar por hash (por exemplo `bcrypt`).
- O CPF é aceito com qualquer sequência de 11 números, sem conferir os dígitos verificadores.
