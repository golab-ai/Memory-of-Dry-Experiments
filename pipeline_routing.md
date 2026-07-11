# Pipeline路由决策经验

记录用户prompt与pipeline类型的映射关系、关键词识别经验。

---

## 📝 2026-07-07 15:26:48

**原始Prompt**: 对phosphine膦配体库进行配体优化，使用 ligand-match-fix_replaced.xlsx 数据训练产率预测模型，筛选top未见配体（top unseen ligands），并生成推荐配体的分子结构图。推荐结果发送实验室验证。

**Pipeline类型**: reaction_optimization

Prompt特征：包含“配体优化”“产率预测模型”“top unseen ligands”“分子结构图”“实验室验证”等关键词，明确指向化学空间探索与实验反馈闭环。应路由至reaction_optimization pipeline，适用于配体/催化剂虚拟筛选、建模预测和湿实验迭代优化场景。

---

## 📝 2026-07-07 15:48:34

**原始Prompt**: 基于 7K7O_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

用户输入包含蛋白结构文件（7K7O_clean.pdb）并要求靶向分子生成与合成路线规划，明确指向药物设计-合成一体化流程，因此路由到 drug_design_synthesis pipeline。

---

## 📝 2026-07-07 15:52:53

**原始Prompt**: 基于 SMARCA2_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

用户Prompt同时包含蛋白结构文件和合成路线规划需求，触发drug_design_synthesis流程。该流程适用于基于靶点结构的从头分子生成、可合成性评估及路线设计场景，采用先设计后合成的串行策略。

---

## 📝 2026-07-07 15:58:13

**原始Prompt**: 请计算材料发现中二氧化碳增稠剂体系的黏度，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p03/p1d.mol。

**Pipeline类型**: materials_discovery

关键词包含'计算'、'黏度'、'分子文件路径'以及材料体系，触发materials_discovery pipeline的simulation阶段。

---

## 📝 2026-07-07 16:01:39

**原始Prompt**: 请基于工作目录中 4ZBF_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

包含蛋白结构文件（*.pdb）且要求同时实现「靶向分子生成」与「合成路线规划」的任务，应路由至 drug_design_synthesis pipeline。关键特征：输入明确给出蛋白质对象，需求涵盖生成与合成两阶段，且不指定预定义分子库。

---

## 📝 2026-07-07 16:12:11

**原始Prompt**: 基于 7XZ5_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

当用户的提示包含蛋白质结构文件（如PDB）、靶向分子生成以及合成路线规划时，应路由到 drug_design_synthesis pipeline。关键特征：提供蛋白结构 + 要求生成与之结合的分子 + 规划合成。可简化为“基于结构的药物设计+逆合成分析”场景。

---

## 📝 2026-07-07 16:15:32

**原始Prompt**: 请基于工作目录中CDK2_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- 关键词“靶向分子生成”+“规划合成路线”+蛋白结构文件(CDK2_clean.pdb)
- 组合意图：基于受体结构的从头药物设计+可合成性评估
- 路由至 drug_design_synthesis pipeline，集成分子生成→对接筛选→合成路线规划

---

## 📝 2026-07-07 16:20:21

**原始Prompt**: 请基于工作目录中GPR139_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

当用户提示同时包含蛋白结构文件（.pdb）和靶向分子生成及合成路线需求时，路由到 drug_design_synthesis pipeline。关键特征：结构文件 + 生成 + 合成规划。优先激活基于结构的药物设计子流程，结合可合成性评估。

---

## 📝 2026-07-07 16:25:57

**原始Prompt**: 请计算电解液体系的等温压缩系数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p10/ec.mol。

**Pipeline类型**: materials_discovery

用户要求计算电解液体系的等温压缩系数，输入为.mol分子文件。关键词“等温压缩系数”、“电解液”触发材料物理性质计算，路由至 materials_discovery pipeline 的模拟阶段。该阶段自动调用分子动力学引擎执行在线性质和热力学量计算。

---

## 📝 2026-07-07 16:28:03

**原始Prompt**: 请基于工作目录中 Nav1.7_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

当用户 prompt 同时包含靶点结构（PDB文件）与分子生成、合成路线规划意图时，路由至 drug_design_synthesis pipeline。关键特征词：蛋白结构、靶向分子生成、合成路线。

---

## 📝 2026-07-07 16:33:41

**原始Prompt**: 为Suzuki E/Z选择性相关反应推荐最高转化率的反应条件，优化catalyst、base、solvent和cosolvent组合，使用 suzuki-ez.xlsx数据进行主动学习迭代优化。

**Pipeline类型**: reaction_optimization

用户要求优化Suzuki E/Z选择性反应条件（catalyst, base, solvent, cosolvent），并指定使用主动学习迭代。特征：多变量组合优化、明确数据源（Excel文件）、目标为最大化转化率。路由至 reaction_optimization pipeline，该管道支持基于贝叶斯优化的实验设计、数据加载和迭代建议。

---

## 📝 2026-07-07 16:39:44

**原始Prompt**: 请基于工作目录中 HPK1_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

用户提示中同时包含靶向分子生成和合成路线规划，且明确指定了蛋白结构文件（HPK1_clean.pdb），属于多阶段药物设计任务，因此路由至 drug_design_synthesis pipeline，并拆分为 drug_design_plan → drug_design_execute 两阶段执行。

---

## 📝 2026-07-07 16:49:33

**原始Prompt**: 为Buchwald-Hartwig胺化反应进行配体、碱和添加剂的条件优化，基于实验数据 Buchwald-HartwigDataset.csv进行迭代优化，推荐下一批最值得测试的实验组合。

**Pipeline类型**: reaction_optimization

包含反应条件优化、实验设计、数据集分析等关键词的提示应路由至 reaction_optimization pipeline。检测到指定反应类型（如 Buchwald-Hartwig）、历史数据文件（.csv）、待优化参数（配体/碱/添加剂）和迭代推荐需求时触发。

---

## 📝 2026-07-07 16:52:10

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

用户prompt包含湿实验数据存储路径、两个特定样本、配置文件位置和总体对比需求，触发lab_analysis pipeline。关键特征：'湿实验结果'指定数据类型，'样本一和样本二'需批量对比，'配置文件'提供分析方法，'总体对比结果'要求汇总分析。对应lab_analysis的batch模式，处理2个样本与4个实验交叉的8项分析。

---

## 📝 2026-07-07 16:52:51

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

用户prompt中包含数据目录路径、要求对比多个样本在所有实验上的结果，触发lab_analysis pipeline。关键特征：指定了湿实验数据存放位置、多样本对比意图。

---

## 📝 2026-07-07 16:53:04

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

用户prompt指定湿实验数据存储路径，要求分析多个样本的所有实验结果并进行总体对比，应路由至lab_analysis pipeline。关键特征：明确的数据目录路径、多样本对比请求、引用配置文件。

---

## 📝 2026-07-07 16:56:42

**原始Prompt**: Phosphine优化的湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: reaction_optimization

当用户提示词明确涉及‘分析湿实验结果’、‘对比样本’且指定数据目录和配置文件时，应路由到 reaction_optimization pipeline，优先触发实验后分析流程（reaction_plan → reaction_execute → lab_submission → result_analysis）。关键词：湿实验、结果文件、配置文件、对比。

---

## 📝 2026-07-07 17:04:58

**原始Prompt**: Phosphine优化的湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: reaction_optimization

用户请求包含关键词 '分析…结果' 和 '对比'，需要比较多个样本在多组实验上的表现。此类任务应路由至 results_analysis 或 comparison 子流水线，而非直接优化。关键特征：明确指定的多个样本ID、实验目录、输出总体对比结论的需求。

---

## 📝 2026-07-07 17:05:18

**原始Prompt**: 比较硼自由基参与甲烷氟化反应中两条氟转移通道的过渡态构型与反应选择性，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

用户输入包含'比较…过渡态构型与反应选择性'、'DFT优化'，明确要求机理分析和过渡态计算，自动路由至 mechanism_discovery 流水线，依次执行机理规划、过渡态生成和 DFT 优化阶段。

---

## 📝 2026-07-07 17:07:56

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

当用户要求分析实验数据目录中的湿实验结果，并对两个或多个样本在所有实验上进行对比时，应路由到 lab_analysis pipeline。关键特征：涉及实验结果文件（如LC-MS）、配置文件的目录、样本间总体对比。

---

## 📝 2026-07-07 17:15:38

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

用户提供存储湿实验结果的目录，要求对比多个样本在「所有实验」上的结果，且配置文件与数据同目录。这类任务触发 lab_analysis pipeline，自动采用 batch 模式进行批量对比分析。

---

## 📝 2026-07-07 17:17:16

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图。

**Pipeline类型**: reaction_optimization

Prompt包含'推荐膦配体'、'训练产率模型'、'筛选top unseen ligands'、'生成分子图'等关键意图，指向反应优化场景，触发reaction_optimization pipeline。主要特征：特定反应类型（D反应），需要结合历史与湿实验数据，以配体为中心优化产率并可视化推荐结果。

---

## 📝 2026-07-07 17:19:44

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图，并发送至实验室。

**Pipeline类型**: reaction_optimization

User prompts that combine analysis of historical reaction data with new wet-lab results to train a yield prediction model, screen a virtual ligand library for top unseen candidates, generate molecular images, and submit them for experimental validation should be routed to the reaction_optimization pipeline. Key indicators: 'recommend phosphine ligands', 'train yield model with historical and new data', 'screen top unseen ligands', 'generate molecular graphs', 'send to lab'.

---

## 📝 2026-07-07 17:54:42

**原始Prompt**: 请计算材料发现中PEG低聚物的汽化焓，分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p08/peg8.mol。

**Pipeline类型**: materials_discovery

用户请求计算分子体系（如PEG低聚物）的汽化焓，并明确提供分子结构文件（.mol）路径时，应路由至materials_discovery pipeline。关键特征：特定分子的热力学性质计算、给出分子文件路径、涉及物理性质预测。

---

## 📝 2026-07-07 20:10:37

**原始Prompt**: 请计算路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p02/Polypegbdo10.mol中固态电解质高分子材料的稳态构型。

**Pipeline类型**: materials_discovery

用户 prompt 包含明确的分子结构文件路径（如 .mol、.pdb）和“稳态构型”、“几何优化”、“能量最小化”等关键词时，路由至 materials_discovery pipeline。simulation 阶段自动处理结构优化任务。

---

## 📝 2026-07-07 21:53:15

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p13中的聚氨酯力学性能数据，训练AI模型并对分子库进行虚拟筛选。筛选条件：E > 1 GPa。

**Pipeline类型**: materials_discovery

用户prompt中包含明确的数据路径、材料类型（聚氨酯）、目标力学性能（E > 1 GPa）以及虚拟筛选需求，触发 materials_discovery pipeline 的 ai_screening 阶段。核心路由特征：关键词“训练AI模型”+“虚拟筛选”+定量性能阈值。

---

## 📝 2026-07-07 22:48:52

**原始Prompt**: 为Suzuki-Miyaura偶联反应推荐最佳反应条件（溶剂、试剂、反应物组合），基于已有实验数据进行主动学习迭代，输出下一轮优先实验建议。

**Pipeline类型**: reaction_optimization

提示中包含“反应条件推荐”、“Suzuki-Miyaura偶联”、“主动学习迭代”和“已有实验数据”等关键词时，直接路由至reaction_optimization管道。该管道适用于需从历史数据出发、通过迭代搜索在离散或连续化学空间中优化反应收率/选择性等目标的场景。

---

## 📝 2026-07-07 23:12:03

**原始Prompt**: 请计算酰亚胺材料的热膨胀系数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p07/p07.mol。

**Pipeline类型**: materials_discovery

用户要求计算材料（酰亚胺）的热膨胀系数，并提供分子文件路径 → 触发 materials_discovery pipeline，直接进入 simulation 阶段，匹配热物理属性计算子流程。关键特征：属性计算（热膨胀系数）、分子输入文件、材料名称明确。

---

## 📝 2026-07-07 23:41:04

**原始Prompt**: 请计算高分子材料聚乙烯熔体的定压热容，分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p09/pe100.mol。

**Pipeline类型**: materials_discovery

用户prompt包含高分子材料属性计算（聚乙烯熔体、定压热容）及分子文件路径，触发materials_discovery pipeline的simulation阶段。关键路由特征：材料种类（polymer）、性质（heat capacity）、指定输入文件（.mol），均指向分子动力学模拟子任务。

---

## 📝 2026-07-08 01:13:39

**原始Prompt**: 请计算材料发现中水凝胶体系的径向分布函数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p04/hydrogelchain.mol。

**Pipeline类型**: materials_discovery

当用户请求包含材料发现任务（如“计算材料发现中水凝胶体系的径向分布函数”）并指定分子输入文件路径时，直接路由到 materials_discovery pipeline 的 simulation 阶段。关键特征：水凝胶体系、径向分布函数(RDF)、.mol 文件路径。

---

## 📝 2026-07-08 02:21:54

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p15中的玻璃态高分子力学性能数据，训练AI模型(30epoch)并对分子库进行虚拟筛选。筛选条件：E > 3 GPa。

**Pipeline类型**: materials_discovery

用户明确指出数据路径、训练epochs和筛选条件，任务类型为材料虚拟筛选。pipeline路由到materials_discovery，因其涵盖“玻璃态高分子力学性能”数据处理、AI模型训练和基于性质（E>3 GPa）的分子库筛选，符合材料发现的典型流程。

---

## 📝 2026-07-08 06:01:19

**原始Prompt**: 基于 TS2.xyz、TS5.xyz、TS8.xyz，分析C-H键活化过程中的可能过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

当用户提供多个XYZ结构文件（如反应物、产物或中间体）并请求过渡态分析及DFT优化时，应路由至 mechanism_discovery pipeline。关键特征：输入为多结构文件、明确提及过渡态构型或C-H活化等反应机理步骤、需要从结构生成到优化的全流程。

---

## 📝 2026-07-08 10:42:17

**原始Prompt**: 请基于路径 /mnt/nas/opencode_data_2/runjob/polymer_systems/p06中的可回收高分子材料多性质数据，训练预测模型(30个epoch)并对分子库进行虚拟筛选。筛选条件：Tg在100°C到200°C之间，Tm在200°C到250°C之间，Td在250°C到300°C之间，E ≥1GPa，开环焓的范围是-20到-10 kJ/mol。筛选结果送往实验室进行验证。

**Pipeline类型**: materials_discovery

Prompt specifies multi-property data training (30 epochs) and virtual screening with Tg, Tm, Td, E, and ring-opening enthalpy constraints → triggers `materials_discovery` pipeline. Key routing features: explicit data path, training epochs, multi-property targets, and downstream lab validation intent.

---

## 📝 2026-07-08 15:26:37

**原始Prompt**: 基于 TS2.xyz、TS5.xyz、TS8.xyz，分析C-H键活化过程中的可能过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

- 用户输入包含已生成的过渡态结构文件（TS2.xyz, TS5.xyz, TS8.xyz），要求分析C-H键活化过渡态构型并进行DFT优化。
- 关键路由特征：涉及从预先生成的TS坐标出发，进行后续的DFT优化和振动分析，属于机理发现流程的后期步骤，匹配mechanism_discovery pipeline。

---

## 📝 2026-07-08 17:00:57

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

当用户要求分析指定目录下的湿实验结果，涉及多个样本（如样本一、样本二）在所有实验上的数据，并提供配置文件进行对比时，路由至 lab_analysis pipeline。关键特征：多样本对比、实验数据目录、配置文件路径、需要总体对比结果。

---

## 📝 2026-07-08 17:03:23

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图。

**Pipeline类型**: reaction_optimization

用户请求涉及反应优化（为D反应推荐膦配体）、基于历史与湿实验数据训练产率模型、筛选 top unseen ligands 并生成分子图。这些特征明确指向 reaction_optimization pipeline：需进行配体虚拟筛选、产率预测建模与可视化。关键路由信号：配体/催化剂推荐、数据驱动候选排序、unseen 化合物评估。

---

## 📝 2026-07-08 21:11:54

**原始Prompt**: 请基于路径 /mnt/nas/opencode_data_2/runjob/polymer_systems/p06中的可回收高分子材料多性质数据，训练预测模型(30个epoch)并对分子库进行虚拟筛选。筛选条件：Tg在100°C到200°C之间，Tm在200°C到250°C之间，Td在250°C到300°C之间，E ≥1GPa，开环焓的范围是-20到-10 kJ/mol。

**Pipeline类型**: materials_discovery

当用户prompt包含材料性质数据路径、明确要求训练预测模型并基于多性质范围进行虚拟筛选（如Tg、Tm、Td、E、开环焓等）时，路由至materials_discovery pipeline的ai_screening阶段。关键特征：指定数据路径、定义训练周期（如30 epoch）、多目标物性约束条件。

---

## 📝 2026-07-08 22:27:11

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p15中的玻璃态高分子力学性能数据，训练AI模型(30epoch)并对分子库进行虚拟筛选。筛选条件：E > 3 GPa。

**Pipeline类型**: materials_discovery

用户明确要求训练AI模型并基于训练结果进行虚拟筛选，提供了数据路径和材料属性条件（E>3 GPa）。关键触发特征：'训练AI模型' + '虚拟筛选' + '力学性能' + 条件阈值。可直接路由到 materials_discovery 的 ai_screening 阶段。

---

## 📝 2026-07-09 01:29:54

**原始Prompt**: 为腈亚胺与烯烃的1,3-偶极环加成反应枚举可能的过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

用户prompt要求枚举1,3-偶极环加成反应的过渡态构型并进行DFT优化，明确目标为机理发现中的过渡态搜索与验证，触发mechanism_discovery流水线。关键特征：反应类型指定、过渡态枚举、DFT计算。

---

## 📝 2026-07-09 01:40:06

**原始Prompt**: 为不对称硫醇加成反应推荐最优催化剂和反应条件，并基于已有实验数据 NS_acetal_dataset_with_pdt.csv进行主动学习优化，输出下一轮优先实验建议。

**Pipeline类型**: reaction_optimization

当用户请求包含以下特征时路由至 reaction_optimization pipeline：(1) 明确要求为特定反应推荐最优催化剂和反应条件；(2) 需要基于现有实验数据集（如CSV文件）进行数据驱动优化；(3) 涉及主动学习、下一轮实验设计或Bayesian优化等关键词；(4) 目标为收率、对映选择性等指标。典型prompt示例：'为不对称硫醇加成反应推荐最优催化剂和反应条件，并基于已有实验数据进行主动学习优化，输出下一轮优先实验建议'。

---

## 📝 2026-07-09 01:52:48

**原始Prompt**: 针对芳基底物范围数据进行配体推荐，优化electrophile、nucleophile和ligand的组合，筛选高产率候选实验，基于实验数据aryl-scope-ligand.csv 进行迭代优化。

**Pipeline类型**: reaction_optimization

当用户输入涉及配体推荐、底物范围优化、高产率候选筛选，且提供实验数据文件（如CSV）进行迭代优化时，路由到 reaction_optimization pipeline。关键特征：组合优化、数据驱动、配体/底物/亲电试剂/亲核试剂多变量协同提升产率。

---

## 📝 2026-07-09 02:14:01

**原始Prompt**: 以 demo1.xyz 为初始结构，生成硼烷体系的可能过渡态构型。无需进行DFT优化。

**Pipeline类型**: mechanism_discovery

用户要求从给定初始结构生成可能的过渡态构型，无需DFT优化，属于机理发现前置步骤。关键特征：明确提供初始结构文件（如 demo1.xyz）、生成“过渡态”而非优化、无需电子结构计算 → 路由到 mechanism_discovery 管道的 ts_generation 阶段。

---

## 📝 2026-07-09 03:26:27

**原始Prompt**: 计算这个电解液分子NC1OCCCO1的热力学性质，包括焓、熵、自由能。

**Pipeline类型**: qm_thermo

用户请求明确包含热力学性质（焓、熵、自由能）及分子标识（SMILES: NC1OCCCO1），直接触发 qm_thermo pipeline。关键路由特征：thermochemistry keywords + chemical structure input, no need for complex conversational branching.

---

## 📝 2026-07-09 03:35:44

**原始Prompt**: 计算这个反应的液相自由能：2CO2+6H2->C_H_OH+3H2O

**Pipeline类型**: qm_thermo

用户要求计算液相反应自由能（含明确反应式），触发热力学性质计算需求。路由至qm_thermo管道，因其专门处理反应热力学量（ΔG、ΔH等）的计算，支持指定溶剂环境。

---

## 📝 2026-07-09 04:26:23

**原始Prompt**: 计算以下几个分子的电化学窗口：COC(=O)OCC(F)(F)FO=C1OC=CO1、C1=CC=C(OC2=CC=CC=C2)C=C1

**Pipeline类型**: qm_thermo

多分子SMILES列表，要求计算电化学窗口、氧化还原电位等热力学性质 → 应路由到 qm_thermo pipeline：需进行分子结构准备、构象搜索、DFT优化/单点能、溶剂化模型计算。

---

## 📝 2026-07-09 04:33:47

**原始Prompt**: 计算这个反应的气相自由能：CO2+3H2 -> CH3OH+H2O

**Pipeline类型**: qm_thermo

For prompts containing an explicit chemical reaction equation (e.g., CO2 + 3H2 -> CH3OH + H2O) and a request for gas-phase free energy, route to qm_thermo pipeline. This involves stoichiometric balancing detection and extraction of individual species. Simple small-molecule reactions with ≤5 species and gas-phase conditions are best served by this pipeline.

---

## 📝 2026-07-09 04:42:33

**原始Prompt**: 计算Si体系的声子谱计算(supercell 4x4x4, delta 0.01)

**Pipeline类型**: phonon_spectrum

当用户请求包含“声子谱”、“phonon spectrum”且给出supercell大小（如4x4x4）与位移幅度（如delta）时，直接路由到phonon_spectrum pipeline。关键特征词：声子、phonon、supercell、delta。

---

## 📝 2026-07-09 05:18:15

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p11中的聚酰亚胺热性能数据，训练各性质的AI模型(30epoch)，并对该目录下的筛选分子库进行虚拟筛选。筛选条件：Tg ≥ 250°C，Tm ≥ 300°C，Td ≥ 450°C。

**Pipeline类型**: materials_discovery

用户请求训练预测模型并对分子库进行虚拟筛选，提供数据路径、材料性质（Tg, Tm, Td）及阈值条件时，路由至 materials_discovery pipeline。关键特征：训练AI模型、虚拟筛选、聚酰亚胺、热性能。

---

## 📝 2026-07-09 17:00:57

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p12中的聚酰亚胺气体渗透性数据，训练AI模型(30epoch)并对分子库进行虚拟筛选。筛选条件：O2 ≥ 5 Barrer，N2渗透率最低的前50个，CO2 ≥ 50 Barrer，CH4渗透率最低的前50个，H2 ≥ 100 Barrer。

**Pipeline类型**: materials_discovery

用户prompt若同时包含材料数据路径、AI模型训练要求和基于多重性能指标的虚拟筛选，应路由至materials_discovery pipeline，该pipeline支持数据加载、模型训练和条件筛选的端到端任务。

---

## 📝 2026-07-10 10:17:25

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p11中的聚酰亚胺热性能数据，训练各性质的AI模型(50epoch)，并对该目录下的筛选分子库进行虚拟筛选。筛选条件：Tg ≥ 250°C，Tm ≥ 300°C，Td ≥ 450°C。

**Pipeline类型**: materials_discovery

用户prompt同时包含“训练AI模型”(指定epoch数)与“虚拟筛选”(多性能条件)，且针对材料科学领域(聚酰亚胺热性能)，自动路由至materials_discovery管线的ai_screening阶段。关键判定词：训练模型、虚拟筛选、Tg/Tm/Td性能阈值。

---

## 📝 2026-07-10 12:12:36

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p11中的聚酰亚胺热性能数据，训练各性质的AI模型(50epoch)，并对该目录下的筛选分子库进行虚拟筛选。筛选条件：Tg ≥ 250°C，Tm ≥ 300°C，Td ≥ 450°C。

**Pipeline类型**: materials_discovery

当用户要求基于材料热性能数据训练AI模型并对分子库进行虚拟筛选时，应路由到 materials_discovery 管道的 ai_screening 阶段。Prompt 特征包括：'训练...AI模型'、'虚拟筛选'、明确的材料性质阈值（Tg、Tm、Td）。

---

## 📝 2026-07-10 13:41:10

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p14中的玻璃态高分子热性能数据，训练AI模型(50epoch)并对分子库进行虚拟筛选。筛选条件：Tg ≥ 150°C，Td ≥ 350°C。

**Pipeline类型**: materials_discovery

用户要求基于玻璃态高分子热性能数据训练AI模型并虚拟筛选，目标属性为Tg和Td，属于材料发现中的定量预测与高通量筛选场景，应路由至materials_discovery pipeline，启用AI筛选阶段。

---

## 📝 2026-07-10 14:47:22

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

When user prompt contains keywords like '实验结果分析', '样本', '对比结果', '实验数据', and specifies data path, route to lab_analysis pipeline. The prompt should indicate a need to analyze experimental results with comparison.

---

## 📝 2026-07-10 17:18:41

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

用户请求对两轮实验的产率进行分析，触发lab_analysis pipeline，选定yield_comparison分析模式。关键词：'产率进行分析'、'第一轮实验和第二轮实验'，表明需要对比多批次产率数据。

---

## 📝 2026-07-10 17:31:17

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

当用户要求对比两轮实验的产率时，路由到lab_analysis pipeline的yield_comparison模式。关键特征：多轮实验数据对比、明确提及'产率'、'两轮实验'等关键词。

---

## 📝 2026-07-10 17:34:29

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

用户prompt包含‘湿实验优化’、‘样本x’、‘实验配置’、‘对比结果’等关键词时，应路由到lab_analysis pipeline。该pipeline专用于湿实验数据下载、LCMS解析及产率对比分析，触发条件为明确提到实验目录、配置文件与多轮优化对比。

---

## 📝 2026-07-10 17:35:13

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

当用户prompt提及'湿实验优化结果''分析样本在某轮实验的结果''总体对比'等词，且涉及多轮实验数据比较时，路由至lab_analysis pipeline，分析模式设为yield_comparison。关键判别特征：多轮次结果目录路径、配置文件位置、产率对比意图。

---

## 📝 2026-07-10 17:38:46

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

用户提示包含‘湿实验优化结果’、‘对比结果’、明确要求分析样本二的第二轮实验结果，自动匹配到lab_analysis pipeline，采用batch模式处理单样本。关键路由特征：实验路径指定、样本级对比分析请求。

---

## 📝 2026-07-10 17:40:11

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

当用户提示包含“湿实验优化结果”、“分析样本X在新实验上的结果”、并指定数据目录和配置文件时，应路由至lab_analysis pipeline，该pipeline专门处理实验数据的批量分析、LC-MS执行和产量对比。

---

## 📝 2026-07-10 17:41:41

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

用户请求产率分析，且提及多轮实验结果记录 → 路由到 lab_analysis pipeline，分析模式为 yield_comparison。关键特征：'产率'、'实验记录'、'对比'。

---

## 📝 2026-07-10 17:43:32

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

用户请求涉及多轮实验产率对比分析，关键词包含'第一轮实验'、'第二轮实验'、'产率'、'分析'。触发lab_analysis pipeline，且内部子任务链识别出对比分析模式，路由至yield_comparison阶段。后续类似请求需提取实验轮次标识（数字、批次名）用于自动化文件匹配。

---

## 📝 2026-07-10 17:46:22

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

用户提示中包含'第一轮'、'第二轮'、'实验结果'、'产率分析'等关键词时，路由到 lab_analysis pipeline，并启用 yield_comparison 模式。

---

## 📝 2026-07-10 17:47:15

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

用户 Prompt 中包含“实验轮次”和“产率分析”关键词时，路由至 lab_analysis pipeline，并自动选择 yield_comparison 分析模式。触发特征：明确要求比较多个实验轮次的结果，且数据存放于“分析目录”。

---

## 📝 2026-07-10 17:47:33

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析

**Pipeline类型**: lab_analysis

用户要求对实验结果进行产率分析 → 触发 lab_analysis pipeline，分析模式为 yield_comparison。关键词：产率、分析、实验记录。

---

## 📝 2026-07-10 17:48:18

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户prompt包含实验数据目录、样本编号（如“样本二”）、配置文件同在目录中，且任务为分析湿实验优化结果时，路由至lab_analysis pipeline。关键特征：明确的数据路径、指定样本、配置文件并存。

---

## 📝 2026-07-11 00:16:02

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p12中的聚酰亚胺气体渗透性数据，训练AI模型(50epoch)并对分子库进行虚拟筛选。筛选条件：O2 ≥ 5 Barrer，N2渗透率最低的前50个，CO2 ≥ 50 Barrer，CH4渗透率最低的前50个，H2 ≥ 100 Barrer。

**Pipeline类型**: materials_discovery

用户prompt中明确要求“训练AI模型(50epoch)”和“虚拟筛选”，且路径指向聚酰亚胺气体渗透数据，属于材料发现场景。路由到materials_discovery的ai_screening阶段，关键特征：数据路径+模型训练epoch+多气体约束(O2,N2,CO2,CH4,H2)。

---

## 📝 2026-07-11 00:20:24

**原始Prompt**: 计算乙烯与丁二烯发生Diels-Alder环加成反应的可能过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

用户prompt包含“Diels-Alder环加成反应”、“过渡态构型”、“DFT优化”等关键词，明确要求计算反应路径的关键过渡态，自动路由至 mechanism_discovery pipeline。该pipeline按 mechanism_plan → ts_generation → dft_optimization 顺序执行。

---

## 📝 2026-07-11 01:27:45

**原始Prompt**: 请计材料发现中算水凝胶体系的热膨胀系数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p05/hydrogelchain.mol。

**Pipeline类型**: materials_discovery

用户prompt提及材料发现、水凝胶聚合物体系及热膨胀系数计算，并给出.mol分子文件路径时，路由至materials_discovery pipeline。关键特征：热膨胀系数、聚合物/水凝胶、.mol输入文件。

---

## 📝 2026-07-11 09:38:42

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

当用户prompt包含指定目录路径（如/mnt/nas/...）、要求分析特定样本（如“样本二”）在新的湿实验结果时，应路由至lab_analysis pipeline。关键特征：明确的数据存储路径、样本标识、分析新实验结果的指令。

---

## 📝 2026-07-11 09:39:13

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户prompt包含指定路径的实验数据、样本选择（如“样本二”）及配置文件位置，要求分析湿实验优化结果，触发lab_analysis pipeline。关键特征：目录路径、样本编号、配置文件、分析结果意图。

---

## 📝 2026-07-11 09:49:42

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

User prompt mentions wet-lab experiment optimization results stored in a specific directory ('/mnt/nas/opencode_data_2/runjob/experiment_results_round2'), requests analysis of a specific sample (sample 2), and notes configuration files are co-located. This pattern triggers the lab_analysis pipeline, which handles LC-MS data processing and yield comparison for wet-lab experiments.

---

## 📝 2026-07-11 10:00:19

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析。

**Pipeline类型**: lab_analysis

当用户请求对多轮实验结果进行产率分析，并提及“分析目录”时，路由至lab_analysis pipeline，使用yield_comparison模式。关键特征：多实验对比、产率指标、已有结构化实验记录目录。

---

## 📝 2026-07-11 10:02:53

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户要求分析指定目录(/mnt/nas/opencode_data_2/runjob/experiment_results_round2)中的实验结果，并指定样本二，配置文件也在同一目录。触发lab_analysis pipeline，执行批量分析模式(lab_plan→experiment_download→file_match→lcms_execute→yield_comparison)。

---

## 📝 2026-07-11 10:19:21

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验D上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户要求分析特定样本在不同实验条件下的湿实验优化结果，指定目录和配置文件，触发 lab_analysis pipeline。关键特征：提及“第二轮湿实验优化”“样本二”“新实验D”，明确数据位置，需进行 LCMS 数据解析和产量对比。

---

## 📝 2026-07-11 10:21:02

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析样本二在新实验D上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

当用户prompt明确指出实验数据目录路径（如/mnt/nas/.../experiment_results_round2），并要求分析特定样本（“样本二”）在指定新实验（“新实验D”）上的结果，且提及配置文件在同目录时，路由到lab_analysis pipeline，采用批量模式（batch）执行完整分析流程。

---

## 📝 2026-07-11 10:23:30

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析新配体在新实验D上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户请求分析第二轮湿实验优化结果，提供存储目录及配置文件，明确提到‘新配体在新实验D上的结果’——此类涉及湿实验数据（LCMS、产率比较）的任务应路由至lab_analysis pipeline。关键特征：包含实验轮次、结果目录路径、实验名称标识（如实验D）。

---

## 📝 2026-07-11 10:24:03

**原始Prompt**: 第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析新配体在新实验D上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户请求分析湿实验优化结果，并指定数据目录和特定实验标签时，应路由到 lab_analysis pipeline。关键特征：提及“实验结果”、“新配体”、“新实验X”、配置文件同目录。处理流程：数据下载→文件匹配→LCMS分析→产率对比。

---

## 📝 2026-07-11 10:24:29

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析。

**Pipeline类型**: lab_analysis

用户明确请求对历史实验产率进行分析，提及多轮实验记录已存在于分析目录，触发lab_analysis pipeline。关键路由特征：多轮实验对比、产率分析、已有数据目录。

---

## 📝 2026-07-11 10:36:07

**原始Prompt**:   第二轮湿实验优化的结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results_round2中，请分析新配体在实验D上的结果，配置文件也在相同目录中。

**Pipeline类型**: lab_analysis

用户指定了数据目录和实验ID（实验D），且明确提到“第二轮湿实验优化结果”，触发 lab_analysis pipeline。Pipeline 采用 batch 模式，对单一样本进行分析，路径中包含配置文件，自动完成从实验下载到产率比较的全流程。

---

## 📝 2026-07-11 10:36:42

**原始Prompt**: 第一轮实验和第二轮实验结果记录在分析目录中，请帮我对产率进行分析。

**Pipeline类型**: lab_analysis

用户请求对多轮实验产率分析，特征：多轮次对比（第一轮vs第二轮），指定目录，产率分析任务。直接路由到lab_analysis管道，分析模式自动选为yield_comparison。

---

