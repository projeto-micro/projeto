# Product API

**Responsável** -> Vitor Fengler

**Repositórios**: https://github.com/projeto-micro/product.git e https://github.com/projeto-micro/product-service.git

Serviço responsável pelo catálogo de produtos da loja.

- **Porta interna:** `8082`
- **Base path via gateway:** `/products`
- **Banco:** PostgreSQL — tabela `products`

## Modelo

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `id` | `string (UUID)` | gerado | Identificador único |
| `name` | `string` | sim | Nome do produto |
| `price` | `number` | sim | Preço (≥ 0) |
| `unit` | `string` | sim | Unidade de medida (ex: `kg`, `un`) |

## Endpoints

### `POST /products` — Criar produto

**Body:**
```json
{
  "name": "Arroz",
  "price": 10.50,
  "unit": "kg"
}
```

**Resposta:** `201 Created` com header `Location: /products/{id}`

---

### `GET /products` — Listar produtos

**Query params:**

| Param | Obrigatório | Descrição |
|---|---|---|
| `name` | não | Filtro por nome (case-insensitive, parcial) |

**Resposta:** `200 OK`
```json
[
  {
    "id": "uuid",
    "name": "Arroz",
    "price": 10.50,
    "unit": "kg"
  }
]
```

---

### `GET /products/{id}` — Buscar por ID

**Resposta:** `200 OK` ou `404 Not Found`

```json
{
  "id": "uuid",
  "name": "Arroz",
  "price": 10.50,
  "unit": "kg"
}
```

---

### `PUT /products/{id}` — Atualizar produto

**Body:**
```json
{
  "name": "Arroz Integral",
  "price": 12.00,
  "unit": "kg"
}
```

**Resposta:** `200 OK` ou `404 Not Found`

---

### `DELETE /products/{id}` — Deletar produto

**Resposta:** `204 No Content` ou `404 Not Found`

---

### `GET /products/health-check` — Health check

**Resposta:** `200 OK`

## Banco de dados

Migration `V1__create_products.sql`:

```sql
CREATE TABLE products (
    id VARCHAR(36) PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    unit VARCHAR(50) NOT NULL
);
```

## Validações

- `name` não pode ser vazio
- `price` não pode ser nulo nem negativo
- `unit` não pode ser vazio
