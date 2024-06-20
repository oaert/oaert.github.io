### go 单元测试gin框架mock请求
[mock请求学习](https://stackoverflow.com/questions/57733801/how-to-set-mock-gin-context-for-bindjson/67034058#67034058)

#### 示例代码
```go
func MockJsonPost(c *gin.Context /* the test context */, content interface{}) {
    c.Request.Method = "POST" // or PUT
    c.Request.Header.Set("Content-Type", "application/json")

    jsonbytes, err := json.Marshal(content)
    if err != nil {
        panic(err)
    }
    
    // the request body must be an io.ReadCloser
    // the bytes buffer though doesn't implement io.Closer,
    // so you wrap it in a no-op closer
    c.Request.Body = io.NopCloser(bytes.NewBuffer(jsonbytes))
}
```
```go
func TestMyHandler(t *testing.T) {
    w := httptest.NewRecorder()
    ctx, _ := gin.CreateTestContext(w) 

    ctx.Request = &http.Request{
        Header: make(http.Header),
    }


    MockJsonPost(ctx, map[string]interface{}{"foo": "bar"})
    
    MyHandler(ctx)
    assert.EqualValues(t, http.StatusOK, w.Code)
} 
```