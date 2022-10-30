# My lab reports

1. [Week 0](lab-report-1-week-0.html)
2. [Week 1](lab-report-2-week-1.html)
3. [Week 3](lab-report-3-week-3.html)
4. [Week 5](lab-report-4-week-5.html)

# Basic information

## Part 1: a listicle

Please **read all of the below**.

- I am Faith Okamoto, a second year (woot, survived the first one!) in Revelle College.

  ![Revelle College logo](https://revelle.ucsd.edu/_images/revelle-logo.png)
  
- > Pymgy hippos are the best hippos.

  *Don't believe me? See [here](hippos.html)*
  
- My family has two pets. In order of size:

  1. Snowball/Snowy, a Bichon mix. Small white dog, but larger than...
  2. Diana, a large black rabbit.
  
  It may be hard to believe, but small dogs are larger than large rabbits.

---

## Part 2: What was doing, coding-wise.

I have a job at an on-campus lab. Currently [when this was originally written] they have me fiddling with a Shiny app. It uses R to create HTML. I knew the `ggplot2` package in R but I've had to learn new functions such as `dbGetQuery` and `renderUI`. There are a truly frightening number of packages used which I'm fairly unfamiliar with. See the top of the file:

```
library(shiny)
library(shinydashboard)
library(shinyjs)
library(DT)
library(RPostgres)
library(shinyWidgets)
library(shinymanager)
library(ggplot2)
library(plotly)
library(scrypt)
library(DBI)
library(dplyr)
library(reshape2)
library(rjson)
library(ggpubr)
library(lares)
```
