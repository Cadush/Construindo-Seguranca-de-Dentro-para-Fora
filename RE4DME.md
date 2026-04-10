# 🔐 Construindo Segurança de Dentro para Fora

> *"Segurança se constrói de dentro para fora."*
> — Analistas, engenheiros e especialistas em segurança da informação

---

## 📌 Contexto

Segundo a **Cybersecurity Insiders 2024** (citado pela IBM), **83% das organizações sofreram pelo menos um ataque interno**. Esse dado reforça que proteger exclusivamente o perímetro já não é suficiente.

Uma arquitetura de segurança eficaz deve ser concebida como um **ecossistema integrado**, combinando pessoas, processos e tecnologia — baseada em risco, visibilidade e resiliência.

### Pilares de uma arquitetura de segurança

| Pilar | Descrição |
|---|---|
| Governança e Gestão de Riscos | Estrutura de controle e tomada de decisão |
| IAM | Gestão de identidade e acesso |
| Segurança de Endpoints | Proteção de dispositivos e servidores |
| Segurança de Rede e Perímetro | Controle de tráfego e fronteiras |
| Segurança em Nuvem e Aplicações | Proteção de workloads e APIs |
| Monitoramento, Detecção e Resposta | Visibilidade e reação a incidentes |
| Cultura e Conscientização | Pessoas como parte da defesa |
| Gestão de Vulnerabilidades | Identificação e correção contínua |
| Continuidade e Resiliência | Capacidade de recuperação |

---

## 🎯 Objetivo

Este artigo analisa **práticas de hardening** em ambientes **Microsoft Active Directory** e **Linux**, com foco em:

- Evidenciar vulnerabilidades comuns
- Apresentar ferramentas de auditoria
- Reduzir a superfície de ataque
- Aumentar a maturidade de segurança

> O objetivo **não** é guiar uma metodologia completa de pentest, mas expor vulnerabilidades e demonstrar a importância de auditorias internas.

Para exemplificar a exploração de falhas em Active Directory, é utilizado como base o laboratório CTF **"Attacktive Directory"**.

---

## 🔍 Fase 1 — Reconhecimento e Exploração

### NMAP — Varredura de Rede

Identificação de portas abertas, serviços ativos e versões expostas no host alvo.

<img width="514" height="341" alt="Resultado do NMAP" src="https://github.com/user-attachments/assets/0aff80de-95da-483a-9e79-66d78f129dc3" />

---

### Enum4linux — Enumeração SMB

Ferramenta baseada em Perl para enumerar informações de sistemas Windows via SMB: usuários, grupos, compartilhamentos e dados do SAM (Security Account Manager).

<img width="650" height="456" alt="Resultado do Enum4linux" src="https://github.com/user-attachments/assets/0f5f49f1-8388-4a05-874c-c76fe5fcd9aa" />

---

### ldapsearch — Consulta LDAP

Coleta e enumeração de informações do domínio: contas de usuários, grupos, atributos e estrutura do diretório.

<img width="581" height="415" alt="Resultado do ldapsearch" src="https://github.com/user-attachments/assets/541bbe52-2201-45b9-a772-fa7427f7c71a" />

---

### Impacket — Validação de Contas

Validação de contas de usuários válidos no domínio por meio da suíte Impacket.

<img width="1314" height="323" alt="Resultado do Impacket" src="https://github.com/user-attachments/assets/a93e4510-63da-4dfc-8a93-2dece28ece14" />

---

### Evil-WinRM — Acesso Remoto

Estabelecimento de sessão remota no host alvo via WinRM com credenciais obtidas.

<img width="396" height="52" alt="Sessão Evil-WinRM" src="https://github.com/user-attachments/assets/a4ed3ee6-9796-4519-a0e2-03b3c3644676" />

---

### 🚨 Fragilidades Identificadas no Laboratório

- Enumeração de usuários **sem autenticação**
- Políticas de senha **fracas**
- Exposição de **serviços críticos**

Essas falhas permitiram a progressão do ataque até a obtenção de acesso remoto ao host — sem exploits complexos, apenas má configuração.

---

## 🛡️ Fase 2 — Auditoria de Segurança com PingCastle

O **PingCastle** é uma ferramenta de auditoria focada em Active Directory. Analisa a infraestrutura do domínio e gera relatórios detalhados com riscos, vulnerabilidades e más práticas.

### Módulos disponíveis

| Opção | Função |
|---|---|
| `healthcheck` | Varredura completa do AD com score de risco |
| `entraid` | Análise do Microsoft Entra ID (Azure AD) |
| `conso` | Consolida múltiplos relatórios em um só |
| `carto` | Mapa de relações e trusts entre domínios |
| `scanner` | Verificações de segurança em workstations |
| `export` | Exporta usuários e computadores do AD |
| `advanced` | Menu avançado com opções técnicas extras |

---

### Healthcheck — Análise Principal

A opção `healthcheck` realiza uma varredura abrangente no AD, analisando:

- Políticas de senha
- Delegações perigosas
- Contas privilegiadas
- Configurações inseguras (Kerberos, NTLM)
- Relações de confiança (trusts)

<img width="1338" height="385" alt="PingCastle em execução" src="https://github.com/user-attachments/assets/e8a5c8f6-ddf1-4908-bc5f-218c70342a25" />

Durante a execução, serão solicitadas **credenciais com privilégios administrativos** e o **domínio alvo** (ex: `empresa.local`).

---

### Relatórios Gerados

Ao final, o PingCastle gera relatórios em dois formatos:

- 📄 **HTML** — Relatório interativo e detalhado para análise técnica
- 🧾 **PDF** — Versão consolidada para documentação executiva

**Boas práticas de nomenclatura:**
```
AD-Healthcheck_empresa.local_2026-03-25.html
AD-Healthcheck_empresa.local_2026-03-25.pdf
```
Isso facilita versionamento, rastreabilidade e comparação entre análises ao longo do tempo.

---

### Painel de Riscos

<img width="925" height="490" alt="Painel de riscos PingCastle" src="https://github.com/user-attachments/assets/0307a22f-6ef3-43cb-be28-21a2672a9e5f" />

As cores indicam a gravidade dos achados:

| Cor | Nível |
|---|---|
| 🟦 Azul | Informativo / Baixo risco |
| 🟨 Amarelo | Atenção |
| 🟧 Laranja | Risco relevante |
| 🟥 Vermelho | Risco crítico |

---

## 📋 Plano de Resposta à Auditoria (GRC)

O responsável por GRC deve estruturar o plano de resposta em ciclo contínuo:

1. **Classificação de riscos** — Priorização por criticidade e impacto ao negócio
2. **Validação técnica** — Verificação prática para reduzir falsos positivos
3. **Análise de impacto real** — Avaliação de cenários de ataque e consequências operacionais
4. **Plano de correção e hardening** — Ações corretivas alinhadas às boas práticas
5. **Reexecução e melhoria contínua** — Validação das correções e evolução da maturidade

---

## 📚 Referências e Leituras Recomendadas

Para aprofundamento no tema:

- *The Shellcoder's Handbook* — Fundamentos de exploração
- *Hacking: The Art of Exploitation* — Técnicas de ataque
- *Windows Internals* — Funcionamento interno do Windows
- *The Rootkit Arsenal* — Técnicas avançadas de persistência

---

## 🏁 Conclusão

A análise evidencia que vulnerabilidades críticas em ambientes Active Directory **não estão necessariamente associadas a falhas complexas**, mas a más práticas recorrentes:

- Excesso de privilégios
- Uso de protocolos legados
- Ausência de monitoramento efetivo
- Gestão inadequada de identidades

> Ferramentas como **PingCastle** e **Lynis** são instrumentos de visibilidade — sua eficácia depende da capacidade analítica dos profissionais envolvidos.

A construção de um ambiente seguro não depende exclusivamente de tecnologias avançadas, mas da **integração entre pessoas, processos e ferramentas**, aliada a uma cultura organizacional orientada à segurança.

**A segurança deixa de ser apenas uma camada técnica e passa a assumir um papel estratégico dentro das organizações.**

---

> 📎 *Artigo publicado originalmente no [LinkedIn](https://www.linkedin.com)*
> 
> ⚠️ *Todo o conteúdo técnico foi executado em ambiente controlado (CTF). Não utilize estas técnicas em sistemas sem autorização explícita.*
