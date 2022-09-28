 

### 토지 AVM

1. **개별공시지가 매칭 및 실거래 바인딩**
- KAKAO PNU CODE 추출 코드


  ![[current.png]]
  ```r
  KAKAO_LOCATION <-  
    function(KEYWORD){
      require(jsonlite)
      require(httr)
      require(rvest)
      app.key = sample(c('XXX','XXX',"XXX"),1)
      aa = POST('https://dapi.kakao.com/v2/local/search/address.json', add_headers(Authorization = paste0("KakaoAK ",app.key)), 
                query = list(
                  query=KEYWORD
                )
      )
  
      bb = read_html(aa) %>% html_text() %>% fromJSON()
      if(length(bb$documents)==0){
        message("주소를 검색할 수 없습니다.")
        TT2 = NULL
      }else{
        TT = bb$documents
        if(is.na(TT$road_address)){
          KO = 0
        }else{
          KO = 1
        }
        ## JSON 데이터에 x,y 좌표 외에 아래 내용이 포함되어 있음
        TT2 = data.frame(ADDRESS = KEYWORD,
                         SIDO =TT$address$region_1depth_name[1],
                         SGG = TT$address$region_2depth_name[1],
                         EMD = TT$address$region_3depth_name[1],
                         HEMD = TT$address$region_3depth_h_name[1],
                         BUN1 = TT$address$main_address_no[1],
                         BUN2 = ifelse(is.null(TT$address$sub_address_no[1]), "", TT$address$sub_address_no[1]),
                         SAN = TT$address$mountain_yn[1],
                         R_ADDRESS=ifelse(KO==0, NA, TT$road_address$address_name[1]),
                         LON=ifelse(is.na(TT$x), NA, TT$x[1]),
                         LAT=ifelse(is.na(TT$y), NA, TT$y[1]),
                         EMD_CODE = TT$address$b_code[1],
                         HEMD_CODE = TT$address$h_code[1]
        )
      }
      if(length(TT2)==0){
        NULL
      }else{
        if(!is.data.frame(TT2)){
          NULL
        }else{
          TT2$SPNU = ""
          TT2 = TT2 %>% dplyr::mutate(
            SPNU = paste0(EMD_CODE, "-", 
                          ifelse(SAN=="N", "1", "2"),
                          sprintf("%04s", BUN1) %>% str_replace_all(" ", "0"),
                          sprintf("%04s", BUN2) %>% str_replace_all(" ", "0"))
          )
          TT2 %>% head(1)
        }
      }
    }  
  ```

- 표제부 정리
  
  - 집합건물, 일반건물 분리 및 PNU 생성
  
  - 도로명 주소 누락 및 텍스트 비정상인 경우, API 호출로 주소 생성
    
    ```r
    for(i in 1:length(missing_address)){
      try({
        result <- KAKAO_LOCATION(missing_address[i])$R_ADDRESS
        if(!is.null(result)){
          BLD_ILBAN[대지_위치==missing_address[i], 도로명_대지_위치:=result]
        }
      }, silent=T) # try, silent API 같은데서는 이게 편함.
    }
    ```
    
    ```r
    #도로명 주소 패턴 참
    
    regex = "(^| )[\\w[:punct:]]+(대로|로|길|거리|고개)([:space:]{1}|$)"
    ```

- 실거래 정리
  
  - <mark>filter_condition : DPOS_GBN!=C &  JIBUN_GBN==0</mark>
  
  - 면적 정리    
      - 올림, 내림 면적 생성 
      - <mark>매칭과정에서 면적이 완벽하게 일치하기 어려우므로 올림내림 조건을 만족하는 경우로 한함</mark>    
      - 일반 주택의 경우, 번지가 명확하지 않기 때문에 본번의 첫자리 텍스트, 본번의 글자수, 도로명, 건축일자, 면적으로 키값을 만듦    
      - 토지의 경우에는 PNU, 지목명, 용도지역명을 키로 함    
      - 공장은 상가 거래사례를 바인딩

- 현재 저장 파일은 기본은 parquet파일로 하고 있음
  
  - <mark>rds, txt는 느리거나 인코딩 제한문제가 있음</mark>
