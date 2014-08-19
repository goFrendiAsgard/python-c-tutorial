Accessing C function with Python
================================

Why do you want to do this?
---------------------------

* You can write Python code fast, but computer will execute it slowly.
* You cannot write C code fast, but computer will execute is fastly.
* You want something easy to code that executed fastly.
* You know C, and you are not bothered to write some performance-critical function in C, but you don't want to write C all the time.

Where is this example come from?
--------------------------------

From [tutorialspoint](http://www.tutorialspoint.com/python/python_further_extensions.htm)

Let's get started
-----------------

Based on the tutorialspoint's article, we already have make 3 files:
```
    |--- hello.c            (a `C` source code that use Python API)
    |
    |--- setup.py           (a typical Python module setup installation file to 
    |                        compile and install `hello.c` as Python module)
    |
    |--- hello_world.py     (an example Python file that will use function 
    |                        declared in `hello.c`)
    |
    |--- readme.md          (this file)
``` 

* First of all, I'm sure you don't want to ruin your entire system while trying to do this. That's why we need a virtual environment. 
  To make a virtual environment, you need to have virtualenv installed. To install virtualenv, do this:
    ```
    sudo pip install virtualenv
    ```
* You can make a new virtual environment by doing this (we are going to make a new virtual environment named `venv`):
    ```
    virtualenv venv
    ```
* Activate your virtual environment by doing this:
    ```
    source venv/bin/activate
    ```
* Once your virtual environment ready, you can try to compile install our `hello.c` as `helloworld` module in Python:
    ```
    python setup.py install
    ```
* Now run `hello_world.py` by doing this:
    ```
    python hello_world.py
    ```
* Last, deactivate your virtual environment by doing this:
    ```
    deactivate
    ```
* Sometime, C libraries doesn't created with intention to be accessed in Python. In this case you will need to write a wrapper.
  for those libraries. `python-opencv` is a python wrapper for `open-cv` library.

 Source Codes
 -------------

 * hello.c
    ```c
    #include <Python.h>

    static PyObject* helloworld(PyObject* self)
    {
        return Py_BuildValue("s", "Hello, Python extensions!!");
    }

    static char helloworld_docs[] =
        "helloworld( ): Any message you want to put here!!\n";

    static PyMethodDef helloworld_funcs[] = {
        {"helloworld", (PyCFunction)helloworld, 
         METH_NOARGS, helloworld_docs},
        {NULL}
    };

    void inithelloworld(void)
    {
        Py_InitModule3("helloworld", helloworld_funcs,
                       "Extension module example!");
    }
    ```

* setup.py
    ```python
    from distutils.core import setup, Extension
    setup(name='helloworld', version='1.0',  \
          ext_modules=[Extension('helloworld', ['hello.c'])])
    ```

* hello_world.py
    ```python
    import helloworld
    print helloworld.helloworld()
    ```

