# map使用

### map使用

```
package main

import "fmt"

func main() {

	m := map[string]string{ //第一种定义方式
		"name" : "wangjian" ,
		"password" :"test",
	}
	m1 := make(map[string]string) //使用make来定义
	fmt.Println("m:"  , m)
	fmt.Println("m1:" , m1)
	//遍历map
	for k , v := range(m){
		fmt.Printf( "key:%s , values is %s \n"  , k , v )
	}

	//获取map 里面的值

	if n , ok  := m["name"]; ok {
		fmt.Println("n:"  , n , ok )
	}else{
		fmt.Println("不存在相关的值"   )
	}

	//删除值
	fmt.Println("##############删除值################"   )
	delete(m , "name")
	for k , v := range(m){
		fmt.Printf( "key:%s , values is %s \n"  , k , v )
	}


}

```

