# Net One

``` C
#include "../common/common.c"

#define NAME "net1"
#define UID 998
#define GID 998
#define PORT 2998

void run()
{
  char buf[12];
  char fub[12];
  char *q;

  unsigned int wanted;

  wanted = random();

  sprintf(fub, "%d", wanted);

  if(write(0, &wanted, sizeof(wanted)) != sizeof(wanted)) {
      errx(1, ":(\n");
  }

  if(fgets(buf, sizeof(buf)-1, stdin) == NULL) {
      errx(1, ":(\n");
  }

  q = strchr(buf, '\r'); if(q) *q = 0;
  q = strchr(buf, '\n'); if(q) *q = 0;

  if(strcmp(fub, buf) == 0) {
      printf("you correctly sent the data\n");
  } else {
      printf("you didn't send the data properly\n");
  }
}

int main(int argc, char **argv, char **envp)
{
  int fd;
  char *username;

  /* Run the process as a daemon */
  background_process(NAME, UID, GID); 
  
  /* Wait for socket activity and return */
  fd = serve_forever(PORT);

  /* Set the client socket to STDIN, STDOUT, and STDERR */
  set_io(fd);

  /* Don't do this :> */
  srandom(time(NULL));

  run();
}
```

En este reto ahora recibimos el entero en bytes little-endian, debemos convertirlo a int. Esto es lo opuesto al primer reto:
``` python
import socket
import struct

s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("0.0.0.0",2997))

n = s.recv(4)
print(n)

n = struct.unpack("I",n)[0]
s.send(str(n).encode())
print(s.recv(1024))
```
