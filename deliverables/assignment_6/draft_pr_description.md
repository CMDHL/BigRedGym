# Draft PR title

`[DRAFT] Evaluate frozen Go2Trot-to-Go2Height composition`

# Summary

- Add and register the Go2Height task and its height command.
- Add reproducible MuJoCo evaluators for both policy checkpoints and their composition.
- Add frozen A->B direct switching and 0.25/0.50/1.00 s live action blending.
- Preserve the project boundary: no retraining and no learned composer in this draft.

# Current evidence

Checkpoints:

- A: `logs/go2trot/Nov16_20-08-42_trot_v1/model_500.pt`
- B: `logs/go2height/Oct16_18-01-11_height_v1/model_500.pt`

- A survived 30/30 reduced walking-gate cases but passed 0/30 because of jitter and gait-quality failures.
- Before transitions, A reached 0.17 and 0.32 m/s for 0.20 and 0.40 m/s commands.
- B alone: 4/8 successful stop-and-crouch transitions.
- Direct A->B: 4/8 successful transitions.
- 0.25/0.50/1.00 s blends: 7/8, 6/8, and 3/8 successful transitions.
- All composition cases: 0.01-0.02 m steady height RMSE and <=12.2 degrees tilt.
- The best 0.25 s blend had 0.54-0.59 rad peak joint-target steps; not ready for hardware.

# Next work in this Draft PR

- Run 50 randomized-phase trials for the selected 0.25 s blend and direct/B-alone baselines.
- Add the held-out domain-randomized readiness set.
- Investigate the large peak target step before requesting hardware testing.

# Validation

- `python -m pytest tests/unit_tests/test_go2_composition.py tests/unit_tests/test_go2height_readiness.py tests/unit_tests/test_go2trot_performance.py gym/envs/go2/test_go2trot.py -q`
- Result: `24 passed`.
- `python scripts/evaluate_go2height_readiness.py --replicas 4 --seconds 4 --transition_seconds 2 --output output/data/assignment_6_go2height_readiness.json`
- `python scripts/evaluate_go2_composition.py --replicas 4 --seconds 5 --transition_seconds 2 --output output/data/assignment_6_go2_composition.json`

# Known limitations

- MuJoCo CPU only; no hardware or cross-backend evidence.
- Four deterministic phase replicas per speed are diagnostic, not the final 50-trial assessment.
- A survives but fails the strict gait-quality gate, and the best blend remains below the 95% readiness threshold.
