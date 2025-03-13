# Documentação da Estrutura de Dados

## Visão Geral

Esta documentação descreve a estrutura de dados utilizada para armazenar informações sobre espécies, terrenos e eventos em um sistema de gerenciamento de cultivos.

## Estrutura de Dados

### 1. `species` (Espécies)
Armazena informações sobre as espécies cultivadas.

**Campos:**
- `scientific_name` (string): Nome científico da espécie.
- `common_name` (string): Nome comum da espécie.
- `growth_time` (objeto): Tempo de crescimento da espécie.
  - `unit_of_measure` (string): Unidade de medida (dias, meses, anos).
  - `value` (int): Valor numérico do tempo de crescimento.
- `water_requirement` (objeto): Requisitos de água para a espécie.
  - `unit_of_measure` (string): Unidade de medida (litros, m³, mm³).
  - `value` (float): Quantidade de água necessária.

### 2. `plots` (Terrenos)
Armazena informações sobre as áreas de cultivo.

**Campos:**
- `area` (objeto): Dimensão do terreno.
  - `unit_of_measure` (string): Unidade de medida (m², km², ha).
  - `value` (float): Tamanho da área.
- `coordinates` (array de floats): Coordenadas geográficas (latitude, longitude).
- `city` (string): Cidade onde o terreno está localizado.
- `state` (string): Estado onde o terreno está localizado.
- `country` (string): País onde o terreno está localizado.

### 3. `events` (Eventos)
Lista de eventos relacionados ao plantio, manutenção e colheita.

Cada evento contém informações sobre clima, espécie envolvida e dados específicos do tipo de evento.

#### Estrutura Comum dos Eventos:
- `species_id` (ObjectId): Referência à espécie.
- `plot_id` (ObjectId): Referência ao terreno.
- `event_date` (string): Data do evento no formato `YYYY-MM-DD`.
- `climate` (objeto): Dados climáticos no dia do evento.
  - `temperature` (objeto): Informações de temperatura.
    - `min` (float): Temperatura mínima.
    - `med` (float): Temperatura média.
    - `max` (float): Temperatura máxima.
    - `unit_of_measure` (string): Unidade de medida (°C, °F).
  - `humidity` (objeto): Umidade do ar.
    - `unit_of_measure` (string): Unidade de medida (%, g/m³).
    - `value` (float): Valor da umidade.
  - `wind` (objeto): Velocidade do vento.
    - `unit_of_measure` (string): Unidade de medida (km/h, m/s).
    - `value` (float): Velocidade.
  - `rain` (objeto): Dados de chuva.
    - `min` (float): Volume mínimo de chuva.
    - `med` (float): Volume médio de chuva.
    - `max` (float): Volume máximo de chuva.
    - `unit_of_measure` (string): Unidade de medida (mm, in).
  - `rain_probability` (objeto): Probabilidade de chuva.
    - `unit_of_measure` (string): Unidade de medida (%).
    - `value` (float): Probabilidade.

#### Tipos de Eventos

##### a) `planting` (Plantio)
- `planted_area` (objeto): Área plantada.
  - `unit_of_measure` (string): Unidade de medida (m², km², ha).
  - `value` (float): Tamanho da área plantada.
- `planted_quantity` (objeto): Quantidade de plantas.
  - `unit_of_measure` (string): Unidade de medida (unidades, kg, g).
  - `value` (float): Quantidade plantada.
- `irrigation` (array de objetos): Histórico de irrigação.
  - `unit_of_measure` (string): Unidade de medida (litros, mm).
  - `quantity` (float): Quantidade utilizada.
- `observations` (string): Observações gerais.

##### b) `maintenance` (Manutenção)
- `planting_id` (ObjectId): Referência ao plantio relacionado.
- `dead_plants` (objeto): Plantas mortas.
  - `unit_of_measure` (string): Unidade de medida (unidades, kg, g).
  - `value` (int): Quantidade.
- `average_growth` (objeto): Crescimento médio.
  - `unit_of_measure` (string): Unidade de medida (cm, m).
  - `value` (float): Crescimento registrado.
- `fertilizer` (array de objetos): Aplicativos de fertilizantes.
  - `unit_of_measure` (string): Unidade de medida (kg, g).
  - `quantity` (float): Quantidade aplicada.
- `observations` (string): Observações gerais.

##### c) `harvest` (Colheita)
- `planting_id` (ObjectId): Referência ao plantio relacionado.
- `price` (float): Preço da colheita.
- `harvested_quantity` (array de objetos): Quantidade colhida.
  - `unit_of_measure` (string): Unidade de medida (unidades, kg, g).
  - `quantity` (float): Quantidade colhida.
- `losses` (array de objetos): Perdas na colheita.
  - `unit_of_measure` (string): Unidade de medida (unidades, kg, g).
  - `quantity` (float): Quantidade perdida.


Esta estrutura fornece uma base sólida para o gerenciamento de cultivos, abrangendo desde o cadastro de espécies e terrenos até o monitoramento de eventos.

---

# Json Schema

```json
{
  "$jsonSchema": {
    "bsonType": "object",
    "required": ["species", "plots", "events"],
    "properties": {
      "species": {
        "bsonType": "object",
        "required": ["scientific_name", "common_name", "growth_time", "water_requirement"],
        "properties": {
          "scientific_name": { "bsonType": "string" },
          "common_name": { "bsonType": "string" },
          "growth_time": {
            "bsonType": "object",
            "properties": {
              "unit_of_measure": { "bsonType": "string" },
              "value": { "bsonType": "int" }
            }
          },
          "water_requirement": {
            "bsonType": "object",
            "properties": {
              "unit_of_measure": { "bsonType": "string" },
              "value": { "bsonType": "double" }
            }
          }
        }
      },
      "plots": {
        "bsonType": "object",
        "required": ["area", "coordinates", "city", "state", "country"],
        "properties": {
          "area": {
            "bsonType": "object",
            "properties": {
              "unit_of_measure": { "bsonType": "string" },
              "value": { "bsonType": "double" }
            }
          },
          "coordinates": {
            "bsonType": "array",
            "items": { "bsonType": "double" },
            "minItems": 2,
            "maxItems": 2
          },
          "city": { "bsonType": "string" },
          "state": { "bsonType": "string" },
          "country": { "bsonType": "string" }
        }
      },
      "events": {
        "bsonType": "array",
        "items": {
          "bsonType": "object",
          "required": ["species_id", "plot_id", "climate", "type"],
          "properties": {
            "species_id": { "bsonType": "objectId" },
            "plot_id": { "bsonType": "objectId" },
            "climate": { "bsonType": "object" },
            "type": { "bsonType": "string", "enum": ["planting", "maintenance", "harvest"] },
            "planted_area": {
              "bsonType": "object",
              "properties": {
                "unit_of_measure": { "bsonType": "string" },
                "value": { "bsonType": "double" }
              }
            }
          }
        }
      }
    }
  }
}
