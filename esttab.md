##Estout Package 

### Install 

```stata
. ssc install estout, replace
```

* provides tools for making regression tables in Stata (www.stata.com)

**Has following sub-packages** 

* **esttab** Command to produce publication-style regression tables that display nicely in Stata's results window or, optionally, are exported to various formats such as CSV, RTF, HTML, or LaTeX. esttab is a user-friendly wrapper for the estout command
* **estout** Generic program to compile a table of coefficients, "significance stars", summary statistics, standard errors, t- or z-statistics, p-values, confidence intervals, or other statistics for one or more models previously fitted and stored. The table is displayed in the results window or written to a text file for use with, e.g., spreadsheets or LaTeX
* **eststo** Utility to store estimation results for later tabulation. eststo is an alternative to official Stata's estimates store. Main advantages of eststo over estimates store are that the user does not have to provide a name for the stored estimation set and that eststo may be used as a prefix command
* **estadd** Program to add extra results to the returns of an estimation command. This is useful to make the the results available for tabulation.
* **estpost** Program to prepare results from commands such as summarize, tabulate, or correlate for tabulation by esttab or estout.

### Eststo - a Wrapper for Estimates store

* storing estimates 
* naming not required, makes up a name 
* you can name however if you want

```stata
. sysuse auto
```

```stata
. eststo clear
```

* erases stored estimates sets post regression
* let's run a regression using the auto data

```stata
. regress price weight mpg
```

```
. eststo
(est1 stored)
```

* the name assigned to the stored estimates is printed out
* let's run another regression 

```stata
. regress price weight mpg foreign
```

```output
. regress price weight mpg

      Source |       SS           df       MS      Number of obs   =        74
-------------+----------------------------------   F(2, 71)        =     14.74
       Model |   186321280         2  93160639.9   Prob > F        =    0.0000
    Residual |   448744116        71  6320339.67   R-squared       =    0.2934
-------------+----------------------------------   Adj R-squared   =    0.2735
       Total |   635065396        73  8699525.97   Root MSE        =      2514

------------------------------------------------------------------------------
       price |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
      weight |   1.746559   .6413538     2.72   0.008      .467736    3.025382
         mpg |  -49.51222   86.15604    -0.57   0.567    -221.3025     122.278
       _cons |   1946.069    3597.05     0.54   0.590    -5226.245    9118.382
------------------------------------------------------------------------------
```

* run eststo to save

```stata
. eststo
(est2 stored)
```

* stata tells you the name 
* let's print out the table using estout


```
. estout, style(fixed)

                     est1         est2
                        b            b
weight           1.746559     3.464706
mpg             -49.51222      21.8536
foreign                        3673.06
_cons            1946.069    -5853.696
```

```stata
. eststo clear
```

* will wipe out the stored estimates

### Use eststo as a prefix command

* we'll prefix eststo to our regress

```stata
. eststo: regress price weight mpg

      Source |       SS           df       MS      Number of obs   =        74
-------------+----------------------------------   F(2, 71)        =     14.74
       Model |   186321280         2  93160639.9   Prob > F        =    0.0000
    Residual |   448744116        71  6320339.67   R-squared       =    0.2934
-------------+----------------------------------   Adj R-squared   =    0.2735
       Total |   635065396        73  8699525.97   Root MSE        =      2514

------------------------------------------------------------------------------
       price |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
      weight |   1.746559   .6413538     2.72   0.008      .467736    3.025382
         mpg |  -49.51222   86.15604    -0.57   0.567    -221.3025     122.278
       _cons |   1946.069    3597.05     0.54   0.590    -5226.245    9118.382
------------------------------------------------------------------------------
(est1 stored)
```

* this prints out the assigned est name after the output

```stata
. eststo: regress price weight mpg foreign

      Source |       SS           df       MS      Number of obs   =        74
-------------+----------------------------------   F(3, 70)        =     23.29
       Model |   317252881         3   105750960   Prob > F        =    0.0000
    Residual |   317812515        70  4540178.78   R-squared       =    0.4996
-------------+----------------------------------   Adj R-squared   =    0.4781
       Total |   635065396        73  8699525.97   Root MSE        =    2130.8

------------------------------------------------------------------------------
       price |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
      weight |   3.464706    .630749     5.49   0.000     2.206717    4.722695
         mpg |    21.8536   74.22114     0.29   0.769    -126.1758     169.883
     foreign |    3673.06   683.9783     5.37   0.000     2308.909    5037.212
       _cons |  -5853.696   3376.987    -1.73   0.087    -12588.88    881.4934
------------------------------------------------------------------------------
(est2 stored)
```

* again prints out the assigned name
* let's use estout again to print the table (try witout fixed)

```stata
. estout, style(fixed)
```

## ESTTAB 

1. Use eststo to store multiple estimates
2. Apply esttab to compse a regression table

* diff b/t esttab and estout, esttab produces a fully formatted table

```
. eststo clear
. sysuse auto
. eststo: regress price weight mpg
. eststo: regress price weight mpg foreign
. esttab
```

### Change table content

* default esttab is to display raw point estimates along with t-statistics 
* print the number of obs in the table footer
* all can be changed 
* lets' replace the t statistic with standard errors, add the adjusted R-squared and omite significance asterisks

```
. esttab, se ar2 nostar
```

The t statistics can be replaced with: 
* p-values (p)
* confidence intervals (ci)
* or any parameter statistic contained in the estimates (see aux())
* pr2 for pseudo-R-squared and bic option to include anython other scal stats contained in teh stored estimates
* for instance scalars(F df_m df_r)

* also the point estimates may be replaced with other statistics 
* eg. beta coefficients are printed and the t-stats are suppressed:

```
. esttab, beta not
```

* more options are providec by the `main()` or thru `estout`

NOTE: back to slides


### Layout, label, titles

* `wide` option arrranges point est. with any other stored parameter
* `plain` option produces a minimally formatted table with all display formats set to Stata's `%9.0g`
* `compress` reduces horizontal spacing to fit more models on the screen
* `label` option causes variable labels to be used 
* `mtitles` specifies that model titles be printed in the header 
* let's use some of these options

```
. esttab, wide
```

* let's create a table with `se ar2 nostar brackets label`, add a title, model title and add a note

```
. esttab, se ar2 nostar brackets label title(This is a
>  regression table) nonumbers mtitles("Model A" "Mode
> l B") addnote("Source: auto.dta")

This is a regression table
----------------------------------------------
                          Model A      Model B
----------------------------------------------
Weight (lbs.)               1.747        3.465
                          [0.641]      [0.631]

Mileage (mpg)              -49.51        21.85
                          [86.16]      [74.22]

Car type                                3673.1
                                       [684.0]

Constant                   1946.1      -5853.7
                         [3597.0]     [3377.0]
----------------------------------------------
Observations                   74           74
Adjusted R-squared          0.273        0.478
----------------------------------------------
Standard errors in brackets
Source: auto.dta
```


NOTE: back to slides 

### Numerical formats 

* Let's display p-values and the R-squared with 4 decimal places and point estimates with the `%9.0g` format

```
. esttab, b(%9.0g) p(4) r2(4) nostar wide
```

* Available formats are Stata's display formats, such as `%9.0g` or `%8.2f` (see [d] Format)
* You can request a fixed format by specifying an integer indicating the desired number of decimal points 

NOTE: back to slides

### Output formats

* spreadsheet 
```
. esttab using example.csv
(output written to example.csv)
```

* as word: 
```
. esttab using example.rtf
(output written to example.rtf)
```


### Digging deeper into estout

* esttab is a wrapper, to get all the commands that are wrapped around estout run: 

```
. esttab, noisily
estout ,
 cells(b(fmt(a3) star) t(fmt(2) par("{ralign @modelwid
> th:{txt:(}" "{txt:)}}")))
 stats(N, fmt(%18.0g) labels(`"N"'))
 starlevels(* 0.05 ** 0.01 *** 0.001)
 varwidth(12)
 modelwidth(12)
 abbrev
 delimiter(" ")
 smcltags
 prehead(`"{hline @width}"')
 posthead("{hline @width}")
 prefoot("{hline @width}")
 postfoot(`"{hline @width}"' `"t statistics in parenth
> eses"' `"@starlegend"')
 varlabels(, end("" "") nolast)
 mlabels(, depvar)
 numbers
 collabels(none)
 eqlabels(, begin("{hline @width}" "") nofirst)
 interaction(" # ")
 level(95)
 style(esttab)
```

## estpost 

* estpost command [ arguments ] [, options ]

```stata
. sysuse auto
. estpost summarize price weight rep78 mpg
. esttab, cells("count mean sd min max") noobs
```


* more examples 
* summarize 

```stata
sysuse auto
. estpost summarize price mpg rep78 foreign, listwise
. esttab, cells("mean sd min max") nomtitle nonumber
```

* tabstat 
* use columns(variables) and columns(statistics) to determine whether to display variables or statistics in the columns (default is columns(variables))

```stata
.sysuse auto
.estpost tabstat price mpg rep78, listwise statistics(mean sd)
.esttab, cells("price mpg rep78") nomtitle nonumber
```

Now type columns(statistics) to print statistics in columns: 

```stata
. sysuse auto
. estpost tabstat price mpg rep78, listwise statistics(mean sd) columns(statistics)
. esttab, cells("mean sd") nomtitle nonumber
```

### subgroups (summarize)

```
. sysuse auto
. by foreign: eststo: quietly estpost summarize prices mpg rep78, listwise
. esttab, cells("mean sd") label nodepvar
```

### tabstat subgroups

```stata
. sysuse auto
. estpost tabstat price mpg rep78, by(foreign) statisics(mean sd) columns(statistics) listwise
. esttab, main(mean) aux(sd) nostar unstack noobs nonot nomtitle nonumber
```