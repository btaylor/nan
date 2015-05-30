#Weak Callbacks

## NanWeakCallbackInfo
Argument to weak callbacks.
You may need to free any external resources attached to the object.
```c++
template<typename T>
class NanWeakCallbackInfo {
 public:
  typedef void (*Callback)(const NanWeakCallbackInfo<T>& data);

  v8::Isolate *GetIsolate() const;

   /**
    *   Get the parameter that was associated with the weak handle.
    */
  T *GetParameter() const;

   /**
    *   Get pointer from internal field, index can be 0 or 1.
    */
  void *GetInternalField(int index) const;
};
```

### Example
```c++
void weakCallback(const NanWeakCallbackInfo<int> &data) {
  int *parameter = data.GetParameter();
  delete parameter;
}

NanPersistent<v8::Object> obj;
int *data = new int(0);
obj.SetWeak(data, callback, NanWeakCallbackType::kParameter);
```
