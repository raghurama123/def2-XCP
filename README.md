# def2-XCP: definitions of Exchange-Correlation functionals, Please!

## Table of Contents
1. [Overview](#overview)
2. [Exact Exchange](#exact-exchange)
3. [Generalized Gradient Approximations](#generalized-gradient-approximations)
4. [Hybrid DFT Functionals](#hybrid-dft-functionals)
5. [Range-Separated Hybrid DFT Functionals](#range-separated-hybrid-dft-functionals)
   - [Two-parameter range-Separated Hybrid DFT Functionals](#two-parameter-range-separated-hybrid-dft-functionals)
   - [Three-parameter range-Separated Hybrid DFT Functionals](#three-parameter-range-separated-hybrid-dft-functionals)
   - [Four-parameter range-Separated Hybrid DFT Functionals](#four-parameter-range-separated-hybrid-dft-functionals)
6. [B3LYP, LC-BLYP, CAM-B3LYP](#blyp-based-functionals)
7. [References](#references) 

---

Definitions of various exchange-correlation (XC) functionals used in density functional theory (DFT) are collected here.

## Overview

In DFT, the exchange-correlation functional $E_{\text{XC}}$ plays a crucial role in determining the total energy of a system. It is typically divided into exchange $E_{\text{X}}$ and correlation $E_{\text{C}}$ components.

$$ E_{\text{XC}} = E_{\text{X}} + E_{\text{C}} \tag{1} $$


## Exact Exchange

The Hartree--Fock (HF) model completely captures the exchange interaction; hence, the exact exchange implies the exchange term calculated with HF. We can define the exact exchange energy term as given in [this page on wikipedia](https://en.wikipedia.org/wiki/Hybrid_functional)

$$E_{\text{X}}^{\text{exact}} = E_{\text{X}}^{\text{HF}} = -\frac{1}{2} \sum_i \sum_j \int \int \phi_i^{\*}(r_1) \phi_j^{\*}(r_2) \frac{1}{r_{12}} \phi_j(r_1) \phi_i(r_2) \tag{2} $$

or equivalently as

$$E_{\text{X}}^{\text{exact}} = E_{\text{X}}^{\text{HF}} = -\frac{1}{2} \sum_i \sum_j \int \int \phi_i^{\*}(r_1)\phi_j(r_1)\frac{1}{r_{12}} \phi_i(r_2)\phi_j^{\*}(r_2) \tag{3} $$

## Generalized Gradient Approximations

### `PBEPBE`, Gaussian 16 C.01 (aka PBE)

```
 IExCor= 1009 DFT=T Ex=PBE Corr=PBE ExCW=0 ScaHFX=  0.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
```

## Hybrid DFT Functionals

<a id="eq4"> </a>

$$
 E_{\text{XC}}^{\text{hDFT}} = P_2 E_{\text{X}}^{\text{HF}} + P_1[ P_4 E_{\text{X}}^{\text{local}} + P_3 \Delta E_{\text{X}}^{\text{non-local}} ] + P_6 E_{\text{C}}^{\text{local}} + P_5 \Delta E_{\text{C}}^{\text{non-local}} \tag{4}
$$


- In Gaussian 16 C.01, $E_{\text{X}}^{\text{local}}$ is the Slater-exchange functional,  $E_{\text{X}}^{\text{S}}$, keyword is `S`
- $P_1$ is usually set to either 1.0 or 0.0. $P_1=0.0$ implies $E_{\text{X}}^{\text{DFA}}=0$. Let's assume that we'll always have $P_1=1.0$.
- $P_2$ is printed in the output file as `ScaHFX`.
- `ScaDFX` has four values in the order $P_4$, $P_3$, $P_6$, and $P_5$.

### `B3LYP`, Gaussian 16 C.01 (aka B3LYP-G)
```
 IExCor=  402 DFT=T Ex+Corr=B3LYP ExCW=0 ScaHFX=  0.200000
 ScaDFX=  0.800000  0.720000  1.000000  0.810000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
```
Various terms entering [Eq. (4)](#eq4) are

-  $E_{\text{X}}^{\text{local}} =  E_{\text{X}}^{\text{LSDA}}$
-  $\Delta E_{\text{X}}^{\text{non-local}} =  E_{\text{X}}^{\text{B}}- E_{\text{X}}^{\text{LSDA}}$
-  $E_{\text{C}}^{\text{local}} =  E_{\text{C}}^{\text{LSDA}}$
-  $\Delta E_{\text{C}}^{\text{non-local}} =  E_{\text{C}}^{\text{LYP}}-E_{\text{C}}^{\text{LSDA}}$

$$
 E_{\text{XC}}^{\text{B3LYP}} = 0.2 E_{\text{X}}^{\text{HF}} + 0.8 E_{\text{X}}^{\text{LSDA}} + 0.72 \Delta E_{\text{X}}^{\text{B}} +  E_{\text{C}}^{\text{LSDA}} + 0.81 \Delta E_{\text{C}}^{\text{LYP}}
$$

By expanding the $\Delta$-terms, we get

$$
 E_{\text{XC}}^{\text{B3LYP}} = 0.2 E_{\text{X}}^{\text{HF}} + 0.8 E_{\text{X}}^{\text{LSDA}} + 0.72 [ E_{\text{X}}^{\text{B}} - E_{\text{X}}^{\text{LSDA}}] +  E_{\text{C}}^{\text{LSDA}} + 0.81 [ E_{\text{C}}^{\text{LYP}} - E_{\text{C}}^{\text{LSDA}} ]
$$

which finally results in

$$
 E_{\text{XC}}^{\text{B3LYP}} = 0.2 E_{\text{X}}^{\text{HF}} + 0.08 E_{\text{X}}^{\text{LSDA}} + 0.72 E_{\text{X}}^{\text{B}} +  0.19 E_{\text{C}}^{\text{LSDA}} + 0.81 E_{\text{C}}^{\text{LYP}}
$$

- With IOp control[^1][^2], one can _construct_ using `BLYP/basis  IOp(3/76=1000002000) IOp(3/77=0720008000) IOp(3/78=0810010000)`

- Here is a [nice comparison of B3LYP definitions across software](https://chemistry.stackexchange.com/questions/39411/definition-of-the-b3lyp-functional-in-common-qc-programs).

### `PBE1PBE`, Gaussian 16 C.01 (aka PBE0)

```
 IExCor= 1009 DFT=T Ex+Corr=PBE1PBE ExCW=0 ScaHFX=  0.250000
 ScaDFX=  0.750000  0.750000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
```

Various terms entering [Eq. (4)](#eq4) are

-  $E_{\text{X}}^{\text{local}} =  E_{\text{X}}^{\text{LSDA}}$
-  $E_{\text{X}}^{\text{non-local}} =  E_{\text{X}}^{\text{B}}$
-  $E_{\text{C}}^{\text{local}} =  E_{\text{C}}^{\text{LSDA}}$
-  $E_{\text{C}}^{\text{non-local}} =  E_{\text{C}}^{\text{LYP}}$

$$
 E_{\text{XC}}^{\text{B3LYP}} = 0.25 E_{\text{X}}^{\text{HF}} + 0.75 E_{\text{X}}^{\text{LSDA}} + 0.75 \Delta E_{\text{X}}^{\text{non-local}} +  E_{\text{C}}^{\text{LSDA}} + 0.81 \Delta E_{\text{C}}^{\text{non-local}}
$$

## Range-Separated Hybrid DFT Functionals

The exact exchange term can be partitioned (i.e., range-separated) into short-range and long-range parts

$$ E_{\text{X}}^{\text{HF}} = E_{\text{X}}^{\text{HF, LR}} + E_{\text{X}}^{\text{HF, SR}}$$

by defining the Coulomb operator as

$$\frac{1}{r_{12}} = \frac{\texttt{erf}(\omega r_{12})}{r_{12}} + \frac{\texttt{erfc}(\omega r_{12})}{r_{12}} = \frac{\texttt{erf}(\omega r_{12})}{r_{12}} + \frac{[1 - \texttt{erf}(\omega r_{12})]}{r_{12}} $$

Further, in the hybrid-DFT formalism, we have

$$ E_{\text{X}}^{\text{hDFT}} = \alpha E_{\text{X}}^{\text{HF}} + (1 - \alpha) E_{\text{X}}^{\text{DFA}}  $$

For example, in PBE0, we have $\alpha=0.25$ and DFA=PBE.

Combining the ideas of both RS and hDFT, we arrive at the RS-hDFT formalism, where $E_{\text{X}}$ is defined as

$$ E_{\text{X}}^{\text{RS-hDFT}} (\omega) = \alpha E_{\text{X}}^{\text{HF}} + (1 - \alpha) E_{\text{X}}^{\text{DFA}} + \beta E_{\text{X}}^{\text{LR-HF}} (\omega) - \beta E_{\text{X}}^{\text{LR-DFA}} (\omega)  $$

The parameter $\alpha$ defines the fraction of fixed exact exchange contribution.

### Two-parameter range-separated Hybrid DFT Functionals

These RS-hDFT XC functionals do not have a fixed amount of global exact exchange term, so $\alpha=0$.

$$ E_{\text{X}}^{\text{RS-hDFT}} (\omega) =  E_{\text{X}}^{\text{DFA}} + \beta E_{\text{X}}^{\text{LR-HF}} (\omega) - \beta E_{\text{X}}^{\text{LR-DFA}} (\omega)  $$

#### `LC-BLYP`, Gaussian 16 C.01
```
 IExCor=10402 DFT=T Ex=LC-B+HF Corr=LYP ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.470000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.470000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- $\omega$ = wLong = 0.47 
- $\beta$ = cLong = 1.0 

- Orca 6.0.0 uses  $\omega$ 0.33 (see page 458 of manual)
  
#### `LC-wPBE`, Gaussian 16 C.01
```
 IExCor=32609 DFT=T Ex+Corr=LC-wPBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  0.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- $\omega$ = wLong = 0.4 
- $\beta$ = cLong = 1.0

- Can be called using IOp using
```
wPBEhPBE/basisset   IOp(3/76=1000010000) IOp(3/77=0000010000) IOp(3/107=0400000000) IOp(3/108=0400000000) IOp(3/119=1000000000) IOp(3/120=1000000000) IOp(3/130=-1) IOp(3/131=-1)
```

```
 IExCor= 2609 DFT=T Ex=wPBEhh Corr=PBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  0.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```
- ` IOp(3/78=1000010000)` seems to be the default
- wPBEh is exchange part of HSE with screening constant ω = 0.15 according to [this blog](https://cfilomquantum.blogspot.com/2016/06/density-functionals-in-gaussian-09-rev.html)[^3]

- Another possibility of using IOp is
  
```
/basisset IOp(3/74=32609) IOp(3/76=1000010000) IOp(3/77=0000010000) IOp(3/107=0400000000) IOp(3/108=0400000000) IOp(3/119=1000000000) IOp(3/120=1000000000) IOp(3/130=-1) IOp(3/131=-1)
```

```
 IExCor=32609 DFT=T Ex=LC-wPBEhh Corr=PBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  0.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- Yet another possibility of using IOp is
```
/6-31G IOp(3/74=32609) IOp(3/76=1000010000) IOp(3/77=0000010000) IOp(3/107=0400000000) IOp(3/108=0400000000) IOp(3/119=1000000000) IOp(3/120=0000010000) IOp(3/130=-1) IOp(3/131=-1)
```

```
 IExCor=32609 DFT=T Ex=LC-wPBEhh Corr=PBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  0.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  1.000000 cLong=  0.000000
```
- According to the last definition, we have

$$
E_{\text{X}}^{\text{LC-}\omega\text{PBE}} (\omega=0.4) =  E_{\text{X}}^{\text{DFA (Slater)}} + \beta E_{\text{X}}^{\text{LR-HF}} (\omega=0.4) - \beta E_{\text{X}}^{\text{LR-DFA (Slater)}} (\omega = 0.4)
$$

- This is not the same as LC-PBE in Orca 6.0.0

### Three-parameter range-separated Hybrid DFT Functionals

#### `CAM-B3LYP`, Gaussian 16 C.01
```
 IExCor=20419 DFT=T Ex+Corr=CAM-B3LYP ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  0.810000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.330000 cFull=  0.190000 cShort=  0.000000 cLong=  0.460000
 DFx  wShort=  0.000000 wLong=  0.330000 cFull=  0.190000 cShort=  0.000000 cLong=  0.460000
```

- $\omega$ = wLong = 0.33
- $\alpha$ = cFull + cShort = 0.19
- $\beta$ = cLong = 0.46

- Orca 6.0.0 uses the same settings

### Four-parameter range-separated Hybrid DFT Functionals

$$ E_{\text{X}}^{\text{RS-hDFT}} (\omega) = \alpha_1 E_{\text{X}}^{\text{HF}} + \alpha_2 E_{\text{X}}^{\text{DFA}} + \beta_1 E_{\text{X}}^{\text{LR-HF}} (\omega) - \beta_2 E_{\text{X}}^{\text{LR-DFA}} (\omega)  $$

#### `LC-wHPBE`, Gaussian 16 C.01
```
 IExCor=33909 DFT=T Ex+Corr=LC-wHPBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  1.000000 cLong=  0.000000
```

- $\omega$ = wLong = 0.4
- $\alpha_1$ = cFull + cShort (HFx) = 0.0
- $\alpha_2$ = cFull + cShort (DFx) = 1.0 
- $\beta_1$ = cLong (HFx) = 1.0
- $\beta_2$ = cLong (DFx) = 0.0

- Another possibility of using IOp is
  
```
/basiset IOp(3/74=33909) IOp(3/76=1000010000) IOp(3/77=1000010000) IOp(3/78=1000010000) IOp(3/107=0400000000) IOp(3/108=0400000000) IOp(3/119=1000000000) IOp(3/120=0000010000) IOp(3/130=-1) IOp(3/131=-1)
```

```
 IExCor=33909 DFT=T Ex=LC-HSEH+HF Corr=PBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  1.000000 cLong=  0.000000
```
- According to the last definition, we have

$$
E_{\text{X}}^{\text{LC-}\omega\text{HPBE}} (\omega=0.4) =  E_{\text{X}}^{\text{DFA (PBE)}} + E_{\text{X}}^{\text{LR-HF}} (\omega=0.4) -  E_{\text{X}}^{\text{LR-DFA (PBE)}} (\omega = 0.4)
$$

- If we set `HSEH1PBE` it automatically sets $\omega=0.11$ for the short-range part.

- Another sleek way of using IOp (without invoking `LC-` or using a prefix `3` in IOp(3/74) is
```
/basisset IOp(3/74=3909) IOp(3/76=1000010000) IOp(3/107=0400000000) IOp(3/108=0400000000) IOp(3/119=1000000000) IOp(3/120=0000010000) IOp(3/130=-1) IOp(3/131=-1) 
```
which returns
```
 IExCor= 3909 DFT=T Ex=HSEH+HF Corr=PBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  1.000000 cLong=  0.000000
```
  
#### `wB97X-D`, Gaussian 16 C.01
```
 IExCor= 4639 DFT=T Ex+Corr=wB97XD ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=121
 HFx  wShort=  0.000000 wLong=  0.200000 cFull=  0.222036 cShort=  0.000000 cLong=  0.777964
 DFx  wShort=  0.000000 wLong=  0.200000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- $\omega$ = wLong = 0.2
- $\alpha_1$ = cFull + cShort (HFx) = 0.222036
- $\alpha_2$ = cFull + cShort (DFx) = 0.0 
- $\beta_1$ = cLong (HFx) = 0.777964
- $\beta_2$ = cLong (DFx) = 1.0

## B3LYP, LC-BLYP, CAM-B3LYP

### B3LYP
```
#P BLYP/basisset IOp(3/76=1000002000) IOp(3/77=0720008000) IOp(3/78=0810010000) SCF(maxcycles=100)  Int(Grid=ultrafine)
```

```
 IExCor=  402 DFT=T Ex=B+HF Corr=LYP ExCW=0 ScaHFX=  0.200000
 ScaDFX=  0.800000  0.720000  1.000000  0.810000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
```

### LC-BLYP
```
BLYP/basisset IOp(3/76=1000010000) IOp(3/77=1000010000) IOp(3/78=1000010000) IOp(3/107=0470000000,3/108=0470000000) IOp(3/119=1000000000) IOp(3/120=1000000000) IOp(3/130=-1) IOp(3/131=-1)
```
or

```
/basisset IOp(3/74=402) IOp(3/76=1000010000) IOp(3/77=1000010000) IOp(3/78=1000010000) IOp(3/107=0470000000,3/108=0470000000) IOp(3/119=1000000000) IOp(3/120=1000000000) IOp(3/130=-1) IOp(3/131=-1)
```

or the following should also work
```
BLYP/basisset IOp(3/76=1000010000) IOp(3/77=1000010000) IOp(3/107=0470000000,3/108=0470000000) IOp(3/119=1000000000) IOp(3/120=1000000000) IOp(3/130=-1) IOp(3/131=-1)
```

```
 IExCor=  402 DFT=T Ex=B+HF Corr=LYP ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.470000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.470000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

### CAM-B3LYP
```
/basisset IOp(3/74=419) IOp(3/76=1000010000) IOp(3/77=1000010000) IOp(3/78=0810010000) IOp(3/107=0330000000,3/108=0330000000) IOp(3/119=0460000000) IOp(3/120=0460000000) IOp(3/130=01900) IOp(3/131=01900)
```

```
 IExCor=  419 DFT=T Ex=B+HF Corr=V5LYP ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  0.810000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.330000 cFull=  0.190000 cShort=  0.000000 cLong=  0.460000
 DFx  wShort=  0.000000 wLong=  0.330000 cFull=  0.190000 cShort=  0.000000 cLong=  0.460000
```

## References

[^1]: Gaussian 16 C.01 Manual. (Accessed Aug 2024). [https://gaussian.com/dft/](https://gaussian.com/dft/)
[^2]: Guide to Overlay 3 for Gaussian 16. (Accessed Aug 2024). [https://gaussian.com/overlay3/](https://gaussian.com/overlay3/)
[^3]: Blog, Laboratory of Organic Materials of the Institute of Solid State Physics, University of Latvia. (Accessed Aug 2024). [https://cfilomquantum.blogspot.com/2016/06/density-functionals-in-gaussian-09-rev.html](https://cfilomquantum.blogspot.com/2016/06/density-functionals-in-gaussian-09-rev.html).
[^4]: NWChem Manual. (Accessed Aug 2024). [https://nwchemgit.github.io/Density-Functional-Theory-for-Molecules.html](https://nwchemgit.github.io/Density-Functional-Theory-for-Molecules.html)
[^5]: Orca 6.0.0 Manual.

