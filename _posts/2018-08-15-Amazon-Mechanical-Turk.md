---
layout: post
title:  "Amazon Mechanical Turk "
date:   2018-08-15 00:00:00 +0900
category: Tool&Service
tags: [Amazon mTurk]
---


연구 과제의 Ground Truth (GT)로 활용하기 위해 Amazon Mechanical Turk (mTurk)를 사용해 보았습니다. 외국 논문을 보면 Amazon mTurk 플랫폼이 널리 사용되고 있었는데, 한국은 mTurk 서비스 제한 국가였기 때문이었는지 활용사례를 찾기 어려웠는데요, 마침 연구를 시작할 시점이었던 2017년 6월에 mTurk에서 requester 등록 국가를 확대함에 따라 한국에서도 사용할 수 있게 되었습니다.

mTurk 사용자 유형은 Human Intelligence Tasks (HITs)를 만들 수 있는 Requester와 HIT과 HIT에 참여하여 작업을 수행할 수 있는 worker로 나눌 수 있습니다.  다만 Worker는 아직까지도 제한이 많은데, Social media에서 찾아본 바에 의하면 미국에 거주하더라도 worker 등록이 거절되는 경우도 꽤 흔하다고 합니다. 

그래도 worker로 등록되기만 하면 HIT에 참여해서 보상을 받을 수 있습니다. 보상 금액은 $0.01부터 $10달러 이상까지 다양합니다.

![Image](https://dl.dropboxusercontent.com/s/v9kapxhs8jpsnbs/00.JPG){: width="100%" height="100%"}
_Fig. 1. Worker가 참여할 수 있는 HIT 목록_

​     

이번 포스팅은 Requester의 입장에서 HIT을 생성하고 진행하는 과정에 대해 소개하려고 합니다.

​     

## Requester - HIT 생성하기

### [HIT 템플릿 선택]

HIT을 만들기 위해서는 Requester로써 가입을 해야 합니다. Create - New Project를 누르면 아래와 같은 화면이 나타납니다. 여러가지 템플릿을 제공하지만, 기능이 제한적이라서 Subvey Link를 선택하고 직접 구축한 사이트를 연결해 보겠습니다.

![Image](https://dl.dropboxusercontent.com/s/c8ed50u5eksbo1z/01.jpg){: width="100%" height="100%"}
_Fig. 2. Project(HIT) 템플릿 선택_

​     

### [HIT 속성 정보 입력]
이제 HIT의 기본 정보(제목, 설명, 키워드), 참여 제한 인원, HIT 만료 기한, 보상 금액, 자동 승인 기한 등을 입력합니다. 재미있는 점은 Worker에 특정한 조건을 걸 수도 있다는 점입니다. 승인된 HIT의 수, 거주지, HIT 승인 비율, 나이, 직업, 자산, 사용 언어, social media 계정 여부 등 제법 디테일합니다. 아쉽게도 대부분의 조건은 유료입니다.

![Image](https://dl.dropboxusercontent.com/s/1c7z39okh4hqviq/02.jpg){: width="100%" height="100%"}
_Fig. 3. Project(HIT) 속성 입력_

​     

### [HIT 화면 레이아웃 구성]

연구에 대한 설명을 작성하고, 직접 구축한 사이트의 링크를 추가하기만 하면 됩니다. survey code는 다음 섹션에서 설명하겠습니다.

![Image](https://dl.dropbox.com/s/ebrjzhh7gu7j6vk/03.jpg){: width="100%" height="100%"}
_Fig. 4. Project (HIT) 화면 구성_


​     
### [사이트 구축 및 적용]

가능하다면 구글 설문지나 서베이 몽키를 활용하는 것이 정신건강에 훨씬 이롭습니다. 저는 어쩔 수 없이 Cafe24 호스팅을 구입해서 사이트를 직접 만들었는데, 아쉽게도 호스팅이 만료되어 캡쳐는 못했습니다. 제가 만든 사이트는 연구용 Survey를 수행해야 되어서, 아래와 같이 구성하였습니다.

① 연구참여동의서 - 연구 목적, 참가 자격, 동의 여부를 확인합니다. 연구에 참여하고자 할 경우, 사용자의 Worker ID를 등록할 수 있도록 했습니다. 참고로 mTurk에 사용자의 이메일, 주소 및 전화번호 등을 수집하는 것은 금지하고 있으며 오직 Worker ID로만 식별 가능합니다. Worker ID는 DB에 저장되어 결과를 맵핑하여 보상 처리 및 중복 참여를 막기 위한 용도로 사용하였습니다. <br/>
② 연구조사 페이지 - 설문조사 및 GT 확보용으로 만들었습니다. 설문조사 결과는 별도 DB에 저장하였습니다. <br/>
③ 연구종료 페이지 - 사용자가 모든 응답을 종료하면, 화면에 랜덤으로 생성한 문자열을 띄웠습니다. 이 문자열을 HIT 화면 내 Survey Code 입력 창에 붙여넣도록 합니다. Survey Link에서 Requester가 mTurk에서 확인할 수 있는 것은 Worker ID와 Survey Code가 맵핑된 엑셀 파일입니다. 엑셀 파일에는 응답 시작/종료 일시, 소요 시간 등의 정보도 포함되어 있습니다. 

​     

### [__Publish Batch + 금액 결제__]
HIT 속성을 최종적으로 확인하고 비용을 결제합니다. 보상금액*참가자 수에 <u>mTurk 사용 요금</u>이 추가됩니다. 이전에 연구를 수행했을 때는 총 520명의 참가자를 모집하고 보상금액은 $1로 책정했더니, 인당 $0.4의 수수료가 붙어서 비용이 상당히 올라갔습니다.  

![Image](https://dl.dropbox.com/s/hd17tgr13i8cqk8/04.jpg){: width="100%" height="100%"}
_Fig. 5. Publish Batch 예시 화면_


​     
### [__HIT 진행 및 결과 확인__]
HIT이 정상적으로 생성되었으면, Worker들이 작업을 수행하기 시작합니다. 생각보다 엄청나게 몰려들어서 HIT을 종료하는 데, 몇 시간 정도면 충분합니다! 

​    

이제 마지막으로 Worker가 성실히 작업했는지 확인하고 승인하는 과정이 남아있습니다. DB에 저장된 Survey Code를 mTurk에서 맵핑해서 Worker ID를 찾아내고 작업을 평가할 수 있습니다. Worker가 불성실하게 작업(응답시간이 너무 짧거나, 한 줄 찍기 등)했다면 보상을 거부할 수 있습니다. 단, Requester가 지정된 기한까지 승인을 하지 않으면 자동 승인 된다. 제가 개설한 HIT은 너무 빨리 끝나서 응답 내용을 하나하나 확인하고 승인하기는 어려웠어요. 결국 전부 승인해 줬습니다.

​     

### [__후기__]
- 언젠가 또 mTurk를 사용할 일이 있을 것 같아, 기억을 짜내어 설명을 상세하게 작성해 보려 했습니다. 나름 흥미로운 경험이었어요.
- Worker ID나 Survey Code에 오타가 종종 발견되어, Mapping 시 가끔 문제가 생겼다. 특히 숫자 1과 l(L)은 단골이었죠. 8과 B도 있었고요.
- Worker로부터 시스템에 관한 불만의 메일을 몇 통 받았습니다 (뒤로가기를 눌러서 중복 참여로 오류가 났거나, 응답 조건에 맞지 않아서 버튼이 비활성화 되는 문제 등). 그들은 돈을 받지 못할까봐 꽤 불안해 했습니다.
- 영어를 잘 하면 훨씬 수월할 것 같습니다.


​     
