Personal fork of DIAYN-PyTorch to use on a project in university. For the original repository please check: https://github.com/alirezakazemipour/DIAYN-PyTorch

# DIAYN-PyTorch

While intelligent  creatures can explore their environments and learn useful skills without supervision, many RL algorithms are heavily on the basis that acquiring skills is only achieved via defining them as explicit reward functions to learn.
    
Thus, in order to translate the natural behavior of creatures in learning **diverse** skills to a suitable mathematical formulation, DIAYN (Diversity is All You Need) was proposed for learning useful skills **without any domain-specific reward function**.
    
Instead of the real reward of the environment, DIAYN optimizes the following objective:

<p align="center">
  <img src="Results/equation.png", height=40>
</p>

that _`z`_ is the skill that the agent is learning and, since we desire learned skills to be **as diverse as possible**, _`z`_ is modeled by a Uniform random variable that has the highest standard variation.
    
The above equation simply implies that the reward of any diverse task is equal to measuring how hard recognizing the skill _`z`_ is, given the state _`s`_ that the agent has visited compared to the real distribution over _`z`_ (which is Uniform distribution in DIAYN paper.)   
The bigger r<sub>z</sub>(s, a) is, the more ambiguous skill _`z`_ is thus, the state _`s`_ should be visited more for task _`z`_ so, the agent finally acquires this skill.

Concurrently to learn r<sub>z</sub>(s, a), any conventional RL method can be utilized to learn a policy and DIAYN uses SAC.

**This repository is a PyTorch implementation of Diversity is All You Need and the SAC part of the code is based on [this repo](https://github.com/alirezakazemipour/SAC).**

## Results
> x-axis in all of the corresponding plots in this section are counted by number episode.

### Hopper
>number of skills = 20

similar to the environment's goal| similar to the environment's goal | Similar to an alternate goal
:-----------------------:|:-----------------------:|:-----------------------:
![](Gifs/Hopper/skill7.gif)| ![](Gifs/Hopper/skill8.gif)| ![](Gifs/Hopper/skill9.gif)

### BipedalWalker
>number of skills = 50

similar to the environment's goal| Emergent behavior| Emergent behavior
:-----------------------:|:-----------------------:|:-----------------------:
![](Gifs/BipedalWalker/skill11.gif)| ![](Gifs/BipedalWalker/skill12.gif)| ![](Gifs/BipedalWalker/skill40.gif)


### MountainCarContinuous
>number of skills = 20

similar to the environment's goal| Emergent behavior| Emergent behavior
:-----------------------:|:-----------------------:|:-----------------------:
![](Gifs/MountainCar/skill3.gif)| ![](Gifs/MountainCar/skill7.gif)| ![](Gifs/MountainCar/skill8.gif)


## Dependencies
- tensorboard == 2.16.2
- cffi == 1.16.0
- Cython == 0.29.37
- imageio==2.34.0
- gym == 0.17.3
- mujoco-py == 2.0.2.5
- numpy == 1.26.4
- opencv_contrib_python == 4.9.0.80
- psutil == 5.9.8
- torch == 1.13.0
- tqdm == 4.66.2
- patchelf == 0.17.2.1
- lockfile == 0.12.2
- box2d-py == 2.3.8
## Installation
```bash
pip3 install -r requirements.txt
```
## Usage
### How to run
```bash
usage: main.py [-h] [--env_name ENV_NAME] [--interval INTERVAL] [--do_train]
               [--train_from_scratch] [--mem_size MEM_SIZE]
               [--n_skills N_SKILLS] [--reward_scale REWARD_SCALE]
               [--seed SEED]

Variable parameters based on the configuration of the machine or user's choice

optional arguments:
  -h, --help            show this help message and exit
  --env_name ENV_NAME   Name of the environment.
  --interval INTERVAL   The interval specifies how often different parameters
                        should be saved and printed, counted by episodes.
  --do_train            The flag determines whether to train the agent or play
                        with it.
  --train_from_scratch  The flag determines whether to train from scratch or
                        continue previous tries.
  --mem_size MEM_SIZE   The memory size.
  --n_skills N_SKILLS   The number of skills to learn.
  --reward_scale REWARD_SCALE   The reward scaling factor introduced in SAC.
  --seed SEED           The randomness' seed for torch, numpy, random & gym[env].
```
- **In order to train the agent with default arguments, execute the following command and use `--do_train` flag, otherwise the agent would be tested** (You may change the memory capacity, the environment and number of skills to learn based on your desire.):
```shell
python3 main.py --mem_size=1000000 --env_name="Hopper-v3" --interval=100 --do_train --n_skills=20
```
You may change ``Hopper-v3`` to other environments such as ``BipedalWalker-v3`` and ``MountainCarcontinuous-v0``, as well as other arguments such as the number of skills to learn freely.

- **If you want to keep training your previous run, execute the following:**
```shell
python3 main.py --mem_size=1000000 --env_name="Hopper-v3" --interval=100 --do_train --n_skills=20 --train_from_scratch
```

## Environments tested
- [x] Hopper-v3
- [x] bipedalWalker-v3
- [x] MountainCarContinuous-v0

## Structure
```bash
├── Brain
│   ├── agent.py
│   ├── __init__.py
│   ├── model.py
│   └── replay_memory.py
├── Checkpoints
│   ├── BipedalWalker
│   │   └── params.pth
│   ├── Hopper
│   │   └── params.pth
│   └── MountainCar
│       └── params.pth
├── Common
│   ├── config.py
│   ├── __init__.py
│   ├── logger.py
│   └── play.py
├── Gifs
│   ├── BipedalWalker
│   │   ├── skill11.gif
│   │   ├── skill40.gif
│   │   └── skill7.gif
│   ├── Hopper
│   │   ├── skill2.gif
│   │   ├── skill8.gif
│   │   └── skill9.gif
│   └── MountainCar
│       ├── skill3.gif
│       ├── skill7.gif
│       └── skill8.gif
├── LICENSE
├── main.py
├── README.md
├── requirements.txt
└── Results
    ├── BipedalWalker
    │   ├── running_logq.png
    │   ├── skill11.png
    │   ├── skill40.png
    │   └── skill7.png
    ├── equation.png
    ├── Hopper
    │   ├── running_logq.png
    │   ├── skill2.png
    │   ├── skill8.png
    │   └── skill9.png
    ├── MountainCar
    │   ├── running_logq.png
    │   ├── skill3.png
    │   ├── skill7.png
    │   └── skill8.png
    └── r_z.png
```
1. _Brain_ dir consists of the neural network structure and the agent decision-making core.
2. _Common_ consists of minor codes that are common for most RL codes and do auxiliary tasks like logging and... .
3. _main.py_ is the core module of the code that manages all other parts and makes the agent interact with the environment.

## Reference

1. [_Diversity is All You Need: Learning Skills without a Reward Function_, Eysenbach, 2018](https://arxiv.org/abs/1802.06070)

## Acknowledgment
**Big thanks to:**

1. [@ben-eysenbach ](https://github.com/ben-eysenbach) for [sac](https://github.com/ben-eysenbach/sac).
2. [@p-christ](https://github.com/p-christ) for [DIAYN.py](https://github.com/p-christ/Deep-Reinforcement-Learning-Algorithms-with-PyTorch/blob/master/agents/hierarchical_agents/DIAYN.py).
3. [@johnlime](https://github.com/johnlime) for [RlkitExtension](https://github.com/johnlime/RlkitExtension).
4. [@Dolokhow](https://github.com/Dolokhow) for [rl-algos-tf2 ](https://github.com/Dolokhow/rl-algos-tf2).
