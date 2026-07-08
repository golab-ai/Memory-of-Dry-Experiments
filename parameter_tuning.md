# 参数调优经验

记录不同任务的最佳参数配置、调优经验。

---

## 📝 2026-07-07 15:26:48

**原始Prompt**: 对phosphine膦配体库进行配体优化，使用 ligand-match-fix_replaced.xlsx 数据训练产率预测模型，筛选top未见配体（top unseen ligands），并生成推荐配体的分子结构图。推荐结果发送实验室验证。

**Pipeline类型**: reaction_optimization

- 分子指纹: radius=2, nBits=2048 (平衡分辨率和维度)
- 模型: RandomForestRegressor(n_estimators=200, max_depth=10, random_state=42)，使用5折交叉验证优化超参数，防止过拟合
- Top unseen推荐数量: 初始推荐top 10-20个候选，避免合成压力过大
- 数据预处理: 去除产率缺失项，对产率进行归一化可选，避免模型偏向高产率值

---

## 📝 2026-07-07 15:48:34

**原始Prompt**: 基于 7K7O_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- 对接网格中心选自关键残基质心，网格尺寸扩大至25×25×25 Å³ 以完全覆盖口袋
- 分子生成过滤条件：MW 200-500，logP 1-5，可旋转键≤8，满足类药性
- 合成路线规划搜索宽度优先，最小得分阈值0.6以保证路线可行性

---

## 📝 2026-07-07 15:52:53

**原始Prompt**: 基于 SMARCA2_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

分子生成参数：生成分子数100，多样性权重0.7，结合位点半径10 Å；逆合成搜索：最大深度10，分支因子5，仅使用市售原料库，单步反应超时60秒。

---

## 📝 2026-07-07 15:58:13

**原始Prompt**: 请计算材料发现中二氧化碳增稠剂体系的黏度，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p03/p1d.mol。

**Pipeline类型**: materials_discovery

最佳配置：
- 时间步长: 1 fs
- Nose-Hoover thermostat: relaxation 100 fs
- 剪切速率: 10^9 - 10^10 s^-1范围测试
- 平衡阶段: 至少1 ns NPT达到均衡密度
- 输出频率: 每1000步保存热力学量

---

## 📝 2026-07-07 16:01:39

**原始Prompt**: 请基于工作目录中 4ZBF_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

• 分子生成：过滤条件设置 QED ≥ 0.4、SAS ≤ 6、LogP 在 1-5 之间，确保类药性；对接后保留 Vina 打分 ≤ -8.0 kcal/mol 的分子
• 合成路线：设置最长反应步数为 5，禁用危险试剂（如重氮化合物），优先使用已建立的反应类型（如酰胺偶联）；对不可得的中间体触发回溯搜索
• 多目标权衡：对同时满足高对接评分且合成步数≤3 的分子给予优先推荐

---

## 📝 2026-07-07 16:12:11

**原始Prompt**: 基于 7XZ5_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

分子生成参数：口袋采样距离阈值6-8 Å，生成分子数量1000-5000，对接分数截断<-8 kcal/mol，类药性(QED)>0.5，可合成性分数(SAscore)<4。逆合成参数：最大深度4，每步最大转化数100，起始原料必须现货可得，避免高成本反应（如不对称氢化）。

---

## 📝 2026-07-07 16:15:32

**原始Prompt**: 请基于工作目录中CDK2_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- 生成分子数量：初始设为100，平衡多样性与计算成本
- 对接 exhaustiveness 设为16，在精度与速度间折中
- 合成路线最大深度=8 可避免过于复杂路线，提高可行性
- 仅保留结合能≤-8.0 kcal/mol的分子进入合成规划，减少无效计算

---

## 📝 2026-07-07 16:20:21

**原始Prompt**: 请基于工作目录中GPR139_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- 分子生成：箱约束距离阈值6-8 Å，多样性参数temperature=0.7，类药性权重drug_qed > 0.4
- 合成规划：最大步数≤5，商业可购买起始原料优先，库存匹配阈值>0.8
- 输出数量：保留Top-10对接得分兼可合成分子

---

## 📝 2026-07-07 16:25:57

**原始Prompt**: 请计算电解液体系的等温压缩系数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p10/ec.mol。

**Pipeline类型**: materials_discovery

推荐关键参数：
- 时间步长 2 fs，键约束 LINCS
- 压力耦合常数 2 ps，等温压缩系数参考值 4.5e-5 bar⁻¹ 用于压力耦合
- 生产段长度不低于 40 ns 以保证体积涨落收敛
- 排除前 10 ns 平衡数据，避免初始瞬态影响
- 若体积涨落过大，延长模拟至 100 ns 并检查系统是否出现相分离

---

## 📝 2026-07-07 16:28:03

**原始Prompt**: 请基于工作目录中 Nav1.7_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

生成分子权重: docking_score 占 0.7, QED 0.2, SA_Score 0.1；对接 exhaustiveness=8；分子过滤: 对接得分 <= -8.5 kcal/mol 作为候选阈值。合成路线长度限制 <= 8 步，起始原料限制商用可得 (in-stock=true)。

---

## 📝 2026-07-07 16:33:41

**原始Prompt**: 为Suzuki E/Z选择性相关反应推荐最高转化率的反应条件，优化catalyst、base、solvent和cosolvent组合，使用 suzuki-ez.xlsx数据进行主动学习迭代优化。

**Pipeline类型**: reaction_optimization

- 采集函数: Upper Confidence Bound (UCB)，kappa=1.96 平衡探索与利用。
- 核函数: Matern 5/2，适配化学空间平滑性。
- 数据标准化: 启用，因各变量尺度差异大。
- 初始实验数: 使用全部已有数据点作为先验，无需额外初始设计。
- 批次大小: 1（逐次推荐），保证精细化调整。

---

## 📝 2026-07-07 16:39:44

**原始Prompt**: 请基于工作目录中 HPK1_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- **口袋定义**: 以配体或关键残基为中心，扩展半径 10 Å 定义为结合位点。
- **生成约束**: 限制分子量 ≤ 500，可旋转键数 ≤ 5，logP 0~5，确保类药性。
- **合成参数**: 最大合成步数 8, 优先选用高置信度单体/反应，避免保护基策略复杂化。

---

## 📝 2026-07-07 16:49:33

**原始Prompt**: 为Buchwald-Hartwig胺化反应进行配体、碱和添加剂的条件优化，基于实验数据 Buchwald-HartwigDataset.csv进行迭代优化，推荐下一批最值得测试的实验组合。

**Pipeline类型**: reaction_optimization

贝叶斯优化采集函数推荐期望改进（EI），平衡探索与利用。初始随机实验数设为 10-20 以构建代理模型，每轮推荐 5-10 个候选。收敛监控：若连续多轮最佳收率不变或 EI 下降，可调大探索参数（如增加 κ 值）。

---

## 📝 2026-07-07 16:52:10

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

配置文件需提前放置于相同目录，包含离子色谱、质谱峰检测、比对参数等。建议在配置中固定保留时间窗口与对齐参数，确保两个样本数据可比性。无需额外调参。

---

## 📝 2026-07-07 16:53:04

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

采用批量分析模式(batch)，自动并行处理多实验。实验数量无需手动指定，系统从配置目录自动检测（本例识别8个实验并全部成功执行）。建议保持默认批量大小以平衡资源利用。

---

## 📝 2026-07-07 16:56:42

**原始Prompt**: Phosphine优化的湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: reaction_optimization

- 对比指标：自动提取 yield、selectivity、conversion 等反应优化关键指标，默认展示差值及相对变化百分比。\n- 显著性检验：对重复实验数据默认使用 t-test（α=0.05），可调整置信水平。

---

## 📝 2026-07-07 17:04:58

**原始Prompt**: Phosphine优化的湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: reaction_optimization

对比分析时推荐配置：
- 对比指标：同时输出产率、纯度和转化率，并标注胜出样本
- 数据对齐：按实验ID内连接，避免错位
- 汇总方式：对重复实验取中位数以减少离群值影响
- 可视化参数：自动生成双柱状图，设置差异阈值 >2% 高亮

---

## 📝 2026-07-07 17:05:18

**原始Prompt**: 比较硼自由基参与甲烷氟化反应中两条氟转移通道的过渡态构型与反应选择性，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

- TS 优化必须同时计算频率以确认唯一虚频
- 使用 calcfc 在第一步计算精确力常数，提高收敛稳定性
- 溶剂效应在优化和频率计算中一致包含
- 当出现多余虚频时，检查初始构型并沿虚频振动方向微调后重新优化

---

## 📝 2026-07-07 17:15:38

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

- **batch 分析**: 启用 batch 模式后，自动扫描所有实验并生成全部对比任务（本次 2 样本自动生成 8 个分析项，全部成功）。
- **实验发现**: 依赖配置文件自动发现实验清单，无需显式传入实验列表，实验数可为 0 交由系统推断。

---

## 📝 2026-07-07 17:17:16

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图。

**Pipeline类型**: reaction_optimization

模型训练：XGBoost，n_estimators=200, max_depth=6, learning_rate=0.1，使用ECFP4指纹（半径2，2048位）。筛选：预测产率阈值>65%，确保推荐质量。调优建议：若数据量少，降低树深度并增加交叉验证折数；可视化配体时统一SMILES标准化。

---

## 📝 2026-07-07 17:19:44

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图，并发送至实验室。

**Pipeline类型**: reaction_optimization

- RandomForest yield model: n_estimators=100, max_depth=5 to prevent overfitting on small datasets; use Morgan fingerprints (radius=2, nBits=2048) as features.
- Ligand screening: top_k=10, apply Tanimoto diversity filter (threshold 0.7) to avoid recommending near-duplicates.
- Molecule image generation: kekulize SMILES first, use drawOptions with atom indices for clarity.

---

## 📝 2026-07-07 17:54:42

**原始Prompt**: 请计算材料发现中PEG低聚物的汽化焓，分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p08/peg8.mol。

**Pipeline类型**: materials_discovery

- **温度**：298 K（标准条件）。
- **压力**：1 bar（液相），气相可用NVT系综。
- **力场**：OPLS-AA，对PEG适配良好。
- **模拟时长**：平衡1 ns，生产5 ns，步长2 fs。
- **液相尺寸**：至少500个分子，消除表面效应。
- **静电处理**：PME，截断1.2 nm。

---

## 📝 2026-07-07 20:10:38

**原始Prompt**: 请计算路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p02/Polypegbdo10.mol中固态电解质高分子材料的稳态构型。

**Pipeline类型**: materials_discovery

高分子体系优化建议：启用周期性边界条件，非键截断半径 12 Å，PME 长程静电处理；若结构初始扭曲严重，先采用最陡下降法 200 步预松弛，再切换至 L-BFGS 收敛。

---

## 📝 2026-07-07 21:53:15

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p13中的聚氨酯力学性能数据，训练AI模型并对分子库进行虚拟筛选。筛选条件：E > 1 GPa。

**Pipeline类型**: materials_discovery

随机森林模型推荐参数：n_estimators=200, max_depth=15, min_samples_leaf=5，使用5折交叉验证，特征采用标准化处理。关键调优点：针对小数据集可增加 n_estimators 防止过拟合，阈值过滤时使用预测值而非原始单位。

---

## 📝 2026-07-07 22:48:52

**原始Prompt**: 为Suzuki-Miyaura偶联反应推荐最佳反应条件（溶剂、试剂、反应物组合），基于已有实验数据进行主动学习迭代，输出下一轮优先实验建议。

**Pipeline类型**: reaction_optimization

高斯过程核函数选择Matérn 5/2，长度尺度初始化为1.0并按最大后验估计优化。采集函数探索参数ξ设为0.01。主动学习轮次设置为5-8轮，每轮建议10个候选。对离散变量使用连续松弛激活后再映射回具体试剂。

---

## 📝 2026-07-07 23:12:03

**原始Prompt**: 请计算酰亚胺材料的热膨胀系数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p07/p07.mol。

**Pipeline类型**: materials_discovery

• 温度范围：200K – 400K，步长 20K，覆盖典型服役区间
• 压力：1 atm，各向同性控压
• 平衡阶段：前 200 ps 用于平衡，后 800 ps 采集体数据
• 时间步长：1 fs，适用于全原子力场（OPLS-AA）
• 推荐力场：若分子含芳杂环，优先使用 PCFF 或 COMPASS

---

## 📝 2026-07-07 23:41:04

**原始Prompt**: 请计算高分子材料聚乙烯熔体的定压热容，分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p09/pe100.mol。

**Pipeline类型**: materials_discovery

热容计算推荐使用NPT系综，时间步长1.0 fs，先进行能量最小化与100 ps NPT预平衡，再对每个目标温度进行200 ps生产模拟。温度步长20-30 K，在熔点以上范围扫描。采用Nosé-Hoover恒温恒压器，耦合常数0.1-0.5 ps。采用波动公式或焓对温度微分计算Cp，注意消除初始瞬态影响。

---

## 📝 2026-07-08 01:13:39

**原始Prompt**: 请计算材料发现中水凝胶体系的径向分布函数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p04/hydrogelchain.mol。

**Pipeline类型**: materials_discovery

RDF 计算推荐参数：bin 宽度 0.1 Å（精确度与噪声平衡最佳），截止距离设为盒子长度的一半；若体系含水，需单独为水氧-水氧对计算 RDF。若输入仅为单一 .mol 结构（无轨迹），需先进行短平衡模拟再采样。

---

## 📝 2026-07-08 02:21:54

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p15中的玻璃态高分子力学性能数据，训练AI模型(30epoch)并对分子库进行虚拟筛选。筛选条件：E > 3 GPa。

**Pipeline类型**: materials_discovery

- 训练epoch数：30，平衡训练时间和模型收敛，避免过拟合。
- 筛选阈值：E > 3 GPa，根据材料应用需求设定。
- 建议：可尝试学习率0.001、batch_size 32、EarlyStopping防止过拟合。

---

## 📝 2026-07-08 06:01:19

**原始Prompt**: 基于 TS2.xyz、TS5.xyz、TS8.xyz，分析C-H键活化过程中的可能过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

DFT级别：对于有机金属C-H活化，推荐使用色散校正的杂化泛函（如B3LYP-D3或M06-L）搭配def2-SVP基组，金属原子使用有效核势（如SDD或def2-ECP）；
过渡态优化：opt=ts, calcfc, noeigentest 可提高收敛性；如果虚频与目标反应坐标不一致，可结合IRC与重新优化。

---

## 📝 2026-07-08 10:42:17

**原始Prompt**: 请基于路径 /mnt/nas/opencode_data_2/runjob/polymer_systems/p06中的可回收高分子材料多性质数据，训练预测模型(30个epoch)并对分子库进行虚拟筛选。筛选条件：Tg在100°C到200°C之间，Tm在200°C到250°C之间，Td在250°C到300°C之间，E ≥1GPa，开环焓的范围是-20到-10 kJ/mol。筛选结果送往实验室进行验证。

**Pipeline类型**: materials_discovery

Training with 30 epochs balanced convergence and overfitting for multi-property regression. No learning rate or architecture tuning mentioned; default settings used. For similar tasks, consider early stopping on validation loss and separate output heads per property to handle different dynamic ranges.

---

## 📝 2026-07-08 15:26:37

**原始Prompt**: 基于 TS2.xyz、TS5.xyz、TS8.xyz，分析C-H键活化过程中的可能过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

- 泛函：B3LYP-D3(BJ) 或 M06-2X，对C-H键活化过渡态有良好描述。
- 基组：def2-SVP（优化）/ def2-TZVP（单点能校正）。
- 溶剂模型：SMD (对应实验溶剂)。
- 过渡态优化设置：`Opt=TS, CalcFC, NoEigenTest, Freq`，网格使用 `Grid=UltraFine`。
- 收敛标准可适当提高：`Conver=7` 或 `TightOpt`。

---

## 📝 2026-07-08 17:00:57

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

- 自动批次模式（batch）无需逐实验调用，一次性处理所有匹配的实验
- 样本标识建议使用中文原名，由工具内部进行索引匹配
- 配置文件与数据同目录可减少路径配置歧义

---

## 📝 2026-07-08 17:03:23

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图。

**Pipeline类型**: reaction_optimization

• 产率模型：n_estimators=100-200，max_depth=3-5，learning_rate=0.05-0.1 防止小数据过拟合。
• 指纹参数：radius=2，nBits=2048 平衡区分度与稀疏性。
• 筛选 top-k：建议 k=5-10，过大可能引入低产率候选，过小则探索不足。
• 若数据量 < 50 条，建议使用留一法交叉验证，避免过拟合误判。

---

## 📝 2026-07-08 21:11:54

**原始Prompt**: 请基于路径 /mnt/nas/opencode_data_2/runjob/polymer_systems/p06中的可回收高分子材料多性质数据，训练预测模型(30个epoch)并对分子库进行虚拟筛选。筛选条件：Tg在100°C到200°C之间，Tm在200°C到250°C之间，Td在250°C到300°C之间，E ≥1GPa，开环焓的范围是-20到-10 kJ/mol。

**Pipeline类型**: materials_discovery

- 训练周期：30 epoch在中小规模材料数据集上通常可实现良好收敛且不易过拟合，可作为基准值。
- 批量筛选：一次性传入所有物性约束，避免多次过滤造成性能损耗；数值区间需注意单位与数据列的一致性（如E单位GPa）。

---

## 📝 2026-07-08 22:27:11

**原始Prompt**: 请基于路径/mnt/nas/opencode_data_2/runjob/polymer_systems/p15中的玻璃态高分子力学性能数据，训练AI模型(30epoch)并对分子库进行虚拟筛选。筛选条件：E > 3 GPa。

**Pipeline类型**: materials_discovery

用户指定 30 个训练周期，对于力学性能数据集通常足够收敛。建议配合早停机制和验证集监控避免过拟合。若数据量小于 500 条，可尝试增加 epoch 至 50 并降低学习率。

---

