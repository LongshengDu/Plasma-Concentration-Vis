# **Pharmacokinetics Model Formulation**

Longsheng Du

April 26, 2025

## Fundamentals of Pharmacokinetics

Pharmacokinetics is the study of how drugs move through the body, including absorption, distribution, metabolism, and excretion. Plasma concentration (the amount of drug in the bloodstream at a given time) serves as a key measure of a medication's presence and activity in the body. For optimal therapeutic outcomes, many medications require plasma concentrations within a specific range:

* **Below minimum effective concentration (MEC)**: Insufficient therapeutic effect
* **Within therapeutic window**: Optimal clinical effect
* **Above maximum tolerated concentration**: Increased risk of adverse effects

Different drug formulations (immediate release, sustained release, extended release) are designed to achieve specific plasma concentration profiles to optimize therapeutic benefit.

## Modeling using Bateman Function

The Bateman function provides a mathematical model for describing drug concentration in plasma over time following oral administration. This first-order kinetic model balances two competing processes, absorption phase and elimination phase:

$$
C(t) = A \cdot (e^{-K_e \cdot t} - e^{-K_a \cdot t})
$$

Where:
* $C(t)$ = Plasma concentration at time $t$
* $A$ = Coefficient based on dose and distribution volume
* $K_e$ = Elimination rate constant (related to $T_{1/2}$)
* $K_a$ = Absorption rate constant
* $t$ = Time since dose administration

## Solving the Bateman Function

The pharmacokinetic model is characterized by three key parameters, which are properties of the medication typically determined through clinical trials or medical studies. 

* $T_{max}$: Time to maximum concentration (hours)
* $C_{max}$: Peak plasma concentration (ng/ml)
* $T_{1/2}$: Elimination half-life (hours)

To apply the Bateman function in practice, we need to determine the function parameters from known pharmacokinetic values:

1. **Elimination rate constant** ($K_e$): Calculated from ${T_{1/2}}$, since drug elimination follows exponential decay model:

   $$C(t) = C_0 \cdot e^{-K_e \cdot t}$$

   At $t = T_{1/2}$, $C(t) = \frac{C_0}{2}$, leading to:

   $$\frac{C_0}{2} = C_0 \cdot e^{-K_e \cdot T_{1/2}}$$

   $$K_e = \frac{\ln(2)}{T_{1/2}}$$

2. **Absorption rate constant** ($K_a$): Calculated from $T_{max}$ and $K_e$, since at peak concentration ($t = T_{max}$), the derivative of the concentration function is zero:

   $$\frac{dC(t)}{dt}\bigg|_{t=T_{max}} = 0$$

   $$\frac{K_a e^{-K_a T_{max}} - K_e e^{-K_e T_{max}}}{e^{-K_e T_{max}} - e^{-K_a T_{max}}} = 0$$

   Solving for $K_a$:

   $$\frac{\ln(K_a) - \ln(K_e)}{K_a - K_e} = T_{max}$$

3. **Coefficient** ($A$): Once $K_a$ and $K_e$ are known, $A$ is determined using $C_{max}$ and $T_{max}$:
   $$A = \frac{C_{max}}{e^{-K_e T_{max}} - e^{-K_a T_{max}}}$$

## Extended Implementation with Metabolic Factor

This implementation extends the basic Bateman model by incorporating several advanced features:

1. **Metabolic Factor**: An adjustment parameter that accounts for individual variations in drug metabolism, affecting how quickly the drug is absorbed and eliminated
   - Adjusted $T_{max} = T_{max} / \text{Metabolic Factor}$
   - Adjusted $C_{max} = C_{max} / \sqrt{\text{Metabolic Factor}}$
   - Adjusted $K_e = K_e \times \text{Metabolic Factor}$

2. **Multiple-dose regimens**: Simulating realistic medication schedules with varying frequency

3. **Flexible dosing patterns**:
   - Variable dosing frequency and interval throughout the day
   - Skip-day dosing patterns
   - Customizable initial dose timing

4. **Advanced metrics**:
   - Area Under the Curve (AUC) calculations for total drug exposure
   - Average concentration measurements
   - End-of-day concentration tracking

## Collected Sample Data

**Table 1: Sample A Simulation Result**

| Metabolic Factor | Adj. $\mathbf{C_{max}}$ (ng/ml) | Adj. $\mathbf{T_{max}}$ (h) | Adj. $\mathbf{T_{1/2}}$ (h) | EoD Conc. (ng/ml)|  AUC/d (ng*h/ml)  |
|:----------------:|:-------------------------------:|:---------------------------:|:---------------------------:|:----------------:|:-----------------:|
|     1.00         |            2000                 |            1.00             |            5.00             |        522       |     12787         |
|     0.75         |            2309                 |            1.33             |            6.67             |        883       |     16994         |
|     1.25         |            1789                 |            0.80             |            3.00             |        319       |     10005         |
1. Estimated properties of 100mg caffeine from coffee s at metabolic factor = 1
2. Simulation of 1 day, one light roasted coffee drink (12g beans) at 13:00
3. EoD Conc. represent concentration at end of day

**Table 2: Sample B Simulation Result**

|   Dosing   | $\mathbf{C_{max}}$ (ng/ml) | $\mathbf{T_{max}}$ (h) | $\mathbf{T_{1/2}}$ (h) | Max (ng/ml) | EoD Avg. (ng/ml) | Avg. (ng/ml)  | AUC/d (ng*h/ml) |
|:----------:|:--------------------------:|:----------------------:|:----------------------:|:-----------:|:----------------:|:-------------:|:---------------:|
|  2/d, q.8h |            40              |            2           |            21          |    192      |        152       |      146      |     3504        |
|  2/d, q.8h |            55              |            2           |            21          |    264      |        209       |      201      |     4818        |
|     1/d    |            80              |            2           |            21          |    209      |        137       |      148      |     3552        |
|  2/d, q.8h |            80              |            2           |            21          |    384      |        305       |      292      |     7008        |
|     1/d    |            85              |            3           |            21          |    224      |        152       |      162      |     3895        |
|     1/d    |            130             |            3           |            21          |    342      |        233       |      248      |     5957        |
|     1/d    |            120             |            5           |            21          |    324      |        236       |      244      |     5854        |
|    1/2d    |            120             |            5           |            21          |    203      |        121       |      126      |     3035        |
|    2/3d    |            120             |            5           |            21          |    261      |        145       |      155      |     3725        |
|    3/4d    |            120             |            5           |            21          |    295      |        184       |      190      |     4551        |
1. Metabolic factor = 0.75
2. Simulation over 14 days, daily initial dose at 08:00
3. EoD Avg. represent average concentration at end of each day
