# JSM2DPOL

Computes extinction, emission and polarisation of the three-component dust model by  
[Siebenmorgen R. (2023, A&A, 670A, 115)](https://arxiv.org/abs/2211.10146). Relative masses are derived from dust abundances and size distributions.  
The subroutine `sigtDark_EbvAvPol` computes the cross-sections ($cm^2/g$ ISM dust) and the relative mass of submicron-sized grains, also called "dark dust".

---

## 🌌 Dust Model

- **Cross-sections**: Based on [Siebenmorgen R. (2023, A&A, 670A, 115)](https://arxiv.org/abs/2211.10146)  
- **Column density**: Based on Eq. (2) and Eq. (3) from [Siebenmorgen & Chini (2023)](https://arxiv.org/abs/2311.03310)

---

## 🪶 Dust Components

1. **Nano-particles (VSG)**  
   - Graphite + PAHs (2175 Å bump, far-UV reddening)  
   - Nano-silicates (far-UV)
   - mid IR continuum emission and PAH bands

2. **Large grains**  
   - Prolate ($a/b > 1$), aligned (IDG model) or PDG (set via d.Q files)
   - Optical constants:  
     - aSi: amorphous silicates [Demyk et al. (2022)](https://arxiv.org/abs/2209.06513)  
     - aC: amorp[hous carbon [Zubko (1996)](https://ui.adsabs.harvard.edu/abs/1996MNRAS.282.1321Z/abstract)  
   - Radii: 6–250 nm ([Mathis et al., 1977](https://ui.adsabs.harvard.edu/abs/1977ApJ...217..425M/abstract))
   - Grey extinction in the far UV and about linear decline in the optical, far IR emission

3. **Submicron-sized grains (Dark Dust)**  
   - Fluffy (10% vacuum) composites of aC and aSi grains 
   - Radii: 250 nm - 3 µm
   - Grey extinction in the optical and about linear decline in the IR, submillimetre emission
   
---

## 📥 Input Files (in `./Input/`)

- `d.Q*` files: Efficiency Q data for grain types, using the Bruggeman mixing rule when needed, e.g. for vacuum inclusion (porosity) 
  - `d.QellipaC`  
  - `d.QellipSi`  
  - `d.QellipDark`
  - `d.Qmie_vGr`
  - `d.Qmie_vSi`

- `jsmTaufit.inp`:  
  Contains `tauV`, `Ebv_obs`, and `P_serk` (Serkowski polarisation fit)

- `w12_vv2_Ralf_283wave.dat`:  
  Wavelength grid

- `jsm12fit.inp`:  
  Dust parameters for fitting reddening:
  - Abundances: `abuc`, `abusi`, `abuvsi`, `abucvgr`, `abucpahs`
  - Sizes: `qmrn`, `alec`, `alesi`, `arad_polmin_aC`, `arad_polmin_Si`, `arad_polmax`, `aled`

- `PAH2170.wq`:  Drude Parameters (x0 (mic) and gamma) of PAH contributons to the 2175AA bump

---

## 📤 Output Files (in `./Output/`)

- `Kappa.out`: Absorption (`sa_*`) and scattering (`ss_*`)  
- `Kappa4fit.out`: Observational wavelength range  
- `PolKappa.out`: Polarised cross-sections (`sigp_*`, dark dust set to zero)  
- `tau4fit.out`: Extinction/reddening curves (absolute and normalized)
- `emis.out`: Flux in [erg/s/Hz/ster] pro g IM as a function of wavelength for total and the indiviudal dust component
- `emipol.out`: Polarised and total flux in [erg/s/Hz/ster] pro g IM as a function of wavelength. Total flux emis, total polarised flux
  and polarised flux of aC, aSi and dark dust components

---

## 📐 Cross-Section Notation

All cross-sections ($K$) are in units of $cm^2/g$-dust, scaled to optical depth via `Nl`, `Nd`.

---

## 📁 Project Structure

    jsm2Dpol/
    ├── src/
    │   ├── jsm2Dpol_main.f90         # Main program
    │   ├── jsm2Dpol_mods.f90         # Config/constants/functions modules
    │   ├── jsm2Dpol_utils.f90        # General-purpose utilities
    │   └── jsm2Dpol_subroutines.f90  # Physics subroutines and shared variables
    ├── example/                      
    │   ├── Input/                    # Sample set of inputs
    │   ├── Output/               
    │   └── af90.j               
    ├── Makefile                      # Build script
    ├── LICENSE
    └── README.md   

---

## ⚙️ Compilation

Build the executable `af90.j`:

```bash
make
# or:
make jsm
```

Move the executable next to your `./Input/` and `./Output/` folders:

```bash
./af90.j
```

---

## 🧾 Version History
- **05/2025** — Refactored modules, added reading dust parameters from d.Q files 
- **04/2025** — Translated from FORTRAN77 to FORTRAN90  
- **26.11.2024** — Updated version  
- **24.04.2023** — Unified SpT with GAIA parallax distances  
- **20.09.2022** — Updated optical constants ([Demyk+22](https://arxiv.org/abs/2209.06513))  
- **04.12.2021** — Introduced dark dust  
- **1991** — Original release (Siebenmorgen PhD; [Siebenmorgen & Kruegel 1992, A&A](https://adsabs.harvard.edu/full/1992A%26A...259..614S))

---

## 👤 Credits

Please contact [Ralf Siebenmorgen](mailto:Ralf.Siebenmorgen@eso.org) for any issues.  
