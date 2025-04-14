## CLCP (C Logger Code Preparation)

> *In many of my projects, I encounter situations where I need to log information or output information and terminate with an error. I've created CSL - a full-featured logger implementation for projects. However, when writing libraries, I realized that adding extra dependencies to code, complicating its operation and maintenance is not beneficial. If code depends on the same logger but different versions, it creates a problem. Not to mention that updating everything everywhere is difficult for me as the person who wrote this and is obligated to maintain the code in working condition. Therefore, I decided to create a code template with simple logging functionality that can be conveniently copied and pasted into your header file without adding unnecessary dependencies while maintaining the core logger functionality.*

## Dependencies

```c
#include <stdio.h>
#include <stdarg.h>
#include <stdlib.h>
```

## Code

```c
// --------------- CLCP ---------------
typedef enum {
    LEVEL_FATAL = 0,
    LEVEL_WARN  = 1,
    LEVEL_DEBUG = 2,
    LEVEL_TODO  = 3,
    LEVEL_UNIMPLEMENTED = 4
} LogLevel;

static const char* const LEVEL_NAMES[] = {
    "FATAL", "WARN", "DEBUG", "TODO", "UNIMPLEMENTED"
};

static inline void lmessage(LogLevel level, const char* format, ...) {
    fprintf(stderr, "[%s] ", LEVEL_NAMES[level]);
    
    va_list args;
    va_start(args, format);
    vfprintf(stderr, format, args);
    va_end(args);
    
    fprintf(stderr, "\n");
    
    if (level == LEVEL_FATAL || level == LEVEL_UNIMPLEMENTED) {
        exit(1);
    }
}

#define lfatal(...) lmessage(LEVEL_FATAL, __VA_ARGS__)
#define lwarn(...)  lmessage(LEVEL_WARN, __VA_ARGS__)
#define ldebug(...) lmessage(LEVEL_DEBUG, __VA_ARGS__)
#define ltodo(...)  lmessage(LEVEL_TODO, __VA_ARGS__)
#define lunimp(...) lmessage(LEVEL_UNIMPLEMENTED, "Not implemented: " __VA_ARGS__)
//------------------------------

```
