# grohl
--
    import "github.com/technoweenie/grohl"

Grohl is an opinionated library for outputting logs in a key=value structure.
This event data is used to drive metrics and monitoring services.

Dave Grohl is the lead singer of Foo Fighters. I hear he's also passionate about
event driven metrics.

## Usage

```go
var CurrentContext = &Context{make(Data), CurrentLogger, "s", nil}
```

#### func  AddContext

```go
func AddContext(key string, value interface{})
```

#### func  BuildLog

```go
func BuildLog(data Data, addTime bool) string
```
Builds a log message as a single line from log data.

#### func  Counter

```go
func Counter(sampleRate float32, bucket string, n ...int)
```

#### func  DeleteContext

```go
func DeleteContext(key string)
```

#### func  ErrorBacktrace

```go
func ErrorBacktrace(err error) string
```

#### func  ErrorBacktraceLines

```go
func ErrorBacktraceLines(err error) []string
```

#### func  Format

```go
func Format(value interface{}) string
```

#### func  Gauge

```go
func Gauge(sampleRate float32, bucket string, value ...string)
```

#### func  Log

```go
func Log(data Data)
```

#### func  Report

```go
func Report(err error, data Data)
```

#### func  SetTimeUnit

```go
func SetTimeUnit(unit string)
```

#### func  TimeUnit

```go
func TimeUnit() string
```

#### func  Timing

```go
func Timing(sampleRate float32, bucket string, d ...time.Duration)
```

#### func  Watch

```go
func Watch(logger Logger, logch chan Data)
```

#### type ChannelLogger

```go
type ChannelLogger struct {
}
```


#### func  NewChannelLogger

```go
func NewChannelLogger(channel chan Data) (*ChannelLogger, chan Data)
```

#### func (*ChannelLogger) Log

```go
func (l *ChannelLogger) Log(data Data) error
```

#### type Context

```go
type Context struct {
	Logger            Logger
	TimeUnit          string
	ExceptionReporter ExceptionReporter
}
```


#### func  NewContext

```go
func NewContext(data Data) *Context
```

#### func (*Context) Add

```go
func (c *Context) Add(key string, value interface{})
```

#### func (*Context) Counter

```go
func (c *Context) Counter(sampleRate float32, bucket string, n ...int)
```

#### func (*Context) Delete

```go
func (c *Context) Delete(key string)
```

#### func (*Context) Gauge

```go
func (c *Context) Gauge(sampleRate float32, bucket string, value ...string)
```

#### func (*Context) Log

```go
func (c *Context) Log(data Data) error
```

#### func (*Context) Merge

```go
func (c *Context) Merge(data Data) Data
```

#### func (*Context) New

```go
func (c *Context) New(data Data) *Context
```

#### func (*Context) Report

```go
func (c *Context) Report(err error, data Data) error
```
Implementation of ExceptionReporter that writes to a grohl logger.

#### func (*Context) Timer

```go
func (c *Context) Timer(data Data) *Timer
```
A timer tracks the duration spent since its creation.

#### func (*Context) Timing

```go
func (c *Context) Timing(sampleRate float32, bucket string, d ...time.Duration)
```

#### type Data

```go
type Data map[string]interface{}
```


#### type ExceptionReporter

```go
type ExceptionReporter interface {
	Report(err error, data Data) error
}
```


#### type IoLogger

```go
type IoLogger struct {
	AddTime bool
}
```

A really basic logger that builds lines and writes to any io.Writer. This
expects the writers to be threadsafe.

#### func  NewIoLogger

```go
func NewIoLogger(stream io.Writer) *IoLogger
```

#### func (*IoLogger) Log

```go
func (l *IoLogger) Log(data Data) error
```

#### type Logger

```go
type Logger interface {
	Log(Data) error
}
```


```go
var CurrentLogger Logger = NewIoLogger(nil)
```

#### func  SetLogger

```go
func SetLogger(logger Logger) Logger
```

#### type Statter

```go
type Statter interface {
	Counter(sampleRate float32, bucket string, n ...int)
	Timing(sampleRate float32, bucket string, d ...time.Duration)
	Gauge(sampleRate float32, bucket string, value ...string)
}
```


```go
var CurrentStatter Statter = CurrentContext
```

#### type Timer

```go
type Timer struct {
	Started  time.Time
	TimeUnit string
}
```


#### func  NewTimer

```go
func NewTimer(data Data) *Timer
```

#### func (*Timer) Elapsed

```go
func (t *Timer) Elapsed() time.Duration
```

#### func (*Timer) Finish

```go
func (t *Timer) Finish()
```
Writes a final log message with the elapsed time shown.

#### func (*Timer) Log

```go
func (t *Timer) Log(data Data) error
```
Writes a log message with extra data or the elapsed time shown. Pass nil or use
Finish() if there is no extra data.
