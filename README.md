# Construindo-Segurança-de-Dentro-para-Fora
Analisando o cenário atual, é possível compreender que os vetores de ataques internos ainda existem. Segundo a Cybersecurity Insiders 2024 (citado pela IBM), 83% das organizações sofreram pelo menos um ataque interno. Ressalta-se, portanto, um ponto frequentemente destacado por analistas, engenheiros e especialistas: “Segurança se constrói de dentro para fora”.
Muito se discute sobre as etapas da segurança da informação em ambientes corporativos; entretanto, uma arquitetura de segurança eficaz deve ser concebida como um ecossistema integrado, que combina pessoas, processos e tecnologia. Mais do que a adoção de controles isolados, trata-se de uma abordagem estruturada, baseada em risco, visibilidade e resiliência.
Nesse contexto, uma arquitetura de segurança deve contemplar, no mínimo, os seguintes pilares: governança e gestão de riscos, gestão de identidade e acesso (IAM), segurança de endpoints e dispositivos, segurança de rede e perímetro, segurança em nuvem e aplicações, monitoramento, detecção e resposta, cultura e conscientização, gestão de vulnerabilidades e testes, além de continuidade e resiliência.
Uma arquitetura de segurança não deve ser encarada apenas como um conjunto de ferramentas, mas como uma estratégia contínua e integrada. Em um cenário no qual ameaças internas representam uma parcela significativa dos incidentes, proteger exclusivamente o perímetro já não é suficiente. A maturidade em segurança está diretamente relacionada à capacidade da organização de controlar identidades, monitorar comportamentos e fomentar uma cultura na qual cada usuário também atue como parte da defesa.
Com base nessa visão, o presente artigo tem como objetivo analisar práticas de hardening (endurecimento) de sistemas. Atualmente, o Microsoft Active Directory e sistemas Linux configuram-se como pilares na estrutura de servidores e na gestão de identidades em ambientes corporativos, estando cada vez mais presentes em organizações de diversos portes. No entanto, muitas vezes, esses ambientes não recebem as devidas tratativas no âmbito da segurança da informação.
Dessa forma, este estudo busca evidenciar vulnerabilidades comuns, bem como apresentar o uso de ferramentas de auditoria e boas práticas de hardening, com foco na redução da superfície de ataque e no aumento da maturidade de segurança.
Para melhor exemplificar a exploração de falhas em um ambiente Active Directory, será utilizado como base, para a narrativa técnica, um laboratório de Capture The Flag (CTF), especificamente o “Attacktive Directory”.
Embora este estudo tenha como foco ambientes Active Directory, é importante destacar que sistemas Linux também desempenham papel crítico na infraestrutura corporativa e estão igualmente sujeitos a falhas de configuração e vulnerabilidades. A análise de segurança nesses ambientes será abordada em um estudo complementar, com foco no uso da ferramenta Lynis.

O objetivo deste trabalho não é guiar uma metodologia completa de pentest, mas sim expor vulnerabilidades identificadas no ambiente. Para isso, o conteúdo será dividido em duas etapas principais: análise e exploração. Na sequência, prosseguimos para um aprofundamento no estudo de auditoria de segurança.
Reconhecimento com NMAP:











<img width="514" height="341" alt="image" src="https://github.com/user-attachments/assets/0aff80de-95da-483a-9e79-66d78f129dc3" />









O Enum4linux é uma ferramenta de linha de comando baseada em Perl, amplamente utilizada em auditorias de segurança para enumerar informações de sistemas Windows via SMB, incluindo usuários, grupos, compartilhamentos e dados do serviço SAM (Security Account Manager).
<img width="650" height="456" alt="image" src="https://github.com/user-attachments/assets/0f5f49f1-8388-4a05-874c-c76fe5fcd9aa" />




O ldapsearch é uma ferramenta de linha de comando usada para realizar consultas em serviços LDAP, possibilitando a coleta e enumeração de informações de um domínio, como contas de usuários, grupos, atributos e estrutura do diretório.
<img width="581" height="415" alt="image" src="https://github.com/user-attachments/assets/541bbe52-2201-45b9-a772-fa7427f7c71a" />


Iniciamos o processo de validação de contas de usuários válidos por meio das ferramentas da suíte Impacket. 
<img width="1314" height="323" alt="image" src="https://github.com/user-attachments/assets/a93e4510-63da-4dfc-8a93-2dece28ece14" />




Por fim, foi utilizada a ferramenta Evil-WinRM para estabelecer uma sessão remota no host alvo via WinRM.
<img width="396" height="52" alt="image" src="https://github.com/user-attachments/assets/a4ed3ee6-9796-4519-a0e2-03b3c3644676" />


No laboratório utilizado, foram identificadas fragilidades como enumeração de usuários sem autenticação, políticas de senha fracas e exposição de serviços críticos, permitindo a progressão do ataque até a obtenção de acesso remoto ao host.
Para aprofundamento no tema, recomenda-se que o leitor busque uma base sólida em sistemas operacionais, aliada ao estudo de exploração de vulnerabilidades e à prática em ambientes controlados. Materiais clássicos como The Shellcoder’s Handbook e Hacking: The Art of Exploitation fornecem fundamentos importantes sobre exploração, enquanto obras como Windows Internals e The Rootkit Arsenal permitem compreender o funcionamento interno dos sistemas.
Partindo do entendimento de que já possuímos uma visão clara de algumas vulnerabilidades, o objetivo não é se aprofundar na exploração em si, mas compreender os motivos e a importância da realização de auditorias internas de segurança em servidores.
Vale ressaltar que o mito de que o Linux é impenetrável não se sustenta. Assim como qualquer outro sistema operacional, ele pode apresentar vulnerabilidades exploráveis. É possível, por exemplo, identificar e explorar falhas relacionadas a serviços como SMB (protocolo de compartilhamento de arquivos), entre outros, quando não configurados adequadamente.
A partir deste ponto, será aprofundada a análise de auditoria de segurança em ambientes Active Directory, com foco na identificação de vulnerabilidades e na aplicação de boas práticas de hardening.



O PingCastle é uma ferramenta de auditoria de segurança focada em ambientes Active Directory (AD). Ele analisa a infraestrutura do domínio e gera relatórios detalhados com riscos, vulnerabilidades e más práticas, ajudando equipes de segurança a identificar pontos fracos rapidamente.
1 – healthcheck (score the risk of a domain)
Essa é a função principal do PingCastle.
    • Faz uma varredura completa no Active Directory 
    • Analisa: 
        ◦ Políticas de senha 
        ◦ Delegações perigosas 
        ◦ Contas privilegiadas 
        ◦ Configurações inseguras (Kerberos, NTLM, etc.) 
    • Gera um relatório HTML com: 
        ◦ Score de risco 
        ◦ Recomendações de correção

2 – entraid (score the risk of Entra ID)
Analisa o ambiente Microsoft Entra ID (antigo Azure AD).
    • Avalia segurança da identidade na nuvem 
    • Verifica: 
        ◦ MFA 
        ◦ Políticas de acesso condicional 
        ◦ Contas privilegiadas 
    • Também gera score de risco 
Ideal para empresas híbridas (AD + Cloud).
3 – conso (aggregate multiple reports into a single one)
Consolida vários relatórios em um só.
    • Junta resultados de múltiplos domínios ou execuções 
    • Permite visão centralizada 
 Útil para:
    • Ambientes grandes 
    • Consultorias (vários clientes/domínios) 
4 – carto (build a map of all interconnected domain)
Cria um mapa de relações entre domínios.
    • Mostra trusts entre domínios/forest 
    • Ajuda a entender caminhos de ataque lateral 
 Muito útil para:
    • Pentest interno 
    • Análise de movimentação lateral (lateral movement) 
5 – scanner (perform specific security checks on workstations)
Faz verificações específicas em máquinas (workstations/servers).
    • Analisa configurações locais 
    • Pode identificar: 
        ◦ Problemas de segurança 
        ◦ Configurações fracas 
Complementa o AD com visão de endpoints.
6 – export (export users or computers)
Exporta dados do AD.
    • Lista: 
        ◦ Usuários 
        ◦ Computadores 
    • Pode ser usado para: 
        ◦ Auditoria 
        ◦ Scripts 
        ◦ Integração com outras ferramentas 
7 – advanced (open the advanced menu)
Menu avançado com funções extras.
    • Opções mais específicas e técnicas 
    • Usado por quem já tem mais experiência 
Normalmente não é necessário no início.
0 – exit
Sai da ferramenta.
<img width="1338" height="385" alt="image" src="https://github.com/user-attachments/assets/e8a5c8f6-ddf1-4908-bc5f-218c70342a25" />


Para iniciar a análise completa do ambiente, será utilizada a opção Healthcheck, responsável por realizar uma varredura abrangente no Active Directory, identificando vulnerabilidades, más práticas e riscos de segurança.
Durante a execução:
    • Privilégios elevados serão requisitados
A ferramenta solicitará credenciais com permissões administrativas (preferencialmente um usuário com privilégios de leitura completos no domínio), garantindo acesso às informações necessárias para uma análise precisa. 
    • Definição do domínio alvo
Será solicitado o domínio a ser avaliado (ex: empresa.local), que servirá como escopo da varredura. 
    • Processo de análise automatizada
O PingCastle realizará a coleta e correlação de dados relacionados a: 
        ◦ Contas privilegiadas 
        ◦ Políticas de senha 
        ◦ Configurações de autenticação (Kerberos/NTLM) 
        ◦ Relações de confiança (trusts) 
        ◦ Delegações e permissões críticas 
Geração de Relatórios
Ao final da execução, serão gerados relatórios nos seguintes formatos:
    • 📄 HTML – Relatório interativo e detalhado para análise técnica 
    • 🧾 PDF – Versão consolidada, ideal para documentação e apresentação executiva 
 Boas práticas:
Recomenda-se salvar os relatórios utilizando um padrão de nomenclatura com data da execução, por exemplo:
AD-Healthcheck_empresa.local_2026-03-25.html
AD-Healthcheck_empresa.local_2026-03-25.pdf
Isso facilita o versionamento, rastreabilidade e comparação entre análises ao longo do tempo.
 <img width="925" height="490" alt="image" src="https://github.com/user-attachments/assets/0307a22f-6ef3-43cb-be28-21a2672a9e5f" />


Esse painel mostra categorias de risco divididas em cores e tópicos:
As cores indicam gravidade:
    • 🟦 Azul → informativo / baixo risco 
    • 🟨 Amarelo → atenção 
    • 🟧 Laranja → risco relevante 
    • 🟥 Vermelho → risco crítico 

O responsável pela área de Governança, Riscos e Compliance (GRC) deve ser capaz de estruturar um plano de resposta à auditoria de forma organizada e orientada a risco. Esse processo não deve se limitar à análise superficial dos resultados, mas sim envolver uma abordagem crítica e contínua, composta pelas seguintes etapas:
    • Classificação de riscos: priorização dos achados com base em sua criticidade e potencial de impacto para o negócio; 
    • Validação técnica: verificação prática das vulnerabilidades identificadas, evitando a dependência exclusiva das ferramentas e reduzindo falsos positivos; 
    • Análise de impacto real: avaliação das possíveis consequências de exploração, considerando cenários de ataque e impacto operacional; 
    • Plano de correção e hardening: definição de ações corretivas, mitigatórias e de fortalecimento do ambiente, alinhadas às boas práticas de segurança; 
    • Reexecução e melhoria contínua: nova execução dos testes para validação das correções e acompanhamento da evolução da maturidade de segurança. 
Dessa forma, o processo de auditoria deixa de ser pontual e passa a atuar como um ciclo contínuo de melhoria, essencial para a sustentação da segurança da informação no ambiente corporativo.
A análise conduzida evidencia que a segurança em ambientes corporativos, especialmente aqueles baseados em Active Directory, com considerações aplicáveis a outros sistemas, ainda apresenta fragilidades significativas, muitas vezes decorrentes de falhas de configuração, ausência de governança contínua e baixa maturidade em processos de controle.
Os resultados obtidos por meio das ferramentas de auditoria demonstram que vulnerabilidades críticas não estão necessariamente associadas a falhas complexas, mas sim a más práticas recorrentes, como excesso de privilégios, uso de protocolos legados, ausência de monitoramento efetivo e gestão inadequada de identidades. Tais fatores reforçam que o risco interno — seja ele intencional ou acidental — permanece como um dos principais vetores de comprometimento.
Nesse contexto, destaca-se que ferramentas como o PingCastle e o Lynis não devem ser encaradas como soluções definitivas, mas como instrumentos de visibilidade, cuja eficácia depende diretamente da capacidade analítica dos profissionais envolvidos. A auditoria de segurança, portanto, não se limita à execução de ferramentas, mas exige um processo estruturado que envolva validação técnica, análise de impacto e priorização baseada em risco.
Adicionalmente, torna-se evidente que a maturidade em segurança da informação está intrinsicamente ligada à adoção de uma abordagem contínua e cíclica, composta por avaliação, correção, validação e reavaliação. Esse ciclo permite não apenas a mitigação de vulnerabilidades existentes, mas também a evolução progressiva do nível de segurança da organização.
Por fim, conclui-se que a construção de um ambiente seguro não depende exclusivamente de tecnologias avançadas, mas da integração entre pessoas, processos e ferramentas, aliada a uma cultura organizacional orientada à segurança. Em um cenário onde os ataques internos são cada vez mais relevantes, a capacidade de controlar identidades, reduzir privilégios e monitorar comportamentos torna-se um diferencial estratégico para a proteção dos ativos críticos.
Dessa forma, a segurança deixa de ser apenas uma camada técnica e passa a assumir um papel estratégico dentro das organizações, sendo um fator determinante para a continuidade e confiabilidade dos negócios.

