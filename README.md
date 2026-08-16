# SoccerTwos POCA Reinforcement Learning Agent

A portfolio project demonstrating **multi-agent deep reinforcement learning** with Unity ML-Agents. The agent was trained in the SoccerTwos environment using the **POCA trainer with self-play**, then exported to ONNX for Unity inference.

> This repository contains the final model artifacts, the training configuration, and the original run metadata. Large model files are stored with Git LFS.

## Results at a Glance

| Metric | Recorded result |
|---|---:|
| Final checkpoint | **9,566,754 steps** |
| Final self-play ELO | **1281.13** |
| Initial ELO | **1200.00** |
| Recorded training runtime | **~4 h 43 m** |
| Trainer | **POCA** |
| Final ONNX model | **~1.77 MB** |
| Final PyTorch checkpoint | **~28.43 MB** |

The ELO value is the **self-play ELO recorded by ML-Agents during training**. It is not presented as an external win-rate benchmark because this repository does not contain a separate held-out tournament evaluation.

## Project Goal

The goal was to train agents that can learn competitive soccer behavior through repeated interaction rather than hard-coded rules. The project demonstrates a complete reinforcement-learning workflow:

1. configure a Unity multi-agent environment;
2. train with POCA and self-play;
3. monitor policy and self-play metrics;
4. save model checkpoints;
5. export the final policy to ONNX for deployment in Unity.

## Training Architecture

```text
Unity SoccerTwos Environment
            |
            v
   Multi-agent observations
            |
            v
       POCA trainer
            |
      self-play loop
            |
            v
     Policy optimization
            |
            v
   PyTorch checkpoint (.pt)
            |
            v
     ONNX policy model
            |
            v
       Unity inference
```

## Training Configuration

The portable configuration is available at [`config/SoccerTwos.yaml`](config/SoccerTwos.yaml).

| Setting | Value |
|---|---:|
| Batch size | 2,048 |
| Buffer size | 20,480 |
| Learning rate | 0.0003 |
| PPO epsilon | 0.20 |
| Lambda | 0.95 |
| Epochs per update | 3 |
| Hidden units | 512 |
| Hidden layers | 2 |
| Discount factor (gamma) | 0.99 |
| Time horizon | 1,000 |
| Checkpoint interval | 500,000 steps |
| Configured maximum | 50,000,000 steps |
| Self-play save interval | 50,000 steps |
| Team-change interval | 200,000 steps |
| Self-play window | 10 |
| Latest-model play ratio | 0.5 |
| Initial ELO | 1,200 |
| Training time scale | 20x |

The run ended at approximately **9.57 million steps**, before the configured 50 million step ceiling.

## Recorded Environment

Training metadata reports:

- Python **3.10.11**
- Unity **2021.3.14f1**
- ML-Agents **1.1.0.dev0**
- PyTorch **2.3.0+cpu**
- NumPy **1.23.5**
- headless training with `--no-graphics`

The original run was executed on Windows using a compiled SoccerTwos environment.

## Repository Structure

```text
.
├── README.md
├── .gitattributes
├── .gitignore
├── config/
│   ├── SoccerTwos.yaml
│   └── resolved_training_config.yaml
├── docs/
│   └── training_summary.md
├── models/
│   ├── SoccerTwos.onnx
│   └── SoccerTwos-final.pt
└── training/
    ├── training_status.json
    └── timers.json
```

## Model Artifacts

### `models/SoccerTwos.onnx`
Final inference model exported by ML-Agents. This is the artifact intended for deployment through a Unity ML-Agents `Behavior Parameters` component.

### `models/SoccerTwos-final.pt`
Final PyTorch training checkpoint retained as a training artifact. Exact trainer-state resume behavior can depend on the original ML-Agents results directory and version, so the ONNX file should be treated as the primary portable deployment artifact.

## Reproducing the Training Setup

Install Git LFS before cloning so model artifacts are downloaded correctly:

```bash
git lfs install
git clone https://github.com/rizalhp/soccer-twos-poca-rl.git
cd soccer-twos-poca-rl
```

With a compatible Unity SoccerTwos executable and ML-Agents environment, a new run can be started with:

```bash
mlagents-learn config/SoccerTwos.yaml \
  --env=/path/to/SoccerTwos \
  --run-id=SoccerTwos \
  --no-graphics
```

The compiled Unity environment itself is not included in this repository.

## Training Evidence

The repository preserves the original run metadata rather than every intermediate model checkpoint:

- [`training/training_status.json`](training/training_status.json) records the checkpoint history and final self-play ELO.
- [`training/timers.json`](training/timers.json) records runtime, package versions, policy statistics, losses, and self-play metrics.
- [`docs/training_summary.md`](docs/training_summary.md) provides a recruiter-friendly interpretation of those artifacts.

Intermediate checkpoints, TensorBoard event files, duplicate generated configuration, and headless Unity player logs were removed from the current portfolio layout because they added substantial clutter without improving reproducibility.

## What This Project Demonstrates

- multi-agent reinforcement learning;
- Unity ML-Agents training workflow;
- POCA trainer configuration;
- competitive self-play;
- reinforcement-learning hyperparameter tuning;
- checkpoint and experiment management;
- PyTorch model training;
- ONNX model export for inference;
- Git LFS model artifact management;
- evidence-based documentation of experimental results.

## Limitations

- The recorded ELO is an internal self-play metric, not an independent benchmark.
- No separate evaluation set, fixed opponent tournament, or win-rate report is included.
- The Unity environment executable/source is not stored here.
- The run stopped at ~9.57M steps although the configuration allowed up to 50M steps.
- Training was CPU-based; no comparison against GPU training efficiency is claimed.

## Original Model Hub

This project was originally published on Hugging Face as [`rizalhp/poca-SoccerTwos`](https://huggingface.co/rizalhp/poca-SoccerTwos) and has been reorganized here as a cleaner GitHub portfolio case study.
