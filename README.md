# Technical Program Walk-Through: NASA SC(2)-0714 Airfoil Analysis

## Overview: What This Program Does
This program analyzes the aerodynamic behavior of the NASA SC(2)-0714 airfoil using a 2-D source panel method implemented in MATLAB. Its purpose is to compute surface pressure distributions, lift coefficients, and the zero-lift angle of attack across a range of angles, and to compare those results with available NASA wind tunnel data.

## Step 1: Reading and Plotting the Airfoil Geometry
The program begins by reading airfoil surface coordinates from the Excel file `airfoil_data.xlsx`. These points define the exact geometry of the NASA SC(2)-0714 airfoil.
The coordinates are reordered so they:
<br />

* Start at the leading edge
* Proceed clockwise around the airfoil surface
* Form a single, closed boundary required by the panel method

The geometry is plotted immediately to verify that the airfoil was imported and ordered correctly.

<p align="center">
  <img src="https://i.imgur.com/8UOwnuo.png" height="80%" width="80%" alt="NACA SC(2)-0714 airfoil geometry"/>
  <br />
  <i>Figure 9.0: NACA SC(2)-0714 airfoil geometry</i>
</p>

## Step 2: Discretizing the Airfoil into Panels
Once the geometry is confirmed, the airfoil surface is divided into 128 straight panels. Each panel represents a small segment of the airfoil over which the flow properties are assumed constant.
<br />

For each panel, the program computes:
* Panel length ($\Delta S$)
* Panel orientation angle ($\theta_i$)
* Control point location ($\zeta_i, \eta_i$)

These geometric definitions follow the conventions shown in the report and are critical for correctly enforcing boundary conditions.

<p align="center">
  <img src="https://i.imgur.com/vVEG4GU.png" height="80%" width="80%" alt="Panel geometry and definitions"/>
  <br />
  <i>Figure 2.3: Panel geometry, control points, and angle definitions</i>
</p>

## Step 3: Defining Flow Conditions
The program evaluates the airfoil at five angles of attack, from $-3^{\circ}$ to $-7^{\circ}$. These values are stored in an array so the same analysis can be repeated automatically for each case without modifying the code.
The freestream velocity is normalized ($V_{\infty} = 1$), keeping all results nondimensional and simplifying the equations.

## Step 4: Building and Solving the Source Panel System
Each panel is modeled as a constant-strength source that influences the flow at every other panel. Using the governing equations of the source panel method, the program builds two coefficient matrices:
<br />

* One for normal velocity influence
* One for tangential velocity influence

The no-penetration boundary condition is enforced at every control point, producing a system of linear equations. Solving this system yields the source strength $q_i$ for each panel.

<p align="center">
  <img src="https://i.imgur.com/NxMMXxl.png" height="80%" width="80%" alt="Source panel equations"/>
  <br />
  <i>Figure 2.1: Key source panel equations used in the program</i>
</p>

## Step 5: Computing Velocity and Pressure
With the source strengths known, the program calculates the tangential velocity at each control point. The pressure coefficient ($C_p$) is then computed using Bernoulli’s equation.
This produces a full surface pressure distribution for each angle of attack. All intermediate values ($\zeta_i, q_i, V_{ti}, \text{ and } C_{pi}$) are saved in tables for inspection and verification.

## Step 6: Pressure Distribution Results
The program plots $C_p$ versus $\zeta$ for each angle of attack, showing how pressure varies along the airfoil surface.
Separate plots are generated for:
<br />

* **$-3^{\circ}$:** (See Figure 4.1)
* **$-4^{\circ}$:** (See Figure 4.2)
* **$-5^{\circ}$:** (See Figure 4.3)
* **$-6^{\circ}$:** (See Figure 4.4)
* **$-7^{\circ}$:** (See Figure 4.5)

The $-4^{\circ}$ case is especially important because it includes a direct comparison with NASA wind tunnel data, providing experimental validation of the numerical method.

<p align="center">
  <img src="https://i.imgur.com/GadNLiG.png" height="45%" width="45%" />
  <img src="https://i.imgur.com/z4rhnCW.png" height="45%" width="45%" />
  <br />
  <i>Figures 4.1 & 4.2: Pressure distributions at -3° and -4° (including NASA validation)</i>
</p>

<p align="center">
  <img src="https://i.imgur.com/xQwHMIE.png" height="30%" width="30%" />
  <img src="https://i.imgur.com/6GrcI38.png" height="30%" width="30%" />
  <img src="https://i.imgur.com/L4A8kPj.png" height="30%" width="30%" />
  <br />
  <i>Figures 4.3, 4.4, & 4.5: Pressure distributions at -5°, -6°, and -7°</i>
</p>

## Step 7: Lift Coefficient Calculation
The lift coefficient is calculated by numerically integrating the pressure forces acting on each panel. This summation accounts for panel length and orientation relative to the freestream.

<p align="center">
  <img src="https://i.imgur.com/cIhhsSc.png" height="60%" width="60%" alt="Lift coefficient summation equation"/>
  <br />
  <i>Figure 2.2: Lift coefficient summation equation</i>
</p>

## Step 8: Lift Curve and Zero-Lift Angle
After computing lift for each angle of attack, the program generates a $C_l$ vs. $\alpha$ plot. A smooth interpolation is applied to this curve to determine the zero-lift angle of attack, which identifies a zero-lift angle of approximately $-7.8^{\circ}$.

<p align="center">
  <img src="https://i.imgur.com/fE97zOu.png" height="80%" width="80%" alt="Lift coefficient vs angle of attack"/>
  <br />
  <i>Figure 5.0: Lift coefficient vs. angle of attack</i>
</p>

## Step 9: Verification and Reliability
The program’s correctness is verified in two ways:
<br />

1. **Agreement with NASA pressure data** at $\alpha = -4^{\circ}$ (Figure 4.2)
2. **Exact agreement with hand calculations** presented in Appendix C

These checks confirm that the numerical implementation is correct and that any remaining discrepancies are due to the limitations of inviscid, potential-flow theory.

## Final Remarks
This program demonstrates how the source panel method can be used to efficiently model airfoil aerodynamics, predict pressure distributions, and identify lift trends. While viscous effects are not included, the results closely match experimental data within the method’s expected limits.
