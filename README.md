# go-practice


##### Unmarshal VS Decoder
Decoder会把所有的json读入内存，如果对Read的I/O性能比较敏感，可以考虑先把数据读出到[]byte再解析；如果对内存占用比较敏感，就直接使用decoder接口。json.Encoder会有一个全局的缓存池给不同的Encoder复用。如果要解析大量的json的话用json.Encoder或许会更好。

参考：
[#1](https://stackoverflow.com/questions/21197239/decoding-json-in-golang-using-json-unmarshal-vs-json-newdecoder-decode)
[#2](https://golangtc.com/t/56051db8b09ecc7a4200013a)

