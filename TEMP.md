# Data Science Application results 

```
library(tidyverse)
testSamples_raw <- read_csv("C:\\Users\\exequ\\Downloads\\datasciencetest\\testSamples.csv", col_names = TRUE)
transData_raw <- read_csv("C:\\Users\\exequ\\Downloads\\datasciencetest\\transData.csv", col_names = TRUE)

testSamples <- unique(testSamples_raw)
transData <- unique(transData_raw)

inner_merged <- inner_join(testSamples, transData, by = "sample_id")

glimpse(inner_merged)
summary(inner_merged)
```

#### What is the aproximate probability distribution between the test group and the control group?
The relative frequency distribution for each group is:
   *Control group --> 75.2% (44,886 users) 
   *Test group -->which is 24.8% (14,835 users)
 
```
testSamples %>%
  group_by(test_group) %>%
  summarise (n = n()) %>%
  mutate(freq = n / sum(n))

ggplot(testSamples, aes(x=test_group)) + 
  geom_bar(aes(y = ..prop.., group = 1)) +
  scale_x_continuous(breaks=0:1, labels=c("Control","Test")) +
  labs(x="Test Group", y="Proportion %")
```

#### Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL? 


#### Is a user that must call-in to cancel more likely to generate more revenues? 


#### Is a user that must call-in more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?

