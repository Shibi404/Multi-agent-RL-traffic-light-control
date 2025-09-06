# Decentralized Reinforcement Learning Traffic Light Control

DQN branch contains the code for multi-agent DQN controlled intelligent traffic lights.

Currently DQN branch has been approved by the original Flow community and waiting to be merged.

For detailed changes compared to original Flow code, please refer to the [PR](https://github.com/flow-project/flow/pull/964)

# Citing 

For more theoretical details such as optima proof, system design, and performance comparison, please refer and cite our [paper](https://arxiv.org/pdf/2009.01502.pdf):
```
@ARTICLE{9275391,
  author={P. {Zhou} and X. {Chen} and Z. {Liu} and T. {Braud} and P. {Hui} and J. {Kangasharju}},
  journal={IEEE Transactions on Intelligent Transportation Systems}, 
  title={DRLE: Decentralized Reinforcement Learning at the Edge for Traffic Light Control in the IoV}, 
  year={2020},
  doi={10.1109/TITS.2020.3035841}}
```

# Demo:
https://youtu.be/p2sMtN_mW8s


Original instructions, please refer to [Flow](https://flow-project.github.io/) 

---

## 🚀 Quick Start (WSL2 + Ubuntu)

## 🖥️ 1. Install WSL2 + Ubuntu

On **Windows PowerShell (Admin):**
```powershell
wsl --install -d Ubuntu
```
Reboot if prompted, then open **Ubuntu** from the Start Menu.

Verify version:
```powershell
wsl -l -v
```
If Ubuntu shows **VERSION 1**, upgrade:
```powershell
wsl --set-version Ubuntu 2
```

Optional: configure WSL resources.  
Create `%UserProfile%\.wslconfig` (on Windows):
```ini
[wsl2]
memory=6GB
processors=2
swap=8GB
```
Apply changes:
```powershell
wsl --shutdown
```

---

## ⚙️ 2. Install dependencies in Ubuntu

Update packages:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential tmux sumo sumo-tools
```

Set **SUMO environment variables**:
```bash
echo 'export SUMO_HOME=/usr/share/sumo' >> ~/.bashrc
echo 'export PATH="$SUMO_HOME/bin:$PATH"' >> ~/.bashrc
echo 'export PYTHONPATH="$SUMO_HOME/tools:$PYTHONPATH"' >> ~/.bashrc
source ~/.bashrc
```

Check SUMO installation:
```bash
sumo --version
```

---

## 📦 3. Install Miniconda (Python 3.7)

Inside Ubuntu:
```bash
cd ~
curl -LO https://repo.anaconda.com/miniconda/Miniconda3-py37_4.12.0-Linux-x86_64.sh
bash Miniconda3-py37_4.12.0-Linux-x86_64.sh -b -p $HOME/miniconda3
```

Initialize Conda:
```bash
eval "$($HOME/miniconda3/bin/conda shell.bash hook)"
conda init bash
exec bash
```

---

## 📂 4. Clone this repo
```bash
git clone https://github.com/Shibi404/Multi-agent-RL-traffic-light-control.git
cd Multi-agent-RL-traffic-light-control
```

---

## 📋 5. Setup Conda environment

Create the environment from `environment.yml`:
```bash
conda env create -f environment.yml
conda activate flow
```

Install the repo:
```bash
pip install -e . --no-deps
```

---

## ▶️ 6. Run Simulations

### Ring road simulation
- **Headless:**
```bash
export SUMO_BINARY=sumo
python examples/simulate.py ring
```

- **With GUI (if WSLg available):**
```bash
export SUMO_BINARY=sumo-gui
python examples/simulate.py ring
```

### Traffic light grid (baseline, no RL)
```bash
export SUMO_BINARY=sumo-gui
python examples/simulate.py traffic_light_grid
```

### Train RL agent
```bash
cd examples
RAY_NUM_CPUS=1 OMP_NUM_THREADS=1 SUMO_BINARY=sumo python train.py multiagent_traffic_light_grid   --algorithm PPO   --num_cpus 1   --num_steps 3000   --rollout_size 100
```

### Visualize trained policy
```bash
CHECKPT_DIR=$(ls -dt ~/ray_results/*/* | head -n 1)
export SUMO_BINARY=sumo-gui
python train.py multiagent_traffic_light_grid   --algorithm PPO   --checkpoint_path "$CHECKPT_DIR"
```

---

## 🛠️ 7. Troubleshooting

- **WSL closes while idle** → run in `tmux`; check `.wslconfig` memory/CPU limits.  
- **Ray kills workers** → keep `N_CPUS=0` and `N_ROLLOUTS=1` in configs.  
- **protobuf descriptor errors** → use `protobuf==3.19.6`.  
- **`import flow` fails** → reinstall repo with `pip install -e . --no-deps`.  

---
