# Basic Import Statements

Python's `import` system lets you bring external code — from the standard library, third-party packages, or your own modules — into a script. Rather than writing everything from scratch, you reuse existing, tested functionality. This document covers how imports work, how to write them correctly, and what happens under the hood.

## The `import` Statement

The `import` keyword loads a module and makes it available in the current script.

```python
import math
```

>[!NOTE]
>After this line, everything defined in `math` is accessible — but only through the module name as a prefix (see [Module Namespaces](#module-namespaces)).

## Importing Entire Modules

When you import a module, Python locates it, executes its top-level code, and binds the result to a name in your current namespace.

```python
import os
import sys
import random
```

You can also give a module a shorter alias using `as`:

```python
import numpy as np
import pandas as pd
```

>[!TIP]
>This is especially useful for modules with long names that you reference frequently.

## Using Module Functions (`module.function()`)

After importing a module, access its contents using dot notation:

```python
import math

result = math.sqrt(16)     # 4.0
rounded = math.floor(3.9)  # 3
pi = math.pi               # 3.141592653589793
```

The module name acts as a qualifier — it makes clear *where* a function comes from and avoids name conflicts between modules.

```python
import os
import sys

current_dir = os.getcwd()
python_version = sys.version
```

If you'd rather call a function directly without the prefix, use `from`:

```python
from math import sqrt

result = sqrt(16)  # No prefix needed
```

You can also import multiple specific names at once:

```python
from math import sqrt, floor, pi
```

>[!NOTE]
>`from module import *` imports everything from a module without a prefix. Avoid it — it clutters the namespace and makes it hard to tell where names come from.

## Importing Multiple Modules

Import each module on its own line. This is the recommended style (PEP 8):

```python
import os
import sys
import json
```

Grouping multiple imports on one line works but is discouraged:

```python
import os, sys, json  # Works, but not recommended
```

**Conventional order (PEP 8):**

1. Standard library modules (`os`, `sys`, `math`, ...)
2. Third-party packages (`numpy`, `requests`, ...)
3. Local/project modules

Separate each group with a blank line:

```python
import os
import sys

import numpy as np
import requests

import my_module
```

## Import Statement Placement

Place all imports at the **top of the file**, after any module docstring and before any other code.

```python
"""Module docstring (optional)."""

import os
import math

# Rest of the code starts here
def my_function():
    ...
```

Imports inside functions or conditionals are valid Python, but use them sparingly — only when an import is intentionally deferred (e.g., to avoid circular imports or for optional dependencies):

```python
def load_data():
    import pandas as pd  # Deferred import — acceptable in specific cases
    return pd.read_csv("data.csv")
```

## Module Namespaces

Each module has its own **namespace** — an isolated scope where its names live. When you write `import math`, Python does *not* dump `sqrt`, `pi`, and everything else into your script's global scope. Instead, they remain inside the `math` namespace and you access them via `math.sqrt`, `math.pi`, etc.

This prevents **name collisions**:

```python
import math
import cmath  # Complex math — also has sqrt()

real_root = math.sqrt(9)    # math's sqrt
complex_root = cmath.sqrt(-1)  # cmath's sqrt
```

>[!NOTE]
>Without namespaces, two modules defining a function with the same name would overwrite each other.

When you use `from math import sqrt`, you copy `sqrt` directly into *your* namespace. That's fine for one import, but importing the same name from two modules will cause the second to silently overwrite the first:

```python
from math import sqrt
from cmath import sqrt  # Overwrites math.sqrt — silent bug
```

>[!TIP]
>Prefer `import module` and dot notation when working with multiple modules that might share names.
