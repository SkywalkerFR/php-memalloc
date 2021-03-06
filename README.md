# Memalloc  [![Releases](https://img.shields.io/github/release/SkywalkerFR/php-memalloc/all.svg?style=flat-square&logo=php&colorB=97ca00&logoColor=ffffff)](https://github.com/SkywalkerFR/php-memalloc/releases)

It's a tiny lib made for PHP to use by simplifying memory sharing (cache/buffer/datasets/persistent variable)  
_(Only compatible with unix systems yet)_

# Why Memalloc ?
Use of shared memory is a fast method of data exchange between processes, mainly because there is no kernel involvement in passing data after the segments are created. Methods of this kind are often called interprocess communication (IPC). Other IPC methods include pipes, message queues, RPC, and sockets.  
This fast and reliable ability to exchange data between applications is invaluable when working with an ecosystem of applications needing to communicate with each other. The usual method of using databases to exchange information between applications often causes slow queries and even blocking I/O, depending on the size of the ecosystem. With shared memory, there's no I/O slowing a script execution down.

# How to use ?
Just need to add the memalloc.php file with a require in your script :
```php
require_once('lib/memalloc.php');
```
_(Obviously you can change his location)_

# Functions list

Funct                                             |Utility
--------------------------------------------------|-------------------------------------
```memalloc_write($stackname, $data)```           | Writing/overwriting in shared memory
```memalloc_read($stackname)```                   | Reading in shared memory
```memalloc_delete($stackname)```                 | Delete an existing stack in the shared memory
```memalloc_clean()```                            | If necessary to delete oldest stacks (20% free memory after)
```memalloc_purge()```                            | If necessary to clear the memory instantly


# Example
```php
<?php

require_once('lib/memalloc.php');

memalloc_write('var1', 'Foobar');
$data = memalloc_read('var1');
$data.= '123';

memalloc_write('var1', $data);
echo memalloc_read('var1');

memalloc_delete('var1');

?>
```
Output will display ```Foobar123```  
   
At the end of this script, ```var1``` stack is destroyed, but if you don't delete it, the next time the script is executed, the stack will still be available.  
_(As long as the apache2 service isn't restarted obviously)_

By default, ```$memalloc_max_memory``` is equal to ```50Mo```  
and ```$memalloc_autoclean``` is equal to ```80%```. 

When process memory exceeds by default 50Mo, the function ```memalloc_clean()``` is called to delete oldest stacks, to reach by default 80% of used memory (20% free).

__Have fun!__
