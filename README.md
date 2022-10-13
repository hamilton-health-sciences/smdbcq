# SMDP-BCQ

This repository contains an installable version of the SMDP-BCQ model.

## Installation

The model has been tested using Python 3.9 and PyTorch 1.9 on Linux, and Python 3.10 on Windows. To install using Pip:

    $ python3 -m pip install pytorch_smdbcq
    
Note that the demo requires [`d3rlpy`](https://github.com/takuseno/d3rlpy), which needs to build a C++ extension. On Windows, this means you will need the "C++ Worklow". Pip will tell you how to install this if you don't have it installed already.

## Demo

The demo runs the model for one epoch using the offline CartPole dataset in `d3rlpy`, and saves the model in `cartpole.pt` in the current directory:

    $ python3 -m smdbcq --demo
    
Further options can be found with the option `--help`.

### On your own data

More realistically, the model can be run on arbitrary data. The simplest way to do this is to create a `TensorDataset` containing the required data:

```python
from torch.utils.data import TensorDataset, DataLoader

from smdbcq import SMDBCQ

# Data
#   k (Tensor): size N x 1, the number of Markov timesteps for each of N transitions.
#   state (Tensor): size N x P, the observed state for each of N transitions.
#   option (Tensor): size N x 1, the observed option taken for each of N transitions.
#   next_state (Tensor): size N x P, the observed "next" state for each of N transitions.
#   reward (Tensor): size N x 1, the observed semi-Markov reward for each of N transitions.
#   not_done (Tensor): size N x 1, whether or not a trajectory terminates with each of N transitions (0 or 1).
dataset = TensorDataset(k, state, option, next_state, reward, not_done)

# Model
model = SMDBCQ(num_actions=len(torch.unique(option)),
               state_dim=state.shape[-1],
               device="cuda")

# Training loop
num_epochs = 1000
batch_size = 32
dataloader = DataLoader(dataset, batch_size=batch_size)
for _ in range(num_epochs):
    for batch in dataloader:
        model.train(batch)
```

## See also

Our application of this model to [warfarin dosing](https://github.com/hamilton-health-sciences/warfarin) (*under review*) and [experiments](https://github.com/mary-wu/smdp) validating its estimation (CHIL 2022).
