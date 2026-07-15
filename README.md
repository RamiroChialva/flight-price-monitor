[My workflow.json](https://github.com/user-attachments/files/30055313/My.workflow.json)
{
  "name": "My workflow",
  "nodes": [
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1fau0xOXYEyD2sf9LfsDClwrdRELEZiBl3XlZ07G6RME",
          "mode": "list",
          "cachedResultName": "Historial Vuelos",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1fau0xOXYEyD2sf9LfsDClwrdRELEZiBl3XlZ07G6RME/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 809776186,
          "mode": "list",
          "cachedResultName": "Hoja 1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1fau0xOXYEyD2sf9LfsDClwrdRELEZiBl3XlZ07G6RME/edit#gid=809776186"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Fecha": "={{ $now }}",
            "Origen": "BUE",
            "Destino": "LIM",
            "Aerolinea": "={{ $('Buscar vuelos en Google Flights').first().json.best_flights[0].flights[0].airline }}",
            "Precios": "={{ $('Buscar vuelos en Google Flights').first().json.best_flights[0].price }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "Fecha",
              "displayName": "Fecha",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Origen",
              "displayName": "Origen",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Destino",
              "displayName": "Destino",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Precios",
              "displayName": "Precios",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Aerolinea",
              "displayName": "Aerolinea",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        768,
        -16
      ],
      "id": "d72adc45-9782-4211-bd7b-0a6802785c63",
      "name": "Sheet con Data",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "thO3Kv6uPAYxSnEc",
          "name": "Google Sheets OAuth2 API"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "ramiro.chialva@alumnos.udemm.edu.ar",
        "subject": "Encontré un precio bajo a LIM!",
        "message": "=¡Buenas noticias! Encontré un vuelo que se ajusta a tu presupuesto.\n\n✈️ Ruta: AEP -> LIM\n💵 Precio actual: USD {{ $('Buscar vuelos en Google Flights').item.json.best_flights[0].price }}\n🏢 Aerolínea: {{ $('Buscar vuelos en Google Flights').item.json.best_flights[0].flights[0].airline }}\n⏱️ Duración total: {{ Math.round($('Buscar vuelos en Google Flights').item.json.best_flights[0].total_duration / 60) }} horas.\n\nhttps://www.google.com/travel/flights?q=Flights%20from%20AEP%20to%20LIM%20on%202026-10-15%20through%202026-10-22",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        944,
        0
      ],
      "id": "2711ddf1-c35e-41c8-a279-8dbd4de60b21",
      "name": "Envío de Mensaje vía Mail",
      "webhookId": "1a54a58b-04d3-43e6-a442-da15a47e4aec",
      "credentials": {
        "gmailOAuth2": {
          "id": "9Cac5671H6CwbWac",
          "name": "Gmail OAuth2 API"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "ed18705d-c75d-47fa-8582-b8c4d76dd2a3",
              "leftValue": "={{ $json.best_flights[0].price }}",
              "rightValue": 500,
              "operator": {
                "type": "number",
                "operation": "lte"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.filter",
      "typeVersion": 2.3,
      "position": [
        560,
        0
      ],
      "id": "c259acc8-97fc-460b-9aa5-f9b918ba9b7d",
      "name": "Filtrar Precio <500"
    },
    {
      "parameters": {
        "url": "=https://serpapi.com/search.json",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "serpApi",
        "sendQuery": true,
        "queryParameters": {
          "parameters": [
            {
              "name": "engine",
              "value": "google_flights"
            },
            {
              "name": "departure_id",
              "value": "AEP"
            },
            {
              "name": "arrival_id",
              "value": "LIM"
            },
            {
              "name": "outbound_date",
              "value": "2026-10-15"
            },
            {
              "name": "return_date",
              "value": "2026-10-22"
            },
            {
              "name": "currency",
              "value": "USD"
            },
            {
              "name": "hl",
              "value": "es"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.4,
      "position": [
        208,
        0
      ],
      "id": "771297fc-1515-4093-a369-282580caf6e5",
      "name": "Buscar vuelos en Google Flights",
      "credentials": {
        "serpApi": {
          "id": "F16oxo2qs4K24Sr7",
          "name": "SerpAPI account 2"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "259d2f00-ffc8-4576-8dd4-d51c86b9e0b7",
              "leftValue": "={{ $json.best_flights?.length > 0 }}",
              "rightValue": "",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        352,
        0
      ],
      "id": "eac77d9b-5bfe-4cea-bb63-a55435589e18",
      "name": "If"
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "triggerAtHour": 9
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.3,
      "position": [
        0,
        0
      ],
      "id": "50bcc83a-8b25-45b0-a3ca-b9af6d118356",
      "name": "Schedule Trigger"
    }
  ],
  "pinData": {},
  "connections": {
    "Sheet con Data": {
      "main": [
        [
          {
            "node": "Envío de Mensaje vía Mail",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Buscar vuelos en Google Flights": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "Filtrar Precio <500",
            "type": "main",
            "index": 0
          }
        ],
        []
      ]
    },
    "Schedule Trigger": {
      "main": [
        [
          {
            "node": "Buscar vuelos en Google Flights",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Filtrar Precio <500": {
      "main": [
        [
          {
            "node": "Sheet con Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "availableInMCP": false
  },
  "versionId": "d6106904-57ff-406b-a5b3-e3b57df15a37",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "8e274cbfc9ab5df2b33c71cccde3673291a1a5b5b28a1d97cd1e5b13de725504"
  },
  "nodeGroups": [],
  "id": "Hz0Qc0kILT9wBPaI",
  "tags": []
}
