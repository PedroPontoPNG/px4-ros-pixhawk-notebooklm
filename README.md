# Caderno Temático NotebookLM: Integração PX4, Pixhawk e Ecossistema ROS

> **Status do Projeto:** Concluído

## 🎯 Contexto e Objetivos

Este repositório serve como um portfólio técnico e guia de estudo focado na integração vertical de robótica aérea e móvel autónoma. O objetivo principal deste caderno temático, desenvolvido com o apoio do **Google NotebookLM**, é consolidar o conhecimento necessário para transitar desde a montagem física e calibração do hardware de controlo de voo até à implementação de algoritmos avançados de navegação e simulação de software.

**Objetivos de Estudo:**
* Compreender os passos essenciais de montagem e calibração do controlador Pixhawk.
* Analisar a transição de arquitetura, workspaces e padrões de comunicação entre o ROS 1 e o ROS 2.
* Dominar o ecossistema de firmware PX4, focando na compilação do código-fonte, ambientes de simulação (SITL) e técnicas de debugging.
* Estudar a arquitetura do stack Nav2 para o planeamento de caminhos e desvio de obstáculos através de Árvores de Comportamento (Behavior Trees).

---

## 📚 Curadoria de Fontes

Para alimentar a base de conhecimento (grounding) do NotebookLM, foram selecionadas e estruturadas as seguintes fontes abertas em formato PDF:

1. **Pixhawk Kit User Guide V8 (3D Robotics)**
   * *Foco técnico:* Manual prático detalhando os componentes do kit (FMU, Buzzer, Safety Switch, Power Module) e o fluxo inicial de montagem, conexão de periféricos, carga de firmware e calibração.
   * 📁 *Disponível em: `https://www.rcworld.co.za/downloads/pixhawk.pdf`*
2. **Robot Operating System (ROS) – Prof. Walter Fetter Lages (UFRGS)**
   * *Foco técnico:* Introdução detalhada ao ROS 1 (conceitos de nós, tópicos, serviços e `roscore`) em contraste direto com as evoluções arquiteturais do ROS 2 (DDS, ausência de mestre centralizado e gestão de ciclo de vida de nós).
   * 📁 *Disponível em: `https://www.ece.ufrgs.br/~fetter/eng10051/ros.pdf`*
3. **Navegação no ROS 2 Nav2 – Prof. Walter Fetter Lages (UFRGS)**
   * *Foco técnico:* Documentação exaustiva sobre o stack de navegação autónoma Nav2, explicando o funcionamento dos servidores de ação (`planner_server`, `controller_server`), mapas de custo (`Costmap 2D`) e workspaces de simulação em Gazebo.
   * 📁 *Disponível em: `https://www.ece.ufrgs.br/~fetter/cca99006/ros2_nav2.pdf`*
4. **Getting Started with PX4 For Contributors – Mark West**
   * *Foco técnico:* Guia voltado para programadores focado nos níveis de contribuição do ecossistema PX4, compilação de código através de Toolchains ou instâncias Docker, simulação SITL (Software-in-the-Loop) com Gazebo/AirSim e debugging através do VSCode.
   * 📁 *Disponível em: `[/fontes/Getting-Started-as-a-contributor-on-PX4.pdf](https://px4.io/wp-content/uploads/2020/07/Getting-Started-as-a-contributor-on-PX4.pdf)`*
  
   ## 🛠️ Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Abaixo estão registadas as iterações, falhas e refinamentos estratégicos realizados no NotebookLM para extrair informações precisas dos manuais técnicos, demonstrando o processo de engenharia reversa e o raciocínio por trás dos resultados obtidos.

### Caso de Uso 1: Montagem Crítica de Hardware e Segurança
* **Prompt Inicial (Teste 1):** *"Como eu monto e ligo a placa Pixhawk no meu drone?"*
* **Resultado Obtido:** A IA gerou uma resposta genérica sobre conectar cabos de bateria e motores, misturando conceitos de placas genéricas (como Arduino ou controladoras antigas) e ignorando as especificidades do kit V8.
* **Troubleshooting ("Cicatriz"):** O NotebookLM precisava de restrição de contexto baseada em segurança. Em hardware Pixhawk, a orientação em relação ao centro de gravidade e a conexão física de periféricos de segurança específicos são condições obrigatórias para o firmware passar no teste de armamento (*pre-arm check*).
* **Prompt Refinado (Final):** *"Com base estritamente no documento `pixhawk.pdf`, quais são as regras mandatórias de orientação geométrica da placa e quais são os periféricos exatos rotulados como '(Required)' que devem ser conectados para a inicialização do sistema?"*
* **Resultado Final:** Sucesso. A IA mapeou que a Pixhawk deve ser montada o mais próximo possível do centro de gravidade do veículo, obrigatoriamente orientada com a seta impressa apontando para a frente. Além disso, isolou que os periféricos obrigatórios *(Required)* para a inicialização são o **Buzzer** (sinalizador sonoro) e o **Safety Switch** (interruptor de segurança).

### Caso de Uso 2: Resolução de Erros no Ambiente de Desenvolvimento (SITL)
* **Prompt Inicial (Teste 1):** *"O meu simulador do PX4 deu erro no VSCode dizendo que falta um arquivo, como consertar?"*
* **Resultado Obtido:** A IA sugeriu reinstalar as extensões de C++ do VSCode e clonar todo o repositório do PX4 novamente, uma solução genérica que consumiria horas desnecessárias.
* **Troubleshooting ("Cicatriz"):** O erro de caminhos de ficheiros na simulação SITL dentro do VSCode é um problema conhecido de mapeamento de diretórios quando se trabalha com workspaces espelhados de desenvolvimento (`src_vsc`). O documento focado em contribuidores aborda diretamente essa "cicatriz" técnica.
* **Prompt Refinado (Final):** *"Analise o Apêndice do guia `Getting-Started-as-a-contributor-on-PX4.pdf`. O que o autor instrui fazer quando o VSCode exibe um diálogo de erro após rodar a task 'preLaunchTask gazebo.iris' indicando que não foi possível abrir o arquivo 'iris.world'?"*
* **Resultado Final:** Sucesso técnico exato. O NotebookLM extraiu a solução prescrita na página 32: o erro ocorre porque o ambiente busca o ficheiro no diretório virtualizado de debug (`src_vsc`). A solução é copiar manualmente o ficheiro `iris.world` localizado no diretório original de build (`/home/<user>/src/Firmware/Tools/sitl_gazebo/worlds/iris.world`) para a pasta correspondente dentro de `src_vsc` e reiniciar o editor.

### Caso de Uso 3: Mapeamento Arquitetural do Ecossistema de Navegação
* **Prompt Inicial (Teste 1):** *"Como o Nav2 faz o robô andar sem bater nas coisas?"*
* **Resultado Obtido:** A IA explicou o conceito abstrato de SLAM e sensores laser, sem mapear a arquitetura real baseada em servidores de ação do ROS 2.
* **Troubleshooting ("Cicatriz"):** O ecossistema Nav2 não funciona de forma linear. Ele delega o planeamento e o controlo a servidores isolados coordenados por lógica de árvores de comportamento (Behavior Trees). O prompt precisava de exigir a taxonomia exata apresentada no meio académico.
* **Prompt Refinado (Final):** *"Utilizando o documento `ros2_nav2.pdf`, estruture uma explicação técnica sobre como o stack gerencia a separação de tarefas de navegação autónoma. Identifique os nós responsáveis por: 1) Planejar o caminho global; 2) Controlar a velocidade local; e 3) Orquestrar as ações e recuperações de falhas."*
* **Resultado Final:** A IA gerou a divisão arquitetural correta: o `bt_navigator` coordena o fluxo via Árvores de Comportamento; o `planner_server` calcula o caminho global livre de obstáculos num mapa estático; o `controller_server` gera as referências de velocidade locais para desviar de obstáculos dinâmicos; e o sistema invoca nós de recuperação (*recoveries*) em caso de bloqueio total.

---
