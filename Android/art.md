# Bin
## dex2oat

## dexdump

## dexlist

## dalvikvm

## oatdump

# Compiler


# Runtime

## Runtime

### src
- art/runtime/runtime.h
- art/runtime/runtime.cc

### 重要函数
- JNI_CreateJavaVM()
  - Runtime::Create()
    - Runtime::Runtime()
    - Runtime::Init()

  - Runtime::Current()
  - Runtime::Start()

## ThreadList

### src
- art/runtime/thread_list.h
- art/runtime/thread_list.cc

### 重要函数
- ThreadList::DumpForSigQuit()
  - Dump(os, dump_native_stack)
  - DumpUnattachedThreads(os, dump_native_stack && kDumpUnattachedThreadNativeStackForSigQuit)

- ThreadList::ThreadList()
- ThreadList::SuspendAll()

## Thread
### src
- art/runtime/thread.h
- art/runtime/thread.cc

### 重要函数
- Thread::Startup()
- Thread::Attach()
  - Thread::Thread()
  - Thread::Init()
    - thread_list->Register(this)
  - set thread name

- Thread::Current()
- Thread::Dump()
  - Thread::DumpState()
  - Thread::DumpStack()
    - DumpNativeStack()
      - libbacktrace
        - 通过ptrace获取用户态寄存器
        - unwind
    - DumpJavaStack()
      - StackDumpVisitor dumper
      - dumper.WalkStack()

## ManagedStack
### src
- art/runtime/managed_stack.h
- art/runtime/managed_stack.cc

## SignalCatcher
- 处理SIGQUIT
- 处理SIGUSR1

### src
- art/runtime/signal_catcher.h
- art/runtime/signal_catcher.cc

### 重要函数
- SignalCatcher::SignalCatcher()

- SignalCatcher::Run()
  - signal_catcher->WaitForSignal(self, signals);
  - signal_catcher->HandleSigQuit()
    - DumpCmdLine()
    - Runtime::DumpForSigQuit()
    - Output(os.str())

  - signal_catcher->HandleSigUsr1()



## FaultManager
### src
- art/runtime/fault_handler.h
- art/runtime/fault_handler.cc

### class
- FaultManager
- FaultHandler
  - NullPointerHandler
    - NullPointerHandler::NullPointerHandler()
      - FaultManager::AddHandler()
        - generated_code_handlers_.push_back(handler)
    - NullPointerHandler::Action()
      - art_quick_throw_null_pointer_exception_from_signal()
  - SuspensionHandler
  - StackOverflowHandler
  - JavaStackTraceHandler

### 全局变量
- FaultManager fault_manager

### 重要函数
- FaultManager::FaultManager()
  - sigaction(SIGSEGV, nullptr, &oldaction_)

- FaultManager::Init()
  - AddSpecialSignalHandlerFn()
    - InitializeSignalChain()
    - chains[signal].AddSpecialHandler(sa)

- art_fault_handler()
  - FaultManager::HandleFault()
    - generated_code_handlers_
    - HandleFaultByOtherHandlers()
    - art_sigsegv_fault()

