#Callbacks
V8 can call back to your code through these functions.

##Types
```c++
typedef void(*NanFunctionCallback)(const NanFunctionCallbackInfo<v8::Value>&);
typedef void(*NanGetterCallback)
    (v8::Local<v8::String>, const NanPropertyCallbackInfo<v8::Value>&);
typedef void(*NanSetterCallback)(
    v8::Local<v8::String>,
    v8::Local<v8::Value>,
    const NanPropertyCallbackInfo<void>&);
typedef void(*NanPropertyGetterCallback)(
    v8::Local<v8::String>,
    const NanPropertyCallbackInfo<v8::Value>&);
typedef void(*NanPropertySetterCallback)(
    v8::Local<v8::String>,
    v8::Local<v8::Value>,
    const NanPropertyCallbackInfo<v8::Value>&);
typedef void(*NanPropertyEnumeratorCallback)
    (const NanPropertyCallbackInfo<v8::Array>&);
typedef void(*NanPropertyDeleterCallback)(
    v8::Local<v8::String>,
    const NanPropertyCallbackInfo<v8::Boolean>&);
typedef void(*NanPropertyQueryCallback)(
    v8::Local<v8::String>,
    const NanPropertyCallbackInfo<v8::Integer>&);
typedef void(*NanIndexGetterCallback)(
    uint32_t,
    const NanPropertyCallbackInfo<v8::Value>&);
typedef void(*NanIndexSetterCallback)(
    uint32_t,
    v8::Local<v8::Value>,
    const NanPropertyCallbackInfo<v8::Value>&);
typedef void(*NanIndexEnumeratorCallback)
    (const NanPropertyCallbackInfo<v8::Array>&);
typedef void(*NanIndexDeleterCallback)(
    uint32_t,
    const NanPropertyCallbackInfo<v8::Boolean>&);
typedef void(*NanIndexQueryCallback)(
    uint32_t,
    const NanPropertyCallbackInfo<v8::Integer>&);
```

##NanReturnValue
Used for setting the return value in callbacks.
```c++
template<typename T>
class NanReturnValue {
 public:
  // Handle setters
  template <typename S> inline void Set(const v8::Handle<S> &handle);
  template <typename S> inline void Set(const NanGlobal<S> &handle);

  // Fast primitive setters
  inline void Set(bool value);
  inline void Set(double i);
  inline void Set(int32_t i);
  inline void Set(uint32_t i);

  // Fast JS primitive setters
  inline void SetNull();
  inline void SetUndefined();
  inline void SetEmptyString();

  // Convenience getter for isolate
  inline v8::Isolate *GetIsolate() const;
};
```

##NanFunctionCallbackInfo
The argument information given to function call callbacks.  This
class provides access to information about the context of the call,
including the receiver, the number and values of arguments, and
the holder of the function.
```c++
template<typename T>
class NanFunctionCallbackInfo {
 public:
  NanReturnValue<T> GetReturnValue() const;

  v8::Local<v8::Function> Callee();
  v8::Local<v8::Value> Data();
  v8::Local<v8::Object> Holder();
  bool IsConstructCall();
  int Length() const;
  v8::Local<v8::Value> operator[](int i) const;
  v8::Local<v8::Object> This() const;
  v8::Isolate *GetIsolate() const;
};
```

##NanPropertyCallbackInfo
The information passed to a property callback about the context of the property access.
This class provides access to information about the context of the access, including the
receiver and the holder of the property.
```c++
template<typename T>
class NanPropertyCallbackInfo : public NanPropertyCallbackInfoBase<T> {
 public:
  NanReturnValue<T> GetReturnValue() const;
  
  v8::Isolate* GetIsolate() const;
  v8::Local<v8::Value> Data() const;
  v8::Local<v8::Object> This() const;
  v8::Local<v8::Object> Holder() const;
};
```
