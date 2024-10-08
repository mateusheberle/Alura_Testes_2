# Testes de integração

1. pubspec.yaml
```sh
dev_dependencies:
  integration_test:
    sdk: flutter
```

2. nova pasta fora da /lib
    "/integration_test"
        app_test.dart

3. detalhar quais os testes necessários
4. detalahr os passos necessários 

métodos de desenvolvimento
    DDD - Domain Driven Design
    BDD - Behaviour Driven Development
    TDD - Test Driven Development

DDD - Domain Driven Design
  focado no domínio, o que vai aparecer pro layout
  foco na funcionalidade
  como vai resolver o problema depois em código
vantagens  
  fácil de aplicar
  didático
  app pequenos
desvantagens
  difícil crescer
  manutenção
  teste última coisa a se pensar   

TDD - Test Driven Development  
  baseado em teste
  teste primeiro depois cria a feature
  1º- teste
  2º- funcionalidade
  3º- design
vantagens
  garante qualidade
  rapidez crescente
  grandes apps
desvantagens
  dificil de aprender
  projeto inicial não é bom
  lento no inicio
teste evidencia ação

BDD - Behaviour Driven Development
  baseado em comportamento
  foco na funcionalidade, depois teste, por fim design
vantagens
  menos retrabalho
  didático
  apps grandes
destangens
  depende de comunicação
  documentação
  custoso
1º funcionalidade
2º teste
3º design