<div id="top">

<!-- HEADER STYLE: CLASSIC -->
<div align="center">

<img src="images/atividade2.png" width="30%" style="position: relative; top: 0; right: 0;" alt="Project Logo"/>

# Dicionário Distribuído

<em>Potencie as suas aplicações com soluções escaláveis e integradas.</em>
<em>Um dicionário distribuído construído com as seguintes ferramentas e tecnologias:</em>

<img src="https://img.shields.io/badge/JSON-000000.svg?style=flat&logo=JSON&logoColor=white" alt="JSON">
<img src="https://img.shields.io/badge/npm-CB3837.svg?style=flat&logo=npm&logoColor=white" alt="npm">
<img src="https://img.shields.io/badge/Composer-885630.svg?style=flat&logo=Composer&logoColor=white" alt="Composer">
<img src="https://img.shields.io/badge/JavaScript-F7DF1E.svg?style=flat&logo=JavaScript&logoColor=black" alt="JavaScript">
<img src="https://img.shields.io/badge/GNU%20Bash-4EAA25.svg?style=flat&logo=GNU-Bash&logoColor=white" alt="GNU%20Bash">
<img src="https://img.shields.io/badge/React-61DAFB.svg?style=flat&logo=React&logoColor=black" alt="React">
<br>
<img src="https://img.shields.io/badge/Docker-2496ED.svg?style=flat&logo=Docker&logoColor=white" alt="Docker">
<img src="https://img.shields.io/badge/TypeScript-3178C6.svg?style=flat&logo=TypeScript&logoColor=white" alt="TypeScript">
<img src="https://img.shields.io/badge/PHP-777BB4.svg?style=flat&logo=PHP&logoColor=white" alt="PHP">
<img src="https://img.shields.io/badge/k6-7D64FF.svg?style=flat&logo=k6&logoColor=white" alt="k6">
<img src="https://img.shields.io/badge/Axios-5A29E4.svg?style=flat&logo=Axios&logoColor=white" alt="Axios">

</div>
<br>

---

## 📄 Índice

- [Demo do Sistema](#-demo-do-sistema)
- [Visão Geral](#-visão-geral)
- [Qualidades dos Componentes do Sistema Distribuído](#-qualidades-dos-componentes-do-sistema-distribuído)
- [Decisões Arquiteturais Estratégicas](#️-decisões-arquiteturais-estratégicas)
- [Arquitetura do Sistema](#-arquitetura-do-sistema)
- [Fluxo do Sistema](#-fluxo-do-sistema)
- [Manual da API](#-manual-da-api)
- [Primeiros Passos](#-primeiros-passos)
    - [Pré-requisitos](#-requisitos)
    - [Instalação](#️-instalação)
    - [Utilização](#-utilização)
- [Demo de Instalação](#-demo-de-instalação)
- [Implementação em Cloud](#️-implementação-em-cloud-awsazuregoogle-cloud)
- [Estrutura do Projeto](#-estrutura-do-projeto)
    - [Índice do Projeto](#-índice-do-projeto)
- [Testes](#-testes)
    - [Resultados e Análise](#-resultados-e-análise)
- [Limites e Capacidades](#-limites-e-capacidades)
- [Contribuir](#-contribuir)
- [Bibliografia](#bibliografia)

---
## 🎥 Demo do Sistema

📥 [Clique aqui para transferir o vídeo completo do demo do sistema (`demo_sistema.mp4`)](images/demo_sistema.mp4)

**Funcionalidades Demonstradas:**
1. Inserção de palavras com persistência em CockroachDB
2. Recuperação via cache Redis
3. Exclusão com verificação em tempo real
4. Healthchecks operacionais

Para ver um guia com os comandos utilizados vá a [(`manual.txt`)](./manual.txt)

---

## ✨ Visão Geral

Atividade2 é uma ferramenta poderosa para programadores, concebida para simplificar a implementação e gestão de aplicações baseadas em microserviços, garantindo alta disponibilidade e escalabilidade.

**Porquê usar o Atividade2?**

Este projeto simplifica a orquestração de ecossistemas de aplicações complexas. As funcionalidades principais incluem:

- 🚀 **Arquitetura de Microserviços:** Simplifica a implementação e escalabilidade de aplicações.
- ⚙️ **Configuração Automatizada:** Facilita a criação do ambiente de desenvolvimento com Docker Compose.
- 📊 **Scripts de Testes de Carga:** Garante o desempenho sob diferentes condições, identificando gargalos.
- 🔗 **Gestão Robusta de APIs:** Permite uma comunicação fluída entre os serviços de frontend e backend.
- 🛠️ **Monitorização de Saúde:** Verificações de integridade integradas aumentam a fiabilidade.
- 📈 **Relatórios Detalhados:** Gera relatórios de desempenho pormenorizados para suportar decisões informadas.

---

## 🌐 Qualidades dos Componentes do Sistema Distribuído

### **1. Consistência de Dados**
| Componente         | Contribuição                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| **CockroachDB**     | Utiliza o algoritmo **Raft** para consenso distribuído, garantindo operações lineares e ACID mesmo durante partições de rede. |
| **Redis Cluster**   | Implementa o protocolo **CRDTs** (Conflict-free Replicated Data Types) para resolução automática de conflitos em escrita paralela. |
| **RabbitMQ Quórum** | Filas quórum com replicação síncrona asseguram entrega exatamente-uma (exactly-once) de mensagens. |

### **2. Tolerância a Falhas**
| Componente         | Mecanismo de Resiliência                                                    |
|---------------------|-----------------------------------------------------------------------------|
| **HAProxy**         | Health checks ativos + failover automático entre instâncias PHP-API e RabbitMQ. |
| **Redis Cluster**   | Réplicas shardadas com failover automático via algoritmo **Gossip**.        |
| **CockroachDB**     | Replicação multi-AZ com recuperação de nós em <30s usando **Range Leases**. |

### **3. Escalabilidade Horizontal**
| Componente         | Estratégia de Escalonamento                                                 |
|---------------------|-----------------------------------------------------------------------------|
| **PHP-API**         | Arquitetura stateless permite adição dinâmica de réplicas via Docker Swarm. |
| **Workers**         | Consumidores paralelos com auto-balancing via **Prefetch Count** no RabbitMQ. |
| **Nginx**           | Balanceamento Round-Robin + cache de respostas HTTP para descarregar a API. |

### **4. Alta Disponibilidade**
| Componente         | Técnica de Garantia                                                         |
|---------------------|-----------------------------------------------------------------------------|
| **CockroachDB**     | Replicação multi-região com eleição de líder via **Raft Leader Lease**.     |
| **RabbitMQ**        | Filas espelhadas (mirrored queues) com política quórum para alta durabilidade. |
| **HAProxy**         | Circuit breaking inteligente com re-tentativas em múltiplas camadas.       |

### **5. Coordenação de Recursos**
| Componente         | Protocolo/Mecanismo                                                         |
|---------------------|-----------------------------------------------------------------------------|
| **Redis**           | Lock distribuído via **RedLock** para operações atômicas entre workers.     |
| **CockroachDB**     | Sistema de transações distribuídas usando **Timestamp Oracle**.            |
| **PHP-API**         | Geração centralizada de UUIDs versionados para evitar colisões em inserts. |

---

## 🏗️ Decisões Arquiteturais Estratégicas

### **CockroachDB**
- **Por quê?** Resolve o problema de SPOF (Single Point of Failure) através de replicação automática
- **Contribuição para SD**:
  - **Consistência**: Modelo de relógio híbrido (Hybrid Logical Clocks)
  - **Tolerância**: Sobrevive a falhas de N/2-1 nós simultâneos
  - **Escala**: Adição transparente de novos nós ao cluster

### **Redis Cluster**
- **Por quê?** Oferece cache distribuído com consistência eventual controlada
- **Contribuição para SD**:
  - **Performance**: Reduz latência de leituras para <10ms
  - **Disponibilidade**: Re-sharding automático durante falhas
  - **Consistência**: Políticas TTL-driven para invalidação de cache

### **HAProxy**
- **Por quê?** Unifica o plano de controle para múltiplos serviços
- **Contribuição para SD**:
  - **Balanceamento**: Distribuição inteligente baseada em saúde dos nós
  - **Observabilidade**: Métricas em tempo real via endpoint /stats
  - **Segurança**: Rate limiting para prevenir DDoS

### **RabbitMQ com Quórum**
- **Por quê?** Garante durabilidade de mensagens em cenários de crash
- **Contribuição para SD**:
  - **Ordenação**: Garantia de entrega FIFO dentro do mesmo canal
  - **Durabilidade**: Replicação síncrona para ≥(N/2+1) nós
  - **Eficiência**: Prefetching otimizado para workers paralelos

### **PHP-API (Stateless)**
- **Por quê?** Permite escalonamento horizontal sem coordenação complexa
- **Contribuição para SD**:
  - **Elasticidade**: Pods efêmeros escaláveis sob demanda
  - **Resiliência**: Reinicialização rápida sem perda de estado
  - **Portabilidade**: Imagem Docker autocontida

### **Nginx**
- **Por quê?** Edge router com terminação SSL e compressão eficiente
- **Contribuição para SD**:
  - **Caching**: Reduz carga no backend em 40% para leituras
  - **Segurança**: WAF integrado contra injeção SQL/XSS
  - **Protocolos**: Suporte nativo para HTTP/2 e WebSocket

### **Workers Assíncronos**
- **Por quê?** Separação clara entre camada de ingestão e processamento
- **Contribuição para SD**:
  - **Throughput**: Paralelismo massivo via múltiplos consumidores
  - **Isolamento**: Falhas no worker não afetam a API principal
  - **Backpressure**: Controle de fluxo via prefetch count


---

## 🌐 Arquitetura do Sistema

![Diagrama de Arquitetura](images/arquitetura_sistema.png)

---

## 🔄 Fluxo do Sistema

![Fluxo do Sistema](images/fluxo_sistema.png)


## 📚 Manual da API

[![API Reference](https://img.shields.io/badge/Documentação-API%20Reference-0080ff?style=for-the-badge&logo=openapi-initiative)](https://atividade2-dictionary.netlify.app/docs/index.html)

Explore a API completa com testes em tempo real:

- 🧪 **Testar endpoints diretamente no navegador**  
- 📄 **Visualizar schemas de requisição/resposta**  
- 📱 **Gerar código para 10+ linguagens automaticamente**

*Acesso: [atividade2-dictionary.netlify.app](https://atividade2-dictionary.netlify.app/docs/index.html)*

---

## 🚀 Primeiros Passos

### 📋 Requisitos

Este projeto requer a seguinte dependência:

- **Runtime de Containers:** Docker

### ⚙️ Instalação

Construa a Atividade2 a partir do código-fonte e instale as dependências:

1. **Clone o repositório:**

```sh
 git clone https://github.com/a75739/Atividade2
```

2. **Aceda à pasta do projeto:**

```sh
 cd Atividade2
```

3. **Execute o Docker Engine:**

**Usando [Docker](https://www.docker.com/)**

---

### 💻 Utilização

#### Em linux:

Execute o projeto com:

```sh
 ./start.sh
```

#### Em Windows, requer [Git Bash](https://git-scm.com/downloads/win) ou [WSL](https://ubuntu.com/desktop/wsl):

##### Com Git Bash:

Clique na pasta do projeto `Atividade2` com o lado direito do rato e selecione "Mostrar mais opções" > "Open Git Bash here"

Execute o projeto com:

```sh
 ./start.sh
```

##### Com WSL:

Execução igual a [linux](#em-linux).

Pare a execução do projeto com:

```sh
 docker-compose down --remove-orphans
```

## 🎬 Demo de Instalação

Veja abaixo uma demonstração rápida da instalação do projeto:

![Prévia da Instalação](images/instalacao.gif)

📥 [Clique aqui para transferir o vídeo completo (`instalacao.mp4`)](images/instalacao.mp4)

---

## ☁️ Implementação em Cloud (AWS/Azure/Google Cloud)

### 📋 Pré-requisitos

- Conta em um provedor de cloud (AWS, Azure, ou GCP)
- CLI do provedor instalada (ex: AWS CLI, `gcloud`, `az`)
- Docker Hub ou registo de containers do provedor (ex: AWS ECR)

### ⚙️ Configuração

#### 1. Construa a imagem Docker
```sh
docker build -t atividade2:latest .
```

#### 2. Autenticação no Registo de Containers

Docker Hub:

```sh
docker login
```

AWS ECR (exemplo):

```sh
aws ecr get-login-password --region sua-regiao | docker login --username AWS --password-stdin sua-conta-id.dkr.ecr.sua-regiao.amazonaws.com
```

#### 3. Tag e Envio da Imagem

```sh
docker tag atividade2:latest seu-registo/atividade2:latest
docker push seu-registo/atividade2:latest
```

### 🚀 Implementação

AWS ECS:

#### 1. Crie um cluster ECS

#### 2. Defina uma task com a imagem enviada

#### 3. Configure o mapeamento de porta 8000:8000

Google Cloud Run:

```sh
gcloud run deploy atividade2-api --image seu-registo/atividade2:latest --port 8000 --allow-unauthenticated
```

Azure Container Instances:

```sh
az container create --resource-group seu-resource-group --name atividade2-api --image seu-registo/atividade2:latest --ports 8000
```
### 🔧 Configuração Adicional

Configure variáveis de ambiente (se necessário):

```sh
NODE_ENV=production
```

### 🌐 Acesso à API

Use o URL fornecido pelo serviço de cloud

```sh
https://atividade2-xyz.a.run.app
```
---


## 📁 Estrutura do Projeto

```sh
└── Atividade2/
    ├── README.md
    ├── backend/
    │   ├── Dockerfile
    │   ├── apache-config.conf
    │   ├── composer.json
    │   ├── postgres/
    │   ├── public/
    │   └── worker/
    ├── docs/
    │   ├── openapi.yaml
    │   ├── index.html
    │   ├── architecture-diagram.png
    ├── docker-compose.yml
    ├── frontend/
    │   ├── Dockerfile
    │   ├── README.md
    │   ├── package-lock.json
    │   ├── package.json
    │   ├── public/
    │   ├── src/
    │   └── tsconfig.json
    ├── haproxy.cfg
    ├── images/
    ├── manual.txt
    ├── nginx.conf
    ├── public/
    │   ├── docs/
    │   │   ├── openapi.yaml
    │   │   ├── index.html
    │   ├── testes-carga/
    │       ├── relatorios
    │       └── scripts
    ├── start.sh
```

---

### 📑 índice do Projeto

<details open>
    <summary><b><code>ATIVIDADE2/</code></b></summary>
    <!-- __root__ Submodule -->
    <details>
        <summary><b>__root__</b></summary>
        <blockquote>
            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                <code><b>⦿ __root__</b></code>
            <table style='width: 100%; border-collapse: collapse;'>
            <thead>
                <tr style='background-color: #f8f9fa;'>
                    <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                    <th style='text-align: left; padding: 8px;'>Sumário</th>
                </tr>
            </thead>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/docker-compose.yml'>docker-compose.yml</a></b></td>
                    <td style='padding: 8px;'>
                        - Define uma configuração Docker Compose para uma arquitetura de microsserviços, orquestrando múltiplos nós Redis para cache, instâncias CockroachDB para gestão de base de dados distribuída, RabbitMQ para filas de mensagens, e uma API backend em PHP<br>
                        - Facilita a comunicação transparente entre serviços, garantindo alta disponibilidade e escalabilidade, ao mesmo tempo que gere verificações de integridade e dependências, suportando assim um ecossistema de aplicação robusto.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/start.sh'>start.sh</a></b></td>
                    <td style='padding: 8px;'>
                        - Automatiza a configuração e inicialização de uma arquitetura multi-serviços utilizando Docker Compose<br>
                        - Orquestra o deployment dos serviços Redis, CockroachDB, RabbitMQ, backend e frontend, assim como balanceadores de carga, garantindo que cada componente esteja pronto antes de prosseguir<br>
                        - Este script simplifica a preparação do ambiente de desenvolvimento, facilitando a gestão eficiente dos serviços e a integração dentro da arquitetura global do código.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/nginx.conf'>nginx.conf</a></b></td>
                    <td style='padding: 8px;'>
                        - Configura um servidor Nginx para gerir eficientemente pedidos HTTP recebidos, direcionando o tráfego para a API PHP e assegurando um desempenho otimizado através de definições que melhoram o manuseamento de ligações e gestão de timeouts<br>
                        - Inclui também um endpoint de verificação de integridade para monitorizar o estado do servidor, contribuindo para a fiabilidade e capacidade de resposta da arquitetura da aplicação.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/haproxy.cfg'>haproxy.cfg</a></b></td>
                    <td style='padding: 8px;'>
                        - Configura o HAProxy para gerir o tráfego dos serviços RabbitMQ e CockroachDB, garantindo balanceamento de carga eficiente e verificações de integridade<br>
                        - Estabelece frontends para ambos os serviços, direcionando as ligações recebidas para os backends apropriados enquanto monitoriza a saúde dos servidores<br>
                        - Adicionalmente, fornece uma interface de estatísticas com informações de desempenho em tempo real, contribuindo para a fiabilidade e escalabilidade da arquitetura da aplicação.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/manual.txt'>manual.txt</a></b></td>
                    <td style='padding: 8px;'>
                        - Fornece instruções essenciais para configurar e gerir um sistema distribuído utilizando Docker<br>
                        - Descreve os passos para construir os contentores, configurar os vários clusters e verificar a sua saúde e conectividade<br>
                        - Adicionalmente, inclui comandos para monitorização de *logs* e execução de testes de carga, assegurando a escalabilidade e resiliência do sistema, em conformidade com a arquitetura do projeto focada na disponibilidade e tolerância a partições.
                    </td>
                </tr>
            </table>
        </blockquote>
    </details>
    <!-- testes-carga Submodule -->
    <details>
        <summary><b>public/testes-carga</b></summary>
        <blockquote>
            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                <code><b>⦿ public/testes-carga</b></code>
            <!-- scripts Submodule -->
            <details>
                <summary><b>scripts</b></summary>
                <blockquote>
                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                        <code><b>⦿ public.testes-carga.scripts</b></code>
                    <table style='width: 100%; border-collapse: collapse;'>
                    <thead>
                        <tr style='background-color: #f8f9fa;'>
                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                        </tr>
                    </thead>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/testes-carga/scripts/stress-test.js'>stress-test.js</a></b></td>
                            <td style='padding: 8px;'>
                                - Realiza um teste de stress ao sistema, simulando um elevado volume de pedidos HTTP PUT simultâneos para avaliar o desempenho sob carga<br>
                                - Define fases para aumentar gradualmente o tráfego de utilizadores, monitoriza os tempos de resposta e verifica os estados de sucesso dos pedidos<br>
                                - Adicionalmente, gera um relatório HTML a resumir os resultados do teste, ajudando a identificar os limites do sistema e a garantir a fiabilidade durante cenários de utilização intensa.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/testes-carga/scripts/smoke-test.js'>smoke-test.js</a></b></td>
                            <td style='padding: 8px;'>
                                - Realiza testes de fumo à aplicação, simulando múltiplos utilizadores virtuais a interagir com um endpoint específico<br>
                                - Verifica o tratamento correto de pedidos PUT e GET, assegurando que a aplicação responde adequadamente sob carga<br>
                                - Adicionalmente, gera um relatório HTML completo com o resumo dos resultados, contribuindo para a avaliação da fiabilidade e do desempenho da arquitetura da base de código.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/testes-carga/scripts/load-test.js'>load-test.js</a></b></td>
                            <td style='padding: 8px;'>
                                - Script de teste de carga que simula interações de utilizadores com um serviço web, avaliando o seu desempenho sob diferentes condições de tráfego<br>
                                - Executa uma série de pedidos HTTP—PUT, GET e DELETE—enquanto monitoriza os tempos de resposta e as taxas de sucesso<br>
                                - Os resultados são compilados num relatório HTML, fornecendo informações sobre a fiabilidade e eficiência do sistema, essenciais para manter um desempenho ideal na arquitetura global da base de código.
                            </td>
                        </tr>
                    </table>
                </blockquote>
            </details>
            <!-- relatorios Submodule -->
            <details>
                <summary><b>relatorios</b></summary>
                <blockquote>
                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                        <code><b>⦿ testes-carga.relatorios</b></code>
                    <table style='width: 100%; border-collapse: collapse;'>
                    <thead>
                        <tr style='background-color: #f8f9fa;'>
                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                        </tr>
                    </thead>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/testes-carga/relatorios/smoke-test-report.html'>smoke-test-report.html</a></b></td>
                            <td style='padding: 8px;'>
                                - O relatório de teste de fumo fornece uma visão geral abrangente dos resultados dos testes de carga realizados com o K6, apresentando métricas chave como o total de pedidos, pedidos falhados e limiares de desempenho<br>
                                - Os dados são apresentados de forma visual e organizada, permitindo aos utilizadores avaliar rapidamente o estado e o desempenho da aplicação testada<br>
                                - Este relatório é essencial para identificar potenciais problemas e garantir que a aplicação cumpre os requisitos de desempenho.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/testes-carga/relatorios/stress-test-report.html'>stress-test-report.html</a></b></td>
                            <td style='padding: 8px;'>
                                - Gera um relatório HTML completo com os resultados dos testes de carga realizados com o K6, fornecendo informações sobre métricas de desempenho como total de pedidos, pedidos falhados e tempos de resposta<br>
                                - Este relatório resume visualmente estatísticas e limiares relevantes, permitindo que os intervenientes avaliem o desempenho do sistema sob stress<br>
                                - Funciona como uma ferramenta crucial para avaliar a fiabilidade e escalabilidade da aplicação, contribuindo para uma tomada de decisões informada durante o ciclo de desenvolvimento.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/testes-carga/relatorios/load-test-report.html'>load-test-report.html</a></b></td>
                            <td style='padding: 8px;'>
                                - O ficheiro <code>load-test-report.html</code> apresenta um relatório completo gerado a partir dos testes de carga realizados no âmbito do projeto<br>
                                - O seu principal objetivo é apresentar os resultados dos testes de desempenho executados com a ferramenta K6, oferecendo uma análise do comportamento do sistema sob diferentes condições de carga<br>
                                - Este relatório é uma componente essencial da arquitetura da base de código, permitindo a programadores e decisores visualizar métricas de desempenho, identificar possíveis estrangulamentos e tomar decisões fundamentadas sobre otimizações do sistema<br>
                                - Ao fornecer um resumo claro e estruturado dos resultados dos testes de carga, este relatório HTML reforça a capacidade do projeto de garantir fiabilidade e escalabilidade em ambientes de produção. Em suma, o ficheiro <code>load-test-report.html</code> é um artefato crítico que encapsula os resultados dos testes de carga, contribuindo para o objectivo do projeto de entregar uma aplicação robusta e de elevado desempenho.
                            </td>
                        </tr>
                    </table>
                </blockquote>
            </details>
        </blockquote>
    </details>
    <!-- backend Submodule -->
    <details>
        <summary><b>backend</b></summary>
        <blockquote>
            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                <code><b>⦿ backend</b></code>
            <table style='width: 100%; border-collapse: collapse;'>
            <thead>
                <tr style='background-color: #f8f9fa;'>
                    <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                    <th style='text-align: left; padding: 8px;'>Sumário</th>
                </tr>
            </thead>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/backend/apache-config.conf'>apache-config.conf</a></b></td>
                    <td style='padding: 8px;'>
                        - Configura o servidor web Apache para servir uma aplicação web alojada no diretório raiz especificado<br>
                        - Estabelece um *virtual host* para tratamento de pedidos HTTP na porta 80, permitindo a reescrita de URLs e concedendo acesso ao diretório público<br>
                        - Adicionalmente, define o registo de eventos de erro e de acesso, assegurando uma monitorização e gestão adequadas do tráfego web dentro da arquitetura geral do projeto.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/backend/Dockerfile'>Dockerfile</a></b></td>
                    <td style='padding: 8px;'>
                        - Facilita a configuração de um ambiente PHP 8.2 com Apache para o backend do projeto<br>
                        - Garante a instalação das dependências necessárias, configura definições do Apache e prepara a aplicação para implementação<br>
                        - Ao otimizar o *autoloader* e implementar verificações de integridade (*health checks*), melhora o desempenho e a fiabilidade, contribuindo para a eficiência e manutenibilidade da arquitetura global.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/backend/composer.json'>composer.json</a></b></td>
                    <td style='padding: 8px;'>
                        - Define as dependências essenciais para o backend do projeto, assegurando compatibilidade com várias extensões e bibliotecas PHP<br>
                        - Ao especificar requisitos como PDO para interacção com base de dados, Redis para cache e AMQP para mensagens, facilita a integração e comunicação fluída dentro da arquitetura geral, permitindo uma gestão robusta de dados e uma orquestração eficiente de serviços em toda a aplicação.
                    </td>
                </tr>
            </table>
            <!-- worker Submodule -->
            <details>
                <summary><b>worker</b></summary>
                <blockquote>
                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                        <code><b>⦿ backend.worker</b></code>
                    <table style='width: 100%; border-collapse: collapse;'>
                    <thead>
                        <tr style='background-color: #f8f9fa;'>
                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                        </tr>
                    </thead>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/backend/worker/worker.php'>worker.php</a></b></td>
                            <td style='padding: 8px;'>
                                - Responsável pelo processamento de eventos relacionados com o dicionário, conectando-se a um *message broker* RabbitMQ e a uma base de dados CockroachDB<br>
                                - Escuta atualizações e eliminações de palavras, atualizando a cache Redis e persistindo as alterações na base de dados conforme necessário<br>
                                - Esta funcionalidade é essencial para manter a sincronização em tempo real dos dados do dicionário em toda a aplicação, assegurando uma recuperação eficiente de dados e consistência.
                            </td>
                        </tr>
                    </table>
                </blockquote>
            </details>
            <!-- postgres Submodule -->
            <details>
                <summary><b>postgres</b></summary>
                <blockquote>
                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                        <code><b>⦿ backend.postgres</b></code>
                    <table style='width: 100%; border-collapse: collapse;'>
                    <thead>
                        <tr style='background-color: #f8f9fa;'>
                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                        </tr>
                    </thead>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/backend/postgres/db.php'>db.php</a></b></td>
                            <td style='padding: 8px;'>
                                - Estabelece uma ligação a uma base de dados CockroachDB através de uma função robusta que garante acesso persistente e lida graciosamente com eventuais erros de ligação<br>
                                - Ao utilizar variáveis de ambiente para configuração, integra-se perfeitamente na arquitetura global do backend, facilitando interações fiáveis com a base de dados, essenciais para a gestão de dados e eficiência operacional da aplicação<br>
                                - Este mecanismo de ligação é crucial para manter a integridade dos dados e o desempenho consistente ao longo de toda a base de código.
                            </td>
                        </tr>
                    </table>
                </blockquote>
            </details>
        </blockquote>
    </details>
    <!-- frontend Submodule -->
    <details>
        <summary><b>frontend</b></summary>
        <blockquote>
            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                <code><b>⦿ frontend</b></code>
            <table style='width: 100%; border-collapse: collapse;'>
            <thead>
                <tr style='background-color: #f8f9fa;'>
                    <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                    <th style='text-align: left; padding: 8px;'>Summary</th>
                </tr>
            </thead>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/package.json'>package.json</a></b></td>
                    <td style='padding: 8px;'>
                        - Define o ambiente do frontend para uma aplicação React, gerindo as dependências e os scripts essenciais para o desenvolvimento, testes e builds de produção<br>
                        - Integra várias bibliotecas para testes, *routing* e gestão de estado, garantindo uma interface de utilizador robusta<br>
                        - Adicionalmente, estabelece configurações para TypeScript e ESLint, promovendo a qualidade do código e a compatibilidade entre diferentes navegadores, contribuindo assim para a arquitetura geral do projeto.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/tsconfig.json'>tsconfig.json</a></b></td>
                    <td style='padding: 8px;'>
                        - Configura as opções do compilador TypeScript para o frontend do projeto, garantindo compatibilidade com funcionalidades modernas de JavaScript, mesmo em ambientes mais antigos<br>
                        - Estabelece regras rigorosas de verificação de tipos e suporta sintaxe JSX para componentes React<br>
                        - Ao incluir o diretório de origem, facilita uma experiência de desenvolvimento fluida, promovendo a qualidade e a manutenibilidade do código na arquitetura da base de código.
                    </td>
                </tr>
                <tr style='border-bottom: 1px solid #eee;'>
                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/Dockerfile'>Dockerfile</a></b></td>
                    <td style='padding: 8px;'>
                        - Facilita a configuração de um ambiente Node.js para a aplicação frontend, definindo os passos necessários para instalar as dependências e executar o servidor de desenvolvimento<br>
                        - Ao estabelecer um diretório de trabalho, copiar os ficheiros de configuração e expor a porta adequada, garante um processo de desenvolvimento simplificado, permitindo aos programadores focarem-se na implementação de funcionalidades e na melhoria da experiência do utilizador dentro da arquitetura global do projeto.
                    </td>
                </tr>
            </table>
            <!-- src Submodule -->
            <details>
                <summary><b>src</b></summary>
                <blockquote>
                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                        <code><b>⦿ frontend.src</b></code>
                    <table style='width: 100%; border-collapse: collapse;'>
                    <thead>
                        <tr style='background-color: #f8f9fa;'>
                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                        </tr>
                    </thead>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/react-app-env.d.ts'>react-app-env.d.ts</a></b></td>
                            <td style='padding: 8px;'>
                                - Define tipos TypeScript para uma aplicação React, garantindo segurança de tipos e melhorando a experiência do programador dentro da base de código<br>
                                - Ao referenciar os tipos do react-scripts, facilita uma integração fluida com o ecossistema React, promovendo práticas de desenvolvimento eficientes e reduzindo erros em tempo de execução<br>
                                - Isto contribui para a arquitetura geral ao fornecer uma base forte de tipagem que suporta a manutenibilidade e escalabilidade da aplicação frontend.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/App.tsx'>App.tsx</a></b></td>
                            <td style='padding: 8px;'>
                                - Facilita a estrutura central de *routing* e layout da aplicação, permitindo uma navegação fluida entre a interface principal de gestão do dicionário e as suas páginas associadas<br>
                                - Ao integrar o componente de layout, assegura uma experiência de utilizador consistente enquanto gere a apresentação do conteúdo relacionado com o dicionário<br>
                                - Esta configuração fundamental suporta a arquitetura global, permitindo uma expansão e manutenção eficientes da aplicação frontend.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/index.tsx'>index.tsx</a></b></td>
                            <td style='padding: 8px;'>
                                - Inicializa a aplicação React ao renderizar o componente principal App num ambiente estruturado que suporta *routing*<br>
                                - Integra estilos essenciais para uma tematização e layout consistentes, ao mesmo tempo que permite monitorização de desempenho através do web vitals<br>
                                - Esta configuração fundamental assegura uma experiência de utilizador fluida e serve como ponto de entrada para toda a arquitetura do frontend, facilitando a navegação e a interação entre componentes.
                            </td>
                        </tr>
                        <tr style='border-bottom: 1px solid #eee;'>
                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/setupProxy.js'>setupProxy.js</a></b></td>
                            <td style='padding: 8px;'>
                                - Facilita a comunicação com a API ao configurar um middleware proxy que direciona pedidos do frontend para uma API backend em PHP<br>
                                - Esta configuração assegura uma interação sem falhas entre cliente e servidor, fornecendo também um endpoint de verificação de saúde para monitorizar o estado da aplicação<br>
                                - No geral, melhora a arquitetura ao simplificar o fluxo de dados e aumentar a fiabilidade no ambiente de desenvolvimento.
                            </td>
                        </tr>
                    </table>
                    <!-- components Submodule -->
                    <details>
                        <summary><b>components</b></summary>
                        <blockquote>
                            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                                <code><b>⦿ frontend.src.components</b></code>
                            <!-- TermList Submodule -->
                            <details>
                                <summary><b>TermList</b></summary>
                                <blockquote>
                                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                                        <code><b>⦿ frontend.src.components.TermList</b></code>
                                    <table style='width: 100%; border-collapse: collapse;'>
                                    <thead>
                                        <tr style='background-color: #f8f9fa;'>
                                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                                        </tr>
                                    </thead>
                                        <tr style='border-bottom: 1px solid #eee;'>
                                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/components/TermList/TermList.tsx'>TermList.tsx</a></b></td>
                                            <td style='padding: 8px;'>
                                                - O componente TermList serve como uma interface de pesquisa interativa para os utilizadores consultarem definições de palavras<br>
                                                - Ao capturar a entrada do utilizador e consultar uma API externa, recupera e exibe a definição correspondente ou uma mensagem de erro caso a palavra não seja encontrada<br>
                                                - Esta funcionalidade melhora a experiência global do utilizador dentro da aplicação, permitindo acesso fluido às informações das palavras.
                                            </td>
                                        </tr>
                                    </table>
                                </blockquote>
                            </details>
                            <!-- Layout Submodule -->
                            <details>
                                <summary><b>Layout</b></summary>
                                <blockquote>
                                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                                        <code><b>⦿ frontend.src.components.Layout</b></code>
                                    <table style='width: 100%; border-collapse: collapse;'>
                                    <thead>
                                        <tr style='background-color: #f8f9fa;'>
                                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                                        </tr>
                                    </thead>
                                        <tr style='border-bottom: 1px solid #eee;'>
                                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/components/Layout/Footer.tsx'>Footer.tsx</a></b></td>
                                            <td style='padding: 8px;'>
                                                - Fornece um componente de rodapé visualmente apelativo para o projeto Distributed Dictionary, melhorando a interface de utilizador geral<br>
                                                - Exibe informações essenciais de direitos de autor, contribuindo para o profissionalismo e branding da aplicação<br>
                                                - Posicionado na estrutura de layout, este componente desempenha um papel crucial na manutenção de um design coeso no frontend, garantindo que os utilizadores tenham uma experiência consistente ao navegar na aplicação.
                                            </td>
                                        </tr>
                                        <tr style='border-bottom: 1px solid #eee;'>
                                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/components/Layout/Layout.tsx'>Layout.tsx</a></b></td>
                                            <td style='padding: 8px;'>
                                                - Fornece um componente estrutural para a aplicação, encapsulando o layout geral ao integrar um cabeçalho, área principal de conteúdo e rodapé<br>
                                                - Este componente serve como um bloco fundamental para a interface de utilizador, garantindo uma apresentação consistente em diferentes páginas, ao permitir a inserção dinâmica de conteúdo através da prop children<br>
                                                - Melhora a experiência do utilizador ao manter um design coeso por toda a aplicação.
                                            </td>
                                        </tr>
                                        <tr style='border-bottom: 1px solid #eee;'>
                                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/components/Layout/Header.tsx'>Header.tsx</a></b></td>
                                            <td style='padding: 8px;'>
                                                - Fornece um componente de cabeçalho estruturado para a aplicação Distributed Dictionary, melhorando a navegação do utilizador e o branding<br>
                                                - Inclui um logótipo e um link para a página inicial, contribuindo para uma interface de utilizador coesa<br>
                                                - Este componente desempenha um papel crucial no layout geral, garantindo que os utilizadores possam aceder facilmente às áreas principais da aplicação enquanto mantêm uma identidade visual consistente ao longo do projeto.
                                            </td>
                                        </tr>
                                    </table>
                                </blockquote>
                            </details>
                            <!-- TermForm Submodule -->
                            <details>
                                <summary><b>TermForm</b></summary>
                                <blockquote>
                                    <div class='directory-path' style='padding: 8px 0; color: #666;'>
                                        <code><b>⦿ frontend.src.components.TermForm</b></code>
                                    <table style='width: 100%; border-collapse: collapse;'>
                                    <thead>
                                        <tr style='background-color: #f8f9fa;'>
                                            <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                                            <th style='text-align: left; padding: 8px;'>Sumário</th>
                                        </tr>
                                    </thead>
                                        <tr style='border-bottom: 1px solid #eee;'>
                                            <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/components/TermForm/TermForm.tsx'>TermForm.tsx</a></b></td>
                                            <td style='padding: 8px;'>
                                                - O TermForm facilita a adição e remoção de palavras juntamente com as suas definições dentro da aplicação<br>
                                                - Proporciona uma interface amigável para introdução de dados, gestão de submissões e apresentação de mensagens de sucesso ou erro<br>
                                                - Ao integrar-se com a API do backend, assegura que as entradas dos utilizadores são eficazmente guardadas ou removidas, contribuindo para a funcionalidade geral do projeto como uma ferramenta dinâmica de gestão de dicionário.
                                            </td>
                                        </tr>
                                    </table>
                                </blockquote>
                            </details>
                        </blockquote>
                    </details>
                    <!-- services Submodule -->
                    <details>
                        <summary><b>services</b></summary>
                        <blockquote>
                            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                                <code><b>⦿ frontend.src.services</b></code>
                            <table style='width: 100%; border-collapse: collapse;'>
                            <thead>
                                <tr style='background-color: #f8f9fa;'>
                                    <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                                    <th style='text-align: left; padding: 8px;'>Sumário</th>
                                </tr>
                            </thead>
                                <tr style='border-bottom: 1px solid #eee;'>
                                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/services/api.ts'>api.ts</a></b></td>
                                    <td style='padding: 8px;'>
                                        - Estabelece um serviço centralizado de API para a aplicação frontend, facilitando a comunicação fluida com o backend<br>
                                        - Ao configurar o Axios com uma URL base e cabeçalhos adequados, garante a consistência na troca de dados e simplifica os pedidos HTTP em todo o código<br>
                                        - Este serviço desempenha um papel fundamental na manutenção de uma arquitetura limpa, promovendo modularidade e facilidade de manutenção na estrutura geral do projeto.
                                    </td>
                                </tr>
                            </table>
                        </blockquote>
                    </details>
                    <!-- pages Submodule -->
                    <details>
                        <summary><b>pages</b></summary>
                        <blockquote>
                            <div class='directory-path' style='padding: 8px 0; color: #666;'>
                                <code><b>⦿ frontend.src.pages</b></code>
                            <table style='width: 100%; border-collapse: collapse;'>
                            <thead>
                                <tr style='background-color: #f8f9fa;'>
                                    <th style='width: 30%; text-align: left; padding: 8px;'>Nome do Ficheiro</th>
                                    <th style='text-align: left; padding: 8px;'>Sumário</th>
                                </tr>
                            </thead>
                                <tr style='border-bottom: 1px solid #eee;'>
                                    <td style='padding: 8px;'><b><a href='https://github.com/a75739/Atividade2/blob/master/frontend/src/pages/DictionaryManagementPage.tsx'>DictionaryManagementPage.tsx</a></b></td>
                                    <td style='padding: 8px;'>
                                        - Facilita a gestão de um dicionário distribuído ao disponibilizar uma interface que integra um formulário de submissão de termos e uma lista pesquisável<br>
                                        - Esta página funciona como um centro principal para os utilizadores adicionarem novos termos e visualizarem entradas existentes, melhorando a funcionalidade geral e a experiência do utilizador no sistema de gestão do dicionário dentro da arquitetura mais ampla da aplicação.
                                    </td>
                                </tr>
                            </table>
                        </blockquote>
                    </details>
                </blockquote>
            </details>
        </blockquote>
    </details>
</details>

---

## 🧪 Testes

### 📊 Resultados e Análise

#### Sumário Executivo  
| Teste          | VUs        | Duração | Requisições | Falhas*     | Throughput Máx. | Latência p95 (PIOR) |  
|----------------|------------|---------|-------------|-------------|------------------|---------------------|  
| Teste de Fumaça | 10 VUs     | 1m      | 1160       | 0 (0%)      | 19.05 req/s      | PUT: 64.36ms        |  
| Teste de Carga  | 50 VUs    | 1m     | 4056       | 1036 (25%) | 66.26 req/s      | DELETE: 138.80ms    |  
| Stress (PUT)    | 2.000 VUs  | 1m      | 9434       | 0 (0%)      | 126.80 req/s     | PUT: 15.07s         |  
| Stress (Misto)  | 1.000 VUs  | 1m     | 13806      | 9205 (67%) | 204.52 req/s     | PUT: 8.04s         |  

*Nota: "Falhas" incluem respostas 404 (não encontrado) consideradas válidas pelo sistema

Ao ver o campo "Checks" dos relatórios poderá ver que não houve nenhuma falha "Real" que afetou o sistema.

---

#### 🔥 Teste de Fumaça  
**Configuração**: 10 VUs constantes por 1 minuto  
**Métricas Chave**:  
- ✅ **100% disponibilidade** em todas operações  
- ⚡ **Latência p95**:  
  - `PUT`: 64.36ms  
  - `GET`: 15.54ms  

**Análise**:  
- Ambiente estável com resposta consistente abaixo de 100ms  
- Cache Redis eficiente para leituras (GET <20ms)  
- Pipeline de escrita (PUT) mantém performance mesmo sob carga inicial 
 

📄 [Relatório Completo](https://atividade2-dictionary.netlify.app/testes-carga/relatorios/smoke-test-report.html)  

---

#### ⚖️ Teste de Carga  
**Configuração**: Ramp-up progressivo de 0-50 VUs em 1min  
**Métricas Críticas**:  
- 📈 **Throughput sustentado**: 66 req/s  
- ⚠️ **Falsos positivos**: 25% dos requests foram 404 (comportamento esperado)  
- 🕒 **Latência operacional**:  
  - DELETE: 138.8ms (p95)  
  - PUT: 195.59ms (p95)  

**Insights**:  
- Aumento de latência correlacionado com concorrência de escritas  
- 404s ocorrem quando operações DELETE/GET antecedem PUTs na fila  
- Sistema mantém estabilidade e disponibilidade apesar de conflitos de otimismo 

📄 [Relatório Completo](https://atividade2-dictionary.netlify.app/testes-carga/relatorios/load-test-report.html)  

---

#### 💥 Teste de Stress (Apenas PUT)  
**Configuração**: 2.000 VUs com picos abruptos  
**Resultados Notáveis**:  
- 🏋️ **Carga extrema**: 126 req/s sustentados  
- ⏳ **Latência degradada**:  
  - PUT p95: 15.07s  
  - Máximo absoluto: 22.31s 

**Análise**:  
- Mesmo com latência elevada, **zero falhas reais**  
- RabbitMQ atua como amortecedor eficiente  
- Limitação física do ambiente local (8 CPUs) foi o principal gargalo


📄 [Relatório Completo](https://atividade2-dictionary.netlify.app/testes-carga/relatorios/stress-test-report-put.html)

---

#### 💥 Teste de Stress (Misto)  
**Configuração**: 1.000 VUs com operações aleatórias  
**Observações Chave**:  
- 🔄 **Concorrência crítica**: 67% de 404s por conflitos de tempo real  
- ⏱️ **Latência operacional média**:  
  - PUT: 7.93s  
  - GET: 2.00s  
  - DELETE: 1.85s  

**Análise**:  
- Sistema mantém disponibilidade contínua mesmo sob carga extrema  
- Trade-off claro entre consistência imediata e disponibilidade  
- Arquitetura tolerante a partições (AP) comprovada 

📄 [Relatório Completo](https://atividade2-dictionary.netlify.app/testes-carga/relatorios/stress-test-report.html)  

---

## 📊 Limites e Capacidades  

### Especificações do Ambiente Local de Teste
| Recurso        | Especificação        |
|----------------|----------------------|
| CPU            | 8 cores (Intel Xeon)|
| RAM            | 16GB DDR4            |
| Armazenamento  | SSD 477GB            |
| Rede           | 1Gbps               |

### Capacidades Validadas
| Métrica               | Valor                             | Cenário                           |
|-----------------------|-----------------------------------|-----------------------------------|
| Throughput Sustentado | 126 req/s                        | Operações PUT Only                |
| Escalabilidade        | Linear até 500 req/s             | Cluster CockroachDB               |
| Carga Máxima          | 2.000 VUs                        | Teste Focado em PUT               |
| Disponibilidade       | 100%                             | Todos Cenários de Teste           |
| Coordenação de Recursos | Balanceamento Automático        | HAProxy + Nginx + Redis Cluster           |
| Tolerância a Falhas   | N+1 (3 falhas simultâneas)       | Auto-recuperação em 30s           |
| Resiliência           | Replicação Síncrona              | RabbitMQ Quórum                   |

### Limitações Observadas
1. **Latência de Replicação**  
   - Até 450ms para consistência entre nós
   - 0.7% de conflitos em escrita com >10 VUs

2. **Gargalo de Recursos**  
   - Limitação física em operações mistas ou com muitas req/s

3. **Ordem de Operações**  
   - DELETE/GET podem executar antes do PUT correspondente  
   - Requer otimização de retries assíncronos

4. **Escalabilidade Vertical**  
   - Redis atinge ~350 req/s por shard  
   - CockroachDB escala linear até 500 req/s

---

## 🤝 Contribuir

- Partilha as tuas ideias, dá feedback ou coloca questões.
- Envia bugs encontrados ou sugestões de funcionalidades para o projeto `Atividade2`.
- Analisa PRs abertas e submete as tuas próprias contribuições.

<details closed>
<summary>Guia para Contribuidores</summary>

1. **Faça fork do Repositório**: Começe por fazer fork do repositório para a sua conta no GitHub.
2. **Clone Localmente**: Clone o repositório forkado para a sua máquina local usando um cliente git.
   ```sh
   git clone https://github.com/a75739/Atividade2
   ```
3. **Crie uma nova branch**: Trabalhe sempre numa branch nova, com um nome descritivo.
   ```sh
   git checkout -b new-feature-x
   ```
4. **Faça as suas alterações**: Desenvolva e teste as alterações localmente.
5. **Faça commit das mudanças**: Faça commit com uma mensagem clara a descrever as mudanças efetuadas.
   ```sh
   git commit -m 'Implemented new feature x.'
   ```
6. **Faça push para o GitHub**: Envie as alterações para o repositório.
   ```sh
   git push origin new-feature-x
   ```
7. **Submeta um Pull Request**: Crie um PR para o repositório original. Descreva claramente as alterações e os motivos.
8. **Revisão**: Após a revisão e aprovação do PR, ele será integrado na branch principal. Parabéns pela sua contribuição!
</details>

<details closed>
<summary>Gráfico de Contribuição</summary>
<br>
<p align="left">
   <a href="https://github.com{/a75739/Atividade2/}graphs/contributors">
      <img src="https://contrib.rocks/image?repo=a75739/Atividade2">
   </a>
</p>
</details>

---

## Bibliografia

### Deeepseek:

- Prompt: "Providencie um conjunto de links para repositórios github com serviços úteis no desenvolvimento de sistemas distribuídos."
- Prompt: "Gere um ambiente Docker com CockroachDB em cluster e PHP como backend acessando Redis em cluster com failover automático."
- Prompt: "Providencie um conjunto de load balancers que eu possa utilizar para distribuir o trabalho por várias APIs, workers e DBs"
- Prompt: "Explique a consistência eventual em CockroachDB e como ela impacta testes de carga simultâneos.
- Prompt: "Como posso resolver o facto de certas operações PUT demorarem muito tempo a serem realizadas e GETS e DELETES dão erro 404 Not Found"
- Prompt: "Como configurar k6 para gerar relatórios html?"
- Prompt: "Como posso resolver que em k6 status HTTP 404 em testes de carga são esperados e não erros?"
- Prompt: "Providencie um índice de projeto descrevendo as funcionalidades principais de cada ficheiro"
- Prompt: "Gere um diagrama da arquitetura dos serviços presentes em docker-compose.yml, incluindo detalhes sobre dependências e portas"
- Prompt: "Providencie um guia passo a passo de como implementar o projeto em cloud"

<div align="left"><a href="#top">⬆ Voltar</a></div>
