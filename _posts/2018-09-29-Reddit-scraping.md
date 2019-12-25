---
layout: post
title:  "PRAW를 활용한 Reddit Scrapping"
date:   2018-09-29 01:15:00 +0900
category: Python
tags: [Python, PRAW, Crawling]
---

## 크롤링(Crawling) vs 스크래핑(Scraping)
시작하기에 앞서, 크롤링(Crawling)과 스크래핑(Scraping)를 구분할 필요가 있을 것 같습니다. 둘 다 데이터를 수집한다는 점에서 목적은 같으나, 방법, 수집 대상 등에서 차이가 있습니다.

스크래핑은 수집 대상이 반드시 웹일 필요는 없으며, DB나 Social media 등 적용 범위가 넓다고 합니다. 어렸을 적에 신문기사를 잘라서 스케치북에 붙여서 냈던 NIE(Newspaper in Education)를 기억하시나요? 그 때는 관심 정보를 찾아 일일이 자르고 오려 붙였다면, 이제는 아날로그적 행위를 자동화 해주는 도구 및 API를 사용하여 데이터를 편리하게 추출하고 정리할 수 있지요.

반면 크롤링은 크롤러(봇, Bot)가 웹페이지의 소스를 긁어오는 것을 의미합니다. 긁어온 데이터로부터 원하는 대로 정보를 추출하는 것을 직접 구현해야 하는 것이 장점이자 단점이 될 수도 있겠습니다. 엄청난 양의 데이터를 처리해야 하고 예외처리 등을 고려해본다면 제법 만만치 않은 작업이 될 것 같네요. 요즘 왠만한 서비스들은 API를 제공하고 있으므로, 필요에 따라 잘 선택하면 될 것 같습니다.
                                               

​                       

## PRAW로 스크래핑하기
아래는 PRAW(The Python Reddit API Wrapper)을 사용하여 Reddit에서 본문과 최상위 레벨의 댓글만 읽어서 CSV파일로 저장하는 파이썬 코드입니다. 실제로 동작하기 위해서는 Reddit에서 **Create application** 절차를 밟고 **Client ID**와 **Secret**을 발급받아서 코드에 적용해야 합니다.

```python
import praw
import csv
import time
from datetime import datetime

CLIENT_ID = '????'
CLIENT_SECRET = '????'
USER_AGENT = '????'
SUB_REDDIT = 'HealthAnxiety' # Crawling할 분야
LIMIT = 999  # 수집할 데이터의 양


def scrap_reddit():
    reddit = praw.Reddit(client_id=CLIENT_ID,
                         client_secret=CLIENT_SECRET,
                         user_agent=USER_AGENT)

    print("=======================================")
    print("Scrapping Reddit Submission")
    print("=======================================\n\n")

    scrap_data = []
    total_subreddit = 0

    for submission in reddit.subreddit(SUB_REDDIT).hot(limit=LIMIT):
        total_subreddit += 1
        submission_dict = vars(submission)

        # Title
        print("[Title %03d] : " % (total_subreddit), submission.title)
        writelist = [datetime.utcfromtimestamp(submission.created_utc), 
                     submission.title, 
                     submission_dict['selftext'].rstrip("\n"), 0]

        # Comment
        post = reddit.submission(id=submission_dict['id'])
        total_cmt = 0
        for top_level_comment in post.comments:
            writelist.append(top_level_comment.body.rstrip("\n"))
            total_cmt += 1

        writelist[3] = total_cmt
        scrap_data.append(writelist)
        print('----------------------------------------------')

    return scrap_data, total_subreddit


def write_csv(scrap_data, total_subreddit):
    with open(time.strftime("data_%Y%m%d_%H%M%S.csv", time.localtime()), 
              'wt', encoding='utf-8') as file:
        writer = csv.writer(file, delimiter=",", lineterminator="\n")

        for data in scrap_data:
            writer.writerow(data)


def main():
    scrap_data, total_subreddit = scrap_reddit()
    write_csv(scrap_data, total_subreddit)


if __name__ == "__main__":
    main()
```
​                ​             


### **_Reference_**
- https://www.promptcloud.com/data-scraping-vs-data-crawling
- https://praw.readthedocs.io/en/latest
    ​        
