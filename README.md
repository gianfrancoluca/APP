# GTI — Entregas & Estoque

App web de gestão integrada de entregas, estoque e movimentações.  
Conecta diretamente às listas do SharePoint via Microsoft Graph API.

---

## 🚀 Deploy no GitHub Pages (5 minutos)

### Passo 1 — Criar repositório no GitHub

1. Acesse [github.com/new](https://github.com/new)
2. Nome: `gti-entregas-estoque`
3. Marque **Public**
4. Clique em **Create repository**
5. Faça upload do arquivo `index.html` para o repositório

### Passo 2 — Ativar GitHub Pages

1. No repositório, vá em **Settings** → **Pages**
2. Em **Source**, selecione **Deploy from a branch**
3. Branch: `main`, pasta: `/ (root)`
4. Clique **Save**
5. Em ~1 minuto seu app estará em: `https://SEU_USUARIO.github.io/gti-entregas-estoque/`

---

## 🔐 Configurar autenticação (Entra ID)

### Passo 3 — Registrar app no Entra ID

1. Acesse [entra.microsoft.com](https://entra.microsoft.com)
2. Vá em **Aplicativos** → **Registros de aplicativo** → **Novo registro**
3. Preencha:
   - **Nome:** GTI Entregas Estoque
   - **Tipos de conta:** Contas neste diretório organizacional apenas
   - **URI de redirecionamento:** Selecione **SPA** e coloque:  
     `https://SEU_USUARIO.github.io/gti-entregas-estoque/`
4. Clique **Registrar**
5. Anote o **Application (client) ID** e o **Directory (tenant) ID**

### Passo 4 — Configurar permissões da API

1. No app registrado, vá em **Permissões de API** → **Adicionar uma permissão**
2. Selecione **Microsoft Graph** → **Permissões delegadas**
3. Adicione:
   - `User.Read`
   - `Sites.ReadWrite.All`
4. Clique **Conceder consentimento do administrador** (botão azul)

### Passo 5 — Obter o Site ID do SharePoint

1. Abra o navegador e acesse a URL abaixo (substitua com seus dados):

```
https://graph.microsoft.com/v1.0/sites/SEU_TENANT.sharepoint.com:/sites/NOME_DO_SEU_SITE?$select=id
```

Por exemplo, se o site SharePoint é `https://rnp.sharepoint.com/sites/GTI`:

```
https://graph.microsoft.com/v1.0/sites/rnp.sharepoint.com:/sites/GTI?$select=id
```

2. Ou use o [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer):
   - Faça login
   - Cole a URL acima
   - O resultado terá algo como: `"id": "rnp.sharepoint.com,abc123,def456"`
   - Copie o valor completo do `id`

### Passo 6 — Atualizar o código

Abra o `index.html` e substitua as 3 linhas no início do JavaScript:

```javascript
const CONFIG = {
  clientId:  'COLE_SEU_CLIENT_ID',
  tenantId:  'COLE_SEU_TENANT_ID',
  siteId:    'COLE_SEU_SITE_ID',
  ...
};
```

Faça commit e push para o GitHub. Pronto!

---

## 📋 Listas SharePoint necessárias

As 3 listas já devem existir no seu site SharePoint:

### GTI_Pedidos
| Coluna | Tipo |
|--------|------|
| Title | Linha de texto |
| Solicitante | Linha de texto |
| Projeto | Linha de texto |
| Modelo | Linha de texto |
| PN | Linha de texto |
| Quantidade | Número |
| ValorUnit | Número |
| DataPedido | Data |
| DataPrevista | Data |
| DataEntrega | Data |
| Status | Escolha (Pendente, Entregue, Cancelado) |
| Observacoes | Várias linhas de texto |

### GTI_Estoque
| Coluna | Tipo |
|--------|------|
| Nome | Linha de texto |
| PN | Linha de texto |
| Quantidade | Número |
| QtdDisponivel | Número |
| StatusEstoque | Escolha (Disponível, Reservado, Movimentado) |
| Origem | Linha de texto |
| DataEntrada | Data |

### GTI_Movimentos
| Coluna | Tipo |
|--------|------|
| Nome | Linha de texto |
| Modelo | Linha de texto |
| Quantidade | Número |
| Destino | Linha de texto |
| Responsavel | Linha de texto |
| Localizacao | Linha de texto |
| SCReferencia | Linha de texto |
| Observacoes | Várias linhas de texto |

---

## ✨ Funcionalidades

- **Login Microsoft 365** — autenticação segura via Entra ID
- **3 abas** — Entregas, Estoque, Movimentos
- **Filtros e busca** — por texto, ano, status
- **Mover para Estoque** — com 1 clique, move pedido para estoque e registra movimento
- **Exportar Excel** — gera planilha com 3 abas
- **Tema escuro** — alternável com botão
- **Responsivo** — funciona no celular
- **Tempo real** — dados compartilhados via SharePoint

---

## 🔧 Solução de problemas

**"AADSTS50011: The redirect URI does not match"**  
→ Verifique se a URI de redirecionamento no Entra ID bate exatamente com a URL do GitHub Pages (incluindo a barra final `/`).

**"Access denied" ao carregar dados**  
→ Verifique se as permissões `Sites.ReadWrite.All` foram concedidas com consentimento do admin.

**Dados não aparecem**  
→ Confirme que o `siteId` está correto e que as listas existem com os nomes exatos: GTI_Pedidos, GTI_Estoque, GTI_Movimentos.
