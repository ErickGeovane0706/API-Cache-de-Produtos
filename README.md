🛍️ API de Cache de Produtos com Memcached
🔄 Demonstração prática de Cache-Aside com Spring Boot + Java 21

📌 README criado em colaboração com Erivan Barros

🎯 Sobre o Projeto

Este projeto demonstra de forma simples e objetiva como implementar uma camada de cache distribuído com Memcached em uma aplicação Java 21 + Spring Boot 3.

A ideia é simular uma fonte de dados lenta (com uma latência proposital de 2 segundos) e mostrar como o cache reduz drasticamente o tempo de resposta após a primeira consulta.

⚡ Como Funciona (Padrão Cache-Aside)
🟥 Cache Miss (primeira consulta)

O produto é buscado no Memcached → não encontrado

A aplicação busca na fonte lenta (simulada)

O resultado é salvo no cache

A resposta volta para o cliente

🟩 Cache Hit (consultas seguintes)

O produto já está no cache

A resposta é retornada instantaneamente

Nenhuma latência simulada é executada

🧰 Tecnologias Utilizadas

☕ Java 21

🚀 Spring Boot 3+

💾 Memcached (servidor de cache)

🔌 Spymemcached 2.12.3 (cliente Java)

🛠️ Maven

🖥️ Pré-requisitos

Certifique-se de ter instalado:

JDK 21

Maven

Memcached em execução na porta 11211

▶️ Iniciar Memcached (Ubuntu/Linux)
sudo apt update
sudo apt install memcached
sudo systemctl start memcached
sudo systemctl status memcached

🚀 Executando o Projeto
1. Clone o repositório
git clone <url-do-seu-repo>
cd nome-do-repo

2. Execute a aplicação
./mvnw spring-boot:run


A API estará disponível em:
👉 http://localhost:8080

🧪 Testando o Cache
Produto disponível para teste:

IDs: 101 e 102

🔴 1. Teste de Cache Miss

Primeira chamada ao produto:

curl http://localhost:8080/api/products/101


🕑 Resultado: ~2 segundos
📢 Log esperado: "Produto não encontrado no cache. Buscando no BD... (CACHE MISS)"

🟢 2. Teste de Cache Hit

Chamada repetida:

curl http://localhost:8080/api/products/101


⚡ Resultado: instantâneo
📢 Log esperado: "Produto encontrado no Memcached! (CACHE HIT)"

📂 Arquivos Importantes
🔧 MemcachedConfig.java

Configura o cliente Memcached e o torna disponível como Bean do Spring.

💼 ProductService.java

Contém:

Busca no cache: memcachedClient.get(id)

Latência simulada: Thread.sleep(2000)

Inserção no cache com TTL de 10min: memcachedClient.set(id, 600, product)

🌐 ProductController.java

Endpoint REST principal:

GET /api/products/{id}

📌 Conclusão

Este projeto mostra de forma clara como o Memcached melhora o desempenho e reduz carga na fonte de dados principal.
É uma implementação simples, didática e ideal para estudos, prototipação e apresentações técnicas.
