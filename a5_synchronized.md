# Synchronized

## Overview

https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html

https://blogs.oracle.com/javamagazine/post/java-thread-synchronization-synchronized-blocks-adhoc-locks

Los _threads_ se comunican principalmente compartiendo el acceso a variables. Esto hace posibles dos tipos de errores: _thread interference_ y _memory consistency errors_. 

La herramienta para evitar estos errores es la *sincronización*.

Sin embargo, la sincronización puede introducir nuevos tipos de errores: _deadlock_, _starvation_ y _livelock_.
