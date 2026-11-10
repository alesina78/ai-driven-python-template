# 🏎️ F1 Dashboard

> **Dashboard Interativo de Fórmula 1** desenvolvido com Streamlit, projetado para análise em tempo real das **5 voltas mais rápidas por circuito** consumindo dados da API OpenF1.
> Aplicação desenvolvida sob arquitetura em camadas, validação de dados rigorosa com Pydantic v2, suíte de testes unitários automatizados, linting estrito e documentação viva.

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

Este projeto foi concebido como um laboratório prático de Python com foco em **qualidade de código em produção**, evoluindo de um script de análise isolado para um produto estruturado com governança de dados e arquitetura resiliente.

**Perguntas que a aplicação responde:**
- Quais são as 5 voltas mais rápidas registradas em determinado circuito durante a sessão?
- Como se comparam os tempos entre diferentes pilotos em tempo real?
- Como garantir que payloads dinâmicos vindos da API OpenF1 passem por parsing limpo e sem estouro de exceção?

---

## 🏗️ Arquitetura da Aplicação

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

**Padrões Arquiteturais Aplicados:**
- **Separation of Concerns (SoC):** Camada de apresentação totalmente isolada da ingestão e regras de negócio.
- **Data Validation Layer:** Modelagem estrita via Pydantic v2 para assegurar que contratos da API OpenF1 sejam respeitados.
- **Fail-Fast & Logging:** Sistema de logs centralizado para rápida rastreabilidade e depuração de requisições HTTP.

---

## 📦 Módulos de Produção

### Camada de Software

| Módulo | Arquivo | Responsabilidade |
|---|---|---|
| **Entrypoint** | `src/f1/dashboard.py` | Interface gráfica em Streamlit, renderização de tabelas e filtros interativos |
| **Domínio** | `src/f1/domain/` | Modelos de dados Pydantic v2, schemas de validação e repositórios |
| **Ingestão** | `src/f1/ingestion/` | Cliente HTTP resiliente para consumo de endpoints em tempo real da OpenF1 API |
| **Processamento** | `src/f1/processing/` | Algoritmos de ordenação, filtragem e cálculo das 5 voltas mais rápidas por pista |
| **Utilitários** | `src/f1/utils/` | Sistema de logging estruturado e leitura centralizada de configurações |

---

## 🧪 Testes Automatizados & Qualidade

Suíte rigorosa de testes unitários desenvolvida com `pytest`, cobrindo validação de schemas, ingestão HTTP e lógica de processamento de voltas:

| Suíte de Testes | Cobertura Mínima | Objetivo |
|---|---|---|
| `tests/unit/` | $\ge$ 50% (37 testes) | Garantir estabilidade da camada de domínio, manipulação de voltas e mocks de API |

```bash
# Executar a suíte de testes unitários
poetry run pytest tests/unit/

# Verificar regras de linting e formatação de código
poetry run ruff check .
```

---

## 📚 Documentação Técnica

A documentação completa do projeto é mantida através do **MkDocs Material**, permitindo navegação estruturada pelas classes, módulos e arquitetura da aplicação.

```bash
# Servir a documentação localmente (http://127.0.0.1:8000)
poetry run task docs

# Compilar o build estático da documentação
poetry run task docs-build
```

---

## ▶️ Como Executar

```bash
# 1. Clonar o repositório
git clone https://github.com/alesina78/f1-dashboard.git
cd f1-dashboard

# 2. Instalar dependências de ambiente e desenvolvimento
poetry install --with dev

# 3. Iniciar a aplicação Streamlit
poetry run streamlit run src/f1/dashboard.py
```

---

## 📁 Estrutura do Repositório

```
f1-dashboard/
│
├── src/f1/                     # Módulos de produção
│   ├── domain/                 # Schemas e validação (Pydantic v2)
│   ├── ingestion/              # Clientes de API (OpenF1)
│   ├── processing/             # Logica de filtro e top voltas
│   ├── utils/                  # Logger e configs
│   └── dashboard.py            # Entrypoint do Streamlit
│
├── tests/
│   └── unit/                   # 37 testes unitários (pytest)
│
├── docs/                       # Arquivos-fonte do MkDocs Material
│
├── pyproject.toml              # Dependências Poetry e configs do Ruff
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
