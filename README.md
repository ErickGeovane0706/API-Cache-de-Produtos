🛍️ API Simples de Cache de Produtos com Memcached
Este projeto é uma demonstração prática de como integrar o Memcached como uma camada de cache rápido em uma aplicação Java 21 usando Spring Boot.

O objetivo é simular um cenário onde a busca por dados é lenta (por exemplo, uma consulta complexa ao banco de dados) e usar o Memcached para armazenar em cache os resultados, melhorando drasticamente o desempenho em chamadas subsequentes.

🎯 Objetivo do Projeto
A aplicação expõe um endpoint REST para buscar informações de produtos. A lógica implementada segue o padrão Cache-Aside:

Cache Miss (Primeira Chamada): Se o produto não estiver no cache, ele é buscado na fonte de dados lenta (um mapa em memória com latência simulada de 2 segundos) e então armazenado no Memcached.

Cache Hit (Chamadas Seguintes): Se o produto estiver no cache, ele é retornado instantaneamente, ignorando a latência da fonte de dados lenta.

🛠️ Tecnologias Utilizadas
Linguagem: Java 21

Framework: Spring Boot 3+ (para criar a API REST)

Servidor de Cache: Memcached

Cliente Java Memcached: Spymemcached (2.12.3)

Build Tool: Maven

⚙️ Pré-requisitos
Para rodar este projeto, você precisará de:

JDK 21 instalado.

Maven instalado.

Um servidor Memcached rodando na porta padrão (11211) na sua máquina local (localhost ou 127.0.0.1).

Como Iniciar o Memcached (Exemplo Linux/Ubuntu)
Bash

# Instalar o Memcached
sudo apt update
sudo apt install memcached

# Iniciar o serviço Memcached na porta padrão (11211)
sudo systemctl start memcached

# Verificar o status (Opcional)
sudo systemctl status memcached 
🚀 Execução do Projeto
Clone o Repositório (Assumindo que o código foi colocado em um repositório).

Compile e Execute a aplicação usando o Maven:

Bash

./mvnw spring-boot:run
A aplicação estará disponível em http://localhost:8080.

🧪 Testes de Demonstração
Use curl ou uma ferramenta como Postman para testar o endpoint e observar a diferença de tempo de resposta entre um Cache Miss e um Cache Hit.

O projeto usa dois IDs de produto: 101 e 102.

1. Teste de Cache Miss (Busca Lenta)
Busque o produto ID 101 pela primeira vez.

Bash

curl http://localhost:8080/api/products/101
Resultado Esperado:

A requisição levará ~2 segundos para completar (devido à simulação de latência).

O console da aplicação deve imprimir: Produto não encontrado no cache. Buscando no BD... (CACHE MISS)

2. Teste de Cache Hit (Busca Rápida)
Repita a busca pelo produto ID 101 imediatamente após a primeira chamada.

Bash

curl http://localhost:8080/api/products/101
Resultado Esperado:

A requisição deve ser quase instantânea (milissegundos).

O console da aplicação deve imprimir: Produto encontrado no Memcached! (CACHE HIT)

📂 Estrutura de Código Chave
MemcachedConfig.java: Configura a conexão e instancia o MemcachedClient como um Bean do Spring.

ProductService.java: Contém a lógica de negócios e a regra de cache.

Usa memcachedClient.get(id) para buscar no cache.

Usa Thread.sleep(2000) para simular a latência do "Banco de Dados".

Usa memcachedClient.set(id, 600, product) para armazenar o resultado por 600 segundos (TTL de 10 minutos).

ProductController.java: Define o endpoint REST /api/products/{id}.

💡 Conclusão
Este projeto demonstra de forma clara a eficácia do uso de uma camada de cache distribuído como o Memcached para reduzir a latência e aliviar a carga na sua fonte de dados primária.
