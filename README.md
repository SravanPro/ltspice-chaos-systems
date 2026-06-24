# Analog Chaotic Oscillators in LTspice

This repository contains LTspice schematic implementations of classic chaotic systems—**Lorenz, Rössler, and Duffing oscillators**—using analog computing building blocks. Every system is constructed using operational amplifiers (op-amps) configured as integrators, summing amplifiers, and inverters, combined with a custom-designed discrete analog multiplier.

To prevent the op-amps from saturating and to keep the signals well within standard $\pm15\text{V}$ power rails, all system equations have been mathematically scaled down by a voltage scaling factor $f$.

---

## 🚀 System Equations & Voltage Scaling

The state variables are scaled as:

$$X = \frac{x}{f}, \quad Y = \frac{y}{f}, \quad Z = \frac{z}{f}$$

### 1. Lorenz Oscillator

* **Original:**

$$\frac{dx}{dt} = \sigma(y-x)$$

$$\frac{dy}{dt} = x(\rho-z)-y$$

$$\frac{dz}{dt} = xy-\beta z$$

* **Scaled ($f$):**

$$\frac{dX}{dt} = \sigma(Y-X)$$

$$\frac{dY}{dt} = X(\rho-fZ)-Y$$

$$\frac{dZ}{dt} = fXY-\beta Z$$

* **Standard Parameters:** $\sigma = 10$, $\rho = 28$, $\beta = \frac{8}{3}$

### 2. Rössler Attractor

* **Original:**

$$\frac{dx}{dt} = -y-z$$

$$\frac{dy}{dt} = x+ay$$

$$\frac{dz}{dt} = b+z(x-c)$$

* **Scaled ($f$):**

$$\frac{dX}{dt} = -Y-Z$$

$$\frac{dY}{dt} = X+aY$$

$$\frac{dZ}{dt} = \frac{b}{f} + Z(fX-c)$$

* **Standard Parameters:** $a = 0.2$, $b = 0.2$, $c = 5.7$

### 3. Duffing Oscillator (Chaos Mode)

* **General Form:**

$$\ddot{x} = ax - d\dot{x} - bx^3 - g\sin(\omega t)$$

* **Standard Form (rearranged):**

$$\ddot{x} + d\dot{x} - ax + bx^3 = -g\sin(\omega t)$$

* **Substituted Parameters ($a=1,\ b=1,\ d=0.1,\ g=0.85$):**

$$\ddot{x} + 0.1\dot{x} - x + x^3 = -0.85\sin(\omega t)$$


---

## ⚡ 4-Quadrant Analog Multiplier Design

---

## 🛠️ Circuit Implementation Blocks

Each chaotic system maps directly onto an analog schematic utilizing:

* **Integrators:** Configured using an op-amp with a feedback capacitor ($C$) and input resistor ($R$), where the time constant $\tau = RC$ establishes the time-scaling of the chaotic attractor.
* **Summing Amplifiers:** Used to combine the multiple differential terms (e.g., calculating $\sigma Y - \sigma X$).
* **Inverters:** Unity-gain inverting configurations used strictly to flip the polarity of state variables where negative coefficients are required.

---

## 📈 How to Run the Simulations

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/yourusername/chaotic-oscillators-ltspice.git
   ```
