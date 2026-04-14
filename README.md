# RPO de Produção

# Gestão de Frota

![Build Status](https://img.shields.io/badge/build-passing-brightgreen?style=for-the-badge)
![Version](https://img.shields.io/badge/version-1.0.0-blue?style=for-the-badge)
![MySQL](https://img.shields.io/badge/MySQL-316192?style=for-the-badge&logo=MySQL&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-316192?style=for-the-badge&logo=PHP&logoColor=white)

> Solução robusta para centralização de dados veiculares, conformidade de condutores e monitoramento de telemetria em tempo real.

---

## Módulos

### 1. Gestão de Ativos e Documentação
Centralização técnica e jurídica para evitar multas administrativas.
* **Ficha do Veículo:** Registro detalhado (Placa, Chassi, RENAVAM, Modelo, Cor e Combustível).
* **Documentação Digital:** Alertas preditivos de vencimento para **IPVA, Licenciamento e Seguro**.
* **Gestão de Condutores:** Controle de validade de **CNH** e categorias (A, B, D, E) com bloqueio de atribuição.

### 2. Controle Operacional e Reservas
Garantia de disponibilidade e integridade física da frota.
* **Checklist Digital:** Inspeção obrigatória de segurança (pneus, luzes, fluidos) via app antes de cada saída.
* **Gestão de Rotas:** Registro de odômetro inicial e final para auditoria de distância percorrida.
* **Agendamento Inteligente:** Sistema de reservas com **trava automática** para veículos em manutenção ou com pendências críticas.

### 3. Custos e Manutenção Preventiva
Foco na redução do TCO (*Total Cost of Ownership*).
* **Manutenção Automática:** Alertas baseados em quilometragem (ex: Troca de óleo a cada $10.000$ km).
* **Gestão de Abastecimento:** Monitoramento de eficiência energética ($km/l$) e integração com cartões de combustível.
* **Controle de Infrações:** Registro de multas, indicação automática do condutor e controle de prazos de recurso.

### 4. Telemetria e Segurança (Real-time)
Monitoramento ativo via GPS e sensores de comportamento.
* **Rastreamento:** Localização exata em tempo real e histórico completo de posições.
* **Comportamento do Motorista:** Detecção de frenagens bruscas, excesso de velocidade e motor ocioso (desperdício).
* **Cerca Eletrônica (Geofencing):** Notificações imediatas para saídas de perímetro permitido ou entrada em zonas de risco.

---

## TELAS

### Interface do Sistema

Gestão de Ativos e Documentação

![Tela 1](1.png)  
*Centralização de dados e alertas de CNH/IPVA*

Controle Operacional

![Tela 2](2.png)
*Checklist digital e travas de agendamento*

Custos e Manutenção 

![Tela 3](3.png)
*Análise de TCO e preventivas*

Telemetria em Tempo Real

![Tela 4](4.png) 
*Monitoramento GPS e comportamento do condutor*

---

## Diagramas

### Diagramas do Sistema

Diagrama de Casos de Uso

![Tela 1](casos_de_uso.png)  
*Diagrama de Casos de Uso*

#### 1. Os Atores (Quem participa?)
* **Condutor:** O usuário operacional. Suas ações são focadas no uso do veículo (Reserva,   Checklist, Rotas).

* **Gestor de Frota:** O administrador. Suas ações são de controle, análise financeira (TCO), gestão de pessoas e configuração.

* **Atores de Sistema (Telemetria, Manutenção, Ativos):** Note que eles também são "atores". Isso porque o sistema de GPS ou o banco de dados de manutenção "avisam" o software sobre eventos (ex: excesso de velocidade ou troca de óleo) sem intervenção humana direta.

#### 2. O Limite do Sistema (O Retângulo)
  O retângulo grande rotulado como "Sistema de Gestão de Frota" define o que faz parte do seu software. Tudo o que está dentro dele são funcionalidades que precisam ser desenvolvidas.

#### 3. As Funcionalidades e Relacionamentos
* **Os Relacionamentos <<include>> (Obrigatórios):** Isso significa que uma tarefa depende necessariamente da outra.

* **Executar Checklist ➔ Validar Fotos:** O motorista não consegue finalizar o checklist se não tirar as fotos obrigatórias. Uma ação "chama" a outra.

* **Analisar Status ➔ Visualizar Alertas:** Para o Gestor ver um alerta, o sistema obrigatoriamente tem que ter feito a análise de "Status Amarelo/Vermelho" (ex: IPVA vencendo em 3 dias).

* **O Relacionamento <<extend>> (Opcional/Condicional):** Este é o mais importante para a sua "Trava Automática".

* **Monitorar KM ➔ Bloquear Veículo:** O bloqueio não acontece sempre. Ele é uma "extensão" que só ocorre se uma condição for atendida (ex: o KM passou de 10.000 km).

#### 4. Mapeamento por Módulos (Cores e Siglas)
* **M1 (Gestão de Ativos):** Validar CNH, verificar documentação, gerenciar condutores.

* **M2 (Controle Operacional):** Realizar reserva, checklist digital, odômetro.

* **M3 (Custos e Manutenção):** Analisar TCO, verificar abastecimento, alertas de manutenção.

* **M4 (Telemetria):** Monitorar localização, detectar frenagens, configurar geofencing.

Diagrama de Estado

![Tela 2](estado.png)
*Diagrama de Estado*

#### 1. Fluxo de Uso Principal (Estados Verdes)
* **Disponível (Status):** O ponto de partida (marcado com um asterisco). O veículo está pronto para ser utilizado.

* **Reservado:** Ocorre quando um condutor solicita a reserva pelo aplicativo.

* **Transição de retorno:** A reserva pode ser rejeitada ou cancelada, voltando o veículo para o estado Disponível.

* **Em Uso:** O veículo entra neste estado após o checkout e liberação pelo condutor/app.

* **Finalização:** Quando a viagem é encerrada (Check-in), o veículo retorna ao estado Disponível.

#### 2. Bloqueios e Manutenção (Estados Laranjas)
* **Bloqueado (Status):** O veículo é impedido de uso por dois motivos principais:

* **Alerta Preventivo de KM:** Gatilho automático ao atingir 8.500km (Módulo de Manutenção).

* **Checkout com Alertas:** Quando o condutor identifica algo irregular no início da viagem.

* **Em Manutenção:** O veículo transita para este estado para realização de serviços (ex: troca de óleo, pneus).

#### 3. Pendências Críticas (Estados Vermelhos)
* **Pendência Crítica (M1):** O veículo é movido diretamente de Disponível para este estado se houver vencimento de documentos (Módulo de Ativos).

* **Pendência Crítica (Sinalizado com ponto vermelho):** Ocorre em duas situações:

* **Detectado Falha Crítica:** Via telemetria enquanto o veículo está Em Uso.

* **Manutenção Concluída:** Após o serviço, o veículo passa por este estágio de validação de pendência antes de retornar ao fluxo normal.

#### Resumo das Transições de Recuperação
Para que o veículo volte a ficar Disponível, ele deve seguir caminhos de regularização:

* **Regularizar Documentação:** Move o veículo da Pendência Crítica para o estado Disponível.

* **Finalização de Viagem/Check-in:** Retorna o veículo do uso para a disponibilidade.


Diagrama de Sequência

![Tela 3](sequencia.png)

Valida o fluxo mais complexo do sistema: a reserva e liberação de um veículo. Ele demonstra como os diferentes módulos interagem em tempo real e como as validações técnicas e de telemetria são aplicadas:

Validações Iniciais: O processo começa com o Condutor solicitando a reserva ao App. O sistema consulta imediatamente os módulos de Ativos (M1) e Manutenção (M3) para verificar a CNH (Categoria D Regular), a quilometragem para a próxima revisão (ex: 8.500 km) e o status do IPVA/Licenciamento.

Alertas e Pré-Aprovação: Como o IPVA e a manutenção estão próximos (em amarelo), o sistema emite alertas preventivos antes de pré-aprovar a reserva.

Checklist e Liberação: Na fase de checkout, o condutor envia o Checklist Digital com fotos e valida o Odômetro Inicial (ex: 25.102 km).

Monitoramento Ativo: Somente após a liberação do veículo, o sistema envia o comando para o módulo de Telemetria & GPS (M4) iniciar o rastreamento em tempo real.

---

## Guia de Configuração

### Pré-requisitos
* **Docker & Docker Compose**
* **Chave de API** (Google Maps ou Mapbox para o dashboard)
* **PostgreSQL 14+**

### Instalação Rápida
```bash
# 1. Clone o repositório
git clone [https://github.com/seu-usuario/gestao-frota.git](https://github.com/seu-usuario/gestao-frota.git)

# 2. Acesse o diretório
cd gestao-frota

# 3. Configure o ambiente
cp .env.example .env

# 4. Suba os containers
docker-compose up -d
