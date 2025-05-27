# SARS传播模型：SEIRD动力学建模与预测

本项目基于经典的SEIRD（易感者-潜伏者-感染者-康复者-死亡者）传染病动力学模型，对2003年SARS疫情的传播趋势进行建模与预测。通过Python实现模型的参数优化和结果可视化，提供数据驱动的疫情分析工具。

---

## 模型概述

### SEIRD模型方程
模型将人群分为五类，通过以下微分方程组描述传播动态：

\[
\begin{cases}
\frac{dS}{dt} = -\beta(t) \frac{SI}{N} \\
\frac{dE}{dt} = \beta(t) \frac{SI}{N} - \sigma E \\
\frac{dI}{dt} = \sigma E - (\mu + d) I \\
\frac{dR}{dt} = \mu I \\
\frac{dD}{dt} = dI
\end{cases}
\]

其中：
- \( \beta(t) \) 为时变传染率（分段函数，分干预前、过渡期、干预后）
- \( \sigma = 1/\text{潜伏期} \), \( \mu = \text{康复率} \), \( d = \text{死亡率} \)
- 初始参数和边界范围见[模型方程](#模型方程)

---

## 实现流程

1. **数据准备**  
   - 从Excel文件（`新建XLSX工作表.xlsx`）读取历史疫情数据，包括累计确诊、疑似病例、康复和死亡人数。
   - 计算易感者 \( S = N - E - I - R - D \)。

2. **模型定义**  
   - 使用`scipy.integrate.solve_ivp`求解微分方程组。
   - 时变传染率 \( \beta(t) \) 的分段逻辑在`odefun`函数中实现。

3. **参数优化**  
   - 定义损失函数（均方误差），通过`scipy.optimize.minimize`优化参数。
   - 参数边界约束确保物理合理性（如潜伏期2-21天）。

4. **预测与可视化**  
   - 绘制历史数据拟合曲线和未来趋势预测图（Matplotlib实现）。

---

## 代码运行指南

### 依赖项
- Python 3.6+
- 必需库：`numpy`, `scipy`, `pandas`, `matplotlib`, `scikit-learn`

安装依赖：
```bash
pip install numpy scipy pandas matplotlib scikit-learn
```
或者参考requirement.txt
