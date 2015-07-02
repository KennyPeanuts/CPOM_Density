# Calculation of variables for CPOM survey 2013		

## Metadata

### Collected by: 

Kenneth Fortino, Leanna Tacik, Carly Martin

### Collected on: 

Late Winter - Summer 2013

### Affiliation: 

Longwood University

### Location: 

* Daulton Pond (DP, sometimes labeled DL)
* Wilkes Lake (WL)
* Lancer Park Pond (LPP)
* Woodland Court Pond (WC)

All ponds located in Farmville, VA

### Description: 

#### Sample Collection

These data represent the compilation of all of the CPOM density data and sediment LOI data collected during the spring 2013 semester and the 2013 PRISM project.  

During the late winter and spring, the samples were collected from LPP and WL in the littoral zone and open regions of the ponds. A single replicate sample was collected from 3 unique locations in each pond on each date.

During the summer sampling in WL, DP, and WC, 2 replicate samples were taken from each of 3 unique locations in the littoral and open parts of the lake.  The descriptions of the sampling locations are in the field notebook.

The samples were collected from each lake using an Ekman dredge and placed into a plastic bin.  Once on shore, the sediments were homogenized and a sample was collected in a 20 ml glass scintillation vial.  The sediments were then dried at 50 dC.

The remaining sediments were then passed through a 250 um mesh in the field.  The material retained by the mesh was preserved in 70% ethanol and taken back to the lab.  

The preserved material was passed through a 1 mm sieve to separate the coarse and fine fractions.  Any material retained buy the sieve (coarse) was dried at 50 dC and weighed.

#### Lab Processing

The AFDM of the CPOM was determined by grinding the dried leaves in a mortar and pestle and then ashing them in a crucible at 550 dC.  In some cases the entire mass of the CPOM did not fit into a crucible so only a portion of the sample was ashed and the AFDM was determined by multiplying the proportion of organic matter in the sample by the dry mass of the sample.
 
The LOI of the complete sediments was determined by ashing the samples for approximately 4 h at 550 dC.

### File History

* Created: 12 Feb 2014 

* Modified:

    * 15 Feb 2014
    * 5 March 2014 - reanalyzed the data with new WL sediment LOI data from 14 June
    * 30 March 2014 - fixed error in CPOM density Calculations, was multiplying by ekaman area and should have been dividing


### Descriptive Variables:

* lake is the abbreviation of the pond name

    * WL = Wilkes Lake
    * LPP = Lancer Park Pond
    * DP = Daulton Pond
    * WC = Woodland Court Pond

* date is the date that the samples were collected

* julian is the julian day of the sample collection

* sa is the surface area of the lake determined by a best estimate of L X W taken from areal photographs on Google Maps (m^2)

* depth is the water depth at the sampling location

* location is whether the sampling location was considered littoral (< 10 m from shore) or open (near the center of the pond)

* site identifies a unique sampling location in the lake.  A description of the sites are in the field notebook.

* rep is the number of the replicate sample taken at each site


### Measured Variables:

* CPOM.empty is the mass of the paper bag or plastic dish that the CPOM was dried in (g)

* CPOM.full is the mass of the paper bag or plastic dish plus the dried CPOM (g)

* CPOM.cruc.num is the number identifying the crucible for AFDM determination (g)

* CPOM.cruc.empty is the mass of the empty crucible for AFDM determination (g)

* CPOM.cruc.full is the mass of the crucible plus ground leaves (g)

* CPOM.cruc.ash is the mass of the crucible plus the ash, following ashing at 550 dC (g)

* sed.empty is the mass of the empty scintillation vial used for sediment dry mass determination (g)

* sed.full is the mass of the scintillation vial plus dried sediments used for sediment dry mass determination (g)

* sed.cruc.num is the number of the crucible used for the sediment LOI determination (g)

* sed.cruc.empty is the mass of the empty crucible used for the sediment LOI determination (g)

* sed.cruc.full is the mass of the crucible plus the dried, homogenized sediment used for the sediment LOI determination (g)

* sed.cruc.ash is the mass of the crucible plus the ash, following ashing at 550 dC (g)

### Calculated Variables

* CPOM.mass.ekman is the mass of the dried CPOM in the Ekman (g/Ekman) The Ekman = 0.0225 m^2

* CPOM.mass is the dry mass of CPOM per m^2

* CPOM.AFDM.dry is the mass of the dry CPOM in the crucible before ashing for CPOM AFDM determination (g)

* CPOM.AFDM.ash is the mass of the ash remaining in the crucible following ashing for CPOM AFDM determination (g)

* CPOM.propOM is the proportion of organic matter in the CPOM based on LOI

* CPOM.AFDM is the ash free dry mass of the CPOM, determined as the proportion of OM in the CPOM multiplied my the mass of the CPOM / m^2 (g/m^2)

* sed.LOI.dry is the mass of the dry sediments prior to ashing (g)

* sed.LOI.ash is the mass of the ash after ashing the sediments at 550 dC for approx. 4 h (g)

* sed.propOM is the proportion of organic matter in the sediments based on LOI 

## R Code

### Import Raw Data

    survey <- read.delim("./data/CPOM_survey_2013_raw.txt", header = T, stringsAsFactors = F)

### Calculate Variables

    CPOM.mass.ekman <- survey$CPOM.full - survey$CPOM.empty
    CPOM.mass <- CPOM.mass.ekman / 0.0225
    CPOM.AFDM.dry <- survey$CPOM.cruc.full - survey$CPOM.cruc.empty
    CPOM.AFDM.ash <- survey$CPOM.cruc.ash - survey$CPOM.cruc.empty
    CPOM.propOM <- (CPOM.AFDM.dry - CPOM.AFDM.ash) / CPOM.AFDM.dry
    CPOM.AFDM <- CPOM.propOM * CPOM.mass
    sed.LOI.dry <- survey$sed.cruc.full - survey$sed.cruc.empty
    sed.LOI.ash <- survey$sed.cruc.ash - survey$sed.cruc.empty
    sed.propOM <- (sed.LOI.dry - sed.LOI.ash) / sed.LOI.dry

## Data Table Output

### Make Data Frame with Calc. Variables

    survey.calc <- data.frame(survey, CPOM.mass.ekman, CPOM.mass, CPOM.AFDM.dry, CPOM.AFDM.ash, CPOM.propOM, CPOM.AFDM, sed.LOI.dry, sed.LOI.ash, sed.propOM)

### Make Data Table

    write.table(survey.calc, file = "./data/CPOM_survey_2013_calc.txt", quote = F, row.names = F, sep = "\t")

