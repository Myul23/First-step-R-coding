# 빅데이터 분석의 첫걸음 R코딩

- Author: 장용식, 최진호
- Book: <https://book.naver.com/bookdb/book_detail.nhn?bid=16324211>
- coding은 example들을 제외하고는 programming으로 넘겼습니다.

---

### Big-data

_3V: volume, velocity, variety_

- 인공지능(AI) = 전문가시스템 + 퍼지시스템 + 유전알고리즘 + 기계학습
- 생성적 적대 신경망(GAN, Generative Adversarial Network): 예술 분야의 창작 활동 등에 활용할
  수 있는 이미지 생성 기술

---

## 3\. 데이터 구조의 이해

| 데이터 유형   | 설명                                                                            |
| ------------- | ------------------------------------------------------------------------------- |
| 벡터          | 동일한 데이터 유형(숫자 또는 문자 등)의 단일 값들이 일차원적으로 구성된 것이다. |
| 요인          | 문자 벡터에 데이터의 레벨이 추가된 데이터 구조                                  |
| 배열          | 동일한 데이터 유형을 갖는 1차원 이상의 데이터 구조                              |
| 리스트        | 각 원소들이 이름과 서로 다른 데이터 유형을 가질 수 있다.                        |
| 데이터 프레임 | 2차원 테이블 형태로, 각 열의 데이터는 동일한 데이터 유형이다.                   |

```r
score <- 70
score
```

    ## [1] 70

```r
score <- c(70, 85, 90)
score
```

    ## [1] 70 85 90

> c( ): concatenate의 약자로 벡터 생성 함수

- assign(“x”, c(70,80,90))보단 x \<- c(70, 85, 90)을 권장한다.

<!-- end list -->

```r
score[4] <- 100; score[3] <-95
score
```

    ## [1]  70  85  95 100

```r
name <- c("알라딘", "자스민", "지니")
name
```

    ## [1] "알라딘" "자스민" "지니"

### 식별자 (identifer)

- 변수 또는 함수 등을 다른 것들과 구별하기 위해 사용하는 이름들을 지칭하는 용어.
- 일련의 문자, 숫자, ‘.’, ‘\_’으로 구성된다. 단,’\_‘이나 숫자 +’.’으로 시작하면 안 된다.
- R에서 정의되어 있는 예약어도 사용할 수 없다.

<!-- end list -->

```r
x <- seq(1, 10, by = 3)
x
```

    ## [1]  1  4  7 10

```r
x <- 1:10
x
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10

```r
x <- 10:1
x
```

    ##  [1] 10  9  8  7  6  5  4  3  2  1

```r
x <- seq(1, 10, length.out = 5)
x
```

    ## [1]  1.00  3.25  5.50  7.75 10.00

```r
x <- c(1,2,3)
y <- rep(x, times = 2)
y
```

    ## [1] 1 2 3 1 2 3

```r
y <- rep(x, each = 2)
y
```

    ## [1] 1 1 2 2 3 3

### 연산자

| 산술 연산자 |   기호    |   예시   |   결과   |
| :---------: | :-------: | :------: | :------: |
|   더하기    |    \+     |  2 + 3   |    5     |
|    빼기     |    \-     |  10 - 3  |    7     |
|   곱하기    |    \*     |   3\*4   |    12    |
|   나누기    |     /     |  4 / 3   | 1.333333 |
|  거듭제곱   | ^ or \*\* |   2^3    |    8     |
|   나머지    |    %%     | 10 %% 3  |    1     |
|     몫      |    %/%    | 10 %/% 3 |    3     |

```r
x <- c(10, 20, 30, 40)
y <- c(2, 4, 6, 8)
z <- c(2, 4)
x + 5
```

    ## [1] 15 25 35 45

```r
x + y
```

    ## [1] 12 24 36 48

```r
x + z
```

    ## [1] 12 24 32 44

| 비교 연산자 | 기호  |    예    | 결과  |
| :---------: | :---: | :------: | :---: |
|    작음     |  \<   | 3 \< 10  | TRUE  |
|    이하     |  \<=  | 3 \<= 10 | TRUE  |
|     큼      |  \>   | 3 \> 10  | FALSE |
|    이상     |  \>=  | 3 \>= 10 | FALSE |
|    같음     |  \==  | 3 == 10  | FALSE |
|  같지 않음  |  \!=  | 3 \!= 10 | TRUE  |

```r
3 < 10
```

    ## [1] TRUE

```r
x <- c(10, 20, 30)
x <= 10
```

    ## [1]  TRUE FALSE FALSE

```r
x[x > 15]
```

    ## [1] 20 30

```r
x <- c(10, 20, 30)
any(x <= 10)
```

    ## [1] TRUE

```r
all(x <= 10)
```

    ## [1] FALSE

| 논리 연산자 |   기호    |      예       |      결과       |
| :---------: | :-------: | :-----------: | :-------------: |
|   논리합    |    \|     | TRUE \| FALSE |      TRUE       |
|   논리곱    |     &     | TRUE & FALSE  |      FALSE      |
|  논리부정   |    \!     | \!0<br />\!2  | FALSE<br />TRUE |
|  진위여부   | isTRUE( ) | isTRUE(FALSE) |      FALSE      |

```r
x <- c(TRUE, TRUE, FALSE, FALSE)
y <- c(TRUE, FALSE, TRUE, FALSE)
x & y
```

    ## [1]  TRUE FALSE FALSE FALSE

```r
x | y
```

    ## [1]  TRUE  TRUE  TRUE FALSE

```r
xor(x, y)
```

    ## [1] FALSE  TRUE  TRUE FALSE

> xor( ): exclusive or

| 특수값 | 기호  |                                                                   |
| :----: | :---: | ----------------------------------------------------------------- |
| 결측치 |  NA   | 데이터 누락                                                       |
|   널   | NULL  | 변수 이름만 있는 경우                                             |
| (불능) |  Inf  | 0이 아닌 수를 0으로 나눈 경우, 수렴하지 않아 값을 특정할 수 없다. |
| (부정) |  NaN  | Not a number, 0을 0으로 나눈 경우와 같이 계산할 수 없는 경우      |

```r
x <- NULL
is.null(x)
```

    ## [1] TRUE

```r
y <- c(1, 2, 3, NA, 5)
y
```

    ## [1]  1  2  3 NA  5

```r
z <- 10/0
z
```

    ## [1] Inf

```r
u <- 0/0
u
```

    ## [1] NaN

### 요인, factor

```r
gender <- c('남', '여', '남')
gender
```

    ## [1] "남" "여" "남"

```r
gender.factor <- factor(gender)
gender.factor
```

    ## [1] 남 여 남
    ## Levels: 남 여

### 배열과 행렬

```r
x <- c(70, 80, 95)
arr <- array(x)
arr
```

    ## [1] 70 80 95

```r
z <- 1:6
arr <- array(z, dim = c(2,3))
arr
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

```r
name <- list(c("국어", "음악"), c("알라딘", "자스민", "지니"))
score <- c(70, 80, 85, 90, 90, 75)
arr <- array(score, dim = c(2,3), dimnames = name)
arr
```

    ##      알라딘 자스민 지니
    ## 국어     70     85   90
    ## 음악     80     90   75

```r
arr[1,]
```

    ## 알라딘 자스민   지니
    ##     70     85     90

```r
arr[, 2]
```

    ## 국어 음악
    ##   85   90

```r
name <- list(c("1행", "2행"), c("1열", "2열", "3열"))
x <- 1:6
mtx <- matrix(x, nrow = 2)
mtx
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

```r
mtx <- matrix(x, nrow = 2, dimnames = name, byrow = T)
mtx
```

    ##     1열 2열 3열
    ## 1행   1   2   3
    ## 2행   4   5   6

```r
y <- c(7, 8, 9)
mtx <- rbind(mtx, y)
rownames(mtx)[3] <- "3행"
mtx
```

    ##     1열 2열 3열
    ## 1행   1   2   3
    ## 2행   4   5   6
    ## 3행   7   8   9

```r
z <- c(10, 11, 12)
mtx <- cbind(mtx, 2)
colnames(mtx)[4] = "4열"
mtx
```

    ##     1열 2열 3열 4열
    ## 1행   1   2   3   2
    ## 2행   4   5   6   2
    ## 3행   7   8   9   2

### 리스트

```r
x <- list("알라딘", 20, c(70, 80))
x
```

    ## [[1]]
    ## [1] "알라딘"
    ##
    ## [[2]]
    ## [1] 20
    ##
    ## [[3]]
    ## [1] 70 80

```r
x[1]
```

    ## [[1]]
    ## [1] "알라딘"

```r
x[[1]]
```

    ## [1] "알라딘"

```r
x <- list(성명 = "알라딘", 나이 = 20, 성적 = c(70, 80))
x
```

    ## $성명
    ## [1] "알라딘"
    ##
    ## $나이
    ## [1] 20
    ##
    ## $성적
    ## [1] 70 80

```r
x[1]
```

    ## $성명
    ## [1] "알라딘"

```r
x[[1]]
```

    ## [1] "알라딘"

### 데이터 프레임

```r
df <- data.frame(성명 = c("알라딘", "자스민"), 나이 = c(20, 19), 국어 = c(70, 85))
df <- cbind(df, 음악 = c(85, 90))
df
```

    ##     성명 나이 국어 음악
    ## 1 알라딘   20   70   85
    ## 2 자스민   19   85   90

```r
df <- rbind(df, data.frame(성명 = "지니", 나이 = 30, 국어 = 90, 음악 = 75))
df
```

    ##     성명 나이 국어 음악
    ## 1 알라딘   20   70   85
    ## 2 자스민   19   85   90
    ## 3   지니   30   90   75

```r
df[3, 2]
```

    ## [1] 30

```r
df[3,]
```

    ##   성명 나이 국어 음악
    ## 3 지니   30   90   75

```r
df[, 2]
```

    ## [1] 20 19 30

```r
df[-2,]
```

    ##     성명 나이 국어 음악
    ## 1 알라딘   20   70   85
    ## 3   지니   30   90   75

```r
df[, -3]
```

    ##     성명 나이 음악
    ## 1 알라딘   20   85
    ## 2 자스민   19   90
    ## 3   지니   30   75

```r
df <- data.frame(성명 = c("알라딘", "자스민"), 나이 = c(20, 19), 국어 = c(70, 85))
str(df)
```

    ## 'data.frame':    2 obs. of  3 variables:
    ##  $ 성명: Factor w/ 2 levels "알라딘","자스민": 1 2
    ##  $ 나이: num  20 19
    ##  $ 국어: num  70 85

> data.frame(stringsAsFactors = T), 문자형 데이터는 다 범주화 시켜버린다.

```r
df[1, 2] <- 21
df
```

    ##     성명 나이 국어
    ## 1 알라딘   21   70
    ## 2 자스민   19   85

```r
df[1, 1] <- "Aladin"
```

    ## Warning in `[<-.factor`(`*tmp*`, iseq, value = "Aladin"): invalid factor level,
    ## NA generated

- 현재 성명은 범주를 알라딘, 자스민 딱 2개로 갖는 factor여서 수정 불가.

<!-- end list -->

```r
df <- data.frame(성명 = c("알라딘", "자스민"), 나이 = c(20, 19), 국어 = c(70, 85), stringsAsFactors = F)
str(df)
```

    ## 'data.frame':    2 obs. of  3 variables:
    ##  $ 성명: chr  "알라딘" "자스민"
    ##  $ 나이: num  20 19
    ##  $ 국어: num  70 85

```r
df[1, 1] <- "Aladin"
df
```

    ##     성명 나이 국어
    ## 1 Aladin   20   70
    ## 2 자스민   19   85

### 데이터 파일 읽기

```r
data(package = "datasets")
```

```r
## quakes
head(quakes)
```

    ##      lat   long depth mag stations
    ## 1 -20.42 181.62   562 4.8       41
    ## 2 -20.62 181.03   650 4.2       15
    ## 3 -26.00 184.10    42 5.4       43
    ## 4 -17.97 181.66   626 4.1       19
    ## 5 -20.42 181.96   649 4.0       11
    ## 6 -19.68 184.31   195 4.0       12

```r
tail(quakes, n = 10)
```

    ##         lat   long depth mag stations
    ## 991  -20.73 181.42   575 4.3       18
    ## 992  -15.45 181.42   409 4.3       27
    ## 993  -20.05 183.86   243 4.9       65
    ## 994  -17.95 181.37   642 4.0       17
    ## 995  -17.70 188.10    45 4.2       10
    ## 996  -25.93 179.54   470 4.4       22
    ## 997  -12.28 167.06   248 4.7       35
    ## 998  -20.13 184.20   244 4.5       34
    ## 999  -17.40 187.80    40 4.5       14
    ## 1000 -21.59 170.56   165 6.0      119

```r
names(quakes)
```

    ## [1] "lat"      "long"     "depth"    "mag"      "stations"

```r
dim(quakes)
```

    ## [1] 1000    5

```r
str(quakes)
```

    ## 'data.frame':    1000 obs. of  5 variables:
    ##  $ lat     : num  -20.4 -20.6 -26 -18 -20.4 ...
    ##  $ long    : num  182 181 184 182 182 ...
    ##  $ depth   : int  562 650 42 626 649 195 82 194 211 622 ...
    ##  $ mag     : num  4.8 4.2 5.4 4.1 4 4 4.8 4.4 4.7 4.3 ...
    ##  $ stations: int  41 15 43 19 11 12 43 15 35 19 ...

```r
write.table(quakes, "c:/dump/quakes.csv", sep = ',')
df <- read.csv("c:/dump/quakes.csv", header = T)
head(df)
```

### 함수

_반복적인 코딩 시간의 절약, 검증된 코드 사용으로 프로그래밍 효과 up_

```r
getTriangleArea <- function(w, h) {
  area <- w*h / 2
  return(area)
}
```

```r
getTriangleArea(10, 5)
```

    ## [1] 25

---

## 4\. 무조건 해보기

```r
## [한국환경공단](http://www.airkorea.or.kr)
city <- c("서울", "부산", "대구", "인천", "광주", "대전", "울산")
pm25 <- c(18, 21, 21, 17, 8, 11, 25)
lat <- c(37.567933, 35.180002, 35.877052, 37.457730, 35.160331, 36.350573, 35.539865)
lon <- c(126.978054, 129.074974, 128.600680, 126.702194, 126.851433, 127.384793, 1219.311469)
```

X-Y 플로팅 차트로 보는 지역별 미세먼지 현황

```r
plot(lon, lat, pch = 19, cex = pm25*0.3, col = rgb(1, 0, 0, 0.3), xlim = c(126, 130), ylim = c(35, 38),
      xlab = "경도", ylab = "위도")
text(lon, lat, labels = city)
```

<img src="first_files/figure-gfm/unnamed-chunk-24-1.png" style="display: block; margin: auto;" />

> cex: 점의 크기를 결정하는 매개변수, 이 값이 달라서 다른 반지름을 가지게 된 것이다.

워드 클라우드로 보는 지역별 미세먼지 현황

```r
## install.packages("wordcloud")
library(wordcloud)
```

```r
wordcloud(city, freq = pm25, colors = rainbow(3), random.color = T)
```

<img src="first_files/figure-gfm/unnamed-chunk-26-1.png" style="display: block; margin: auto;" />

애니메이션: 바람개비 돌리기

```r
install.packages("imager")
library(imager)

img.path <-  "C://dump/pinwheel.png"
img <- load.image(img.path)
plot(img)
```

```r
img <- resize(img.size_z = 400L, size_y = 400L)
plot(img, xlim = c(0, 400), ylim = c(0, 400))
```

```r
angle <- 0
while(T) {
  angle <- angle + 10
  imrotate(img, angle, cx = 200, cy = 200) %>% plot(axes = F)
  ## pic = imrotate(img, angle, cx = 200, cy = 200); plot(pic, axes = F)
  Sys.sleep(0.2)
}
```

- break 조건이 없어서 Esc를 통해 강제 종료시켜야 한다.

### 웹스크래핑: 공공데이터 포털의 API 목록 출력

```r
install.pakcages("rvest")
library(rvest)
```

```r
url <- "https://www.data.go.kr/tcs/dss/selectDataSetList.do"
html <- read.html(url)
html
```

동전 던지기 시뮬레이션

```r
plot(NA, xlab = "동전을 던진 횟수", ylab = "앞면이 나오는 비율", xlim = c(0, 100), ylim = c(0, 1),
      main = "동전 던지는 횟수에 따른 앞면이 나올 확률 변화")
abline(h = 0.5, col = "red", lty = 2)

count <- 0
for(n in 1:100) {
  coin <- sample(c("앞면", "뒷면"), 1)
  if(coin == "앞면") count = count + 1
  prob <- count / n
  points(n, prob)
  Sys.sleep(1)
}
```

<img src="first_files/figure-gfm/unnamed-chunk-32-1.png" style="display: block; margin: auto;" />
