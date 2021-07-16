# 用户输入学习

## 说明
这是laravel用户输入的学习记录  

## 用户输入说明
作为一个服务提供者，自然需要获取用户的需求进行服务，而用户的需求由用户的输入所展现，所以用户输入是获取用户的需求并处理的第一步  

## 引入Input包
1. 在laravel6.x之前的写法
    1. 直接引入
        ```php
        use Illuminate\Support\Facades\Input;
        ```
    2. 在`./config/app.php`定义别名并引入
        ```php
        'aliases' => [
            'Input' => Illuminate\Support\Facades\Input::class,
        ]
        ```
        并在控制器中引入
        ```php
        use Input;
        ```
2. 在laravel6.x之后的写法
    1. 不用引入，使用Request类
        ```php
        use Illuminate\Http\Request;
        ```
        并在方法中动态调用Request
        ```php
        // 或在函数中传入
        public function get(Request $request) {
            return $request->input('id');
        }
        // 不能使用以下方式，此时Resquest为new的，并无数据
        $request = new Resquest;
        $request->input('id');
        ```

## 用户输入书写方式 [文档页面](https://learnku.com/docs/laravel/8.x/requests/9369#d56a10)
1. get方式
    ```php
    // 获取指定key
    // 第一个参数是key， 第二个值是默认值
    // 无默认值
    $res = $request->get('id');
    dd($res); // null
    // 有默认值
    $res = $request->get('id', 0);
    dd($res); // 0
    ```
2. all方式
    ```php
    // 获取所有key
    $res = $request->all();
    dd($res);
    /* 
    array:3 [
    "id" => "2"
    "age" => "20"
    "name" => "hedykan"
    ]
    */
    ```
3. except方式
    ```php
    // 获取除id外所有key
    $res = $request->except('id');
    dd($res);
    /* 
    array:3 [
    "age" => "20"
    "name" => "hedykan"
    ]
    */
    ```
4. only方式
    ```php
    $res = $request->only(['username', 'password']);
    dd($res);
    ```
5. hes方法
    ```php
    // 判断key是否存在
    $res = $request->has('id');
    dd($res); // true or false
    ```