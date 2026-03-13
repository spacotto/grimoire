## Import Aliases

Python lets you bind an import to a shorter or clearer name using the `as` keyword. Aliases reduce repetition, prevent name collisions, and make code easier to read — without changing how the imported module or function behaves.

### The `as` Keyword

`as` assigns a local name to whatever you're importing. The original module is unchanged; only your reference to it differs.

```python
import numpy as np          # np is now your local name for numpy
from math import sqrt as sq # sq is now your local name for sqrt
```

### `import module as alias`

Use this form when you want to import a whole module under a different name.

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({"x": [1, 2, 3]})
plt.plot(df["x"])
plt.show()
```

>[!NOTE]
>The alias replaces the full module name everywhere in the file.

### `from module import function as alias`

Use this form when you only need one (or a few) names from a module, and you want to rename them locally.

```python
from datetime import datetime as dt
from os.path import join as path_join

now = dt.now()
full_path = path_join("/home", "user", "docs")
```

>[!NOTE]
>This keeps the namespace clean: only the aliased name enters your scope.

### When to Use Aliases

| Situation | Why an alias helps |
|---|---|
| Module name is long or deeply nested | Cuts down on repetition |
| Two imports share the same last name | Avoids collision (`import a.utils as a_utils`) |
| You want a more descriptive local name | Clarifies intent at the call site |
| Following community conventions | Makes code immediately recognizable |

>[!TIP]
>Avoid aliases that are too cryptic or that shadow built-ins (`import os as os` adds nothing; `import os as o` removes clarity).

### Common Aliasing Conventions

These are widely recognized across the Python ecosystem:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import tensorflow as tf
import tkinter as tk
import xml.etree.ElementTree as ET
```

>[!NOTE]
>Sticking to these conventions means other developers instantly understand your code.

### Improving Code Readability

Aliases work best when they make the code at the call site *clearer*, not just shorter.

**Before** — noisy, repetitive:
```python
import matplotlib.pyplot
matplotlib.pyplot.figure()
matplotlib.pyplot.plot([1, 2, 3])
matplotlib.pyplot.show()
```

**After** — clean, readable:
```python
import matplotlib.pyplot as plt
plt.figure()
plt.plot([1, 2, 3])
plt.show()
```

>[!TIP]
>A good alias reads naturally in context. If you find yourself explaining what an alias stands for every time you use it, it's probably too short.
