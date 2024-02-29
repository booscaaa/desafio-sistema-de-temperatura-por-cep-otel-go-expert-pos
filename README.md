# Desafio do Sistema de Temperatura com CEP
## Objetivo
Este projeto em Go tem como objetivo desenvolver um sistema que, ao receber um CEP válido, identifica a cidade correspondente e retorna o clima atual em diferentes unidades de temperatura: Celsius, Fahrenheit e Kelvin.

## Requisitos do Sistema
### Requisitos - Serviço A (responsável pelo input):

- O sistema deve receber um input de 8 dígitos via POST, através do schema:  { "cep": "29902555" }
- O sistema deve validar se o input é valido (contem 8 dígitos) e é uma STRING
- Caso seja válido, será encaminhado para o Serviço B via HTTP
- Caso não seja válido, deve retornar:
  - Código HTTP: 422
    - Mensagem: invalid zipcode

### Requisitos - Serviço B (responsável pela orquestração):

- O sistema deve receber um CEP válido de 8 digitos
- O sistema deve realizar a pesquisa do CEP e encontrar o nome da localização, a partir disso, deverá retornar as temperaturas e formata-lás em: Celsius, Fahrenheit, Kelvin juntamente com o nome da localização.
- O sistema deve responder adequadamente nos seguintes cenários:
- Em caso de sucesso:
  - Código HTTP: 200
    - Response Body: { "city: "São Paulo", "temp_C": 28.5, "temp_F": 28.5, "temp_K": 28.5 }
- Em caso de falha, caso o CEP não seja válido (com formato correto):
  - Código HTTP: 422
    - Mensagem: invalid zipcode
​​- ​Em caso de falha, caso o CEP não seja encontrado:
  - Código HTTP: 404
    - Mensagem: can not find zipcode

## Serviços Utilizados
- APIs Externas:
  - ViaCEP para localização.
  - WeatherAPI para informações climáticas.

## Instruções de Execução
### Ambiente de Desenvolvimento
- Pré-requisitos: Certifique-se de ter o Go, Docker e Docker Compose instalados.
- Clonar o repositório: 
  ```go
  git clone https://github.com/booscaaa/desafio-sistema-de-temperatura-por-cep-otel-go-expert-pos.git
  ```
- Acessar a pasta: 
  ```go
  cd desafio-sistema-de-temperatura-por-cep-otel-go-expert-pos
  ```
- Configure a variavel com a chave da api do seu WeatherAPI:
  ```bash
  cd microservices/service-b
  cp config.example.json config.json
  nano config.json

  {
    "weather_api_key": "<API_KEY>"
  }
  ```
- Retorne a pasta raiz do projeto
  ```bash
  cd ../..
  ```
- Execute:
  ```go
  docker compose up --build -d
  ```
- Service A: 
  - HOST: http://localhost:8080
  - Swagger: http://localhost:8080/swagger/index.html
  - Endpoint para validação: http://localhost:8080/cep/{CEP}

- Service B: 
  - HOST: http://localhost:8081
  - Swagger: http://localhost:8081/swagger/index.html
  - Endpoint para validação: http://localhost:8081/cep/{CEP}

- Zipkin
  - HOST: http://localhost:9411


# Testes Automatizados
Os testes cobrem as funcionalidades chave, incluindo a validação de CEPs e a conversão de temperaturas.
### Conversões de Temperatura
- Fahrenheit: F = C * 1.8 + 32
- Kelvin: K = C + 273

### Execute os testes automatizados com os comandos: 
  - Teste e2e e lógica de conversão de temperatura:
    ```bash
    go test ./microservices/service-a/tests/e2e/... ./microservices/service-b/internal/entity/...
    ```

  - Teste e2e:
    ```bash
    go test ./microservices/service-a/tests/e2e/...
    ```

  - Teste da lógica de conversão de temperatura:
    ```bash
    go test ./microservices/service-b/internal/entity/...
    ```


