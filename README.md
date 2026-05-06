# CC8550 -- CI/CD Lab

![CI](https://github.com/SEU-USUARIO/cc8550-cicd-lab/actions/workflows/ci.yml/badge.svg)

Repositorio de apoio para a atividade pratica de CI/CD da disciplina
**CC8550 -- Simulacao e Teste de Software** (Centro Universitario FEI).

---

## Objetivo

Configurar um pipeline de Integracao Continua com **GitHub Actions** para
este repositorio, fazendo-o passar por todas as etapas:
**lint → testes → cobertura minima de 80%**.

---

## Estrutura do Repositorio

```
cc8550-cicd-lab/
├── calculadora.py          # modulo com 6 funcoes matematicas
├── test_calculadora.py     # testes -- INCOMPLETOS (cobertura ~65%)
├── requirements.txt        # dependencias do projeto
└── README.md               # este arquivo
```

---

## Como Comecar

### 1. Faca um fork

Clique em **Fork** no canto superior direito desta pagina.
O fork cria uma copia do repositorio na sua conta pessoal do GitHub
-- e la que o pipeline vai rodar.

### 2. Clone o fork localmente

```bash
git clone https://github.com/Rossi-Luciano/teste_de_software.git
cd cc8550-cicd-lab
```

### 3. Crie uma branch para a atividade

```bash
git checkout -b pipeline
```

### 4. Instale as dependencias

```bash
pip install -r requirements.txt
```

### 5. Verifique o estado atual

```bash
# rode o lint
flake8 . --max-line-length=100

# rode os testes com relatorio de cobertura
pytest --cov=. --cov-report=term-missing
```

Voce vera que a cobertura atual e de aproximadamente **65%** --
abaixo do minimo exigido de 80%.

---

## Etapas da Atividade

### Etapa 1 -- Criar o pipeline

Crie o arquivo `.github/workflows/ci.yml` com as seguintes etapas:

1. Trigger: `push` e `pull_request` para `main`
2. Setup Python 3.11
3. Instalacao via `requirements.txt`
4. Lint com `flake8`
5. Testes com cobertura minima de 80% (`--cov-fail-under=80`)
6. Publicacao do relatorio como artefato

Faca o commit e o push:

```bash
git add .github/workflows/ci.yml
git commit -m "ci: adiciona pipeline inicial"
git push origin pipeline
```

Abra a aba **Actions** no seu repositorio e observe o pipeline rodar.

### Etapa 2 -- Observar a falha

O pipeline devera **falhar** na etapa de cobertura.
Leia o log do step com atencao:

- Quais funcoes nao estao cobertas?
- Quais linhas especificas nao foram executadas?

Use o comando local para inspecionar:

```bash
pytest --cov=. --cov-report=term-missing
```

A coluna `Missing` mostra exatamente os numeros das linhas descobertas.

### Etapa 3 -- Completar os testes

Edite `test_calculadora.py` e escreva os testes que faltam.
Cada funcao deve ter pelo menos:

- Um teste para o caminho feliz (entrada valida, resultado esperado)
- Um teste para o caminho de erro quando aplicavel
  (ex.: divisao por zero, percentual negativo)

Quando a cobertura local atingir 80% ou mais:

```bash
git add test_calculadora.py
git commit -m "test: completa cobertura para divisao, percentual e eh_par"
git push origin pipeline
```

Verifique que o pipeline ficou **verde** na aba Actions.

---

## Entrega

Envie no Moodle o link para o seu repositorio (fork) contendo:

- [ ] Arquivo `.github/workflows/ci.yml` presente e correto
- [ ] Badge de status **passing** visivel neste README
  (substitua `SEU-USUARIO` na linha do badge pelo seu usuario GitHub)
- [ ] Historico de commits mostrando a progressao:
  pipeline vermelho → testes adicionados → pipeline verde

> **Atencao:** o historico de commits faz parte da avaliacao.
> Nao entregue apenas o estado final.

---

## Referencia Rapida

```bash
# lint
flake8 . --max-line-length=100

# testes com cobertura (exibe linhas descobertas)
pytest --cov=. --cov-report=term-missing

# testes com limiar (simula o comportamento do CI)
pytest --cov=. --cov-report=term-missing --cov-fail-under=80
```
