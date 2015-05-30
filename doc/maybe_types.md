#Maybe Types
The `NanMaybeLocal` and `NanMaybe` types are monads that encapsulate possibly
empty `v8::Local` handles.
 
##NanMaybeLocal
A NanMaybeLocal<> is a wrapper around v8::Local<> that enforces a check whether
the v8::Local<> is empty before it can be used.

If an API method returns a NanMaybeLocal<>, the API method can potentially fail
either because an exception is thrown, or because an exception is pending,
e.g. because a previous API call threw an exception that hasn't been caught
yet, or because a TerminateExecution exception was thrown. In that case, an
empty NanMaybeLocal is returned.
```c++
template<typename T>
class NanMaybeLocal {
 public:
  NanMaybeLocal();

  template<typename S>
  NanMaybeLocal(v8::Local<S> that);

  bool IsEmpty() const;

  template<typename S>
  bool ToLocal(v8::Local<S> *out);

  // Will crash if the NanMaybeLocal<> is empty.
  v8::Local<T> ToLocalChecked();

  template<typename S>
  v8::Local<S> FromMaybe(v8::Local<S> default_value) const;
};
```

##NanMaybe
A simple Maybe type, representing an object which may or may not have a
value, see https://hackage.haskell.org/package/base/docs/Data-Maybe.html.

If an API method returns a NanMaybe<>, the API method can potentially fail
either because an exception is thrown, or because an exception is pending,
e.g. because a previous API call threw an exception that hasn't been caught
yet, or because a TerminateExecution exception was thrown. In that case, a
"Nothing" value is returned.
```c++
template<typename T>
class NanMaybe {
 public:
  bool IsNothing() const;
  bool IsJust() const;

  // Will crash if the NanMaybe<> is nothing.
  T FromJust();

  T FromMaybe(const T& default_value);

  bool operator==(const NanMaybe &other);

  bool operator!=(const NanMaybe &other);
};
```

## NanNothing
Construct an empty `NanMaybe` type representing "Nothing".
```c++
template<typename T>
NanMaybe<T> NanNothing();
```

## NanJust
Construct a `NanMaybe` type representing "Just" a value.
```c++
template<typename T>
NanMaybe<T> NanJust(const T &t);
```
