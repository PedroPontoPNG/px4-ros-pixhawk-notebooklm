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

---

## 🛠️ Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Abaixo estão registadas as iterações e refinamentos estratégicos realizados no NotebookLM para extrair informações precisas, evitando que a IA misturasse conceitos de ROS 1 com ROS 2 ou falhasse em detalhes de hardware.

### Caso de Uso 1: Diferenciação de Estrutura de Workspaces (ROS 1 vs ROS 2)
* **Prompt Inicial (Teste 1):** *"Como criar e compilar um pacote de navegação de acordo com os PDFs?"*
* **Resultado Obtido:** O NotebookLM misturou a estrutura do espaço de trabalho do ROS 1 (`catkin_ws` com o diretório `devel`) com os comandos do ROS 2.
* **Troubleshooting ("Cicatriz"):** Percebi que precisava de delimitar rigidamente qual o paradigma de software pretendido, forçando o NotebookLM a olhar para as fontes de forma isolada.
* **Prompt Refinado (Final):** *"Com base exclusivamente nas apresentações do Prof. Walter Fetter Lages, crie uma tabela comparativa evidenciando a estrutura de diretórios temporários e finais de um workspace em
