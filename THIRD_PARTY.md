# Acknowledgements and media provenance

The demonstrations use Go2-W, MuJoCo and a frozen Go2-W rl_sar policy. The
known-map pilot uses rescue-terrain heightfields and the current Astra/Codex
conversation; the later RGB-D experiments use local QRC assets and fresh Codex
App Server threads with the recorded `gpt-6-astra / medium` configuration.

- **Unitree MuJoCo / Go2-W:** robot geometry and simulation assets.
- **QRC terrain assets:** the flat and sloped courses used in these local experiments.
- **RB-TRG / Robio2026 frozen elevation maps:** the four rescue-terrain scenes in the known-map planning pilot.
- **rl_sar:** existing locomotion policy; no new locomotion training was performed.
- **RB-TRG / Robio2026 simulation adapter:** model validation and policy integration. The known-map pilot uses PurePursuit and compares against frozen RB-TRG paths. In the RGB-D tasks, privileged reference execution is used only for feasibility checks before evaluation.
- **MuJoCo, PyTorch and the scientific Python ecosystem:** simulation, inference and analysis.
- **Codex / Astra:** route or motion decisions through the respective experiment interfaces.

The public repository contains selected rendered media and explanatory text.
It does not redistribute meshes, scene XML, trained weights, source experiment
code, login credentials or the raw research dataset. External projects and
assets retain their own terms; their use does not imply endorsement.

RGB-D MP4s preserve the previously rendered selected replay files. Known-map
MP4s are newly rendered from the original stored robot states and map images,
without physics stepping or policy inference. Animated previews are excerpts
reduced in resolution, frame rate and colour count. No new robot motion was
generated. All 3-D observer views are post-run information. Known-map observations
were supplied to that pilot's agent; RGB-D agents did not receive a global map.

Full source hashes, environment records, tests and raw recordings remain in the
local research archive. Original-content license status is in [LICENSE.md](LICENSE.md).
