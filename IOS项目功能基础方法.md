# JSON解析
## Json原生解析
----------------------------------
### NSDictionary的基本使用
-  不可变的,一旦创建,内容就不能添加\删除(不能改动)
- NSDictionary基本使用方法
  - (NSUInteger)count; //返回NSDictionary的键值对数目
  - (id)objectForKey:(id)aKey; //根据key取出value
- NSDictionary遍历方法：
  - 第一步:获取所有的key
  - 第二步:根据key获取value
```objc
  //dict 代表以及创建的字典
  //第一种方法
  for(NSString *key in dict){
     NSLog(@"key = %@,value = %@",key,[dict objectForKey:key]);
  }
  //第二种方法（常用方法） key     value       stop
  [dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
      NSLog(@"%@ --> %@",key,obj);
  }];
```
### NSArray的基本使用
  - 一旦创建成功,内容不可改变 只能存放OC对象
  - NSArray基本使用方法
    - (NSInteger)count; //返回NSArray的键值对数目
    - (id)objectAtIndex:(id)index;//根据下标,获取下标对应的对象
    - (NSInteger)indexOfObject:(NSString)key;//返回元素的下标
    - (BOOL)containsObject:(NSString)key;//数组中是否包含了某个元素
  - NSArray遍历方法：
  ```objc
    //定义一个数组
    NSArray *arr = @[@"one",@"two",@"three",@"four"];
    //对数组进行遍历
    //1) 普通的方式,通过下标访问
    for (int i=0; i<arr.count; i++) {
        NSLog(@"-> %@",arr[i]);
    }
    //2) 快速枚举法 for循环的增强形式
    for (NSString * str in arr) {
         NSLog(@"---> %@",str);
    }
    //3) 使用block的方式,进行访问
    // 数组元素            元素下标     是否停止
    //stop:YES  会停止, stop:NO 不会停止
    [arr enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        if(idx == 2){
            *stop = YES;  //停止  // break;
        }else{
           NSLog(@"idx = %ld,obj = %@",idx,obj);
        } 
    }];
  ```
    
### 思路：当我们要解析JSON时，先把JSON字符串或者在网络上获取NSData转换为一个NSDictionary或者一个NSArray,然后再进行JSON的解析和读取
- 示例代码
```objc
    //此处self._data是网络请求过来的数据或者NSString转换的NSData
    //将JSON字符串转为NSData
    NSData *data = [self._data dataUsingEncoding:NSUTF8StringEncoding];
    //NSJSONSerialization创建NSDictionary
    id obj = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];
    //判断obj是不是NSDictionary类型，如果是NSArray需要修改判断条件
    if([obj isKindOfClass:[NSDictionary class]]){
        //通过NSDictionary objectForKey取出key为 “results” 对应数据  此处是一个NSArray
        id results = [(NSDictionary *) obj objectForKey:@"results"];
        //循环遍历NSArray取出NSDictionary
        for(NSDictionary *dict in results){
            //取出NSArray 中的一个 NSDictionary 中key 为 "_id" 的值
            NSString *_id = [[NSString alloc] initWithString:[dict objectForKey:@"_id"]];
            NSLog(@"_id:   %@",__array);
        }
      }
```




# IOS常用方法
- ## 声明一个对象
```objc
@property(strong,nonatomic) NSString *_data;
```
- ## 创建一个对象
```objc
UITextView *mTv = [[UITextView alloc] init];
```
- ## 把创建的UI组件加到页面上
```java
System.out.println("122");
[self.view addSubview:mTv];
```
- ## 关闭当前界面
```obje
[self dismissViewControllerAnimated:YES completion:nil];
```
- ## 获取屏幕高度和宽度
```objc
CGFloat width = [UIScreen mainScreen].bounds.size.width;//获取屏幕宽度
CGFloat height = [UIScreen mainScreen].bounds.size.height;//获取屏幕高度
```
- ## 为按钮添加点击事件
```objc
[_mBtn addTarget:self action:@selector(click) forControlEvents:UIControlEventTouchUpInside];
```
- ## 跳转界面
```objc
[self presentViewController:view2 animated:YES completion:nil];
```
- ## 弹出一个弹出框（事件实现）
```objc
UIAlertController *alertController = [UIAlertController 
	alertControllerWithTitle:@"标题" message:@"内容" preferredStyle:UIAlertControllerStyleAlert];
	UIAlertAction *cancelAction = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:nil];
	UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
	ViewController2 *view2 = [[ViewController2 alloc] init];
	[self presentViewController:view2 animated:YES completion:nil];
    }];
[alertController addAction:cancelAction];
[alertController addAction:okAction];
[self presentViewController:alertController animated:YES completion:nil];
```
- ## 在原生IOS中调用H5页面中定义的方法
```objc
-(void)stringByEvaluatingJavaScriptFromString:(NSString *) string;
[_webView stringByEvaluatingJavaScriptFromString:[NSString stringWithFormat:@"setUserPhotoImage1('%@')",imageBase64Str]];
```
- ## 给任意一个控件添加点击事件的方法
```objc
UITapGestureRecognizer *tapGesturRecognizer=[[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(click:)];
[tapView addGestureRecognizer:tapGesturRecognizer];
```

# 发送一个网络请求
- 第一步：定义一个NSURL对象
```objc
//定义一个URL
NSString *urlString = @"http://gank.io/api/data/福利/20/1";
urlString = [urlString stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
NSURL *url = [[NSURL alloc]initWithString:urlString];
```
- 第二步：定义一个请求体NSURLRequest
```objc
//定义一个请求体
NSURLRequest *request = [[NSURLRequest alloc] initWithURL:url];
```
- 第三步：发起网络请求并设置代理
```objc
//发起请求
[[NSURLConnection connectionWithRequest:request delegate:self] start];
```

- 如何设置代理？
```objc
//在实现文件（.h）中设置泛型 <NSURLConnectionDelegate>
@interface HttpViewController ()<NSURLConnectionDelegate>

//----------------------------实现代理方法----------------------------
//网络请求的相应结果 
-(void) connection:(NSURLConnection *) connection didReceiveResponse:(nonnull NSURLResponse *)response{
    //response 是此次请求的请求结果响应
    NSLog(@"请求结果响应： %@",response);
}
//接收响应数据 分段接受  网络请求返回的数据在此方法中接收
-(void) connection:(NSURLConnection *)connection didReceiveData:(nonnull NSData *)data{
    //由于didReceiveData是分段调用，所以需要定义一个NSData将每次返回的值拼接在一起才是这个请求返回的值
    [__data appendData:data];
}
//网络请求结束 当网络请求结束 会调用该方法，可以查看定义的NSData查看此次网络请求收到的值
-(void) connectionDidFinishLoading:(NSURLConnection *)connection{
    NSLog(@"网络请求结束");
    NSString *dataString = [[NSString alloc] initWithData:__data encoding:NSUTF8StringEncoding];
    NSLog(@"%@",dataString);  
}
```

