# def2XC: Definitions of Exchange-Correlation Functionals

Definitions of various exchange-correlation (XC) functionals used in density functional theory (DFT) are collected here.

## Overview

In DFT, the exchange-correlation functional $E_{\text{XC}}$ plays a crucial role in determining the total energy of a system. It is typically divided into exchange $E_{\text{X}}$ and correlation $E_{\text{C}}$ components.

$$ E_{\text{XC}} = E_{\text{X}} + E_{\text{C}} $$

### Exact Exchange

The Hartree--Fock (HF) model completely captures the exchange interaction; hence, the exact exchange implies the exchange term calculated with HF.

$$E_{\text{X}}^{\text{exact}} = E_{\text{X}}^{\text{HF}} = \frac{1}{2} \sum_i \sum_j \int \int \phi_i(r_1)\phi_j(r_1)\frac{1}{r_{12}} \phi_i(r_2)\phi_j(r_2)$$


### Range-Separated Hybrid DFT Functionals

The exact exchange term can be partitioned (i.e., range-separated) into short-range and long-range parts

$$ E_{\text{X}}^{\text{HF}} = E_{\text{X}}^{\text{HF, LR}} + E_{\text{X}}^{\text{HF, SR}}$$

by defining the Coulomb operator as

$$\frac{1}{r_{12}} = \frac{\texttt{erf}(\omega r_{12})}{r_{12}} + \frac{\texttt{erfc}(\omega r_{12})}{r_{12}} = \frac{\texttt{erf}(\omega r_{12})}{r_{12}} + \frac{[1 - \texttt{erf}(\omega r_{12})]}{r_{12}} $$

Further, in the hybrid-DFT formalism, we have

$$ E_{\text{X}}^{\text{hDFT}} = \alpha E_{\text{X}}^{\text{HF}} + (1 - \alpha) E_{\text{X}}^{\text{DFA}}  $$

For example, in PBE0, we have $\alpha=0.25$ and DFA=PBE.

Combining the ideas of both RS and hDFT, we arrive at the RS-hDFT formalism, where $E_{\text{X}}$ is defined as


$$ E_{\text{X}}^{\text{RS-hDFT}} (\omega) = \alpha E_{\text{X}}^{\text{HF}} + (1 - \alpha) E_{\text{X}}^{\text{DFA}} + \beta E_{\text{X}}^{\text{LR-HF}} (\omega) - \beta E_{\text{X}}^{\text{LR-DFA}} (\omega)  $$

# CAM-B3LYP, Gaussian 16 C.01
```
 IExCor=20419 DFT=T Ex+Corr=CAM-B3LYP ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  0.810000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.330000 cFull=  0.190000 cShort=  0.000000 cLong=  0.460000
 DFx  wShort=  0.000000 wLong=  0.330000 cFull=  0.190000 cShort=  0.000000 cLong=  0.460000
```

- $\alpha$ = cFull = 0.19
- $\beta$ = cLong = 0.46
- $\omega$ = wLong = 0.33

# LC-BLYP, Gaussian 16 C.01
```
 IExCor=10402 DFT=T Ex=LC-B+HF Corr=LYP ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.470000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.470000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- $\alpha$ = cFull = 0.0
- $\beta$ = cLong = 1.0
- $\omega$ = wLong = 0.47
  
# LC-wPBE, Gaussian 16 C.01
```
 IExCor=32609 DFT=T Ex+Corr=LC-wPBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  0.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- $\alpha$ = cFull = 0.0
- $\beta$ = cLong = 1.0
- $\omega$ = wLong = 0.4

# LC-wHPBE, Gaussian 16 C.01
```
 IExCor=33909 DFT=T Ex+Corr=LC-wHPBE ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=  4
 HFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
 DFx  wShort=  0.000000 wLong=  0.400000 cFull=  0.000000 cShort=  1.000000 cLong=  0.000000
```

- $\alpha$ = cFull = 0.0 ?
- $\beta$ = cLong = 1.0 ?
- $\omega$ = wLong = 0.4
  
# wB97X-D, Gaussian 16 C.01
```
 IExCor= 4639 DFT=T Ex+Corr=wB97XD ExCW=0 ScaHFX=  1.000000
 ScaDFX=  1.000000  1.000000  1.000000  1.000000 ScalE2=  1.000000  1.000000
 IRadAn=      5 IRanWt=     -1 IRanGd=            0 ICorTp=0 IEmpDi=121
 HFx  wShort=  0.000000 wLong=  0.200000 cFull=  0.222036 cShort=  0.000000 cLong=  0.777964
 DFx  wShort=  0.000000 wLong=  0.200000 cFull=  0.000000 cShort=  0.000000 cLong=  1.000000
```

- $\alpha$ = cFull = 0.22
- $\beta$ = cLong = 0.778
- $\omega$ = wLong = 0.20
