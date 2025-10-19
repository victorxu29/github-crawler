![Python](https://img.shields.io/badge/python-3.10+-blue.svg)
![PyTorch](https://img.shields.io/badge/pytorch-2.0+-red.svg)
![Conda](https://img.shields.io/badge/env-conda-green.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)


### Introduction

This is a REDQ implementation of the popular ml-agents crawler env as listed here : 
https://docs.unity3d.com/Packages/com.unity.ml-agents@4.0/manual/Learning-Environment-Examples.html#crawler
and the REDQ paper: https://arxiv.org/abs/2101.05982
From the docs the ml-agents crawler env is 
The reward function is now geometric meaning the reward each step is a product of all the rewards instead of a sum, this helps the agent try to maximize all rewards instead of the easiest rewards.
Body velocity matches goal velocity. (normalized between (0,1))
Head direction alignment with goal direction. (normalized between (0,1))
and 
Vector Observation space: 172 variables corresponding to position, rotation, velocity, and angular velocities of each limb plus the acceleration and angular acceleration of the body.
Actions: 20 continuous actions, corresponding to target rotations for joints.
with a benchmark mean reward of 3000.

This model aims to break through the benchmark mean reward using newer architecture from the traditional SAC / PPO. 


### Architecture
The model is a REDQ implementation with a 512 512 256 256 2 Actor and a 256 512 256 1 Critic.
The model uses a different set of hyperparameters than that of the REDQ paper and deeper networks.
### Preprocessing
The model takes obs from unity and runs through 5000 samples to calculate obs_mean and obs_std from them and then normalizes based on that. I recommend that you save the 
obs_mean and obs_std and reuse then if you are planning to continue training after stopping. For this end, an easy load_model() function has been implemented.
The model has 5000 warmup steps to fill the replay buffer before training actually starts.

### Results
Tried with both the redq hyperparameters as listed in the paper above (target_entropy = -action_dim, UTD_RATIO = 10 ~ 20) and our custom hyperparameters:


| Custom | Standard |
| :-------------- | :-------------: |
| LR: 3e-4   | LR: 3e-4   |
| Discount: 0.99  | Discount: 0.99  |
| Target Smoothing: 0.005  | Target Smoothing: 0.005  |
| Replay buffer size: 2 * 10^5  | Replay buffer size: 10^6 |
| UTD ratio: 35 | UTD ratio: 20  |
| Ensemble size: 10  | Ensemble size: 10  |
| Hidden layers: 4  | Hidden layers: 2  |
| Target Entropy: -action_dim  | Target Entropy: -action_dim * 0.3  |

Standard hyperparameters tended to plateau at around 1800 - 2000, while the custom hyperparameters were able to break through to 3000 after some tweaking.


<img width="700" height="500" alt="ChartGo_20251019182715" src="https://github.com/user-attachments/assets/7a4fa017-b539-4cba-ab4f-bf63ebd12c0d" />



### Usage
Install conda 
    
    conda env create -f environment.yml
    conda activate myenv

    
Copy the model weights(.pth) and copy them into your desired path. 
If you want to keep training use load_model = True and if you just want to see the simulation run with the pre-trained weights use the episodes_to_watch cell.

### Contact
If you have any contributions/ questions you can email runtues4@gmail.com

Developed by Victor Xu '29
