---
title: "Airbnb Analytics"
author: "Analysis of data from Mexico City"
date: "2020-10-20"
output:
  html_document:
    theme: forty
    highlight: kate
    number_sections: yes
    toc: yes
    toc_float: yes
    code_folding: show
---

![](airbnb.jpg)





# Initiation

We will be analyzing AirBnB listing data to create a model to predict the total cost for two people staying 4 nights in AirBnB in **Mexico City**.


```r
# download the data
listings <- vroom::vroom("http://data.insideairbnb.com/mexico/df/mexico-city/2020-06-20/data/listings.csv.gz")
```
# Exploratory Data Analysis

First we will conduct an Exploratory Data Analysis to better understand our data, understanding the number of columns, rows, type of observations. We will also create scatterplots to better understand correlation between different variables. 

## Looking at the raw data


```r
# have an initial look at the data
glimpse(listings)
```

```
## Rows: 21,824
## Columns: 106
## $ id                                           <dbl> 35797, 56074, 61792, 706…
## $ listing_url                                  <chr> "https://www.airbnb.com/…
## $ scrape_id                                    <dbl> 2.02e+13, 2.02e+13, 2.02…
## $ last_scraped                                 <date> 2020-06-23, 2020-06-26,…
## $ name                                         <chr> "Villa Dante", "Great sp…
## $ summary                                      <chr> "Dentro de Villa un estu…
## $ space                                        <chr> "please go to (URL HIDDE…
## $ description                                  <chr> "Dentro de Villa un estu…
## $ experiences_offered                          <chr> "none", "none", "none", …
## $ neighborhood_overview                        <chr> "Centro comercial Santa …
## $ notes                                        <chr> "Si te gustan la tipo ha…
## $ transit                                      <chr> "Uber es buena opción o …
## $ access                                       <chr> "Jardin muy Amplio.", NA…
## $ interaction                                  <chr> "Cualquier duda contácte…
## $ house_rules                                  <chr> "Se renta un  estudio de…
## $ thumbnail_url                                <lgl> NA, NA, NA, NA, NA, NA, …
## $ medium_url                                   <lgl> NA, NA, NA, NA, NA, NA, …
## $ picture_url                                  <chr> "https://a0.muscache.com…
## $ xl_picture_url                               <lgl> NA, NA, NA, NA, NA, NA, …
## $ host_id                                      <dbl> 153786, 265650, 299558, …
## $ host_url                                     <chr> "https://www.airbnb.com/…
## $ host_name                                    <chr> "Dici", "Maris", "Robert…
## $ host_since                                   <date> 2010-06-28, 2010-10-19,…
## $ host_location                                <chr> "Mexico City, Mexico Cit…
## $ host_about                                   <chr> "Master in visual arts, …
## $ host_response_time                           <chr> "N/A", "within an hour",…
## $ host_response_rate                           <chr> "N/A", "100%", "100%", "…
## $ host_acceptance_rate                         <chr> "N/A", "91%", "67%", "10…
## $ host_is_superhost                            <lgl> FALSE, TRUE, FALSE, TRUE…
## $ host_thumbnail_url                           <chr> "https://a0.muscache.com…
## $ host_picture_url                             <chr> "https://a0.muscache.com…
## $ host_neighbourhood                           <chr> NA, "San Rafael", "Conde…
## $ host_listings_count                          <dbl> 2, 2, 1, 4, 3, 3, 4, 2, …
## $ host_total_listings_count                    <dbl> 2, 2, 1, 4, 3, 3, 4, 2, …
## $ host_verifications                           <chr> "['email', 'phone', 'rev…
## $ host_has_profile_pic                         <lgl> TRUE, TRUE, TRUE, TRUE, …
## $ host_identity_verified                       <lgl> FALSE, FALSE, FALSE, TRU…
## $ street                                       <chr> "Mexico City, D.f., Mexi…
## $ neighbourhood                                <chr> NA, "San Rafael", "Conde…
## $ neighbourhood_cleansed                       <chr> "Cuajimalpa de Morelos",…
## $ neighbourhood_group_cleansed                 <lgl> NA, NA, NA, NA, NA, NA, …
## $ city                                         <chr> "Mexico City", "Mexico C…
## $ state                                        <chr> "D.f.", "DF", "Ciudad de…
## $ zipcode                                      <chr> NA, NA, "06140", "04100"…
## $ market                                       <chr> "Mexico City", "Mexico C…
## $ smart_location                               <chr> "Mexico City, Mexico", "…
## $ country_code                                 <chr> "MX", "MX", "MX", "MX", …
## $ country                                      <chr> "Mexico", "Mexico", "Mex…
## $ latitude                                     <dbl> 19.4, 19.4, 19.4, 19.4, …
## $ longitude                                    <dbl> -99.3, -99.2, -99.2, -99…
## $ is_location_exact                            <lgl> FALSE, TRUE, TRUE, TRUE,…
## $ property_type                                <chr> "Villa", "Condominium", …
## $ room_type                                    <chr> "Entire home/apt", "Enti…
## $ accommodates                                 <dbl> 2, 3, 2, 2, 2, 2, 14, 2,…
## $ bathrooms                                    <dbl> 1.0, 1.0, 1.0, 1.0, 1.5,…
## $ bedrooms                                     <dbl> 1, 1, 1, 1, 1, 1, 4, 1, …
## $ beds                                         <dbl> 1, 2, 1, 1, 1, 1, 10, 0,…
## $ bed_type                                     <chr> "Futon", "Real Bed", "Re…
## $ amenities                                    <chr> "{Wifi,Kitchen,\"Free pa…
## $ square_feet                                  <dbl> 32292, 646, 161, NA, 155…
## $ price                                        <chr> "$4,500.00", "$843.00", …
## $ weekly_price                                 <chr> NA, "$4,740.00", NA, "$9…
## $ monthly_price                                <chr> "$124,995.00", "$15,724.…
## $ security_deposit                             <chr> NA, "$2,279.00", "$11,32…
## $ cleaning_fee                                 <chr> NA, "$684.00", "$340.00"…
## $ guests_included                              <dbl> 1, 2, 2, 2, 1, 1, 6, 1, …
## $ extra_people                                 <chr> "$0.00", "$342.00", "$11…
## $ minimum_nights                               <dbl> 1, 4, 2, 6, 4, 1, 2, 4, …
## $ maximum_nights                               <dbl> 7, 150, 21, 180, 365, 73…
## $ minimum_minimum_nights                       <dbl> 1, 4, 2, 6, 4, 1, 2, 4, …
## $ maximum_minimum_nights                       <dbl> 1, 4, 2, 6, 4, 1, 2, 4, …
## $ minimum_maximum_nights                       <dbl> 7, 1125, 21, 180, 365, 7…
## $ maximum_maximum_nights                       <dbl> 7, 1125, 21, 180, 365, 7…
## $ minimum_nights_avg_ntm                       <dbl> 1.0, 4.0, 2.0, 6.0, 4.0,…
## $ maximum_nights_avg_ntm                       <dbl> 7, 1125, 21, 180, 365, 7…
## $ calendar_updated                             <chr> "35 months ago", "4 week…
## $ has_availability                             <lgl> TRUE, TRUE, TRUE, TRUE, …
## $ availability_30                              <dbl> 23, 0, 30, 28, 0, 0, 25,…
## $ availability_60                              <dbl> 53, 0, 60, 58, 19, 0, 45…
## $ availability_90                              <dbl> 83, 0, 90, 88, 49, 18, 7…
## $ availability_365                             <dbl> 358, 0, 180, 363, 319, 1…
## $ calendar_last_scraped                        <date> 2020-06-23, 2020-06-26,…
## $ number_of_reviews                            <dbl> 0, 60, 52, 102, 10, 0, 2…
## $ number_of_reviews_ltm                        <dbl> 0, 2, 1, 10, 2, 0, 11, 1…
## $ first_review                                 <date> NA, 2017-11-18, 2017-11…
## $ last_review                                  <date> NA, 2019-07-24, 2019-11…
## $ review_scores_rating                         <dbl> NA, 97, 98, 98, 100, NA,…
## $ review_scores_accuracy                       <dbl> NA, 10, 10, 10, 10, NA, …
## $ review_scores_cleanliness                    <dbl> NA, 10, 10, 10, 10, NA, …
## $ review_scores_checkin                        <dbl> NA, 10, 10, 10, 10, NA, …
## $ review_scores_communication                  <dbl> NA, 10, 10, 10, 10, NA, …
## $ review_scores_location                       <dbl> NA, 10, 10, 10, 10, NA, …
## $ review_scores_value                          <dbl> NA, 10, 10, 10, 10, NA, …
## $ requires_license                             <lgl> FALSE, FALSE, FALSE, FAL…
## $ license                                      <lgl> NA, NA, NA, NA, NA, NA, …
## $ jurisdiction_names                           <chr> "{\"Mexico City\",\" MX …
## $ instant_bookable                             <lgl> FALSE, TRUE, FALSE, FALS…
## $ is_business_travel_ready                     <lgl> FALSE, FALSE, FALSE, FAL…
## $ cancellation_policy                          <chr> "flexible", "moderate", …
## $ require_guest_profile_picture                <lgl> FALSE, FALSE, FALSE, FAL…
## $ require_guest_phone_verification             <lgl> FALSE, FALSE, FALSE, FAL…
## $ calculated_host_listings_count               <dbl> 1, 2, 2, 3, 2, 3, 4, 2, …
## $ calculated_host_listings_count_entire_homes  <dbl> 1, 2, 0, 2, 2, 1, 2, 0, …
## $ calculated_host_listings_count_private_rooms <dbl> 0, 0, 2, 1, 0, 2, 2, 2, …
## $ calculated_host_listings_count_shared_rooms  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, …
## $ reviews_per_month                            <dbl> NA, 1.89, 1.62, 1.00, 0.…
```

**Before starting with the actual data cleaning, let's visualize which variables have a lot of missing values.**

```r
# identify missing values
plot_missing(listings,group = list(Good = 0.05, OK = 0.4, Bad = 0.8, Remove = 1), geom_label_args = list(),
  title = NULL,
  ggtheme = theme_gray(), theme_config = list(legend.position = c("bottom")))
```

<img src="index_files/figure-html/unnamed-chunk-3-1.png" width="960" />


**We will now look at the type of data to understand the kind of variables and the data type. Ensuring that quantitaive variables are stored as numeric data and dealing with missing values.**

## Transform data type

```r
# transform variables into correct data types
listings_wip <- listings %>% 
  
  # remove dollar signs and transform them to numeric data
  mutate(price = parse_number(price)) %>% 
  mutate(cleaning_fee = parse_number(cleaning_fee)) %>% 
  mutate(extra_people = parse_number(extra_people)) %>% 
  
  # turn character into factor
  mutate(room_type = fct_relevel(room_type,
                                 "Shared room",
                                 "Private room",
                                 "Entire home/apt")) %>% 

  # turn character into factor
  mutate(cancellation_policy = fct_relevel(cancellation_policy,
                                 "flexible",
                                 "moderate",
                                 "strict_14_with_grace_period",
                                 "super_strict_30",
                                 "super_strict_60"))
```

## Deal with missing values: cleaning_fee

```r
## number of NAs
sum(is.na(listings_wip$cleaning_fee))

## setting NAs to 0, because NA means no cleaning was booked
listings_wip$cleaning_fee[is.na(listings_wip$cleaning_fee)] <- 0
```

## Simplify variables:

```r
# Property type

## what are the most frequent types
listings_wip %>% 
  count(property_type, sort = TRUE) 
```

```
## # A tibble: 37 x 2
##    property_type          n
##    <chr>              <int>
##  1 Apartment          13545
##  2 House               3289
##  3 Condominium         1577
##  4 Loft                1120
##  5 Guest suite          571
##  6 Serviced apartment   369
##  7 Guesthouse           202
##  8 Boutique hotel       195
##  9 Hostel               177
## 10 Bed and breakfast    174
## # … with 27 more rows
```

```r
# visualize it with ggplot
ggplot(listings_wip)+
  geom_bar(aes(x=fct_infreq(property_type)))+
  coord_flip() + 
  labs(title = "Apartments clearly being the most common property type",
       subtitle = "Most frequent property types",
         x = "Property type", 
         y = "Count") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

```r
# simplify variable 
listings_wip <- listings_wip %>%
  mutate(prop_type_simplified = case_when(
    property_type %in% c("Apartment","House", "Condominium","Loft") ~ property_type, 
    TRUE ~ "Other"
  ))

# check if simplification worked
listings_wip %>%
  count(property_type, prop_type_simplified) %>%
  arrange(desc(n))  
```

```
## # A tibble: 37 x 3
##    property_type      prop_type_simplified     n
##    <chr>              <chr>                <int>
##  1 Apartment          Apartment            13545
##  2 House              House                 3289
##  3 Condominium        Condominium           1577
##  4 Loft               Loft                  1120
##  5 Guest suite        Other                  571
##  6 Serviced apartment Other                  369
##  7 Guesthouse         Other                  202
##  8 Boutique hotel     Other                  195
##  9 Hostel             Other                  177
## 10 Bed and breakfast  Other                  174
## # … with 27 more rows
```


```r
# Minimum nights

## what are the most common values
listings_wip2 <- listings_wip %>% 
  count(minimum_nights, sort = TRUE) 

## only include listings with minimum nights of 4
listings_wip3 <- listings_wip2 %>% 
  filter(minimum_nights <= 4)


# Display result
listings_wip3 %>%
 kbl(col.names =c("Minimum Nights","Count") ) %>%
  kable_material(c("striped", "hover")) %>%
  kable_styling(fixed_thead = T)
```

<table class=" lightable-material lightable-striped lightable-hover table" style='font-family: "Source Sans Pro", helvetica, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;'>
 <thead>
  <tr>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Minimum Nights </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Count </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10088 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 6474 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 2376 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 490 </td>
  </tr>
</tbody>
</table>

**Which value stands out & what is probably the purpose of listings with such minimum requirements?**

ANSWER: The minimum nights thresholds of **15, 30, 180 and 365** days stand out since they represent a large, but clearly defined period within a year (half a month, one month, half a year, one year). While many listings on Airbnb are peoples' private apartments which they rent out occasionally for a few days, properties with the minimum requirements mentioned above appear to have been built only for the purpose of renting. An example might be an apartment close to a big city which is targeting young professionals that work there for a limited time period. Therefore, those listings are not targeting the typical weekend trip tourists that are looking for a short stay. 


## Summary statistics of variables of interests

**To simplify a few steps later on, we are defining lists of variables of interest.**


```r
# select variables of interest
var_of_interest_quant = c("id", "price", "cleaning_fee", "extra_people", "number_of_reviews", "review_scores_rating")
var_of_interest_qual = c("property_type", "room_type", "neighbourhood")
var_of_interest = c(var_of_interest_quant, var_of_interest_quant)
```

**To begin, let's pick out a few particularly interesting variables and briefly look at their statistics.** 


```r
# compute summary statistics and look for missing values (NA)
skim(listings_wip)
```


<table style='width: auto;'
        class='table table-condensed'>
<caption>(\#tab:unnamed-chunk-9)Data summary</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;">   </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Name </td>
   <td style="text-align:left;"> listings_wip </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Number of rows </td>
   <td style="text-align:left;"> 21824 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Number of columns </td>
   <td style="text-align:left;"> 107 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> _______________________ </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Column type frequency: </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> character </td>
   <td style="text-align:left;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Date </td>
   <td style="text-align:left;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> factor </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> logical </td>
   <td style="text-align:left;"> 15 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> numeric </td>
   <td style="text-align:left;"> 42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ________________________ </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Group variables </td>
   <td style="text-align:left;"> None </td>
  </tr>
</tbody>
</table>


**Variable type: character**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
   <th style="text-align:right;"> empty </th>
   <th style="text-align:right;"> n_unique </th>
   <th style="text-align:right;"> whitespace </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> listing_url </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 37 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> name </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 255 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 21094 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> summary </td>
   <td style="text-align:right;"> 1443 </td>
   <td style="text-align:right;"> 0.93 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 18458 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> space </td>
   <td style="text-align:right;"> 6009 </td>
   <td style="text-align:right;"> 0.72 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 14005 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> description </td>
   <td style="text-align:right;"> 1141 </td>
   <td style="text-align:right;"> 0.95 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 19519 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> experiences_offered </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> neighborhood_overview </td>
   <td style="text-align:right;"> 6312 </td>
   <td style="text-align:right;"> 0.71 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 12861 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> notes </td>
   <td style="text-align:right;"> 13372 </td>
   <td style="text-align:right;"> 0.39 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 7097 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> transit </td>
   <td style="text-align:right;"> 7254 </td>
   <td style="text-align:right;"> 0.67 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 12094 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> access </td>
   <td style="text-align:right;"> 10332 </td>
   <td style="text-align:right;"> 0.53 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 9843 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> interaction </td>
   <td style="text-align:right;"> 7743 </td>
   <td style="text-align:right;"> 0.65 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 11412 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> house_rules </td>
   <td style="text-align:right;"> 9438 </td>
   <td style="text-align:right;"> 0.57 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10357 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> picture_url </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:right;"> 146 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 21216 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_url </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 13139 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_name </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 4149 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_location </td>
   <td style="text-align:right;"> 75 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 625 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_about </td>
   <td style="text-align:right;"> 8718 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 5443 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 7251 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_response_time </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_response_rate </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 52 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_acceptance_rate </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 81 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_thumbnail_url </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:right;"> 106 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 13103 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_picture_url </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 57 </td>
   <td style="text-align:right;"> 109 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 13103 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_neighbourhood </td>
   <td style="text-align:right;"> 9299 </td>
   <td style="text-align:right;"> 0.57 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 171 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_verifications </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 161 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> street </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 166 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 528 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> neighbourhood </td>
   <td style="text-align:right;"> 4894 </td>
   <td style="text-align:right;"> 0.78 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 54 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> neighbourhood_cleansed </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> city </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 146 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 270 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> state </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> zipcode </td>
   <td style="text-align:right;"> 1172 </td>
   <td style="text-align:right;"> 0.95 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 865 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> market </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> smart_location </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 154 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 291 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> country_code </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> country </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> property_type </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bed_type </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenities </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 1714 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 20546 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> weekly_price </td>
   <td style="text-align:right;"> 20852 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 573 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> monthly_price </td>
   <td style="text-align:right;"> 20873 </td>
   <td style="text-align:right;"> 0.04 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 603 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> security_deposit </td>
   <td style="text-align:right;"> 9696 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 671 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> calendar_updated </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 88 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> jurisdiction_names </td>
   <td style="text-align:right;"> 331 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> prop_type_simplified </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>


**Variable type: Date**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:left;"> min </th>
   <th style="text-align:left;"> max </th>
   <th style="text-align:left;"> median </th>
   <th style="text-align:right;"> n_unique </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> last_scraped </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 2020-06-20 </td>
   <td style="text-align:left;"> 2020-06-26 </td>
   <td style="text-align:left;"> 2020-06-21 </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_since </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 2009-02-03 </td>
   <td style="text-align:left;"> 2020-06-16 </td>
   <td style="text-align:left;"> 2016-11-01 </td>
   <td style="text-align:right;"> 2947 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> calendar_last_scraped </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> 2020-06-20 </td>
   <td style="text-align:left;"> 2020-06-26 </td>
   <td style="text-align:left;"> 2020-06-21 </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> first_review </td>
   <td style="text-align:right;"> 5371 </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:left;"> 2011-07-28 </td>
   <td style="text-align:left;"> 2020-06-22 </td>
   <td style="text-align:left;"> 2018-12-04 </td>
   <td style="text-align:right;"> 2113 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> last_review </td>
   <td style="text-align:right;"> 5371 </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:left;"> 2013-12-21 </td>
   <td style="text-align:left;"> 2020-06-25 </td>
   <td style="text-align:left;"> 2020-03-03 </td>
   <td style="text-align:right;"> 1287 </td>
  </tr>
</tbody>
</table>


**Variable type: factor**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:left;"> ordered </th>
   <th style="text-align:right;"> n_unique </th>
   <th style="text-align:left;"> top_counts </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> room_type </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> Ent: 10985, Pri: 10153, Sha: 388, Hot: 298 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> cancellation_policy </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> fle: 11310, mod: 6015, str: 4467, sup: 28 </td>
  </tr>
</tbody>
</table>


**Variable type: logical**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:left;"> count </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> thumbnail_url </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:left;"> : </td>
  </tr>
  <tr>
   <td style="text-align:left;"> medium_url </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:left;"> : </td>
  </tr>
  <tr>
   <td style="text-align:left;"> xl_picture_url </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:left;"> : </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_is_superhost </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.34 </td>
   <td style="text-align:left;"> FAL: 14359, TRU: 7465 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_has_profile_pic </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> TRU: 21784, FAL: 40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_identity_verified </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:left;"> FAL: 15266, TRU: 6558 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> neighbourhood_group_cleansed </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:left;"> : </td>
  </tr>
  <tr>
   <td style="text-align:left;"> is_location_exact </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.83 </td>
   <td style="text-align:left;"> TRU: 18037, FAL: 3787 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> has_availability </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:left;"> TRU: 21824 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> requires_license </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:left;"> FAL: 21824 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> license </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
   <td style="text-align:left;"> : </td>
  </tr>
  <tr>
   <td style="text-align:left;"> instant_bookable </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.57 </td>
   <td style="text-align:left;"> TRU: 12471, FAL: 9353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> is_business_travel_ready </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:left;"> FAL: 21824 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> require_guest_profile_picture </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:left;"> FAL: 21665, TRU: 159 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> require_guest_phone_verification </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:left;"> FAL: 21666, TRU: 158 </td>
  </tr>
</tbody>
</table>


**Variable type: numeric**

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> skim_variable </th>
   <th style="text-align:right;"> n_missing </th>
   <th style="text-align:right;"> complete_rate </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> sd </th>
   <th style="text-align:right;"> p0 </th>
   <th style="text-align:right;"> p25 </th>
   <th style="text-align:right;"> p50 </th>
   <th style="text-align:right;"> p75 </th>
   <th style="text-align:right;"> p100 </th>
   <th style="text-align:left;"> hist </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> id </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.84e+07 </td>
   <td style="text-align:right;"> 1.13e+07 </td>
   <td style="text-align:right;"> 3.58e+04 </td>
   <td style="text-align:right;"> 2.00e+07 </td>
   <td style="text-align:right;"> 3.02e+07 </td>
   <td style="text-align:right;"> 3.86e+07 </td>
   <td style="text-align:right;"> 4.39e+07 </td>
   <td style="text-align:left;"> ▂▃▅▅▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> scrape_id </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.02e+13 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 2.02e+13 </td>
   <td style="text-align:right;"> 2.02e+13 </td>
   <td style="text-align:right;"> 2.02e+13 </td>
   <td style="text-align:right;"> 2.02e+13 </td>
   <td style="text-align:right;"> 2.02e+13 </td>
   <td style="text-align:left;"> ▁▁▇▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_id </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.22e+08 </td>
   <td style="text-align:right;"> 9.68e+07 </td>
   <td style="text-align:right;"> 7.36e+03 </td>
   <td style="text-align:right;"> 3.75e+07 </td>
   <td style="text-align:right;"> 1.02e+08 </td>
   <td style="text-align:right;"> 1.94e+08 </td>
   <td style="text-align:right;"> 3.50e+08 </td>
   <td style="text-align:left;"> ▇▅▃▃▂ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_listings_count </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3.04e+01 </td>
   <td style="text-align:right;"> 2.82e+02 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 5.00e+00 </td>
   <td style="text-align:right;"> 3.33e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> host_total_listings_count </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3.04e+01 </td>
   <td style="text-align:right;"> 2.82e+02 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 5.00e+00 </td>
   <td style="text-align:right;"> 3.33e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latitude </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.94e+01 </td>
   <td style="text-align:right;"> 5.00e-02 </td>
   <td style="text-align:right;"> 1.92e+01 </td>
   <td style="text-align:right;"> 1.94e+01 </td>
   <td style="text-align:right;"> 1.94e+01 </td>
   <td style="text-align:right;"> 1.94e+01 </td>
   <td style="text-align:right;"> 1.96e+01 </td>
   <td style="text-align:left;"> ▁▁▅▇▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> longitude </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> -9.92e+01 </td>
   <td style="text-align:right;"> 4.00e-02 </td>
   <td style="text-align:right;"> -9.93e+01 </td>
   <td style="text-align:right;"> -9.92e+01 </td>
   <td style="text-align:right;"> -9.92e+01 </td>
   <td style="text-align:right;"> -9.92e+01 </td>
   <td style="text-align:right;"> -9.90e+01 </td>
   <td style="text-align:left;"> ▁▁▇▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> accommodates </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3.05e+00 </td>
   <td style="text-align:right;"> 2.21e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 4.00e+00 </td>
   <td style="text-align:right;"> 5.00e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bathrooms </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.40e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.50e+00 </td>
   <td style="text-align:right;"> 5.00e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bedrooms </td>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.42e+00 </td>
   <td style="text-align:right;"> 1.14e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 5.00e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> beds </td>
   <td style="text-align:right;"> 274 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 1.83e+00 </td>
   <td style="text-align:right;"> 1.75e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 5.00e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> square_feet </td>
   <td style="text-align:right;"> 21758 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 1.07e+03 </td>
   <td style="text-align:right;"> 3.99e+03 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 2.37e+02 </td>
   <td style="text-align:right;"> 8.40e+02 </td>
   <td style="text-align:right;"> 3.23e+04 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> price </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.48e+03 </td>
   <td style="text-align:right;"> 4.99e+03 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 4.08e+02 </td>
   <td style="text-align:right;"> 7.25e+02 </td>
   <td style="text-align:right;"> 1.32e+03 </td>
   <td style="text-align:right;"> 3.50e+05 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> cleaning_fee </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.38e+02 </td>
   <td style="text-align:right;"> 4.58e+02 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.12e+02 </td>
   <td style="text-align:right;"> 3.50e+02 </td>
   <td style="text-align:right;"> 2.40e+04 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> guests_included </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.62e+00 </td>
   <td style="text-align:right;"> 1.34e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 2.60e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> extra_people </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.23e+02 </td>
   <td style="text-align:right;"> 2.69e+02 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 2.00e+02 </td>
   <td style="text-align:right;"> 6.84e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> minimum_nights </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.09e+00 </td>
   <td style="text-align:right;"> 2.24e+01 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> maximum_nights </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.86e+02 </td>
   <td style="text-align:right;"> 7.08e+02 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 4.50e+01 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 5.00e+04 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> minimum_minimum_nights </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 3.89e+00 </td>
   <td style="text-align:right;"> 1.96e+01 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> maximum_minimum_nights </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.20e+00 </td>
   <td style="text-align:right;"> 2.26e+01 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 3.00e+00 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> minimum_maximum_nights </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8.47e+02 </td>
   <td style="text-align:right;"> 6.70e+02 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 3.65e+02 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 5.00e+04 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> maximum_maximum_nights </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8.49e+02 </td>
   <td style="text-align:right;"> 6.69e+02 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 3.65e+02 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 5.00e+04 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> minimum_nights_avg_ntm </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.04e+00 </td>
   <td style="text-align:right;"> 2.06e+01 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> maximum_nights_avg_ntm </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 8.48e+02 </td>
   <td style="text-align:right;"> 6.69e+02 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 3.65e+02 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 1.12e+03 </td>
   <td style="text-align:right;"> 5.00e+04 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> availability_30 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.95e+01 </td>
   <td style="text-align:right;"> 1.24e+01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.60e+01 </td>
   <td style="text-align:right;"> 3.00e+01 </td>
   <td style="text-align:right;"> 3.00e+01 </td>
   <td style="text-align:left;"> ▃▁▁▂▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> availability_60 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.21e+01 </td>
   <td style="text-align:right;"> 2.38e+01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 2.20e+01 </td>
   <td style="text-align:right;"> 5.60e+01 </td>
   <td style="text-align:right;"> 6.00e+01 </td>
   <td style="text-align:right;"> 6.00e+01 </td>
   <td style="text-align:left;"> ▂▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> availability_90 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.54e+01 </td>
   <td style="text-align:right;"> 3.47e+01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 4.90e+01 </td>
   <td style="text-align:right;"> 8.50e+01 </td>
   <td style="text-align:right;"> 9.00e+01 </td>
   <td style="text-align:right;"> 9.00e+01 </td>
   <td style="text-align:left;"> ▂▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> availability_365 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.20e+02 </td>
   <td style="text-align:right;"> 1.39e+02 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 8.90e+01 </td>
   <td style="text-align:right;"> 2.11e+02 </td>
   <td style="text-align:right;"> 3.61e+02 </td>
   <td style="text-align:right;"> 3.65e+02 </td>
   <td style="text-align:left;"> ▃▂▃▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> number_of_reviews </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 2.36e+01 </td>
   <td style="text-align:right;"> 4.18e+01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 6.00e+00 </td>
   <td style="text-align:right;"> 2.80e+01 </td>
   <td style="text-align:right;"> 5.55e+02 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> number_of_reviews_ltm </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 9.18e+00 </td>
   <td style="text-align:right;"> 1.50e+01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.20e+01 </td>
   <td style="text-align:right;"> 1.77e+02 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_rating </td>
   <td style="text-align:right;"> 5616 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.51e+01 </td>
   <td style="text-align:right;"> 8.64e+00 </td>
   <td style="text-align:right;"> 2.00e+01 </td>
   <td style="text-align:right;"> 9.40e+01 </td>
   <td style="text-align:right;"> 9.70e+01 </td>
   <td style="text-align:right;"> 1.00e+02 </td>
   <td style="text-align:right;"> 1.00e+02 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_accuracy </td>
   <td style="text-align:right;"> 5632 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.72e+00 </td>
   <td style="text-align:right;"> 8.50e-01 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_cleanliness </td>
   <td style="text-align:right;"> 5632 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.59e+00 </td>
   <td style="text-align:right;"> 9.20e-01 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 9.00e+00 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_checkin </td>
   <td style="text-align:right;"> 5635 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.82e+00 </td>
   <td style="text-align:right;"> 7.20e-01 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_communication </td>
   <td style="text-align:right;"> 5631 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.78e+00 </td>
   <td style="text-align:right;"> 7.90e-01 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_location </td>
   <td style="text-align:right;"> 5636 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.81e+00 </td>
   <td style="text-align:right;"> 6.80e-01 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> review_scores_value </td>
   <td style="text-align:right;"> 5637 </td>
   <td style="text-align:right;"> 0.74 </td>
   <td style="text-align:right;"> 9.58e+00 </td>
   <td style="text-align:right;"> 9.00e-01 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 9.00e+00 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:right;"> 1.00e+01 </td>
   <td style="text-align:left;"> ▁▁▁▁▇ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> calculated_host_listings_count </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 6.09e+00 </td>
   <td style="text-align:right;"> 1.60e+01 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 4.00e+00 </td>
   <td style="text-align:right;"> 1.57e+02 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> calculated_host_listings_count_entire_homes </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 4.09e+00 </td>
   <td style="text-align:right;"> 1.55e+01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 1.57e+02 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> calculated_host_listings_count_private_rooms </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 1.77e+00 </td>
   <td style="text-align:right;"> 4.20e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 1.00e+00 </td>
   <td style="text-align:right;"> 2.00e+00 </td>
   <td style="text-align:right;"> 4.80e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> calculated_host_listings_count_shared_rooms </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 9.00e-02 </td>
   <td style="text-align:right;"> 8.30e-01 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 0.00e+00 </td>
   <td style="text-align:right;"> 2.00e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reviews_per_month </td>
   <td style="text-align:right;"> 5371 </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:right;"> 1.36e+00 </td>
   <td style="text-align:right;"> 1.50e+00 </td>
   <td style="text-align:right;"> 1.00e-02 </td>
   <td style="text-align:right;"> 3.00e-01 </td>
   <td style="text-align:right;"> 8.20e-01 </td>
   <td style="text-align:right;"> 1.92e+00 </td>
   <td style="text-align:right;"> 1.47e+01 </td>
   <td style="text-align:left;"> ▇▁▁▁▁ </td>
  </tr>
</tbody>
</table>

```r
#Output in kable format

favstats(listings_wip$price) %>% 
  kbl(caption = "Listing Price", col.names = c("Minimum", "Q1","Median", "Q3", "Maximum", "Mean", "SD", "Count", "Missing")) %>%
  kable_material(c("striped", "hover")) %>%
  kable_styling(fixed_thead = T)
```

<table class=" lightable-material lightable-striped lightable-hover table" style='font-family: "Source Sans Pro", helvetica, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;'>
<caption>(\#tab:unnamed-chunk-9)Listing Price</caption>
 <thead>
  <tr>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">   </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Minimum </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Q1 </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Median </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Q3 </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Maximum </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Mean </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> SD </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Count </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Missing </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 408 </td>
   <td style="text-align:right;"> 725 </td>
   <td style="text-align:right;"> 1316 </td>
   <td style="text-align:right;"> 349990 </td>
   <td style="text-align:right;"> 1484 </td>
   <td style="text-align:right;"> 4987 </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>

```r
favstats(listings_wip$cleaning_fee) %>% 
  kbl(caption = "Cleaning Fee", col.names = c("Minimum", "Q1","Median", "Q3", "Maximum", "Mean", "SD", "Count", "Missing")) %>%
  kable_material(c("striped", "hover")) %>%
  kable_styling(fixed_thead = T)
```

<table class=" lightable-material lightable-striped lightable-hover table" style='font-family: "Source Sans Pro", helvetica, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;'>
<caption>(\#tab:unnamed-chunk-9)Cleaning Fee</caption>
 <thead>
  <tr>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">   </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Minimum </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Q1 </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Median </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Q3 </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Maximum </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Mean </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> SD </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Count </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Missing </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 112 </td>
   <td style="text-align:right;"> 350 </td>
   <td style="text-align:right;"> 24000 </td>
   <td style="text-align:right;"> 238 </td>
   <td style="text-align:right;"> 458 </td>
   <td style="text-align:right;"> 21824 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>

```r
favstats(listings_wip$review_scores_rating) %>% 
  kbl(caption = "Scores Rating", col.names = c("Minimum", "Q1","Median", "Q3", "Maximum", "Mean", "SD", "Count", "Missing")) %>%
  kable_material(c("striped", "hover")) %>%
  kable_styling(fixed_thead = T)
```

<table class=" lightable-material lightable-striped lightable-hover table" style='font-family: "Source Sans Pro", helvetica, sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;'>
<caption>(\#tab:unnamed-chunk-9)Scores Rating</caption>
 <thead>
  <tr>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">   </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Minimum </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Q1 </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Median </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Q3 </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Maximum </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Mean </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> SD </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Count </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Missing </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 94 </td>
   <td style="text-align:right;"> 97 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 95.1 </td>
   <td style="text-align:right;"> 8.64 </td>
   <td style="text-align:right;"> 16208 </td>
   <td style="text-align:right;"> 5616 </td>
  </tr>
</tbody>
</table>

## Creating informative visualizations

By creating infromative visuals we will  be able to better understand the relationships between different variables in our dataframe and help get a better picture.

### Quantitative variables

**Let's look how the selected quantitative variables are distributed.**

**Observations**          
          
We may see that the distribution of review ratings has a high concentration on scores between *95 and 98* and is **strongly skewed to the left**, which might indicate that people are more likely to give positive feedback.  While for other variables, we may see after adjusting the x-axis to log10 scale, cleaning fees, extra people and price show approximately normal distribution, although there are some outliers in prices which are extremely large due to the longer period of rent. The number of reviews is more uniformly distributed.


```r
# visualizing the quantitative variables
quant_plot1 <- listings_wip %>%
  select(var_of_interest_quant) %>% 
  pivot_longer(cols=2:6, names_to="Category", values_to="Value")

# create ggplot with facet wrap to have each variable next to each other
ggplot(quant_plot1, aes(x=Value)) +
  geom_histogram() +
  facet_wrap(~Category) +
  scale_x_log10() + 
  labs(title = "Review scores stand out - strongly skewed to the left",
       subtitle = "Distribution of selected quantitative variables",
         x = "Log10", 
         y = "Count") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-10-1.png" width="672" />

**Let's look at the relationship between a few selected variables.**

**1. Relationship between price and review score rating**

As per the below Scatter plot it is evident that the price of the stay and the review ratings have a **slightly positive correlation**. However, we must note that most of the listings have a rating score of 70 or higher.


```r
# select relevant variables
quant_plot2 <- listings_wip %>%
  select(id, price, bedrooms, number_of_reviews, minimum_nights, review_scores_rating)

# plot price - rating relationship
ggplot(quant_plot2, aes(x=review_scores_rating, y=price)) +
  geom_point() +
  scale_y_log10() +
  labs(title = "Majority of listing have rating above 70 - afterwards slightly positive correlation",
       subtitle = "Relationship between price and review score rating",
         x = "Review score rating", 
         y = "Price (log10)") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-11-1.png" width="672" />
         

**2. Relationship between price and number of reviews **

The plot below demonstrates that there are **relatively fewer number of ratings for expensive listings**, this could be due to fewer people being able and/or willing to spend the amount required to stay at those properties. 


```r
# plot price - # of reviews relationship
ggplot(quant_plot2, aes(x=number_of_reviews, y=price)) +
  geom_point() +
  scale_y_log10() +
  labs(title = "Intuitive relationship - only few can afford/ review very expensive listings",
       subtitle = "Relationship between price and number of reviews",
         x = "Number of reviews", 
         y = "Price (log10)") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

**3. Relationship between the number of reviews and the rating**

Upon analyzing the relationship between number of reviews and rating score it is evident that as **more people review for a property, it is more likely to be highly rated**.


```r
# plot number of reviews - review rating score
ggplot(quant_plot2, aes(x=number_of_reviews, y=review_scores_rating)) +
  geom_point() +
  scale_y_log10() +
  scale_x_log10() +
  labs(title = "Increasing number of reviews reduces variance in ratings - the more the better",
       subtitle = "Relationship between the number of reviews and the rating",
         x = "Number of reviews (log10)", 
         y = "Rating (log10)") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-13-1.png" width="672" />


### Qualitative variables

**Let's have a first look again on the property type and prices per night.**

**Observation**       

From the graph, we may see among all properties in Mexico, **apartments are the most common type** with more than 12000 properties collected and **lofts are the least common** with only around 1200. This might be because an apartment has better room layout with convenient facilities and relatively acceptable price and thus it is more popular.  
  
We may also see that loft has the highest median price per night among all types, which might also be the reasons for low quantity demanded. While apartment has a medium reasonable price, it is surprising to notice that median price per night for house is lower than that for apartment. One of the reasons might be houses are normally located at rural area and apartments are more central.


```r
# plot the different property types
ggplot(listings_wip, aes(x=fct_infreq(prop_type_simplified))) +
  geom_bar() +
  labs(title = "Appartment by far the most common property type on Airbnb in Mexico",
       subtitle = "Property types in descending order",
         x = "Property types", 
         y = "Count") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-14-1.png" width="672" />

```r
# look at price levels for each type
property_price <- listings_wip %>% 
  group_by(prop_type_simplified) %>% 
  summarise(avg_price = median(price))

# plot median prices for each property type
ggplot(property_price, aes(x=reorder(prop_type_simplified, -avg_price), y=avg_price)) +
  geom_bar(stat="identity") +
  labs(title = "House cheaper than appartment - probably due to less central location",
       subtitle = "Median price by property type in descending order",
         x = "Property types", 
         y = "Median price MXN per night") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-14-2.png" width="672" />


**Let's then analyse the room types and prices per night.**

**Observation**      

When comparing hotel rooms with entire apartments, we might see the **number of entire apartments are nearly 30 times higher than the number of hotel rooms**. This might be because that hotels are more likely to be posted on other hotel websites instead of Airbnb.          

And also, **hotel rooms have the highest median price per night** (more than $1300) among all types of properties, with **shared rooms being the cheapest**. This is intuitively correct as shared rooms tend to be less convenient and luxurious than hotel rooms.


```r
listings_wip %>% 
  count(room_type, sort = TRUE) 
```

```
## # A tibble: 4 x 2
##   room_type           n
##   <fct>           <int>
## 1 Entire home/apt 10985
## 2 Private room    10153
## 3 Shared room       388
## 4 Hotel room        298
```

```r
# plot the different property types
ggplot(listings_wip, aes(x=fct_infreq(room_type))) +
  geom_bar() +
  labs(title = "Entire apartment by far the most common room type",
       subtitle = "Room types in descending order",
         x = "Room types", 
         y = "Count") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

```r
# look at price levels for each type
room_price <- listings_wip %>% 
  group_by(room_type) %>% 
  summarise(avg_price_room = median(price))

# plot median prices for each property type
ggplot(room_price, aes(x=reorder(room_type, -avg_price_room), y=avg_price_room)) +
  geom_bar(stat="identity") +
  labs(title = "Hotel rooms are the most expensive room type - shared rooms the cheapest",
       subtitle = "Median price by property type in descending order",
         x = "Property types", 
         y = "Median price MXN per night") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-15-2.png" width="672" />


**Next, let's find out in which neighborhoods the listings are primarily located.**

**Observation**  
      
We may see **Polanco and Roma Norte have the most listings** with more than 1500 properties. This is because the two neighborhoods are mostly where tourists tend to cluster, and are both affluent areas. As there are higher demands for accommodation from tourists, the provision of listings is also higher than other areas. San Pedro De Los Pinos has the least listing because this neighborhood is located in the west of Mexico City and relatively rural and inconvenient in terms of transportation for tourists.          

It is also noticeable that there are many listings without specified area. Since only high level perspective and many unlabeled values, we are going to have a closer look at the locations in the next step - the mapping. 


```r
# filter out 20 most common neighborhoods
top_neighbourhoods <- listings_wip %>% 
  count(neighbourhood, sort = TRUE) %>% 
  top_n(20)

# visualize it with ggplot
ggplot(top_neighbourhoods, aes(x=reorder(neighbourhood, n), y=n))+
  geom_bar(stat="identity") + 
  coord_flip() + 
  labs(title = "Despite many unlabeled neighbourhoods, Polanco and Roma Norte most common",
       subtitle = "Neighbourhoods with most listings", 
         x = "Neighbourhood", 
         y = "# of listings") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-16-1.png" width="960" />

# Mapping

**Let's have deeper look at the listings' locations by plotting them on a map of mexico city. The color of the dots indicate the price level.**


```r
bubbles_color <- colorNumeric(palette = c("darkgreen", "yellow", "red"), listings_wip$price)
leaflet(data = filter(listings_wip)) %>% 
  addProviderTiles("OpenStreetMap.Mapnik") %>%
  addCircleMarkers(lng = ~longitude, 
                   lat = ~latitude, 
                   radius = 1, 
                   color = ~bubbles_color(price), 
                   fillOpacity = 0.4, 
                   popup = ~listing_url,
                  label =  ~paste(room_type,"|",
                            "Price: ", price)) %>% 
  addLegend("bottomleft", pal = bubbles_color, 
            values = ~price,
            title = "Price of listing",
            labFormat = labelFormat(prefix = "MXN "),
            
  opacity = 5)
```

preservef1a6cfa95e7a258f


**It becomes obvious that the prices seem way to high and undifferentiated. This is because there are a few very high outlier prices. Let's remove outliers and look at the map again.**



```r
# look at statistics to see where to make the cut
print(fav_stats(listings_wip$price))
```

```
##  min  Q1 median   Q3    max mean   sd     n missing
##    0 408    725 1316 349990 1484 4987 21824       0
```

```r
# check length before making the cut
length(listings_wip$price)
```

```
## [1] 21824
```

```r
# make the cut
normal_prices <- listings_wip %>% 
  filter(price > 100) %>% 
  filter(price < 2500)

# check length after making the cut 
length(normal_prices$price)
```

```
## [1] 19895
```

Thereby, we **cut out extreme outliers** while keeping >90% of the values. Let's look at the new map.



```r
bubbles_color <- colorNumeric(palette = c("darkgreen", "yellow", "red"), normal_prices$price)
leaflet(data = filter(normal_prices)) %>% 
  addProviderTiles("OpenStreetMap.Mapnik") %>%
  addCircleMarkers(lng = ~longitude, 
                   lat = ~latitude, 
                   radius = 1, 
                   color = ~bubbles_color(price), 
                   fillOpacity = 0.4, 
                   popup = ~listing_url,
                  label =  ~paste(room_type,"|", "Price: ", price)) %>% 
  addLegend("bottomleft", pal = bubbles_color,
            values = ~price,
            title = "Price of listing",
            labFormat = labelFormat(prefix = "MXN "),
  
  opacity = 5)
```

preserve4f431f3080f70662


# Regression

## Create and analyse target variable Y (price_4_night)

To create the new variable, we continue to use the **normalized prices** (calculated earlier) which exclude outliers.


```r
# Calculate the price for 4 nights for 2 people using the normalized prices to exclude outliers
normal_prices <- normal_prices %>% 
  mutate(
    price_4_nights = case_when(guests_included >= 2 ~ (price*4+cleaning_fee),
                                    TRUE ~ ((price+extra_people)*4+cleaning_fee)),
    log_price_4_nights = log(price_4_nights)
    )
```

Let's compare both distributions: without transformation vs. log transformation.

**Observation**

Since the non-transformed data is clearly *skewed to the right*, it makes sense to continue with the log data which is normally distributed. It would be better to use normal distribution here because we can then analyse correlations and make predictions more accurately.


```r
# create a histogram to observe the distribution - without log10
ggplot(normal_prices,aes(x=price_4_nights)) + 
  geom_histogram() + 
  theme_bw() + 
  scale_y_log10() +
  labs(
    title="The distribution of price_4_nights is skewed to the right", 
    subtitle="Distribution of not transformed data",
    x="Price_4_nights", 
    y="Count") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-21-1.png" width="672" />

```r
# create a histogram to observe the distribution - log10
ggplot(normal_prices,aes(x=log_price_4_nights)) + 
  geom_histogram() + 
  theme_bw() + 
  labs(
    title="The log distribution of price_4_nights is normally distributed", 
    subtitle="Distribution of log transformation",
    x="price_4_nights (log)", 
    y="Count") +
  theme_bw()
```

<img src="index_files/figure-html/unnamed-chunk-21-2.png" width="672" />

## Model 1


```r
# create model 1
model1 <- lm(log_price_4_nights ~ prop_type_simplified + number_of_reviews + review_scores_rating, data=normal_prices)
    msummary(model1)
```

```
##                                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                      7.690839   0.055059  139.68  < 2e-16 ***
## prop_type_simplifiedCondominium  0.064090   0.019102    3.36    8e-04 ***
## prop_type_simplifiedHouse       -0.373289   0.014437  -25.86  < 2e-16 ***
## prop_type_simplifiedLoft         0.128907   0.020792    6.20  5.8e-10 ***
## prop_type_simplifiedOther       -0.178072   0.017844   -9.98  < 2e-16 ***
## number_of_reviews                0.001583   0.000108   14.68  < 2e-16 ***
## review_scores_rating             0.003781   0.000575    6.57  5.1e-11 ***
## 
## Residual standard error: 0.602 on 15041 degrees of freedom
##   (4847 observations deleted due to missingness)
## Multiple R-squared:  0.0764,	Adjusted R-squared:  0.076 
## F-statistic:  207 on 6 and 15041 DF,  p-value: <2e-16
```

```r
    autoplot(model1)
```

<img src="index_files/figure-html/unnamed-chunk-22-1.png" width="672" />

```r
    car::vif(model1)
```

```
##                      GVIF Df GVIF^(1/(2*Df))
## prop_type_simplified 1.03  4            1.00
## number_of_reviews    1.03  1            1.01
## review_scores_rating 1.01  1            1.00
```
**Diagnostics of model 1:**

- Residuals vs. fitted: Linearly assumption is **fulfilled** since the residuals are randomly distributed.
- Normal Q-Q: Normality assumption is decently **fulfilled**, however, small deviations towards higher quantities 
- Scale-Location: Equal variance assumption seems to be **fulfilled**.
- Residuals vs. factor levels: Occasionally, there are a few points with a high leverage impact the estimation.
- Variance Inflation Factor (VIF): **Not an issue** since all well below 5.

*Overall:* The overall model is significant (see F-statistic) and all explanatory variables are significant at least on a p = 0.01 level. However, the model only explains around 5% of the variance, which is very low. 



Let's interpret the coefficient of review_scores_rating and prop_type_simplified. 
*Note:* Since property types are factors with k level, apartment is left out and serves as baseline.


```r
# anti-log the coefficients
condominium <- (exp(0.051008)-1)*100
house <- (exp(-0.371592)-1)*100
loft <- (exp(0.123123)-1)*100
other <- (exp(-0.165880)-1)*100

reviews <- (exp(0.001482)-1)*100
rating <- (exp(0.003389)-1)*100


cat("Condominium =", condominium)
```

```
## Condominium = 5.23
```

```r
cat("\nHouse =", house)
```

```
## 
## House = -31
```

```r
cat("\nLoft =", loft)
```

```
## 
## Loft = 13.1
```

```r
cat("\nOther =", other)
```

```
## 
## Other = -15.3
```

```r
cat("\nReviews =", reviews)
```

```
## 
## Reviews = 0.148
```

```r
cat("\nRating =", rating)
```

```
## 
## Rating = 0.339
```

**Interpretation**

- The model predicts a 4 nights stay in a condominium to be 5.23% more expensive than in an apartment.
- The model predicts a 4 nights stay in a house to be 31% less expensive than in an apartment.
- The model predicts a 4 nights stay in a condominium to be 13.1% more expensive than in an apartment.
- The model predicts a 4 nights stay in a other property types to be 15.3% less expensive than in an apartment.

- The model predicts a 4 nights stay to become 0.14% more expensive with each additional review.
- The model predicts a 4 nights stay to become 0.39% more expensive with each additional rating point


## Model 2: Room_type a good predictor?

Let's see if the **room type** of an airbnb enhances our model, being a good predictor. 


```r
# create model 2 (just adding room_type as a explanatory variable)
model2 <- lm(log_price_4_nights ~ prop_type_simplified + number_of_reviews + review_scores_rating + room_type, data=normal_prices)
    msummary(model2)
```

```
##                                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                      7.10e+00   5.61e-02  126.64  < 2e-16 ***
## prop_type_simplifiedCondominium  4.97e-02   1.56e-02    3.19   0.0014 ** 
## prop_type_simplifiedHouse       -1.23e-02   1.25e-02   -0.98   0.3266    
## prop_type_simplifiedLoft        -6.74e-02   1.71e-02   -3.94  8.2e-05 ***
## prop_type_simplifiedOther       -4.65e-02   1.57e-02   -2.97   0.0030 ** 
## number_of_reviews                2.43e-04   8.93e-05    2.73   0.0064 ** 
## review_scores_rating             3.10e-03   4.69e-04    6.61  4.0e-11 ***
## room_typePrivate room            2.35e-01   3.55e-02    6.63  3.5e-11 ***
## room_typeEntire home/apt         1.00e+00   3.56e-02   28.09  < 2e-16 ***
## room_typeHotel room              8.54e-01   5.22e-02   16.35  < 2e-16 ***
## 
## Residual standard error: 0.491 on 15038 degrees of freedom
##   (4847 observations deleted due to missingness)
## Multiple R-squared:  0.386,	Adjusted R-squared:  0.386 
## F-statistic: 1.05e+03 on 9 and 15038 DF,  p-value: <2e-16
```

```r
    autoplot(model2)
```

<img src="index_files/figure-html/unnamed-chunk-24-1.png" width="672" />

```r
    car::vif(model2)
```

```
##                      GVIF Df GVIF^(1/(2*Df))
## prop_type_simplified 1.38  4            1.04
## number_of_reviews    1.06  1            1.03
## review_scores_rating 1.01  1            1.00
## room_type            1.41  3            1.06
```

**Diagnostics of model 2:**


- Residuals vs. fitted: Linearly assumption is **fulfilled** since the residuals are randomly distributed with equal magnitude and concentration numbers above and below.
- Normal Q-Q: Normality assumption is **fulfilled** for the values in the middle, however, small deviations towards quantities on two extreme sides.
- Scale-Location: Equal variance assumption seems to be **fulfilled** since standarised residuals are randomly distributed with equal magnitude and concentration numbers above and below.
- Residuals vs. factor levels:  Equally distributed above and below, no extreme leverages impact the estimation.
- Variance Inflation Factor (VIF): **Not an issue** since all well below 5.

*Overall:* The overall model is significant (see F-statistic) and most explanatory variables are significant at least on a p = 0.01 level except the number of reviewing and it shows that there is no significant price difference for 4 nights between apartments and houses.This also explains that an additional reviewing will not increase the price significantly. The model explains 38.5% of the variance, which is higher than the original model. Therefore, this adjusted model is stronger when analysing the correlation between prices and other factors and makes a more accurate estimation.


## Additional models: Further questions to explore

**Are the number of bathrooms, bedrooms, beds, or size of the house (accomodates) significant predictors of price_4_nights?**   
     
ANSWER: Since bedrooms, bathrooms, and beds are highly correlated, we only select bedrooms as a new predictor. Looking at the diagnostics, we realize however that there are some outliers that add avoidable errors. Therefore, we create the model again with a cleaned bedrooms variable in the next code of chunk.


```r
# adding bedrooms variable (without removing outliers)
model3 <- lm(log_price_4_nights ~ prop_type_simplified + number_of_reviews + review_scores_rating + room_type  + bedrooms, data=normal_prices)
    msummary(model3)
```

```
##                                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                      6.99e+00   5.57e-02  125.61  < 2e-16 ***
## prop_type_simplifiedCondominium  4.73e-02   1.54e-02    3.08   0.0021 ** 
## prop_type_simplifiedHouse       -2.49e-02   1.24e-02   -2.02   0.0435 *  
## prop_type_simplifiedLoft        -1.29e-02   1.71e-02   -0.75   0.4518    
## prop_type_simplifiedOther       -4.12e-02   1.55e-02   -2.66   0.0078 ** 
## number_of_reviews                2.84e-04   8.82e-05    3.22   0.0013 ** 
## review_scores_rating             3.39e-03   4.64e-04    7.32  2.6e-13 ***
## room_typePrivate room            2.29e-01   3.50e-02    6.55  5.9e-11 ***
## room_typeEntire home/apt         9.45e-01   3.53e-02   26.79  < 2e-16 ***
## room_typeHotel room              8.13e-01   5.16e-02   15.76  < 2e-16 ***
## bedrooms                         8.14e-02   4.14e-03   19.67  < 2e-16 ***
## 
## Residual standard error: 0.485 on 15026 degrees of freedom
##   (4858 observations deleted due to missingness)
## Multiple R-squared:  0.402,	Adjusted R-squared:  0.401 
## F-statistic: 1.01e+03 on 10 and 15026 DF,  p-value: <2e-16
```

```r
    autoplot(model3)
```

<img src="index_files/figure-html/unnamed-chunk-25-1.png" width="672" />

```r
    car::vif(model3)
```

```
##                      GVIF Df GVIF^(1/(2*Df))
## prop_type_simplified 1.42  4            1.05
## number_of_reviews    1.06  1            1.03
## review_scores_rating 1.01  1            1.00
## room_type            1.53  3            1.07
## bedrooms             1.10  1            1.05
```
**After removing outliers**      
      
Comparing the diagnostics of model 3 and 4, we can clearly see the positive effect of removing the outliers. Additionally, it increases *R-squared by 1% to 41%*.      
     
Moreover, to answer the question, yes the number of bedrooms is a significant predictor.


```r
# removing outlier
normal_prices <- normal_prices %>% 
  filter(bedrooms < 6)

# adding bedrooms variable (with removing outliers)
model4 <- lm(log_price_4_nights ~ prop_type_simplified + number_of_reviews + review_scores_rating + room_type  + bedrooms, data=normal_prices)
    msummary(model4)
```

```
##                                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                      6.91e+00   5.56e-02  124.18  < 2e-16 ***
## prop_type_simplifiedCondominium  4.35e-02   1.53e-02    2.85  0.00440 ** 
## prop_type_simplifiedHouse       -2.95e-02   1.23e-02   -2.40  0.01645 *  
## prop_type_simplifiedLoft         4.01e-02   1.73e-02    2.31  0.02077 *  
## prop_type_simplifiedOther       -3.36e-02   1.54e-02   -2.18  0.02951 *  
## number_of_reviews                3.01e-04   8.75e-05    3.44  0.00059 ***
## review_scores_rating             3.39e-03   4.62e-04    7.34  2.2e-13 ***
## room_typePrivate room            2.30e-01   3.47e-02    6.63  3.4e-11 ***
## room_typeEntire home/apt         8.95e-01   3.51e-02   25.49  < 2e-16 ***
## room_typeHotel room              8.18e-01   5.13e-02   15.96  < 2e-16 ***
## bedrooms                         1.62e-01   6.73e-03   24.13  < 2e-16 ***
## 
## Residual standard error: 0.48 on 14981 degrees of freedom
##   (4786 observations deleted due to missingness)
## Multiple R-squared:  0.411,	Adjusted R-squared:  0.411 
## F-statistic: 1.05e+03 on 10 and 14981 DF,  p-value: <2e-16
```

```r
    autoplot(model4)
```

<img src="index_files/figure-html/unnamed-chunk-26-1.png" width="672" />

```r
    car::vif(model4)
```

```
##                      GVIF Df GVIF^(1/(2*Df))
## prop_type_simplified 1.49  4            1.05
## number_of_reviews    1.06  1            1.03
## review_scores_rating 1.01  1            1.00
## room_type            1.74  3            1.10
## bedrooms             1.30  1            1.14
```


**After controlling for other variables, is a listing’s exact location a significant predictor of price_4_nights**     
      
ANSWER: **Yes**, the p-value indicates that the exact location is a *significant predictor*. However, it does not increase the explanation power (R-squared) a lot.


```r
# adding bedrooms variable (with removing outliers)
model5 <- lm(log_price_4_nights ~ prop_type_simplified + number_of_reviews + review_scores_rating + room_type  + bedrooms + is_location_exact, data=normal_prices)
    msummary(model5)
```

```
##                                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                      6.85e+00   5.63e-02  121.59  < 2e-16 ***
## prop_type_simplifiedCondominium  4.35e-02   1.52e-02    2.85  0.00437 ** 
## prop_type_simplifiedHouse       -2.67e-02   1.23e-02   -2.17  0.02981 *  
## prop_type_simplifiedLoft         4.00e-02   1.73e-02    2.31  0.02083 *  
## prop_type_simplifiedOther       -3.12e-02   1.54e-02   -2.02  0.04326 *  
## number_of_reviews                3.01e-04   8.73e-05    3.44  0.00058 ***
## review_scores_rating             3.35e-03   4.61e-04    7.27  3.8e-13 ***
## room_typePrivate room            2.35e-01   3.47e-02    6.79  1.2e-11 ***
## room_typeEntire home/apt         8.97e-01   3.50e-02   25.60  < 2e-16 ***
## room_typeHotel room              8.22e-01   5.12e-02   16.06  < 2e-16 ***
## bedrooms                         1.63e-01   6.72e-03   24.25  < 2e-16 ***
## is_location_exactTRUE            7.30e-02   1.07e-02    6.80  1.1e-11 ***
## 
## Residual standard error: 0.48 on 14980 degrees of freedom
##   (4786 observations deleted due to missingness)
## Multiple R-squared:  0.413,	Adjusted R-squared:  0.412 
## F-statistic:  958 on 11 and 14980 DF,  p-value: <2e-16
```

```r
    autoplot(model5)
```

<img src="index_files/figure-html/unnamed-chunk-27-1.png" width="672" />

```r
    car::vif(model5)
```

```
##                      GVIF Df GVIF^(1/(2*Df))
## prop_type_simplified 1.49  4            1.05
## number_of_reviews    1.06  1            1.03
## review_scores_rating 1.01  1            1.00
## room_type            1.74  3            1.10
## bedrooms             1.30  1            1.14
## is_location_exact    1.00  1            1.00
```

**What is the effect of cancellation_policy on price_4_nights, after we control for other variables?**      
      
ANSWER: Depending on the level of cancellation policy, it provides a significant predictor. The fact that a listing has a moderate cancellation policy makes it 12% more expensive compared to a listing with a flexible cancellation policy. Looking at the scale location of this model raises a few questions regarding the standardized residuals.


```r
# create model considering cancellation policy
model6 <- lm(log_price_4_nights ~ prop_type_simplified + number_of_reviews + review_scores_rating + room_type  + bedrooms + is_location_exact + cancellation_policy, data=normal_prices)
    msummary(model6)
```

```
##                                                 Estimate Std. Error t value
## (Intercept)                                     6.82e+00   5.61e-02  121.49
## prop_type_simplifiedCondominium                 4.42e-02   1.52e-02    2.91
## prop_type_simplifiedHouse                      -2.68e-02   1.22e-02   -2.19
## prop_type_simplifiedLoft                        4.52e-02   1.72e-02    2.62
## prop_type_simplifiedOther                      -2.36e-02   1.54e-02   -1.54
## number_of_reviews                               1.55e-04   8.79e-05    1.77
## review_scores_rating                            3.24e-03   4.60e-04    7.05
## room_typePrivate room                           2.39e-01   3.45e-02    6.93
## room_typeEntire home/apt                        8.88e-01   3.49e-02   25.42
## room_typeHotel room                             8.18e-01   5.11e-02   16.01
## bedrooms                                        1.62e-01   6.69e-03   24.15
## is_location_exactTRUE                           7.12e-02   1.07e-02    6.66
## cancellation_policymoderate                     6.68e-02   9.14e-03    7.31
## cancellation_policystrict_14_with_grace_period  1.15e-01   1.04e-02   11.06
## cancellation_policysuper_strict_60              3.06e-01   4.79e-01    0.64
##                                                Pr(>|t|)    
## (Intercept)                                     < 2e-16 ***
## prop_type_simplifiedCondominium                  0.0036 ** 
## prop_type_simplifiedHouse                        0.0283 *  
## prop_type_simplifiedLoft                         0.0088 ** 
## prop_type_simplifiedOther                        0.1247    
## number_of_reviews                                0.0772 .  
## review_scores_rating                            1.8e-12 ***
## room_typePrivate room                           4.3e-12 ***
## room_typeEntire home/apt                        < 2e-16 ***
## room_typeHotel room                             < 2e-16 ***
## bedrooms                                        < 2e-16 ***
## is_location_exactTRUE                           2.8e-11 ***
## cancellation_policymoderate                     2.9e-13 ***
## cancellation_policystrict_14_with_grace_period  < 2e-16 ***
## cancellation_policysuper_strict_60               0.5234    
## 
## Residual standard error: 0.477 on 14977 degrees of freedom
##   (4786 observations deleted due to missingness)
## Multiple R-squared:  0.418,	Adjusted R-squared:  0.417 
## F-statistic:  768 on 14 and 14977 DF,  p-value: <2e-16
```

```r
    autoplot(model6)
```

<img src="index_files/figure-html/unnamed-chunk-28-1.png" width="672" />

```r
    car::vif(model6)
```

```
##                      GVIF Df GVIF^(1/(2*Df))
## prop_type_simplified 1.50  4            1.05
## number_of_reviews    1.09  1            1.04
## review_scores_rating 1.01  1            1.01
## room_type            1.77  3            1.10
## bedrooms             1.30  1            1.14
## is_location_exact    1.01  1            1.00
## cancellation_policy  1.07  3            1.01
```

```r
# anit-log
(exp(1.13e-01)-1)*100
```

```
## [1] 12
```

**Let's compare all the models to see which one to take for the final prediction**

Comparing the 6 models we have created, we can see that we continuously improved the explanation power (r-squared. Therefore, we are going to continue in our final prediction with model6.


```r
# use huxtable to compare models
huxtable::huxreg(model1,model2, model3, model4, model5, model6)
```

preserve3c6e245f3f35cca5



## Best model for final prediciton

**Given information:**     
Find Airbnb’s that are apartment with a private room, have at least 10 reviews, and an average rating of at least 90. Use your best model to predict the total cost to stay at this Airbnb for 4 nights. Include the appropriate 95% interval with your prediction. Report the point prediction and interval in terms of price_4_nights.


**ANSWER:**        
For a private room in Mexico city with a minimum rating of 90 and at least 10 reviews, the model predicts a **95% price CI of MXN 2057 to MXN 2168**. This corresponds to EUR 82 to EUR 87. Making a sanity check with the actual listings right now, it becomes clear that this price is a quite decent approximation. 


```r
# select data based on given restrictions
final_prediction_data <- normal_prices %>% 
  filter(room_type == "Private room",
         review_scores_rating >= 90,
         number_of_reviews >= 10)

# Make final price predictions and give the corresponding 95% CI
prediction_log_price_4_nights <- predict(model6, newdata = final_prediction_data, interval = "confidence") %>% 
  exp() %>% 
  data.frame() %>%
  summarize(lower_bound = mean(lwr),
            predicted_price = mean(fit),
            upper_bound = mean(upr))
  
prediction_log_price_4_nights %>%
  kbl(col.names = c("Lower Bound (MXN)", "Prediction (MXN)", "Upper Bound (MXN)")) %>%
  kable_classic("striped") %>%
  kable_styling(fixed_thead = T)
```

<table class=" lightable-classic lightable-striped table" style='font-family: "Arial Narrow", "Source Sans Pro", sans-serif; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;'>
 <thead>
  <tr>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Lower Bound (MXN) </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Prediction (MXN) </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> Upper Bound (MXN) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 2048 </td>
   <td style="text-align:right;"> 2100 </td>
   <td style="text-align:right;"> 2153 </td>
  </tr>
</tbody>
</table>

# Details

- Who did you collaborate with: Muhammad Nauman Alam Khan, Tom Invernizzi, Rayna Zhang, Jerome Billiet, Christopher Baumann
- Approximately how much time did you spend on this problem set: Approx. 15h
- What, if anything, gave you the most trouble: --
