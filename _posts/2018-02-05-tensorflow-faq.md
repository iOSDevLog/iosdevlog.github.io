---
layout: post
title: "tensorflow FAQ"
author: iosdevlog
date: 2018-02-05 11:31:31 +0800
description: ""
category: 
tags: [tensorflow]
---

# install tensorflow
---

* Q

*Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX AVX2*

* A

```
$ cat ~/.bashrc
...
export TF_CPP_MIN_LOG_LEVEL=2
```

<https://stackoverflow.com/questions/47068709/your-cpu-supports-instructions-that-this-tensorflow-binary-was-not-compiled-to-u>

# macos use matplotlib
---

* Q: 

...
    from matplotlib.backends import _macosx
RuntimeError: Python is not installed as a framework. The Mac OS X backend will not be able to function correctly if Python is not installed as a framework. See the Python documentation for more information on installing Python as a framework on Mac OS X. Please either reinstall Python as a framework, or try one of the other backends. If you are using (Ana)Conda please install python.app and replace the use of 'python' with 'pythonw'. See 'Working with Matplotlib on
OSX' in the Matplotlib FAQ for more information.

* A:

```
$ echo "backend: TkAgg" >> ~/.matplotlib/matplotlibrc
```

<https://stackoverflow.com/questions/33100969/matplotlib-example-code-not-working-on-python-virtual-environment>
