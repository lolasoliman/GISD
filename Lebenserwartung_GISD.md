---
title: "Scatterplots - Lebenserwartung der Landkreise nach dem GISD"
author: "Marvin Reis"
date: "21 5 2021"
output:
  bookdown::html_document2:
    keep_md: true
    code_folding: hide
    number_sections: false
    fig_caption: true
    theme: cerulean
    highlight: tango
---





```r
Lebenserwartung_dat <- read.csv2("INKAR_Lebenswerwartung_Kreise.csv")

Lebenserwartung_dat <- Lebenserwartung_dat %>% mutate(Kreis = as.numeric(ï..Kennziffer)) %>% select(-ï..Kennziffer)

GISD_data_Kreis <- read.csv("Outfiles/2021_v2/Bund/Kreis/Kreis.csv") %>% filter(Jahr == 2017)

GISD_data_Kreis <- GISD_data_Kreis %>% mutate(Kreis = Kreiskennziffer) %>% select(Kreis, GISD_Score) %>% distinct(Kreis, .keep_all = TRUE) %>% unique()

GISD_Lebenserw_Kreis <- left_join(GISD_data_Kreis, Lebenserwartung_dat, by = "Kreis") %>% mutate(ow = ifelse(Kreis < 11000, 0, 1))
```


```r
#Normalization

GISD_Lebenserw_Kreis$GISD_Score <- (GISD_Lebenserw_Kreis$GISD_Score -min(GISD_Lebenserw_Kreis$GISD_Score ))/(max(GISD_Lebenserw_Kreis$GISD_Score )-min(GISD_Lebenserw_Kreis$GISD_Score ))
```


```r
ggplot(GISD_Lebenserw_Kreis, aes(x = GISD_Score, y = Lebenserwartung)) +
  geom_point(size = 1.5, alpha = 0.5, col = "navy") +
  geom_rug(size = 0.5) + 
  labs(x = "GISD-Score", title = "Lebenserwartung der Landkreise nach dem GISD", subtitle = "im Jahr 2017") +
  theme_rki()
```

![](Lebenserwartung_GISD_files/figure-html/unnamed-chunk-3-1.png)<!-- -->


```r
ggplot(GISD_Lebenserw_Kreis, aes(x = GISD_Score, y = Lebenserwartung)) +
  geom_point(size = 1.5, alpha = 0.5, col = "navy") +
  geom_rug(size = 0.5) +
  geom_smooth(method = loess, col = "red", linetype = "dashed", fill = "grey50", alpha = 0.5) +
  labs(x = "GISD-Score", title = "Lebenserwartung der Landkreise nach dem GISD", subtitle =  "im Jahr 2017, mit Regressionslinie (Loess)",
       y = "Lebenserwartung in Jahren") +
  theme_rki()
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](Lebenserwartung_GISD_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


```r
ggplot(GISD_Lebenserw_Kreis, aes(x = GISD_Score, y = Lebenserwartung)) +
  geom_point(size = 1.5, alpha = 0.5, col = "navy") +
  geom_rug(size = 0.5) +
  geom_smooth(method = lm,col = "red", linetype = "dashed", fill = "grey50", alpha = 0.5) +
  labs(x = "GISD-Score", title = "Lebenserwartung der Landkreise nach dem GISD", subtitle = "im jahr 2017, mit Regressionslinie (Linear)",
       y = "Lebenserwartung in Jahren") +
  theme_rki()
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](Lebenserwartung_GISD_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


```r
ggplot(GISD_Lebenserw_Kreis, aes(x = GISD_Score, y = Lebenserwartung)) +
  geom_point(size = 1, alpha = 0.5) +
  geom_density2d(size = 1, col = "navy", alpha = 0.5) +
  labs(x = "GISD-Score", title = "Lebenserwartung der Landkreise nach dem GISD (Density)", subtitle = "im Jahr 2017",
       y = "Lebenserwartung in Jahren") +
  theme_rki()
```

![](Lebenserwartung_GISD_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


```r
ggplot(GISD_Lebenserw_Kreis, aes(x = GISD_Score, y = Lebenserwartung, col = as.factor(ow))) +
  geom_point(size = 1.5, alpha = 0.5) +
  geom_smooth(method=lm, aes(fill=as.factor(ow)), col = "grey50", linetype = "dashed", fullrange = TRUE) +
  scale_color_rki(name="Ost-/Westdeutschland", labels=c("West", "Ost")) +
  scale_fill_rki(guide=FALSE) +
  geom_rug(size = 0.5, col = "black") + 
  labs(x = "GISD-Score", title = "Lebenserwartung der Landkreise nach dem GISD", subtitle = "im Jahr 2017, nach Ost und West") +
  theme_rki()
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: It is deprecated to specify `guide = FALSE` to remove a guide. Please
## use `guide = "none"` instead.
```

![](Lebenserwartung_GISD_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


```r
ggplot(GISD_Lebenserw_Kreis, aes(x = GISD_Score, y = Lebenserwartung, col = Aggregat)) +
  geom_point(size = 1.5, alpha = 0.5) +
  geom_smooth(method=lm, aes(fill=Aggregat), col = "grey50", linetype = "dashed", fullrange = TRUE) +
  scale_color_rki(name="Stadt/Landkreis", labels=c("kreisfreie Stadt", "Landkreis")) +
  scale_fill_rki(guide=FALSE) +
  geom_rug(size = 0.5, col = "black") + 
  labs(x = "GISD-Score", title = "Lebenserwartung der Landkreise ach dem GISD", subtitle = "im Jahr 2017, nach krsfr. Stadt und Landkreis") +
  theme_rki()
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: It is deprecated to specify `guide = FALSE` to remove a guide. Please
## use `guide = "none"` instead.
```

![](Lebenserwartung_GISD_files/figure-html/unnamed-chunk-8-1.png)<!-- -->