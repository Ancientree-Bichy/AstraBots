# Acknowledgements and media provenance

The demonstrations use the Unitree Go2-W model, local QRC terrain assets,
MuJoCo, a frozen Go2-W rl_sar policy and Codex App Server with the recorded
`gpt-6-astra / medium` configuration.

- **Unitree MuJoCo / Go2-W:** robot geometry and simulation assets.
- **QRC terrain assets:** the flat and sloped courses used in these local experiments.
- **rl_sar:** existing locomotion policy; no new locomotion training was performed.
- **RB-TRG / Robio2026 simulation adapter:** existing model validation and policy integration; privileged reference execution was used only before evaluation.
- **MuJoCo, PyTorch and the scientific Python ecosystem:** simulation, inference and analysis.
- **Codex / Astra:** visual high-level navigation decisions through restricted tools.

The public repository contains selected rendered media and explanatory text.
It does not redistribute meshes, scene XML, trained weights, source experiment
code, login credentials or the raw research dataset. External projects and
assets retain their own terms; their use does not imply endorsement.

MP4s preserve the original selected replay files. Animated previews are excerpts
of those files, reduced in resolution, frame rate and colour count for README
display. No robot motion or scene content has been synthesized. Observer-camera
views and trajectories are labelled as post-run information rather than agent input.

Full source hashes, environment records, tests and raw recordings remain in the
local research archive. Original-content license status is in [LICENSE.md](LICENSE.md).
