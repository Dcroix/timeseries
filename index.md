---
title: "Holt Winters Model"
author: 'Data Scientist: Paolo Hilado'
date: "21/09/2021"
output: html_document
---

# SARIMA and Holt Winter's Model in R
### Load data set from a .xlsx file
```{r, warning = FALSE, message = FALSE}
library(xlsx)
library(forecast)
cdf <- read.xlsx("21. Arima Demo Three.xlsx", sheetIndex = "Sheet1")
```

### Check summary of dataset
```{r}
summary(cdf)
```

### Convert data frame to Time Series on a per week basis
### Visualizations - weekly
```{r}
tsdata <- ts(cdf$Customers, start = 1, frequency = 7)
par(bg = "beige")
plot(tsdata, type = "b", lwd = "2", ylab = "Customers Served", xlab = "Weeks", col = "steelblue",
     pch = 19 )
```

### Forecast using Holt Winter's Model of Rbase
```{r}
HWnter <- HoltWinters(tsdata)
```
If you do ?HoltWinters, you can also set parameters such as alpha (weight
on recent observations), beta (dependency of trend slope on recent trend slope), and gamma (weight on most recent seasonal cycle).

### Get Holt Winters Forecast results
```{r}
# Get Forecast Results
HWres <- forecast(HWnter)
DFres <- round(as.data.frame(HWres),0)
rownames(DFres) = NULL 
#Get the predicted values thru DFres
print(DFres)
# Check Accuracy
accuracy(HWres)
# Plot Forecast
par(bg = "beige")
plot(HWres, col = "firebrick", type = "b", pch = 19)
```

### Now let us try a SARIMA model
```{r}
frcst <- auto.arima(tsdata, trace = T)
```

### Get SARIMA Forecast results
```{r}
# Get Forecast Results
SARIMAres <- forecast(frcst)
DFres2 <- round(as.data.frame(SARIMAres,0),0)
rownames(DFres2) = NULL
#Get the predicted values thru DFres
print(DFres2)
# Get the accuracy
accuracy(frcst)
# Plot Forecast
par(bg = "beige")
plot(SARIMAres, col = "darkorchid", type = "b", pch = 19)
```


## Looks like SARIMA (1,0,0)(0,1,0)[7] with MAPE of 3.01% performs better compared to Holt Winters with MAPE of 5.73%
