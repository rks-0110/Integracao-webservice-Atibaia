
<!--
    Para melhor visualização do arquivo utilize:
    (Ctrl + Shift + V) Visual Studio Code [app]
    ou
    (copiar e colar todo conteudo) https://markdownlivepreview.com/ 
 -->

<a name="top"></a>

# Integração Emissão de NFSe Atibaia | JK
<center>
[Português](./lang/README.pt.md)|[English](./lang/README.en.md)
</center>
___

## Sumário
- [Integração Emissão de NFSe Atibaia | JK](#integração-emissão-de-nfse-atibaia--jk)
  - [Sumário](#sumário)
  - [Objetivos](#objetivos)
  - [Recursos](#recursos)
  - [Prefeitura](#prefeitura)
    - [Links úteis](#links-úteis)
  - [WebService](#webservice)
    - [Funções / Endpoints](#funções--endpoints)
      - [Serviços que não requerem autorização](#serviços-que-não-requerem-autorização)
      - [Serviços que requerem autorização](#serviços-que-requerem-autorização)
  - [Código](#código)
    - [Enviar NFSe](#enviar-nfse)
      - [Método "salvaXml"](#método-salvaxml)
        - [Variáveis](#variáveis)
        - [Contexto](#contexto)
      - [Método "main"](#método-main)
        - [Variáveis](#variáveis-1)
        - [Contexto](#contexto-1)
    - [Cancelar NFSe](#cancelar-nfse)
      - [Método "salvaXml"](#método-salvaxml-1)
        - [Variáveis](#variáveis-2)
        - [Contexto](#contexto-2)
      - [Método "main"](#método-main-1)
        - [Variáveis](#variáveis-3)
        - [Contexto](#contexto-3)
    - [Obs](#obs)
  - [Caminhos](#caminhos)
  - [Atualizações](#atualizações)
    - [***Anterior***](#anterior)
    - [***07/03/2023***](#07032023)
    - [***10/03/2023***](#10032023)
    - [***13/03/2023***](#13032023)
    - [***14/03/2023***](#14032023)
    - [***15/03/2023***](#15032023)

___

## Objetivos

- [x] Enviar arquivo .xml para emissão de Nota Fiscal de Serviço Eletrônica (NFSe);
- [x] Receber .xml de retorno e código de verificação da nota;
- [x] Enviar .xml para cancelamento da NFSe 

[Voltar ao topo](#top)
___

## Recursos
- A linguagem utilizada foi Java, para integração mais fácil com o sistema da empresa. ([NetBeans](https://netbeans.apache.org/) IDE 16).
- Para efeito de testes de requisição foi usilizado também o [PostMan](https://www.postman.com/).
- O WebService utilizado é próprio da prefeitura `http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/emissao`.
- O WebService utilizado é próprio da prefeitura `http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/cancela`.

[Voltar ao topo](#top)
___

## Prefeitura
A prefeitura disponibiliza algums manuais e tutoriais para a implementação com o WebService. 

### Links úteis
- [Prefeitura](https://www.atibaia.sp.gov.br/)
- [Página inicial NFSe](https://nfse.prefeituradeatibaia.com.br/ords/atibaia/f?p=901:70:0)
  - [Manuais de ajuda e legislação](https://nfse.prefeituradeatibaia.com.br/ords/atibaia/f?p=901:72)
  - [Tutoriais do sistema NFS-e](https://nfse.prefeituradeatibaia.com.br/ords/atibaia/f?p=901:3)
- [Contribuintes/Login](https://nfse.prefeituradeatibaia.com.br/ords/atibaia/f?p=890:101::::::)

[Voltar ao topo](#top)
___

## WebService
Aparentemente não é muito estável, podem ocorrer quedas ou passar por manutenções que podem durar bastante tempo.  
Como observado no dia 14/03/2023 onde com um teste às 08:22 houve um retorno positivo, porém ao longo do dia passou a retornar somente o seguinte erro:  
>Exception in thread "main" java.io.IOException: Server returned HTTP response code: 500 for URL: http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/emissao/simula  

### Funções / Endpoints
>- [x] indica que pode ser acessado pelo browser
>- [ ] indica que NÃO pode ser acessado pelo browser  

#### Serviços que não requerem autorização
- [x] Serviço utilizado para verificar se serviços relacionados a emissão de NFSe estão disponíveis.
  - http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/hello
- [x] Serviço utilizado para visualizar modelo de arquivo xml a ser enviado.
  - http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/exemplo 
- [ ] Serviço utilizado para verificar veracidade da NFSe
  - http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/consulta
  
#### Serviços que requerem autorização
- [ ] Serviço utilizado para realizar a SIMULAÇÃO do envio da NFSe.
  - http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/emissao/simula
- [ ] Serviço utilizado para realizar o ENVIO da NFSe.
  -  http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/emissao
- [ ] Serviço utilzado para realizar o CANCELAMENTO da NFSe.
  - http://ws.prefeituradeatibaia.com.br/WSNfses/nfseresources/ws/v2/cancela

[Voltar ao topo](#top)
___

## Código
### Enviar NFSe
#### <u>Método "salvaXml"</u>
##### Variáveis
>filePath = caminho onde será salvo o retorno do XML  

##### Contexto
*Recebe como parâmetro o conteúdo do xml response, cria novo arquivo .txt especificado em `filePath`, o conteúdo é escrito no arquivo, há duas situações de 'System.out.println' que retornam se foi ou não possível salvar o arquivo.*

#### <u>Método "main"</u>
##### Variáveis
>conteudoRetorno = String responsável por passar o conteúdo do xml para que seja salvo pelo método `salvaXml`.  

>xmlFilePath = camilnho do xml de envio *("J:\\RP-GRF\\NFSE-Jk\\ESTAB-101\\RETORNO\\RETORNO.txt")*.

>xmlBody = esta variável irá armazenar o conteúdo do xml passado non parâmetro anterior.

>url = armazena o valor da url que será usada para fazer uma requisição no webservice.

>status = armazena o [http status](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status) de uma requisição *(200 == ok | outros == falha ou erro)*.

##### Contexto
*Inicia tentando conexão com a `url`, especifica o método 'POST', passa as autorizações (inscrição municipal e token de acesso), especifica o tipo do conteúdo 'application/xml'.*  
*Lê o retorno e o transfere para `xmlBody`, recebe o http status e o armazena em `status`, então o conteudo do `xmlBody` é passado como 'String' para `conteudoRetorno`*.
*Por fim retorna o status com 'System.out.println' e então chama o método `salvaXml`.*  


### Cancelar NFSe
#### <u>Método "salvaXml"</u>
##### Variáveis
>filePath = caminho onde será salvo o retorno do XML

##### Contexto
*Recebe como parâmetro o conteúdo do xml response, cria novo arquivo .txt especificado em `filePath`, o conteúdo é escrito no arquivo, há duas situações de 'System.out.println' que retornam se foi ou não possível salvar o arquivo.*

#### <u>Método "main"</u>
##### Variáveis
>conteudoRetorno = String responsável por passar o conteúdo do xml para que seja salvo pelo método `salvaXml`.  

>xmlFilePath = camilnho do xml de envio *("J:\\RP-GRF\\NFSE-Jk\\ESTAB-101\\RETORNO\\RETORNO.txt")*.

>xmlBody = esta variável irá armazenar o conteúdo do xml passado non parâmetro anterior.

>url = armazena o valor da url que será usada para fazer uma requisição no webservice.

>status = armazena o [http status](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status) de uma requisição *(200 == ok | outros == falha ou erro)*.

##### Contexto
*Inicia tentando conexão com a `url`, especifica o método 'POST', passa as autorizações (inscrição municipal e token de acesso), especifica o tipo do conteúdo 'application/xml'.*  
*Lê o retorno e o transfere para `xmlBody`, recebe o http status e o armazena em `status`, então o conteudo do `xmlBody` é passado como 'String' para `conteudoRetorno`*.
*Por fim retorna o status com 'System.out.println' e então chama o método `salvaXml`.*  

### Obs
O projeto visa tratar um xml por vez, ou seja não armazena xmls de NFSes anteriores, o mesmo vale para os xmls de retorno, que são sempre substituídos para salvar espaço de armazenamento.

[Voltar ao topo](#top)
___

## Caminhos
- *xml de envio: J:\RP-GRF\NFSE-JK\ESTAB-101\XML*  
- *xml de retorno: J:\RP-GRF\NFSE-JK\ESTAB-101\RETORNO* 

___

<!-- ## Erros

___ -->

## Atualizações
### ***Anterior***
Seguindo o material disponível no [site da prefeitura](https://www.atibaia.sp.gov.br/#) e dentro do sistema, inicialmente foram feitos testes de requisição no [Postman](https://www.postman.com/), em seguida tentou-se explorar a possibilidade de se fazer a integração com [Python](https://www.python.org/).
### ***07/03/2023***
Iniciou-se o desenvolvimento do projeto em java devido a maior facilidade que seria implementar com o sistema.  
Utilizando [NetBeans IDE](https://netbeans.apache.org/).  
Em testes subsequentes foi comum enfrentar alguns [erros](#erros) recorrentes.  
### ***10/03/2023***
Projeto de envio de NFSe funcionando.
### ***13/03/2023***
Problemas ao tentar implementar o cancelamento de NFSe.
### ***14/03/2023***
Projeto separado em duas aplicações java diferentes Enviar e Cancelar.  
Após 08:22 (último teste bem sucedido) o webservice ficou em provavel manutenção pelo resto do dia.
### ***15/03/2023***
Retorno do [suporte do provedor](tributos@presconinformatica.com.br) informando que uma correção havia sido efetuada.  
Projeto foi concluído, compilação bem sucedida e teste com conexão remota no cliente também com resultados positivos.  

[Voltar ao topo](#top)

