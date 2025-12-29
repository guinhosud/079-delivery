# 079 DELIVERY (MVP pronto para rodar)

Este repositório contém 3 projetos:
- **api/** (Node + Express + Prisma + Postgres): multi-cidades, multi-lojas, catálogo, taxas e pedidos.
- **admin/** (Next.js): painel simples para Admin e para Loja (pedidos + status).
- **mobile/** (Expo React Native): app do Cliente (catálogo + carrinho + pedido por WhatsApp/PIX).

## 1) Pré-requisitos
- Node.js LTS (18 ou 20)
- Postgres (local ou em nuvem)
- (Opcional) Android Studio / Xcode para emular

## 2) Configurar Banco (Postgres)
Crie um banco chamado `delivery079` (ou outro nome).

Exemplo (psql):
```sql
CREATE DATABASE delivery079;
```

## 3) Configurar e rodar a API
Abra um terminal na pasta do projeto e rode:

```bash
npm install
cd api
cp .env.example .env
```

Edite `api/.env` e coloque sua conexão do Postgres em `DATABASE_URL`.

Depois:
```bash
npm run db:push
npm run db:seed
npm run dev
```

API vai subir em: http://localhost:3333

### Logins (seed)
- Admin:
  - email: `admin@079delivery.com`
  - senha: `admin123`
- Loja: você cria no painel admin, depois define a senha da loja no cadastro.

## 4) Rodar o Painel Admin
Em outro terminal:
```bash
cd admin
cp .env.example .env.local
npm install
npm run dev
```
Painel: http://localhost:3000

## 5) Rodar o App Mobile (Cliente)
Em outro terminal:
```bash
cd mobile
cp .env.example .env
npm install
npm run start
```

No app, configure o IP da sua máquina no `EXPO_PUBLIC_API_URL` (no `mobile/.env`).
Ex.: `http://192.168.0.10:3333`

## 6) Fluxo do MVP
1. Admin entra no painel e cria a **Cidade (Itaporanga)**.
2. Admin cria uma **Loja** (com WhatsApp e PIX) e define **Taxas de Entrega** por bairro.
3. Admin cria **Categorias** e **Produtos** para a loja.
4. Cliente abre o app, escolhe cidade e loja, adiciona itens no carrinho e finaliza.
5. No checkout, o cliente:
   - copia o PIX (copia-e-cola) e/ou
   - abre o WhatsApp com o pedido pronto (mensagem gerada).
6. Loja entra com login da loja e muda status do pedido: `RECEBIDO → PREPARANDO → SAIU_PARA_ENTREGA → ENTREGUE` (ou `CANCELADO`).

## Observações importantes
- O pedido é gravado na API **antes** de abrir o WhatsApp (para a loja ver no painel).
- A entrega é por taxa definida pela loja (por bairro). Pode evoluir para cálculo por distância depois.
- PIX: o MVP usa **chave PIX** (copia-e-cola) configurada na loja. Você pode evoluir para QR dinâmico depois.

Bom uso!
