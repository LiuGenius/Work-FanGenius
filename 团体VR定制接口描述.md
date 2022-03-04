## 接口一

### 添加报告
  #### 接口地址：
  
  ```
    public static String Host = "http://192.168.0.119:8080";//IP地址文本文档放在根目录，方便修改
    
    String address = Host + "/equipmentReport/saveEquipmentReportByOtherEqui";
  ```
  
  #### 请求方式：POST(form-data) 表单提交
  #### 请求参数：
  
  ```
    File file;//文件类型
    // TODO: 2022/3/4 文本文档放在根目录，方便修改
    String orgId;//String类型
    String reportName;//String类型
    String otherEquiName;//String类型
  ```
  
  