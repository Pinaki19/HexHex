# HexHex v0.6

AlphaGo Zero adaptation of Hex. [Image of intend](https://user-images.githubusercontent.com/33026629/32346749-47b65b36-c049-11e7-9bac-08bc42cf9dae.png)

See [here](https://www.gwern.net/docs/rl/2017-silver.pdf) for full paper.


## Getting Started

### Option 1: Manual Installation

* Python >= 3.6

* Pytorch (see [here](https://pytorch.org/get-started/locally/) for installation info)

* install requirements.txt

### Option 2: Installation with pipenv

```
# install pipenv to manage dependencies
pip install pipenv 

# install dependencies
# use --skip-lock due to a recent regression in pipenv: https://github.com/pypa/pipenv/issues/2284
pipenv install -r requirements.txt --skip-lock

# activate virtual environment
pipenv shell 

# create model, training data, train model, and evaluate
./tests/run_example.py
```

### Execution

* run all scripts from root directory

* copy sample_config as initial configuration file `cp sample_config.ini config.ini`

* change parameters in `config.ini`

* run scripts
    - `python -m hex.training.repeated_self_training`

### Visualize Training with Tensorboard
Training will automatically create log files for tensorboard.
Those can be viewed with

`python3 -m tensorboard.main --logdir runs/`

### Loading Into hexgui
[hexgui](https://github.com/ryanbhayward/hexgui) can be used for interactive play as well.
It also allows replay view of `FF4` files generated by `hexboard.export_as_FF4`.
To load the hex ai into hexgui:
- go to `Program -> new program`
- enter command to start `play_cli.py`. Note that the working directory option is not supported. 
If you use `pipenv`, you might have to create a bash script for this purpose:
```bash
#!/bin/bash
cd /path/to/hex
pipenv run python play_cli.py
```
- The ai can then be used by `Program -> connect local program`

## Features

* board representation with logic + switch rule

* network to evaluate positions
  * output activation of network is sigmoid for each stone
  * these are probabilities of how likely that stone wins the game
  * loss function is bewteen prediction of selected stone and outcome of game

* scripts for
  * creating models with hyperparameters
  * batch-wise self-play to generate datasets
  * training and validating models
  * evaluating models against each other
  * ELO rating via `output_ratings` in `hex/elo/elo.py`
  * iterative training loop

* config to control plenty of hyperparameters

* playable gui `hex/interactive/interactive.py`

* trained model `models/five_board_wd0.001.pt` for 5x5 board

* puzzle validation set `data/puzzle.pt` for endgame strategy evaluation

* Monte Carlo tree search
