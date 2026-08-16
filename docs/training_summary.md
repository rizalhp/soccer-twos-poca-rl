# Training Summary

## Run Overview

This document summarizes the original SoccerTwos POCA training artifacts preserved in this repository.

| Item | Recorded value |
|---|---:|
| Final checkpoint step | 9,566,754 |
| Final self-play ELO | 1281.12698 |
| Initial self-play ELO | 1200.0 |
| Recorded runtime | 17,003.35 seconds (~4 h 43 m) |
| Trainer | POCA |
| ML-Agents | 1.1.0.dev0 |
| PyTorch | 2.3.0+cpu |
| Python | 3.10.11 |
| NumPy | 1.23.5 |
| Unity player | 2021.3.14f1 |

## Training Setup

The behavior used a POCA trainer with a 2-layer network of 512 hidden units per layer. Training used a batch size of 2,048, a buffer size of 20,480, a constant learning rate of 3e-4, gamma 0.99, and a time horizon of 1,000.

Self-play was enabled with:

- initial ELO: 1200;
- save interval: 50,000 steps;
- team-change interval: 200,000 steps;
- opponent swap interval: 2,000 steps;
- historical opponent window: 10;
- probability of playing the latest model: 0.5.

The engine ran headless at a time scale of 20. The original command recorded in the timing metadata used `mlagents-learn`, the SoccerTwos executable, `--no-graphics`, and `--force`.

## Training Outcome

The final training-status artifact reports a self-play ELO of **1281.13**, approximately 81 ELO points above the configured starting value. The final checkpoint was produced at **9,566,754 steps**.

This should be interpreted as evidence that the training loop and self-play ranking were functioning. It should **not** be interpreted as a standalone win-rate claim because no independent fixed-opponent evaluation or tournament results are stored with the run.

## Optimization Signals

The timing artifact contains policy entropy, policy/value losses, baseline estimates, episode length, extrinsic reward, group cumulative reward, learning-rate values, and self-play ELO updates. The final recorded learning rate remained 0.0003, consistent with the constant schedule in the configuration.

## Headless Player Warnings

The original Unity player log contained repeated shader warnings/errors such as unsupported Standard and UI shaders. The run was explicitly configured with `--no-graphics` and a null graphics device, so these messages are consistent with headless execution rather than evidence that the reinforcement-learning communication loop failed. The log also showed the ML-Agents communicator registering successfully.

Because the player log was mostly renderer noise and repeated messages, it was removed from the current portfolio layout after the relevant environment facts were captured here.

## Artifact Retention Decision

The original export contained multiple historical `.onnx` and `.pt` checkpoints plus a TensorBoard event file. For a portfolio repository, those intermediate artifacts added significant storage and navigation overhead while the training metadata already records the checkpoint history.

The cleaned repository therefore retains:

- the final ONNX inference model;
- the final PyTorch checkpoint;
- the portable training configuration;
- the original resolved configuration;
- training status metadata;
- timing/statistics metadata.

This preserves the useful evidence and reproducibility context while keeping the repository focused and recruiter-friendly.
