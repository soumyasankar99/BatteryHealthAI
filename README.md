
# 🔋 PyBaMM - Battery Modeling in Google Colab

## 📌 Overview
PyBaMM (Python Battery Mathematical Modelling) is an open-source library for battery simulation, providing fast and flexible tools for modeling **lithium-ion batteries, degradation, and electrochemical behaviors.** 
This repository demonstrates how to use PyBaMM in **Google Colab** for battery simulations.

---

## 🚀 Installation
To install PyBaMM in Google Colab or a local environment, run:

```bash
pip install pybamm
```

To verify installation:
```python
import pybamm
print(pybamm.__version__)
```

---

## 📖 Quick Start - Running a Simple Battery Model
The following script simulates a **lithium-ion battery** using the **Doyle-Fuller-Newman (DFN) model**:

```python
import pybamm

# Define the model
model = pybamm.lithium_ion.DFN()

# Create a simulation instance
sim = pybamm.Simulation(model)

# Solve for 1 hour
sim.solve([0, 3600])

# Plot results
sim.plot()
```

---

## 🔬 PyBaMM Battery Models
PyBaMM supports different levels of battery modeling complexity:

| Model | Description | Use Case |
|--------|-------------|----------|
| **SPM** (Single Particle Model) | Assumes uniform lithium concentration in particles | Quick estimations |
| **SPMe** (SPM with electrolyte) | Includes electrolyte dynamics | Faster than DFN, good balance |
| **DFN** (Doyle-Fuller-Newman) | Full electrochemical model | High accuracy, research |

To switch between models, simply change:
```python
model = pybamm.lithium_ion.SPM()  # Single Particle Model
```
```python
model = pybamm.lithium_ion.SPMe()  # SPM with Electrolyte
```
```python
model = pybamm.lithium_ion.DFN()  # Full DFN Model
```

---

## ⚙️ Customizing Simulations
### 1️⃣ Change Battery Parameters (Capacity, Current, Voltage Limits)
```python
param = pybamm.ParameterValues("Chen2020")  # Load Li-ion battery parameters
param.update({"Current function [A]": 10})  # Set a 10A discharge current

model = pybamm.lithium_ion.DFN()
sim = pybamm.Simulation(model, parameter_values=param)
sim.solve([0, 3600])
sim.plot()
```

### 2️⃣ Simulate Different Charging & Discharging Profiles
```python
import numpy as np
current_values = np.concatenate([-5*np.ones(1800), 5*np.ones(1800)])  # 30 mins discharge, 30 mins charge
time_values = np.linspace(0, 3600, len(current_values))

current_function = pybamm.Interpolant(time_values, current_values, pybamm.t)
param.update({"Current function [A]": current_function})

sim = pybamm.Simulation(model, parameter_values=param)
sim.solve([0, 3600])
sim.plot()
```

### 3️⃣ Simulate Battery Degradation
```python
model = pybamm.lithium_ion.DFN({"SEI": "ec reaction limited"})
sim = pybamm.Simulation(model)
sim.solve([0, 100000])  # Long-term simulation
sim.plot()
```

---

## 📚 Learn More
- 📄 **Official Documentation:** [PyBaMM Docs](https://pybamm.readthedocs.io/en/latest/)
- 📂 **GitHub Repo:** [PyBaMM GitHub](https://github.com/pybamm-team/PyBaMM)
- 📖 **Research Paper:** [Nature Article on PyBaMM](https://www.nature.com/articles/s41524-020-0285-6)

---

## 🤝 Contributions & Issues
Feel free to **open issues or contribute** to this repository if you have ideas for improvements or additional PyBaMM examples!

📧 For any queries, reach out via [GitHub Issues](https://github.com/pybamm-team/PyBaMM/issues).

🚀 **Happy Battery Modeling!** 🔋
