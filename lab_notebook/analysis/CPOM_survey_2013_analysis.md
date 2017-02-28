# Analysis of the CPOM Survey Data

## Metadata

* file creation date unknown

### Modified

* 19 Sept 2016 - KF - created publication figure of Density by pond
* 27 Sept 2016 - KF - created publication figure of LOI by pond and LOI by CPOM Density

## Import Data

### Working Directory

### Import

    survey <- read.delim("./data/CPOM_survey_2013_calc.csv", header = T, stringsAsFactors = F, sep = ",")

## Data Analysis

### Research Questions

1) What is the density of CPOM in the study ponds?

2) How does the density of CPOM vary among ponds?

3) How does the density of CPOM vary within ponds?
     a. Littoral vs Open Habitats
     b. Within Littoral and Open Habitats

4) Does the density of CPOM vary with date (WL only)?

5) Is there a relationship between CPOM and percent OM in the sediments?

### Analysis

Dates of samples

    with(survey, tapply(date, lake, unique))

~~~~

$DP
[1] "5/13/13"

$LPP
[1] "3/20/13"

$WC
[1] "5/14/13"

$WL
[1] "2/20/13" "2/27/13" "6/14/13"

~~~~

    with(survey, tapply(depth, location, summary))

#### Density of CPOM in all samples

    summary(survey$CPOM.AFDM)

~~~~
  
    Min.  1st Qu.   Median     Mean  3rd Qu.     Max.     NAs 
   3.436   10.790   40.210  162.300  182.600 1179.000       12 

~~~~

    par(las = 1)
    plot(jitter(rep(1, length(survey$CPOM.AFDM)), 3) ,survey$CPOM.AFDM, xlim = c(0.5, 1.5), axes = F, ylab = "CPOM Density (g AFDM/m^2)", xlab = " ")
    axis(2)
    abline(h = 0)
    points(1, median(survey$CPOM.AFDM, na.rm = T), pch = 8, col = 2)
    dev.copy(jpeg, "./ouput/plots/CPOM_Dens_Scatterplot.jpg")
    dev.off()

![scatter plot of CPOM AFDM](../output/plots/CPOM_Dens_Scatterplot.jpg)

    par(las = 1)
    boxplot(survey$CPOM.AFDM, ylab = "CPOM Density (g AFDM/m^2)", col = 8)
    dev.copy(png, "./output/plots/CPOM_Dens_Boxplot.png")
    dev.off()

![Boxplot of the density of CPOM in all of the survey lakes during the 2013 survey](../output/plots/CPOM_Dens_Boxplot.png)

    par(las = 1)
    hist(survey$CPOM.AFDM, breaks = 10, xlab = "CPOM Density (g AFDM/m^2)", main = " ", col = 8)
    dev.copy(png, "./output/plots/CPOM_Dens_Hist.png")
    dev.off()

![Frequency histogram of the density of CPOM in all of the survey lakes during the 2013 survey](../output/plots/CPOM_Dens_Hist.png)

The data show that for all of the samples the density of CPOM is mainly under 100 g AFDM / m^2 however there are local patches of higher CPOM with densities between 400 and 1200 g AFDM / m^2.

This is not due to differences in then location of the samples since there are 30 open and 24 littoral samples.

There is potentially a bias introduced by the lakes because most of the samples come from WL and the least from LPP

~~~~

 tapply(survey$CPOM.AFDM, survey$lake, length)
 DP LPP  WC  WL 
 12   6  12  24 
>

~~~~

#### Density of CPOM among lakes

    tapply(survey$CPOM.AFDM, survey$lake, summary)
    tapply(survey$CPOM.AFDM, survey$lake, sd, na.rm = T)

~~~~

$DP
    Min.  1st Qu.   Median     Mean  3rd Qu.     Max. SD
   5.937   16.420   37.450  175.400   59.300 1067.000 343.5094

$LPP
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. SD
  55.81  112.50  220.60  398.70  534.70 1179.00 435.8988

$WC
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD           NAs 
  3.436   8.144  10.740  36.000  40.210 195.800  55.37966     1 

$WL
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. SD            NAs 
  5.981  10.930  24.740 147.800 234.000 526.600 194.05770     11 

~~~~

##### Plot of CPOM Density by lake

    par(las = 1, mar = c(5, 6, 3, 3))
    plot(jitter(rep(1, length(survey$CPOM.AFDM[survey$lake == "DP" & survey$location == "littoral"])), 5), survey$CPOM.AFDM[survey$lake == "DP" & survey$location == "littoral"]/1000, pch = 19, cex = 1.5, cex.lab = 1.2, xlim = c(0.5, 4.5), ylim = c(0, 1.2), axes = F, xlab = " ", ylab = expression(paste("Leaf Litter Density (kg AFDM m"^{-2}, ")")))
    points(jitter(rep(1, length(survey$CPOM.AFDM[survey$lake == "DP" & survey$location == "open"])), 5), survey$CPOM.AFDM[survey$lake == "DP" & survey$location == "open"]/1000, cex = 1.5)
    points(jitter(rep(2, length(survey$CPOM.AFDM[survey$lake == "LPP" & survey$location == "littoral"])), 5), survey$CPOM.AFDM[survey$lake == "LPP" & survey$location == "littoral"]/1000, pch = 19, cex = 1.5)
    points(jitter(rep(2, length(survey$CPOM.AFDM[survey$lake == "LPP" & survey$location == "open"])), 5), survey$CPOM.AFDM[survey$lake == "LPP" & survey$location == "open"]/1000, cex = 1.5)
    points(jitter(rep(3, length(survey$CPOM.AFDM[survey$lake == "WC" & survey$location == "littoral"])), 2), survey$CPOM.AFDM[survey$lake == "WC" & survey$location == "littoral"]/1000, pch = 19, cex =1.5)
    points(jitter(rep(3, length(survey$CPOM.AFDM[survey$lake == "WC" & survey$location == "open"])), 2), survey$CPOM.AFDM[survey$lake == "WC" & survey$location == "open"]/1000, cex = 1.5)
       points(jitter(rep(4, length(survey$CPOM.AFDM[survey$lake == "WL" & survey$location == "littoral"])), 2), survey$CPOM.AFDM[survey$lake == "WL" & survey$location == "littoral"]/1000, pch = 19, cex = 1.5)
    points(jitter(rep(4, length(survey$CPOM.AFDM[survey$lake == "WL" & survey$location == "open"])), 2), survey$CPOM.AFDM[survey$lake == "WL" & survey$location == "open"]/1000, cex = 1.5)
    axis(1, at = c(1, 2, 3, 4), labels = c("Daulton", "Lancer Pk.", "Woodland", "Wilck's"), tick = T, cex.axis = 0.8)
    axis(2, cex.axis = 1.2)
    box(lwd = 1)
    legend(3, 1.2, c("Littoral  ", "Open  "), pch = c(19, 1))
    dev.copy(jpeg, "./output/plots/CPOM_Dens_by_lake_scatter.jpg")
    dev.off()

![CPOM Density scatterplot by lake](../output/plots/CPOM_Dens_by_lake_scatter.jpg)



    par(las = 1)
    plot(CPOM.AFDM ~ as.factor(lake), data = survey, xlab = "Pond", ylab = "CPOM Density (g AFDM / m^2)", col = 8)
    dev.copy(png, "./output/plots/CPOM_by_pond.png")
    dev.off()

    par(las = 1)
    plot(log(CPOM.AFDM) ~ as.factor(lake), data = survey, xlab = "Pond", ylab = "ln CPOM Density (g AFDM / m^2)", col = 8)
    dev.copy(png, "./output/plots/lnCPOM_by_pond.png")
    dev.off()

![CPOM AFDM by pond for the 2013 survey](../output/plots/CPOM_by_pond.png)

CPOM AFDM by pond for the 2013 survey

![Natural Log transformed CPOM AFDM by pond for the 2013 survey](../output/plots/lnCPOM_by_pond.png)

Natural Log transformed CPOM AFDM by pond for the 2013 survey

    par(las = 1, mfcol = c(4, 1), mar = c(5, 12, 4, 12))
    hist(survey$CPOM.AFDM[survey$lake == "DP"], breaks = 5,  xlim = c(0, 1200), col = 8, main = "DP", xlab = " ")
    hist(survey$CPOM.AFDM[survey$lake == "LPP"], breaks = 5, xlim = c(0, 1200), col = 8, main = "LPP", xlab = " ")
    hist(survey$CPOM.AFDM[survey$lake == "WC"], breaks = 5,  xlim = c(0, 1200), col = 8, main = "WC", xlab = " ")
    hist(survey$CPOM.AFDM[survey$lake == "WL"], breaks = 5,  xlim = c(0, 1200), col = 8, main = "WL", xlab = "CPOM Density (g AFDM/m^2)")
    dev.copy(png, "./output/plots/CPOM_by_pond_Hist.png")
    dev.off()

![Frequency histograms of CPOM (g AFDM / m^2) for each pond in the 2013 survey](../output/plots/CPOM_by_pond_Hist.png)

Frequency histograms of CPOM (g AFDM / m^2) for each pond in the 2013 survey

##### ANOVA of transformed CPOM ~ Pond

    pond.cpom.aov <- aov(log(CPOM.AFDM) ~ as.factor(lake), data = survey)
    summary(pond.cpom.aov)
    TukeyHSD(pond.cpom.aov)

###### Output

~~~~
                Df Sum Sq Mean Sq F value Pr(>F)  
as.factor(lake)  3  25.88   8.626   3.955  0.015 *
Residuals       38  82.88   2.181                 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
12 observations deleted due to missingness
>     TukeyHSD(pond.cpom.aov)
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = log(CPOM.AFDM) ~ as.factor(lake), data = survey)

$`as.factor(lake)`
             diff        lwr        upr     p adj
LPP-DP  1.6832945 -0.3003995  3.6669885 0.1208622
WC-DP  -0.8932908 -2.5493720  0.7627904 0.4775580
WL-DP   0.1165091 -1.4717172  1.7047354 0.9972406
WC-LPP -2.5765853 -4.5901110 -0.5630597 0.0075183
WL-LPP -1.5667854 -3.5248823  0.3913115 0.1561122
WL-WC   1.0097999 -0.6155328  2.6351326 0.3537328

~~~~

The ANOVA with ln transformation shows a significant model and Tukeys HSD shows that LPP has more CPOM than WC, otherwise the CPOM density in the ponds is not significantly different.

#### Density of CPOM within a lake

#### Comparison of the open and littoral habitats.

    tapply(survey$CPOM.AFDM, survey$location, summary)
    tapply(survey$CPOM.AFDM, survey$location, sd, na.rm = T)
    

~~~~

$littoral
    Min.  1st Qu.   Median     Mean  3rd Qu.     Max. SD          NAs  
   9.712   44.070  112.600  283.100  449.200 1179.000 346.92397   2 

$open
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.   SD         NAs 
  3.436   7.363  10.830  29.310  26.570 214.900   47.92569   10 

~~~~

    par(las = 1)
    plot(CPOM.AFDM ~ as.factor(location), data = survey, col = 8, xlab = "location in pond", ylab = "CPOM Density (g AFDM/m^2)")
    dev.copy(png, "./output/plots/CPOM_by_location.png")
    dev.off()

![CPOM Density (g AFDM/m^2) by location in the pond in the 2013 survey](../output/plots/CPOM_by_location.png)

CPOM Density (g AFDM/m^2) by location in the pond in the 2013 survey

    plot(log(CPOM.AFDM) ~ as.factor(location), data = survey, col = 8, xlab = "location in pond", ylab = "ln CPOM Density (g AFDM/m^2)")
    dev.copy(png, "./output/plots/lnCPOM_by_location.png")
    dev.off()

![CPOM Density (g AFDM/m^2) by location in the pond in the 2013 survey](../output/plots/lnCPOM_by_location.png)

ln CPOM Density (g AFDM/m^2) by location in the pond in the 2013 survey

    par(las = 1, mfcol = c(2, 1), mar = c(5, 10, 5, 10))
    hist(survey$CPOM.AFDM[survey$location == "littoral"], xlim = c(0, 1200), col = 8, main = "Littoral", xlab = " ")
    hist(survey$CPOM.AFDM[survey$location == "open"], xlim = c(0, 1200), col = 8, main = "Open", xlab = "CPOM Density (g AFDM/m^2)")
    dev.copy(png, "./output/plots/CPOM_by_location_hist.png")
    dev.off()

![Frequency histogram of CPOM Density (g AFDM/m^2) by location in the pond in the 2013 survey](../output/plots/CPOM_by_location_hist.png)


Frequency histogram of CPOM Density (g AFDM/m^2) by location in the pond in the 2013 survey

##### ANOVA of transformed CPOM ~ Location

    location.cpom.aov <- lm(log(CPOM.AFDM) ~ as.factor(location), data = survey)
    anova(location.cpom.aov)

###### Output

~~~~

Analysis of Variance Table

Response: log(CPOM.AFDM)
                    Df Sum Sq Mean Sq F value    Pr(>F)    
as.factor(location)  1 44.781  44.781      28 4.658e-06 ***
Residuals           40 63.973   1.599                      

~~~~

The results show that there is significantly more CPOM in the littoral locations than in the open locations. 


#### Relationship between CPOM and sediment %OM

##### Variation in %OM 

    summary(survey$sed.propOM)
    sd(survey$sed.propOM, na.rm = T)

~~~~

 Min.      1st Qu.   Median    Mean      3rd Qu.    Max.  SD      NAs 
 0.007268  0.080530  0.106700  0.103200  0.127700  0.2230 0.05475 13

~~~~

   par(las = 1)
   boxplot(survey$sed.propOM * 100, col = 8, ylab = "Percent Organic Matter (LOI 550)")
   dev.copy(png, "./output/plots/percOM_boxplot.png")
   dev.off()

![Percent sediment organic matter (LOI 550) in 2013 survey ponds](../output/plots/percOM_boxplot.png)

Percent sediment organic matter (LOI 550) in 2013 survey ponds

   par(las = 1)
   hist(survey$sed.propOM * 100, col = 8, breaks = 10)
   dev.copy(png, "./output/plots/percOM_hist.png")
   dev.off()

![Frequency histogram percent sediment organic matter (LOI 550) in 2013 survey ponds](../output/plots/percOM_hist.png)

Frequency histogram percent sediment organic matter (LOI 550) in 2013 survey ponds

Overall the sediment organic matter ranges from between 8 and 13% but there is a cluster of samples with <5% organic matter and a cluster of samples with >20% organic matter.  It is likely that the low percent organic matter samples, are those that were very sandy.

##### Variation in percent OM by lake

    tapply(survey$sed.propOM * 100, survey$lake, summary)
    tapply(survey$sed.propOM * 100, survey$lake, sd, na.rm = T)

~~~~

$DP
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.   SD           NAs 
  2.363  10.800  20.710  15.830  21.230  22.300   8.597525     6.000 

$LPP
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
  11.12   11.58   12.38   12.72   13.50   15.27  1.567903

$WC
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
  10.08   10.74   11.60   12.26   13.05   17.73  2.115659

$WL
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD        NAs 
 0.7268  2.4520  8.0530  6.1660  8.8560 10.1700  3.645933  7.0000 


~~~~

###### Test of Sediment OM in lakes
 
    lake.loi.aov <- aov(sed.propOM ~ lake, data = survey)
    summary(lake.loi.aov)
    TukeyHSD(lake.loi.aov)

~~~~
ANOVA of LOI by lake 
 
            Df Sum Sq Mean Sq F value  Pr(>F)   
lake         3  9.633   3.211   6.664 0.00104 **
Residuals   37 17.829   0.482     

$lake
              diff        lwr        upr     p adj
LPP-DP  0.01917956 -1.0588048  1.0971640 0.9999599
WC-DP  -0.02361644 -0.9571783  0.9099454 0.9998848
WL-DP  -0.99049705 -1.8771159 -0.1038782 0.0235015
WC-LPP -0.04279601 -0.9763579  0.8907659 0.9993174
WL-LPP -1.00967661 -1.8962954 -0.1230578 0.0203197
WL-WC  -0.96688060 -1.6708553 -0.2629060 0.0037933

~~~~

    par(las = 1)
    plot((sed.propOM * 100) ~ as.factor(lake), data = survey, col = 8, xlab = "Pond", ylab = "Percent Sediment Organic Matter (LOI 550)")
    dev.copy(png, "./output/plots/percOM_by_lake.png")
    dev.off()

![Percent sediement organic matter (LOI 550) in the different ponds in 2013 survey](../output/plots/percOM_by_lake.png)

    par(las = 1)
    plot((sed.propOM * 100) ~ jitter(as.numeric(as.factor(lake)), 1), data = survey, ylim = c(0, 25), col = 1, xlab = "Pond", ylab = "Percent Sediment Organic Matter (LOI 550)", axes = F)
    axis(2)
    axis(1, c("DP", "LPP", "WC", "WL"), at = c(1, 2, 3, 4))
    box()
    dev.copy(png, "./output/plots/percOM_by_lake_pts.png")
    dev.off()

![Percent sediement organic matter (LOI 550) in the different ponds in 2013 survey](../output/plots/percOM_by_lake_pts.png)
Percent sediement organic matter (LOI 550) in the different ponds in 2013 survey

    par(las = 1, mfcol = c(4, 1), mar = c(5, 10, 5, 10))
    hist(survey$sed.propOM[survey$lake == "DP"] * 100, breaks = 10, col = 8, xlim = c(0, 25), xlab = " ", main = "DP")
    hist(survey$sed.propOM[survey$lake == "LPP"] * 100, breaks = 10, col = 8, xlim = c(0, 25), xlab = " ", main = "LPP")
    hist(survey$sed.propOM[survey$lake == "WC"] * 100, breaks = 10, col = 8, xlim = c(0, 25), xlab = " ", main = "WC")
    hist(survey$sed.propOM[survey$lake == "WL"] * 100, breaks = 10, col = 8, xlim = c(0, 25), xlab = "Percent Sediment Organic Matter (LOI 550)", main = "WL")
    dev.copy(png, "./output/plots/percOM_by_lake_hist.png")
    dev.off()

![Frequency histogram of percent sediement organic matter (LOI 550) in the different ponds in 2013 survey](../output/plots/percOM_by_lake_hist.png)

Frequency histogram of percent sediement organic matter (LOI 550) in the different ponds in 2013 survey

There are differences in the way that sediment OM is distributed among the lakes.  The two catch-basin lakes (LPP and WC) have the most homogeneous sediment organic matter with virtually all samples falling between 10 and 15% sediment OM.  WL has the lowest sediment OM with virtually all samples falling below 10%.  DP shows a bimodal distribution with a 2 samples under 10% and 4 samples over 20%, so overall DP has the greatest sediment OM.

##### Variation in Perc. OM by location

    tapply(survey$sed.propOM * 100, survey$location, summary)

~~~~

$littoral
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NAs 
 0.7268  3.3920 10.6700  9.3270 12.8600 17.7300  7.0000 

$open
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NAs 
  1.536   8.188  10.390  11.030  11.900  22.300   6.000 

~~~~

###### Test of Sediment OM in lakes
 
    loc.loi.aov <- aov(sed.propOM ~ location, data = survey)
    summary(loc.loi.aov)

~~~~
# ANOVA of the LOI by location
 
> summary(loc.loi.aov)
            Df  Sum Sq  Mean Sq F value Pr(>F)
location     1 0.00289 0.002889   0.963  0.333
Residuals   39 0.11704 0.003001               
13 observations deleted due to missingness

~~~~
    
    par(las = 1)
    plot(sed.propOM * 100 ~ as.factor(location), data = survey, col = 8)
    dev.copy(png, "./output/plots/percOM_by_location.png")
    dev.off()

![Percent sediement organic matter (LOI 550) in open and littoral regions in 2013 survey](../output/plots/percOM_by_location.png)

Percent sediement organic matter (LOI 550) in open and littoral regions in 2013 survey

    par(las = 1, mfcol = c(2, 1), mar = c(5, 10, 5, 10))
    hist(survey$sed.propOM[survey$location == "littoral"] * 100, breaks = 10, xlim = c(0, 25), col = 8, main = "Littoral", xlab = " ")
    hist(survey$sed.propOM[survey$location == "open"] * 100, breaks = 10,  xlim = c(0, 25), col = 8, main = "Open", xlab = "Sediment Percent Organic Matter (LOI 550)")
    dev.copy(png, "./output/plots/percOM_by_location_hist.png")
    dev.off()

![Frequency histogram of percent sediement organic matter (LOI 550) in open and littoral regions in 2013 survey](../output/plots/percOM_by_location <- hist.png)

Frequency histogram of percent sediement organic matter (LOI 550) in open and littoral regions in 2013 survey

Overall there is not a big difference between the littoral and open locations the cluster of high sediment OM in DP is from the open habitat. There were samples with low percent OM from both the littoral and open habitats.

##### Relationship between CPOM and sediment percent OM

    par(las = 1)
    plot((sed.propOM * 100) ~ CPOM.AFDM, data = survey, subset = location == "littoral",  xlim = c(0, 1200), ylim = c(0, 25), pch = 16, ylab = "Percent Sediment Organic Matter (LOI 550)", xlab = "CPOM Density (g AFDM/m^2)")
    points((sed.propOM * 100) ~ CPOM.AFDM, data = survey, subset = location == "open")
    legend(0.6, 25, c("littoral", "open"), pch = c(16, 1))
    dev.copy(png, "./output/plots/percOM_by_CPOM.png")
    dev.off()

![Percent sediement organic matter (LOI 550) plotted against CPOM (g AFDM/m^2) in open and littoral regions in 2013 survey](../output/plots/percOM_by_CPOM.png)

_Percent sediement organic matter (LOI 550) plotted against CPOM (g AFDM/m^2) in open and littoral regions in 2013 survey_

    par(las = 1, mar = c(5, 6, 3, 3))
    plot((sed.propOM * 100) ~ log(CPOM.AFDM), data = survey, subset = location == "littoral", xlim = c(0, 7),   ylim = c(0, 25), pch = 19, ylab = "Percent Sediment Organic Matter (LOI 550)", xlab = expression(paste("ln Leaf Litter Density (g AFDM m"^{-2}, ")")), cex = 1.5, cex.axis = 1.2, cex.lab = 1.2)
    points((sed.propOM * 100) ~ log(CPOM.AFDM), data = survey, subset = location == "open", cex = 1.5)
    legend(5, 25, c("littoral", "open"), pch = c(19, 1))
    dev.copy(jpeg, "./output/plots/percOM_by_lnCPOM.jpg")
    dev.off()

![Percent sediement organic matter (LOI 550) plotted against CPOM (g AFDM/m^2) in open and littoral regions in 2013 survey](../output/plots/percOM_by_lnCPOM.jpg)

_Percent sediement organic matter (LOI 550) plotted against ln transformed CPOM (ln g AFDM/m^2) in open and littoral regions in 2013 survey_


There is no relationship between CPOM Density and sediment organic matter content.

    summary(lm(sed.propOM ~ CPOM.AFDM, data = survey)) 

~~~~
Regression of Sediment percent OM and Leaf Litter Density

Call:
lm(formula = sed.propOM ~ CPOM.AFDM, data = survey)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.098768 -0.018620 -0.001738  0.019615  0.109888 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.134e-01  1.148e-02   9.881 6.05e-11 ***
CPOM.AFDM   -1.514e-05  4.089e-05  -0.370    0.714    

Residual standard error: 0.05686 on 30 degrees of freedom
  (22 observations deleted due to missingness)
Multiple R-squared:  0.004549, Adjusted R-squared:  -0.02863 
F-statistic: 0.1371 on 1 and 30 DF,  p-value: 0.7138

~~~~

## Plot for Manuscript

    par(las = 1, mar = c(2, 8, 2, 8), mfcol = c(2, 1))
    #plot of LOI by lake
    plot(sed.propOM * 100 ~ jitter(as.numeric(as.factor(lake)), 0.5), data = survey, subset = location == "littoral", pch = 19, axes = F, ylab = "Sediment Organic Matter (%)", xlab = " ", ylim = c(0, 25), xlim = c(0.5, 4.5), cex.lab = 0.8)
    points(sed.propOM * 100 ~ jitter(as.numeric(as.factor(lake)), 0.5), data = survey, subset = location == "open", pch = 1)
    axis(2)
    axis(1, c("Daulton", "Lancer Pk.", "Woodland", "Wilcke's"), at = c(1, 2, 3, 4), cex.axis = 0.65)
    box()
    legend(3.5, 25, c("Littoral", "Open"), pch = c(19, 1), cex = 0.8)
    text(0.75, 24, "A", cex = 1)
    #plot of LOI by CPOM Density
    # make x variable
    CPOM.Dens.kg <- survey$CPOM.AFDM/1000
    par(mar = c(4, 8, 0.6, 8))
    plot(sed.propOM * 100 ~ CPOM.Dens.kg, data = survey, subset = location == "littoral", pch = 19, ylab = "Sediment Organic Matter (%)", xlab = expression(paste("Leaf Litter Density (kg AFDM m"^{-2}, ")")), ylim = c(0, 25), xlim = c(0, 1.2), cex.lab = 0.8)
    points(sed.propOM * 100 ~ CPOM.Dens.kg, data = survey, subset = location == "open", pch = 1)
    text(0.1, 24, "B", cex = 1)
    dev.copy(jpeg, "./output/plots/LOI_survey.jpg")
    dev.off()

![LOI Survey](../output/plots/LOI_survey.jpg)
