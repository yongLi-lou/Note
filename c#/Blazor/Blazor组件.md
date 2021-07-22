# Blazor组件

* 组件创建

  ```c#
  //h5部分
  <div><!--code-->@Title
     <img @attributes="Uncatch" > 
    </div>
    
  //c#部分
   @code{
    [Parameter]
      public string Title { get; set; }//单向绑定
    
    [Parameter(CaptureUnmatchedValues = true)]
  
      public Dictionary<string, object> Uncatch { get; set; }//不匹配的其他参数值。自由参数
    
    [CascadingParameter]
      public string color { get; set; }//值来自于父组件通过<CascadingValue Value="@_color">@Body</CascadingValue>
    
  }
    
  ```

  

* 组件的使用

  ```c#
  @using ..//引用组件命名空间或者在_import里全局添加
  
  <Home Title="123" src='https://www.nogizaka46-cn.com/images/index/top-banner2.png?12'></Home>
  ```

  