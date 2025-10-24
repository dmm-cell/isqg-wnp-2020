# isqg-wnp-2020
智能海洋预报
time=2020-08-01~2020-08-07, Lcut_km=350, taper_frac=0.10, depth_max_N2=1000, MATLAB R2023b, 文件=glo_WNP_2020.nc


# isqg-wnp-2020（仅数据 | no plots）

使用 **isQG** 方法，在一周时间窗内从 CMEMS/MERCATOR 海洋产品重建内部地转场。  
本仓库**只产出数据**，不生成任何图形；绘图由组员在下游完成。

---

## 目录结构
isqg-wnp-2020/
├─ README.md
├─ .gitignore
├─ LICENSE
├─ src/
│ ├─ run_isQG_eval_noplot.m # 函数：isQG 反演 + 纵向评估（不画图）
│ └─ isqg_param_sweep.m # 脚本：参数扫描 → 汇总 CSV（不画图）
└─ outputs/ # 结果输出目录（首次运行自动生成）


> 说明：仓库设计为“数据生产端”，默认**不提交**体量较大的 `.mat/.nc` 到 Git。  
> 若你确需提交，可以修改 `.gitignore`。

---

## 快速开始（单机运行）

1. **配置路径与时间窗**  
   打开 `src/isqg_param_sweep.m`，把 `fnc` 改为你的 NetCDF 路径（例如 `E:\glo\glo_WNP_2020.nc`），确认起止时间 `tStart/tEnd`（UTC）。

2. **运行（MATLAB R2023b）**
   ```matlab
   run('src/isqg_param_sweep.m')
结果（均在 outputs/）

isQG_eval_*.mat：核心三维场（u_isqg, v_isqg, c_tot, rho_prime, N2）及纵向评估指标。

sweep_summary_*.csv：多组参数 (Lcut_km, taper_frac) 的表层技能汇总表（相关/误差/能量），便于同学挑参数。
输出说明
三维核心场（数组顺序：(lon, lat, depth)）

u_isqg, v_isqg：isQG 速度（m s⁻¹）

c_tot：地转流函数样量（m² s⁻¹）

rho_prime：密度扰动（kg m⁻³）

N2（随深度 1D）：浮力频率平方（s⁻²）

许多绘图库默认 (lat, lon)，绘图时注意转置。
纵向评估指标（随深度的一维列向量）

r_u, r_v, r_spd：与观测速度（uo/vo 周均）的相关

rmse_u, rmse_v, rmse_spd：均方根误差

eke_obs, eke_isqg：观测/反演 EKE 剖面（0.5⟨u²+v²⟩）

评估默认只在内区计算（四周裁掉 interior_frac，默认为 0.10）以减弱边界效应。
常用参数（可在 opts 中设置）

Lcut_km：低波数截止（km，默认 350），用于滤除大尺度背景

taper_frac：二维余弦边界窗比例（默认 0.10）

depth_max_N2：估计 N² 的最大深度（默认 1000 m）

interior_frac：评估内区比例（默认 0.10）

use_gsw：若安装 GSW/TEOS-10，将用其计算 ρ 与 N²；否则回退至线性状态方程
仅数据、不画图

本仓库不包含任何绘图代码。
如需 CSV 切片或 NetCDF 导出，可在函数内加入选项或另外编写导出脚本，但默认仅保存 .mat 以保持仓库轻量。
