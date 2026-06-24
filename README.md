# Analog Chaotic Oscillators in LTspice

This repository contains LTspice schematic implementations of classic chaotic systems—**Lorenz, Rössler, and Duffing oscillators**—using analog computing building blocks. Every system is constructed using operational amplifiers (op-amps) configured as integrators, summing amplifiers, and inverters, combined with a custom-designed discrete analog multiplier.

To prevent the op-amps from saturating and to keep the signals well within standard $\pm15\text{V}$ power rails, all system equations have been mathematically scaled down by a voltage scaling factor $f$.

---

## 🚀 System Equations & Voltage Scaling

The state variables are scaled as:

$$X = \frac{x}{f}, \quad Y = \frac{y}{f}, \quad Z = \frac{z}{f}$$

### 1. Lorenz Oscillator

<img width="1216" height="872" alt="Image" src="https://github.com/user-attachments/assets/de6228ce-e22b-4e7a-9422-8b003e878238" />

<img width="1072" height="843" alt="Image" src="https://github.com/user-attachments/assets/8174f580-4ee2-424a-b1a4-0da4ed5ffed8" />

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

<img width="1172" height="847" alt="Image" src="https://github.com/user-attachments/assets/9a9d68ee-4417-4597-b89a-d5e3de800d1a" />

<img width="1080" height="846" alt="Image" src="https://github.com/user-attachments/assets/05525ba7-a50a-4ee3-84cd-b0a526f778fc" />

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

<img width="1918" height="653" alt="Image" src="https://github.com/user-attachments/assets/ccf28fd6-61e2-4215-9077-d9b12462e13b" />

<img width="1917" height="830" alt="Image" src="https://github.com/user-attachments/assets/acf5499b-ccc6-4f0b-b8fc-c0b1d0c8baee" />

* **General Form:**

$$\ddot{x} = ax - d\dot{x} - bx^3 - g\sin(\omega t)$$

* **Standard Form (rearranged):**

$$\ddot{x} + d\dot{x} - ax + bx^3 = -g\sin(\omega t)$$

* **Substituted Parameters ($a=1,\ b=1,\ d=0.1,\ g=0.85$):**

$$\ddot{x} + 0.1\dot{x} - x + x^3 = -0.85\sin(\omega t)$$

---

## ⚡ 4-Quadrant Analog Multiplier Design

<img width="1455" height="828" alt="Image" src="https://github.com/user-attachments/assets/3dff9699-1d2e-40d0-8ce5-e9d1616b2f88" />

<img width="1915" height="840" alt="Image" src="https://github.com/user-attachments/assets/28ec6308-0486-45a3-b033-5583aa8d4db3" />

The multiplier is built around four BJTs exploiting the exponential $V_{BE}$–$I_C$ relationship. The goal is to produce $V_o = V_1 \cdot V_2$.

### BJT KVL Constraint

Applying KVL around the base-emitter loop of the four transistors and using $V_{BE} = \eta V_T \ln(I/I_S)$:

$$V_{BE_1} + V_{BE_2} = V_{BE_3} + V_{BE_4}$$

$$\ln\!\left(\frac{I_1 I_2}{I_S^2}\right) = \ln\!\left(\frac{I_3 I_4}{I_S^2}\right)$$

$$I_1 I_2 = I_3 I_4$$

### Collector Current Expressions

Using op-amp virtual short and KCL at each node:

$$I_1 = \frac{5}{R} + \frac{V_1}{2R}$$

$$I_2 = \frac{5}{R} + \frac{V_2}{2R}$$

$$I_3 = \frac{5}{R}$$

$$I_4 = \frac{5}{R} + \frac{V_o}{R_o} + \frac{V_1+V_2}{2R}$$

### Substitution into (1)

Expanding the left-hand side:

$$I_1 I_2 = \frac{25}{R^2} + \frac{5(V_1+V_2)}{2R^2} + \frac{V_1 V_2}{4R^2}$$

Expanding the right-hand side:

$$I_3 I_4 = \frac{25}{R^2} + \frac{5V_o}{R \cdot R_o} + \frac{5(V_1+V_2)}{2R^2}$$

The $\dfrac{25}{R^2}$ and $\dfrac{5(V_1+V_2)}{2R^2}$ terms cancel, leaving:

$$\frac{V_1 V_2}{4R^2} = \frac{5\,V_o}{R \cdot R_o}$$

### General Result

$$\boxed{V_o = \frac{R_o}{20R} \cdot V_1 V_2}$$

### With $R_o = 20R$ (i.e. $R_o = 20\text{k}\Omega$, $R = 1\text{k}\Omega$)

$$V_o = V_1 \cdot V_2$$

