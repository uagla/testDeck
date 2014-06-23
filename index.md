---
title       : CURB-65 Mortality prediction in Community Acquired Pneumonia
subtitle    : A Clinical Decision Rule
author      : Urko Aguirre
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [bootstrap, quiz, shiny, interactive]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Introduction 

* Community-acquired pneumonia (CAP) is the leading cause of death from infectious disease in the world and is a major burden on healthcare resources. 

* In the assessment and management of CAP, disease severity assessment is crucial because it guides therapeutic options (need of hospital or intensive care unit (ICU) admission, suitability of discharge to the  home, etc...).

*  The routine clinical judgment is often insufficient for assessing the severity of CAP: Clinical judgment alone may underestimate its severity and may lead to variations in the rates of admission to a hospital or ICU or even in discharged patients.

* An index which stratifies patients according to their risk mortality and features is the best tool for the management and assessment of the disease. 

--- 

## CURB-65: a Pneumonia Severity index. 

* It is a clinical rule recommended by the British Thoracic Society for the assessment of the severity of the pneumonia. CURB-65 involves calculating a score, which places a given patient into one of 6 risk classes (0-5). The higher the score, the higher the risk mortality is. 

| Variable           |  Description                    |     Condition                        |   Points  
|--------------------|---------------------------------|--------------------------------------|--------------
| `Age`              | Patient's age                   |    Age > 65 years                    |       1       |                    |                                 |                                      |
| `Confusion`        | Mental status                   |    Mentally altered                  |       1       |                    |                                 |                                      |
| `BUN`              | Blood urea nitrogen, in mmol/l  |    BUN > 7 mmol/l                    |       1       |                    |                                 |                                      |
| `RR`               | Respiratory rate                |    RR >= 30                          |       1       |                    |                                 |                                      |
| `SBP_DBP`          | Systolic and diastolic measure  |    Systolic <90 or Diastolic <60 mmHg|       1       |                                          

The total score is the sum of the obtained points in each variable. Management can be classified as:

| CURB-65            |  Management                     |   
|--------------------|---------------------------------|
| 0-1                | Home                            |                                           
| 2                  | Likely to need admission        |             
| 3-4-5              | Admission and manage as Severe  |    


---- 
### Example 
Suposse that a 80 aged and no mentally altered woman goes to the Emergency Room and has been diagnosed with CAP. Physicians have performed some tests in order to determine the severity of the disease. Results indicate that: a BUN = 10 mmol/l,  RR = 45 and diastolic pressure of 50.

```r
Age <- 80
Confusion <- 0
BUN <- 10
RR <- 45
SBP_DBP = 1  # since DBP = 50 < 60.
curb65 <- ifelse(Age > 65, 1, 0) + ifelse(Confusion == 1, 1, 0) + ifelse(BUN > 
    7, 1, 0) + ifelse(RR >= 30, 1, 0) + ifelse(SBP_DBP == 1, 1, 0)
management <- c("Home", "Home", "Likely to need admission", "Admission and manage as Severe", 
    "Admission and manage as Severe", "Admission and manage as Severe")
decission <- c(curb65, management[curb65 + 1])
decission
```

```
## [1] "4"                              "Admission and manage as Severe"
```


---- 

## Application
An screenshot of the performed [application](https://ggdrq.shinyapps.io/ProjectDef/) . 

<div class="container-fluid">
  <div class="row-fluid">
    <div class="span4">
      <form class="well">
        <p>Symptons</p>
        <div>
          <label class="control-label" for="Age">1. Age of patient</label>
          <input id="Age" type="slider" name="Age" value="60" class="jslider" data-from="0" data-to="110" data-step="1" data-skin="plastic" data-round="FALSE" data-locale="us" data-format="#,##0.#####" data-smooth="FALSE"/>
        </div>
        <div id="Confusion" class="control-group shiny-input-radiogroup">
          <label class="control-label" for="Confusion">2. Confusion (Mental Test)</label>
          <label class="radio">
            <input type="radio" name="Confusion" id="Confusion1" value="N" checked="checked"/>
            <span>No</span>
          </label>
          <label class="radio">
            <input type="radio" name="Confusion" id="Confusion2" value="Y"/>
            <span>Yes</span>
          </label>
        </div>
        <label for="BUN">3. Blood Urea Nitrogen (mmol/l)</label>
        <input id="BUN" type="number" value="4" min="0"/>
        <label for="RR">4. Respiratory Rate</label>
        <input id="RR" type="number" value="20" min="0"/>
        <div id="SBP_DBP" class="control-group shiny-input-radiogroup">
          <label class="control-label" for="SBP_DBP">5. Blood pressure: Systolic &lt; 90 mmHg or Diastolic &lt;=60 mmHg</label>
          <label class="radio">
            <input type="radio" name="SBP_DBP" id="SBP_DBP1" value="N" checked="checked"/>
            <span>No</span>
          </label>
          <label class="radio">
            <input type="radio" name="SBP_DBP" id="SBP_DBP2" value="Y"/>
            <span>Yes</span>
          </label>
        </div>
        <hr/>
      </form>
    </div>
    <div class="span8">
      <h3>CURB-65 score:</h3>
      <pre id="val" class="shiny-text-output"></pre>
      <h4>Management:</h4>
      <pre id="management" class="shiny-text-output"></pre>
    </div>
  </div>
</div> 


--- 


