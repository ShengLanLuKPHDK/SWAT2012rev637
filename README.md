# SWAT2012rev637
Development of DrainP module based on the SWAT source code version 637.

#Changes in DrainP
**Three files added

We added a file to calculate the P leaching through out the profile (solplch.f90), a file to calculate how much tile drain flow was contributed from each soil layer (tileqsplit.f90), and a file to print soil P budges and soil P contents for the whole soil profile (soilPout.f90).
On top of that, many files were modified to include to new variables. In total 25 files were modified and 3 files were added.

**Additional edit not relevant to DrainP

I had some problems for point source files in our set up. When the starting year of point source file (eg 2000) are later than the simulation starting year (eg 1989), point source are not read properly by the model. I adjusted that in the readyr.f and recyear.f. These changes are not verified, so ignore them if they are not done properly.
