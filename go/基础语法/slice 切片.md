# 切片
```
package main

import "fmt"

func main(){
	arr := [...] int{ 0 ,1, 2, 3, 4 }
	s1:=arr[ 1 : 4 ]
	fmt.Println("s1:" , s1 ) //获取下标 1开始到 第二个 不包含2
	s2 := arr[1:]
	fmt.Println("s2:" ,s2 )//获取下标 1开始到结尾

	//改变值
	changes(s1)

	fmt.Println( s1 )//改变s1，结果  [1 35 3]

	fmt.Println( "arr:" , arr )//获取整个arr 结果 ： arr: [0 1 35 3 4]
	extend()
}


func changes(arr [] int )  {
	arr[1] =  35
}

func extend(){
	arr := [...] int{ 0 ,1, 2, 3, 4 , 5, 6 , 7  }
	s1 := arr[2:6]
	s2 := s1[3:5] //其中 5 不能超出   cap(s1)
	fmt.Println( "s1:" , s1 )//结果s1: [2 3 4 5]
	fmt.Println( "s2:" , s2 )// s2: [5 6]
	fmt.Println( len(arr) )
	fmt.Println( cap(s1) )
	//总结 slice 可以往后 不能往前
}
```