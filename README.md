# 📋 Lista de Tarefas - Aplicação Conteinerizada

> Trabalho Prático - Ciclo 02  
> **Disciplina:** Sistemas Distribuídos (2025.2)  
> **Professor:** Álvaro Lopes Bastos  
> **Instituição:** UniEvangélica - Engenharia de Software

## 👥 Integrantes

- **Nome:** Kauã Gabriell Nazário de Azevedo  
- **RA:** 2411070
- **Nome:** Pedro Gabriel Dezani Dantas
- **RA:** 2411622


---

## 📖 Sobre o Projeto

Este projeto implementa uma aplicação de **Lista de Tarefas** (To-Do List) utilizando uma arquitetura **cliente-servidor distribuída** com containerização via **Docker**.

A aplicação é composta por:
- **Frontend:** Interface web desenvolvida em HTML, CSS e JavaScript
- **Backend:** API RESTful construída com Node.js e Express
- **Banco de Dados:** PostgreSQL para persistência de dados *(implementação extra)*

---

## 🏗️ Arquitetura

```
┌─────────────────┐      HTTP      ┌─────────────────┐
│                 │ ───────────────>│                 │
│    Frontend     │                 │     Backend     │
│  (Nginx:80)     │ <───────────────│  (Node.js:3000) │
│                 │                 │                 │
└─────────────────┘                 └────────┬────────┘
                                             │
                                             │ SQL
                                             ▼
                                    ┌─────────────────┐
                                    │   PostgreSQL    │
                                    │    (:5432)      │
                                    └─────────────────┘
```

### Containers

| Container | Imagem Base | Porta | Exposição | Descrição |
|-----------|-------------|-------|-----------|-----------|
| `frontend` | nginx:alpine | 80 | Externa | Serve os arquivos estáticos da interface |
| `backend` | node:18-alpine | 3000 | Externa | API RESTful para gerenciamento de tarefas |
| `postgres` | postgres:15-alpine | 5432 | **Interna** | Banco de dados relacional |

> **🔒 Nota de Segurança:** A porta 5432 do PostgreSQL **não é exposta externamente**. O banco de dados é acessível apenas internamente pela rede Docker, garantindo que apenas os containers `backend` possam se comunicar com ele.

---

## 🚀 Como Executar

### Pré-requisitos

Certifique-se de ter instalado em sua máquina:
- [Docker](https://www.docker.com/get-started) (versão 20.10 ou superior)
- [Docker Compose](https://docs.docker.com/compose/install/) (versão 2.0 ou superior)

### Passo a Passo

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/KauaGabriell/Lista-Tarefas-Docker.git
   cd Lista-Tarefas-Docker
   ```

2. **Inicie a aplicação:**
   ```bash
   docker compose up
   ```
   
   Ou, para executar em segundo plano:
   ```bash
   docker compose up -d
   ```

3. **Acesse a aplicação:**
   
   Abra seu navegador e acesse: **http://localhost**

4. **Para encerrar a aplicação:**
   ```bash
   docker compose down
   ```

   Para remover também os volumes (dados do banco):
   ```bash
   docker compose down -v
   ```

---

## 🔧 Endpoints da API

A API Backend expõe os seguintes endpoints:

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/tarefas` | Lista todas as tarefas |
| `POST` | `/tarefas` | Cria uma nova tarefa |

### Exemplo de Requisição

**POST /tarefas**
```json
{
  "descricao": "Estudar Docker e Kubernetes"
}
```

**Resposta:**
```json
{
  "id": 1,
  "descricao": "Estudar Docker e Kubernetes",
  "concluida": false
}
```

---

## 📁 Estrutura do Projeto

```
Lista-Tarefas-Docker/
├── backend/
│   ├── src/
│   │   └── index.js
│   ├── package.json
│   └── Dockerfile
├── frontend/
│   ├── index.html
│   ├── styles.css
│   ├── script.js
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## 🐳 Detalhes dos Dockerfiles

### Backend Dockerfile
- **Imagem base:** `node:18-alpine`
- **Porta exposta:** 3000
- **Otimizações:** Multi-stage build, cache de dependências

### Frontend Dockerfile
- **Imagem base:** `nginx:alpine`
- **Porta exposta:** 80
- **Servidor web:** Nginx para servir arquivos estáticos

### Banco de Dados
- **Imagem:** `postgres:15-alpine`
- **Porta exposta:** 5432
- **Persistência:** Volume Docker para dados

---

## 🌐 Rede Docker

Os containers se comunicam através de uma **rede bridge** customizada chamada `tarefas-network`, permitindo:
- Isolamento dos containers
- Comunicação via nomes de serviço (DNS interno)
- Segurança através de políticas de rede

### 🔐 Segurança

**Portas Expostas:**
- ✅ **Frontend (80):** Exposta para acesso público
- ✅ **Backend (3000):** Exposta para o frontend consumir a API
- 🔒 **PostgreSQL (5432):** **NÃO exposta externamente** - apenas acessível dentro da rede Docker

**Boas Práticas Implementadas:**
- Banco de dados isolado na rede interna
- Comunicação entre containers via DNS interno
- Apenas serviços necessários expostos ao host

> **⚠️ Observação:** Este projeto é configurado para ambiente de **desenvolvimento/acadêmico**. Em um ambiente de **produção**, seria necessário:
> - Utilizar secrets/variáveis de ambiente seguras
> - Implementar HTTPS com certificados SSL
> - Configurar firewall e políticas de rede mais restritivas
> - Usar autenticação e autorização robustas
> - Não expor a porta do backend diretamente (usar reverse proxy)

---

## ✅ Funcionalidades

- ✔️ Adicionar novas tarefas
- ✔️ Listar todas as tarefas
- ✔️ Marcar tarefas como concluídas
- ✔️ Persistência de dados no PostgreSQL
- ✔️ Interface responsiva e intuitiva

---

## 🛠️ Tecnologias Utilizadas

- **Docker & Docker Compose** - Conteinerização e orquestração
- **Node.js & Express** - Backend e API REST
- **PostgreSQL** - Banco de dados relacional
- **Nginx** - Servidor web para frontend
- **HTML/CSS/JavaScript** - Interface do usuário

---

## 📝 Notas Adicionais

- O banco de dados é inicializado automaticamente na primeira execução
- Os dados são persistidos em volumes Docker
- A aplicação está configurada para ambiente de desenvolvimento
- Todas as dependências são instaladas automaticamente durante o build
- **Segurança:** O PostgreSQL não é exposto externamente, apenas containers da mesma rede Docker podem acessá-lo

---

## 🤝 Contribuições

Este projeto foi desenvolvido como atividade acadêmica para a disciplina de Sistemas Distribuídos.

---

## 📄 Licença

Este projeto é de uso acadêmico e educacional.

---

**Desenvolvido com 💙 por Kauã Gabriell e Pedro Gabriel**