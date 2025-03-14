# Documentação das Collections no MongoDB

Este documento descreve a estrutura e as validações (via JSON Schema) das collections utilizadas no projeto de Reflorestamento. As collections configuradas são: species, plots e events. Além disso, são criados índices para melhorar o desempenho das consultas.

## 1. Coleção `species`
A collection `species` armazena informações sobre as espécies de plantas, contendo dados essenciais para identificar e caracterizar cada espécie.

### Estrutura e Validações

- scientific_name (`string`):<br>
Nome científico da espécie.<br>
Obrigatório.

- common_name (`string`):<br>
Nome popular da espécie.<br>
Obrigatório.

- growth_time (`object`):<br>
Informações sobre o tempo de crescimento da espécie.<br>
Obrigatório.

  - unit_of_measure (`string`): Unidade de medida (ex.: meses, anos).
  - value (`int`): Valor numérico do tempo de crescimento.

- water_requirement (`double`):<br>
Quantidade de água necessária para a espécie.<br>
Obrigatório.

- unit_of_measurement_for_planting (`string`):<br>
Unidade de medida para o plantio.<br>
Obrigatório.

- unit_of_measurement_for_harvest (`string`):<br>
Unidade de medida para a colheita.<br>
Obrigatório.

- unit_of_measurement_for_loss (`string`):<br>
Unidade de medida para perdas.<br>
Obrigatório.

### Exemplo de Documento
```json
{
  "scientific_name": "Quercus robur",
  "common_name": "Carvalho",
  "growth_time": {
    "unit_of_measure": "years",
    "value": 50
  },
  "water_requirement": 3.5,
  "unit_of_measurement_for_planting": "tree",
  "unit_of_measurement_for_harvest": "wood volume",
  "unit_of_measurement_for_loss": "tree"
}
```

## 2. Coleção `plots`
A collection `plots` armazena informações sobre as áreas destinadas ao reflorestamento, permitindo a localização geográfica e caracterização do espaço.

### Estrutura e Validações
- area (`object`):<br>
Detalhes da área do plot.<br>
Obrigatório.
  - unit_of_measure (`string`): Unidade de medida (ex.: m², km², ha).<br>
  - value (`double`): Valor numérico da área.

- coordinates (`array`):<br>
Array contendo duas posições (longitude, latitude) em formato double.<br>
Obrigatório.<br>
(mínimo 2 itens e máximo 2 itens)

- city (`string`):<br>
Cidade onde o plot está localizado.<br>
Obrigatório.

- state (`string`):<br>
Estado onde o plot está localizado.<br>
Obrigatório.

- country (`string`):<br>
País onde o plot está localizado.<br>
Obrigatório.

### Exemplo de Documento
```json
{
  "area": {
    "unit_of_measure": "ha",
    "value": 15.5
  },
  "coordinates": [-46.633309, -23.550520],
  "city": "São Paulo",
  "state": "SP",
  "country": "Brasil"
}
```

## 3. Coleção `events`
A collection `events` registra eventos relacionados aos processos de plantio, manutenção e colheita. Ela utiliza o operador `oneOf` para aceitar diferentes estruturas de documento, dependendo do tipo de evento.

### Validação Comum: Campo `climate`
Todos os eventos possuem um campo climate que segue o seguinte schema:

- day (`date`):<br>
Data do evento no formato ISODate.<br>
Obrigatório.

- temperature (`object`):<br>
Informações de temperatura.<br>
Obrigatório.
  - min (`double`): Temperatura mínima.<br>
  - med (`double`): Temperatura média.<br>
  - max (`double`): Temperatura máxima.

- humidity (`double`):<br>
Umidade.<br>
Obrigatório.

- wind (`double`):<br>
Velocidade do vento.<br>
Obrigatório.

- rain (`double`):<br>
Quantidade de chuva.<br>
Obrigatório.

- rain_probability (`double`):<br>
Probabilidade de chuva.<br>
Obrigatório.

### Tipos de Eventos

a) Evento de Plantio (`planting`)
- species_id (`objectId`):<br>
Referência à espécie plantada.<br>
Obrigatório.

- plot_id (`objectId`):<br>
Referência ao plot onde ocorreu o plantio.<br>
Obrigatório.

- type (`string`):<br>
Valor fixo "planting".<br>
Obrigatório.

- climate:
Conforme definido na validação comum.

- planted_area (`object`):<br>
Área plantada.<br>
Obrigatório.<br>
  - unit_of_measure (`string`): Unidade de medida (ex.: m², km², ha).<br>
  - value (`double`): Valor numérico da área plantada.

- planted_quantity (`double`):<br>
Quantidade de plantas plantadas.<br>
Obrigatório.

- observations (`string`):<br>
Observações sobre o evento.<br>
Obrigatório.

b) Evento de Manutenção (`maintenance`)
- planting_id (`objectId`):<br>
Referência ao evento de plantio associado.<br>
Obrigatório.

- species_id (`objectId`):<br>
Referência à espécie.<br>
Obrigatório.

- plot_id (`objectId`):<br>
Referência ao plot.<br>
Obrigatório.

- type (`string`):<br>
Valor fixo "maintenance".<br>
Obrigatório.

- climate:
Conforme definido na validação comum.

- dead_plants (`int`):<br>
Quantidade de plantas mortas.<br>
Obrigatório.

- fertilizer (`double`):<br>
Quantidade de fertilizante aplicada.<br>
Obrigatório.

- pesticide (`double`):<br>
Quantidade de pesticida aplicada.<br>
Obrigatório.

- observations (`string`):<br>
Observações sobre o evento.<br>
Obrigatório.

c) Evento de Colheita (`harvest`)
- planting_id (`objectId`):<br>
Referência ao evento de plantio associado.<br>
Obrigatório.

- species_id (`objectId`):<br>
Referência à espécie.<br>
Obrigatório.

- plot_id (`objectId`):<br>
Referência ao plot.<br>
Obrigatório.

- type (`string`):<br>
Valor fixo "harvest".<br>
Obrigatório.

- climate:<br>
Conforme definido na validação comum.

- price (`double`):<br>
Preço associado à colheita.<br>
Obrigatório.

- harvested_quantity (`double`):<br>
Quantidade colhida.<br>
Obrigatório.

- losses (`double`):<br>
Quantidade de perdas durante a colheita.<br>
Obrigatório.

- observations (`string`):<br>
Observações sobre o evento.
Obrigatório.

### Exemplo de Documento para Evento de Plantio
```json
{
  "species_id": "60f7a7f9b4d1c70d2c123456",
  "plot_id": "60f7a7f9b4d1c70d2c654321",
  "type": "planting",
  "climate": {
    "day": "2025-04-01T00:00:00Z",
    "temperature": {
      "min": 15.0,
      "med": 20.0,
      "max": 25.0
    },
    "humidity": 80.0,
    "wind": 5.0,
    "rain": 10.0,
    "rain_probability": 70.0
  },
  "planted_area": {
    "unit_of_measure": "ha",
    "value": 1.2
  },
  "planted_quantity": 100.0,
  "observations": "Plantio realizado com sucesso."
}
```

## 4. Índices Criados
Para otimizar as consultas, os seguintes índices são criados:

- Coleção `species`:<br>
Índice único no campo `scientific_name`.

- Coleção `plots`:<br>
Índice no campo `coordinates` (ideal para buscas por localização geográfica).

- Coleção `events`:<br>
Índice no campo `climate.day` para facilitar consultas baseadas na data dos eventos.

