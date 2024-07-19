
# 테스트 방법

```
xcrun simctl openurl booted 'hyundaiselection://home/product/1090/591'
```

```
adb shell am start -W -a android.intent.action.VIEW -d hyundaiselection://home/product/1090/591
```




# Deeplink 종류별 필요성에 대한 고민과 구축 방향




# Deep Link



# Universal Links & App link



# Deffered Deeplink



# Dynamic Links



# One Link





```json
{"categoryDisplays": 
 [
	 {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 10, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 18, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 1, "displayProductGroup": [Object], "productDisplayId": 22, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 31, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 45, "showYn": "Y", "usedYn": "Y"}
	 ], 
	 "description": "월단위로 다양한 차종을
자유롭게 구독", "displayNumber": 1, "displayProductCategoryId": 3, "displayProductCategoryName": "Monthly 구독", "hasAnyProducts": "Y", "mainImageFileId": "EvjHASg0S2WQLcNGNutmDQ", "productSalesStrategyCode": "PDSS000001", "serviceId": "zBo2_VHEQNaRWDK_McZYMQ", "showYn": "Y", "transportationAreaGroupId": 1, "usedYn": "Y"}
```

```json
{"displayProductCategoryId": 3, "transportationAreaGroupId": 1}
```


```json
{"categoryDisplays": [{"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 10, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 18, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 1, "displayProductGroup": [Object], "productDisplayId": 22, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 31, "showYn": "Y", "usedYn": "Y"}, {"displayNumber": 0, "displayProductGroup": [Object], "productDisplayId": 45, "showYn": "Y", "usedYn": "Y"}], "description": "월단위로 다양한 차종을
자유롭게 구독", "displayNumber": 1, "displayProductCategoryId": 3, "displayProductCategoryName": "Monthly 구독", "hasAnyProducts": "Y", "mainImageFileId": "EvjHASg0S2WQLcNGNutmDQ", "productSalesStrategyCode": "PDSS000001", "serviceId": "zBo2_VHEQNaRWDK_McZYMQ", "showYn": "Y", "transportationAreaGroupId": 1, "usedYn": "Y"}
```