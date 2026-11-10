# 🏎️ F1 Dashboard

> **Dashboard Interativo de Fórmula 1** desenvolvido com Streamlit, projetado para análise em tempo real das **5 voltas mais rápidas por circuito** consumindo dados da API OpenF1.
> Aplicação desenvolvida sob arquitetura em camadas, validação de dados rigorosa com Pydantic v2, suíte de testes unitários automatizados, linting estrito e governança completa de engenharia de software.

[![Python](https://img.shields.io/badge/Python-3.14-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)](https://streamlit.io/)
[![Pydantic](https://img.shields.io/badge/Pydantic-v2-E92063?style=flat-square&logo=pydantic&logoColor=white)](https://docs.pydantic.dev/)
[![Pytest](https://img.shields.io/badge/Testes-pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white)](https://pytest.org/)
[![Ruff](https://img.shields.io/badge/Ruff-Linting-261230?style=flat-square&logo=ruff&logoColor=white)](https://github.com/astral-sh/ruff)
[![Poetry](https://img.shields.io/badge/Poetry-Gerenciamento-60A5FA?style=flat-square&logo=poetry&logoColor=white)](https://python-poetry.org/)
[![Status](https://img.shields.io/badge/Status-Ativo-2ea44f?style=flat-square)]()

---

## 🎯 O Problema de Negócio & Objetivo

A análise de telemetria e tempos de volta na Fórmula 1 frequentemente exige a manipulação de dados de alta frequência expostos por APIs públicas. O objetivo deste projeto é fornecer um **dashboard operacional ágil e confiável** para visualização do desempenho em pista, enquanto demonstra padrões de engenharia de software de nível corporativo.

Este projeto foi concebido como um laboratório prático com foco em **qualidade de código em produção**, evoluindo de um script de análise isolado para um produto estruturado com governança de dados, tipagem estática e arquitetura resiliente.

**Perguntas que a aplicação responde:**
- Quais são as 5 voltas mais rápidas registradas em determinado circuito durante a sessão?
- Como se comparam os tempos entre diferentes pilotos em tempo real?
- Como garantir que payloads dinâmicos vindos da API OpenF1 passem por parsing limpo e sem estouro de exceção?

---

## 🏗️ Arquitetura & Modularização

O projeto adota **separação por camadas** com responsabilidades bem definidas. Cada camada é coesa e possui acoplamento mínimo com as demais.

```
┌─────────────────────────────────────────────────────────────┐
│                       F1 Dashboard                          │
├──────────────┬──────────────┬──────────────┬────────────────┤
│ Ingestão     │ Domínio      │ Processamento│ Apresentação   │
│ Client OpenF1│ Pydantic v2  │ Top 5 Voltas │ Streamlit Web  │
├──────────────┴──────────────┴──────────────┴────────────────┤
│            Governança, Logging & Testes (Pytest)            │
└─────────────────────────────────────────────────────────────┘
```

### Regras da Arquitetura
- **`ingestion/`**: Conexão HTTP resiliente com a API OpenF1 e parse bruto. Não contém regras de negócio.
- **`domain/`**: Schemas Pydantic v2, entidades e validações. Não possui dependências de infraestrutura.
- **`processing/`**: Regras de transformação, ordenação e cálculo das top voltas.
- **`utils/`**: Utilitários transversais (logging estruturado, configurações via `.env`).
- **`dashboard.py`**: Entrypoint da interface gráfica em Streamlit.

---

## 📦 Módulos de Produção

| Camada | Arquivo / Diretório | Responsabilidade |
|---|---|---|
| **Entrypoint** | `src/f1/dashboard.py` | Interface gráfica em Streamlit, renderização de tabelas e filtros |
| **Domínio** | `src/f1/domain/` | Modelos Pydantic v2, schemas de validação e regras de negócio |
| **Ingestão** | `src/f1/ingestion/` | Cliente HTTP resiliente para consumo de endpoints em tempo real da OpenF1 API |
| **Processamento** | `src/f1/processing/` | Algoritmos de ordenação, filtragem e cálculo das 5 voltas mais rápidas por pista |
| **Utilitários** | `src/f1/utils/` | Sistema de logging estruturado e leitura de configurações de ambiente |

---

## 🛠️ Padrões de Engenharia & Qualidade

### 1. Gerenciamento de Ambiente com Poetry
O uso do **Poetry** é obrigatório para gerenciamento de dependências e ambientes reproduzíveis (`poetry.lock`). O uso de `pip install` direto é proibido.

```bash
# Instalar dependências completas (com grupos de dev e docs)
poetry install --with dev,docs

# Adicionar nova dependência
poetry add pydantic
poetry add --group dev pytest ruff
```

### 2. Qualidade de Código & Linting (Ruff)
Seguimos o padrão **PEP 8** com validação automatizada via **Ruff**. O código deve passar 100% no lint antes de submissões.

```toml
# pyproject.toml
[tool.ruff]
line-length = 88
target-version = "py314"
src = ["src", "tests"]

[tool.ruff.lint]
select = ["E", "W", "F", "I", "B", "C4", "UP", "N"]
ignore = ["E501"]
```

```bash
# Verificar linting e formatação
poetry run ruff check .
poetry run ruff format . --check
```

### 3. Testes Automatizados & Cobertura (Pytest)
Cobertura mínima obrigatória de **50%** em testes unitários isolados (`tests/unit/`).

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "--cov=src/f1 --cov-report=term-missing --cov-fail-under=50"
```

```bash
# Executar suíte de testes
poetry run pytest

# Executar com relatório HTML de cobertura
poetry run pytest --cov=src/f1 --cov-report=html
```

### 4. Logging Estruturado & Variáveis de Ambiente
- **Sem `print()`:** Uso do módulo `logging` estruturado centralizado em `src/f1/utils/logger.py`.
- **Zero Secrets:** Credenciais e URLs via `.env` (nunca commitados; exemplo disponível em `.env.example`).
- **Imports Absolutos:** Proibido uso de imports relativos (`from f1.domain import ...`).
- **Type Hints:** Obrigatório o uso de tipagem em todas as assinaturas de funções e métodos.

---

## 📚 Documentação Técnica (MkDocs Material)

A documentação do projeto é versionada no próprio repositório utilizando **MkDocs Material**.

```bash
# Servir a documentação localmente em http://127.0.0.1:8000
poetry run task docs

# Gerar build estático da documentação
poetry run task docs-build
```

---

## ▶️ Como Executar

```bash
# 1. Clonar o repositório
git clone https://github.com/alesina78/f1-dashboard.git
cd f1-dashboard

# 2. Configurar o ambiente com Poetry
poetry install --with dev

# 3. Copiar variáveis de ambiente de exemplo
cp .env.example .env

# 4. Executar o Dashboard Streamlit
poetry run streamlit run src/f1/dashboard.py
```

---

## 🤝 Padrões de Contribuição & PRs

Antes de abrir um Pull Request, certifique-se de que a sua contribuição cumpre o checklist:

- [ ] Código passa 100% no lint: `poetry run ruff check .`
- [ ] Todos os testes unitários passaram: `poetry run pytest`
- [ ] Cobertura de testes mantida acima do limite ($\ge 50\%$)
- [ ] Type hints inclusos em novas funções
- [ ] Nenhuma credencial ou secret incluída
- [ ] Documentação atualizada (se alterou regras de negócio ou contratos)

---

## 📁 Estrutura do Repositório

```
f1-dashboard/
├── .env.example                # Exemplo de variáveis de ambiente
├── pyproject.toml              # Configuração do Poetry, Ruff e Pytest
├── poetry.lock                 # Lockfile de dependências
├── src/
│   └── f1/                     # Módulos da aplicação
│       ├── domain/             # Schemas Pydantic v2 e validações
│       ├── ingestion/          # Cliente HTTP para API OpenF1
│       ├── processing/         # Lógica de cálculo e filtragem das voltas
│       ├── utils/              # Logger estruturado e configs
│       └── dashboard.py        # Entrypoint da interface Streamlit
├── tests/
│   ├── unit/                   # Testes unitários com Pytest (37 testes)
│   └── integration/            # Testes de integração HTTP
├── docs/                       # Fonte da documentação MkDocs Material
└── README.md
```

---

## 👤 Autor

**Alessandro Lesina Giordano**
Senior Data Engineer & Analytics Specialist | 25+ anos em TI Corporativa

[![LinkedIn](https://img.shields.io/badge/LinkedIn-alessandrolesina-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/alessandrolesina/)
[![GitHub](https://img.shields.io/badge/GitHub-alesina78-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/alesina78)
[![Email](https://img.shields.io/badge/Email-giordano.dba%40gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:giordano.dba@gmail.com)

---

*Dados em tempo real consumidos a partir da API pública [OpenF1](https://openf1.org/).*
