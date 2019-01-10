---
title: "Sandwiches"
output: 
    html_document:
        keep_md: true
---



![](https://www.flickr.com/photos/skywhisperer/5724550360/)

## Puzzle

The neighborhood sandwich store makes the best sandwiches! They’ve got everything from classics like BLTs to more unusual options like Fluffernutters. Since many of their specialty ingredients keep going bad, they've decided to cut their selection and only focus on their best-selling sandwich. 

To help with the decision, the storeowners have collected data on their customers’ favorite sandwiches. Most people listed several varieties (in no particular order). Here’s a sample of the data:


names        sandwiches                                                            
-----------  ----------------------------------------------------------------------
Abby         Denver; BLT; Torta ahogada; Barbecue                                  
Abigail      BLT; Ftira; Primanti; Ice cream; Choripán                             
Adam         Corned beef; Montadito; Cheesesteak; Tripleta; Dagwood; Jambon-beurre 
Alexa        Dagwood; Mortadella                                                   
Alexandria   Slider; Beschuit met muisjes; Chicken salad                           
Ana          Fried brain; Polish boy; Vegetable; Pudgy Pie; Dagwood                

In this sample, the Dagwood sandwich is the most popular. 

In the full dataset, **what is the most popular sandwich among the customers?**

## Solution

Let's take a look at the file in R. Note that there are a few non-English letters that could give you some trouble depending on how you import the data into R. Sometimes the letters get "translated" into a mix of letters and punctuation (i.e. "ChoripÃ¡n" rather than "Choripán").


```r
sw
```

```
## # A tibble: 6 x 2
##   names      sandwiches                                                   
##   <chr>      <chr>                                                        
## 1 Abby       Denver; BLT; Torta ahogada; Barbecue                         
## 2 Abigail    BLT; Ftira; Primanti; Ice cream; Choripán                    
## 3 Adam       Corned beef; Montadito; Cheesesteak; Tripleta; Dagwood; Jamb~
## 4 Alexa      Dagwood; Mortadella                                          
## 5 Alexandria Slider; Beschuit met muisjes; Chicken salad                  
## 6 Ana        Fried brain; Polish boy; Vegetable; Pudgy Pie; Dagwood
```

As first step, let's separate out the sandwiches into individual observations using the handy `tidyr` function, `separate_rows()`. 


```r
sw %>% 
    separate_rows(sandwiches, sep = "; ") 
```

```
## # A tibble: 25 x 2
##    names   sandwiches   
##    <chr>   <chr>        
##  1 Abby    Denver       
##  2 Abby    BLT          
##  3 Abby    Torta ahogada
##  4 Abby    Barbecue     
##  5 Abigail BLT          
##  6 Abigail Ftira        
##  7 Abigail Primanti     
##  8 Abigail Ice cream    
##  9 Abigail Choripán     
## 10 Adam    Corned beef  
## # ... with 15 more rows
```

Keep in mind that omitting the space in the separator may cause some of the results not to match up. For example, "BLT" and " BLT" would require an extra cleaning step, such as `mutate(sandwiches = str_trim(sandwiches))`.

Next, we can count the sandwiches to determine which type is the most popular. Adding `sort = TRUE` brings the most popular sandwich to the top of the tibble.


```r
sw %>% 
    separate_rows(sandwiches, sep = "; ") %>% 
    count(sandwiches, sort = TRUE)
```

```
## # A tibble: 22 x 2
##    sandwiches               n
##    <chr>                <int>
##  1 Dagwood                  3
##  2 BLT                      2
##  3 Barbecue                 1
##  4 Beschuit met muisjes     1
##  5 Cheesesteak              1
##  6 Chicken salad            1
##  7 Choripán                 1
##  8 Corned beef              1
##  9 Denver                   1
## 10 Fried brain              1
## # ... with 12 more rows
```

That's it! With this small sample, you've got the basics of a working wrangling script that you can try out on the full data.
