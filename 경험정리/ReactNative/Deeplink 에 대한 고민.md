
# 테스트 방법

```
xcrun simctl openurl booted 'hyundaiselection://home/products/100030/100018'
```

```
xcrun simctl openurl booted 'hyundaiselection://home/event/?uri=https%3A%2F%2Fhyundai-selection-event.com%2F0001'


xcrun simctl openurl booted 'hyundaiselection://promotion?eventId=e16deb201c9a4ebdb32b0f'


xcrun simctl openurl booted 'hyundaiselection://home/event/bdf910a99c264572a2332e'

'hyundaiselection://home/event/bdf910a99c264572a2332e'
```



```
adb shell am start -W -a android.intent.action.VIEW -d hyundaiselection://home/product/100030/100018
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