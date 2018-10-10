 Data Science Application results 

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

### What is the aproximate probability distribution between the test group and the control group?
The relative frequency distribution for each group is:

   - Control group --> 75.2% (44,886 users) 
   - Test group --> 24.8% (14,835 users)
 
 <img src="/IMG.png" height="300" width="300">
 
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

### Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL? 
The frequency table clearly indicates users that CALL instead of doing it on the WEB have a higher percentage "REBILL" level. 

   - Control group --> 92.7%  
   - Test group --> 94.8%
   
 <img src="/q1.1.png" height="175" width="220">

```
inner_merged %>%
  group_by(test_group, transaction_type) %>%
  summarise (n = n()) %>%
  mutate(freq = n / sum(n))
```  


:warning:OPTIONAL:warning:
```
We can also check if the proportion between both groups differs via the Chi-squared test of independence: 

HO:Control = Test & H1:Control =\ Test
  P-Value < 5%, which indicates we can we can rejet H0 at a significance level of 5%. 

chisq.test(inner_merged$test_group, inner_merged$transaction_type) 

We would reject the null hypothesis and conclude that here is statistical difference between them. Furthur analysis could be done such as interval confidence. 
``` 
 
### Is a user that must call-in to cancel more likely to generate more revenues? 
TRUE; 31.5 vs 25.9 
```
inner_merged %>%
  group_by(test_group, transaction_type) %>%
  summarise(avg = mean(transaction_amount)) 
```

TRUE; 28.2 vs 22.2
```
inner_merged %>%
  group_by(test_group) %>%
  summarise(Tot_avg = mean(transaction_amount)) 
```
### Is a user that must call-in more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?
FALSE: 0.0282 vs 0.0178 
```
inner_merged %>%
  group_by(test_group, transaction_type) %>%
  summarise(n = n()) %>%
  filter(transaction_type == "CHARGEBACK" | transaction_type == "REBILL"  ) %>%
  mutate(charRate = n / sum(n)) %>%
  summarise(charRate = charRate[1]/charRate[2])
```
