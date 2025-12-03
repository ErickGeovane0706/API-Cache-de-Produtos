🛍️ API de Cache de Produtos com Memcached (Java 21 + Spring Boot)

Uma demonstração prática e objetiva de como implementar cache distribuído de alta performance utilizando Memcached em uma aplicação Java moderna com Spring Boot 3.

O projeto simula um cenário real onde a fonte de dados é lenta (como um banco de dados pesado), e por isso utiliza o padrão Cache-Aside para acelerar respostas e reduzir carga no backend.

🎯 Objetivo do Projeto

Demonstrar como:

Armazenar dados de forma temporária no Memcached

Reduzir drasticamente o tempo de resposta da API

Aplicar o padrão Cache-Aside em Java + Spring

Integrar o cliente Spymemcached ao Spring Boot

O objetivo final é exibir claramente a diferença entre:

🟥 Cache Miss → consulta lenta (~2 segundos)
🟩 Cache Hit → retorno instantâneo (memória RAM)

⚙️ Arquitetura e Tecnologias
Categoria	Tecnologia
Linguagem	Java 21
Framework	Spring Boot 3+
Cache	Memcached
Cliente Memcached	Spymemcached 2.12.3
Build	Maven
Exposição	API REST
📦 Como Funciona (Padrão Cache-Aside)
🔴 1. Cache Miss

A API recebe a requisição /api/products/{id}

Verifica no Memcached

Caso não exista → busca na “fonte lenta” (simulada com Thread.sleep(2000))

Salva o resultado no cache (TTL = 600s)

Retorna ao cliente

🟢 2. Cache Hit

A API recebe a requisição

Encontra o dado no Memcached

Devolve em milissegundos

🧰 Pré-requisitos

JDK 21+

Maven 3+

Memcached ativo na porta padrão 11211

🖥️ Iniciando o Memcached (Linux/Ubuntu)
sudo apt update
sudo apt install memcached
sudo systemctl start memcached
sudo systemctl status memcached

🚀 Execução do Projeto

Compile e execute:

./mvnw spring-boot:run


API disponível em:

http://localhost:8080

🧪 Como Testar (cURL ou Postman)
1️⃣ Cache Miss (primeira requisição)
curl http://localhost:8080/api/products/101


Resposta ~2 segundos

Log esperado:

Produto não encontrado no cache. Buscando no BD... (CACHE MISS)

2️⃣ Cache Hit (requisição subsequente)
curl http://localhost:8080/api/products/101


Resposta instantânea

Log esperado:

Produto encontrado no Memcached! (CACHE HIT)

📂 Arquivos Importantes
MemcachedConfig.java

Configura o cliente

Conecta ao servidor de cache

ProductService.java

Lógica principal do cache

Simula fonte de dados lenta

TTL de 10 minutos

ProductController.java

Endpoint /api/products/{id}

Chama o service para buscar produto

📁 Estrutura Geral do Projeto
src/
 └── main/java
     └── com.example.cache
          ├── config/MemcachedConfig.java
          ├── controller/ProductController.java
          ├── service/ProductService.java
          └── model/Product.java

📘 Exemplo de Produto Retornado
{
  "id": 101,
  "name": "Notebook Gamer",
  "price": 4999.90
}

🤝 Colaboração

Este projeto foi desenvolvido com colaboração especial de:

Erivan Barros
🔗 GitHub: https://github.com/Erivanb

💡 Conclusão

Este projeto demonstra, de forma clara e prática, como o uso de um sistema de cache como Memcached:

✔ melhora o desempenho
✔ reduz a carga no backend
✔ acelera o tempo de resposta
✔ escala facilmente

Ideal para quem está aprendendo arquitetura de sistemas, otimização, desempenho e boas práticas com Spring Boot.
