# 🏭 Templates — Fábrica de Prompts & Workflows n8n

> Repositório central de **prompts de IA** e **workflows de automação n8n** para agentes de atendimento, vendas e operações.

---

## 📋 Índice

- [O que é este repositório?](#o-que-é-este-repositório)
- [Estrutura de Pastas](#estrutura-de-pastas)
- [Como Usar](#como-usar)
  - [Fábrica de Prompts](#fábrica-de-prompts)
  - [Workflows n8n](#workflows-n8n)
- [Convenções e Padrões](#convenções-e-padrões)
- [Guia de Contribuição](#guia-de-contribuição)
- [Base de Conhecimento](#base-de-conhecimento)

---

## O que é este repositório?

Este repositório serve como **fonte única da verdade** para dois tipos de ativos:

| Ativo | Descrição |
|---|---|
| 🤖 **Prompts** | System prompts versionados para agentes de IA (atendimento, vendas, suporte) |
| ⚙️ **Workflows n8n** | Automações exportadas do n8n em formato JSON, prontas para importar |

**Princípios:**
- **Versionamento** — todo prompt e workflow tem versão explícita (V1, V2...)
- **Documentação inline** — cada arquivo documenta seu próprio propósito e uso
- **Reutilização** — componentes modulares que podem ser combinados
- **Rastreabilidade** — changelog de cada alteração

---

## Estrutura de Pastas

```
Templates/
│
├── README.md                          ← Este arquivo
│
├── knowledge/                         ← Base de conhecimento e referências
│   └── ENGENHARIA_DE_PROMPT_AGENTES_ATENDIMENTO.md
│
├── prompts/                           ← Fábrica de Prompts
│   ├── agentes/                       ← Agentes conversacionais completos
│   │   └── [NOME_AGENTE]_V[N].md
│   ├── classificadores/               ← Prompts de classificação (intenção, estágio CRM, etc.)
│   │   └── [NOME_CLASSIFICADOR]_V[N].md
│   ├── extratores/                    ← Prompts de extração de dados estruturados
│   │   └── [NOME_EXTRATOR]_V[N].md
│   └── transformadores/               ← Prompts de transformação/reescrita de texto
│       └── [NOME_TRANSFORMADOR]_V[N].md
│
├── workflows/                         ← Workflows n8n (JSON exportado)
│   ├── atendimento/                   ← Fluxos de atendimento ao cliente
│   │   └── [NOME_WORKFLOW]_V[N].json
│   ├── vendas/                        ← Fluxos de vendas e follow-up
│   │   └── [NOME_WORKFLOW]_V[N].json
│   └── integrações/                   ← Conectores e integrações entre sistemas
│       └── [NOME_WORKFLOW]_V[N].json
│
└── exemplos/                          ← Exemplos de conversas e casos de uso reais
    └── [CONTEXTO]_exemplo.md
```

---

## Como Usar

### Fábrica de Prompts

#### 1. Encontrar o prompt certo

Navegue pela pasta `prompts/` de acordo com o tipo:

| Tipo | Pasta | Quando usar |
|---|---|---|
| **Agente** | `prompts/agentes/` | Precisa de um bot conversacional completo |
| **Classificador** | `prompts/classificadores/` | Precisa categorizar mensagens, intenções ou estágios |
| **Extrator** | `prompts/extratores/` | Precisa extrair dados estruturados de texto livre |
| **Transformador** | `prompts/transformadores/` | Precisa reescrever, resumir ou formatar texto |

#### 2. Estrutura padrão de um arquivo de prompt

Todo arquivo de prompt segue este cabeçalho:

```markdown
---
nome: [Nome do Agente/Prompt]
versao: V[N]
tipo: agente | classificador | extrator | transformador
modelo_testado: gpt-4o | claude-3-5-sonnet | gemini-2.0-flash
temperatura: 0.3
criado_em: YYYY-MM-DD
atualizado_em: YYYY-MM-DD
autor: [nome]
status: rascunho | homologado | producao | depreciado
---

## Descrição
[O que este prompt faz e quando usar]

## Changelog
- V1 (YYYY-MM-DD): Criação inicial
- V2 (YYYY-MM-DD): [O que mudou e por quê]

## Prompt
[O prompt em si]

## Exemplos de Uso
[2-3 exemplos de input/output esperado]

## Notas de Uso
[Limitações, cuidados, contextos onde não funciona bem]
```

#### 3. Usar o prompt em produção

**Via n8n (recomendado):**
1. Copie o conteúdo do campo `## Prompt` do arquivo `.md`
2. Cole no nó **AI Agent** ou **OpenAI Chat Model** do n8n
3. Configure o modelo e temperatura conforme o cabeçalho do arquivo
4. Conecte as ferramentas necessárias

**Via API direta:**
```json
{
  "model": "gpt-4o",
  "temperature": 0.3,
  "messages": [
    {
      "role": "system",
      "content": "[conteúdo do campo ## Prompt]"
    },
    {
      "role": "user",
      "content": "[mensagem do usuário]"
    }
  ]
}
```

#### 4. Versionar alterações

Ao modificar um prompt existente:
1. **Nunca edite** um prompt com `status: producao` diretamente
2. Crie uma cópia com a versão incrementada: `NOME_V2.md`
3. Atualize o `status` da versão antiga para `depreciado`
4. Adicione entrada no `## Changelog` da nova versão
5. Teste antes de marcar como `producao`

---

### Workflows n8n

#### 1. Importar um workflow

1. Abra o n8n
2. Vá em **Workflows → Import from file**
3. Selecione o arquivo `.json` da pasta `workflows/`
4. Configure as credenciais necessárias (veja a seção `## Credenciais` dentro do JSON ou no arquivo `.md` companion)

#### 2. Estrutura padrão de um workflow

Cada workflow na pasta deve ter:

```
workflows/atendimento/
├── NOME_WORKFLOW_V1.json          ← Arquivo exportado do n8n
└── NOME_WORKFLOW_V1.md            ← Documentação companion
```

O arquivo `.md` companion documenta:

```markdown
---
nome: [Nome do Workflow]
versao: V[N]
categoria: atendimento | vendas | integrações
n8n_versao: [versão do n8n onde foi criado]
criado_em: YYYY-MM-DD
status: rascunho | homologado | producao | depreciado
---

## O que faz
[Descrição do fluxo em linguagem simples]

## Trigger
[Como o workflow é disparado: webhook, schedule, manual, etc.]

## Integrações
- [Sistema A] → [Sistema B]
- [Sistema B] → [Sistema C]

## Credenciais necessárias
- [ ] OpenAI API Key
- [ ] WhatsApp Business API
- [ ] Supabase URL + Key
- [ ] [outras]

## Variáveis de ambiente
| Variável | Descrição | Exemplo |
|---|---|---|
| `WEBHOOK_URL` | URL do webhook de entrada | `https://...` |

## Fluxo resumido
1. [Passo 1]
2. [Passo 2]
3. [Passo 3]

## Changelog
- V1 (YYYY-MM-DD): Criação inicial
```

#### 3. Exportar um workflow para o repositório

1. No n8n, abra o workflow
2. Menu **⋮ → Download**
3. Salve o `.json` na pasta correta com o padrão de nome
4. Crie o arquivo `.md` companion com a documentação
5. Commite ambos os arquivos juntos

---

## Convenções e Padrões

### Nomenclatura de Arquivos

```
[NOME_EM_MAIUSCULO_COM_UNDERLINE]_V[NUMERO].extensão

Exemplos:
  AGENTE_VITORIA_V8.md
  CLASSIFICADOR_ESTAGIO_CRM_V3.md
  EXTRATOR_DADOS_PEDIDO_V1.md
  WORKFLOW_ATENDIMENTO_WHATSAPP_V2.json
```

### Status de Ciclo de Vida

```
rascunho → homologado → producao → depreciado
```

| Status | Significado |
|---|---|
| `rascunho` | Em desenvolvimento, não usar em produção |
| `homologado` | Testado e aprovado, pronto para produção |
| `producao` | Atualmente em uso em produção |
| `depreciado` | Substituído por versão mais nova, não usar |

### Modelos Recomendados por Tipo de Tarefa

| Tarefa | Modelo Recomendado | Temperature |
|---|---|---|
| Agente conversacional | `gpt-4o` ou `claude-3-5-sonnet` | 0.3 – 0.5 |
| Classificação | `gpt-4o-mini` ou `gemini-2.0-flash` | 0.0 – 0.2 |
| Extração de dados | `gpt-4o-mini` | 0.0 |
| Transformação de texto | `gpt-4o` | 0.4 – 0.7 |
| Geração criativa | `claude-3-5-sonnet` | 0.7 – 1.0 |

---

## Guia de Contribuição

### Adicionando um novo prompt

```bash
# 1. Crie o arquivo na pasta correta
touch prompts/agentes/AGENTE_NOVO_V1.md

# 2. Use o template padrão (cabeçalho + seções obrigatórias)

# 3. Teste com pelo menos 10 cenários antes de marcar como homologado

# 4. Atualize este README se criar uma nova categoria
```

### Adicionando um novo workflow

```bash
# 1. Exporte o JSON do n8n

# 2. Salve na pasta correta
cp ~/Downloads/workflow.json workflows/atendimento/NOME_WORKFLOW_V1.json

# 3. Crie o arquivo companion de documentação
touch workflows/atendimento/NOME_WORKFLOW_V1.md

# 4. Preencha toda a documentação antes de commitar
```

### Regras gerais

- ✅ Sempre versione — nunca sobrescreva um arquivo em `producao`
- ✅ Sempre documente — sem documentação, o ativo não existe para o time
- ✅ Sempre teste — mínimo de 10 cenários para prompts, 3 execuções completas para workflows
- ❌ Nunca commite credenciais, tokens ou chaves de API
- ❌ Nunca delete arquivos — marque como `depreciado`

---

## Base de Conhecimento

A pasta `knowledge/` contém documentos de referência para o time:

| Arquivo | Descrição |
|---|---|
| [ENGENHARIA_DE_PROMPT_AGENTES_ATENDIMENTO.md](./knowledge/ENGENHARIA_DE_PROMPT_AGENTES_ATENDIMENTO.md) | Guia completo de engenharia de prompt para agentes de atendimento |

### Leitura recomendada antes de criar prompts

1. **Fundamentos** — Seção 1 e 2 do guia de engenharia de prompt
2. **Máquinas de Estado** — Seção 4 (padrão usado em todos os agentes deste repositório)
3. **Guardrails** — Seção 8 (obrigatório para qualquer agente em produção)
4. **Checklist de Qualidade** — Seção 14 (execute antes de marcar como `homologado`)

---

*Mantido pelo time de IA & Automação. Dúvidas? Abra uma issue ou fale no canal #automacoes.*
