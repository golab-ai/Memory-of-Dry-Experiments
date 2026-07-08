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

