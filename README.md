# LABIC _Standards_

Coleção de decisões de padronizações (_standards_) para código, documentação e comunicação do LABIC.

O objetivo dessa documentação é propor um conjunto de regras e boas práticas para o desenvolvimento de _software_, documentação e comunicação no LABIC. A ideia é que essas regras sejam seguidas por todos os membros do laboratório, para garantir a qualidade e a consistência dos projetos desenvolvidos, facilitando o trabalho colaborativo e manutenção do projeto.

O presente documento (_standard_) não é forma alguma uma verdade máxima, mas sim um guia para manter a qualidade e a consistência dos projetos. É esperado que um projeto se baseie nesse documento, mas que também possa ser adaptado conforme a necessidade do projeto.

Assumo que todo membro tem conhecimento básico de controle de versão, _git_ e _GitHub_, e que sabe como criar um _Pull Request_ e revisar o código de outros membros.

O documento deve abordar, principalmente, as seguintes áreas:

# Custódia de informações

Toda informação referente a um projeto deve estar facilmente acessível junto ao produto do projeto. 
Evite escrever documentações em locais desacoplados do projeto, como Google Docs, Notion, etc. Isso dificulta a manutenção e a colaboração, e pode fazer com que a documentação fique desatualizada.

Documente tudo que possível em _plaintext_, ou seja, arquivos de texto simples, como `.md`, `.txt`, etc. Caso queira saber o porquê, assista [este vídeo do No Boilerplate](https://www.youtube.com/watch?v=WgV6M1LyfNY) ou preferivelmente [esta conferência de Dylan Beattie](https://www.youtube.com/watch?v=gd5uJ7Nlvvo) sobre o poder do _plaintext_.

Exemplo: Se descubro alguma limitação na API _export comments_, eu vou ao projeto que usa a API e anoto aquela informação. Nesse momento não é importante que a informação esteja muito organizada, mas que ela esteja lá.

Sobre como organizar essas informações, veja a sessão de [documentação](#documentação).

# Documentação

A documentação é uma parte essencial de qualquer projeto. Ela é a forma de comunicar o que foi feito, como foi feito e como o projeto pode ser utilizado. A documentação deve ser clara, objetiva e completa.

Veja também a sessão sobre [documentação de código](#documentação-de-código).


## Documentação de projeto

Refere-se a documentação que não está diretamente ligada ao código, mas sim ao projeto na totalidade. Pode ser um `README.md` ou semelhantes. Observando a sessão sobre [custódia de informações](#custódia-de-informações), a documentação de projeto deve estar dentro do repositório do projeto e ser o ponto de partida para encontrar informações sobre o projeto. Ele não precisa explicar o que cada módulo faz, mas uma boa base para uma documentação de projeto é:

- O que é o projeto?
  - O que ele faz?
- Como instalar?
- Como usar?
- Como contribuir? E isso inclui:
  - Como subir o ambiente de desenvolvimento (Docker, etc.)
  - Como rodar testes

Conforme o projeto, também devem incluir outras informações se relevantes, como por exemplo: 
- _benchmarks_
- _roadmaps_
- Limitações técnicas
- Limitações de escopo
- [Diagramas](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams)
- etc.

Lembre de manter a documentação amigável, esta em especial, pois ela será lida por pessoas que não estão familiarizadas com o projeto. **Sempre que possível, inclua exemplos**.

# Código

## Documentação de código

Refere-se aos textos de documentação escritos junto ao código.
- Se num módulo, explique o que o módulo deve fazer e possivelmente o que ele não deve fazer
- Se numa função, explique o que a função faz, o que ela retorna e o que ela espera como argumentos. As funções expostas para serem usadas por outros módulos ou usuários devem ter uma atenção especial na documentação.

Deve ser dado preferência a estes sobre documentação não anexa ao código.

Exemplos:
- [_docstring_ do Python](https://peps.python.org/pep-0257/)
- [JSDoc do JavaScript/TypeScript](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html).

## Anote!

Não tenha vergonha de deixar uns `TODO` ou `FIXME` no código. Eles são uma forma de lembrar que algo precisa ser feito ou melhorado, e não devemos nos sentir envergonhados por não fazer algo. Afinal, o código é um trabalho em progresso perpétuo, nós temos tempo finito e precisamos traçar a linha em algum lugar. \
Da mesma forma, não tenha vergonha de anotar se você não tem certeza como uma função de uma biblioteca funciona, isso facilita muito a manter o código, e difere especial atenção ao trecho de código duvidoso, fazendo com que ele seja revisado por alguém que consiga entender.

Exemplo:
```python
def process_list(l: List[int]) -> List[int]:
  # TODO Use numpy for faster processing
  ...
```

## Testes

Testes são uma parte essencial do desenvolvimento de software. Eles garantem que o código funciona conforme esperado e que mudanças futuras não quebrarão o código.

Os testes devem ser escritos junto ao código, e devem cobrir o máximo possível de casos de uso. Testes unitários, de integração e _end-to-end_ são todos importantes e devem ser usados conforme necessário. Recomendo o uso de _test runners_ como [`pytest`](https://docs.pytest.org/en/latest/getting-started.html) para Python e [`jest`](https://jestjs.io/docs/getting-started) para TypeScript para facilitar a escrita e execução dos testes.

Onde manter estes testes, sendo no mesmo arquivo, num arquivo ao lado ao que é testado, ou em um diretório `tests` é uma decisão do projeto e de seus mantenedores. É importante que os testes sejam fáceis de encontrar e de rodar.

Exemplo:
```python
def process_list(l: List[int]) -> List[int]:
  return [i + 1 for i in l]

def test_process_list():
  assert process_list([1, 2, 3]) == [2, 3, 4]
  assert process_list([1, 2, 3, 4]) == [2, 3, 4, 5]
```
Executando `pytest` no diretório do arquivo acima, os testes serão executados e o resultado será mostrado.

```
$ pytest
============================= test session starts ==============================
platform linux -- Python 3.9.7, pytest-6.2.5, pluggy-1.0.0
rootdir: /home/username/projects/project
collected 1 item

test_example.py .                                                        [100%]

============================== 1 passed in 0.01s ===============================
```


## CI/CD

Integração contínua (_continuous integration_, CI) e entrega contínua (_continuous delivery_, CD) são práticas que garantem que o código é testado e entregue de forma automática. Isso garante que o código é testado e que mudanças não quebram o código. Aliado às técnicas de [testes](#testes) descritos anteriormente, os CI/CD são uma ferramenta poderosa para garantir a qualidade e funcionalidade do código.

Configurando um fluxo de CI para um repositório em que o CI executa os testes em todo o repositório, garantimos assim que se o CI passar, o código está em um estado funcional. \
O CD pode ser configurado para fazer deploy automático para um ambiente de produção, especialmente útil para fazer release de novas versões para projetos em produção, ou também para fazer deploy de novas versões de uma página web.

Exemplo (de [abatilo/actions-poetry](https://github.com/abatilo/actions-poetry))

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Python
        uses: actions/setup-python@v4
        # see details (matrix, python-version, python-version-file, etc.)
        # https://github.com/actions/setup-python
      - name: Install poetry
        uses: abatilo/actions-poetry@v2
      - name: Setup a local virtual environment (if no poetry.toml file)
        run: |
          poetry config virtualenvs.create true --local
          poetry config virtualenvs.in-project true --local
      - uses: actions/cache@v3
        name: Define a cache for the virtual environment based on the dependencies lock file
        with:
          path: ./.venv
          key: venv-${{ hashFiles('poetry.lock') }}
      - name: Install the project dependencies
        run: poetry install
      - name: Run the automated tests (for example)
        run: poetry run pytest -v
```

## Colaboração

Para colaborar, sugiro o [GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow) com merge-commits. Para resolver conflitos, sugiro o uso de _rebase_ interativo.

É definido também que o branch `main` é protegido, e que [Pull Requests](#pull-requests) devem ser feitos para a `main` e não para outras branches.

### Reprodutibilidade

Para garantir a reprodutibilidade do código, é recomendado o uso de ambientes virtuais, como [`poetry`](https://python-poetry.org/docs/) para Python[^1], [`yarn`](https://yarnpkg.com/getting-started) para JavaScript/TypeScript, etc. Isso garante que o código é executado em um ambiente controlado e que as dependências são as mesmas em todos os lugares.

[^1]: O Poetry tem algumas vantagens em relação a outros gerenciadores de dependências, como o `pip` e o `requirements.txt`, por exemplo ele pode configurar o ambiente virtual e algumas dependências, além de conter toda sua configuração em um arquivo `pyproject.toml` legível. É muito mais difícil de manter um ambiente dessincronizado com o `poetry.lock` do que com o `requirements.txt` e o `pip`.

### Scripts de projeto

Sempre que viável, use scripts para automatizar tarefas comuns, como formatação, linting, testes, etc. Isso facilita a execução dessas tarefas e garante que elas são feitas da mesma forma em todos os lugares.

Examplo:
Não só instale o `pylint`, mas crie também um script para rodar o `pylint` no projeto usando a ferramenta de gerência de dependências do projeto.

### Commits

Commits devem ser pequenos e frequentes, e devem ser feitos sempre que uma mudança significativa for feita. Commits grandes e com muitas mudanças são difíceis de revisar e entender, e podem causar problemas no futuro.

Sobre mensagens de commit, recomendo seguir o padrão [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#specification), que é um padrão de mensagens de commit que facilita a geração de _changelogs_ e a compreensão do que foi feito em cada commit.

Detalhe a mensagem de commit com o que foi feito, e não o porquê. O porquê pode ser detalhado no corpo da mensagem caso haja necessidade (se não há algum comentário ou documentação já fazendo isso), mas a mensagem deve ser curta e objetiva.

Mensagens de commit são da forma
```
<mensagem do commit>

<comentário/corpo do commit>
```
Exemplos de mensagem de commit:
```
feat(coleta): baixa dados do exportcomments

Utiliza webhooks para se comunicar com o backend.
Ainda não está implementado o tratamento de erros.
```
```
fix!(preprocessing): faz o processamento durante a coleta

Agora o processamento é feito durante a coleta, e não após.

BREAKING CHANGE: Os campos de `text` e `author` foram removidos da saída da coleta.
```

### Issues

Sempre que encontrar um problema, falha, dúvida ou ter uma ideia para melhoria, mas que não seja curta o bastante para um [comentário](#comentários), crie uma _issue_. As issues são uma forma de manter o controle do que precisa ser feito e de discutir ideias e problemas. Caso não seja relevante, ou desatualize, podemos fechar a issue no futuro.

### Pull Requests

Não envie código diretamente para a `main`, sempre faça um _Pull Request_ (PR). O PR é uma forma de revisar o código antes de ser integrado ao projeto, e é uma forma de garantir que o código é revisado e testado antes de ser integrado.

O PR deve ser pequeno e conter apenas uma mudança idealmente. Isso facilita a revisão e a compreensão do que foi feito. O PR deve conter uma descrição clara do que foi feito e do porquê, e deve conter testes para garantir que o código funciona conforme esperado.

É entendido que PRs que mudem coisas básicas precisem de mudanças grandes, neste caso, recomendo dividir em PRs menores e fazer o merge em um branch temporário, que então é re-revisado e re-testado, com o código final sendo integrado à `main`.

### Tags

Tags são uma forma de marcar versões do projeto. Sempre que uma nova versão for lançada, crie uma tag com o número da versão. Isso facilita a identificação de versões e a geração de _changelogs_.
