# 코드정리

**1. 토지 자동평가모형 :**  공시지가를 이용한 지가 추정을 위하여 격차율 정보 추출
   - **tool and library** 
     - R(dplyr, stringr, data.table, jsonlite, httr, rvest, mise, arrow, readxl, writexl)
     - python(polars, pands, numpy, re, pyarrow)
   - **data source**
     - 공시지가 데이터, 건축물대장데이터, 거래사례스크래핑데이터 
   - **method**
     - MAPE, MEDIAN, 좌표간 거리 계산, 도로조건 및 면적조건 등
   - [**sample code**](https://gist.github.com/hanjongseon/e128c4279c883895fd5b43bb91f50d30)

**2. 경매데이터 매칭(매월 웹데이터 작업시)**   
   - 정규식 정리 및 모집단 데이터 매칭   
   - 경매낙찰횟수는 0.8 지수로 역산하여 계산

3. 예상낙찰가 생성(매월 웹데이터 작업시)      
   - 기간별 낙찰가율 정해서 가중평균   
   - <mark>write_sql해도 되지만, oracle에서 작동안함</mark>
     
     ```r
     #가능하면 아래코드 사용할 것
       batch <- apply(DATA, 1, FUN = function(x) paste0("'",trimws(x), "'", collapse = ",")) %>% 
         paste0("SELECT ",., " FROM DUAL UNION ALL", collapse = " ")
       batch = str_sub(batch, 1, -11)
       query <- paste("INSERT INTO XX", "\n", batch)
       query <- query %>% str_replace_all("'NULL'", " NULL")
       query <- query %>% str_replace_all("'NA'", " NA")
       dbSendQuery(mydb,   query)
     ```

4. 토지특성 정리(매월 웹데이터 작업시)      
   - 국가공간정보포털 - 오픈마켓에 원본데이터   
   - 2016년 데이터를 사용하는 이유는 텍스트형식의 칼럼정보때문     
     - SPCFC:용도지구     
     - RSTA_UBPLFC1:도시계획시설 저촉     
     - CFLT_RT1:도시계획시설 저촉률     
     - HARM_WAST:유해시설과의 거리     
     - GEO_AZI:방위     
     - RSTA_ETC:기타제한구역     
     - RSTA_AREA_RATE:기타제한구역면적비율     
     - ROAD_DIST :도로와의 거리     
     - HARM_RAIL : 철도, 고속국도와의 거리

5. KB 시세 스크래핑(매주 금요일 기점으로 월요일에 스크래핑 진행 및 완료)
   - 5,6,7,8번은 공공데이터, 공시알리미, KB, 네이버매물 데이터를 기본으로 함
   - 기본적인 스크래핑은 각각 컴퓨터에서 자동으로 진행되며 스케줄러로 통합작업은 본컴퓨터로 진행

6. NAVER 상가 매물 스크래핑(20일)
   - 상가매물을 자동으로 수집되며 매월 20일 본컴퓨터에서 통합작업 진행하여 공유드라이브에 저장함

7. 토지, 상가, 공장, 일반주택 스크랩(기본적으로 말일, 상가 제외 분기별)
   - 분기별 데이터 기준임. 상가를 제외한 다른 데이터는 분기별로 통합실행
   - 상가는 매월 실행

8. NAVER 집합상가, 지산 매물 스크래핑(약 22일 진행, 27일에 통합작업)
   - 허가키가 필요함(매월 진행전에 각각 컴퓨터에 키를 할당하여야 함)

9. 공시가격 매칭
   - 매년 진행하는 사항임. 공시알리미에서 코드를 수집하고, 코드별 텍스트 오름차순 정렬 기준으로 매칭함. 그러나 기본적인 동층호 텍스트로 대부분이 매칭 가능함.

10. OK저축은행(월말 월초)
    - 경매결과 데이터 및 KB시세 전달

11. 아프로파이낸셜(요청시와 모집단웹데이터 업로드시)
    - 경매결과 데이터 및 KB시세 전달

12. 모집단정리
    - 현상황에서 자동화 단계 이상의 작업 불필요

13. 스케줄러(window)

14. polars(python)
    - MAP TEST

15. 캄보디아
    - ㅅㄷㄴㅅ
      - 
        - ㅅㄷㄴㅅ
        - ㅅㄷㄴㅅ
        - ㅅㄷㄴㅅ
      - ㅅㅅㄷㄴㅅ2
        - 





