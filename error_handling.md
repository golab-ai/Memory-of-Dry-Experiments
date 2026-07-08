# 错误处理经验

记录常见错误及解决方案、异常情况处理。

---

## 📝 2026-07-07 15:26:48

**原始Prompt**: 对phosphine膦配体库进行配体优化，使用 ligand-match-fix_replaced.xlsx 数据训练产率预测模型，筛选top未见配体（top unseen ligands），并生成推荐配体的分子结构图。推荐结果发送实验室验证。

**Pipeline类型**: reaction_optimization

- Excel读取失败: 检查文件路径和扩展名，确保使用dtype指定列类型，处理合并单元格
- RDKit分子无效: 使用Chem.MolFromSmiles()检查返回值，跳过无效分子并记录警告
- 特征缺失: 确认所有配体都有SMILES，若缺失则删除该条记录
- 模型预测性能差: 检查数据量是否足够（建议≥50条），尝试不同指纹或特征，排查数据泄漏
- 实验室反馈异常: 在发送前验证推荐配体SMILES有效性，附带上预测值和模型描述，方便实验人员评估

---

## 📝 2026-07-07 15:52:53

**原始Prompt**: 基于 SMARCA2_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

原始PDB可能含缺失残基或非标准残基，需先修复并质子化；结合位点定义不准会导致生成分子无靶向性，需根据共结晶配体或保守残基确定口袋；逆合成搜索可能超时，需设置搜索时间上限并允许返回不完整路线。

---

## 📝 2026-07-07 15:58:13

**原始Prompt**: 请计算材料发现中二氧化碳增稠剂体系的黏度，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p03/p1d.mol。

**Pipeline类型**: materials_discovery

常见问题：
- .mol文件可能缺乏力场参数：自动调用力场分配工具(oplsaa/gromos)补充
- 初始几何高能：先执行能量最小化（minimize 1e-4）
- 剪切模拟发散：降低剪切速率或增强thermostat耦合

---

## 📝 2026-07-07 16:01:39

**原始Prompt**: 请基于工作目录中 4ZBF_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

• 问题：蛋白结构中包含结晶水/配体导致结合位点被掩盖 → 解决：自动去除 HETATM 记录中非重要的水分子和原始配体，重新定义结合口袋
• 问题：生成分子合成性过低，规划器返回空路线 → 解决：降低合成性阈值，或对分子进行骨架跃迁修正后再送入规划器
• 问题：输入 PDB 格式不规范导致工具解析失败 → 解决：先通过 pdb4amber 或标准清洗脚本规范化格式，检查 ATOM/HETATM 记录连续性

---

## 📝 2026-07-07 16:15:32

**原始Prompt**: 请基于工作目录中CDK2_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- **PDB预处理**：移除杂原子/水分子，仅保留蛋白链A；缺失原子用PDB2PQR补全
- **对接失败**：遇配体原子类型不支持时回退到smina，或自动添加Gasteiger电荷
- **路线规划超时**：若单分子合成路线搜索超过2分钟，返回“无可行路线”，但不阻塞整体流程

---

## 📝 2026-07-07 16:20:21

**原始Prompt**: 请基于工作目录中GPR139_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- PDB结构若含非标准残基，需先清理（任务中已提供clean版本，避免失败）
- 生成分子若不可合成，通过合成规划器过滤，不可合成比例高时降低多样性或收紧类药性
- 对接失败，使用柔性对接或回退到相似口袋结合模式

---

## 📝 2026-07-07 16:25:57

**原始Prompt**: 请计算电解液体系的等温压缩系数，输入分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p10/ec.mol。

**Pipeline类型**: materials_discovery

无错误。若出现 .mol 文件不兼容，用 openbabel 转换前确认键序和原子类型。若压缩系数异常大（>1e-4 bar⁻¹），可能因系统未充分平衡或存在气泡，需重新能量最小化并缓慢升温至目标温度。确保压力耦合算法适用于各向同性液体体系。

---

## 📝 2026-07-07 16:28:03

**原始Prompt**: 请基于工作目录中 Nav1.7_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

蛋白准备若遗漏加氢或去除水分子，导致对接异常：统一用 pdb4amber 去除水/配体，加氢后保存为 .pdbqt。合成规划时，复杂天然产物可能无路线：降低 minimum_returning 或允许更广原料范围，并对分子进行简化破环后再规划。

---

## 📝 2026-07-07 16:33:41

**原始Prompt**: 为Suzuki E/Z选择性相关反应推荐最高转化率的反应条件，优化catalyst、base、solvent和cosolvent组合，使用 suzuki-ez.xlsx数据进行主动学习迭代优化。

**Pipeline类型**: reaction_optimization

可能问题:
- Excel 中缺失值或非数值响应会导致模型拟合失败，处理方案: 预处理过滤含空值的行，并警告用户。
- 变量水平过多导致组合爆炸，限制候选池为历史观测组合，避免生成未定义组合。
- 模型收敛警告时，重启优化并降低核函数长度尺度初始值，成功解决。

---

## 📝 2026-07-07 16:39:44

**原始Prompt**: 请基于工作目录中 HPK1_clean.pdb 蛋白结构，进行靶向分子生成并规划合成路线。

**Pipeline类型**: drug_design_synthesis

- **蛋白预处理失败**: 若 PDB 文件含多构象或非标准残基，先使用 PDBFixer 标准化；缺失侧链用 `add_missing_atoms` 修复。
- **生成分子不合理**: 通过药物相似性过滤（QED ≥ 0.4）剔除，若全部不合格则调整口袋定义或放宽约束。
- **合成路径报错**: 无可用路径时，降低起始物料限制或增加搜索深度，并提示可能需人工干预。

---

## 📝 2026-07-07 16:49:33

**原始Prompt**: 为Buchwald-Hartwig胺化反应进行配体、碱和添加剂的条件优化，基于实验数据 Buchwald-HartwigDataset.csv进行迭代优化，推荐下一批最值得测试的实验组合。

**Pipeline类型**: reaction_optimization

确保 CSV 列名与搜索空间定义一致（大小写、空格），缺失值需提前处理（剔除或均值填充）。若优化器收敛到局部最优，可重启随机采样或调整采集函数；数据量过少时，先进行小规模因子筛选再全因子贝叶斯优化。

---

## 📝 2026-07-07 16:53:04

**原始Prompt**: 湿实验结果被存储在/mnt/nas/opencode_data_2/runjob/experiment_results中，请分析样本一和样本二在所有实验上的结果，配置文件也在相同目录中，最终给出总体对比结果。

**Pipeline类型**: lab_analysis

初始实验计数显示为0，但通过自动扫描配置文件目录发现8个实验并成功分析，无实际错误。若遇到实验数为0的情况，应检查配置文件是否存在于指定目录且命名规范，确保自动检测机制正常工作。

---

## 📝 2026-07-07 17:05:18

**原始Prompt**: 比较硼自由基参与甲烷氟化反应中两条氟转移通道的过渡态构型与反应选择性，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

本次任务执行成功，未出现需记录的错误。建议：若过渡态搜索失败，可改用 relaxed PES 扫描配合 NEB 方法生成初始猜测，并降低初步优化收敛标准以快速获得近似 TS。

---

## 📝 2026-07-07 17:17:16

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图。

**Pipeline类型**: reaction_optimization

湿实验数据可能缺失产率列或配体标识，导致训练失败；解决：自动检测并提示缺失列，或回退仅用历史数据。生成分子图时SMILES无效引发错误；解决：使用RDKit标准化，移除无效配体并记录日志。

---

## 📝 2026-07-07 17:19:44

**原始Prompt**: 为D反应推荐膦配体，用历史数据与新的湿实验数据训练产率模型并筛选 top unseen ligands，同时生成推荐配体的分子图，并发送至实验室。

**Pipeline类型**: reaction_optimization

No errors occurred in this run. Pre-emptive checks implemented: (1) Validate wet-lab yield column for missing values and zero-variance before training. (2) Filter out invalid SMILES in the virtual library using RDKit sanitization. (3) Set API timeout and retry (max 3 attempts) for lab submission to handle network flakes. (4) Ensure PNG generation handles radicals/charges gracefully by disabling kekulization warnings.

---

## 📝 2026-07-07 17:54:42

**原始Prompt**: 请计算材料发现中PEG低聚物的汽化焓，分子文件路径为/mnt/nas/opencode_data_2/ddp_agent_code/huntianling-agent/polymer_systems/p08/peg8.mol。

**Pipeline类型**: materials_discovery

- **文件路径错误**：确认.mol文件存在且路径正确，可用`os.path.isfile`检查。
- **力场参数缺失**：PEG原子类型可能在标准力场中缺失，需通过构建残基或使用用户自定义参数补充。
- **模拟崩溃**：常见于初始结构不合理，应能量最小化并逐步升温；若约束失败，改用LINC + 适当步长。
- **焓值波动大**：延长模拟时间，确保体系充分平衡，检查温度耦合方法。

---

## 📝 2026-07-07 22:48:52

**原始Prompt**: 为Suzuki-Miyaura偶联反应推荐最佳反应条件（溶剂、试剂、反应物组合），基于已有实验数据进行主动学习迭代，输出下一轮优先实验建议。

**Pipeline类型**: reaction_optimization

初始数据极少（<10个点）时模型易过拟合，通过在先验均值中加入已知反应趋势（如电子效应）的化学规则，或使用多任务高斯过程共享相似底物的信息来缓解。当推荐出现重复实验时，通过新添加多样性惩罚项（如最小化与已实验点的Tanimoto相似度）成功避免。

---

## 📝 2026-07-08 06:01:19

**原始Prompt**: 基于 TS2.xyz、TS5.xyz、TS8.xyz，分析C-H键活化过程中的可能过渡态构型，并进行DFT优化。

**Pipeline类型**: mechanism_discovery

过渡态优化收敛失败：常见原因包括初始Hessian质量差或势能面平坦，可添加calcfc或recfc，或采用opt=ts, gdiis；
SCF不收敛：使用scf=xqc或调整积分格点（Int=UltraFine）；
虚频数不为1：检查虚频模式是否对应反应坐标，若不是，尝试手动调整结构后重新优化。

---

