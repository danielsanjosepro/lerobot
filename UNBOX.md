
```python
from lerobot.common.robot_devices.motors.configs import FeetechMotorsBusConfig
from lerobot.common.robot_devices.motors.feetech import FeetechMotorsBus

follower_config = FeetechMotorsBusConfig(
    port="/dev/ttyACM0",
    motors={
                    # name: (index, model)
                    "shoulder_pan": [1, "sts3215"],
                    "shoulder_lift": [2, "sts3215"],
                    "elbow_flex": [3, "sts3215"],
                    "wrist_flex": [4, "sts3215"],
                    "wrist_roll": [5, "sts3215"],
                    "gripper": [6, "sts3215"],
                },
)

follower_arm = FeetechMotorsBus(follower_config)
follower_arm.connect()
follower_arm.read("Present_Position")
```


```python
from lerobot.common.robot_devices.robots.configs import So100RobotConfig
from lerobot.common.robot_devices.robots.manipulator import ManipulatorRobot
from lerobot.common.robot_devices.cameras.configs import OpenCVCameraConfig
from lerobot.common.robot_devices.cameras.opencv import OpenCVCamera

robot = ManipulatorRobot(
    So100RobotConfig(
        leader_arms={"main": leader_config},
        follower_arms={"main": follower_config},
        calibration_dir="calibration/so100",
        cameras={
            "wrist_camera": OpenCVCameraConfig(10, fps=30, width=640, height=480),
             "static_camera": OpenCVCameraConfig(4, fps=30, width=640, height=480),

        },
    )
)
robot.connect()

from lerobot.common.robot_devices.robots.configs import So100RobotConfig
from lerobot.common.robot_devices.robots.manipulator import ManipulatorRobot
from lerobot.common.robot_devices.cameras.configs import OpenCVCameraConfig
from lerobot.common.robot_devices.cameras.opencv import OpenCVCamera

robot = ManipulatorRobot(
    So100RobotConfig(
        leader_arms={"main": leader_config},
        follower_arms={"main": follower_config},
        calibration_dir="calibration/so100",
        cameras={},
    )
)
robot.connect()
```



#### Data collection
Turn cloud off with `--control.push_to_hub=false`. Example data collection:

```
python lerobot/scripts/control_robot.py \
  --robot.type=so100 \
  --control.type=record \
  --control.fps=30 \
  --control.single_task="Grasp a lego block and put it in the bin." \
  --control.repo_id=datasets/lego_test \
  --control.tags='["so100","tutorial"]' \
  --control.warmup_time_s=5 \
  --control.episode_time_s=30 \
  --control.reset_time_s=30 \
  --control.num_episodes=2 \
  --control.push_to_hub=false
```

Press esc to stop recording.

If problem with encoding (seems to be an older Ubuntu problem), check out:
`https://github.com/huggingface/lerobot/issues/705`

=> fixed by changing video codec to libx264 (see commit)

#### Data playback

```
python lerobot/scripts/visualize_dataset_html.py \
  --repo-id datasets/lego_test
```

Throws some warning, but displays the dataset at `http://127.0.0.1:9090`