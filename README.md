# CC8550 — CI/CD Lab

![CI](https://github.com/humbertopellegrini/teste_de_software/actions/workflows/ci.yml/badge.svg)

Repositório de apoio para a atividade prática de CI/CD da disciplina **CC8550 — Simulação e Teste de Software** (Centro Universitário FEI).

---

## Objetivo

Configurar um pipeline de Integração Contínua com **GitHub Actions** para este repositório, fazendo-o passar por todas as etapas:
**lint → testes → cobertura mínima de 80%**.

---

## Estrutura do Repositório

    teste_de_software/
    ├── calculadora.py          # módulo com 6 funções matemáticas
    ├── test_calculadora.py     # testes — INCOMPLETOS (cobertura ~65%)
    ├── requirements.txt        # dependências do projeto
    └── README.md               # este arquivo

---

## Como Começar

### 1. Faça um fork

Acesse [github.com/Rossi-Luciano/teste_de_software](https://github.com/Rossi-Luciano/teste_de_software)
e clique em **Fork** no canto superior direito.
O fork cria uma cópia do repositório na sua conta pessoal do GitHub
— e lá que o pipeline vai rodar.

### 2. Clone o fork localmente

```bash
git clone https://github.com/SEU-USUARIO/teste_de_software.git
cd teste_de_software
```

### 3. Crie uma branch para a atividade

```bash
git checkout -b pipeline
```

### 4. Instale as dependências

```bash
pip install -r requirements.txt
```

### 5. Verifique o estado atual

```bash
# rode o lint
flake8 . --max-line-length=100

# rode os testes com relatório de cobertura
pytest --cov=. --cov-report=term-missing
```

Você verá que a cobertura atual é de aproximadamente **65%** —
abaixo do mínimo exigido de 80%.

---

## Etapas da Atividade

### Etapa 1 — Criar o pipeline

Crie o arquivo `.github/workflows/ci.yml` com as seguintes etapas:

1. Trigger: `push` e `pull_request` para `main`
2. Setup Python 3.11
3. Instalação via `requirements.txt`
4. Lint com `flake8`
5. Testes com cobertura mínima de 80% (`--cov-fail-under=80`)
6. Publicação do relatório como artefato

Faça o commit e o push:

```bash
git add .github/workflows/ci.yml
git commit -m "ci: adiciona pipeline inicial"
git push origin pipeline
```

Abra a aba **Actions** no seu repositório e observe o pipeline rodar.

### Etapa 2 — Observar a falha

O pipeline deverá **falhar** na etapa de cobertura.
Leia o log do step com atenção:

- Quais funções não estão cobertas?
- Quais linhas específicas não foram executadas?

Use o comando local para inspecionar:

```bash
pytest --cov=. --cov-report=term-missing
```

A coluna `Missing` mostra exatamente os números das linhas descobertas.

### Etapa 3 — Completar os testes

Edite `test_calculadora.py` e escreva os testes que faltam.
Cada função deve ter pelo menos:

- Um teste para o caminho feliz (entrada válida, resultado esperado)
- Um teste para o caminho de erro quando aplicável
  (ex.: divisão por zero, percentual negativo)

Quando a cobertura local atingir 80% ou mais:

```bash
git add test_calculadora.py
git commit -m "test: completa cobertura para divisao, percentual e eh_par"
git push origin pipeline
```

Verifique que o pipeline ficou **verde** na aba Actions.

---

## Entrega

Envie no Moodle o link para o seu repositório (fork) contendo:

- [ ] Arquivo `.github/workflows/ci.yml` presente e correto
- [ ] Badge de status **passing** visível neste README
  (substitua `SEU-USUARIO` na linha do badge pelo seu usuário GitHub)
- [ ] Histórico de commits mostrando a progressão:
  pipeline vermelho → testes adicionados → pipeline verde

> **Atenção:** o histórico de commits faz parte da avaliação.
> Não entregue apenas o estado final.

---

## Referência Rápida

```bash
# lint
flake8 . --max-line-length=100

# testes com cobertura (exibe linhas descobertas)
pytest --cov=. --cov-report=term-missing

# testes com limiar (simula o comportamento do CI)
pytest --cov=. --cov-report=term-missing --cov-fail-under=80
```