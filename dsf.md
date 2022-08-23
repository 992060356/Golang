// HandleFunc 在 DefaultServeMux 中为给定模式注册处理函数。 ServeMux 的文档解释了模式是如何匹配的。
// HandleFunc（请求路径， 函数类型（表示这个请求处理的事情））

// 从type可以看出 HandlerFunc 就是一个自定义类型，被定义成func(ResponseWriter, *Request)

//这个函数实现了接口type Handler interface {ServeHTTP(ResponseWriter, *Request)}

/*
注册路由：ServeMux 多路路由器	
		muxEntry  具体路由
type ServeMux struct {
	mu    sync.RWMutex	//锁
	m     map[string]muxEntry	//路由集合
	es    []muxEntry 	//从最长到最短排序条目的切片
	hosts bool       //是否有任何，模式包含主机
}
type muxEntry struct {
	h       Handler		//路由处理逻辑
	pattern string		//请求路径
}
*/
func HandleFunc(pattern string, handler func(ResponseWriter, *Request))
{
    //DefaultServeMux是ServerMux一个全局实例，这个实例在被申明时初始化，只要使用了DefaultServeMux这个变量，其实就是同一个指针而已，也就是独一份。
	//调用路由注册，把自定义处理业务的函数进行路由注册，把函数handle func类型转化为HandelerFunc类型

    DefaultServeMux.HandleFunc(pattern, handler)
}
 
// Example
h1 := func(w http.ResponseWriter, _ *http.Request)
{     
    io.WriteString(w, "Hello from a HandleFunc #1!\n") 
} 
h2 := func(w http.ResponseWriter, _ *http.Request) 
{     
    io.WriteString(w, "Hello from a HandleFunc #2!\n") .
}  
http.HandleFunc("/", h1) 
http.HandleFunc("/endpoint", h2)  
log.Fatal(http.ListenAndServe(":8080", nil))
