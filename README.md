# STATA
Binary Logistic Model Using STATA
//OUTCOME//

tab bchkproff 
tab chkclin 
tab mammotest

gen bchkproffN = .
replace bchkproffN = 1 if bchkproff == 1
replace bchkproffN = 1 if bchkproff == 2
replace bchkproffN = 1 if bchkproff == 3
replace bchkproffN = 1 if bchkproff == 4
replace bchkproffN = 0 if bchkproff == 0

gen chkclinN = .
replace chkclinN = 1 if chkclin == 1
replace chkclinN = 1 if chkclin == 2
replace chkclinN = 1 if chkclin == 3
replace chkclinN = 1 if chkclin == 4
replace chkclinN = 0 if chkclin == 0


gen mammotestN = .
replace mammotestN = 1 if mammotest == 1
replace mammotestN = 1 if mammotest == 2
replace mammotestN = 1 if mammotest == 3
replace mammotestN = 1 if mammotest == 4
replace mammotestN = 0 if mammotest == 0

gen OUTCOME4 = .
replace OUTCOME4 = 3 if bchkproffN == 1
replace OUTCOME4 = 2 if chkclinN == 1
replace OUTCOME4 = 1 if mammotestN == 1
replace OUTCOME4 = 0 if bchkproffN == 0
replace OUTCOME4 = 0 if chkclinN == 0
replace OUTCOME4 = 0 if mammotestN == 0

tab OUTCOME4
proportion OUTCOME4

gen OUTCOME5 = .
replace OUTCOME5 = 1 if bchkproffN == 1
replace OUTCOME5 = 1 if chkclinN == 1
replace OUTCOME5 = 1 if mammotestN == 1
replace OUTCOME5 = 0 if bchkproffN == 0
replace OUTCOME5 = 0 if chkclinN == 0
replace OUTCOME5 = 0 if mammotestN == 0

tab OUTCOME5

gen outcome = .
replace outcome = chkclinN + mammotestN


tab outcome
xtile outcome3g = outcome, nq(3)

tab outcome3g
proportion outcome3g

gen outcome2 = .
replace outcome2 = 0 if outcome == 0
replace outcome2 = 1 if outcome == 1
replace outcome2 = 1 if outcome == 2

tab chkclinN
proportion chkclinN

tab mammotestN
proportion outcome2

//ini yang terakhir
tab outcome2
proportion outcome2


//UNIVARIATE//
**AGE**
tab age 
sum age, detail
swilk age
sum age, detail


tabstat age, stat(n mean sd median min max)

tab ageg
tab ageg outcome3g, row
ologit outcome3g i.ageg, or

tab ageg
tab ageg outcome2, row
logistic outcome2 i.ageg, or

tab ageg
tab ageg OUTCOME4, row
logistic OUTCOME4 i.ageg, or


**RELIGION**
tab religion
gen religionN = .
replace religionN = 3 if religion == 1
replace religionN = 2 if religion == 2
replace religionN = 1 if religion == 3
replace religionN = 1 if religion == 4
replace religionN = 1 if religion == 5
replace religionN = 1 if religion == 6

tab religionN
tab religionN outcome3g, row
ologit outcome3g i.religionN, or

gen religionN2 = .
replace religionN2 = 3 if religion == 1
replace religionN2 = 2 if religion == 2
replace religionN2 = 1 if religion == 3
replace religionN2 = 1 if religion == 4
replace religionN2 = 1 if religion == 5
replace religionN2 = 1 if religion == 6

tab religionN2
tab religionN2 outcome3g, row
ologit outcome3g i.religionN2, or


tab religionN
tab religionN outcome2, row
logistic outcome2 i.religionN, or


**EDUCATION LEVEL**
tab edulevl
gen edulevlN = .
replace edulevlN = 1 if edulevl == 0
replace edulevlN = 1 if edulevl == 1
replace edulevlN = 2 if edulevl == 2
replace edulevlN = 3 if edulevl == 3
replace edulevlN = 4 if edulevl == 4
replace edulevlN = 5 if edulevl == 5

tab edulevlN
tab edulevlN outcome3g, row
ologit outcome3g i.edulevlN, or

tab edulevlN
tab edulevlN outcome2, row
logistic outcome2 i.edulevlN, or

**MARITAL STATUS**
tab marsta
tab marsta outcome3g, row
ologit outcome3g i.marsta, or

tab marsta
tab marsta outcome2, row
logistic outcome2 i.marsta, or

**OCCUPATION**
tab occupt
gen occuptN = .
replace occuptN = 1 if occupt == 7
replace occuptN = 2 if occupt == 1
replace occuptN = 3 if occupt == 5
replace occuptN = 4 if occupt == 3
replace occuptN = 5 if occupt == 4

tab occuptN
tab occuptN outcome3g, row
ologit outcome3g i.occuptN, or

tab occuptN
tab occuptN outcome2, row
logistic outcome2 i.occuptN, or

**MONTHLY INCOME**
tab monthinc
tab monthinc outcome3g, row
ologit outcome3g i.monthinc, or

tab monthinc
tab monthinc outcome2, row
logistic outcome2 i.monthinc, or

**HEALTH INSURANCE**
tab heins
tab heins outcome3g, row
ologit outcome3g heins, or

tab heins
tab heins outcome2, row
logistic outcome2 heins, or

**KNOWLEDGE**
tab knowbcg
tab knowbcgN outcome3g, row
ologit outcome3g i.knowbcg, or


tab knowbcg
tab knowbcg outcome2, row
logistic outcome2 i.knowbcg, or

tab knowbcgN
tab knowbcgN outcome2, row
logistic outcome2 i.knowbcgN, or

gen knowbcgN = .
replace knowbcgN = 2 if knowbcg == 1
replace knowbcgN = 1 if knowbcg == 2
replace knowbcgN = 1 if knowbcg == 3


tab knowbcgN OUTCOME5, row
logistic OUTCOME5 knowbcgN, or


tab knowbcg
tab knowbcgN outcome2, row
logistic outcome2 knowbcgN, or



**BARRIER**
tab barrg
tab barrg outcome3g, row
ologit outcome3g i.barrg, or

tab barrg
tab barrg outcome2, row
logistic outcome2 i.barrg, or



logistic outcome2 i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins knowbcgN i.barrg, or







*INITIAL MODEL
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

*remove  i.occuptN
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest a

est store b

*remove  i.edulevlN 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest b

est store c

*remove  i.ageg
ologit outcome3g i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest c
 
est store d

*remove  i.religionN
ologit outcome3g i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest d

est store e
*remove  heins
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or

lrtest e

est store f
*remove  i.marsta 
ologit outcome3g i.monthinc i.knowbcg i.barrg, or

lrtest f

*Prob > chi2 =    0.0117

*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
**************************************************************
*FIND P VALUE POLYCOTHOMOUS INDEPENDENT
*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store o
ologit outcome3g i.marsta i.knowbcg i.barrg, or
lrtest o
*i.monthinc   Prob > chi2 =    0.0011


ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store p
ologit outcome3g i.marsta i.monthinc  i.barrg, or
lrtest p
*i.knowbcg Prob > chi2 =    0.1676

ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store q
ologit outcome3g i.marsta i.monthinc i.knowbcg , or
lrtest q
*i.barrg Prob > chi2 =    0.0478
**************************************************************


**************************************************************
**************************************************************
**************************************************************
**************************************************************
**************************************************************
**************************************************************


**************************************************************

*remove  i.religionN 
ologit outcome3g i.ageg i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest c

*i.religionN gagal di remove krna lrtest  Prob > chi2 =    0.0385

*FINAL MODEL 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or

**************************************************************


**************************************************************
*FIND P VALUE POLYCOTHOMOUS INDEPENDENT

* i.religionN :  Prob > chi2 =    0.0385

*FINAL MODEL 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
est store o

ologit outcome3g i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest o

* i.ageg  :  Prob > chi2 =    0.0442

*FINAL MODEL 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
est store p

ologit outcome3g i.ageg i.religionN i.monthinc heins i.knowbcg i.barrg, or
lrtest p

*i.marsta  : Prob > chi2 =    0.0123

*FINAL MODEL 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
est store q

ologit outcome3g i.ageg i.religionN i.marsta heins i.knowbcg i.barrg, or
lrtest q

*i.monthinc : Prob > chi2 =    0.0029

*FINAL MODEL 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
est store r

ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.barrg, or
lrtest r

*i.knowbcg : Prob > chi2 =    0.2347 (krn interest factor jd tetap diikutkan)

*FINAL MODEL 
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
est store s

ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg, or
lrtest s
* i.barrg :    Prob > chi2 =    0.0753
**************************************************************




*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store v
ologit outcome3g i.ageg i.religionN i.marsta i.monthinc heins i.knowbcg i.barrg, or
est store z

estimates stat v z

hausman z v





**************************************************************
**************************************************************
*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
**************************************************************
*FIND P VALUE POLYCOTHOMOUS INDEPENDENT
*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store n
ologit outcome3g i.monthinc i.knowbcg i.barrg, or
lrtest n
*i.marsta Prob > chi2 =    0.0117

*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store o
ologit outcome3g i.marsta i.knowbcg i.barrg, or
lrtest o
*i.monthinc   Prob > chi2 =    0.0011

*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store p
ologit outcome3g i.marsta i.monthinc  i.barrg, or
lrtest p
*i.knowbcg Prob > chi2 =    0.1676

*FINAL MODEL
ologit outcome3g i.marsta i.monthinc i.knowbcg i.barrg, or
est store q
ologit outcome3g i.marsta i.monthinc i.knowbcg , or
lrtest q
*i.barrg Prob > chi2 =    0.0478
**************************************************************
**************************************************************




**************************************************************
**************************************************************
***************************** STEPWARD ***********************
**************************************************************
**************************************************************
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
**************************************************************
* i.ageg Prob > chi2 =    0.0346

ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g  i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
lrtest a

drop _est_a

**************************************************************
*i.religionN Prob > chi2 =    0.0270
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
lrtest a

drop _est_a

**************************************************************
*i.edulevlN    Prob > chi2 =    0.0566
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.religionN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
lrtest a

drop _est_a

**************************************************************
*i.marsta  Prob > chi2 =    0.1532
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.religionN i.edulevlN i.occuptN i.monthinc heins i.knowbcg i.barrg, or
lrtest a

drop _est_a

**************************************************************
*i.occuptN  Prob > chi2 =    0.1869
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.monthinc heins i.knowbcg i.barrg, or
lrtest a

drop _est_a

**************************************************************
* i.monthinc Prob > chi2 =    0.0136
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN heins i.knowbcg i.barrg, or
lrtest a

drop _est_a

**************************************************************
* i.knowbcg  Prob > chi2 =    0.1949
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.barrg, or
lrtest a

drop _est_a

**************************************************************
* i.barrg Prob > chi2 =    0.0467
ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg i.barrg, or
est store a

ologit outcome3g i.ageg i.religionN i.edulevlN i.marsta i.occuptN i.monthinc heins i.knowbcg, or
lrtest a

drop _est_a

**************************************************************

****tambahan ageg
label define region 1"rural" 2 "urban"
label value region region 
tab region
table ageg region outcome2, row
//persen
by region, sort : tab1 outcome2, subpop(ageg)
by region, sort : tab1 ageg, subpop(outcome2)
by region, sort : tab1 ageg if outcome2 ==1
by region, sort : tab1 ageg if outcome2 ==0
//
by outcome2, sort : table ageg region, by(outcome2)
tab1 ageg region if outcome2 ==1
tab1 ageg region if outcome2 ==0
****************
//crude OR
tab ageg
by region, sort : logistic outcome2 i.ageg

***religion
tab religionN
by outcome2, sort : table religionN region, by(outcome2)
table religionN region outcome2, row
by region, sort : tab1 religionN if outcome2 ==1
by region, sort : tab1 religionN if outcome2 ==0
****************
//crude OR
tab religionN
by region, sort : logistic outcome2 i.religionN

***edulevlN
tab edulevlN
by outcome2, sort : table edulevlN region, by(outcome2)
table edulevlN region outcome2, row
by region, sort : tab1 edulevlN if outcome2 ==1
by region, sort : tab1 edulevlN if outcome2 ==0
****************
//crude OR
tab religionN
by region, sort : logistic outcome2 i.edulevlN







