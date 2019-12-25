---
layout: post
title:  "likert package 활용하기"
date:   2018-10-10 21:08:00 +0900
category: R
tags: [R, Likert]
---

 likert package는 단어 그대로 리커트 척도(Likert scale)를 plot로 표현하기 위해 사용되는 패키지입니다. 리커트 척도를 간단하게 설명하자면, 제시된 문장에 대하여 응답자의 동의/비동의 수준을 측정하는 것인데요. 예를 들어 "나는 일주일 중에서 수요일이 가장 피곤하다"라는 질문에 대해 1) 매우 그렇다 2) 대체로 그렇다 3) 보통이다 4) 대체로 그렇지 않다 5) 전혀 그렇지 않다 와 같이, 5단계 리커트 척도로 표현할 수 있습니다.

여러 논문을 보다보니 R의 likert package가 널리 사용되는 것 같았고, 그려진 plot도 깔끔하고 왠지 쿨해보였어요. 그렇지만 국내 블로그 중에는 likert package를 소개하는 글은 없는 듯 하여 정리해 봅니다. 사용법은 그렇게 어렵지 않습니다.

​       

### [R 코드]

```R
library(xlsx)
library(likert)
library(reshape)

setwd("C:\\workspace")

# Load excel file 
question <- read.xlsx("./result.xlsx", sheetIndex=1, header=TRUE)

# Header of question
question_header <- c(Opt1 = "Cookie",
                     Opt2 = "Macaron",
                     Opt3 = "Jellybean",
                     Opt4 = "Icecream",
                     Opt5 = "Frozen yogurt")

# Refine header
question <- rename(question, question_header)

# Question options
likert_levels <- c("Dislike very much", 
                   "Dislike somewhat", 
                   "Neither Like nor Dislike", 
                   "Like somewhat", 
                   "Like")

# Question options: convert number to text
for(i in seq_along(question)) {
  question[ ,i] <- factor(question[,i], levels=c(1:5), labels=likert_levels)
}

# Get likert object
question_plot <- likert(question)

# Draw plot
likert.bar.plot(question_plot
                #,plot.percent.high=FALSE 
                #,plot.percent.low=FALSE
                #,plot.percent.neutral=FALSE
                ,centered=FALSE
                ) +
  guides(fill=guide_legend(nrow=2, byrow=TRUE)) +
  ggtitle("How much do you like desserts?") +
  theme_classic() +
  theme(legend.title=element_blank(),
        legend.position="bottom",
        legend.direction = "horizontal",
        legend.text=element_text(size=12),
        legend.key.height=unit(0.6,"cm"),
        plot.margin=unit(c(0.1,0.1,0,0),"cm"),
        strip.text = element_text(size=10),
        axis.title.x = element_text(vjust=-0.8,size=8),
        axis.title.y = element_text(vjust=-0.2,size=8),
        axis.text = element_text(size=12),
        axis.text.x = element_text(angle=0,vjust=0.5),
        plot.title=element_text(face="bold", size=20, vjust=2, color="Blue"))
```

​     

### [결과 화면]



![Image](https://dl.dropboxusercontent.com/s/tul4ppxzfgw9ane/Rplot.jpeg){: width="100%" height="100%"}

​            

### _Reference_

- https://cran.r-project.org/web/packages/likert/likert.pdf
- https://ko.wikipedia.org/wiki/리커트_척도

