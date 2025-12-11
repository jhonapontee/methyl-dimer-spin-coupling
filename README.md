# Orientation Effect on the Magnetic Interaction of Two Methyl Radicals

This repository contains the material for a group-theory and quantum-chemistry project on the **orientation dependence of the magnetic coupling** between two methyl radicals (•CH₃). We compare two idealized conformations of the dimer separated by 3.5 Å:

- **D₃d**: staggered (alternating) conformation  
- **D₃h**: eclipsed conformation  

The central question is how the relative orientation of the radicals affects the **magnetic exchange coupling J** (singlet–triplet gap) and how this can be rationalized using **symmetry and SALC analysis** combined with **DFT broken symmetry** and **CASPT2**.

---

## 1. Project overview

Magnetism at the microscopic level is tightly linked to the **electron spin** and to **spin–spin interactions** between localized magnetic centers. Depending on how these centers interact, the coupling can be:

- **Ferromagnetic**: parallel alignment of spins (high-spin ground state)
- **Antiferromagnetic**: antiparallel alignment of spins (low-spin ground state)

In this project we use a very simple model system:

- A single **methyl radical (•CH₃)**, described by group theory (point group D₃h) and SALCs.
- A **dimer of two methyl radicals** separated by 3.5 Å:
  - One geometry with **D₃d** symmetry (staggered).
  - One geometry with **D₃h** symmetry (eclipsed).

By analyzing the frontier orbitals and their symmetry, we study how the **orientation of the p_z-like magnetic orbitals** affects the exchange coupling J obtained from electronic-structure calculations.

---

## 2. Methods

### 2.1 Group theory and SALC analysis

The theoretical background is based on **group theory** and the construction of **Symmetry-Adapted Linear Combinations (SALCs)** of the hydrogen 1s orbitals and the carbon p orbitals:

- For the **monomer •CH₃ (D₃h)**:
  - Classification of atomic orbitals into irreducible representations.
  - Identification of the **magnetic p_z orbital** (typically transforming as A₂″ in D₃h).
  - Construction of SALCs for the σ-bonding framework.

- For the **dimer (D₃d and D₃h)**:
  - Construction of SALCs / fragment orbitals combining the p_z orbitals on both carbons.
  - Analysis of how these combinations transform under the symmetry operations of D₃d and D₃h.
  - Qualitative prediction of the relative stabilization of the **singlet** vs **triplet** states based on orbital overlap and symmetry-allowed interactions.

The pedagogical slides on **Unit 5: Orbitals and SALC** (H₂O, NH₃, BF₃, etc.) were used as a template to build the SALC analysis for the methyl radical and the dimer.

### 2.2 DFT calculations (Broken Symmetry)

All DFT calculations were performed with **ORCA**:

- **Electronic structure method**: UKS B3LYP  
- **Basis set**: def2-SVP  
- **SCF convergence**: `TightSCF`  
- **Spin states**:
  - **Triplet** state: high-spin reference for the two radicals.
  - **Broken-symmetry “singlet”** solution: antiferromagnetic coupling between the two sites.

For each geometry (D₃d and D₃h), we compute:

- The total energy of the **triplet**, \( E_T^{\text{DFT}} \).
- The total energy of the **broken-symmetry** state, \( E_{\text{BS}}^{\text{DFT}} \).

From these, we can define an approximate **DFT-BS singlet–triplet gap**:

\[
\Delta E_\text{DFT} \approx E_S - E_T \approx E_{\text{BS}}^{\text{DFT}} - E_T^{\text{DFT}}
\]

and use it to estimate an effective **J** parameter.

### 2.3 Multireference benchmark: CASSCF/CASPT2

Because biradicals present strong **static correlation**, a single-determinant DFT description may be insufficient. To obtain a more reliable reference for the magnetic coupling, we perform **CASSCF/CASPT2** calculations:

- **Active spaces**:
  - **CAS(2,2)**: minimal active space with two magnetic p_z orbitals and two unpaired electrons.
  - **CAS(2,6)**: extended active space including additional virtual/near-degenerate orbitals relevant for the radical center.

For each geometry (D₃d and D₃h), we compute:

1. **CASSCF** energies for singlet and triplet:
   - \( E_S^{\text{CASSCF}} \)
   - \( E_T^{\text{CASSCF}} \)

2. **CASPT2-corrected** energies:
   - \( E_S^{\text{CASPT2}} \)
   - \( E_T^{\text{CASPT2}} \)

The **CASPT2 magnetic coupling** J is then evaluated as:

\[
J_{\text{CASPT2}} = \frac{E_S^{\text{CASPT2}} - E_T^{\text{CASPT2}}}{2}
\]

This provides a high-level benchmark to critically assess the DFT-BS estimates.

---

## 3. Repository structure

A possible structure for this repository is:

```text
.
├─ docs/
│  ├─ slides/                 # Original teaching slides (PDFs) on SALC and group theory
│  ├─ project-topic.pdf       # Project description / assignment
│  └─ figures/                # Geometries, orbital plots, diagrams used in the report
├─ inputs/
│  ├─ CH3_radical/            # ORCA inputs for the isolated methyl radical
│  ├─ dimer_D3d/              # ORCA inputs for D3d (staggered) dimer
│  └─ dimer_D3h/              # ORCA inputs for D3h (eclipsed) dimer
├─ outputs/
│  ├─ d3d/                    # ORCA outputs for D3d calculations
│  └─ d3h/                    # ORCA outputs for D3h calculations
├─ caspt2/
│  ├─ d3d/                    # CASSCF/CASPT2 outputs for D3d
│  └─ d3h/                    # CASSCF/CASPT2 outputs for D3h
├─ analysis/
│  ├─ J_extraction/           # Scripts/notebooks to extract J from outputs
│  └─ group_theory/           # LaTeX or notebooks for SALC derivations and character tables
├─ report/                    # Project report (LaTeX/Overleaf source, PDF)
│  ├─ tex/
│  └─ pdf/
├─ README.md
├─ LICENSE
└─ .gitignore
