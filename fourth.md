빅데이터 분석의 첫걸음 R코딩
================

  - Author: 장용식, 최진호
  - Book: <https://book.naver.com/bookdb/book_detail.nhn?bid=16324211>
  - coding은 example들을 제외하고는 programming으로 넘겼습니다.

-----

## 10\. 네이버 오픈 API 활용

### 네이버 OPEN API 기본 사항

1.  API URL, 출력포맷은 XML or JSON 형태가 있다.
2.  요청 변수

| 요청 변수 | 값                           | 설명                                       |
| :-------: | ---------------------------- | ------------------------------------------ |
|   query   | 문자 (필수)                  | 검색을 원하는 질의, UTF-8 인코딩           |
|  display  | 정수 : 기본값 10, 최대값 100 | 검색결과의 출격건수                        |
|   start   | 정수: 기본값 1, 최대 1000    | 검색의 시작위치                            |
|   sort    | 문자: date (default), sim    | 정렬 옵션<br />date(날짜순), sim(유사도순) |

  - “API?요청변수1=값1&요청변수2=값2&…&요청변수n=값n”
  - API와 첫 번째 요청변수 사이에는 ?를, 요청변수 간에는 &를 사용한다.

<!-- end list -->

3.  출력결과 항목

<!-- end list -->

  - NAVER OPEN API 공통만 기술

<table style="text-align: center;">

<tr>

<th colspan="2">

필드

</th>

<th>

값

</th>

<th>

설명

</th>

</tr>

<tr>

<td colspan="2">

rss

</td>

<td>

\-

</td>

<td style="text-align: justify;">

디버그를 쉽게 하고 RSS 리더기만으로 이용할 수 있게 하기 위해 만든 RSS 포맷의 컨테이너

</td>

</tr>

<tr>

<td colspan="2">

channel

</td>

<td>

\-

</td>

<td style="text-align: justify;">

검색 결과를 포함하는 컨테이너 (title, link, description 등의 항목은 참고용으로 무시해도 무방)

</td>

</tr>

<tr>

<td colspan="2">

lastBuildDate

</td>

<td>

datetime

</td>

<td style="text-align: justify;">

검색 결과를 생성한 시간

</td>

</tr>

<tr>

<td colspan="2">

total

</td>

<td>

integer

</td>

<td style="text-align: justify;">

검색 결과 문서의 총 개수

</td>

</tr>

<tr>

<td colspan="2">

start

</td>

<td>

integer

</td>

<td style="text-align: justify;">

검색 결과 문서 중, 문서의 시작점

</td>

</tr>

<tr>

<td colspan="2">

display

</td>

<td>

integer

</td>

<td style="text-align: justify;">

검색된 검색 결과의 개수

</td>

</tr>

<tr>

<td rowspan="5">

item

</td>

<td>

</td>

<td>

\-

</td>

<td style="text-align: justify;">

개발 검색 결과 (아래 3가지와 뉴스 API의 pubDate를 포함)

</td>

</tr>

<tr>

<td>

title

</td>

<td>

string

</td>

<td style="text-align: justify;">

검색 결과 문서의 제목 (검색어와 일치하는 부분은 태그로 감싸져 있다)

</td>

</tr>

<tr>

<td>

link

</td>

<td>

string

</td>

<td style="text-align: justify;">

검색 결과 문서의 제공 네이버 하이퍼텍스트 링크

</td>

</tr>

<tr>

<td>

description

</td>

<td>

string

</td>

<td style="text-align: justify;">

검색 결과 문서의 내용을 요약한 패시지 정보 (패시지에서 검색어와 일치하는 부분은 태그로 감싸져 있다)

</td>

</tr>

</table>

4.  에러 메시지

| 코드 | 에러 메시지                           | 코드 | 에러 메시지                  |
| ---- | ------------------------------------- | ---- | ---------------------------- |
| 000  | 시스템 에러                           |      |                              |
| 010  | 요청 페이지 수가 제한 숫자보다 초과함 | 011  | 부정확한 질의 요청           |
| 020  | 등록되지 않은 키                      | 021  | 키가 일시적으로 사용 불가    |
| 100  | 타당하지 않은 목표 값                 | 101  | 타당하지 않은 검색 결과의 수 |
| 102  | 타당하지 않은 문서의 시작 값          | 110  | 정의되지 않은 정렬 값        |
| 200  | 예약어                                | 900  | 정의되지 않은 에러 발생      |

### 뉴스 검색: 인공지능 키워드

  - API 설정 → URL 작성 → 문서 다운로드 → XML 문서 구조로 변환 → 내용 추출 → 데이터 검색 → 단어 추출 →
    단어 빈도분석 → 워드 클라우드 출력
  - 부여된 API로의 접속 하루 제한을 당연하게도 지키지 못할 것 같아서 실행시키지 않습니다.

<!-- end list -->

``` r
library(RCurl)
library(XML)
library(RmecabKo)
## install_mecab("C:/Rlibs/mecab")
## library(RmecabKo)
```

> install\_mecab( ): RmecabKo 설치 초기에 한국어 사전이 위치할 공간을 정해 다운 받는다.<br />다행히
> RmecabKo의 위치와 같지 않아도 되는 것으로 보인다. 물론 실행하지 않아 알 수 없다.

``` r
searchUrl     <- "https://openapi.naver.com/v1/search/news.xml"
Client_ID     <- "M7..."  ## NAVER API로 받는 것들
Client_Secret <- "HH..."

query <- URLencode(iconv("인공지능", "euc-kr", "UTF-8"))
url <- paste(searchUrl, "?query=", query, "&display=20", sep="")
```

  - 읽을 수 있는 걸로 변환하는 줄 알았는데 UTF-8로 암호화하고?
  - 아.. 내부 설정 변화로 해당 인코딩 방법만의 암호화(인코딩) 방식으로 바뀐다고.

<!-- end list -->

``` r
doc <- getURL(url, httpheader = c("Content-Type" = "application/xml", "X-Naver-Client-id" = Client_ID,
              "X-Naver-Client-Secret" = Client_Secret))
doc
```

  - 현재 application을 통한 API 이용이라 application, xml 형태이므로 xml
  - Naver OPEN API 사용을 위해 2개의 key 제공

<!-- end list -->

``` r
xmlFile <- xmlParse(doc)
df <- xmlToDataFrame(getNodeSet(xmlFile, "//item"))
str(df)
```

#### 뉴스 내용만 가져오기

``` r
description <- df[, 4]
description
```

``` r
description2 <- gsub("\\d|<b>|</b>|&quot;", "", description)
description2
```

  - 정규표현식을 이용한 숫자, \<b /\>, "의 제거

#### 단어 추출

``` r
nouns <- nouns(iconv(description2, "utf-8"))
nouns
```

  - 인코딩 방식을 제거하는 거야, 뭐야

#### 단어 빈도표 만들기

``` r
nouns.all <- unlist(nouns, use.names = F)
nouns.all
```

  - (단어) 벡터로 만들기

<!-- end list -->

``` r
## nouns.all1 <- nouns.all[nchar(nouns.all <= 1)]
nouns.all2 <- nouns.all[nchar(nouns.all >= 2)]
nouns.all2
```

``` r
nouns.freq <- table(nouns.all2)
nouns.freq
```

``` r
nouns.df <- data.frame(nouns.freq, stringsAsFactors = F)
nouns.df
```

#### wordcloud

``` r
nouns.df.sort <- nouns.df[order(-nouns.df$Freq),]
nouns.df.sort
```

  - 빈도에 따라 역순정렬

<!-- end list -->

``` r
wordcloud(nouns.df.sort[, 1], freq = nouns.df.sort[, 2], min.freq = 2, scale = c(3, 0.7), rot.per = 0.25
          random.order = F, random.color = T, colors = rainbow(10))
```

  - frequency table을 빈도 2 이상의 값으로 구성해놓고 min.freq = 1로 설정하는 건 뭐지?

### 연습용 project

네이버 블로그 검색

``` r
library(RCurl)
library(XML)
library(RmecabKo)
## install_mecab("C:/Rlibs/mecab")
## library(RmecabKo)
```

``` r
searchUrl     <- "https://openapi.naver.com/v1/search/blog.xml"
Client_ID     <- "M7..."  ## NAVER API로 받는 것들
Client_Secret <- "HH..."

query <- URLencode(iconv("여행", "euc-kr", "UTF-8"))
url <- paste(searchUrl, "?query=", query, "&display=20", sep="")
```

``` r
doc <- getURL(url, httpheader = c("Content-Type" = "application/xml", "X-Naver-Client-id" = Client_ID,
              "X-Naver-Client-Secret" = Client_Secret))

xmlFile <- xmlParse(doc)
df <- xmlToDataFrame(getNodeSet(xmlFile, "//item"))
str(df)
```

``` r
description <- df[, 4]
description
```

``` r
description2 <- gsub("\\d|<b>|</b>|&quot;", "", description)
description2
```

``` r
nouns <- nouns(iconv(description2, "utf-8"))
nouns <- unlist(nouns, use.names = F)
nouns
```

``` r
nouns <- nouns[nchar(nouns >= 2)]
nouns
```

``` r
nouns.freq <- table(nouns)
nouns.df <- data.frame(nouns.freq, stringsAsFactors = F)
nouns.df
```

``` r
nouns.df.sort <- nouns.df[order(-nouns.df$Freq),]
nouns.df.sort
```

``` r
wordcloud(nouns.df.sort[, 1], freq = nouns.df.sort[, 2], rot.per = 0.25, random.order = F,
          random.color = T, colors = rainbow(10))
```
