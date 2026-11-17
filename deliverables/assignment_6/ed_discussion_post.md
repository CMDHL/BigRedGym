# Training-free composition of Go2 walking and height control

**Team:** Huaijin Hu (hh833), Zoey Mo (zm73)

**Draft PR:** `<DRAFT_PR_URL>`  
**Work-in-progress recording:** `<SCREEN_RECORDING_URL_OR_ATTACHMENT>`

## Goal

We are testing whether frozen walking and height-control policies can be composed without retraining. Policy A will be Go2Trot and policy B is Go2Height. We will compare immediate A->B switching with live joint-target blending over 0.25, 0.50, and 1.00 s. Each policy receives its own observations. A learned composer policy is future work, not part of the current baseline.

## Current result

Both frozen checkpoints are now available. The Go2Trot checkpoint survived all 30 cases in a reduced fixed-command gate, but passed 0/30 because of joint jitter and gait-quality failures. At 2 Hz, linear-velocity RMSE was 0.12 m/s for a 0.50 m/s command and 0.30 m/s for 1.0 m/s.

For stop-and-crouch transitions from commanded 0.20 and 0.40 m/s, A actually reached 0.17 and 0.32 m/s before the transition. Across four gait phases at each speed:

- B alone: 4/8 successes.
- Direct A->B switching: 4/8.
- 0.25 s blend: 7/8.
- 0.50 s blend: 6/8.
- 1.00 s blend: 3/8.

All cases had 0.01-0.02 m steady height RMSE and at most 12.2 degrees tilt. However, the best 0.25 s blend still produced 0.54-0.59 rad peak step-to-step joint-target changes. This is **not ready for hardware**.

## Assessment plan

For B alone, immediate switching, and each blend duration, we will run at least 50 randomized-phase trials from 0, 0.10, and 0.20 m/s with a 0.35->0.20 m height step. Success requires no fall and simultaneous height error <=0.025 m, forward speed <=0.10 m/s, and yaw-rate error <=0.15 rad/s within 2.0 s, sustained for 0.5 s. We will report success with a 95% confidence interval, settling time, maximum tilt, peak joint-target change, and peak torque fraction.

Before hardware, we require >=95% success over 50 held-out domain-randomized trials, zero falls, maximum tilt <=15 degrees, and no joint-limit request. If that passes, we will ask for five supervised Go2 trials at <=0.10 m/s and <=0.05 m height change.

## Feedback requested

1. Is the <=0.10 m/s stopping threshold strict enough for the first supervised hardware test?
2. Should peak joint-target change or peak torque fraction be the primary smoothness/safety gate for blending?
3. Is it reasonable to treat A's gait jitter as a separate limitation while evaluating whether blending improves transition success?

Related work: Christmann, Luo, and Chen, “Expert Composer Policy: Scalable Skill Repertoire for Quadruped Robots,” ICRA 2024: https://arxiv.org/abs/2403.11412
