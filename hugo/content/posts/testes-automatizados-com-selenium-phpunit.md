---
title: "Testes automatizados com Selenium + PHPUnit"
date: "2017-03-14"
aliases:
- /2017/03/testes-automatizados-com-selenium-phpunit/
categories:
- PHP
---

# Introdução

Automatizar tarefas repetitivas é vantajoso por diversos motivos, dentre os quais:

1. Ajuda a evitar lesões por esforço repetivivo
2. Evita erros
3. Libera tempo para se dedicar a tarefas criativas que agreguem valor ao seu negócio

Um caso típico de tarefa repetitiva que pode ser automatizada é o teste funcional de um aplicativo web. A cada nova alteração de código, tanto de backend (PHP, banco de dados), quanto de frontend (Javascript + CSS), uma infinidade de páginas devem ser testadas novamente para garantir que estão com funcionamento adequado.

# Selenium

Para nos ajudar no objetivo de automatizar esses testes, contamos com a biblioteca Selenium (cujo slogan é **Selenium automates browsers**), que se comunica com os principais navegadores de internet do mercado por meio de seus webdrivers, permitindo abrir URLs, clicar em links, inspecionar propriedades de elementos HTML e muito mais.

O Selenium possui _bindings_ para algumas das principais linguagens de programaçaão: Java, Python, PHP, C#, sendo que alguns desses bindings são oficiais, enquanto outros são disponibilizados por terceiros.

![](/images/Screenshot-from-2017-03-13-211245.png "Website do Selenium")

# PHPUnit

O PHPUnit é uma biblioteca do PHP tipicamente dedicada a testes unitários. Entretanto, utilizando-a em conjunto com o Selenium, pode-se gerar testes funcionais automatizados. É uma biblioteca com um objetivo bem definido, voltada para validar se algumas alegações (assertions) são verdadeiras ou falsas. Normalmente alegações falsas são comportamentos inesperados, e portanto são reportados pelo PHPUnit na forma de um relatório ao final da bateria de testes.

Exemplo de aplicação do PHPUnit retirado de [https://phpunit.de/getting-started.html](https://phpunit.de/getting-started.html):

[![](/images/Screenshot-from-2017-03-13-211923.png "Exemplo do PHPUnit")](/images/Screenshot-from-2017-03-13-211923.png)

# Instalação considerando o meu ambiente de desenvolvimento:

Para rodar os testes funcionais via PHPUnit + Selenium (tests/functional/*), acompanhe os passos seguintes:

1. Baixar o phpunit-5.*.*.phar [https://phar.phpunit.de](https://phar.phpunit.de) e copiar para /usr/local/bin, adicionando permissão de execução (chmod a+x)
2. Baixar o Selenium Server (selenium-server-standalone-*.jar) [http://www.seleniumhq.org/download/](http://www.seleniumhq.org/download/) em uma pasta de sua preferência
3. Baixar o Google Chrome Driver [https://sites.google.com/a/chromium.org/chromedriver/](https://sites.google.com/a/chromium.org/chromedriver/) e descompactar na mesma pasta escolhida no passo 2
4. Instalar o pacote phpunit-selenium via composer a partir da pasta raiz do git do G. Fox: composer.phar update --dev
5. Configurar config.json do G. Fox, adicionando o grupo abaixo de acordo com suas credenciais:
    "selenium": {
      "user": "meu.usuario@***.com",
      "password": "minha_senha"
    }
6. Depois de baixar todas as dependências acima:
    1. Entrar na pasta escolhida no passo 2, e rodar servidor do Selenium Server com o comando a seguir
java -jar selenium-server-standalone-3.0.1.jar. OBS1 - O nome do arquivo pode ser diferente conforme a versão. OBS2 - Se der erro para inicializar selenium-server-standalone, verifique se Google Chrome Driver foi instalado no caminho correto, a partir das mensagens de erros.
    2. Por último, rode o phpunit a partir da pasta raiz do repositório git do G. Fox: phpunit-5.5.4.phar --verbose --bootstrap load.inc tests/functional/browseEveryPageTest.php. OBS - O nome do executável pode ser diferente conforme a versão

# Métodos úteis

{{<highlight php>}}
class MyCustomSeleniumTestCase extends PHPUnit_Extensions_Selenium2TestCase {

  protected static $implicitTimeout = 20000;

  public static $browsers = array(
    array(
      'browserName' => 'chrome',
      'sessionStrategy' => 'shared',
    ),
  );

  /* This method is required to keep session alive even after failing tests */
  public function onNotSuccessfulTest($e){
    throw $e;
  }

  protected function setUp() {
    global $CFG;
    try {
      $this->test_user = $CFG->selenium->user;
      $this->test_password = $CFG->selenium->password;
      $this->test_url = $CFG->url->https;
    } catch (Exception $e) {
      throw new Exception('Incorrect config.json settings ($CFG->selenium->user, $CFG->selenium->password, $CFG->url->http)');
    }
    $this->setBrowserUrl($CFG->url->http);
    $this->prepareSession()->currentWindow()->maximize();
  }

  /**
   * Custom assertion
   * Checks if text of html body contains text
   */
  protected function assertPageContains($string) {
    try {
      $this->assertContains($string, $this->byCssSelector('body')->text());
    } catch (PHPUnit_Framework_ExpectationFailedException $e) {
      throw new PHPUnit_Framework_ExpectationFailedException("Test failed. Page <{$this->url()}> does not contain '$string'.");
    }
  }

  /**
   * Custom assertion
   * Checks if text of html body does not contain text
   */
  protected function assertPageDoesNotContain($string) {
    try {
      $this->assertNotContains($string, $this->byCssSelector('html')->attribute('innerHTML'));
    } catch (PHPUnit_Framework_ExpectationFailedException $e) {
      throw new PHPUnit_Framework_ExpectationFailedException("Test failed. Page <{$this->url()}> contains '$string'.");
    }
  }

  /* Block that types a value on autocomplete field, waits for
   * the options and click on the first option */
  protected function clickOnAutocomplete($selector, $value) {
    /*  Remove autocomplete elements that are left over on DOM, before trying to use a new one */
    $this->execute(array(
      'script' => '$(".ui-menu-item").remove()',
      'args' => array(),
    ));
    $this->byCssSelector($selector)->value($value);
    $this->byCssSelector('.ui-menu-item')->click();
    /* Blur event */
    $this->execute(array(
      'script' => '!!document.activeElement ? document.activeElement.blur() : 0',
      'args' => array(),
    ));
  }

  /* Sometimes Selenium cannot click on an element, so we can use this workaround */
  protected function jsClick($selector) {
    $this->execute(array(
      'script' => "$('$selector').click();",
      'args' => array(),
    ));
  }

  protected function waitShort() {
    sleep(2);
  }

  protected function waitLong() {
    sleep(10);
  }

  protected function stop() {
    sleep(99999);
  }

  /* Login */
  protected function testLoginCorrect() {

    /* Wait for 10 seconds at most for an element to be found */
    $this->timeouts()->implicitWait(self::$implicitTimeout);

    /* First we logout */
    $this->url('/login/logout');

    $this->url('/');
    $this->byId('usuario')->value($this->test_user);
    $this->byId('senha')->value($this->test_password);
    $this->byId('login')->submit();

    /* Redirect to home, skipping Tela Favorita */
    $this->url($this->tela_url . '/navegacao');

    $this->assertEquals($this->teste_url . '/navegacao', $this->url());
    $this->assertPageContains('AMBIENTE');
    $this->assertPageContains('NAVEGAR');
  }

  /**
   * Default tests for a page
   *
   * Check for Call Stack
   * Test button INSERIR
   * Test button VISUALIZAR
   */
  protected function defaultPageTests() {
    $this->closeOpenedAlert();
    $url = $this->url();
    /* Check if main page does not have Call Stack */
    $this->assertPageDoesNotContain("Call Stack");

    /* Click on button INSERIR, if exists, and check if does not have Call Stack */
    $this->checkButtonByText('INSERIR', $url);

    /* Do the same check for button BUSCAR */
    if ($this->checkButtonByText(array('BUSCAR', 'VISUALIZAR'))) {
      $this->checkShow();
      $this->url($url);
    }
    $this->checkShow();
  }

  function checkShow() {
    /* Do the same check for button A -> ALTERAR */
    if ($this->checkButtonByText(array('A', 'V'))) {
      $this->checkButtonByText(array('SALVAR', 'GRAVAR'));
    }
  }

  /* Close any opened alert dialog */
  protected function closeOpenedAlert() {
    $this->timeouts()->implicitWait(0);
    try {
      $this->acceptAlert();
    } catch (Exception $e) {
      /* Do nothing... It means that there is no alert dialog at the moment. */
    }
    $this->timeouts()->implicitWait(self::$implicitTimeout);
  }

  protected function checkButtonByText($search_texts, $back_url = false, $implicitTimeout = 0) {

    /* Transforma primeiro parâmetro em array de textos a serem pesquisados */
    if (!is_array($search_texts)) {
      $search_texts = array($search_texts);
    }

    $this->closeOpenedAlert();
    $this->timeouts()->implicitWait($implicitTimeout);
    try {
      /* If exists input #bloqueado or name='bloqueado', set its val to '0' so we can show records */
      $this->select($this->byXPath("//*[@id = '#bloqueado'] | //*[@name = 'bloqueado']"))->selectOptionByValue('0');
    } catch (Exception $e) {
    }

    $xpath_queries = array();
    foreach ($search_texts as $text) {
      $xpath_queries[] = "//*[text()='$text']/ancestor::*[contains(@class, 'btn')] | //*[text()='$text' and contains(@class, 'btn')] | //*[@value='$text' and contains(@class, 'btn')]";
    }
    $xpath_query = implode($xpath_queries, ' | ');

    try {
      $button = $this->byXPath($xpath_query);
    } catch (Exception $e) {
      /* If element not found, return test without errors */
      $this->timeouts()->implicitWait(self::$implicitTimeout);
      return false;
    }

    /* If found any button that matches $search_texts */
    $button->click();
    $this->timeouts()->implicitWait(self::$implicitTimeout);
    $this->closeOpenedAlert();
    $this->assertPageDoesNotContain("Call Stack");
    if (!empty($back_url)) {
      $this->url($back_url);
    }
    return true;
  }
}
{{</highlight>}}
