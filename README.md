# SWAT2012rev637
Development of DrainP module based on the SWAT source code version 637.

# How to use the DrainP module

* Swtich on the DrainP module

The DrainP module is switched on when the parameter *ItileP* is set to 1. The parameter was read in at the end of the *basens.bsn* file.
Example of the last section of a basins.bsn fil:

```text
Subdaily Erosion:
           1.000    | EROS_SPL: The splash erosion coefficient ranges 0.9 - 3.1
           0.700    | RILL_MULT: Multiplier to USLE_K for soil susceptible to rill erosion, ranges 0.5 - 2.0
           1.200    | EROS_EXPO: an exponent in the overland flow erosion equation, ranges 1.5 - 3.0
           0.000    | SUBD_CHSED: 1=Brownlie(1981) model, 2=Yang(1973,1984) model
           0.030    | C_FACTOR: Scaling parameter for Cover and management factor in ANSWERS erosion model
            50.0    | CH_D50 : median particle diameter of channel bed [mm]
           1.570    | SIG_G : geometric standard deviation of particle sizes
       50.000000    | RE_BSN: Effective radius of drains
    10313.750000    | SDRAIN_BSN: Distance between two drain or tile tubes
       42.837500    | DRAIN_CO_BSN: Drainage coefficient
           1.042    | PC_BSN: Pump capacity
        3.004750    | LATKSATF_BSN: Multiplication factor to determine lateral ksat from SWAT ksat input value for HRU
               1    | ITDRN: Tile drainage equations flag
               1    | IWTDN: Water table depth algorithms flag
               0    | SOL_P_MODEL: if = 1, use new soil P model
            1.00    | IABSTR: Initial abstraction on impervious cover (mm)
               0    | IATMODEP: 0 = average annual inputs 1 = monthly inputs
               1    | R2ADJ_BSN: basinwide retention parm adjustment factor
               0    | SSTMAXD_BSN: basinwide retention parm adjustment factor
               0    | ISMAX: max depressional storage code
               0    | IROUTUNIT:
               1    | ITileP
```

* Calibrating the DrainP module

Two parameters K_langmuir and beta_Q_max are used to calibrate the soluble reactive phosphorus leaching. In this excutable, I read in these two patameters as two bacteria parameters in the basins.bsn file that are not used in my model, so the values can be calibrated with SWAT-CUP.
Example of the two parameters:


```text
Bacteria:
        0.612100    | K_langmuir WDPQ: Die-off factor for persistent bacteria in soil solution. [1/day]
        0.109470    | betaQ_max WGPQ : Growth factor for persistent bacteria in soil solution [1/day]
        0.100000    | WDLPQ : Die-off factor for less persistent bacteria in soil solution [1/day]
        3.100000    | WGLPQ : Growth factor for less persistent bacteria in soil solution. [1/day] 
```

# Changes in DrainP
* Three files added

We added a file to calculate the P leaching through out the profile (solplch.f90), a file to calculate how much tile drain flow was contributed from each soil layer (tileqsplit.f90), and a file to print soil P budges and soil P contents for the whole soil profile (soilPout.f90).
On top of that, many files were modified to include to new variables. In total 25 files were modified and 3 files were added.

* Additional edit not relevant to DrainP

I had some problems for point source files in our set up. When the starting year of point source file (eg 2000) are later than the simulation starting year (eg 1989), point source are not read properly by the model. I adjusted that in the readyr.f and recyear.f. These changes are not verified, so ignore them if they are not done properly.

# Compillibg errors
If you got compling errors about the new variables in the DrainP module, that look similar to this:

```fortran
error #6410: This name has not been declared as an array or a function.   [SOL_SOLPCON]
```

Then try to check the modparm.f file and see if all the new variables are declared there. Clean and rebuild and hope for the best.
Sometimes it helps to excluded the sub_subbasin.f and included it again, then clean and rebuild the project.
