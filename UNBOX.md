
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
