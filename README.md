1- Criação e Configuração da VM: Começo e monitoramento.
2- Configuração do Azure Monitor: A ferramenta central para coletar e analisar dados.
3- Criação de Alertas para Eventos Críticos: O foco na exclusão da VM.
4- Teste e Validação: Confirmando que tudo funciona como esperado.
5- Criação do Repositório: Seu entregável final.

1. Criação e Configuração da VM
Primeiro, você precisará de uma VM para monitorar.
Crie uma Máquina Virtual (VM):
  1 - No portal do Azure, procure por "Máquinas Virtuais" e clique em "Criar".
  2 - Escolha um grupo de recursos existente ou crie um novo (ex: rg-monitoramento-vm).
  3 - Selecione uma região, nome da VM (ex: minhavm-teste), imagem (ex: Windows Server 2019 Datacenter ou Ubuntu Server 20.04 LTS), tamanho e configure as credenciais de administrador.
  4 - Não se preocupe muito com as configurações de rede ou disco neste momento, a não ser que você queira testar algo específico, mas o básico já serve para o monitoramento.
  5 - Revise e crie a VM.

2. Configuração do Azure Monitor
O Azure Monitor é o serviço nativo para coletar, analisar e atuar sobre dados de telemetria de seus recursos.
Explore as Métricas da VM:
  1 - Após a VM ser provisionada, vá para o recurso da VM no portal do Azure.
  2 - No menu lateral, em "Monitoramento", clique em "Métricas".
  3 - Explore as métricas disponíveis, como CPU Usage, Network In, Network Out, Disk Read Operations, Disk Write Operations, etc. Isso te dará uma ideia do que o Azure já coleta automaticamente.
    OBS: (Quais métricas são mais relevantes para o desempenho e a saúde de uma VM?)
  4 - Configure o Log Analytics Workspace (Opcional, mas Recomendado):
  5 - Para ter logs mais detalhados e poder executar consultas complexas (KQL - Kusto Query Language), é altamente recomendado configurar um Log Analytics Workspace.
  6 - No portal, procure por "Workspaces do Log Analytics" e crie um novo (ex: law-monitoramento-vm).
  7 - Após criar, volte para sua VM, vá em "Configurações de diagnóstico" (ou "Extensões" e adicione a extensão Log Analytics). Conecte a VM ao seu Log Analytics Workspace recém-criado.

3. Criação de Alertas para Eventos Críticos
O objetivo principal aqui é ser proativo. Vamos criar um alerta para um evento muito específico: a exclusão da VM.
Crie um Alerta de Log de Atividades para Exclusão de VM:
  1 - No portal do Azure, procure por "Azure Monitor" e, no menu lateral, clique em "Alertas".
  2 - Clique em "Criar" e depois em "Regra de alerta".
  3 - Escopo:
       - Clique em "Selecionar escopo".
       - Você pode selecionar o grupo de recursos onde a VM está (ex: rg-monitoramento-vm) ou a própria VM. Para monitorar qualquer exclusão de VM dentro de um grupo de recursos, selecione o grupo de recursos. Para este exercício, selecione o grupo de recursos.
       - Clique em "Concluído".
  4 - Condição:
       - Clique em "Adicionar condição".
       - Em "Tipo de sinal", selecione "Log de Atividades".
       - Em "Nome da operação", selecione "Deletar máquina virtual (microsoft.compute/virtualmachines/delete)". Você pode precisar rolar ou pesquisar.
       - Em "Status do evento", selecione "Bem-sucedido".
       - Clique em "Concluído".
  5 - Ações:
       - Clique em "Adicionar grupo de ações".
       - Um grupo de ações define o que acontece quando o alerta é disparado (enviar e-mail, SMS, chamar um webhook, etc.).
       - Crie um novo grupo de ações (ex: ag-notificacao-vm).
       - Na aba "Noções básicas", dê um nome ao grupo de ações.
       - Na aba "Notificações", adicione uma ação de "Email/SMS/Push/Voz".
       - Selecione "Email" e insira seu endereço de e-mail. Dê um nome para a notificação (ex: Email de Exclusão de VM).
       - Revise e crie o grupo de ações.
       - Selecione o grupo de ações recém-criado e clique em "Selecionar grupo de ações".
       - Detalhes da regra de alerta:
       - Dê um nome à regra de alerta (ex: Alerta-Exclusao-VM).
       - Escolha um grupo de recursos para armazenar a regra de alerta (pode ser o mesmo da VM ou um dedicado para monitoramento).
       - Verifique se o "Estado de habilitar a regra de alerta após a criação" está como "Sim".
       - Clique em "Revisar + criar" e depois em "Criar".

4. Teste e Validação
Como ver se o monitoramento está funcionando.
Exclua a VM:
  1 - Volte para o recurso da sua Máquina Virtual.
  2 - Clique em "Excluir" na barra superior e confirme a exclusão.
  3 - Verifique o Alerta:
      - Aguarde alguns minutos.
  4 - Verifique sua caixa de entrada de e-mail para a notificação do alerta.
  5 - No portal do Azure, vá para "Azure Monitor" > "Alertas" e verifique o histórico de alertas. Você deverá ver o alerta disparado.
