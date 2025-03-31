# Documentação do Modelo de Banco de Dados MongoDB

## Introdução
Este documento descreve a estrutura do banco de dados MongoDB utilizado no projeto de monitoramento de reflorestamento. A base de dados possui três coleções principais: `species_collection`, `plots_collection` e `yield_collection`. Cada uma dessas coleções possui um esquema de validação definido para garantir a integridade dos dados.

link da base: https://www.kaggle.com/datasets/akshatgupta7/crop-yield-in-indian-states-dataset

## Coleções e Estruturas

![mongo (1)](https://github.com/user-attachments/assets/22c6110f-7b4d-4687-a45a-0fffcb6efd97)

### 1. Coleção `species_collection`
A coleção `species_collection` armazena informações sobre espécies vegetais presentes na área de reflorestamento.

#### Estrutura:
```json
{
    "scientific_name": "string",  // Nome científico da espécie
    "common_name": "string"       // Nome popular da espécie
}
```

#### Restrições:
- `scientific_name` e `common_name` são campos obrigatórios.
- Um índice único foi criado para `scientific_name` para evitar duplicatas.

### 2. Coleção `plots_collection`
A coleção `plots_collection` armazena informações sobre áreas de reflorestamento.

#### Estrutura:
```json
{
    "area": "object",    // Detalhes da área da propriedade
    "state": "string",   // Estado onde a área está localizada
    "country": "string"  // País onde a área está localizada
}
```

#### Restrições:
- Os campos `area`, `state` e `country` são obrigatórios.
- Um índice foi criado para o campo `coordinates`.

### 3. Coleção `yield_collection`
A coleção `yield_collection` armazena informações sobre o rendimento agrícola nas áreas reflorestadas.

#### Estrutura:
```json
{
    "crop": "string",                // Nome da cultura cultivada
    "crop_year": "int",           // Ano da safra
    "season": "string",              // Estacao do ano (Spring, Summer, Autumn, Winter)
    "state": "string",               // Estado
    "area": "double",                // Área total cultivada (em hectares)
    "production": "number",             // Quantidade de produção
    "annual_rainfall": "double",     // Precipitação anual (mm)
    "fertilizer": "double",          // Quantidade de fertilizante usada (kg)
    "pesticide": "double",           // Quantidade de pesticida usada (kg)
    "yield": "double"                 // Rendimento calculado (produção por unidade de área)
}
```

#### Restrições:
- Todos os campos são obrigatórios.
- O campo `season` só pode conter um dos valores: `Spring`, `Summer`, `Autumn` ou `Winter`.
- Um índice foi criado para o campo `production`.

## Índices Criados
Os seguintes índices foram definidos para otimizar a consulta dos dados:
- `species_collection`: índice único no campo `scientific_name`.
- `plots_collection`: índice no campo `area`.
- `yield_collection`: índice no campo `production`.
