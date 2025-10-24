# isqg-wnp-2020
智能海洋预报
time=2020-08-01~2020-08-07, Lcut_km=350, taper_frac=0.10, depth_max_N2=1000, MATLAB R2023b, 文件=glo_WNP_2020.nc


**快速开始**
1. 打开 `src/isqg_param_sweep.m`，将 `fnc` 改为你的 NetCDF 路径（如 `E:\glo\glo_WNP_2020.nc`），确认起止时间 `tStart/tEnd`。
2. 在 **MATLAB R2023b** 中运行：
   ```matlab
   run('src/isqg_param_sweep.m')
结果会写入 outputs/：
isQG_eval_*.mat：核心三维场（u_isqg,v_isqg,c_tot,rho_prime,N2）及纵向评估指标。
sweep_summary_*.csv：对多组 (Lcut_km, taper_frac) 的表层技能汇总（相关/误差/能量），便于同学挑参数。
**输出内容**
三维核心场（lon × lat × depth）
u_isqg, v_isqg：isQG 速度（m s⁻¹）
c_tot：地转流函数样量（m² s⁻¹）
rho_prime：密度扰动（kg m⁻³）
N2（depth）：浮力频率平方（s⁻²）
纵向评估指标（随深度的一维列向量）
r_u, r_v, r_spd：与观测速度（uo/vo 周均）的相关
rmse_u, rmse_v, rmse_spd：均方根误差
eke_obs, eke_isqg：观测/反演 EKE 剖面（0.5⟨u²+v²⟩）
参数记录：meta 中保存了所有关键选项（便于复现）
评估默认只在内区计算（四周裁掉 interior_frac，默认为 0.10）以减弱边界效应。
**参数说明**
Lcut_km：低波数截止（单位 km），用于滤除大尺度背景（默认 350）
taper_frac：二维余弦边界窗比例（默认 0.10）
depth_max_N2：估计 N² 的最大深度（默认 1000 m）
interior_frac：评估内区比例（默认 0.10）
**单位说明**
速度 u_isqg, v_isqg：m s⁻¹
c_tot：m² s⁻¹
rho_prime：kg m⁻³
N2：s⁻²
数组顺序一律为 (lon, lat, depth)。许多绘图库默认 (lat, lon)，同学绘图时注意转置。
**本仓库不含任何绘图代码。如需 CSV 切片或 NetCDF 导出，可在函数内部开关（或另写导出脚本）开启，但默认仅保存 .mat 以保持仓库轻量。**
