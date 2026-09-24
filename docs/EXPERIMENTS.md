# Results and limitations

The public videos are selected illustrations of completed experiments. All
42 official executions, including unsuccessful runs, remain in the local
research archive. Preparing this showcase did not run new navigation trials
or change any outcome.

| Series | Tasks × seeds × actors | Astra successes | Local-rule successes |
| --- | --- | --- | --- |
| Ground corridors | 6 × 2 × 2 | 10/12 | 8/12 |
| Short terrain interior | 3 × 2 × 2 | 5/6 | 2/6 |
| Long terrain interior | 3 × 1 × 2 | 2/3 | 0/3 |

Each task/seed pair shares a saved initial physics and policy state. Each Astra
trial starts a fresh context. The baseline is a simple memoryless local velocity
sampler using the same perception preprocessing, controller and action limits.
There is no model access to the privileged reference route.

## Ground corridors

The score difference comes from one flat detour task, repeated with two seeds.
Astra followed a wall around its endpoint; the local rule repeatedly pivoted.
Both Astra attempts at the sloped detour failed despite successful reference
execution before evaluation. These tasks mainly use ground corridors around the
QRC structures; this round alone does not demonstrate terrain climbing.

## Short terrain interior

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

## Long terrain interior

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

The full MP4s are rendered from the original recorded poses, with the actual
RGB-D observations held until their next observation time. They are not newly
generated navigation attempts. Observer cameras and global trajectories are
post-run visualizations, never model inputs. Thinking waits are omitted.

README GIFs are compressed excerpts at the same simulation-time speed. The short
incline preview includes its complete run; the other previews show the labelled
time ranges. The complete selected MP4s preserve unsuccessful maneuvers as well.

## Interpretation limits

- Scores belong to different protocols and should not be pooled.
- This is paused simulation, not continuous real-time control or real-robot deployment.
- RGB-D and proprioception are ideal; goal direction/distance is truth-derived assistance.
- No new gait or footstep policy was learned; a fixed controller handles legs and wheels.
- The baseline is simpler than a mature exploration/navigation system.
- Mixed contact priorities mean friction seeds do not uniformly randomize every contact.
- No ablation separates the effects of vision, memory and reasoning.
- Small task counts cannot establish broad generalization or statistical superiority.

The useful result is a working perception–decision–execution loop with both
successful detours and concrete failures. Reliable exploration and recovery
from arbitrary trapped configurations remain unresolved.

[Back to the demos](../README.md)
