# Bin
## dex2oat

## dexdump

## dexlist

## dalvikvm

## oatdump

# Compiler


# Runtime

## Runtime

### 重要函数
- Runtime::Create()
- Runtime::Init()
- Runtime::Current()

## Thread
### src
- art/runtime/thread.cc


### 重要函数
- Thread::Attach()
- Thread::Current()

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

