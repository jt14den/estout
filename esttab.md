##Estout Package 

### Install 

* provides tools for making regression tables in Stata (www.stata.com)

**Has following sub-packages** 

* **esttab** Command to produce publication-style regression tables that display nicely in Stata's results window or, optionally, are exported to various formats such as CSV, RTF, HTML, or LaTeX. esttab is a user-friendly wrapper for the estout command
* **estout** Generic program to compile a table of coefficients, "significance stars", summary statistics, standard errors, t- or z-statistics, p-values, confidence intervals, or other statistics for one or more models previously fitted and stored. The table is displayed in the results window or written to a text file for use with, e.g., spreadsheets or LaTeX
* **eststo** Utility to store estimation results for later tabulation. eststo is an alternative to official Stata's estimates store. Main advantages of eststo over estimates store are that the user does not have to provide a name for the stored estimation set and that eststo may be used as a prefix command
* **estadd** Program to add extra results to the returns of an estimation command. This is useful to make the the results available for tabulation.
* **estpost** Program to prepare results from commands such as summarize, tabulate, or correlate for tabulation by esttab or estout.

### Eststo or Estimates store

* storing estimates 
* naming not required, makes up a name 
* you can name however if you want
* 

```stata
. eststo clear
```
* erases stored estimates sets 

```stata
. sysuse auto
(1978 Automobile Data)
```

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


```
. esttab, se ar2 nostar

--------------------------------------
                      (1)          (2)
                    price        price
--------------------------------------
weight              1.747        3.465
                  (0.641)      (0.631)

mpg                -49.51        21.85
                  (86.16)      (74.22)

foreign                         3673.1
                               (684.0)

_cons              1946.1      -5853.7
                 (3597.0)     (3377.0)
--------------------------------------
N                      74           74
adj. R-sq           0.273        0.478
--------------------------------------
Standard errors in parentheses
```

```
. esttab, beta not

--------------------------------------------
                      (1)             (2)   
                    price           price   
--------------------------------------------
weight              0.460**         0.913***
mpg                -0.097           0.043   
foreign                             0.573***
--------------------------------------------
N                      74              74   
--------------------------------------------
Standardized beta coefficients
* p<0.05, ** p<0.01, *** p<0.001
```

```
. esttab, wide

------------------------------------------------------
> ----------------
                      (1)                          (2)
>                 
                    price                        price
>                 
------------------------------------------------------
> ----------------
weight              1.747**        (2.72)        3.465
> ***       (5.49)
mpg                -49.51         (-0.57)        21.85
>           (0.29)
foreign                                         3673.1
> ***       (5.37)
_cons              1946.1          (0.54)      -5853.7
>          (-1.73)
------------------------------------------------------
> ----------------
N                      74                           74
>                 
------------------------------------------------------
> ----------------
t statistics in parentheses
* p<0.05, ** p<0.01, *** p<0.001
```

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


```
. esttab, b(%9.0g) p(4) r2(4) nostar wide

------------------------------------------------------
> ----------
                      (1)                       (2)   
>           
                    price                     price   
>           
------------------------------------------------------
> ----------
weight           1.746559     (0.0081)     3.464706   
>   (0.0000)
mpg             -49.51222     (0.5673)      21.8536   
>   (0.7693)
foreign                                     3673.06   
>   (0.0000)
_cons            1946.069     (0.5902)    -5853.696   
>   (0.0874)
------------------------------------------------------
> ----------
N                      74                        74   
>           
R-sq               0.2934                    0.4996   
>           
------------------------------------------------------
> ----------
p-values in parentheses
```

```
. esttab using example.csv
(output written to example.csv)
```
```
. esttab using example.rtf
(output written to example.rtf)
```

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

```stata
--------------------------------------------
                      (1)             (2)   
                    price           price   
--------------------------------------------
weight              1.747**         3.465***
                   (2.72)          (5.49)   

mpg                -49.51           21.85   
                  (-0.57)          (0.29)   

foreign                            3673.1***
                                   (5.37)   

_cons              1946.1         -5853.7   
                   (0.54)         (-1.73)   
--------------------------------------------
N                      74              74   
--------------------------------------------
t statistics in parentheses
* p<0.05, ** p<0.01, *** p<0.001
```
