API de Cache de Produtos com Memcached

Java 21 • Spring Boot 3 • Cache-Aside Pattern

Este projeto demonstra a integração do Memcached como camada de cache distribuído em uma aplicação desenvolvida com Java 21 e Spring Boot 3, aplicando o padrão arquitetural Cache-Aside para melhorar o desempenho em operações de leitura.

1. Objetivo

O propósito deste projeto é apresentar, de forma clara e funcional, como:

implementar uma camada de cache de alta performance;

reduzir a latência em consultas repetidas;

integrar o cliente Spymemcached com o Spring Boot;

demonstrar o fluxo completo de cache miss e cache hit de forma didática.

A aplicação expõe um endpoint REST que busca informações de produtos. Caso o item não esteja em cache, uma fonte de dados lenta é simulada, permitindo observar os ganhos de desempenho obtidos com o uso do Memcached.

2. Tecnologias Utilizadas

Java 21

Spring Boot 3+

Memcached (servidor de cache)

Spymemcached 2.12.3 (cliente Java)

Maven

3. Funcionamento do Sistema
3.1 Cache Miss

Quando o produto solicitado não está presente no cache:

O serviço consulta o Memcached.

Como não há entrada correspondente, realiza a busca na "fonte lenta", simulada por um atraso de 2 segundos.

O resultado é armazenado no cache com tempo de expiração de 600 segundos.

A resposta é retornada ao cliente.

3.2 Cache Hit

Em chamadas subsequentes ao mesmo produto:

O serviço encontra o item no Memcached.

O valor é retornado imediatamente, sem latência perceptível.

Esse comportamento permite comparar claramente o desempenho entre uma consulta fria (cache miss) e uma consulta quente (cache hit).

4. Pré-requisitos

Antes de executar a aplicação, é necessário:

JDK 21 instalado

Maven instalado

Servidor Memcached em execução na porta padrão (11211)

Iniciando o Memcached (Ubuntu)
sudo apt update
sudo apt install memcached
sudo systemctl start memcached
sudo systemctl status memcached

5. Como Executar o Projeto

Compile e execute:

./mvnw spring-boot:run


A API estará disponível em:

http://localhost:8080

6. Testando a API

O projeto disponibiliza dois produtos para teste: 101 e 102.

6.1 Primeira requisição (cache miss)
curl http://localhost:8080/api/products/101


Tempo estimado: ~2 segundos

Log esperado:

Produto não encontrado no cache. Buscando no BD... (CACHE MISS)

6.2 Requisição subsequente (cache hit)
curl http://localhost:8080/api/products/101


Tempo estimado: milissegundos

Log esperado:

Produto encontrado no Memcached! (CACHE HIT)

7. Estrutura do Projeto
src/main/java/com/example/cache
 ├── config
 │    └── MemcachedConfig.java
 ├── controller
 │    └── ProductController.java
 ├── service
 │    └── ProductService.java
 └── model
      └── Product.java

8. Arquivos Principais
MemcachedConfig.java

Configura o cliente do Memcached e define o bean utilizado pelo serviço.

ProductService.java

Implementa a lógica do padrão Cache-Aside, realiza consultas simuladas e gerencia o TTL (600s).

ProductController.java

Fornece o endpoint /api/products/{id} utilizado para testar o fluxo de cache.

9. Dados Exemplo
{
  "id": 101,
  "name": "Notebook Gamer",
  "price": 4999.90
}

10. Créditos e Colaboração

Projeto desenvolvido com colaboração de:

Erivan Barros
GitHub: Erivanb

11. Conclusão

Este projeto evidencia, de forma objetiva, como o uso de um cache distribuído:

reduz a latência de consultas;

diminui a carga sobre fontes de dados lentas;

melhora o desempenho geral da aplicação;

simplifica a escalabilidade horizontal.

É uma implementação didática e sólida do padrão Cache-Aside aplicada ao ecossistema Java + Spring Boot.
