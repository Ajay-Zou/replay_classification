# Replay-Content-Classification
Non-parametric categorization of replay content from multiunit spiking activity.

![](/replay_example.png)

![](/state_probability.png)

### References ###

> Deng, X., Liu, D.F., Karlsson, M.P., Frank, L.M., and Eden, U.T.
(2016). Rapid classification of hippocampal replay content for
real-time applications. Journal of Neurophysiology 116, 2221-2235.

### Installation ###

1. Install miniconda (or anaconda) if it isn't already installed. Type into bash:
```bash
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh;
bash miniconda.sh -b -p $HOME/miniconda
export PATH="$HOME/miniconda/bin:$PATH"
hash -r
```

2. Go to the local repository (`.../Replay-Content-Classification`) and install the anaconda environment for the repository. Type into bash:
```bash
conda update -q conda
conda info -a
conda env create -f environment.yml
source activate Replay-Content-Classification
python setup.py develop
```

### Example Usage ###

1. Classify replays into Inbound-Forward, Inbound-Reverse, Outbound-Forward, Outboud-Reverse using the clusterless decoder:
```python
from replay_classification import ClusterlessDecoder

decoder = ClusterlessDecoder(
    position=linear_distance,
    trajectory_direction=trajectory_direction,
    spike_marks=test_marks,
    replay_speedup_factor=16,
)

decoder.fit()

results = decoder.predict(ripple_marks)
```
2. Classify replays using sorted spikes:
```python
from replay_classification import SortedSpikeDecoder

decoder = SortedSpikeDecoder(
    position=linear_distance,
    trajectory_direction=trajectory_direction,
    spikes=test_spikes,
    replay_speedup_factor=16,
)

decoder.fit()

results = decoder.predict(ripple_spikes)
```
3. Decode only Inbound and Outbound by specifying the observation and state transition order:
```python
from replay_classification import ClusterlessDecoder

decoder = ClusterlessDecoder(
    position=linear_distance,
    trajectory_direction=trajectory_direction,
    spike_marks=test_marks,
    observation_state_order=['Inbound', 'Outbound'],
    state_transition_state_order=['Inbound', 'Outbound'],
    state_names=['Inbound', 'Outbound'],
    initial_conditions='Uniform',
)

decoder.fit()

results = decoder.predict(ripple_spikes)
```
4. Decode replay position:
```python
from replay_classification import ClusterlessDecoder

decoder = ClusterlessDecoder(
    position=linear_distance,
    trajectory_direction=np.ones_like(position), spike_marks=test_marks,
    observation_state_order=[1],
    state_transition_state_order=[1],
    state_names=['replay_position'],
    initial_conditions='Uniform',
)

decoder.fit()

results = decoder.predict(ripple_spikes)
```