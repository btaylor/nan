#Asyncworker
These classes make working with asynchronous code easier.

##NanAsyncWorker
`NanAsyncWorker` is an abstract class that you can subclass to have much of the annoying async queuing and handling taken care of for you.
It can even store arbitrary V8 objects for you and have them persist while the async work is in progress.
```c++
/* abstract */ class NanAsyncWorker {
 public:
  explicit NanAsyncWorker(NanCallback *callback_);

  virtual ~NanAsyncWorker();

  virtual void WorkComplete();

  void SaveToPersistent(
      const char *key, const v8::Local<v8::Value> &value);

  void SaveToPersistent(
      const v8::Handle<v8::String> &key, const v8::Local<v8::Value> &value);

  void SaveToPersistent(
      uint32_t index, const v8::Local<v8::Value> &value);

  v8::Local<v8::Value> GetFromPersistent(const char *key) const;

  v8::Local<v8::Value>
  GetFromPersistent(const v8::Local<v8::String> &key) const;

  v8::Local<v8::Value> GetFromPersistent(uint32_t index) const;

  virtual void Execute() = 0;

  uv_work_t request;

  virtual void Destroy();

 protected:
  NanPersistent<v8::Object> persistentHandle;
  NanCallback *callback;

  virtual void HandleOKCallback();

  virtual void HandleErrorCallback();

  void SetErrorMessage(const char *msg);

  const char* ErrorMessage();
};
```

###Example
```c++
NanCallback callback;
NanAsyncWorker *worker = new NanAsyncWorker(callback);
```

##NanAsyncProgressWorker
`NanAsyncProgressWorker` is an abstract class that you can subclass to have much of the annoying async queuing and handling taken care of for you.
It is derived from `NanAsyncWorker` with additional progress reporting callbacks.
```c++
/* abstract */ class NanAsyncProgressWorker : public NanAsyncWorker {
 public:
  explicit NanAsyncProgressWorker(NanCallback *callback_);

  virtual ~NanAsyncProgressWorker();

  void WorkProgress();

  class ExecutionProgress {
   public:
    void Send(const char* data, size_t size) const;
  };

  virtual void Execute(const ExecutionProgress& progress) = 0;
  virtual void HandleProgressCallback(const char *data, size_t size) = 0;

  virtual void Destroy();
```

###Example
```c++
NanCallback callback;
NanAsyncProgressWorker *worker = new NanAsyncProgressWorker(callback);
```

##NanAsyncQueueWorker
`NanAsyncQueueWorker` will run a `NanAsyncWorker` asynchronously via libuv.
Both the `execute` and `after_work` steps are taken care of for you.
Most of the logic for this is embedded in `NanAsyncWorker`.
```c++
void NanAsyncQueueWorker(NanAsyncWorker *);
```

###Example
```c++
NanAsyncWorker *worker = new NanAsyncWorker(/* ... */);
NanAsyncQueueWorker(worker);
```
