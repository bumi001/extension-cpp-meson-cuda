# C++/CUDA Extensions in PyTorch (Passes CPU and GPU tests)

- This repo includes the latest .cpp and .cu code from pytorch/extension-cpp.
- It uses PyMODINIT_FUNC and PyModuleDef_Init
- It sets up Py_LIMITED_API correctly
- It enables compilation with CUDA support

This example specifically illustrates the use of `meson` to build, install and use a torch C++ extension.

**Using meson.build to build**

To build, use the following command:

```
python -m pip wheel --no-build-isolation --no-clean -Cbuild-dir=build .
```

The last two commandline options are optional and are useful if you want to examine the outputs of the build process.

Note that the `--no-build-isolation` is required, because the extension needs to link against
the torch shared objects. When running without `--no-build-isolation`, it will link against the
temporary torch installation which is removed after the build process is finished.

Also note that python -m build is not recommended. It will result in an unresolved symbol when you try to run the tests.

I am using Python 3.13 and on my system, the resulting wheel is named cpp_extension-0.0.1-cp313-abi3-linux_x86_64.whl.
It is important that you see abi3 in the name of the wheel.

**Installing your build**

On my system I type:

```
pip install cpp_extension-0.0.1-cp313-abi3-linux_x86_64.whl
```

This will install the package to your site_packages/extension_cpp directorry under your virtual environment. You should see a
_C.abi3.so file there.

**Running the tests**

From the same directory in which I built the wheel, I type

```
python test/test_extension.py
```

My output is:
```
........
----------------------------------------------------------------------
Ran 8 tests in 1.404s

OK
```

---

See [here](https://pytorch.org/tutorials/advanced/cpp_custom_ops.html) for the accompanying tutorial.
This repo demonstrates how to write an example `extension_cpp.ops.mymuladd`
custom op that has both custom CPU and CUDA kernels.

The examples in this repo work with PyTorch 2.4+.

## Upstream Authors

[Peter Goldsborough](https://github.com/goldsborough), [Richard Zou](https://github.com/zou3519),
[Daniel Knuettel](https://github.com/daknuett)
