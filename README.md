@

# 코드 정리

---

[ㅅㄷㄴㅅ](https://github.com/hanjongseon/CAMBODIA/blob/main/CELL_PRICE_ADD_PROXY.R)



```python
import pandas as pd
import re


```



```
source('d:/work/default.R')

cell_price <- fread("G:\\내 드라이브\\cambodia\\data_store\\CELL_POP_PRICE_20211005.txt")

refer_table <- mongo_find('CAMBODIA_PROXY_ROAD_DISTANCE')

refer_table <- 
  refer_table %>% 
  rename(
    PROXY_ROAD_DISTANCE = MAIN_ROAD_DISTANCE
  )

cell_price <- 
  cell_price %>% 
  left_join(refer_table)

cell_price %>% head()

fwrite(cell_price, "g:/내 드라이브/cambodia/data_store/CELL_POP_PRICE_PROXY_ADDED_20211111.txt", row.names=FALSE, sep= "|")
```




