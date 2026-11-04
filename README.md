# 基于 ROS2 的移动机器人建图与自主导航

在室内场景完成二维建图、地图保存、初始定位和目标点导航。实验采用 Cartographer、AMCL 与 Nav2，报告和演示材料记录了路径规划、障碍物绕行和到达目标点的过程。

## 效果展示

[实验地图（PGM）](assets/maps/my_map.pgm) · [地图配置](assets/maps/my_map.yaml)

> 原始演示视频约 36 MB，因公开分发授权与画面审核尚未完成，暂未上传。原始材料保留在本地。

## 实现功能

- MS200 激光雷达与底盘里程计输入 Cartographer，生成二维占据栅格地图并保存为 PGM/YAML。
- 加载地图后由 AMCL 定位；在 RViz2 设置初始位姿和 Nav2 Goal。
- Nav2 使用 NavFn 全局规划及 DWB 局部控制，结合激光障碍物层执行避障导航。

## 个人工作

根据两份实验报告，可确认的工作包括 Ubuntu 22.04 / ROS2 Humble 环境部署、建图与导航流程配置和联调、遥控建图、地图保存，以及室内目标点导航演示验证。报告叙述了参数调整过程，但现有源码仅能证明当前参数值，无法独立核实每一项修改的作者及前后效果。

## 环境

RDK X5、MS200 激光雷达、全向移动底盘；Ubuntu 22.04、ROS2 Humble、Cartographer ROS、Nav2、RViz2。硬件型号依据实验报告，未通过本次硬件检查复核。

## 系统流程

```mermaid
flowchart LR
  L[MS200 /scan] --> C[Cartographer]
  O[底盘 /odom 与 TF] --> C
  C --> M[地图 PGM/YAML]
  M --> A[AMCL]
  L --> A
  O --> A
  A --> N[Nav2: NavFn + DWB]
  M --> N
  L --> N
  N --> V[/cmd_vel 底盘]
```

底盘启动文件发布 IMU 数据并启动 Madgwick 滤波，但当前 Cartographer 配置 `use_imu_data = false`，不能将 IMU 写成建图输入。

## 安装与运行

平台功能包的再分发许可未明确，本仓库暂不包含其源码。需先从平台原始提供方取得 `yahboomcar_nav`、`yahboomcar_bringup`、`yahboomcar_base_node`、`yahboomcar_description`、`yahboomcar_ctrl` 及 MS200 驱动，放入工作空间 `src/`。安装 ROS2 Humble、Cartographer ROS、Nav2 和相关驱动依赖后运行：

```bash
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
source install/setup.bash
```

小车端启动底盘 `ros2 launch yahboomcar_bringup yahboomcar_bringup_launch.py`。再启动建图 `ros2 launch yahboomcar_nav map_cartographer_launch.py`（该入口会启动雷达，勿重复启动）。远程端运行 RViz2，可使用 `ros2 launch yahboomcar_nav yahboomcar_mapping_launch.py`；建图时用 `ros2 run yahboomcar_ctrl yahboom_keyboard` 遥控。保存地图可用 `ros2 launch yahboomcar_nav save_map_launch.py map_path:=/absolute/path/my_map`。导航使用 `ros2 launch yahboomcar_nav navigation_dwb_launch.py map:=/absolute/path/my_map.yaml`，在 RViz2 中设置 `2D Pose Estimate` 和 `Nav2 Goal`。这些命令依据原始入口静态核对，未在本机执行。

## 关键配置与限制

当前配置为 `use_odometry = true`、`use_imu_data = false`；导航配置为 AMCL + NavFn + DWB。报告称使用 IMU 融合建图，与配置不符。导航启动默认地图为 `yahboom_map.yaml`，与报告提到的 `my_map.yaml` 不符，故上面显式传入地图路径。保存地图入口采用工作空间相对路径推算，建议显式传入 `map_path`。本仓库只保留一份实验地图；没有定位误差、成功率等测量记录。本次 Windows 环境未进行 ROS2 编译或真机复测。

## 依赖与致谢

平台功能包来自 Yahboom 小车配套材料，原始 `package.xml` 未声明明确许可证；取得再分发许可前不公开源码，也不为整仓库添加新许可证。算法与运行组件来自 [Cartographer](https://github.com/cartographer-project/cartographer)、[ROS2 Cartographer](https://github.com/ros2/cartographer_ros)、[Nav2](https://github.com/ros-navigation/navigation2) 与 ROS2。原始材料中的版权和署名未作改动。
