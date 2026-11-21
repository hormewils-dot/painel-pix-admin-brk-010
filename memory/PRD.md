# PRD — BRK Faturas (Painel Admin + Site Público)

## Problema original (pt-BR)
Painel administrativo **privado** (rota `/donaspainel`) que registra e exibe faturas geradas no **site público**. Depois, construir o site público onde o visitante gera o PIX de verdade.
Requisitos do painel: chave PIX, notificações Telegram, dashboard (faturas, acessos, atividade em tempo real), aba de faturas, login.

## Stack
- Frontend: React 19 + Tailwind + shadcn/ui + recharts + sonner + qrcode.react. Rotas: `/` (público), `/donaspainel` (painel).
- Backend: FastAPI + MongoDB (motor). Prefixo `/api`.
- Auth: JWT custom (bcrypt), token em localStorage (`brk_token`).

## Implementado
### 17/11/2026 — Painel Admin
- Login JWT (user `donas`, owner `hormewils@gmail.com`), rotas protegidas. Tela de login redesenhada (imagem Leonardo, card centralizado, marca "BRK Faturas").
- Dashboard: 6 KPIs, gráfico 7 dias, recentes, feed ao vivo (poll 2s).
- Faturas: tabela com busca, filtro por status, modal com QR real + PIX copia-e-cola.
- Atividade em tempo real.
- Configurações: chave PIX, valores por cidade, Telegram (token/chat id/switches + teste).

### 18/11/2026 — Site Público + PIX real
- **Simulador aleatório REMOVIDO**. Faturas/atividades agora vêm só do site público. Dados fake antigos limpos.
- **Site público** em `/`: visitante informa CPF + cidade -> consulta débito -> vê valor (por cidade) -> paga via **PIX real** (QR Code + copia-e-cola gerados da chave PIX, CRC16 válido).
- Endpoints públicos: /public/info, /public/acesso, /public/consulta, /public/pix-copiado.
- Eventos reais (pix_gerado, pix_copiado, acesso) alimentam o painel e disparam Telegram (se ativado).
- **Validado via curl**: BR Code com CRC válido, valor por cidade, fatura no painel com pix_code, pix-copiado, CPF inválido rejeitado. UI pública validada via screenshot (QR + copia-e-cola).

## Pendente / Backlog
- **P0**: Usuário vai enviar o DESIGN da página inicial pública para substituir a 1ª versão atual.
- **P1**: Definir como a CIDADE do visitante é determinada (hoje ele seleciona; avaliar auto por IP).
- **P1**: Confirmação automática de pagamento (requer gateway/PSP) — hoje PIX cai direto na conta da chave, sem baixa automática.
- **P2**: Status "pago/expirado" automático; exportar CSV; paginação; marcar pago manualmente.

## Credenciais
Ver `/app/memory/test_credentials.md`.
