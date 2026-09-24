# How the agent controls the robot

## Two interfaces, not one shared observation contract

The **known-map pilot** gives Astra a global height map and true pose. Astra
submits world XY waypoints; PurePursuit converts them to body velocity before
the fixed locomotion policy executes them. Each submission advances at most 2 s.
Geometry queries evaluate the proposed route without searching for one. The same
Codex conversation retains development and cross-episode experience. See the
[known-map method and demonstrations](KNOWN_MAP.md).

The **RGB-D experiments** withhold the map and true pose. Astra directly chooses
body velocities from the current camera/proprioceptive inputs and ideal goal
guidance. Each episode has its own isolated model thread. The rest of this page
describes this RGB-D interface; its restrictions must not be attributed to the
earlier known-map pilot.

## RGB-D: three control layers

| Component | Responsibility | Update rate |
| --- | --- | --- |
| Astra | Interpret perception, choose a body velocity and duration, revise the next action | After each 0.25, 0.5 or 1 s movement |
| Frozen Go2-W rl_sar | Convert command and proprioception into 12 leg targets and 4 wheel-speed targets | 50 Hz |
| Low-level torque feedback and MuJoCo | Apply actuator control and advance physics | 500 Hz |

The simulator pauses while Astra thinks. The current velocity command is held
during its execution interval; the locomotion policy continues responding to
proprioception. Astra does not receive new frames midway through that interval.

## RGB-D observation boundary

The head camera is fixed to `base_link`, at `[0.32, 0, 0.10]` m, facing forward
and down 15°. It moves with body roll and pitch. RGB-D is 320 × 240 with a 70°
vertical field of view and valid depth from 0.15 to 5 m. Occlusion remains; depth
outside the valid range is unknown rather than assumed to be free ground.

The agent receives:

- Actual RGB and depth visualization, plus queries of current-frame metric depth and body-frame points.
- IMU, gravity direction, joint positions/velocities and the previous command.
- Ideal horizontal goal distance and heading-relative bearing, computed from simulator truth.
- Its own observation/action history in a fresh context for this episode.

It receives no map, world position, absolute heading, true base linear velocity,
scene filename, reference route or observer-camera image. No SLAM, odometry or
multi-frame geometry fusion is implemented. Episode history persists; no history
from another trial is supplied.

## RGB-D navigation tools

| Tool | Behavior |
| --- | --- |
| `observe()` | Read the cached current observation and budgets |
| `inspect_local(obs_id, region)` | Examine already-visible depth pixels; no hidden-terrain lookup |
| `act(obs_id, vx, wz, duration_s, note)` | Execute a bounded movement and return a fresh observation |
| `remember(note)` | Keep compact notes for this episode |
| `finish()` | Request zero-velocity settling and independent arrival evaluation |
| `give_up(reason)` | End explicitly with failure |

`vx` ranges from −0.15 to 0.4 m/s; `wz` is bounded by ±0.8 rad/s; lateral command
is zero. The observation ID prevents stale actions. Queries do not advance
physics. Ordinary text cannot move the robot or declare success.

The video's `note` is a brief model-authored explanation. Numeric action fields
drive the robot. A note is not a complete reasoning trace or proof of the
correctness of the underlying perception.

## RGB-D isolation and evaluation

Each episode uses a fresh `gpt-6-astra / medium` App Server thread in a restricted
filesystem/process namespace. Source assets, maps, evaluation logs and other
episode histories are not mounted. Shell, browsing and generic host-file tools
are disabled; only the navigation tools are exposed.

The evaluator has private simulator truth to check arrival, speed, attitude,
support and termination. Privileged reference routes certify task feasibility
before evaluation. Neither those routes nor post-run trajectories are supplied
to Astra. There is no automatic fallback to a route planner.

For example, the [long flat success](../assets/videos/long-flat.mp4) includes
retreat and a changed passing line; the [wave failure](../assets/videos/trapped-failure.mp4)
includes repeated ineffective recovery. Both behaviors depend on the fixed
locomotion controller's capabilities.

[Back to the demos](../README.md) · [Results and limits](EXPERIMENTS.md)
