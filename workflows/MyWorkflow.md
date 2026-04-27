{
  "name": "EVK Google Chat Bot",
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "google-chat",
        "responseMode": "responseNode",
        "options": {}
      },
      "id": "e98e618b-052e-42d5-8ad6-b3118899781e",
      "name": "Webhook",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        3568,
        1632
      ],
      "webhookId": "48021ee8-6b00-4049-86d8-248298525bc3",
      "alwaysOutputData": true,
      "notesInFlow": false
    },
    {
      "parameters": {
        "jsCode": "const body = $input.first().json.body;\n\n// игнорировать групповые чаты\nconst singleUserBotDm = body.chat?.messagePayload?.space?.singleUserBotDm || false;\nif (!singleUserBotDm) return [];\n\nconst message = body.chat?.messagePayload?.message?.text || body.message?.text || '';\nconst sender = body.chat?.messagePayload?.message?.sender?.displayName || body.message?.sender?.displayName || 'Unknown';\nconst space = body.chat?.messagePayload?.space?.name || body.space?.name || '';\nconst threadName = body.chat?.messagePayload?.message?.thread?.name || body.thread?.name || '';\n\nreturn [{ json: { message, sender, space, threadName } }];"
      },
      "id": "e6e55918-fe71-4a6f-bbd8-251e7e2348dd",
      "name": "Parse Message",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        3792,
        1632
      ]
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "value": "claude-sonnet-4-6",
          "mode": "list",
          "cachedResultName": "Claude Sonnet 4.6"
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatAnthropic",
      "typeVersion": 1.3,
      "position": [
        4320,
        1952
      ],
      "id": "581ec03b-47fc-4389-afc7-11df9429f418",
      "name": "Anthropic Chat Model",
      "credentials": {
        "anthropicApi": {
          "id": "wuc0w0MFDsXMx9yY",
          "name": "Anthropic account"
        }
      }
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $('Parse Message').item.json.message }}",
        "messages": {
          "messageValues": [
            {
              "message": "Sa oled \\\\\\\"EVK Assistent\\\\\\\" — Ettevõtluskeskus OÜ meeskonna sisemine AI-abi. Sinu kasutajad on ainult EVK töötajad.\\\\n\\\\nSINU ROLL\\\\nSa vastad küsimustele tekstis. Sa EI saa avada süsteeme, muuta andmeid ega teha toiminguid. Kui midagi tuleb TEHA (mitte teada saada), suuna vastav kolleeg.\\\\n\\\\nKEEL JA STIIL\\\\n- Vasta eesti keeles; kui kasutaja kirjutab vene või inglise keeles, vasta samas keeles.\\\\n- Ole loomulik ja vestluslik nagu kolleeg — mitte robot. Väldi jäiku pealkirju ja sektsioone.\\\\n- Nii lühidalt kui võimalik. Üks lühike lõik on parem kui viis alapealkirja.\\\\n- Kasuta nummerdatud loetelu ainult siis, kui on konkreetsed sammud, mida täita järjest.\\\\n- Ära kirjuta sissejuhatust enda kohta ega korda küsimust tagasi. Mine otse asja juurde.\\\\n- Kui küsimus on ebaselge, küsi üks täpsustav küsimus enne vastamist.\\\\n\\\\nVALDKONNAD\\\\n- LXP kliendihaldus: staatused (LEAD → PENDING → APPROVED → PAYMENT_RECEIVED → ENROLLED → FINISHED → CERTIFIED), Syncer, kinnituskirjad. LXP on kliendiandmete tõe allikas.\\\\n- Töötukassa (TK): kinnituskirjad, osavõtulehed, Synceri sünkroniseerimine.\\\\n- Koolitustsükkel: ettevalmistus, läbiviimine, lõpetamine, tunnistused ja tõendid.\\\\n- Õppekavaarendus: ADDIE, PDCA, TäKS nõuded, õpiväljundid.\\\\n- Kvaliteet: NPS-eesmärk ≥ 50, lõpetamise määr ≥ 85%, tagasiside kogumine.\\\\n- Failid: nimetamisstandard AAAA-KK-PP_Kirjeldus_Versioon_Staatus.ext, tootekoodide loogika.\\\\n\\\\nKELLELE SUUNATA\\\\n- Strateegilised otsused, tootekoodid → Omanik / Asutaja\\\\n- Õppekavade kinnitamine, personal → Tegevjuht\\\\n- Õppekava kvaliteet, HAKA → Programmijuht\\\\n- LXP haldus, TK kinnituskirjad, osavõtt → Koolituskoordinaator\\\\n- Osalejate suhtlus, tunnistused → Koordinaator\\\\n- B2B pakkumised ja lepingud → B2B müügispetsialist\\\\n- Automatiseerimine, AI-tööriistad → Digiarengu spetsialist\\\\n\\\\nKRIITILISED REEGLID\\\\n- Ära soovita LXP staatuste muutmist ilma kinnituskirja olemasolu kontrollimata. APPROVED nõuab LXP-s nähtavat kinnituskirja.\\\\n- Ära tee juhtimisotsuseid (kinnitamine, strateegia).\\\\n- Ära koosta ega saada arveid — aita ainult summasid kontrollida.\\\\n- Ära jaga isikuandmeid (isikukoodid, eratelefonid) ega finantsinfot.\\\\n- Ära anna sisselogimisinfot (LXP admin, Syncer, Merit, EHIS).\\\\n- Ebakindluse korral ole aus ja suuna vastava kolleegi juurde.\\\\n- Sa EI saa avada pilte, faile ega manuseid — töötad ainult tekstiga. Kui kasutaja saadab pildi või faili, selgita seda ausalt.\\\\n\\\\nVASTAMISE STRUKTUUR\\\\nVasta nagu kolleeg Slackis — lühidalt ja konkreetselt. Ei ole vaja sissejuhatust, sektsioone ega kokkuvõtet. Lihtsalt vasta küsimusele.\\\\nKui küsimust pole (nt \\\"tere\\\"), vasta lühidalt ja küsi, millega saad aidata.\\\\n\\\\nMEENUTUSED (maini ainult siis, kui otseselt asjakohane)\\\\n- Tunnistused: eemalda grupikohtumised → genereeri → pane tagasi.\\\\n- Merit arve: kontrolli summasid kinnituskirja ja osavõtulehe järgi.\\\\n- NPS < 50 või lõpetamine < 85%: soovita PDCA ACT-sammu.\\\\n- Täisnimi + isikukood + kontakt sõnumis: hoiata GDPR-riskist."
            }
          ]
        },
        "batching": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.9,
      "position": [
        4240,
        1728
      ],
      "id": "67b8fb3c-5b1b-43e2-b769-911d82f12a53",
      "name": "Basic LLM Chain"
    },
    {
      "parameters": {
        "respondWith": "json",
        "responseBody": "{}",
        "options": {}
      },
      "type": "n8n-nodes-base.respondToWebhook",
      "typeVersion": 1.1,
      "position": [
        4016,
        1536
      ],
      "id": "9793bf71-dfbe-4bee-8a35-8b993bfb669f",
      "name": "Respond to Webhook"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://chat.googleapis.com/v1/{{ $json.space }}/messages",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "googleApi",
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "text",
              "value": "⏳ Mõtlen..."
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        4016,
        1728
      ],
      "id": "e56de8f3-98e3-461f-92bc-ea32f64d26a4",
      "name": "Send Thinking",
      "credentials": {
        "googleApi": {
          "id": "HlkcI4UbmTNeTK7o",
          "name": "Google Service Account account"
        }
      }
    },
    {
      "parameters": {
        "method": "PATCH",
        "url": "=https://chat.googleapis.com/v1/{{ $('Send Thinking').item.json.name }}?updateMask=text",
        "authentication": "predefinedCredentialType",
        "nodeCredentialType": "googleApi",
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "text",
              "value": "={{ $json.text }}"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        4816,
        1728
      ],
      "id": "e6cbdd26-ab1c-4af8-8214-df807b382091",
      "name": "Update Message",
      "credentials": {
        "googleApi": {
          "id": "HlkcI4UbmTNeTK7o",
          "name": "Google Service Account account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const raw = $input.first().json.text || '';\n\nconst cleaned = raw\n  .replace(/^[=]*#+\\s*/gm, '')       // убирает =# и # в начале строк\n  .replace(/\\*\\*(.+?)\\*\\*/g, '*$1*') // **bold** → *bold*\n  .trim();\n\nreturn [{ json: { text: cleaned } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        4592,
        1728
      ],
      "id": "64e37724-9669-4ed2-b75a-9eb562559b76",
      "name": "Format Output"
    },
    {
      "parameters": {
        "content": "Где хранить базу знаний\nРекомендация: Google Drive как источник документов + векторное хранилище для поиска (RAG).\n\nАрхитектура RAG в n8n\n\nWebhook → Parse Message\n              ↓\n    [Supabase Vector Store Retrieval]  ← pgvector\n              ↓\n    Basic LLM Chain (контекст из базы + вопрос)\n              ↓\n    Format Output → Update Message\nОтдельный workflow для индексации:\n\nGoogle Drive (новый/изменённый файл)\n    → Google Drive нода (читать файл)\n    → Разбить на чанки (Text Splitter нода)\n    → Embed: OpenAI Embeddings (text-embedding-3-small)\n    → Supabase Vector Store (Insert)\nn8n имеет встроенные ноды для всего этого. Supabase — самый простой старт.\n\nБЕЗОПАСНОСТЬ БАЗЫ ДАННЫХ\nРиск: содержимое документов уходит в OpenAI (embeddings) и Gemini (ответы). Supabase хранит данные — нужно выбрать EU-регион.\nПравило: индексировать ТОЛЬКО процессные документы (инструкции, шаблоны, стандарты).\nНЕ индексировать: файлы с исикукодами, зарплатами, личными данными участников.\nВарианты если нужна максимальная безопасность:\n\nSupabase EU-регион + OpenAI EU data processing agreement\nИли полностью локально: Ollama + pgvector на своём сервере (сложнее)\nМодели:\n\nGemini 2.0 Flash — для ответов (дешёвый, отличный мультиязык: эстонский, русский, английский)\nOpenAI text-embedding-3-small — только для индексации (~$0.02 на 1M токенов)\nСЛЕДУЮЩИЕ ШАГИ (в очереди):\n\nAsana: бот распознаёт задачу → создаёт в Asana → ремайндеры на неделю и день\nGoogle Calendar: коннектор + ремайндеры. Для каждой задачи выбор — Asana или Calendar (в идеале всё интегрировать вместе)\nПоиск по Google Shared Drives: структура в Drive ещё строится — бот должен помогать найти нужный файл (часть RAG)",
        "height": 976,
        "width": 416
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        3056,
        1536
      ],
      "typeVersion": 1,
      "id": "37ca01ef-443d-4bac-a640-d6bf75012f95",
      "name": "Sticky Note"
    },
    {
      "parameters": {
        "content": "Архитектура:\n\nWebhook → Parse Message\n              ↓\n        Intent Router  ← Gemini классифицирует намерение\n         /    |    \\    \\\n        ↓     ↓     ↓    ↓\n      RAG   Asana  Cal  Drive\n     (docs) (task) (ev) (search)\n              ↓\n        Format → Update Message\nПочему это лучше всего:\n\nКаждая фича — отдельный sub-workflow в n8n. Разрабатываешь и тестируешь независимо, не ломая основной воркфлоу\nIntent Router — одна нода с Gemini, которая определяет что хочет пользователь: ask_question / create_task / create_event / find_file\nЛегко добавлять — новая фича = новый sub-workflow + один case в роутере\nПример классификации:\n\n\"Meenuta mulle homme koosolek\" → create_event\n\"Mis on APPROVED staatus?\" → ask_question (RAG)\n\"Loo Asanas ülesanne\" → create_task\n\"Leia fail kinnituskirja kohta\" → find_file\nЧто добавить позже — история чата (conversation memory) через Supabase, чтобы бот помнил контекст разговора.",
        "height": 784,
        "width": 224
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        3280,
        2432
      ],
      "typeVersion": 1,
      "id": "a0612038-a422-46ba-8525-937959562e8c",
      "name": "Sticky Note1"
    }
  ],
  "pinData": {
    "Webhook": [
      {
        "json": {
          "headers": {
            "host": "evk.app.n8n.cloud",
            "user-agent": "Google-gsuiteaddons",
            "content-length": "3709",
            "accept": "application/json",
            "accept-encoding": "gzip, br",
            "authorization": {
              "__redacted": true,
              "reason": "node_defined_field",
              "canReveal": false
            },
            "cdn-loop": "cloudflare; loops=1; subreqs=1",
            "cf-connecting-ip": "66.249.80.3",
            "cf-ew-via": "15",
            "cf-ipcountry": "US",
            "cf-ray": "9f0cba784bf1765b-DFW",
            "cf-visitor": "{\"scheme\":\"https\"}",
            "cf-worker": "n8n.cloud",
            "content-type": "application/json; charset=utf-8",
            "x-forwarded-for": "66.249.80.3, 162.159.104.26",
            "x-forwarded-host": "evk.app.n8n.cloud",
            "x-forwarded-port": "443",
            "x-forwarded-proto": "https",
            "x-forwarded-server": "traefik-prod-users-gwc-93-59f57ff78b-2xvxf",
            "x-is-trusted": "yes",
            "x-real-ip": "66.249.80.3"
          },
          "params": {},
          "query": {},
          "body": {
            "commonEventObject": {
              "userLocale": "en",
              "hostApp": "CHAT",
              "platform": "WEB",
              "timeZone": {
                "id": "Europe/Tallinn",
                "offset": 10800000
              }
            },
            "authorizationEventObject": {
              "systemIdToken": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjY0NzAxNGY5YTRhNGNiYmI2ZTlhYTFmOWUzMGVlNmNjNzBkYTc0MmEiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJodHRwczovL2V2ay5hcHAubjhuLmNsb3VkL3dlYmhvb2svZ29vZ2xlLWNoYXQiLCJhenAiOiIxMDMyMTA4NDU0Njg0MjQxMTYxNzkiLCJlbWFpbCI6InNlcnZpY2UtMTA3NDczMTk0MTE1MkBnY3Atc2EtZ3N1aXRlYWRkb25zLmlhbS5nc2VydmljZWFjY291bnQuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImV4cCI6MTc3Njk0OTQ0MSwiaWF0IjoxNzc2OTQ1ODQxLCJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJzdWIiOiIxMDMyMTA4NDU0Njg0MjQxMTYxNzkifQ.XB9Ee4Nay96wpMaxKZGFCXevqFxqsbb_K7aBVV412salozj_br92mlUzr8pMlV0ykUtsvUAozxKSXk2gSROdNlnw93hcqOMp2yvg9TyCluEEYPj1-2OHFWp-HByPj4DZqypTXrZfdKZ-nbP6yQ-hZqFSkLyXnQ3osyZHFQa5vx-h1ag3FnNKrrOuuU5jcq1xuyobNGJpVTPclvA7VpxO9-BIMdjPuyVY-CLtoPCjp42eAezIKB6_kKN6jCTbyR-Xbh_fHLFX8pNrumg3TXPdMrt8AXA2iIyM_DcfztKm27dzHSqx2bWRo8lWdpGYY9F4Ffe4oj8ixwiJT-evyTBfHQ"
            },
            "chat": {
              "user": {
                "name": "users/101668536435316678458",
                "displayName": "Emilia Kyutt",
                "avatarUrl": "https://lh3.googleusercontent.com/a/ACg8ocJE7D8VaK0-sIXQY-gC4pPp9jpKu3FnyXP1kAd8-D0pT7j4nVg=k-no",
                "email": "emilia@ettevotluskeskus.ee",
                "type": "HUMAN",
                "domainId": "34c7l1g"
              },
              "eventTime": "2026-04-23T12:04:01.558015Z",
              "messagePayload": {
                "space": {
                  "name": "spaces/nsteYyAAAAE",
                  "type": "DM",
                  "singleUserBotDm": true,
                  "spaceThreadingState": "THREADED_MESSAGES",
                  "spaceType": "DIRECT_MESSAGE",
                  "spaceHistoryState": "HISTORY_ON",
                  "lastActiveTime": "2026-04-23T12:04:01.558015Z",
                  "membershipCount": {
                    "joinedDirectHumanUserCount": 1
                  },
                  "spaceUri": "https://chat.google.com/dm/nsteYyAAAAE?cls=11"
                },
                "message": {
                  "name": "spaces/nsteYyAAAAE/messages/pW46GPLBLkc.pW46GPLBLkc",
                  "sender": {
                    "name": "users/101668536435316678458",
                    "displayName": "Emilia Kyutt",
                    "avatarUrl": "https://lh3.googleusercontent.com/a/ACg8ocJE7D8VaK0-sIXQY-gC4pPp9jpKu3FnyXP1kAd8-D0pT7j4nVg=k-no",
                    "email": "emilia@ettevotluskeskus.ee",
                    "type": "HUMAN",
                    "domainId": "34c7l1g"
                  },
                  "createTime": "2026-04-23T12:04:01.558015Z",
                  "text": "Hello, please describe what you can do shortly",
                  "thread": {
                    "name": "spaces/nsteYyAAAAE/threads/pW46GPLBLkc",
                    "retentionSettings": {
                      "state": "PERMANENT"
                    }
                  },
                  "space": {
                    "name": "spaces/nsteYyAAAAE",
                    "type": "DM",
                    "singleUserBotDm": true,
                    "spaceThreadingState": "THREADED_MESSAGES",
                    "spaceType": "DIRECT_MESSAGE",
                    "spaceHistoryState": "HISTORY_ON",
                    "lastActiveTime": "2026-04-23T12:04:01.558015Z",
                    "membershipCount": {
                      "joinedDirectHumanUserCount": 1
                    },
                    "spaceUri": "https://chat.google.com/dm/nsteYyAAAAE?cls=11"
                  },
                  "argumentText": "Hello, please describe what you can do shortly",
                  "retentionSettings": {
                    "state": "PERMANENT"
                  },
                  "messageHistoryState": "HISTORY_ON",
                  "formattedText": "Hello, please describe what you can do shortly"
                },
                "configCompleteRedirectUri": "https://chat.google.com/api/bot_config_complete?token=AEgAulx7tkIXNfw7iBF98sv7UC_v5SZV0r8Q_Dt_qyq7o2p7FBI6fUaHvxUFBOlVG4KELYHb9YJwAug-xeXMq9U-Wnz8Hj09dTS8jFUm1xz3bxzQzjgt9ixiGnLmEsqvtQ4eytqWg4YYyMt-U_hv_iZWww%3D%3D"
              }
            }
          },
          "webhookUrl": "https://evk.app.n8n.cloud/webhook/google-chat",
          "executionMode": "production"
        },
        "pairedItem": {
          "item": 0
        }
      }
    ]
  },
  "connections": {
    "Webhook": {
      "main": [
        [
          {
            "node": "Parse Message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Parse Message": {
      "main": [
        [
          {
            "node": "Respond to Webhook",
            "type": "main",
            "index": 0
          },
          {
            "node": "Send Thinking",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send Thinking": {
      "main": [
        [
          {
            "node": "Basic LLM Chain",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Anthropic Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "Basic LLM Chain",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Basic LLM Chain": {
      "main": [
        [
          {
            "node": "Format Output",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Format Output": {
      "main": [
        [
          {
            "node": "Update Message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate"
  },
  "versionId": "56450c71-58a5-4353-af50-32d22c95fe6d",
  "meta": {
    "instanceId": "42dcedcb0d0da33eb6b5b6ce82734967fd9739e67ee1d466602b1ae8a308625c"
  },
  "id": "hMjbLIdUOeqWIRae",
  "tags": []
}
