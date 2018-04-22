# LIME-in-R
Using the LIME (Local Interpretable Model Agnostic explanation ) 
library(caret)
install.packages("lime")
library(lime)
ctg <- read.csv("CTG.csv")  
head(ctg) ; dim(ctg)

ctg$NSP  <-  factor(ctg$NSP)
ctg_train <- ctg[1:2120 ,1:3]   ### out of 2126 rows in ctg i am selecting top 2120 for train and dropping
                                ##      the NSP (Y )column
ctg_lab  <-  ctg[1:2120 , 4]   ##       data frame with only the Y variable( NSP)

ctg_test <-  ctg[2120 :2126 , 1:3]  ## on this i will do prediction, have droped  the NSP column as well

ctg_model <-  train(ctg_train  ,ctg_lab , method = 'rf' )  ## use the train function from the caret pkg 
                                                           #    and build random forest algo

ctg_explain <- lime(ctg_train , ctg_model, bin_continuous = TRUE, quantile_bins = FALSE)
ctg_explanation <-  explain(ctg_test , ctg_explain , n_labels = 1, n_features = 2 , kernel_width = 0.5)

summary(ctg_explanation)

 plot_features(ctg_explanation)  ### you get the plot which gives the explanation behind the prediction
