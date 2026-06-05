# Different-Layer-of-IHP-130-nm-PDK
## RPPD
<img width="720" height="376" alt="image" src="https://github.com/user-attachments/assets/b264c810-c631-4380-ba00-bd222f7a207c" />

This is describing **Rppd**, which in IHP PDK means a **P+ polysilicon resistor**.

The key sentence is:

> "Rppd utilizes unsalicided, p-doped gate polysilicon as resistor material."

Let's decode it.

* **R** → resistor
* **pp** → P-doped polysilicon
* **d** → doped

So the resistor is made using:

* **Polysilicon material**
* **P-type doping**
* **Unsilicided poly**

"Unsalicided" is important because:

Normally silicidation reduces resistance a lot (makes it behave more like metal). For a resistor, we actually want resistance, so the silicide layer is intentionally blocked.

The resistance follows:

[
R = R_s \times \frac{L}{W}
]

where:

* (R_s) = sheet resistance
* (L) = resistor length
* (W) = resistor width

The table gives:

* Sheet resistance ≈ **260 Ω/□ (target)**

So if:

* Length = 20 µm
* Width = 2 µm

Then:

[
R=260\times \frac{20}{2}
]

[
R=2600\ \Omega
]

R=R_s\frac{L}{W}

Other parameters in your table:

* **Matching coefficient = 15 nm**

  Lower value → better matching between two identical resistors. Important in analog layouts like current mirrors, differential pairs, charge pumps, PLL loop filters.

* **Temperature coefficients**

  TC tells how resistance changes with temperature.

  * TC1 = 170 ppm/K
  * TC2 = 0.4 ppm/K²

* **Recommended width ≥ 2 µm**

  This is for precision because narrow resistors suffer more from process variation and edge effects.

* **Max current density = 1.2 mA/µm**

  Going above this can create reliability problems.

For layout considerations (useful for your analog/PLL work):

* Use **common centroid/interdigitated structures** for matching-critical resistors.
* Keep orientation identical for matched resistors.
* Add **dummy poly segments** at edges.
* Use wider resistors (>2 µm) for precision.
* Keep away from noisy digital blocks.
* Avoid metal crossing over sensitive resistor regions if coupling is important.

# what is silicidation?

Silicidation (often called **salicide = self-aligned silicide**) is a fabrication process used to **reduce the resistance of polysilicon gates and source/drain regions** by forming a metal-silicon compound on top of silicon.

The word comes from:

* **Silicon** + **metal** → **silicide**
* **Self-aligned** → forms only where silicon is exposed

Typical metals used:

* Titanium (Ti)
* Cobalt (Co)
* Nickel (Ni)

The process is roughly:

1. Deposit metal over the wafer
2. Heat the wafer
3. Metal reacts only with exposed silicon/poly
4. Unreacted metal is removed

Result:

```text
Before silicidation:

Metal contact
     |
Poly gate
==========
Silicon

High resistance
```

```text
After silicidation:

Metal contact
     |
Silicide layer
==============
Poly gate
==========
Silicon

Lower resistance
```

Why use silicidation?

* Reduces **gate resistance**
* Reduces **source/drain resistance**
* Improves speed
* Lowers RC delay
* Improves transistor performance

Why do resistor structures like **Rppd** use **unsalicided poly**?

If silicide is added:

```text
Poly resistor resistance ↓↓↓ drastically
```

Example:

* Unsalicided poly: ~260 Ω/□
* Silicided poly: could become only a few Ω/□

Then the resistor would almost behave like a conductor instead of a resistor.

So in resistor layout, a **silicide block layer** is added:

```text
Poly resistor
+--------------------+
|    Silicide block  |
+--------------------+
```

This prevents salicide formation and preserves the intended resistance value.

For your PLL/analog work, this matters because:

* **Transistor gates** → usually silicided (lower resistance)
* **Precision resistors (Rppd)** → usually unsalicided
* Wrong salicide settings can completely change resistance values and break circuits like loop filters or bias networks.

  # Salblock Layer.

   he **SALBLOCK (salicide block)** layer is placed there intentionally.

Remember the Rppd description:

> "Rppd utilizes **unsalicided**, p-doped gate polysilicon as resistor material."

To make the poly remain **unsalicided**, the process must prevent silicide formation. That is the job of **SALBLOCK**.

Without SALBLOCK:

```text
Poly resistor
     ↓
Silicidation occurs
     ↓
Resistance becomes very low
     ↓
Resistor value becomes wrong
```

With SALBLOCK:

```text
Poly resistor
+-----------------------+
|      SALBLOCK         |
+-----------------------+
          ↓
Silicide cannot form
          ↓
Poly remains resistive
          ↓
Rppd keeps ~260 Ω/□
```

Physically during fabrication:

* Metal for silicidation is deposited over the wafer.
* Areas **without SALBLOCK** → metal reacts with silicon/poly → silicide forms.
* Areas **with SALBLOCK** → reaction is prevented.
* The poly underneath remains high resistance.

For your layout, the stack is conceptually:

```text
Rppd structure

Poly
+------------------+
|                  |
+------------------+

SALBLOCK
+------------------+
|                  |
+------------------+
```

A practical layout point for analog/VLSI:

* SALBLOCK should fully cover the resistor body.
* Usually it extends beyond the resistor edges according to DRC rules.
* If part of the resistor is accidentally left uncovered, local silicidation can occur and change the effective resistance.
* Contact regions may intentionally be treated differently depending on the PDK rule.

So if you saw **SALBLOCK inside Rppd**, that is not an extra feature — it is **essential**. Without it, **Rppd would not behave as a proper precision resistor.**

**When to resistor ar keeping side by side i can merge them till the the distance between gatepoly to gate poly distance is .18**
 <img width="335" height="360" alt="image" src="https://github.com/user-attachments/assets/5eeba1fe-d670-431f-9caa-bc7a4cc5f27c" />
