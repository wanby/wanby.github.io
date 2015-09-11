###Node version 0.12

##### 1. export general function template:
  ```
  void Init(Handle<Object> exports) { // use object to store key-function values
    Isolate* isolate = Isolate::GetCurrent();
    HandleScope scope(isolate);
    exports->Set(String::NewFromUtf8(isolate, "add"),
            (FunctionTemplate::New(isolate, Add))->GetFunction());
  }
  ```
  
  or
  
  ```
  void Init(Handle<Object> exports) {
    NODE_SET_METHOD(exports, "add", Add);
  }
  ```
  
#### 2. export callbacks / object factory
  ```
  void Init(Handle<Object> exports, Handle<Object> module) { // two arguments
    NODE_SET_METHOD(module, "exports", RunCallback);
  }
  ```
  
#### 3. throw exception
  ```
  Isolate* isolate = Isolate::GetCurrent();
  isolate->ThrowException(Exception::TypeError());
  ```
  
#### 4. callback
  ```
  void RunCallback(const FunctionCallbackInfo<Value>& args) {
    Isolate* isolate = Isolate::GetCurrent();
    HandleScope scope(isolate);
  
    Local<Function> cb = Local<Function>::Cast(args[0]);
    const unsigned argc = 1;
    Local<Value> argv[argc] = { String::NewFromUtf8(isolate, "hello world") };
    cb->Call(isolate->GetCurrentContext()->Global(), argc, argv);
  }
  
  ```
  
#### 5. create object
  ```
  Local<Object> obj = Object::New(isolate);
  obj->Set(String::NewFromUtf8(isolate, "msg"), args[0]->ToString());
  ```
  
#### 6. object wrap
  ```
  static v8::Persistent<v8::Function> constructor; // ?
  
  
  // Prepare constructor template
  Local<FunctionTemplate> tpl = FunctionTemplate::New(isolate, New);
  tpl->SetClassName(String::NewFromUtf8(isolate, "MyObject"));
  tpl->InstanceTemplate()->SetInternalFieldCount(1);

  // Prototype
  NODE_SET_PROTOTYPE_METHOD(tpl, "plusOne", PlusOne);    // or
  //PrototypeTemplate()->Set("a", FunctionTemplate::New());
  //PrototypeTemplate()->Set("b", Number::New())


  constructor.Reset(isolate, tpl->GetFunction()); // ?
  
  exports->Set(String::NewFromUtf8(isolate, "MyObject"),
               tpl->GetFunction());
  ```
  
  
  
  ```
  void MyObject::New(const FunctionCallbackInfo<Value>& args) {
    Isolate* isolate = Isolate::GetCurrent();
    HandleScope scope(isolate);
  
    if (args.IsConstructCall()) {
      // Invoked as constructor: `new MyObject(...)`
      double value = args[0]->IsUndefined() ? 0 : args[0]->NumberValue();
      MyObject* obj = new MyObject(value);
      obj->Wrap(args.This());
      args.GetReturnValue().Set(args.This());
    } else {
      // Invoked as plain function `MyObject(...)`, turn into construct call.
      const int argc = 1;
      Local<Value> argv[argc] = { args[0] };
      Local<Function> cons = Local<Function>::New(isolate, constructor);
      args.GetReturnValue().Set(cons->NewInstance(argc, argv));
    }
  }
  ```
