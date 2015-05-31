#Converters
These functions convert `v8::Value`s to other `v8::Value` types and native types.
Since type conversion is not guaranteed to succeed, they return `Maybe` types.

## NanTo
Converts a v8::Handle<v8::Value> to a different subtype of v8::Value or to a native
data type. Returns a `NanMaybeLocal<>` or a `NanMaybe<>` accordingly.

```c++
NanMaybeLocal<v8::Boolean> NanTo<>(v8::Handle<v8::Value> val);
NanMaybeLocal<v8::Int32>   NanTo<>(v8::Handle<v8::Value> val);
NanMaybeLocal<v8::Integer> NanTo<>(v8::Handle<v8::Value> val);
NanMaybeLocal<v8::Object>  NanTo<>(v8::Handle<v8::Value> val);
NanMaybeLocal<v8::Number>  NanTo<>(v8::Handle<v8::Value> val);
NanMaybeLocal<v8::String>  NanTo<>(v8::Handle<v8::Value> val);
NanMaybeLocal<v8::Uint32>  NanTo<>(v8::Handle<v8::Value> val);

NanMaybe<bool>             NanTo<>(v8::Handle<v8::Value> val);
NanMaybe<double>           NanTo<>(v8::Handle<v8::Value> val);
NanMaybe<int32_t>          NanTo<>(v8::Handle<v8::Value> val);
NanMaybe<int64_t>          NanTo<>(v8::Handle<v8::Value> val);
NanMaybe<uint32_t>         NanTo<>(v8::Handle<v8::Value> val);
```

###Example
```c++
v8::Local<v8::Value> val;
NanMaybeLocal<v8::String> = NanTo<v8::String>(val);
NanMaybe<double> = NanTo<double>(val);
```
