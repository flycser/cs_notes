# Json 教程

## 概览



## com.alibaba.fastjson

### 如何保持读取与写入文件时, Json对象属性保持原有顺序

```java
// 顺序
jsonArray = (JSONArray) JSON.parse(FileUtils.readFileToString(new File(fileName)), Feature.OrderedField); // Featured.OrderedField
for (int i = 0; i < jsonArray.size(); i++) {
	System.out.println(jsonArray.getJSONObject(i).toJSONString());
  JSONObject jsonObject = jsonArray.getJSONObject(i);
  JSONObject jsonObject2 = new JSONObject(new LinkedHashMap<>()); // LinkedHashMap
  for (String key : jsonObject.keySet()) {
    jsonObject2.put(key, jsonObject.get(key));
  }
  System.out.println(jsonObject2);
}
```

