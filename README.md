## Golang 踩坑记

1. 对 slice 进行 append 操作时，如果 cap 足够，不产生新的 slice

   

2. map 不使用 make 初始化时，是一个空指针，对空指针操作会直接导致 panic

   ``` golang
   var m1 map[string]int   //  == nil
   m2 := map[string]int{}  //  != nil
   m3 := make(map[string]int, 0) //  != nil
   ```

   

3. for i, v := range slice {} 中的v只有一个，每次都会将值拷贝到v里面，所以下列做法是错误的

   ```go
   s := []string {"a", "b", "c"}
   m := make(map[int]*string)
   
   for i, v := range s {
       m[i] = &v   //  正确的做法是 m[i] = &s[i]
   }
   
   fmt.Println(*m[0], *m[1], *m[2])  //  输出的是 "c c c"，因为v
   ```

   

4. make(chan int)  和  make(chan int, 1) 是不一样的

   chan  一旦被写入数据后，当前  goruntine  就会被阻塞，直到有人接收才可以（即 " <- ch"），如果没人接收，它就会一直阻塞着。而如果  chan  带一个缓冲，就会把数据放到缓冲区中，直到缓冲区满了，才会阻塞

   

5. interface 只有在类型和值都为 nil 时才等于 nil

   ```go
   var i interface{}
   var s *string
   
   fmt.Println(s, s == nil) // <nil>, true
   
   fmt.Println(i, i == nil)  // <nil>, true
   i = s
   fmt.Println(i, i == nil)  // <nil>, false
   ```
   
   

5. 以下代码在 Go 中是合法的
   
   ```go
   type Stu struct {}
   func (*Stu) Print() {
       fmt.Println("Hello, world")
   }
   
   func main() {
       var s *Stu  // s == nil
       s.Print()
   }
   ```
   
   
   
   
   
   