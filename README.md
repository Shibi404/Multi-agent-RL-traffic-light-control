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

### 1. Install WSL2 + Ubuntu
Open **Windows PowerShell (Admin)** and run:
```powershell
wsl --install -d Ubuntu
```
Reboot, then open **Ubuntu** from the Start Menu.

---

### 2. Install Dependencies
Inside Ubuntu, run:
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

---

### 3. Clone the Repository
```bash
git clone https://github.com/Shibi404/Multi-agent-RL-traffic-light-control.git
cd Multi-agent-RL-traffic-light-control
```

---

### 4. Setup Conda Environment
Make sure **Miniconda** is installed. Then run:
```bash
conda env create -f environment.yml
conda activate flow
pip install -e . --no-deps
```

---

## ▶️ Run Simulations

### Basic Ring Road Simulation
- **Headless (no GUI):**
```bash
export SUMO_BINARY=sumo
python examples/simulate.py ring
```

- **With GUI (if WSLg is available):**
```bash
export SUMO_BINARY=sumo-gui
python examples/simulate.py ring
```

---

### Baseline (No RL) Traffic Light Grid
```bash
export SUMO_BINARY=sumo-gui
python examples/simulate.py traffic_light_grid
```

---

### Train RL Agent on Traffic Light Grid
```bash
cd examples
RAY_NUM_CPUS=1 OMP_NUM_THREADS=1 SUMO_BINARY=sumo python train.py multiagent_traffic_light_grid   --algorithm PPO   --num_cpus 1   --num_steps 3000   --rollout_size 100
```

---

### Visualize Trained Policy
```bash
CHECKPT_DIR=$(ls -dt ~/ray_results/*/* | head -n 1)
export SUMO_BINARY=sumo-gui
python train.py multiagent_traffic_light_grid   --algorithm PPO   --checkpoint_path "$CHECKPT_DIR"
```

---

## 📖 References
- [Flow Documentation](https://flow-project.github.io/)  
- [SUMO User Docs](https://sumo.dlr.de/docs/)  
- [Ray RLlib Docs](https://docs.ray.io/en/latest/rllib/)  

---

💡 **Tip:** For better performance, adjust `--num_cpus`, `--num_steps`, and `--rollout_size` based on your system.

