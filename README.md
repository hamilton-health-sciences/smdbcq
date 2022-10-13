# SMDP-BCQ

This repository contains an installable version of the SMDP-BCQ model.

## Installation

The model has been tested using Python 3.9 and PyTorch 1.9. To install using Pip:

    $ python3 -m pip install smdbcq

## Example usage

The model can be estimated using built-in data from [d3rlpy](https://github.com/takuseno/d3rlpy) as follows (using CartPole as an example):

```python
from d3rlpy.datasets import get_cartpole

from smdbcq import SMDBCQ, wrap_d3rlpy_dataset

model = SMDBCQ(**params)
dataset = wrap_d3rlpy_dataset(get_cartpole())

for _ in range(numsteps):
    model.fit(dataset.sample())
```

## See also

Our application of this model to [warfarin dosing](https://github.com/hamilton-health-sciences/warfarin) (*under review*) and [experiments](https://github.com/mary-wu/smdp) validating its estimation (CHIL 2022).
