# typing

```python
from typing import Optional, Literal
```





# rpy2


```python
import rpy2
import os
os.environ['R_HOME'] = '/public/software/apps/R-4.2.1-gcc910'
os.environ['LD_LIBRARY_PATH'] = '/public/software/apps/R-4.2.1-gcc910/lib' + os.environ.get('LD_LIBRARY_PATH','')

import rpy2
import os
os.environ['R_HOME'] = '/public/home/caojun/anaconda3/envs/r4.2clone'
os.environ['LD_LIBRARY_PATH'] = '/public/home/caojun/anaconda3/envs/r4.2clone/lib/R/lib' + os.environ.get('LD_LIBRARY_PATH','')
os.environ['LD_LIBRARY_PATH'] = '/public/home/caojun/anaconda3/envs/r4.2clone/lib' + os.environ.get('LD_LIBRARY_PATH','')

 
from rpy2.robjects import r


```


```python
def _dlopen_rlib(r_home: typing.Optional[str]):
    """Open R's shared C library.

    This is only relevant in ABI mode."""
    if r_home is None:
        raise ValueError('r_home is None. '
                         'Try python -m rpy2.situation')
    
    r_home=os.path.join(r_home,"lib/R")
    lib_path = rpy2.situation.get_rlib_path(r_home, platform.system())
    if lib_path is None:
        raise ValueError('The library path cannot be None.')
    else:
        rlib = ffi.dlopen(lib_path)
    return rlib
```



# logging



# flit
