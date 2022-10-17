<img src="https://user-images.githubusercontent.com/51168329/159381241-c0b2b316-22de-492c-9703-23e84e860551.png">
<div align="center">
  <a href="https://github.com/andreinaoliveira/QA-Base-Automation"><img alt="hits" src="https://hits.sh/github.com/andreinaoliveira/QA-Base-Automation.svg"/></a>
  <a href="https://github.com/andreinaoliveira/QA-Base-Automation/graphs/commit-activity"><img src="https://img.shields.io/github/last-commit/andreinaoliveira/qa-base-automation"></a>
  <a href="https://github.com/andreinaoliveira/QA-Base-Automation"><img src="https://img.shields.io/badge/status-complete-light"></a>
  <a href="https://github.com/andreinaoliveira/QA-Base-Automation/stargazers"><img src="https://img.shields.io/github/stars/andreinaoliveira/QA-Base-Automation?style=social"></a>
  <a href="https://github.com/andreinaoliveira/QA-Base-Automation/network/members"><img src="https://img.shields.io/github/forks/andreinaoliveira/QA-Base-Automation?style=social"></a>
  <a href="https://github.com/andreinaoliveira"><img src="https://img.shields.io/github/followers/andreinaoliveira?style=social"></a>
</div>

# 💬 Sobre
Base para automação de testes utilizando a linguagem Python com as tecnologias do Selenium WebDriver e UnitTest e com a estrutura organizacional CMT (Controller Model Test), uma adaptação do MVC.

Para exemplificar o funcionamento da base será automatizado a tela de login do site Netflix. para cobrir os seguintes cenários de teste:
- CT01 - Acessar tela de Boas Vinda
- CT02 - Acessar tela de Login
- CT03 - Senha Inválida
- CT04 - Usuário Inválido
- CT05 - Usuário Válido

# 🧾 Índice
- <a href="#-instalação">Instalação</a>
- <a href="#-desenvolvimento">Desenvolvimento</a>
  - <a href="#-controller">Controller</a>
    - <a href="#formatpy">/fortmat.py</a>
    - <a href="#logpy">/log.py</a>
    - <a href="#webdriverpy">/webdriver.py</a>
      - <a href="#__code">Code</a>
      - <a href="#find">Find</a>
      - <a href="#click">Click</a>
      - <a href="#set">Set</a>
  - <a href="#-model">Model</a>
    - <a href="#check">Check</a>
    - <a href="#click">Click</a>
    - <a href="#set">Set</a>
  - <a href="#-test">Test</a>
    - Em andamento
- <a href="#-cenários-de-teste">Cenários de Teste</a>
  - Em andamento


# 💾 Instalação

**Projeto**

```
git clone https://github.com/andreinaoliveira/QA-Base-Automation.git
```

**Dependencias**
* Python 3

**Módulos**

Os módulos devem ser instalados no cmd com os comandos abaixos
```
pip install behave
pip install selenium
pip install webdriver-manager
```

# 🖥 Desenvolvimento
## 🕹 Controller

### /log.py

Importa a biblioteca de loggin e formata a mensagem de log. Nesse arquivo é criado as funções debug(), info() e error(). Cada função recebe a mensagem que será enviada como log. Essas funções são chamadas em controller/webdriver. A linha referente a basic config está por padrão comentada para evitar poluição visual no terminal. Contudo, quando houver erros, os logs de erro serão chamados.

```python
import logging
log_format = '%(asctime)s :: %(name)s :: %(levelname)s :: %(module)s :: %(message)s'
#logging.basicConfig(format=log_format, level=logging.INFO, filemode='w')


def degub(message):
    logging.debug(message)

def info(message):
    logging.info(message)

def error(message):
    logging.error(message)
```

### /webdriver.py

Em webdriver.py é criada a classe Element com os seguintes atribuitos e importações:
- driver: recebe o webdriver que será criado apenas no teste.
- name: nome do elemento ex.: Botão Sign In. O nome será enviado apenas nos log's. 
- element: Ao ser encontrado, o elemento é salvo nesse atributo.
- as_Code_Type...: É a referência do elemento. É necessário atribuir valor a um dos itens para poder usar as funções da classe. 

```python
import os
import sys
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from controller import log
```

```python
class Element:
    def __init__(self, driver, name):
        self.driver = driver
        self.name = name
        self.element = None
        self.as_1_ID = None
        self.as_2_CLASS_NAME = None
        self.as_3_NAME = None
        self.as_4_TAG_NAME = None
        self.as_5_LINK_TEXT = None
        self.as_6_PARTIAL_LINK_TEXT = None
        self.as_7_CSS_SELECTOR = None
        self.as_8_XPATH = None
```

As funções da classe ao serem chamadas (find, click e set), executará as ações e retornará [True] ou [False] de acordo com o sucesso ou não da atividade. Portanto, além de executar a ação você poderá comparar o resultado, por exemplo, checar se retornou True, ou seja, checar se a ação foi executada com sucesso.

### __Code

A função é chamada na função anterior, find(). Recebe um valor inteiro na variável code, o valor está no range de 1 a 8 e se refere ao tipo de referência do elemento (id, class etc.) o código é o mesmo do atributo as_Code_Type da classe. Exemplo, instanciando a classe como as_1_ID o code da função deve ser _ code(1) para buscar por ID.
O ideal é que a função seja chamada apenas pelo find(), nunca diretamente.

```python
    def _code(self, code):
        if code == 1:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.ID, self.as_1_ID))
            )
        elif code == 2:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.CLASS_NAME, self.as_2_CLASS_NAME))
            )
        elif code == 3:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.NAME, self.as_3_NAME))
            )
        elif code == 4:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.TAG_NAME, self.as_4_TAG_NAME))
            )
        elif code == 5:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.LINK_TEXT, self.as_5_LINK_TEXT))
            )
        elif code == 6:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.PARTIAL_LINK_TEXT, self.as_6_PARTIAL_LINK_TEXT))
            )
        elif code == 7:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.CSS_SELECTOR, self.as_7_CSS_SELECTOR))
            )
        elif code == 8:
            self.element = WebDriverWait(self.driver, 10).until(
                EC.presence_of_element_located((By.XPATH, self.as_8_XPATH))
            )
        else:
            log.error('Código informado em find() fora do range ou inválido.')
            os.system("pause")
            sys.exit()
```

### Find
1. A Função tenta localizar o elemento e envia um log informando essa tentativa.
2. Não encontrando, imprime o log de erro e retorna Falso.
3. Encontrando, imprime log informando sucesso e retorna True.

```python
    def find(self, code):
        try:
            log.degub('Buscando ' + self.name)
            self._code(code)
        except Exception as e:
            log.error('Erro ao identificar ' + self.name)
            return False
        else:
            log.info(self.name + ' Identificado(a)')
            return True
```

### Click
1. Chama find() para atribuir valor ao atributo element da classe.
2. Tenta clicar no elemento identificado.
4. Não conseguindo, imprime o log de erro e retorna Falso
5. Conseguindo, imprime log informando sucesso e retorna True

```python
    def click(self, code):
        Element.find(self, code)
        try:
            self.element.click()
        except Exception as e:
            log.error('Erro ao clicar em ' + self.name)
            return False
        else:
            log.info(self.name + ' Clicado(a)')
            return True
```

### Set
1. Chama find() para atribuir valor ao atributo element da classe.
2. Tenta clicar no elemento identificado.
4. Não conseguindo, imprime o log de erro e retorna Falso.
5. Conseguindo, imprime log informando sucesso e retorna True.

```python
    def set(self, code, info):
        Element.find(self, code)
        try:
            self.element.send_keys(info)
        except Exception as e:
            log.error('Erro ao escerver ' + self.name)
            return False
        else:
            log.info(self.name + ' Inserido(a)')
            return True
```

## 🔧 Model
Modelo armazena todas as páginas de um sistema web em aquivos .py diferentes. Cada arquivo possui uma classe que se refere a página web em questão. O ideal é que os principais elementos da página sejam instanciados nessa classe herdando da classe Element de controller/webdriver. Para exemplificar, criamos o modelo da página de login da Netflix (login.py)

Como atribuito possuir:
- driver

As funções da página são divididas em: 
- Check: Checa se está na página, checa se alguma mensagem de erro é apresentada etc.
- Click: realiza o clique em qualquer elemento da página.
- Set: Insere alguma informação na página.

### Check
1. Instancia o elemento passando o driver e o nome do elemento.
2. Atribuir o valor da referência.
3. Retorna a função find que busca a referência informada.

```python
    def check_page_welcome(self):
        e = Element(self.driver, '[Welcome] Page')
        e.as_2_CLASS_NAME = 'our-story-card-title'
        return e.find(2)
```

### Click
1. Instancia o elemento passando o driver e o nome do elemento.
2. Atribuir o valor da referência.
3. Retorna a função click que tentará clicar na referência informada.

```python
    def click_signin_welcome(self):
        e = Element(self.driver, '[Welcome] Sign In button')
        e.as_5_LINK_TEXT = 'Sign In'
        return e.click(5)
```

### Set
1. Instancia o elemento passando o driver e o nome do elemento.
2. Atribuir o valor da referência.
3. Retorna a função set que tentará inserir uma informação na referência informada.

```python
    def set_email(self, email_or_number):
        e = Element(self.driver, '[Login] Email')
        e.as_1_ID = 'id_userLoginId'
        return e.set(1, email_or_number)
```

## 🧪 Test
Em desenvolvimento