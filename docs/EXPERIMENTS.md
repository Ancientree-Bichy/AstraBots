# Results and limitations

The public videos illustrate two experiment families: **16 known-map executions**
and **42 RGB-D executions**. All source records, including unsuccessful runs,
remain local. Preparing this showcase did not run new navigation trials or
change any outcome.

## Known-map path planning

Four fixed tasks, two friction seeds, Astra versus re-executed frozen RB-TRG
paths: **Astra 3/8; RB-TRG 4/8**. Astra received global elevation and true pose,
submitted its own world-coordinate waypoints, and used the same PurePursuit and
rl_sar stack as the baseline. This pilot retained cross-episode conversation
history. Its tools did not generate replacement routes for the model.

Scene 2 succeeded twice; Scene 3 succeeded once and failed once near the goal.
Scenes 1 and 4 ended in Astra abandonment. The matched Scene 3 outcomes also
differ in seed and prior conversation experience, so route choice is not isolated
as the cause. See [the complete planning results and caveats](KNOWN_MAP.md).

## RGB-D navigation: three later protocols

| Series | Tasks × seeds × actors | Astra successes | Local-rule successes |
| --- | --- | --- | --- |
| Ground corridors | 6 × 2 × 2 | 10/12 | 8/12 |
| Short terrain interior | 3 × 2 × 2 | 5/6 | 2/6 |
| Long terrain interior | 3 × 1 × 2 | 2/3 | 0/3 |

Each RGB-D task/seed pair shares a saved initial physics and policy state. Each Astra
trial starts a fresh context. The baseline is a simple memoryless local velocity
sampler using the same perception preprocessing, controller and action limits.
There is no model access to the privileged reference route.

### Ground corridors

The score difference comes from one flat detour task, repeated with two seeds.
Astra followed a wall around its endpoint; the local rule repeatedly pivoted.
Both Astra attempts at the sloped detour failed despite successful reference
execution before evaluation. These tasks mainly use ground corridors around the
QRC structures; this round alone does not demonstrate terrain climbing.

### Short terrain interior

Both endpoints are supported by the actual course. Astra completed the straight
task twice, the internal turn once out of two attempts, and the short incline
twice. The turn failure became mechanically trapped.

The incline's target surface rise is 0.454 m; measured body rise in Astra runs is
about 0.391/0.394 m, because stopping within the goal radius is allowed.

**The uphill comparison is sensitive to stopping rules.** The baseline requested
`finish` at 0.25 m, while success used a 0.30 m radius. Its uphill endpoints were
about 0.311/0.317 m away. Timeouts do not demonstrate an inability to climb, and
the 2/2 versus 0/2 score cannot be interpreted as an isolated locomotion advantage.
The agent prompt also did not explicitly state the numerical radius. Original
outcomes are retained without post-hoc rescoring.

### Long terrain interior

| Task | Reference route | Actual Astra travel | Astra outcome | Local-rule outcome |
| --- | --- | --- | --- | --- |
| Flat serpentine | 11.40 m | 12.37 m | success, 51.65 s | timeout, 240 s |
| Sloped serpentine | 9.15 m | 11.50 m | success, 55.16 s | timeout, 240 s |
| Flat wave | 9.45 m | 7.42 m | gave up, 1.71 m remaining | timeout, 240 s |

Times above are simulated. The two successful Astra runs took approximately
422 and 414 seconds of wall time. Only one seed was used per task, so these
trials do not estimate a stable long-range success rate.

This protocol increased the budget from 120 to 240 simulated seconds, made the
0.30 m finish radius explicit for both actors, and required remaining supported
by the course rather than using surrounding ground. It is therefore not a
path-length-only ablation. The reference lengths are not shortest-path ground truth.

## What the media shows

The MP4s are rendered from original recorded poses. In the RGB-D videos, the
actual camera observations remain on screen until their next observation time.
In the known-map videos, the right panel instead shows the actual latest global
map observation and the recorded waypoint submission. These are not new
navigation attempts. The 3-D observer cameras are post-run views; thinking waits
are omitted. The known-map pilot did receive its own past trajectory on the map,
whereas the RGB-D navigator received no global trajectory.

README GIFs are compressed excerpts at the same simulation-time speed. The short
incline preview includes its complete run; the other previews show the labelled
time ranges. The complete selected MP4s preserve unsuccessful maneuvers as well.

## Interpretation limits

- Scores belong to different protocols and should not be pooled; map access versus RGB-D is not a controlled ablation here.
- This is paused simulation, not continuous real-time control or real-robot deployment.
- The pilot's map/pose are truth inputs; RGB-D and proprioception in later runs are ideal, with truth-derived goal guidance.
- No new gait or footstep policy was learned; a fixed controller handles legs and wheels.
- The RGB-D baseline is a simple local rule; the known-map comparator is the dedicated RB-TRG planner's frozen path.
- Mixed contact priorities mean friction seeds do not uniformly randomize every contact.
- No ablation separates the effects of vision, memory and reasoning.
- Small task counts cannot establish broad generalization or statistical superiority.

The useful result is a working perception–decision–execution loop with both
successful detours and concrete failures. Reliable exploration and recovery
from arbitrary trapped configurations remain unresolved.

[Back to the demos](../README.md)
