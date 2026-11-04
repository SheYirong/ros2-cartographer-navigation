# 鍩轰簬 ROS2 鐨勭Щ鍔ㄦ満鍣ㄤ汉寤哄浘涓庤嚜涓诲鑸?

鍦ㄥ鍐呭満鏅畬鎴愪簩缁村缓鍥俱€佸湴鍥句繚瀛樸€佸垵濮嬪畾浣嶅拰鐩爣鐐瑰鑸€傚疄楠岄噰鐢?Cartographer銆丄MCL 涓?Nav2锛屾姤鍛婂拰婕旂ず鏉愭枡璁板綍浜嗚矾寰勮鍒掋€侀殰纰嶇墿缁曡鍜屽埌杈剧洰鏍囩偣鐨勮繃绋嬨€?

## 鏁堟灉灞曠ず

[瀹為獙鍦板浘锛圥GM锛塢(assets/maps/my_map.pgm) 路 [鍦板浘閰嶇疆](assets/maps/my_map.yaml)

> 鍘熷婕旂ず瑙嗛绾?36 MB锛屽洜鍏紑鍒嗗彂鎺堟潈涓庣敾闈㈠鏍稿皻鏈畬鎴愶紝鏆傛湭涓婁紶銆傚師濮嬫潗鏂欎繚鐣欏湪鏈湴銆?

## 瀹炵幇鍔熻兘

- MS200 婵€鍏夐浄杈句笌搴曠洏閲岀▼璁¤緭鍏?Cartographer锛岀敓鎴愪簩缁村崰鎹爡鏍煎湴鍥惧苟淇濆瓨涓?PGM/YAML銆?
- 鍔犺浇鍦板浘鍚庣敱 AMCL 瀹氫綅锛涘湪 RViz2 璁剧疆鍒濆浣嶅Э鍜?Nav2 Goal銆?
- Nav2 浣跨敤 NavFn 鍏ㄥ眬瑙勫垝鍙?DWB 灞€閮ㄦ帶鍒讹紝缁撳悎婵€鍏夐殰纰嶇墿灞傛墽琛岄伩闅滃鑸€?

## 涓汉宸ヤ綔

鏍规嵁涓や唤瀹為獙鎶ュ憡锛屽彲纭鐨勫伐浣滃寘鎷?Ubuntu 22.04 / ROS2 Humble 鐜閮ㄧ讲銆佸缓鍥句笌瀵艰埅娴佺▼閰嶇疆鍜岃仈璋冦€侀仴鎺у缓鍥俱€佸湴鍥句繚瀛橈紝浠ュ強瀹ゅ唴鐩爣鐐瑰鑸紨绀洪獙璇併€傛姤鍛婂彊杩颁簡鍙傛暟璋冩暣杩囩▼锛屼絾鐜版湁婧愮爜浠呰兘璇佹槑褰撳墠鍙傛暟鍊硷紝鏃犳硶鐙珛鏍稿疄姣忎竴椤逛慨鏀圭殑浣滆€呭強鍓嶅悗鏁堟灉銆?

## 鐜

RDK X5銆丮S200 婵€鍏夐浄杈俱€佸叏鍚戠Щ鍔ㄥ簳鐩橈紱Ubuntu 22.04銆丷OS2 Humble銆丆artographer ROS銆丯av2銆丷Viz2銆傜‖浠跺瀷鍙蜂緷鎹疄楠屾姤鍛婏紝鏈€氳繃鏈纭欢妫€鏌ュ鏍搞€?

## 绯荤粺娴佺▼

```mermaid
flowchart LR
  L[MS200 /scan] --> C[Cartographer]
  O[搴曠洏 /odom 涓?TF] --> C
  C --> M[鍦板浘 PGM/YAML]
  M --> A[AMCL]
  L --> A
  O --> A
  A --> N[Nav2: NavFn + DWB]
  M --> N
  L --> N
  N --> V[/cmd_vel 搴曠洏]
```

搴曠洏鍚姩鏂囦欢鍙戝竷 IMU 鏁版嵁骞跺惎鍔?Madgwick 婊ゆ尝锛屼絾褰撳墠 Cartographer 閰嶇疆 `use_imu_data = false`锛屼笉鑳藉皢 IMU 鍐欐垚寤哄浘杈撳叆銆?

## 瀹夎涓庤繍琛?

骞冲彴鍔熻兘鍖呯殑鍐嶅垎鍙戣鍙湭鏄庣‘锛屾湰浠撳簱鏆備笉鍖呭惈鍏舵簮鐮併€傞渶鍏堜粠骞冲彴鍘熷鎻愪緵鏂瑰彇寰?`yahboomcar_nav`銆乣yahboomcar_bringup`銆乣yahboomcar_base_node`銆乣yahboomcar_description`銆乣yahboomcar_ctrl` 鍙?MS200 椹卞姩锛屾斁鍏ュ伐浣滅┖闂?`src/`銆傚畨瑁?ROS2 Humble銆丆artographer ROS銆丯av2 鍜岀浉鍏抽┍鍔ㄤ緷璧栧悗杩愯锛?

```bash
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
source install/setup.bash
```

灏忚溅绔惎鍔ㄥ簳鐩?`ros2 launch yahboomcar_bringup yahboomcar_bringup_launch.py`銆傚啀鍚姩寤哄浘 `ros2 launch yahboomcar_nav map_cartographer_launch.py`锛堣鍏ュ彛浼氬惎鍔ㄩ浄杈撅紝鍕块噸澶嶅惎鍔級銆傝繙绋嬬杩愯 RViz2锛屽彲浣跨敤 `ros2 launch yahboomcar_nav yahboomcar_mapping_launch.py`锛涘缓鍥炬椂鐢?`ros2 run yahboomcar_ctrl yahboom_keyboard` 閬ユ帶銆備繚瀛樺湴鍥惧彲鐢?`ros2 launch yahboomcar_nav save_map_launch.py map_path:=/absolute/path/my_map`銆傚鑸娇鐢?`ros2 launch yahboomcar_nav navigation_dwb_launch.py map:=/absolute/path/my_map.yaml`锛屽湪 RViz2 涓缃?`2D Pose Estimate` 鍜?`Nav2 Goal`銆傝繖浜涘懡浠や緷鎹師濮嬪叆鍙ｉ潤鎬佹牳瀵癸紝鏈湪鏈満鎵ц銆?

## 鍏抽敭閰嶇疆涓庨檺鍒?

褰撳墠閰嶇疆涓?`use_odometry = true`銆乣use_imu_data = false`锛涘鑸厤缃负 AMCL + NavFn + DWB銆傛姤鍛婄О浣跨敤 IMU 铻嶅悎寤哄浘锛屼笌閰嶇疆涓嶇銆傚鑸惎鍔ㄩ粯璁ゅ湴鍥句负 `yahboom_map.yaml`锛屼笌鎶ュ憡鎻愬埌鐨?`my_map.yaml` 涓嶇锛屾晠涓婇潰鏄惧紡浼犲叆鍦板浘璺緞銆備繚瀛樺湴鍥惧叆鍙ｉ噰鐢ㄥ伐浣滅┖闂寸浉瀵硅矾寰勬帹绠楋紝寤鸿鏄惧紡浼犲叆 `map_path`銆傛湰浠撳簱鍙繚鐣欎竴浠藉疄楠屽湴鍥撅紱娌℃湁瀹氫綅璇樊銆佹垚鍔熺巼绛夋祴閲忚褰曘€傛湰娆?Windows 鐜鏈繘琛?ROS2 缂栬瘧鎴栫湡鏈哄娴嬨€?

## 渚濊禆涓庤嚧璋?

骞冲彴鍔熻兘鍖呮潵鑷?Yahboom 灏忚溅閰嶅鏉愭枡锛屽師濮?`package.xml` 鏈０鏄庢槑纭鍙瘉锛涘彇寰楀啀鍒嗗彂璁稿彲鍓嶄笉鍏紑婧愮爜锛屼篃涓嶄负鏁翠粨搴撴坊鍔犳柊璁稿彲璇併€傜畻娉曚笌杩愯缁勪欢鏉ヨ嚜 [Cartographer](https://github.com/cartographer-project/cartographer)銆乕ROS2 Cartographer](https://github.com/ros2/cartographer_ros)銆乕Nav2](https://github.com/ros-navigation/navigation2) 涓?ROS2銆傚師濮嬫潗鏂欎腑鐨勭増鏉冨拰缃插悕鏈綔鏀瑰姩銆?

