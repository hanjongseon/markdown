# 코드정리

1. [토지 자동평가모형](./토지_AVM_1.md) 
   - 실거래 기반 공시가격 격차율 예측

2. 경매데이터 매칭   
   - 정규식 정리 및 모집단 데이터 매칭   
   - 경매낙찰횟수는 0.8 지수로 역산하여 계산

3. 예상낙찰가 생성   
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

4. 토지특성 정리   
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

5. KB 시세 스크래핑
   - 

6. NAVER 상가 매물 스크래핑

7. 토지, 상가, 공장, 일반주택 스크랩

8. NAVER 집합상가, 지산 매물 스크래핑

9. 공시가격 매칭

10. OK저축은행

11. 아프로파이낸셜

12. 모집단정리

13. 스케줄러(window)

14. polars(python)
