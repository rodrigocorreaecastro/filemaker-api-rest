# FileMaker API Rest PHP


## Instalação

### Usando o Composer
Você pode usar o gerenciador de pacotes `composer` para instalar. Ou execute:

    $ php composer.phar require airmoi/filemaker "*"

ou adicione:

    "airmoi/filemaker": "^2.2"

para seu arquivo composer.json

### Instalação manual

Você também pode instalar manualmente a API em seu projeto. Basta baixar a fonte [ZIP](https://github.com/airmoi/FileMaker/archive/master.zip) e extrair seu conteúdo em seu projeto.

## Uso

PASSO 1: Inclua o carregamento automático da API

```php
require '/path/to/autoloader.php';
```
*Esta etapa é facultativa se você estiver usando o compositor*

PASSO 2 : Criar uma instância do FileMaker
```php
use airmoi\FileMaker\FileMaker;

$fm = new FileMaker($database, $host, $username, $password, $options);
```

PASSO 3: Use-o da mesma maneira que usaria a API oficial...

...E aproveite a conclusão de código usando seu IDE favorito e suporte a php 7 sem aviso prévio.


### Exemplo de código de demonstração

```php
<?php
header('Content-Type: application/json; charset=utf-8');

$data = [];


use airmoi\FileMaker\FileMaker;
use airmoi\FileMaker\FileMakerException;
use airmoi\FileMaker\FileMakerValidationException;


require('/path/to/autoloader.php');


$fm = new FileMaker('database', 'localhost', 'userFilemaker', 'pwdFilemaker', ['prevalidate' => true]);


try {

    $layout_selected = 'pacotes_online';
    $data['config']['layout'] = $layout_selected;

    $findCommand = $fm->newFindCommand($layout_selected);

    $id = (isset($_GET['id'])) ? $_GET['id'] : null; // '81309...81357' OR 81309
    if ($id):
        $findCommand->addFindCriterion('id_cadastro_pacotes', $id);
    endif;

    $nome = (isset($_GET['nome']))?$_GET['nome']:null; //'nome'
    if ($nome):
        $findCommand->addFindCriterion('nome', $nome);
    endif;

    $situacao = (isset($_GET['situacao']))?$_GET['situacao']:null; // 'Ativo' OR 'Inativo'
    if ($situacao):
        $findCommand->addFindCriterion('situacao', $situacao);
    endif;

    $codigo_pacote = (isset($_GET['codigo_pacote']))?$_GET['codigo_pacote']:null;
    if ($codigo_pacote):
        $findCommand->addFindCriterion('codigo_pacote', $codigo_pacote);
    endif;


    try {
        $records = $findCommand->execute()->getRecords();

        $data['config']['registros'] = count($records);

        foreach($records as $record):
            $data["codigo_pacote_digital"] = trim( $record->getField('codigo_pacote_digital') );
            $data["valor_comissao_para_venda"] = trim( $record->getField('valor_comissao_para_venda') );
            $data["pacote_minimo"] = trim( $record->getField('pacote_minimo') );
            $data["autenticacao"] = trim( $record->getField('autenticacao') );
            $data["id_produto"] = trim( $record->getField('id_produto') );
            $data["nome"] = trim( $record->getField('nome') );
            $data["valor_pacote"] = trim( $record->getField('valor_pacote') );
            $data["pf_pj"] = trim( $record->getField('pf_pj') );
            $data["analogico_digital"] = trim( $record->getField('analogico_digital') );
            $data["situacao"] = trim( $record->getField('situacao') );
            $data["obs"] = trim( $record->getField('obs') );
            $data["id_cadastro_pacotes"] = trim( $record->getField('id_cadastro_pacotes') );
            $data["upload"] = trim( $record->getField('upload') );
            $data["desconto"] = trim( $record->getField('desconto') );
            $data["cidade"] = trim( $record->getField('cidade') );
            $data["grupo_pacotes"] = trim( $record->getField('grupo_pacotes') );
            $data["pacote_referencia"] = trim( $record->getField('pacote_referencia') );
            $data["desconto_pagamento_local"] = trim( $record->getField('desconto_pagamento_local') );
            $data["desconto_fatura_digital"] = trim( $record->getField('desconto_fatura_digital') );
            $data["desconto_cartao_credito"] = trim( $record->getField('desconto_cartao_credito') );
        endforeach;

        $fm->endProfile('IrG6U+Rx0F5bLIQCUb9gOw==');
        $fm->logout();


    }//.try()
    catch (FileMakerException $e) {
        $data['error'] = 'Não foi localizado nenhum registro até o momento.';
        $data['message'] = $e->getMessage();
        $data['code'] = $e->getCode();
    }//.catch()


} catch (FileMakerException $e) {
    $data['error'] = "EXCEPTION";
    $data['at'] = $e->getFile();
    $data['line'] = $e->getLine();
    $data['code'] = $e->getCode();
    $data['message'] = $e->getMessage();
    $data['stack'] = $e->getTraceAsString();
} catch (Exception $e) {
    $data['error'] = "EXCEPTION";
    $data['at'] = $e->getFile();
    $data['line'] = $e->getLine();
    $data['code'] = $e->getCode();
    $data['message'] = $e->getMessage();
    $data['stack'] = $e->getTraceAsString();
}


echo json_encode($data);
```
