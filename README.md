<p align="center" width="100%">
    <img width="" src="/assets/BTG-Logo.png"> 
</p>

<h3 align="center">
  Desafio Técnico Backend BTG Pactual
</h3>

<p align="center">
  <img alt="Language: Java" src="https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white">
  <img alt="Framework: Spring" src="https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white">
  <img alt="Docker" src="https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white">
  <img alt="Database: MongoDB" src="https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white">
  <img alt="Queue: RabbitMQ" src="https://img.shields.io/badge/Rabbitmq-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white">
</p>

---

## Sobre o desafio
O objetivo deste projeto é implementar um **microserviço de pedidos** que consome mensagens de uma fila RabbitMQ, processa e persiste os dados em um banco **MongoDB**, expondo uma **API REST** para consulta de:
- Lista de pedidos por cliente
- Quantidade total de pedidos por cliente
- Valor total dos pedidos por cliente

A API é paginada e otimizada com **índices para consultas por cliente**.

---

## Arquitetura da solução

### Fluxo de processamento
1. **Producer** envia mensagem JSON para fila RabbitMQ.
2. **Consumer** (Spring AMQP) processa a mensagem e grava o pedido no MongoDB.
3. **MongoDB** armazena as informações com **índice no campo `customerId`** para consultas rápidas.
4. **API REST** expõe endpoints para consultas agregadas e paginadas.


---

## 🛠️ Tecnologias utilizadas

| Categoria          | Tecnologia / Versão                                 |
|--------------------|-----------------------------------------------------|
| Linguagem          | Java 24                                             |
| Framework          | Spring Boot 3.5.4, Spring Data MongoDB, Spring AMQP |
| Banco de Dados     | MongoDB 7.x                                         |
| Mensageria         | RabbitMQ                                            |
| Contêinerização    | Docker, Docker Compose                              |
| Build              | Maven                                               |
| Testes (a implementar) | JUnit 5, Spring Boot Test                           |

---

## 📂 Estrutura da base de dados

Coleção: `tb_orders`

```java
@Document(collection = "tb_orders")
public class OrderEntity {

    @MongoId
    private Long orderId;

    @Indexed(name = "customer_id_index")
    private Long customerId;

    @Field(targetType = FieldType.DECIMAL128)
    private BigDecimal total;

    private List<OrderItem> items;
}

public class OrderItem {

    private String product;
    private Integer quantity;

    @Field(targetType = FieldType.DECIMAL128)
    private BigDecimal price;
    ...
}
```
