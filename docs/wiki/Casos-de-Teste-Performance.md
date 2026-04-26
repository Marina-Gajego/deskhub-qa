# Casos de Teste de Performance

Este documento apresenta as condições de teste e os cenários mapeados para os testes de performance da API do DeskHub utilizando o **k6**. O foco é garantir que o sistema (CPU e Memória) suporte múltiplos acessos concorrentes, especialmente em rotas críticas como o processamento de login e a gestão do banco em memória.

## Condições de Teste
1. **Load Test (Carga de Pico Estimado)**: Simulação do comportamento de múltiplos usuários simultâneos (ex: 5 VUs no CI ou 20 VUs local) durante um período de pico padrão do escritório. Foca em manter a estabilidade do tempo de resposta (p95 < 500ms) e baixa taxa de erros (< 1%).

## Casos de Teste

### 1. Cenário: Fluxo Completo de Usuário (Load Test)
**Objetivo:** Validar o comportamento do backend sob carga simulando o caminho feliz completo de múltiplos usuários interagindo com o DeskHub simultaneamente.

| ID | Prioridade | Condição | Passos (VU) | Critérios de Sucesso (Thresholds) |
|:---|:---|:---|:---|:---|
| CTP01 | Alta | 2 (Load) | 1. `POST /api/auth/register` (Gerar usuário único) <br> 2. `POST /api/auth/login` (Gerar Token) <br> 3. `GET /api/desks` (Consultar mesas disponíveis do dia) <br> 4. `POST /api/reservations` (Criar reserva em mesa aleatória) <br> 5. `GET /api/reservations/my` (Listar reservas pessoais) <br> 6. `DELETE /api/reservations/:id` (Cancelar reserva criada) | - **http_req_duration**: p(95) < 500ms <br> - **http_req_failed**: rate < 1% <br> - Status HTTP esperado para cada requisição (200, 201 ou 409). |

